---
title: Servidor de GATT de Bluetooth
description: En este artículo se proporciona información general sobre el perfil del servidor de atributo genérico (GATT) de Bluetooth para las aplicaciones para la Plataforma universal de Windows (UWP), junto con código de muestra para casos de uso comunes.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f59ae45486ee72f9d901898f6b03674e6b3e299c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370090"
---
# <a name="bluetooth-gatt-server"></a>Servidor de GATT de Bluetooth


**API importantes**
- [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)


En este artículo se muestran las API del servidor de atributo genérico (GATT) de Bluetooth para las aplicaciones para la Plataforma universal de Windows (UWP), junto con código de muestra para tareas de servidor GATT comunes. 
- Definir los servicios admitidos
- Publicar el servidor para que lo puedan detectar clientes remotos
- Anunciar la compatibilidad con el servicio
- Responder a solicitudes de lectura y escritura
- Enviar notificaciones a los clientes suscritos

## <a name="overview"></a>Información general
Normalmente, Windows funciona en el rol de cliente. No obstante, surgen muchos escenarios que requieren que Windows también actúe como un servidor de GATT de Bluetooth LE. Casi todos los escenarios en los dispositivos de IoT, junto con la mayoría de las comunicaciones BLE multiplataforma, requerirán que Windows actúe como un servidor de GATT. Además, el envío de notificaciones a dispositivos puestos cercanos se ha convertido en un escenario popular que también requiere esta tecnología.  
> Antes de continuar, asegúrate de que te queden claro todos los conceptos que se describen en los [documentos de cliente de GATT](gatt-client.md).  

Las operaciones de servidor se centrarán en el proveedor de servicios y el elemento GattLocalCharacteristic. Estas dos clases proporcionará la funcionalidad necesaria para declarar, implementar y exponer una jerarquía de datos a un dispositivo remoto.

## <a name="define-the-supported-services"></a>Definir los servicios admitidos
La aplicación puede declarar uno o varios servicios que Windows publicará. Cada servicio se identifica de manera exclusiva mediante un UUID. 

### <a name="attributes-and-uuids"></a>Atributos y UUID
Cada servicio, característica y descriptor se define por su propio UUID único de 128 bits.
> Todas las API de Windows usan el término GUID, pero el estándar de Bluetooth los define como UUID. Para nuestros propósitos, estos dos términos son intercambiables, de modo que seguiremos empleando el término UUID. 

Si el atributo es estándar y está definido por la definición SIG de Bluetooth, también tendrá un identificador corto de 16 bits correspondiente (por ejemplo, UUID del nivel de batería es 0000**2A19**-0000-1000-8000-00805F9B34FB y el identificador corto es 0x2A19). Estos UUID estándares pueden verse en [GattServiceUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) y [GattCharacteristicUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids).

Si la aplicación implementa su propio servicio personalizado, será necesario generar un UUID personalizado. Esto se hace fácilmente en Visual Studio a través de Herramientas -> CreateGuid (usa la opción 5 para obtenerlo en el formato "xxxx xxxxxxxx-xxxx-..."). Este UUID ahora se puede usar para declarar nuevos servicios, características o descriptores locales.

#### <a name="restricted-services"></a>Servicios restringidos
Los siguientes servicios están reservados por el sistema y no se pueden publicar en estos momentos:
1. Servicio de información del dispositivo (DIS)
2. Servicio de perfil de atributo genérico (GATT)
3. Servicio de perfil de acceso genérico (GAP)
4. Servicio de dispositivos de interfaz humana (HOGP)
5. Servicio de parámetros de examen (SCP)

> Intentar crear un servicio bloqueado hará que el objeto BluetoothError.DisabledByPolicy se devuelva desde la llamada a CreateAsync.

#### <a name="generated-attributes"></a>Atributos generados
Los siguientes descriptores los genera automáticamente el sistema, en función del objeto GattLocalCharacteristicParameters proporcionado durante la creación de la característica:
1. Configuración de la característica de cliente (si la característica está marcada como indicable u obligatoria).
2. Descripción de usuario de la característica (si se establece la propiedad UserDescription). Consulta la propiedad GattLocalCharacteristicParameters.UserDescription para más información.
3. Formato de la característica (un descriptor para cada formato de presentación especificado).  Consulta la propiedad GattLocalCharacteristicParameters.PresentationFormats para más información.
4. Formato agregado de la característica (si se especifica más de un formato de presentación).  Consulta la propiedad GattLocalCharacteristicParameters.PresentationFormats para más información.
5. Propiedades extendidas la característica (si la característica está marcada con el bit de propiedades extendidas).

> El valor del descriptor de propiedades extendidas se determina a través de las propiedades de característica ReliableWrites y WritableAuxiliaries.

> Intentar crear un descriptor reservado producirá una excepción.

> Ten en cuenta que la difusión no se admite en estos momentos.  Especificar el objeto GattCharacteristicProperty de difusión producirá una excepción.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Crear la jerarquía de servicios y características
El objeto GattServiceProvider se usa para crear y anunciar la definición de servicio principal raíz.  Cada servicio requiere su propio objeto ServiceProvider que acepta un GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Los servicios principales son el nivel superior del árbol de GATT. Los servicios principales contienen características, así como otros servicios (denominados servicios incluidos o secundarios). 

Ahora, rellena el servicio con las características y descriptores necesarios:

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
Como se indicó anteriormente, también es un buen lugar para declarar los controladores de eventos para las operaciones que admite cada característica.  Para responder correctamente a las solicitudes, una aplicación debe definir y establecer un controlador de eventos para cada tipo de solicitud que el atributo admite.  Si no se registra un controlador, el resultado será que el sistema completará la solicitud inmediatamente con *UnlikelyError*.

### <a name="constant-characteristics"></a>Características constantes
En ocasiones, hay valores de característica que no cambiarán durante el ciclo de vida de la aplicación. En ese caso, es aconsejable declarar una característica constante para impedir la activación innecesaria de la aplicación: 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>Publicar el servicio
Una vez que el servicio se haya definido completamente, el siguiente paso es publicar la compatibilidad para el servicio. Esto informa al sistema operativo que el servicio se debe devolver cuando dispositivos remotos realizan la detección de servicios.  Tendrás que establecemos dos propiedades, IsDiscoverable y IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: Anuncia el nombre descriptivo en dispositivos remotos en el anuncio, impide que pueda detectar el dispositivo.
- **IsConnectable**:  Anuncia un anuncio conectable para su uso en función de periférico.

> Cuando un servicio es reconocible y conectable, el sistema agregará el UUID del servicio para el paquete de anuncio.  El paquete de anuncio contiene 31 bytes y un UUID de 128 bits ocupa 16 de ellos.

> Ten en cuenta que cuando un servicio se publica en primer plano, una aplicación debe llamar a StopAdvertising cuando se suspenda.

## <a name="respond-to-read-and-write-requests"></a>Responder a solicitudes de lectura y escritura
Como vimos anteriormente, mientras se declaran las características necesarias, el objeto GattLocalCharacteristics tiene 3 tipos de eventos: ReadRequested, WriteRequested y SubscribedClientsChanged.

### <a name="read"></a>Leer
Cuando un dispositivo remoto intenta leer un valor de una característica (y no es un valor constante), se llama al evento ReadRequested. La característica para la que se llamó al método read, así como los argumentos (que contienen información sobre el dispositivo remoto) se pasan al delegado: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>Escritura
Cuando un dispositivo remoto intenta escribir un valor en una característica, se llama al evento WriteRequested con detalles sobre el dispositivo remoto, qué característica se deben escribir y el valor en sí: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
Hay 2 tipos de escrituras: con y sin respuesta. Usa GattWriteOption (una propiedad en el objeto GattWriteRequest) para saber qué tipo de escritura realiza el dispositivo remoto. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificaciones a los clientes suscritos
Las notificaciones son las operaciones de servidor de GATT más frecuentes y realizan la función crítica de insertar datos en los dispositivos remotos. En ocasiones, es recomendable notificar a todos los clientes suscritos, pero en otras, es aconsejable elegir a qué dispositivos enviar el nuevo valor: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Cuando un dispositivo nuevo se suscribe a las notificaciones, se llama al evento SubscribedClientsChanged: 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> Ten en cuenta que una aplicación puede obtener el tamaño máximo de notificaciones para un cliente determinado mediante la propiedad MaxNotificationSize.  El sistema truncará todos los datos que tengan un tamaño mayor al tamaño máximo.

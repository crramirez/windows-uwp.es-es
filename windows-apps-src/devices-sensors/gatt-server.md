---
author: msatranjr
title: Servidor GATT de Bluetooth
description: Este artículo se proporciona una visión general de servidor de perfil de atributo genérico (GATT) de Bluetooth para las aplicaciones de la plataforma Universal de Windows (UWP), junto con el código de muestra para casos de uso común.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b8a941b7b80bd5d34e88798ec586d9c1d52e2887
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7163096"
---
# <a name="bluetooth-gatt-server"></a>Servidor GATT de Bluetooth


**API importantes**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


En este artículo muestra las API de servidor de atributo genérico (GATT) de Bluetooth para las aplicaciones de la plataforma Universal de Windows (UWP), junto con el código de ejemplo para tareas comunes de servidor GATT: 
- Definir los servicios admitidos
- Publicar el servidor para que se pueda descubrir los clientes remotos
- Soporte técnico para el servicio se anuncian
- Responder para leer y escribir solicitudes
- Enviar notificaciones a los clientes suscritos

## <a name="overview"></a>Introducción
Windows funciona normalmente en el rol de cliente. No obstante, muchos escenarios surgen que requieren Windows para que actúe como Bluetooth LE GATT servidor también. Casi todos los escenarios en los dispositivos de IoT, junto con la mayoría de las comunicaciones entre plataformas BLE requiere Windows en un servidor GATT. Además, enviar notificaciones a dispositivos transportable cercanos ha convertido en un escenario popular que requiere esta tecnología también.  
> Asegúrese de que todos los conceptos de los [documentos de cliente GATT](gatt-client.md) sean claros antes de continuar.  

Las operaciones de servidor se giran alrededor del proveedor de servicios y la GattLocalCharacteristic. Estas dos clases proporcionará la funcionalidad necesaria para declarar, implementar y exponer una jerarquía de datos en un dispositivo remoto.

## <a name="define-the-supported-services"></a>Definir los servicios admitidos
La aplicación puede declarar uno o varios servicios que se publique por Windows. Cada servicio se identifica por un UUID. 

### <a name="attributes-and-uuids"></a>Atributos y UUID
Cada servicio, la característica y el descriptor se definen por es propio único UUID de 128 bits.
> Todas las API de Windows usa el término GUID, pero el estándar de Bluetooth define como UUID. Para nuestros propósitos, estos dos términos son intercambiables para continuaremos usando el término UUID. 

Si el atributo es estándar y están definidos por definido por el SIG de Bluetooth, también tendrán un identificador breve de 16 bits correspondiente (por ejemplo, UUID del nivel de batería es 0000**2A19**-0000-1000-8000-00805F9B34FB y el identificador corto es 0x2A19). Estos UUID estándar puede verse en [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) y [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx).

Si está implementando la aplicación es propio servicio personalizado, un UUID personalizado se han de generarse. Esto es fácilmente en Visual Studio a través de herramientas -> CreateGuid (uso opción 5 acceder a ella en el formato "xxxxxxxx-xxxx-… xxxx"). Este uuid ahora puede usarse para declarar descriptores, las características o nuevos servicios locales.

#### <a name="restricted-services"></a>Servicios restringidos
Los siguientes servicios están reservados para el sistema y no se puede publicar en este momento:
1. Servicio de información de dispositivo (DIS)
2. Servicio de perfil de atributo genérico (GATT)
3. Servicio de perfil de acceso genérico (BPA)
4. Servicio de dispositivos de interfaz humana (HOGP)
5. Analizar parámetros de servicio (SCP)

> Al intentar crear un servicio bloqueado dará como resultado BluetoothError.DisabledByPolicy devueltos por la llamada a CreateAsync.

#### <a name="generated-attributes"></a>Atributos generados
Los descriptores siguientes son generado automáticamente por el sistema, en función de la GattLocalCharacteristicParameters proporcionado durante la creación de la característica:
1. Configuración de característica de cliente (si la característica está marcada como indicatable u obligatoria).
2. Descripción característico de usuario (si se establece la propiedad UserDescription). Consulta la propiedad GattLocalCharacteristicParameters.UserDescription para obtener más información.
3. Formato característico (un descriptor para cada formato de presentación especificado).  Consulta la propiedad GattLocalCharacteristicParameters.PresentationFormats para obtener más información.
4. Formato agregados característico (si se especifica más de un formato de presentación).  Propiedad GattLocalCharacteristicParameters.See PresentationFormats para obtener más información.
5. Propiedades extendidas característico (si la característica está marcada con el bit de las propiedades extendidas).

> El valor del descriptor de propiedades extendidas se determina mediante las propiedades de característica ReliableWrites y WritableAuxiliaries.

> Al intentar crear un descriptor reservado dará como resultado una excepción.

> Ten en cuenta que la difusión no se admite en este momento.  Especificar el GattCharacteristicProperty difusión dará como resultado una excepción.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>Crea la jerarquía de servicios y características
El GattServiceProvider se usa para crear y anunciar la definición del servicio principal de raíz.  Cada servicio requiere es propio objeto ServiceProvider que toma un GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Servicios principales son el nivel superior del árbol GATT. Servicios principales contienen las características, así como otros servicios (denominados 'Incluido' o servicios secundarios). 

Ahora, rellena el servicio con las características necesarias y descriptores:

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
Como se indicó anteriormente, también es un buen lugar para declarar los controladores de eventos para las operaciones que admite cada característica.  Para responder a solicitudes correctamente, una aplicación debe definido y establece un controlador de eventos para el atributo es compatible con cada tipo de solicitud.  Si no se registra un controlador dará como resultado la solicitud se completó inmediatamente con *UnlikelyError* por el sistema.

### <a name="constant-characteristics"></a>Características constantes
A veces, hay valores de características que no cambie durante el transcurso de ciclo de vida de la aplicación. En ese caso, es aconsejable declarar una característica de constante para evitar la activación de aplicaciones innecesarias: 

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
Una vez que el servicio se ha definido por completo, el siguiente paso es publicar compatibilidad para el servicio. Esto informa que el sistema operativo que se debe devolver el servicio cuando dispositivos remotos realizan una detección de servicios.  Tendrás que establecer dos propiedades - IsDiscoverable y IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: anuncia el nombre descriptivo a dispositivos remotos en el anuncio, hacer que el dispositivo reconocibles.
- **IsConnectable**: anuncia un anuncio para su uso en la función periférica conectable.

> Cuando un servicio es reconocible y Connectable, el sistema agregará el Uuid del servicio para el paquete de anuncio.  Hay solo 31 bytes en el paquete de anuncio y un UUID de 128 bits ocupa 16 de ellos.

> Ten en cuenta que cuando se publica un servicio en primer plano, una aplicación debe llamar a StopAdvertising cuando se suspende la aplicación.

## <a name="respond-to-read-and-write-requests"></a>Responder para leer y escribir solicitudes
Como hemos visto anteriormente, mientras que al declarar las características necesarias, GattLocalCharacteristics tiene 3 tipos de eventos - ReadRequested, WriteRequested y SubscribedClientsChanged.

### <a name="read"></a>Leer
Cuando un dispositivo remoto intenta leer un valor de una característica (y no es un valor constante), se llama al evento de ReadRequested. La característica que se llamó a la operación de lectura en, así como argumentos (que contiene información sobre el dispositivo remoto) se pasa al delegado: 

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
Cuando un dispositivo remoto intenta escribir un valor a una característica, se llama al evento de WriteRequested con detalles sobre el dispositivo remoto, qué característica para escribir en y el propio valor: 

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
Existen 2 tipos de escrituras - con y sin respuesta. Usar GattWriteOption (una propiedad en el objeto GattWriteRequest) para averiguar qué tipo de escritura que está realizando el dispositivo remoto. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificaciones a los clientes suscritos
Más frecuente de las operaciones de servidor GATT, las notificaciones de realiza la función crítica de inserción de datos en los dispositivos remotos. En ocasiones, querrás notificar a todos los clientes suscritos pero othertimes es posible que quieras seleccionar qué dispositivos para enviar el nuevo valor: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Cuando un nuevo dispositivo se suscribe a las notificaciones, se llama el evento SubscribedClientsChanged: 

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
> Ten en cuenta que una aplicación puede obtener el tamaño máximo de notificación para un cliente determinado con la propiedad MaxNotificationSize.  Los datos más grandes que el tamaño máximo se truncará por el sistema.

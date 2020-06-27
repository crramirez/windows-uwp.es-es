---
title: Servidor Bluetooth GATT
description: En este artículo se proporciona información general sobre el servidor de Perfil de atributo genérico (GATT) de Bluetooth para aplicaciones Plataforma universal de Windows (UWP), junto con código de ejemplo para casos de uso comunes.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 65a4643e6a73e0eb015fc40c7354d0cd307fa0d1
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469550"
---
# <a name="bluetooth-gatt-server"></a>Servidor Bluetooth GATT

En este artículo se muestran las API de servidor de atributo genérico (GATT) de Bluetooth para aplicaciones Plataforma universal de Windows (UWP), junto con código de ejemplo para las tareas comunes del servidor GATT:

- Definir los servicios admitidos
- Publicar servidor para que los clientes remotos puedan detectarlos
- Anunciar soporte técnico para el servicio
- Responder a las solicitudes de lectura y escritura
- Enviar notificaciones a los clientes suscritos

> [!Important]
> Debe declarar la funcionalidad "Bluetooth" en *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

**API importantes**

- [**Windows. Devices. Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows. Devices. Bluetooth. GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Información general

Windows suele funcionar en el rol de cliente. No obstante, se producen muchos escenarios que requieren que Windows actúe también como un servidor de Bluetooth LE GATT. Casi todos los escenarios de los dispositivos de IoT, junto con la mayoría de las comunicaciones de BLE multiplataforma, requerirán que Windows sea un servidor GATT. Además, el envío de notificaciones a dispositivos portátil cercanos se ha convertido en un escenario popular que requiere también esta tecnología.  

Las operaciones del servidor girarán alrededor del proveedor de servicios y del GattLocalCharacteristic. Estas dos clases proporcionarán la funcionalidad necesaria para declarar, implementar y exponer una jerarquía de datos en un dispositivo remoto.

## <a name="define-the-supported-services"></a>Definir los servicios admitidos
La aplicación puede declarar uno o varios servicios que se van a publicar en Windows. Cada servicio se identifica de forma única mediante un UUID.

### <a name="attributes-and-uuids"></a>Atributos y UUID
Cada servicio, característica y descriptor se define por su propio UUID único de 128 bits.
> Todas las API de Windows usan el término GUID, pero el estándar Bluetooth los define como UUID. Para nuestros fines, estos dos términos son intercambiables, por lo que seguiremos usando el término UUID. 

Si el atributo es estándar y está definido por el firma de Bluetooth, también tendrá un identificador corto de 16 bits correspondiente (por ejemplo, el UUID de nivel de batería es 0000**2A19**-0000-1000-8000-00805F9B34FB y el identificador corto es 0x2A19). Estos UUID estándar pueden verse en [GattServiceUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) y [GattCharacteristicUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids).

Si su aplicación está implementando su propio servicio personalizado, se tendrá que generar un UUID personalizado. Esto se realiza fácilmente en Visual Studio a través de herramientas-> CreateGuid (use la opción 5 para obtenerla en "xxxxxxxx-xxxx-... XXXX "formato). Este UUID se puede usar ahora para declarar nuevos servicios locales, características o descriptores.

#### <a name="restricted-services"></a>Servicios restringidos
Los servicios siguientes están reservados por el sistema y no se pueden publicar en este momento:
1. Servicio de información de dispositivos (DIS)
2. Servicio de Perfil de atributo genérico (GATT)
3. Servicio de Perfil de acceso genérico (GAP)
4. Servicio de HID (HOGP)
5. Servicio de parámetros de análisis (SCP)

> Al intentar crear un servicio bloqueado, se devolverá BluetoothError. DisabledByPolicy desde la llamada a CreateAsync.

#### <a name="generated-attributes"></a>Atributos generados
El sistema genera automáticamente los siguientes descriptores, según el GattLocalCharacteristicParameters proporcionado durante la creación de la característica:
1. Configuración de características de cliente (si la característica está marcada como indicatable o notificable).
2. Descripción del usuario de la característica (si se establece la propiedad UserDescription). Consulte la propiedad GattLocalCharacteristicParameters. UserDescription para obtener más información.
3. Formato característico (un descriptor para cada formato de presentación especificado).  Consulte la propiedad GattLocalCharacteristicParameters. PresentationFormats para obtener más información.
4. Formato de agregado característico (si se especifica más de un formato de presentación).  GattLocalCharacteristicParameters. Consulte la propiedad PresentationFormats para obtener más información.
5. Propiedades extendidas de características (si la característica está marcada con el bit de las propiedades extendidas).

> El valor del descriptor de propiedades extendidas se determina mediante las propiedades de la característica ReliableWrites y WritableAuxiliaries.

> Si se intenta crear un descriptor reservado, se producirá una excepción.

> Tenga en cuenta que en este momento no se admite la difusión.  Si se especifica el GattCharacteristicProperty de difusión, se producirá una excepción.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Crear la jerarquía de servicios y características
GattServiceProvider se usa para crear y anunciar la definición del servicio principal raíz.  Cada servicio requiere su propio objeto ServiceProvider que toma un GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Los servicios principales son el nivel superior del árbol GATT. Los servicios principales contienen características y otros servicios (denominados "incluidos" o servicios secundarios). 

Ahora, rellene el servicio con las características y los descriptores necesarios:

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
Como se mostró anteriormente, también es un buen lugar para declarar controladores de eventos para las operaciones que cada característica admite.  Para responder a las solicitudes correctamente, debe definirse una aplicación y establecer un controlador de eventos para cada tipo de solicitud que el atributo admita.  Si no se registra un controlador, el sistema finalizará inmediatamente la solicitud con *UnlikelyError* .

### <a name="constant-characteristics"></a>Características constantes
A veces, hay valores característicos que no cambiarán durante el transcurso de la duración de la aplicación. En ese caso, es aconsejable declarar una característica constante para evitar la activación de aplicaciones innecesaria: 

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
Una vez que el servicio se ha definido totalmente, el paso siguiente es publicar la compatibilidad para el servicio. Esto informa al sistema operativo que se debe devolver el servicio cuando los dispositivos remotos realizan una detección de servicios.  Tendrá que establecer dos propiedades: IsDiscoverable y IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: anuncia el nombre descriptivo de los dispositivos remotos en el anuncio, lo que permite que el dispositivo se pueda detectar.
- **IsConnectable**: anuncia un anuncio conectable para su uso en el rol periférico.

> Cuando un servicio es reconocible y conectable, el sistema agregará el UUID del servicio al paquete de anuncios.  Solo hay 31 bytes en el paquete de anuncios y un UUID de 128 bits ocupa 16 de ellos.

> Tenga en cuenta que cuando se publica un servicio en primer plano, una aplicación debe llamar a StopAdvertising cuando la aplicación se suspende.

## <a name="respond-to-read-and-write-requests"></a>Responder a las solicitudes de lectura y escritura
Como vimos anteriormente al declarar las características necesarias, GattLocalCharacteristics tiene 3 tipos de eventos: ReadRequested, WriteRequested y SubscribedClientsChanged.

### <a name="read"></a>Lectura
Cuando un dispositivo remoto intenta leer un valor de una característica (y no es un valor constante), se llama al evento ReadRequested. La característica a la que se llamó la lectura y los argumentos (que contienen información sobre el dispositivo remoto) se pasan al delegado: 

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
Cuando un dispositivo remoto intenta escribir un valor en una característica, se llama al evento WriteRequested con detalles sobre el dispositivo remoto, la característica en la que se va a escribir y el propio valor: 

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
Hay 2 tipos de escrituras: con y sin respuesta. Use GattWriteOption (una propiedad en el objeto GattWriteRequest) para averiguar qué tipo de escritura está realizando el dispositivo remoto. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificaciones a los clientes suscritos
La mayoría de las operaciones del servidor GATT, las notificaciones realizan la función crítica de insertar datos en los dispositivos remotos. A veces, querrá notificar a todos los clientes suscritos, pero othertimes puede elegir a qué dispositivos se va a enviar el nuevo valor: 

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
> Tenga en cuenta que una aplicación puede obtener el tamaño máximo de notificación para un cliente determinado con la propiedad MaxNotificationSize.  El sistema truncará los datos mayores que el tamaño máximo.

---
author: msatranjr
title: Servidor de GATT Bluetooth
description: En este artículo se proporciona una visión general del servidor de perfiles de atributo genérico (GATT) de Bluetooth para las aplicaciones de la plataforma de Windows Universal (UWP), junto con el código de ejemplo para casos de uso comunes.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 27154fbb535b76995fba97702e65a9c0b2a8291c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "610770"
---
# <a name="bluetooth-gatt-server"></a>Servidor de GATT Bluetooth


**API importantes**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


En este artículo se muestra cómo las API de servidor de atributo genérico de Bluetooth (GATT) para las aplicaciones de la plataforma de Windows Universal (UWP), junto con el código de ejemplo para tareas comunes de servidor GATT: 
- Definir los servicios compatibles
- Publicar el servidor de modo que puedan ser detectado por los clientes remotos
- Anunciar la compatibilidad para el servicio
- Responder para leer y escribir solicitudes
- Enviar notificaciones a los clientes está suscritos

## <a name="overview"></a>Introducción
Windows normalmente funciona en el rol de cliente. No obstante, muchos escenarios surgen que requieren Windows para que actúe como un Bluetooth LE GATT también al servidor. Casi todos los escenarios para los dispositivos IoT, junto con la mayoría de las comunicaciones entre plataformas habilitar requiere Windows para que sea un servidor GATT. Además, el envío de notificaciones a dispositivos wearable cercanos ha convertido en un escenario más popular que requiere esta tecnología así como.  
> Asegúrese de que todos los conceptos en el [cliente GATT documentos](gatt-client.md) estén borrar antes de continuar.  

Las operaciones del servidor girarán en torno al proveedor de servicios y la GattLocalCharacteristic. Estas dos clases proporcionan la funcionalidad necesaria para declarar, implementar y exponer una jerarquía de datos a un dispositivo remoto.

## <a name="define-the-supported-services"></a>Definir los servicios compatibles
La aplicación puede declarar uno o más servicios que se publicará por parte de Windows. Cada servicio se identifica mediante un UUID. 

### <a name="attributes-and-uuids"></a>Los atributos y los identificadores UUID
Cada servicio, la característica y el descriptor se definen por es propio UUID único de 128 bits.
> Todas las API de Windows utilizan el término GUID, pero el estándar Bluetooth define estos como UUID. Para nuestros fines, estos dos términos son intercambiables, por lo que vamos a continuar usando el término UUID. 

Si el atributo es estándar y está definido por definido por el SIG de Bluetooth, también tendrá un identificador corto de 16 bits correspondiente (por ejemplo, UUID de nivel de batería es**2A19**de 0000-0000-1000-8000-00805F9B34FB y el identificador corto es 0x2A19). Estos UUID estándar puede verse en [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) y [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx).

Si está implementando la aplicación es propio servicio personalizado, tendrá un UUID personalizado que se genere. Esto es fácilmente en Visual Studio a través de herramientas -> CreateGuid (opción de usar 5 para obtener en el formato "xxxxxxxx-xxxx-... xxxx"). Ya se puede usar este uuid para declarar descriptores, características o servicios locales de nuevo.

#### <a name="restricted-services"></a>Servicios restringidos
Los siguientes servicios están reservados por el sistema y no se pueden publicar en este momento:
1. Servicio de información de dispositivo (DIS)
2. Servicio de perfiles de atributo genérico (GATT)
3. Servicio de perfiles de acceso genérico (espacio)
4. Servicio de dispositivos de interfaz humana (HOGP)
5. Analizar los parámetros servicio (SCP)

> Intenta crear un servicio bloqueado, se producirá en BluetoothError.DisabledByPolicy que se devuelven desde la llamada a CreateAsync.

#### <a name="generated-attributes"></a>Atributos generados
Los descriptores de siguientes son generado automáticamente por el sistema, en función de la GattLocalCharacteristicParameters proporcionado durante la creación de la característica:
1. Configuración de características de cliente (si la característica está marcada como indicatable u obligatoria).
2. Característica descripción de usuario (si se establece la propiedad UserDescription). Vea la propiedad GattLocalCharacteristicParameters.UserDescription para obtener más información.
3. Formato de característica (un descriptor para cada formato de presentación especificado).  Vea la propiedad GattLocalCharacteristicParameters.PresentationFormats para obtener más información.
4. Formato agregado característicos (si se especifica más de un formato de presentación).  Propiedad GattLocalCharacteristicParameters.See PresentationFormats para obtener más información.
5. Propiedades extendidas característicos (si la característica está marcada con el bit de las propiedades extendidas).

> El valor del descriptor de propiedades extendidas se determina a través de las propiedades de característica ReliableWrites y WritableAuxiliaries.

> Intenta crear un descriptor de reservado, se producirá una excepción.

> Tenga en cuenta que de difusión no se admite en este momento.  Especificación de la GattCharacteristicProperty de difusión, se producirá una excepción.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>Creación de la jerarquía de servicios y características
El GattServiceProvider se usa para crear y anunciar la definición del servicio principal raíz.  Cada servicio requiere es propio objeto ServiceProvider que toma un identificador de GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Servicios principales son el nivel superior del árbol de GATT. Servicios principales contienen características, así como otros servicios (denominadas 'Included' o servicios secundarios). 

Ahora, rellene el servicio con las características necesarias y descriptores de:

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
Como se indicó anteriormente, también es un buen lugar para declarar controladores de eventos para las operaciones que admite cada característica.  Para responder a las solicitudes de correctamente, una aplicación debe definido y establecer un controlador de eventos para cada tipo de solicitud es compatible con el atributo.  Error al registrar un controlador, se producirá la solicitud se completen inmediatamente con *UnlikelyError* por el sistema.

### <a name="constant-characteristics"></a>Características de constantes
A veces, hay valores característicos que no cambiará durante el transcurso de la duración de la aplicación. En ese caso, es aconsejable declarar una constante característica para evitar que la activación de la aplicación innecesarios: 

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
Una vez que el servicio se ha definido totalmente, el siguiente paso es publicar el soporte técnico para el servicio. Esto le informa que el sistema operativo que se debe devolver el servicio cuando dispositivos remotos realizan una detección del servicio.  Tendrá que establecer dos propiedades - IsDiscoverable y IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: anuncia el nombre descriptivo para dispositivos remotos en el anuncio, hacer que el dispositivo que se pueda detectar.
- **IsConnectable**: anuncia un anuncio conectable para su uso en función de periférico.

> Cuando un servicio se detectable y Connectable, el sistema agregará el Uuid de servicio para el paquete de anuncio.  Hay sólo 31 bytes en el paquete de anuncios y un UUID de 128 bits ocupa 16 de ellos!

> Tenga en cuenta que cuando se publica un servicio en primer plano, una aplicación debe llamar a StopAdvertising cuando se suspende la aplicación.

## <a name="respond-to-read-and-write-requests"></a>Responder para leer y escribir solicitudes
Tal y como se ha visto anteriormente mientras que al declarar las características necesarias, GattLocalCharacteristics tener 3 tipos de eventos - ReadRequested, WriteRequested y SubscribedClientsChanged.

### <a name="read"></a>Leer
Cuando un dispositivo remoto intenta leer un valor de una característica (y no es un valor constante), se llama al evento ReadRequested. La característica que se ha llamado en el nivel de lectura, así como args (que contiene información sobre el dispositivo remoto) se pasa al delegado: 

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
Cuando un dispositivo remoto intenta escribir un valor en una característica, se llama al evento WriteRequested con detalles sobre el dispositivo remoto, qué característica para escribir en y el propio valor: 

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
Existen 2 tipos de escrituras - con y sin respuesta. Use GattWriteOption (una propiedad en el objeto GattWriteRequest) para averiguar qué tipo de escritura que está llevando a cabo el dispositivo remoto. 

## <a name="send-notifications-to-subscribed-clients"></a>Enviar notificaciones a los clientes está suscritos
Más frecuentes de las operaciones de servidor del GATT, las notificaciones de realizan la función de inserción de datos en los dispositivos remotos crítica. A veces, querrá notificar a todos los clientes suscritos pero othertimes es posible que desee elegir qué dispositivos para enviar el nuevo valor para: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Cuando un dispositivo nuevo se suscribe a las notificaciones, se llama el evento SubscribedClientsChanged: 

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
> Tenga en cuenta que una aplicación puede obtener el tamaño máximo de notificación para un cliente concreto con la propiedad MaxNotificationSize.  Los datos que supera el tamaño máximo se truncarán por el sistema.

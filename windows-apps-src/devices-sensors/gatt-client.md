---
author: msatranjr
title: Cliente de GATT Bluetooth
description: En este artículo se proporciona una visión general del cliente de perfil de atributo genérico (GATT) de Bluetooth para las aplicaciones de la plataforma de Windows Universal (UWP), junto con el código de ejemplo para casos de uso comunes.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 555fec6d534c07898acd911f9cd84a11ac66dcd8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "305330"
---
# <a name="bluetooth-gatt-client"></a>Cliente de GATT Bluetooth


**API importantes**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

En este artículo se muestra el uso de las API de cliente de atributo genérico de Bluetooth (GATT) para las aplicaciones de la plataforma de Windows Universal (UWP), junto con el código de ejemplo para tareas comunes de cliente GATT:
- Consulta para dispositivos cercanos
- Conectar con el dispositivo
- Enumerar los servicios compatibles y características del dispositivo
- Lectura y escritura a una característica
- Suscríbase para cambia el valor de notificaciones cuando característicos

## <a name="overview"></a>Introducción
Los desarrolladores pueden usar las API en el espacio de nombres [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) para tener acceso a dispositivos Bluetooth LE. Los dispositivos BluetoothLE exponen su funcionalidad a través de una colección de:

-   Servicios
-   Características
-   Descriptores

Servicios de definición el contrato funcional del dispositivo LE y contengan una colección de características que definen el servicio. Dichas características, a su vez, contienen descriptores que las describen. Estos 3 términos forma genérica se conocen como los atributos de un dispositivo.

Las API de Bluetooth LE GATT exponer objetos y funciones en lugar de obtener acceso al transporte sin procesar. Las API de GATT también permiten a los programadores trabajar con dispositivos Bluetooth LE con la capacidad de realizar las siguientes tareas:

-   Realizar la detección de atributos
-   Valores de atributo de escritura y lectura
-   Registrar una devolución de llamada para el evento ValueChanged de característica

Para crear una implementación útil un programador debe tener conocimientos previos de los servicios de GATT y características que tenga la intención de la aplicación para consumir y al proceso de manera que los datos binarios proporcionados por la API se transforman en los valores de la característica específica datos útiles antes de que se presentan al usuario. Las API de Bluetooth GATT exponen solo los primitivos básicos requeridos para comunicarse con un dispositivo BluetoothLE. Para interpretar los datos, debe definirse un perfil de aplicación, ya sea mediante un perfil estándar de un SIG de Bluetooth o mediante un perfil personalizado implementado por un proveedor de dispositivos. Un perfil crea un contrato vinculante entre la aplicación y el dispositivo, que indica qué representan los datos intercambiados y cómo interpretarlos.

Para mayor comodidad, el SIG de Bluetooth ofrece una [lista de perfiles públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec).

## <a name="query-for-nearby-devices"></a>Consulta para dispositivos cercanos
Existen dos métodos principales para consultar dispositivos cercanos:
- DeviceWatcher en Windows.Devices.Enumeration
- AdvertisementWatcher en Windows.Devices.Bluetooth.Advertisement

Se explica el método 2ª extensamente en la documentación de [anuncio](ble-beacon.md) por lo que no se tratarán mucho aquí, pero la idea básica es para buscar la dirección Bluetooth de dispositivos cercanos que satisfacen el [Filtro de anuncio](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)de determinado. Una vez que tenga la dirección, se puede llamar a [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) para obtener una referencia al dispositivo. 

Ahora, hacer una copia en el método DeviceWatcher. Un dispositivo Bluetooth LE es igual que cualquier otro dispositivo en Windows y se puede consultar mediante las [API de enumeración](https://msdn.microsoft.com/library/windows/apps/BR225459). Utilice la clase [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) y pasar una cadena de consulta que especifica los dispositivos que se busca: 

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```
Una vez que ha iniciado la DeviceWatcher, recibirá [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) para cada dispositivo que satisfaga la consulta en el controlador para el evento [se ha agregado](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) para los dispositivos en cuestión. Para obtener una vista más detallada en DeviceWatcher vea la completa de ejemplo [en depósito](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Conectar con el dispositivo
Una vez que se detecta un dispositivo deseado, usar el [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) para obtener el objeto Bluetooth LE dispositivo para el dispositivo en cuestión: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
Por otro lado, eliminación de todas las referencias a un BluetoothLEDevice objeto para un dispositivo (y si ninguna otra aplicación en el sistema tiene una referencia al dispositivo) activarán automáticos desconectar después de un período de tiempo de espera pequeñas. 

```csharp
bluetoothLeDevice.Dispose();
```
Si la aplicación necesita obtener acceso al dispositivo nuevo, basta con volver a crear el objeto de dispositivo y obtener acceso a una característica (Esto se describe en la siguiente sección) se activará el sistema operativo para volver a conectarse cuando sea necesario. Si el dispositivo está cercano, obtendrá acceso al dispositivo en caso contrario, que devolverá con un error de DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Enumerar servicios compatibles y características
Ahora que tiene un objeto BluetoothLEDevice, el siguiente paso es descubrir qué datos expone el dispositivo. El primer paso para hacer esto es para los servicios de consulta: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Una vez que se ha identificado el servicio de interés, el paso siguiente es para las características de consulta. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
El sistema operativo devuelve que una lista de sólo lectura de GattCharacteristic que, a continuación, puede realizar operaciones en los objetos.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Realizar operaciones de lectura y escritura en una característica

La característica es que la unidad fundamental de GATT en función de comunicación. Contiene un valor que representa una parte distinta de datos en el dispositivo. Por ejemplo, la característica de nivel de la batería tiene un valor que representa el nivel de la batería del dispositivo.

Leer las propiedades de característica para determinar las operaciones que se admiten:
```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

Si se admite la lectura, puede leer el valor: 
```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```
Escritura en una característica sigue un modelo similar: 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **Sugerencia**: obtener cómodos con el uso de [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) y [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Su funcionalidad será indispensable cuando se trabaja con los búferes sin procesar que obtener desde muchas de las APIs Bluetooth. 
## <a name="subscribing-for-notifications"></a>Para las notificaciones de la suscripción

Asegúrese de que la característica admite indicar o Notify (compruebe las propiedades de característica para asegurarse de que). 

> **Reservar**: indicar se considera más confiable porque cada evento ha cambiado el valor se combina con una confirmación desde el dispositivo de cliente. Notificar a es más frecuente debido a que la mayoría de las transacciones GATT tendría en su lugar conservar la energía en lugar de ser extremadamente confiable. En cualquier caso, todos los se administra en el nivel de controlador no obtener necesarios para la aplicación. Colectivamente nos referiremos a ellos como simplemente "notificaciones" pero ahora sabe. 

Hay dos aspectos que se encargan de antes de obtener las notificaciones:
- Escribir en el Descriptor de configuración de características de cliente (CCCD)
- Controlar el evento Characteristic.ValueChanged

Escritura en la CCCD indica que el dispositivo del servidor que este cliente desea conocer cada vez que cambia el valor de característica determinada. Para ello: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
Ahora, evento ValueChanged del GattCharacteristic se llamará a cada vez que se obtiene cambia el valor en el dispositivo remoto. Lo único que queda es implementar el controlador: 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```



---
author: msatranjr
title: Cliente GATT de Bluetooth
description: Este artículo se proporciona una visión general de cliente de perfil de atributo genérico (GATT) de Bluetooth para las aplicaciones de la plataforma Universal de Windows (UWP), junto con el código de muestra para casos de uso común.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 345e6f82ddf97c2595dad0029ca432f075a6190b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7572708"
---
# <a name="bluetooth-gatt-client"></a>Cliente GATT de Bluetooth


**API importantes**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

En este artículo se muestra el uso de las API de cliente de atributo genérico (GATT) de Bluetooth para las aplicaciones de la plataforma Universal de Windows (UWP), junto con el código de ejemplo para tareas de cliente GATT comunes:
- Consulta dispositivos cercanos
- Conectar con el dispositivo
- Enumerar las características del dispositivo y los servicios admitidos
- Leer y escribir en una característica
- Suscribir cambia el valor de notificaciones cuando la característica

## <a name="overview"></a>Introducción
Los desarrolladores pueden usar las API del espacio de nombres [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) para tener acceso a dispositivos Bluetooth LE. Los dispositivos BluetoothLE exponen su funcionalidad a través de una colección de:

-   Servicios
-   Características
-   Descriptores

Servicios de definen el contrato funcional del dispositivo LE y contienen una colección de características que definen el servicio. Dichas características, a su vez, contienen descriptores que las describen. Estos 3 términos detector se conocen como los atributos de un dispositivo.

La API de Bluetooth LE GATT exponen objetos y funciones, en lugar de se acceso al transporte sin procesar. Las API de GATT también permiten a los desarrolladores trabajar con dispositivos Bluetooth LE con la capacidad de realizar las siguientes tareas:

-   Realizar la detección de atributos
-   Leer y escribir valores de atributo
-   Registrar una devolución de llamada para el evento de característica ValueChanged

Para crear una implementación útil el desarrollador debe tener conocimientos previos de los servicios GATT y características de que la aplicación pretende consumir y debe procesar los valores de las características específicas que los datos binarios proporcionados por la API se conviertan en obtener datos útiles antes de que se presenta al usuario. Las API de Bluetooth GATT exponen solo los primitivos básicos requeridos para comunicarse con un dispositivo BluetoothLE. Para interpretar los datos, debe definirse un perfil de aplicación, ya sea mediante un perfil estándar de un SIG de Bluetooth o mediante un perfil personalizado implementado por un proveedor de dispositivos. Un perfil crea un contrato vinculante entre la aplicación y el dispositivo, que indica qué representan los datos intercambiados y cómo interpretarlos.

Para mayor comodidad, el SIG de Bluetooth ofrece una [lista de perfiles públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec).

## <a name="query-for-nearby-devices"></a>Consulta dispositivos cercanos
Hay dos métodos principales para consultar los dispositivos cercanos:
- DeviceWatcher en Windows.Devices.Enumeration
- AdvertisementWatcher en Windows.Devices.Bluetooth.Advertisement

Se explica el método 2a ampliamente en la documentación de [anuncio](ble-beacon.md) por lo que no que se tratan mucho aquí, pero la idea básica es buscar la dirección de Bluetooth de dispositivos cercanos que satisfacen el [Filtro de anuncios](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)de determinado. Una vez que tenga la dirección, puedes llamar a [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) para obtener una referencia al dispositivo. 

Ahora, vuelve al método DeviceWatcher. Un dispositivo Bluetooth LE es igual que cualquier otro dispositivo de Windows y se puede consultar mediante las [API de enumeración](https://msdn.microsoft.com/library/windows/apps/BR225459). Usar la clase [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) y pasar una cadena de consulta especifica los dispositivos para buscar: 

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
Una vez que hayas iniciado la DeviceWatcher, recibirás [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) para cada dispositivo que cumple la consulta en el controlador para el evento [se ha agregado](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) para los dispositivos en cuestión. Para una información más detallada DeviceWatcher consulta se ha completado el de muestra [en Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Conecta el dispositivo
Una vez que se detecta un dispositivo deseado, usa el [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) para obtener el objeto de dispositivo Bluetooth LE para el dispositivo en cuestión: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
Por otro lado, eliminar todas las referencias a un BluetoothLEDevice objeto para un dispositivo (y si ninguna otra aplicación en el sistema tiene una referencia al dispositivo) desencadenan automáticos desconectar después de un período de tiempo de espera pequeño. 

```csharp
bluetoothLeDevice.Dispose();
```
Si la aplicación necesita tener acceso al dispositivo de nuevo, simplemente volver a crear el objeto de dispositivo y el acceso a una característica (se explica en la siguiente sección) se activará el sistema operativo para volver a conectar cuando sea necesario. Si el dispositivo está cerca, obtendrás acceso al dispositivo en caso contrario devolverá con un error de DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Enumerar los servicios admitidos y características
Ahora que ya tienes un objeto BluetoothLEDevice, el siguiente paso es detectar qué datos expone el dispositivo. El primer paso para hacer esto es para consultar los servicios: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Una vez que se ha identificado el servicio de interés, el siguiente paso es consultar las características. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
El sistema operativo devuelve que una lista de ReadOnly de GattCharacteristic objetos que, a continuación, puede realizar operaciones en.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Realizar operaciones de lectura y escritura en una característica

La característica es comunicación basada en la unidad fundamental de GATT. Contiene un valor que representa una parte distinta de datos en el dispositivo. Por ejemplo, la característica de nivel de batería tiene un valor que representa el nivel de batería del dispositivo.

Leer las propiedades de característica para determinar qué operaciones son compatibles:
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

Si se admite la lectura, puedes leer el valor: 
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
Escritura a una característica sigue un patrón similar: 
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
> **Sugerencia**: te sientas cómodo con el uso de [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) y [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Su funcionalidad será indispensable al trabajar con los búferes sin procesar que obtienes de muchas de las APIs de Bluetooth. 
## <a name="subscribing-for-notifications"></a>Para las notificaciones de la suscripción

Asegúrese de que la característica admite indique o Notify (comprobar las propiedades de característica para asegurarse de que). 

> **Reservar**: indicar se considera más confiable, porque cada evento ha cambiado el valor está relacionada con una confirmación desde el dispositivo cliente. Notificar es más frecuente porque la mayoría de las transacciones de GATT harían en su lugar ahorrar energía en lugar de ser extremadamente confiable. En cualquier caso, todo esto se controla en la capa del controlador para que la aplicación no se involucra. Manera colectiva nos referiremos a ellos como simplemente "notificaciones", pero ahora sabes. 

Hay dos cosas que se encargará del antes de obtener notificaciones:
- Escribir en el Descriptor de la característica de configuración de cliente (CCCD)
- Controlar el evento Characteristic.ValueChanged

Escritura en la CCCD indica que el dispositivo del servidor que este cliente quiere saber cada vez que cambia el valor de característica determinada. Para ello: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
Ahora, se llamará evento ValueChanged del GattCharacteristic cada vez que se obtiene cambia el valor en el dispositivo remoto. Lo único que queda es implementar el controlador: 

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



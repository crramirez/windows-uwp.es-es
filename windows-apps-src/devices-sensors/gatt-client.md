---
title: Cliente de GATT de Bluetooth
description: En este artículo se proporciona información general sobre el perfil del cliente de atributo genérico (GATT) de Bluetooth para las aplicaciones para la Plataforma universal de Windows (UWP), junto con código de muestra para casos de uso comunes.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 35488417497ac157969ff2641fbeaa0d4bb02591
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370122"
---
# <a name="bluetooth-gatt-client"></a>Cliente de GATT de Bluetooth


**API importantes**

-   [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

En este artículo se muestra el uso de las API del cliente de atributo genérico (GATT) de Bluetooth para las aplicaciones para la Plataforma universal de Windows (UWP), junto con código de muestra para tareas de cliente GATT comunes.
- Consulta en busca de dispositivos cercanos
- Conectar a un dispositivo
- Enumerar los servicios admitidos y las características del dispositivo
- Leer y escribir en una característica
- Suscribirse para recibir notificaciones cuando cambie el valor de una característica

## <a name="overview"></a>Información general
Los desarrolladores pueden usar las API del espacio de nombres [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) para acceder a dispositivos Bluetooth LE. Los dispositivos Bluetooth LE exponen su funcionalidad a través de una colección de:

-   Servicios
-   Características
-   Descriptores

Los servicios definen el contrato funcional del dispositivo LE y contienen una colección de características que definen el servicio. Dichas características, a su vez, contienen descriptores que las describen. Estos 3 términos se conocen genéricamente como los atributos de un dispositivo.

Las API GATT de Bluetooth LE exponen objetos y funciones, en lugar del acceso al transporte sin procesar. Las API de GATT también permiten a los desarrolladores trabajar con dispositivos Bluetooth LE, ofreciéndoles la posibilidad de realizar las siguientes tareas:

-   Detectar los atributos
-   Leer y escribir los valores de atributo
-   Registrar una devolución de llamada para el evento de característica ValueChanged

Para crear una implementación útil, el desarrollador debe tener conocimientos previos sobre los servicios y las características GATT que la aplicación pretende consumir, y debe procesar los valores de características específicos para que los datos binarios proporcionados por la API se conviertan en datos útiles antes de presentárselos al usuario. Las API de Bluetooth GATT exponen solo los primitivos básicos requeridos para comunicarse con un dispositivo Bluetooth LE. Para interpretar los datos, debe definirse un perfil de aplicación, ya sea mediante un perfil estándar de un SIG de Bluetooth o mediante un perfil personalizado implementado por un proveedor de dispositivos. Un perfil crea un contrato vinculante entre la aplicación y el dispositivo, que indica qué representan los datos intercambiados y cómo interpretarlos.

Para mayor comodidad, el SIG de Bluetooth ofrece una [lista de perfiles públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec).

## <a name="query-for-nearby-devices"></a>Consulta en busca de dispositivos cercanos
Hay dos métodos principales para realizar una consulta en busca de dispositivos cercanos:
- DeviceWatcher en Windows.Devices.Enumeration
- AdvertisementWatcher en Windows.Devices.Bluetooth.Advertisement

El segundo método se describe ampliamente en la documentación [Anuncio](ble-beacon.md), por lo que no se tratará mucho aquí. Sin embargo, el concepto básico es buscar la dirección de Bluetooth de los dispositivos cercanos que cumplan con un [filtro de anuncio](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter) determinado. Una vez que tengas la dirección, puedes llamar a [BluetoothLEDevice.FromBluetoothAddressAsync](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) para obtener una referencia al dispositivo. 

Ahora, volvamos al método DeviceWatcher. Un dispositivo Bluetooth LE es igual que cualquier otro dispositivo en Windows y puede consultarse mediante las [API de enumeración](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration). Usa la clase [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) y pasa una cadena de consulta que especifique los dispositivos que se deben buscar: 

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
Tras iniciar DeviceWatcher, recibirás un objeto [DeviceInformation](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para cada dispositivo que cumpla la consulta en el controlador del evento [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) para los dispositivos en cuestión. Para información más detallada sobre DeviceWatcher, consulta el ejemplo completo [en Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Conectarse al dispositivo
Una vez que se detecte un dispositivo deseado, usa el objeto [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) para obtener el objeto de dispositivos Bluetooth LE del dispositivo en cuestión: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
Por otro lado, al disponer de todas las referencias a un objeto BluetoothLEDevice de un dispositivo (y si ninguna otra aplicación en el sistema tiene una referencia a él), se desencadenará una desconexión automática después de un corto período de tiempo de espera. 

```csharp
bluetoothLeDevice.Dispose();
```
Si la aplicación necesita acceder al dispositivo de nuevo, basta con volver a crear el objeto de dispositivo y acceder a una característica (que se explica en la siguiente sección) hará que el sistema operativo se vuelva a conectar cuando sea necesario. Si el dispositivo está cerca, obtendrás acceso a él. De lo contrario, se devolverá con un error de DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Enumerar las características y los servicios admitidos
Ahora que ya tienes un objeto BluetoothLEDevice, el siguiente paso es descubrir qué datos expone el dispositivo. El primer paso para ello es realizar una consulta en busca de servicios: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Una vez que se haya identificado el servicio deseado, el siguiente paso es consultar en busca de características. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
El sistema operativo devuelve una lista ReadOnly de los objetos GattCharacteristic en los que puedes realizar operaciones.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Realizar operaciones de lectura y escritura en una característica

La característica es una unidad fundamental de comunicaciones basadas en GATT. Contiene un valor que representa una parte distinta de los datos del dispositivo. Por ejemplo, la característica de nivel de batería tiene un valor que representa el nivel de batería del dispositivo.

Lee las propiedades de característica para determinar qué operaciones se admiten:
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
La escritura en una característica sigue un patrón similar: 
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
> **Sugerencia**: Familiarícese con el uso de [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) y [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter). Su funcionalidad será indispensable cuando trabajes con los búferes sin procesar que recibes de muchas de las API de Bluetooth. 
## <a name="subscribing-for-notifications"></a>Suscribirse a notificaciones

Asegúrate de que la característica admita Indicar o Notificar (comprueba las propiedades de la característica). 

> **Reservar**: Indicar se considera más confiable, porque cada evento ha cambiado el valor se combina con una confirmación desde el dispositivo cliente. Notificar es más frecuente porque la mayoría de las transacciones de GATT prefieren ahorrar energía en lugar de ser extremadamente confiable. En cualquiera de los casos, todo esto se controla en el nivel del controlador, por lo que no se involucra la aplicación. Los denominaremos colectivamente "notificaciones", pero ahora ya sabes. 

Hay dos cosas que hay que tener en cuenta antes de obtener notificaciones:
- Escribir en el descriptor de característica de configuración de cliente (CCCD)
- Controlar el evento Characteristic.ValueChanged

Escribir en el CCCD indica al dispositivo servidor que este cliente quiere saber cada vez que cambie el valor de una característica determinada. Para ello: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
Ahora, se llamará al evento ValueChanged del objeto GattCharacteristic cada vez que cambie el valor en el dispositivo remoto. Lo único que queda por hacer es implementar el controlador: 

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



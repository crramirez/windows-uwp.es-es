---
title: Cliente de GATT de Bluetooth
description: En este artículo se proporciona información general sobre las aplicaciones cliente de Perfil de atributo genérico (GATT) de Bluetooth para Plataforma universal de Windows (UWP), junto con código de ejemplo para casos de uso comunes.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2d4ec2c3d849833b4a1673c4a4f425f32c42d00f
ms.sourcegitcommit: 662fcfdc08b050947e289a57520a2f99fad1a620
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91353765"
---
# <a name="bluetooth-gatt-client"></a>Cliente de GATT de Bluetooth

En este artículo se muestra el uso de las API de cliente de atributo genérico (GATT) de Bluetooth para aplicaciones Plataforma universal de Windows (UWP), junto con código de ejemplo para las tareas de cliente de GATT comunes:

- Consultar dispositivos cercanos
- Conectar con el dispositivo
- Enumerar los servicios y las características admitidos del dispositivo
- Leer y escribir en una característica
- Suscribirse a las notificaciones cuando cambia el valor de característica

> [!Important]
> Debe declarar la funcionalidad "Bluetooth" en *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **API importantes**
>
> - [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
> - [**Windows. Devices. Bluetooth. GenericAttributeProfile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Introducción

Los desarrolladores pueden usar las API del espacio de nombres [**Windows. Devices. Bluetooth. GenericAttributeProfile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) para tener acceso a dispositivos Bluetooth le. Los dispositivos Bluetooth LE exponen su funcionalidad a través de una colección de:

- Servicios
- Características
- Descriptores de

Los servicios definen el contrato funcional del dispositivo LE y contienen una colección de características que definen el servicio. Dichas características, a su vez, contienen descriptores que las describen. Estos 3 términos se conocen genéricamente como atributos de un dispositivo.

Las API de Bluetooth LE GATT exponen objetos y funciones, en lugar de tener acceso al transporte sin procesar. Las API de GATT también permiten a los desarrolladores trabajar con dispositivos Bluetooth LE con la capacidad de realizar las siguientes tareas:

- Realizar la detección de atributos
- Leer y escribir valores de atributo
- Registrar una devolución de llamada para el evento de característica ValueChanged

Para crear una implementación útil, un desarrollador debe tener un conocimiento previo de los servicios y características de GATT que la aplicación piensa consumir y procesar los valores característicos específicos, de modo que los datos binarios proporcionados por la API se transformen en datos útiles antes de que se presenten al usuario. Las API de Bluetooth GATT exponen solo los primitivos básicos requeridos para comunicarse con un dispositivo Bluetooth LE. Para interpretar los datos, debe definirse un perfil de aplicación, ya sea mediante un perfil estándar de un SIG de Bluetooth o mediante un perfil personalizado implementado por un proveedor de dispositivos. Un perfil crea un contrato vinculante entre la aplicación y el dispositivo, que indica qué representan los datos intercambiados y cómo interpretarlos.

Para mayor comodidad, el SIG de Bluetooth ofrece una [lista de perfiles públicos](https://www.bluetooth.com/specifications/adopted-specifications#gattspec).

## <a name="query-for-nearby-devices"></a>Consultar dispositivos cercanos

Hay dos métodos principales para consultar los dispositivos cercanos:

- DeviceWatcher en Windows. Devices. Enumeration
- AdvertisementWatcher en Windows. Devices. Bluetooth. Advertisement

El segundo método se describe a largo plazo en la documentación de los [anuncios](ble-beacon.md) , por lo que no se tratará en gran medida, pero la idea básica es encontrar la dirección de Bluetooth de los dispositivos cercanos que satisfacen el [filtro de anuncios](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)determinado. Una vez que tenga la dirección, puede llamar a [BluetoothLEDevice. FromBluetoothAddressAsync](/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) para obtener una referencia al dispositivo.

Ahora, vuelva al método DeviceWatcher. Un dispositivo Bluetooth LE es igual que cualquier otro dispositivo de Windows y se puede consultar mediante las [API de enumeración](/uwp/api/Windows.Devices.Enumeration). Use la clase [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) y pase una cadena de consulta que especifique los dispositivos que se van a buscar:

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

Una vez que haya iniciado la DeviceWatcher, recibirá [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para cada dispositivo que cumpla la consulta del controlador para el evento [agregado](/uwp/api/windows.devices.enumeration.devicewatcher.added) para los dispositivos en cuestión. Para obtener una visión más detallada de la DeviceWatcher, vea el ejemplo completo [en github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

## <a name="connecting-to-the-device"></a>Conexión al dispositivo

Una vez que se detecta un dispositivo deseado, use [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) para obtener el objeto de dispositivo Bluetooth le para el dispositivo en cuestión:

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

Por otro lado, la eliminación de todas las referencias a un objeto BluetoothLEDevice para un dispositivo (y si ninguna otra aplicación del sistema tiene una referencia al dispositivo) desencadenará una desconexión automática después de un período de tiempo de espera pequeño.

```csharp
bluetoothLeDevice.Dispose();
```

Si la aplicación necesita acceder de nuevo al dispositivo, basta con volver a crear el objeto de dispositivo y obtener acceso a una característica (descrita en la sección siguiente) para que el sistema operativo se vuelva a conectar cuando sea necesario. Si el dispositivo está cerca, obtendrá acceso al dispositivo; de lo contrario, devolverá un error DeviceUnreachable.  

> [!NOTE]
> La creación de un objeto [BluetoothLEDevice](/uwp/api/windows.devices.bluetooth.bluetoothledevice) llamando solo a este método no inicia (necesariamente) una conexión. Para iniciar una conexión, establezca [GattSession. MaintainConnection](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattsession.maintainconnection) en `true` , o llame a un método de detección de servicios no almacenado en caché en **BluetoothLEDevice**o realice una operación de lectura/escritura en el dispositivo.
>
> - Si **GattSession. MaintainConnection** está establecido en true, el sistema espera indefinidamente una conexión y se conecta cuando el dispositivo está disponible. No hay nada para que la aplicación espere, ya que **GattSession. MaintainConnection** es una propiedad.
> - En el caso de las operaciones de detección de servicios y de lectura y escritura en GATT, el sistema espera una hora finita y variable. Cualquier cosa, desde el instante hasta cuestión de minutos. Factores que informan sobre el tráfico en la pila y cómo se pone en cola la solicitud. Si no hay ninguna otra solicitud pendiente y el dispositivo remoto es inaccesible, el sistema esperará siete (7) segundos antes de que se agote el tiempo de espera. Si hay otras solicitudes pendientes, cada una de las solicitudes de la cola puede tardar siete (7) segundos en procesarse, por lo que la suya más larga es hacia la parte posterior de la cola, lo que más tiempo esperará.
>
> Actualmente, no se puede cancelar el proceso de conexión.

## <a name="enumerating-supported-services-and-characteristics"></a>Enumerar los servicios y las características admitidos

Ahora que tiene un objeto BluetoothLEDevice, el siguiente paso es detectar qué datos expone el dispositivo. El primer paso para hacerlo es consultar los servicios:

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

Una vez identificado el servicio de interés, el siguiente paso es consultar las características.

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

El sistema operativo devuelve una lista de solo lectura de objetos GattCharacteristic en los que puede realizar operaciones.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Realizar operaciones de lectura y escritura en una característica

La característica es la unidad fundamental de la comunicación basada en GATT. Contiene un valor que representa un fragmento de datos distinto en el dispositivo. Por ejemplo, la característica de nivel de batería tiene un valor que representa el nivel de batería del dispositivo.

Lea las propiedades de características para determinar qué operaciones se admiten:

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

La escritura en una característica sigue un patrón similar:

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **Sugerencia**: [DataReader y DataReader](/uwp/api/windows.storage.streams.datareader) son indispensables cuando se trabaja con los búferes sin procesar [que se obtienen](/uwp/api/windows.storage.streams.datawriter) de muchas de las API de Bluetooth.

## <a name="subscribing-for-notifications"></a>Suscripción a notificaciones

Asegúrese de que la característica admite indicar o notificar (Compruebe las propiedades de característica para asegurarse).

> **Además**: indica que se considera más confiable porque cada evento de valor cambiado se acopla con una confirmación del dispositivo cliente. La notificación es más frecuente, ya que la mayoría de las transacciones GATT en lugar de ahorrar energía en lugar de ser extremadamente confiable. En cualquier caso, todo esto se controla en el nivel de controlador para que la aplicación no se vea afectada. Nos referiremos colectivamente a ellos simplemente como "notificaciones", pero ahora sabe.

Hay dos cosas que debe tener en cuenta antes de recibir las notificaciones:

- Escribir en el descriptor de configuración de características de cliente (CCCD)
- Controlar el evento característico. ValueChanged

Al escribir en el CCCD se indica al dispositivo del servidor que este cliente desea conocer cada vez que cambia el valor específico de la característica. Para ello, siga estos pasos:

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

Ahora, se llamará al evento ValueChanged de GattCharacteristic cada vez que se cambie el valor en el dispositivo remoto. Lo único que queda es implementar el controlador:

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```
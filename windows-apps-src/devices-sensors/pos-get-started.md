---
title: Introducción al punto de servicio
description: Este artículo contiene información acerca de cómo empezar a trabajar con las API del punto de servicio Windows Runtime.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: f5f19d1337a7ae49f46ab65d8420fedb775eeb2f
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730387"
---
# <a name="getting-started-with-point-of-service"></a>Introducción al punto de servicio

Los dispositivos de punto de servicio, punto de venta o punto de servicio son periféricos informáticos que se usan para facilitar las transacciones comerciales. Algunos ejemplos de dispositivos de punto de servicio son los registros de caja electrónicos, los escáneres de códigos de barras, los lectores de bandas magnéticas y las impresoras de recepción.

Aquí aprenderá los conceptos básicos de la interacción con dispositivos de punto de servicio mediante el Windows Runtime punto de API de servicio. Trataremos la enumeración de dispositivos, comprobará las capacidades de los dispositivos, los dispositivos de la demanda y el uso compartido de dispositivos. Usamos un dispositivo de escáner de código de barras como ejemplo, pero casi todas las instrucciones se aplican a cualquier dispositivo de punto de servicio compatible con UWP. (Para obtener una lista de los dispositivos compatibles, consulte [compatibilidad con dispositivos de punto de servicio](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Búsqueda y conexión a periféricos de punto de servicio

Para que una aplicación pueda usar un dispositivo de punto de servicio, se debe emparejar con el equipo en el que se ejecuta la aplicación. Hay varias maneras de conectarse a los dispositivos de punto de servicio, ya sea mediante programación o a través de la aplicación de configuración.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Conexión a dispositivos mediante el uso de la aplicación de configuración
Cuando conecta un dispositivo de punto de servicio como un escáner de código de barras en un equipo, se muestra igual que cualquier otro dispositivo. Puede encontrarlo en la sección **dispositivos > Bluetooth & otros dispositivos** de la aplicación de configuración. Puede emparejar con un dispositivo de punto de servicio seleccionando **Agregar Bluetooth u otro dispositivo**.

Es posible que algunos dispositivos de punto de servicio no aparezcan en la aplicación de configuración hasta que se enumeren mediante programación con las API de punto de servicio.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Obtención de un único punto de servicio con GetDefaultAsync
En un caso de uso sencillo, puede tener un solo punto de periférico de servicio conectado al equipo en el que se ejecuta la aplicación y quiere configurarlo lo antes posible. Para ello, recupere el dispositivo "predeterminado" con el método **GetDefaultAsync** , como se muestra aquí.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Si se encuentra el dispositivo predeterminado, el objeto de dispositivo recuperado está listo para ser reclamado. "Reclamar" un dispositivo proporciona a una aplicación acceso exclusivo a él, evitando comandos en conflicto de varios procesos.

> [!NOTE] 
> Si hay más de un punto de servicio conectado al equipo, **GetDefaultAsync** devuelve el primer dispositivo que encuentra. Por esta razón, use **FindAllAsync** a menos que esté seguro de que solo un punto de servicio es visible para la aplicación.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Enumerar una colección de dispositivos con FindAllAsync

Cuando se conecta a más de un dispositivo, debe enumerar la colección de objetos de dispositivo **PointOfService** para encontrar el que desea notificar. Por ejemplo, el código siguiente crea una colección de todos los escáneres de códigos de barras conectados actualmente y, a continuación, busca en la colección un escáner con un nombre específico.

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>Ámbito de la selección del dispositivo
Al conectarse a un dispositivo, puede que desee limitar la búsqueda a un subconjunto de periféricos de punto de servicio al que tiene acceso la aplicación. Con el método **GetDeviceSelector** , puede seleccionar el ámbito de la selección para recuperar dispositivos conectados solo por un método determinado (Bluetooth, USB, etc.). Puede crear un selector que busque dispositivos a través de **Bluetooth**, **IP**, **local**o **todos los tipos de conexión**. Esto puede ser útil, ya que la detección de dispositivos inalámbricos tarda mucho tiempo en comparación con la detección local (cableada). Puede garantizar un tiempo de espera determinista para la conexión del dispositivo local mediante la limitación de **FindAllAsync** a tipos de conexión **locales** . Por ejemplo, este código recupera todos los escáneres de códigos de barras accesibles a través de una conexión local. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Reaccionar a los cambios de conexión de dispositivo con DeviceWatcher

A medida que se ejecuta la aplicación, en ocasiones los dispositivos se desconectarán o se actualizarán, o se deberán agregar nuevos dispositivos. Puede usar la clase **DeviceWatcher** para tener acceso a eventos relacionados con el dispositivo, de modo que la aplicación pueda responder en consecuencia. Este es un ejemplo de cómo usar **DeviceWatcher**, con códigos auxiliares de método que se llamarán si se agrega, quita o actualiza un dispositivo.

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Comprobando las capacidades de un dispositivo de punto de servicio
Incluso dentro de una clase de dispositivo, como los escáneres de código de barras, los atributos de cada dispositivo pueden variar considerablemente entre modelos. Si su aplicación requiere un atributo de dispositivo específico, puede que tenga que inspeccionar cada objeto de dispositivo conectado para determinar si se admite el atributo. Por ejemplo, quizás su empresa requiere que las etiquetas se creen con un patrón de impresión de código de barras específico. Aquí se muestra cómo puede comprobar si un escáner de códigos de barras conectado admite una simbología. 

> [!NOTE]
> Una simbología es la asignación de lenguaje que usa un código de barras para codificar los mensajes.

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>Usar la clase Device. Capabilities
La clase **Device. Capabilities** es un atributo de todas las clases de dispositivo de punto de servicio y se puede usar para obtener información general sobre cada dispositivo. Por ejemplo, en este ejemplo se determina si un dispositivo admite informes de estadísticas y, si lo hace, recupera estadísticas de los tipos admitidos.

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>Reclamar un dispositivo de punto de servicio
Antes de poder usar un dispositivo de punto de servicio para la entrada o salida activa, debe reclamarlo, lo que concede a la aplicación acceso exclusivo a muchas de sus funciones. Este código muestra cómo reclamar un dispositivo de escáner de código de barras, una vez que haya encontrado el dispositivo con uno de los métodos descritos anteriormente.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>Conservar el dispositivo
Cuando se usa un dispositivo de punto de servicio a través de una conexión de red o Bluetooth, es posible que desee compartir el dispositivo con otras aplicaciones de la red. (Para obtener más información sobre esto, consulte [uso compartido de dispositivos](#sharing-a-device-between-apps)). En otros casos, puede que desee mantener en el dispositivo un uso prolongado. En este ejemplo se muestra cómo conservar un escáner de código de barras reclamado después de que otra aplicación haya solicitado que el dispositivo se libere.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Entrada y salida

Una vez que haya reclamado un dispositivo, está casi listo para usarlo. Para recibir la entrada del dispositivo, debe configurar y permitir que un delegado reciba datos. En el ejemplo siguiente, se declara un dispositivo de escáner de código de barras, se establece su propiedad Decode y, a continuación, se llama a **EnableAsync** para habilitar la entrada descodificada desde el dispositivo. Este proceso varía entre las clases de dispositivos, por lo que para obtener instrucciones sobre cómo configurar un delegado para dispositivos sin códigos de barras, consulte el [ejemplo de aplicación de UWP](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors)pertinente.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>Compartir un dispositivo entre aplicaciones

Los dispositivos de punto de servicio se suelen usar en casos en los que más de una aplicación necesitará tener acceso a ellos en un breve período.  Un dispositivo puede compartirse cuando se conecta a varias aplicaciones de forma local (USB u otra conexión cableada), o a través de una red Bluetooth o IP. En función de las necesidades de cada aplicación, es posible que un proceso tenga que desechar su demanda en el dispositivo. Este código desecha el dispositivo de escáner de código de barras reclamado, lo que permite a otras aplicaciones reclamarlo y usarlo.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> Tanto la clase de dispositivo de servicio como el punto de servicio reclamado implementan la [interfaz IClosable](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Si un dispositivo está conectado a una aplicación a través de una red o Bluetooth, los objetos solicitados y no reclamados se deben eliminar antes de que otra aplicación pueda conectarse.

## <a name="see-also"></a>Vea también
+ [Ejemplo de escáner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de cajón de caja]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de presentación de líneas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de franjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)


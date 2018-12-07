---
title: Tareas iniciales con punto de servicio
description: En este artículo se incluye información sobre la introducción a las API de UWP del punto de servicio.
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0e537e40d5224f2522cb5ecebd92664d1794dd06
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8798264"
---
# <a name="getting-started-with-point-of-service"></a>Tareas iniciales con punto de servicio

Los dispositivos de punto de servicio o punto de venta son periféricos del equipo usados para facilitar las transacciones comerciales. Entre los dispositivos de punto de servicio se incluyen cajas registradoras electrónicas, escáneres de códigos de barras, lectores de bandas magnéticas e impresoras de recibos.

Aquí aprenderás los conceptos básicos a la hora de interactuar con dispositivos de punto de servicio mediante el uso de las API de punto de servicio de la Plataforma universal de Windows (UWP). Hablaremos sobre la enumeración de dispositivos, la comprobación de las funcionalidades del dispositivo así como de la reclamación de dispositivos y el uso compartido. Usamos como ejemplo un dispositivo de escáner de código de barras, pero casi todas estas directrices se aplican a cualquier dispositivo de punto de servicio compatible con UWP. (Para obtener una lista de dispositivos compatibles, consulta [Compatibilidad con dispositivos de punto de servicio](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Búsqueda y conexión de periféricos de punto de servicio

Antes de que una aplicación pueda usar un dispositivo de punto de servicio, debe estar emparejado con el equipo en el que se ejecuta la aplicación. Hay varias formas de conectarse a dispositivos de punto de servicio, ya sea a través de la programación o a través de la aplicación Configuración.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Conectarse a dispositivos a través de la aplicación Configuración
Al conectar un dispositivo de punto de servicio, como un escáner de código de barras, en un PC, aparecerá como cualquier otro dispositivo. Puedes encontrarlo en la sección **Dispositivos > Bluetooth y otros dispositivos** de la aplicación Configuración. Ahí podrás emparejarlo con un dispositivo de punto de servicio seleccionando **Agregar Bluetooth u otro dispositivo**.

Algunos dispositivos de punto de servicio pueden no aparecer en la aplicación Configuración hasta que se enumeren a través de la programación usando las API de un punto de servicio.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Obtener un solo dispositivo de punto de servicio con GetDefaultAsync
En un caso de uso sencillo, es posible que solo tengas un periférico de punto de servicio conectado al PC en el que se ejecuta la aplicación, y que quieras configurarlo lo más rápido posible. Para ello, recupera el dispositivo "predeterminado" con el método **GetDefaultAsync** como se muestra aquí.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Si se encuentra el dispositivo predeterminado, el objeto de dispositivo recuperado está listo para reclamarse. "Reclamar" un dispositivo le da a la aplicación acceso exclusivo, evitando los comandos conflictivos de múltiples procesos.

> [!NOTE] 
> Si hay más de un dispositivo de punto de servicio conectado al PC, **GetDefaultAsync** devolverá el primer dispositivo que encuentre. Por este motivo, usa **FindAllAsync** a menos que estés seguro de que solo hay un dispositivo de punto de servicio visible para la aplicación.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Enumerar una colección de dispositivos con FindAllAsync

Cuando estés conectado a más de un dispositivo, debes enumerar la colección de objetos de dispositivos de **PointOfService** para encontrar el que quieras reclamar. Por ejemplo, el siguiente código crea una colección de todos los escáneres de códigos de barras conectados en este momento y, a continuación, busca en la colección un escáner con un nombre específico.

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
Cuando te conectas a un dispositivo, puedes limitar la búsqueda a un subconjunto de periféricos de punto de servicio a las que tu aplicación tenga acceso. Con el método **GetDeviceSelector**, puedes definir el ámbito de la selección para recuperar los dispositivos conectados únicamente por un método determinado (Bluetooth, USB, etc.). Puedes crear un selector que busque dispositivos en **Bluetooth**, **IP**, **Local** o **Todos los tipos de conexión**. Esto puede ser útil, ya que la detección de dispositivos inalámbricos tarda mucho tiempo en comparación con la detección local (con cable). Puedes garantizar un tiempo de espera determinista para la conexión del dispositivo local limitando **FindAllAsync** a **Local** para los tipos de conexión. Por ejemplo, este código recupera todos los escáneres de códigos de barras accesibles a través de una conexión local. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Reaccionar ante los cambios de la conexión de dispositivo con DeviceWatcher

Mientras se ejecuta la aplicación, a veces, los dispositivos se desconectan o se ejecutan o habrá nuevos dispositivos que tengan que agregarse. Puedes usar la clase **DeviceWatcher** para acceder a los eventos relacionados con el dispositivo, para que la aplicación pueda responder según corresponda. Este es un ejemplo de cómo usar **DeviceWatcher**, con códigos auxiliares del método para que se llame si un dispositivo se agrega, se elimina o se actualiza.

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

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Comprobar las funcionalidades de un dispositivo de punto de servicio
Incluso dentro de una clase de dispositivo, como escáneres de códigos de barras, los atributos de cada dispositivo pueden variar considerablemente entre los modelos. Si la aplicación requiere un atributo de dispositivo específico, deberás inspeccionar cada objeto de dispositivo conectado para determinar si el atributo es compatible. Por ejemplo, quizás tu empresa necesita que las etiquetas se creen usando un patrón de impresión específica de códigos de barras. Aquí te mostramos cómo se puede comprobar para ver si un escáner de códigos de barras conectado admite un simbología. 

> [!NOTE]
> Una simbología es la asignación de idioma que utiliza un código de barras para codificar mensajes.

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

### <a name="using-the-devicecapabilities-class"></a>Uso de la clase Device.Capabilities
La clase **Device.Capabilities** es un atributo de todas las clases de dispositivos de punto de servicio y puede usarse para obtener información general sobre cada dispositivo. Por ejemplo, en este ejemplo se determina si un dispositivo es compatible con los informes de estadísticas y, si es así, recuperar las estadísticas de todos los tipos compatibles.

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
Antes de poder usar un dispositivo de punto de servicio de entrada o salida activa, debes reclamarlo, concediendo así a la aplicación acceso exclusivo a muchas de sus funciones. Este código muestra cómo reclamar un dispositivo de escáner de códigos de barras, una vez que hayas encontrado el dispositivo mediante uno de los métodos descritos anteriormente.

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
Al usar un dispositivo de punto de servicio mediante una red o la conexión Bluetooth, puedes compartir el dispositivo con otras aplicaciones de la red. (Para obtener más información, consulta [Compartir dispositivos](#sharing-a-device-between-apps)). En otros casos, puedes mantener este dispositivo para un uso prolongado. Este ejemplo muestra cómo conservar un escáner de códigos de barras reclamado, después de que otra aplicación haya solicitado que se libere el dispositivo.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Entrada y salida

Después de solicitar un dispositivo, ya está casi listo para usarlo. Para recibir una entrada desde el dispositivo, debes configurar y habilitar un delegado para recibir datos. En el ejemplo siguiente, se reclama un escáner de códigos de barras, se establece su propiedad de descodificación y, a continuación, se llama a **EnableAsync** para habilitar la entrada descodificada desde el dispositivo. Este proceso varía entre las clases de dispositivo, por lo tanto, para obtener instrucciones sobre cómo configurar un delegado para dispositivos que no son de códigos de barras, consulta la [muestra de aplicación para UWP](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors) relevante.

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

Los dispositivos de punto de servicio se suelen usar en casos donde se necesita más de una aplicación para acceder a ellos en un período breve.  Un dispositivo puede compartirse cuando se conecta a múltiples aplicaciones locales (USB u otra conexión con cables) o a través de Bluetooth o de una red IP. Según las necesidades de cada aplicación, un proceso puede eliminar su reclamación en el dispositivo. Este código elimina nuestro dispositivo de escáner de códigos de barras reclamado y permite a otras aplicaciones reclamarlo y que lo usen.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> Tanto las clases de dispositivos de punto de servicio reclamados como los que no implementan la [interfaz IClosable](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Si un dispositivo está conectado a una aplicación a través de Bluetooth o de la red, tantos los objetos reclamados como los que no deben eliminarse antes de que otra aplicación pueda conectarse.

## <a name="see-also"></a>Consulta también
+ [Ejemplo de escáner de códigos de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de caja registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de visualización de líneas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de bandas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de impresora de punto de servicio](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)


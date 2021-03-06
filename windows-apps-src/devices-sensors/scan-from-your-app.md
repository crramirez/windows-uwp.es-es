---
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: Digitalizar desde tu aplicación
description: Aprende aquí a digitalizar contenido de tu aplicación con ayuda de un escáner plano, un alimentador o un origen de digitalización configurado automáticamente.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c306c225d200fe0636b3195699afe0441bc252bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175439"
---
# <a name="scan-from-your-app"></a>Digitalizar desde tu aplicación


**API importantes**

-   [**Windows.Devices.Scanners**](/uwp/api/Windows.Devices.Scanners)
-   [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)
-   [**DeviceClass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass)

Aprende aquí a digitalizar contenido de tu aplicación con ayuda de un escáner plano, un alimentador o un origen de digitalización configurado automáticamente.

**Importante**    Las API de [**Windows. Devices. Scanner**](/uwp/api/Windows.Devices.Scanners) forman parte de la [familia de dispositivos](../get-started/universal-application-platform-guide.md)de escritorio. Las aplicaciones solo pueden usar estas API en la versión de escritorio de Windows 10.

Para digitalizar contenido desde tu aplicación, antes deberás enumerar los escáneres disponibles. Para ello, declara un nuevo objeto [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para obtener el tipo [**DeviceClass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass). Solo se mostrarán aquellos escáneres que estén instalados localmente con controladores de WIA y que estén disponibles para tu aplicación.

Tras enumerar los escáneres disponibles, la aplicación puede usar los parámetros de digitalización configurados automáticamente según el tipo de escáner o, simplemente, digitalizar con el escáner plano o el dispositivo multifunción. Para usar los parámetros configurados automáticamente, el escáner debe estar habilitado para dicha opción y no estar dotado de un escáner plano o alimentador. Para más información, consulta [Digitalización configurada automáticamente](/windows-hardware/drivers/image/auto-configured-scanning).

## <a name="enumerate-available-scanners"></a>Enumerar los escáneres disponibles

Windows no detecta los escáneres automáticamente. Debes llevar a cabo este paso para que tu aplicación se comunique con el escáner. En este ejemplo, la enumeración de dispositivos de digitalización se realiza usando el espacio de nombres [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration).

1.  Primero, agrégalos a tu archivo de definición de clase usando instrucciones.

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  A continuación, implementa un monitor de dispositivos para empezar a enumerar los escáneres. Para más información, consulta [Enumerar dispositivos](enumerate-devices.md).

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  Crea un controlador de eventos para cuando se agregue un escáner.

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =>
             {
                MainPage.Current.NotifyUser(String.Format("Scanner with device id {0} has been added", deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## <a name="scan"></a>Examinar

1.  **Obtener un objeto ImageScanner**

Para cada tipo de enumeración [**ImageScannerScanSource**](/uwp/api/Windows.Devices.Scanners.ImageScannerScanSource), independientemente de si es **Default**, **AutoConfigured**, **Flatbed** o **Feeder**, primero debes crear un objeto [**ImageScanner**](/uwp/api/Windows.Devices.Scanners.ImageScanner) llamando al método [**ImageScanner.FromIdAsync**](/uwp/api/windows.devices.scanners.imagescanner.fromidasync) como se muestra aquí.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **Solo digitalización**

Para digitalizar con la configuración predeterminada, tu aplicación se basa en el nombre de espacios [**Windows.Devices.Scanners**](/uwp/api/Windows.Devices.Scanners) para seleccionar un escáner, y digitaliza desde ese origen. No se cambia la configuración de digitalización. Los escáneres posibles son autoconfigurado, plano o alimentador. Este tipo de digitalización probablemente tendrá como resultado una operación de digitalización correcta, aunque digitalice desde un origen incorrecto, como un escáner plano en vez de alimentador.

**Nota:**    Si el usuario coloca el documento para digitalizar en el alimentador, el escáner digitalizará desde el escáner en su lugar. Si el usuario intenta digitalizar desde un alimentador vacío, el proceso no generará ningún archivo digitalizado.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **Digitalizar desde un origen autoconfigurado, plano o alimentador**

Tu aplicación puede usar la [Digitalización autoconfigurada](/windows-hardware/drivers/image/auto-configured-scanning) del dispositivo, de modo que digitalice usando la configuración óptima. Con esta opción, el propio dispositivo puede determinar la configuración de digitalización, como el modo de color y la resolución, en función del contenido que se vaya a digitalizar. El dispositivo selecciona la configuración de digitalización en tiempo de ejecución para cada trabajo de digitalización nuevo.

**Nota:**    No todos los escáneres admiten esta característica, por lo que la aplicación debe comprobar si el escáner es compatible con esta característica antes de usar esta opción.

En este ejemplo, la aplicación comprueba primero si el escáner es capaz de autoconfigurarse y, a continuación, procede a la digitalización. Para especificar un escáner plano o alimentador, basta con que sustituyas **AutoConfigured** por **Flatbed** o **Feeder**.

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings.
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## <a name="preview-the-scan"></a>Previsualizar la digitalización

Puedes agregar código para previsualizar la digitalización antes de iniciar el proceso de digitalización en una carpeta. En el siguiente ejemplo, la aplicación comprueba si el escáner **Flatbed** es compatible con la previsualización y luego previsualiza la digitalización.

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>Cancelar la digitalización

Puedes permitirles a los usuarios cancelar la digitalización mientras se está llevando a cabo el proceso, como aquí.

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## <a name="scan-with-progress"></a>Digitalización con progreso

1.  Crea un objeto **System.Threading.CancellationTokenSource**.

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  Configura el controlador de eventos y obtén el progreso de la digitalización.

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>Digitalización en la biblioteca de imágenes

Los usuarios pueden digitalizar en cualquier carpeta de forma dinámica usando la clase [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker), pero debes declarar la funcionalidad *Biblioteca de imágenes* en el manifiesto para permitirles digitalizar en la carpeta. Para obtener más información sobre las funcionalidades de la aplicación, consulta [Declaraciones de funcionalidades de las aplicaciones](../packaging/app-capability-declarations.md).
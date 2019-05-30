---
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: Digitalizar desde tu aplicación
description: Aprende aquí a digitalizar contenido de tu aplicación con ayuda de un escáner plano, un alimentador o un origen de digitalización configurado automáticamente.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a314d0acdc3df1e0b53b1d78445b6ab1b71bf92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369745"
---
# <a name="scan-from-your-app"></a>Digitalizar desde tu aplicación


**API importantes**

-   [**Windows.Devices.Scanners**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners)
-   [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)
-   [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass)

Aprende aquí a digitalizar contenido de tu aplicación con ayuda de un escáner plano, un alimentador o un origen de digitalización configurado automáticamente.

**Importante**  el [ **Windows.Devices.Scanners** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners) API forman parte del escritorio [familia de dispositivos](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). Las aplicaciones pueden usar estas API solo en la versión de escritorio de Windows 10.

Para digitalizar contenido desde tu aplicación, antes deberás enumerar los escáneres disponibles. Para ello, declara un nuevo objeto [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para obtener el tipo [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass). Solo se mostrarán aquellos escáneres que estén instalados localmente con controladores de WIA y que estén disponibles para tu aplicación.

Tras enumerar los escáneres disponibles, la aplicación puede usar los parámetros de digitalización configurados automáticamente según el tipo de escáner o, simplemente, digitalizar con el escáner plano o el dispositivo multifunción. Para usar los parámetros configurados automáticamente, el escáner debe estar habilitado para dicha opción y no estar dotado de un escáner plano o alimentador. Para más información, consulta [Digitalización configurada automáticamente](https://docs.microsoft.com/windows-hardware/drivers/image/auto-configured-scanning).

## <a name="enumerate-available-scanners"></a>Enumerar los escáneres disponibles

Windows no detecta los escáneres automáticamente. Debes llevar a cabo este paso para que tu aplicación se comunique con el escáner. En este ejemplo, la enumeración de dispositivos de digitalización se realiza usando el espacio de nombres [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration).

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

## <a name="scan"></a>Digitalización

1.  **Obtener un objeto ImageScanner**

Para cada tipo de enumeración [**ImageScannerScanSource**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners.ImageScannerScanSource), independientemente de si es **Default**, **AutoConfigured**, **Flatbed** o **Feeder**, primero debes crear un objeto [**ImageScanner**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners.ImageScanner) llamando al método [**ImageScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.scanners.imagescanner.fromidasync) como se muestra aquí.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **Solo digitalizarán**

Para digitalizar con la configuración predeterminada, tu aplicación se basa en el nombre de espacios [**Windows.Devices.Scanners**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners) para seleccionar un escáner, y digitaliza desde ese origen. No se cambia la configuración de digitalización. Los escáneres posibles son autoconfigurado, plano o alimentador. Este tipo de digitalización probablemente tendrá como resultado una operación de digitalización correcta, aunque digitalice desde un origen incorrecto, como un escáner plano en vez de alimentador.

**Tenga en cuenta**  si el usuario coloca el documento que se va a examinar en el alimentador, el analizador se examinará desde el plano. Si el usuario intenta digitalizar desde un alimentador vacío, el proceso no generará ningún archivo digitalizado.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **Examen de configurado automáticamente, escáner plano o alimentador de origen**

Tu aplicación puede usar la [Digitalización autoconfigurada](https://docs.microsoft.com/windows-hardware/drivers/image/auto-configured-scanning) del dispositivo, de modo que digitalice usando la configuración óptima. Con esta opción, el propio dispositivo puede determinar la configuración de digitalización, como el modo de color y la resolución, en función del contenido que se vaya a digitalizar. El dispositivo selecciona la configuración de digitalización en tiempo de ejecución para cada trabajo de digitalización nuevo.

**Tenga en cuenta**  no todos los escáneres admiten esta característica, por lo que la aplicación debe comprobar si el escáner admite esta característica antes de usar esta configuración.

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

Los usuarios pueden digitalizar en cualquier carpeta de forma dinámica usando la clase [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker), pero debes declarar la funcionalidad *Biblioteca de imágenes* en el manifiesto para permitirles digitalizar en la carpeta. Para obtener más información sobre las funcionalidades de la aplicación, consulta [Declaraciones de funcionalidades de las aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

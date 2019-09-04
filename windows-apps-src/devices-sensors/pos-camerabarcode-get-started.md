---
title: Tareas iniciales con un escáner de código de barras basado en cámara
description: Aprender a utilizar el escáner de código de barras basado en cámara
ms.date: 09/02/2019
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243290"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Tareas iniciales con un escáner de código de barras basado en cámara

Los fragmentos de código usados aquí son solo para fines de demostración. Para obtener un ejemplo práctico, vea el [ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Paso 1: Agregar declaraciones de funcionalidad al manifiesto de la aplicación

1. En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2. Selecciona la pestaña **Funcionalidades**
3. Marca las casillas de **Cámara web** y **PointOfService**

>[!NOTE]
> La funcionalidad **Cámara Web** es necesaria para que el descodificador de software reciba los fotogramas a descodificar, así como para proporcionar una vista previa de la aplicación

## <a name="step-2-add-using-directives"></a>Paso 2: Agregar directivas Using

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Paso 3: Definición del selector de dispositivos

### <a name="option-a-find-all-barcode-scanners"></a>**Opción A: Buscar todos los escáneres de código de barras**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Opción B: Ámbito del selector de dispositivos a tipo de conexión**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Paso 4: Enumerar todos los escáneres de códigos de barras

Si no espera que la lista de dispositivos cambie a lo largo de la vida útil de la aplicación, puede enumerar una instantánea una sola vez con [DeviceInformation. FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), pero si cree que la lista de escáneres de códigos de barras podría cambiar durante el tiempo de vida de su en su lugar, debe usar una aplicación [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) .  

> [!Important]
> El uso de GetDefaultAsync para enumerar dispositivos PointOfService puede provocar un comportamiento incoherente, ya que solo devuelve el primer dispositivo que se encuentra en la clase y esto cambia de una sesión a otra.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Opción A: Enumerar una instantánea de escáneres de código de barras**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Consulta [*Enumerar una instantánea de dispositivos*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) para obtener más información sobre el uso de *FindAllAsync*.

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Opción B: Enumerar los escáneres de códigos de barras disponibles y ver los cambios en los escáneres disponibles**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Consulta [*Enumerar y observar cambios en dispositivo*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) y [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) para obtener más información.

## <a name="step-5-identify-camera-barcode-scanners"></a>Paso 5: Identificación de escáneres de códigos de barras de cámara

Un escáner de código de barras basado en cámara se crea dinámicamente cuando Windows empareja las cámaras conectadas a tu equipo con un descodificador de software.  Cada conjunto descodificador-equipo es un escáner de código de barras totalmente funcional.

Para cada escáner de códigos de barras de la recopilación de dispositivos resultante, puede diferenciar entre escáneres de código de barras de cámara y escáneres de códigos de barras físicos mediante la comprobación de la propiedad [*BarcodeScanner. VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) .  Un valor de VideoDeviceID que no sea NULL indica que el objeto de escáner de código de barras de la colección de dispositivos es un escáner de código de barras de cámara.  Si tiene más de un escáner de códigos de barras de la cámara, es posible que desee crear una colección independiente que excluya los escáneres de códigos de barras físicos.

Los escáneres de códigos de barras de la cámara que usan el descodificador que se distribuye con Windows se identifican como:

> Microsoft BarcodeScanner (*nombre de tu cámara*)

Si tiene más de una cámara y están integradas en el chasis del equipo, el nombre puede diferenciar entre las cámaras *frontal* y *trasera* .

> [!NOTE]
> En el futuro, podrían liberarse descodificadores de software adicionales con diferentes esquemas de nomenclatura.

Cuando se inicia la DeviceWatcher (paso 4), se enumera a través de cada dispositivo conectado. Aquí se agregan los escáneres disponibles a una colección de escáner de código de barras y se enlaza la colección a un cuadro de lista.

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

Cuando el SelectedIndex del cuadro de lista cambia (el primer elemento está seleccionado de forma predeterminada en el fragmento de código anterior), se consulta la información del dispositivo.

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Paso 6: Reclamar el escáner de códigos de barras de la cámara

Usa [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) para obtener el uso exclusivo del escáner de código de barras basado en cámara.

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>Paso 7: Versión preliminar proporcionada por el sistema

Una vista previa de cámara es necesaria para que el usuario apunte correctamente a los códigos de barras con la cámara.  Windows proporciona una sencilla vista previa de cámara que inicia un cuadro de diálogo para el control básico del escáner de códigos de barras de la cámara.  Basta con llamar a [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) para abrir el cuadro de diálogo y a [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) para cerrarlo al finalizar.

> [!TIP]
> Consulta [Vista previa de hospedaje](pos-camerabarcode-hosting-preview.md) para hospedar en tu aplicación la vista previa del escáner de código de barras basado en cámara.

## <a name="step-8-initiate-scan"></a>Paso 8: Iniciar examen

Puedes iniciar el proceso de digitalización llamando a [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Dependiendo del valor de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , es posible que el escáner examine solo un código de barras y, después, detenga o digitalice continuamente hasta que llame a [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Establece el valor deseado de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar el comportamiento del escáner cuando se descodifica un código de barras.

| Valor | Descripción |
| ----- | ----------- |
| True   | Escanear solo un código de barras y detener |
| False  | Escanear códigos de barra continuamente sin detenerse |

## <a name="see-also"></a>Vea también

### <a name="samples"></a>Muestras

- [Ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)

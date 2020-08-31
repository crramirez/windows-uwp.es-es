---
title: Introducción con escáner de códigos de barras de la cámara
description: Use estas instrucciones paso a paso y fragmentos de código para empezar a usar un escáner de códigos de barras de la cámara.
ms.date: 09/02/2019
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: f7206250b2495ae2c905d2aece2b8cd1b85d6859
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168479"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Introducción a un escáner de código de barras de cámara

Los fragmentos de código usados aquí son solo para fines de demostración. Para obtener un ejemplo práctico, vea el [ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Paso 1: agregar declaraciones de funcionalidad al manifiesto de la aplicación

1. En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2. Seleccione la pestaña **capacidades** .
3. Active las casillas de **cámara web** y **PointOfService**

>[!NOTE]
> La funcionalidad de **cámara web** es necesaria para que el descodificador de software reciba fotogramas de la cámara para descodificarlos, así como para proporcionar una vista previa de la aplicación.

## <a name="step-2-add-using-directives"></a>Paso 2: agregar directivas Using

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Paso 3: definición del selector de dispositivos

### <a name="option-a-find-all-barcode-scanners"></a>**Opción A: Buscar todos los escáneres de códigos de barras**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Opción B: ámbito del selector de dispositivos para el tipo de conexión**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Paso 4: enumerar todos los escáneres de códigos de barras

Si no espera que la lista de dispositivos cambie a lo largo de la vida útil de la aplicación, puede enumerar una instantánea una sola vez con [DeviceInformation. FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), pero si cree que la lista de escáneres de códigos de barras podría cambiar durante el tiempo de vida de la aplicación, debe usar una [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) en su lugar.  

> [!Important]
> El uso de GetDefaultAsync para enumerar dispositivos PointOfService puede producir un comportamiento incoherente, ya que simplemente devuelve el primer dispositivo que se encuentra en la clase y esto puede cambiar de una sesión a una sesión.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Opción A: enumerar una instantánea de los escáneres de código de barras**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Vea [*enumerar una instantánea de los dispositivos*](./enumerate-devices.md#enumerate-a-snapshot-of-devices) para obtener más información sobre el uso de *FindAllAsync*.

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Opción B: enumerar los escáneres de códigos de barras disponibles y ver los cambios en los escáneres disponibles**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Consulte [*enumeración y inspección de los cambios de los dispositivos*](./enumerate-devices.md#enumerate-and-watch-devices) y la [*DeviceWatcher*](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) para obtener más información.

## <a name="step-5-identify-camera-barcode-scanners"></a>Paso 5: identificación de los escáneres de códigos de barras de la cámara

Un escáner de código de barras de la cámara se crea dinámicamente a medida que Windows empareja las cámaras conectadas al equipo con un descodificador de software.  Cada par de descodificador de cámara es un escáner de código de barras totalmente funcional.

Para cada escáner de códigos de barras de la recopilación de dispositivos resultante, puede diferenciar entre escáneres de código de barras de cámara y escáneres de códigos de barras físicos mediante la comprobación de la propiedad [*BarcodeScanner. VideoDeviceID*](/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) .  Un valor de VideoDeviceID que no sea NULL indica que el objeto de escáner de código de barras de la colección de dispositivos es un escáner de código de barras de cámara.  Si tiene más de un escáner de códigos de barras de la cámara, es posible que desee crear una colección independiente que excluya los escáneres de códigos de barras físicos.

Los escáneres de códigos de barras de la cámara que usan el descodificador que se distribuye con Windows se identifican como:

> Microsoft BarcodeScanner (*nombre de la cámara aquí*)

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

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Paso 6: reclamar el escáner de códigos de barras de la cámara

Use [BarcodeScanner. ClaimScannerAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) para obtener el uso exclusivo del escáner de códigos de barras de la cámara.

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

## <a name="step-7-system-provided-preview"></a>Paso 7: versión preliminar proporcionada por el sistema

Se necesita una vista previa de la cámara para que el usuario pueda dirigirse correctamente a la cámara en códigos de barras.  Windows proporciona una sencilla vista previa de cámara que inicia un cuadro de diálogo para el control básico del escáner de códigos de barras de la cámara.  Simplemente llame a [ClaimedBarcodeScanner. ShowVideoPreview](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) para abrir el cuadro de diálogo y [ClaimedBarcodeScanner. HideVideoPreview](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) para cerrarlo cuando termine.

> [!TIP]
> Consulte [vista previa de hospedaje](pos-camerabarcode-hosting-preview.md) para hospedar la vista previa del escáner de códigos de barras de la cámara en la aplicación.

## <a name="step-8-initiate-scan"></a>Paso 8: iniciar el examen

Puede iniciar el proceso de digitalización mediante una llamada a [**StartSoftwareTriggerAsync**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Dependiendo del valor de [**IsDisabledOnDataReceived**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , es posible que el escáner examine solo un código de barras y, después, detenga o digitalice continuamente hasta que llame a [**StopSoftwareTriggerAsync**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Establezca el valor deseado de [**IsDisabledOnDataReceived**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar el comportamiento del analizador cuando se descodifica un código de barras.

| Value | Descripción |
| ----- | ----------- |
| True   | Examinar solo un código de barras y detener |
| Falso  | Examinar los códigos de barras continuamente sin detenerse |

## <a name="see-also"></a>Vea también

### <a name="samples"></a>Ejemplos

- [Ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
---
title: Usar un desencadenador de software
description: Aprende a controlar la digitalización desde software de.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 2b6f06ea66767a1bcdd7e20fa05aa7af275eb892
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351966"
---
# <a name="use-a-software-trigger"></a>Usar un desencadenador de software

Puede ser útil para controlar la digitalización desde el software si estás usando un escáner de código de barras en modo de presentación, o si el escáner no tiene un desencadenante físico, como los escáneres de código de barras basados en cámaras. Puedes iniciar el proceso de digitalización llamando a [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Según el valor de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived), el escáner puede digitalizar solo un código de barras y parar, o digitalizar continuamente hasta llamar a [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Establece el valor deseado de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar el comportamiento del escáner cuando se descodifica un código de barras.

| Valor | Descripción |
| ----- | ----------- |
| Verdadero   | Escanear solo un código de barras y detener |
| Falso  | Escanear códigos de barra continuamente sin detenerse |


> [!Important]
> Confirma que el escáner de código de barras es compatible con el uso de desencadenante de software, primero comprobando la propiedad [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

El siguiente ejemplo muestra cómo iniciar el examen con un desencadenador de software, que se detendrá el examen una vez que digitalice un código de barras:

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]
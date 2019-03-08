---
title: Usar un desencadenador de software
description: Obtenga información sobre cómo controlar el acto de análisis de software.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 2b6f06ea66767a1bcdd7e20fa05aa7af275eb892
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645530"
---
# <a name="use-a-software-trigger"></a>Usar un desencadenador de software

Puede ser útil para controlar la digitalización desde el software si estás usando un escáner de código de barras en modo de presentación, o si el escáner no tiene un desencadenante físico, como los escáneres de código de barras basados en cámaras. Puede iniciar el proceso de exploración mediante una llamada a [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Dependiendo del valor de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) el analizador puede analizar sólo un código de barras, a continuación, detener o analizar continuamente hasta que llame a [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Establezca el valor deseado de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar el comportamiento del analizador cuando se descodifica un código de barras.

| Valor | Descripción |
| ----- | ----------- |
| True   | Escanear solo un código de barras y detener |
| False  | Escanear códigos de barra continuamente sin detenerse |


> [!Important]
> Confirme que el escáner admite el uso del desencadenador de software mediante la comprobación de la propiedad [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

El ejemplo siguiente muestra cómo iniciar el análisis mediante un desencadenador de software, que se detendrá la digitalización una vez que examina un código de barras:

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
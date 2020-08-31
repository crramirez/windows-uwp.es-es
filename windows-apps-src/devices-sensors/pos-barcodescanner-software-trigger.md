---
title: Usar un desencadenador de software
description: Obtenga información sobre cómo controlar el proceso de exploración de códigos de barras mediante programación con un desencadenador de software asincrónico.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: baa556958750b4f88997746786a9b5fe92e601b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168519"
---
# <a name="use-a-software-trigger"></a>Usar un desencadenador de software

Puede ser útil controlar el acto de digitalizar desde el software si usa un escáner de código de barras en el modo de presentación o si el escáner no tiene un desencadenador físico, como un escáner de códigos de barras basado en la cámara. Puede iniciar el proceso de digitalización mediante una llamada a [StartSoftwareTriggerAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

En función del valor de [IsDisabledOnDataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , el escáner puede examinar solo un código de barras y, a continuación, detener o examinar continuamente hasta que llame a [StopSoftwareTriggerAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Establezca el valor deseado de [IsDisabledOnDataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar el comportamiento del analizador cuando se descodifica un código de barras.

| Value | Descripción |
| ----- | ----------- |
| True   | Examinar solo un código de barras y detener |
| Falso  | Examinar los códigos de barras continuamente sin detenerse |


> [!Important]
> Confirme que el escáner de códigos de barras admite el uso de desencadenadores de software. para ello, compruebe primero la propiedad [IsSoftwareTriggerSupported](/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

En el ejemplo siguiente se muestra cómo iniciar el análisis mediante un desencadenador de software, que detendrá el análisis una vez que recorre un código de barras:

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
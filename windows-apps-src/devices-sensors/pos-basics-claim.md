---
author: TerryWarwick
title: Modelo de notificación de dispositivo PointOfService
description: Más información acerca del modelo de notificación de PointOfService
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 202234530945e55ef9c0d0fb68cf9ca83d2e15c3
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983741"
---
# <a name="point-of-service-device-claim-model"></a>Modelo de notificación de dispositivo de punto de servicio

## <a name="claiming-a-device-for-exclusive-use"></a>Reclamar un dispositivo para uso exclusivo

Después de haber creado un objeto de dispositivo PointOfService correctamente, debes reclamarlo mediante el método de notificación adecuado para el tipo de dispositivo para poder usar el dispositivo para la entrada o salida.  La notificación concede a la aplicación acceso exclusivo a muchas de las funciones del dispositivo para garantizar que una aplicación no interfiere con el uso del dispositivo por parte de otra aplicación.  Solo una aplicación puede reclamar un dispositivo PointOfService para uso exclusivo cada vez. 

Este ejemplo muestra cómo reclamar un dispositivo de escáner de códigos de barras después de haber creado correctamente un objeto de escáner de códigos de barras.

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

> [!Warning]
> Una notificación se puede perder en las siguientes circunstancias:
> 1. Otra aplicación ha solicitado una notificación del mismo dispositivo y la aplicación no emitió un **RetainDevice** en respuesta al evento **ReleaseDeviceRequested**.  (Consulta [Negociación de notificaciones](#Claim-negotiation) a continuación para obtener más información.)
> 2. Se ha suspendido la aplicación, lo que ha originado que se cierre el objeto del dispositivo y que la notificación ya no sea válida. (Consulta [Ciclo de vida de objetos de dispositivos](pos-basics-deviceobject.md#device-object-lifecycle) para obtener más información.)

### <a name="apis-used-for-claiming"></a>API que se usan para reclamar

|Dispositivo|Notificación |
|-|:-|
|BarcodeScanner | [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | 
|CashDrawer | [ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | 
|LineDisplay | [ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |
|MagneticStripeReader | [ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) | 
|PosPrinter | [ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) | 
|

## <a name="claim-negotiation"></a>Negociación de notificaciones

Dado que Windows es un entorno de multitarea, es posible que varias aplicaciones del mismo equipo requieran acceso a periféricos de forma cooperativa.  Las APIs de PointOfService proporcionan un modelo de negociación que permite que varias aplicaciones compartan periféricos conectados al equipo.

Cuando una segunda aplicación del mismo equipo solicita una notificación para un periférico PointOfService que ya se ha solicitado por otra aplicación, se publica una notificación de evento **ReleaseDeviceRequested**. La aplicación con la notificación activa debe responder a la notificación de eventos mediante una llamada a **RetainDevice** si la aplicación está usando actualmente el dispositivo para evitar la pérdida de la notificación. 

Si la aplicación con la notificación activa no responde con **RetainDevice** al instante, se supone que la aplicación se ha suspendido o que no necesita el dispositivo y la notificación se revoca y se da a la nueva aplicación. 

Este ejemplo muestra cómo conservar un escáner de códigos de barras reclamado, después de que otra aplicación haya solicitado que se libere el dispositivo.  

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
{
    // Retain exclusive access to the device
    myScanner.RetainDevice();  
}
```
### <a name="apis-used-for-claim-negotiation"></a>API que se usan para la negociación de notificaciones

|Dispositivo reclamado|Notificación de lanzamiento| Conservar dispositivo |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|

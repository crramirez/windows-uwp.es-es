---
title: Notificaciones de dispositivo PointOfService y habilitar modelo
description: Más información sobre la declaración de PointOfService y el modelo de habilitación
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: bc3a8afbc0d3ca4655e0b1745090db633bcd92b7
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684675"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Notificaciones de dispositivo de punto de servicio y habilitar modelo

## <a name="claiming-for-exclusive-use"></a>Reclamar para uso exclusivo

Después de haber creado un objeto de dispositivo PointOfService correctamente, debes reclamarlo mediante el método de notificación adecuado para el tipo de dispositivo para poder usar el dispositivo para la entrada o salida.  La notificación concede a la aplicación acceso exclusivo a muchas de las funciones del dispositivo para garantizar que una aplicación no interfiere con el uso del dispositivo por parte de otra aplicación.  Solo una aplicación puede reclamar un dispositivo PointOfService para uso exclusivo cada vez. 

> [!Note]
> La acción de la demanda establece un bloqueo exclusivo en un dispositivo, pero no lo pone en un estado operativo.  Vea [Habilitar el dispositivo para operaciones de e/s](#enable-device-for-io-operations) para obtener más información.

### <a name="apis-used-to-claim--release"></a>API usadas para notificar o liberar

|Dispositivo|Notificación | Publicación | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Habilitar dispositivo para operaciones de e/s

La acción de la demanda simplemente establece derechos exclusivos en el dispositivo, pero no lo pone en un estado operativo.  Para recibir eventos o realizar operaciones de salida, debe habilitar el dispositivo mediante **EnableAsync**.  Por el contrario, puede llamar a **DisableAsync** para dejar de escuchar los eventos del dispositivo o realizar la salida.  También puede usar **IsEnabled** para determinar el estado del dispositivo.

### <a name="apis-used-enable--disable"></a>API usadas habilitada/deshabilitada

| Dispositivo | Habilitar | Deshabilitas | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | No aplicable ¹ | No aplicable ¹ | No aplicable ¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ la presentación de líneas no requiere habilitar explícitamente el dispositivo para las operaciones de e/s.  La habilitación se realiza automáticamente mediante las API de LineDisplay de PointOfService que realizan la e/s.

## <a name="code-sample-claim-and-enable"></a>Ejemplo de código: Claim y enable

Este ejemplo muestra cómo reclamar un dispositivo de escáner de códigos de barras después de haber creado correctamente un objeto de escáner de códigos de barras.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> Una notificación se puede perder en las siguientes circunstancias:
> 1. Otra aplicación ha solicitado una notificación del mismo dispositivo y la aplicación no emitió un **RetainDevice** en respuesta al evento **ReleaseDeviceRequested**.  (Consulta [Negociación de notificaciones](#claim-negotiation) a continuación para obtener más información.)
> 2. Se ha suspendido la aplicación, lo que ha originado que se cierre el objeto del dispositivo y que la notificación ya no sea válida. (Consulta [Ciclo de vida de objetos de dispositivos](pos-basics-deviceobject.md#device-object-lifecycle) para obtener más información.)


## <a name="claim-negotiation"></a>Negociación de notificaciones

Dado que Windows es un entorno de multitarea, es posible que varias aplicaciones del mismo equipo requieran acceso a periféricos de forma cooperativa.  Las APIs de PointOfService proporcionan un modelo de negociación que permite que varias aplicaciones compartan periféricos conectados al equipo.

Cuando una segunda aplicación del mismo equipo solicita una notificación para un periférico PointOfService que ya se ha solicitado por otra aplicación, se publica una notificación de evento **ReleaseDeviceRequested**. La aplicación con la notificación activa debe responder a la notificación de eventos mediante una llamada a **RetainDevice** si la aplicación está usando actualmente el dispositivo para evitar la pérdida de la notificación. 

Si la aplicación con la notificación activa no responde con **RetainDevice** al instante, se supone que la aplicación se ha suspendido o que no necesita el dispositivo y la notificación se revoca y se da a la nueva aplicación. 

El primer paso es crear un controlador de eventos que responda al evento **ReleaseDeviceRequested** con **RetainDevice**.  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

Después, registre el controlador de eventos en asociación con el dispositivo reclamado.

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
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

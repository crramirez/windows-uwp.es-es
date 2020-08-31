---
title: Notificaciones de dispositivo PointOfService y habilitar modelo
description: Use el punto de servicio de notificaciones de dispositivo y habilite las API para reclamar dispositivos y habilitarlos para operaciones de e/s.
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 1977fd5db2f2e026ae4bbab21de9683f275e96d3
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053755"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Notificaciones de dispositivo de punto de servicio y habilitar modelo

## <a name="claiming-for-exclusive-use"></a>Reclamar para uso exclusivo

Después de haber creado correctamente un objeto de dispositivo PointOfService, debe reclamarlo mediante el método de notificaciones adecuado para el tipo de dispositivo antes de poder usar el dispositivo para la entrada o salida.  La solicitud concede a la aplicación acceso exclusivo a muchas de las funciones del dispositivo para asegurarse de que una aplicación no interfiera con el uso del dispositivo por parte de otra aplicación.  Solo una aplicación puede reclamar un dispositivo PointOfService para su uso exclusivo a la vez. 

> [!Note]
> La acción de la demanda establece un bloqueo exclusivo en un dispositivo, pero no lo pone en un estado operativo.  Vea [Habilitar el dispositivo para operaciones de e/s](#enable-device-for-io-operations) para obtener más información.

### <a name="apis-used-to-claim--release"></a>API usadas para notificar o liberar

|Dispositivo|Notificación | Release | 
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

| Dispositivo | Habilitar | Disable | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | No aplicable ¹ | No aplicable ¹ | No aplicable ¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ la presentación de líneas no requiere habilitar explícitamente el dispositivo para las operaciones de e/s.  La habilitación se realiza automáticamente mediante las API de LineDisplay de PointOfService que realizan la e/s.

## <a name="code-sample-claim-and-enable"></a>Ejemplo de código: Claim y enable

En este ejemplo se muestra cómo reclamar un dispositivo de escáner de código de barras después de haber creado correctamente un objeto de escáner de código de barras.

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
> Una demanda se puede perder en las siguientes circunstancias:
> 1. Otra aplicación ha solicitado una solicitud del mismo dispositivo y la aplicación no ha emitido un **RetainDevice** en respuesta al evento **ReleaseDeviceRequested** .  (Consulte [negociación de notificaciones](#claim-negotiation) a continuación para obtener más información).
> 2. La aplicación se ha suspendido, lo que dio lugar a que se cerrara el objeto de dispositivo y, como resultado, la demanda ya no es válida. (Consulte [ciclo de vida de objetos de dispositivo](pos-basics-deviceobject.md#device-object-lifecycle) para obtener más información).


## <a name="claim-negotiation"></a>Negociación de notificaciones

Puesto que Windows es un entorno con varias tareas, es posible que varias aplicaciones del mismo equipo requieran acceso a los periféricos de forma cooperativa.  Las API de PointOfService proporcionan un modelo de negociación que permite que varias aplicaciones compartan periféricos conectados al equipo.

Cuando una segunda aplicación en el mismo equipo solicita una notificación para un periférico PointOfService que ya ha reclamado otra aplicación, se publica una notificación de evento **ReleaseDeviceRequested** . La aplicación con la notificación activa debe responder a la notificación de eventos mediante una llamada a **RetainDevice** si la aplicación está usando actualmente el dispositivo para evitar la pérdida de la notificación. 

Si la aplicación con la solicitud activa no responde con **RetainDevice** inmediatamente, se supone que la aplicación se ha suspendido o no necesita el dispositivo y la solicitud se revoca y se asigna a la nueva aplicación. 

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



### <a name="apis-used-for-claim-negotiation"></a>API usadas para la negociación de notificaciones

|Dispositivo reclamado|Notificación de versión| Conservar dispositivo |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|

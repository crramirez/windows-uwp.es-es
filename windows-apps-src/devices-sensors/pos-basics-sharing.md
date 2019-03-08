---
title: Uso compartido del dispositivo PointOfService
description: Uso compartido de periféricos PointOfService con otros usuarios
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618950"
---
# <a name="pointofservice-device-sharing"></a>Uso compartido del dispositivo PointOfService

Obtenga información sobre cómo compartir periféricos Bluetooth conectado o red con otros equipos en un entorno donde varios PC dependen de periféricos compartidos en lugar de dedicado periférico adjunto a cada equipo.

## <a name="device-sharing"></a>Uso compartido del dispositivo

Red y Bluetooth periféricos PointOfService conectados normalmente se usan en un entorno wheere varios dispositivos cliente comparten los mismos periféricos a lo largo del día.  En un entorno comercial o de servicios de alimentación ocupado cualquier retraso en la capacidad de un dispositivo de cliente para adjuntar en un dispositivo periférico tiene un impacto en el rendimiento en el que un colaborador puede cerrar una transacción con el cliente y pasar a la siguiente. En un escenario de restaurante servicio rápido donde se usa una impresora de recepción como una impresora de cocina para transferir los detalles de pedido de un cliente a la cocina de preparación habrá varios dispositivos de cliente aceptar pedidos de clientes.  Una vez completada la orden de cada dispositivo de cliente debe ser capaz de notificación de la impresora compartida y se imprime inmediatamente el orden de la cocina.

En estos entornos, es importante para que la aplicación completamente **dispose** el dispositivo de objeto para que otro puede reclamar el mismo dispositivo.

Eliminación de un PosPrinter al final de un bloque "using"

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Eliminación de un PosPrinter mediante una llamada a Dispose() explícitamente

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Usar métodos de la API 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]

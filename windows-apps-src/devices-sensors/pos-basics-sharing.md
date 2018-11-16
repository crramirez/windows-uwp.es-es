---
author: TerryWarwick
title: Uso compartido de dispositivos PointOfService
description: Compartir dispositivos periféricos de PointOfService con otras personas
ms.author: jken
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 591ba592d1c17b03ae29c6fb98ef546bc18b8bdc
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981496"
---
# <a name="pointofservice-device-sharing"></a>Uso compartido de dispositivos PointOfService

Obtén información sobre cómo compartir la red o periféricos Bluetooth conectado con otros equipos en un entorno donde varios equipos dependen de periféricos compartidos en lugar de dedicada periféricos conectados a cada equipo.

## <a name="device-sharing"></a>Uso compartido de dispositivos

Red y Bluetooth periféricos de PointOfService conectados por lo general, se usan en un entorno wheere varios dispositivos cliente están compartiendo los mismos periféricos durante todo el día.  En un entorno comercial o servicios de alimentos ocupado cualquier retraso en la posibilidad de que un dispositivo de cliente adjuntar a un periférico tiene un impacto en la eficacia en el que un asociado puede cerrar una transacción con el cliente y pasar al siguiente. En un escenario de restaurantes servicio rápido donde se usa una impresora de recibos como una impresora de cocina para transferir los detalles del pedido de un cliente a la cocina para la preparación habrá varios dispositivos de cliente, toma nota de pedidos de clientes.  Una vez completado el orden de cada dispositivo de cliente debe ser capaz de notificación de la impresora compartida e imprimir inmediatamente el orden de la cocina.

En estos entornos, es importante para la aplicación totalmente **Eliminar** el objeto de dispositivo para que otro puede reclamar el mismo dispositivo.

Eliminar una PosPrinter al final de un bloque 'usando'

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


Eliminar una PosPrinter mediante una llamada a Dispose() explícitamente

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Métodos de API que se usa 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]

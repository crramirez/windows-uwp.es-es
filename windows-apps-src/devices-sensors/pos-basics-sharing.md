---
title: Uso compartido de dispositivos PointOfService
description: Obtenga información acerca de cómo compartir periféricos conectados a la red o Bluetooth con otros equipos en un entorno en el que varios equipos confían en periféricos compartidos.
ms.date: 06/14/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4fad9bc75ed0ff79be1596a3c99445c7e9f97b1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163269"
---
# <a name="pointofservice-device-sharing"></a>Uso compartido de dispositivos PointOfService

Obtenga información acerca de cómo compartir periféricos conectados a la red o Bluetooth con otros equipos en un entorno en el que varios equipos se basan en periféricos compartidos en lugar de periféricos dedicados conectados a cada equipo.

## <a name="device-sharing"></a>Uso compartido de dispositivos

Los periféricos PointOfService conectados a la red y Bluetooth se suelen usar en un entorno wheere varios dispositivos cliente comparten los mismos periféricos a lo largo del día.  En un entorno de servicios minoristas o alimenticios ocupado, cualquier retraso en la capacidad para que un dispositivo cliente se conecte a un periférico tiene un impacto en la eficacia en la que un asociado puede cerrar una transacción con el cliente y pasar a la siguiente. En un escenario de restaurante de servicio rápido en el que se usa una impresora de recepción como impresora de cocina para transferir los detalles del pedido de un cliente a la cocina para su preparación, habrá varios dispositivos cliente que realicen pedidos de clientes.  Una vez completado el pedido, cada dispositivo cliente debe ser capaz de reclamar la impresora compartida e imprimir inmediatamente el pedido de la cocina.

En estos entornos, es importante que la aplicación **elimine** completamente el objeto de dispositivo para que otro pueda reclamar el mismo dispositivo.

Eliminación de una PosPrinter al final de un bloque ' Using '

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


Desechar un PosPrinter mediante una llamada explícita a Dispose ()

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Métodos de API usados 

+ [BarcodeScanner. Dispose](/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer. Dispose](/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay. Dispose](/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader. Dispose](/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter. Dispose](/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
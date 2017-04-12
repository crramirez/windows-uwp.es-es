---
author: mukin
title: Impresora POS
description: "En este artículo se incluye información sobre la familia de dispositivos de impresora de punto de servicio."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d8340af651157bb6fae82785812f259c16d0a6c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pos-printer"></a>Impresora POS

Permite a los desarrolladores de aplicaciones imprimir en [impresoras de recibos](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.posprinter) conectadas a la red o mediante Bluetooth usando el lenguaje de control de impresora Epson ESC/POS.

## <a name="requirements"></a>Requisitos
Las aplicaciones que requieren este espacio de nombres requieren la adición del objeto "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) al manifiesto del paquete de aplicación.

## <a name="device-support"></a>Compatibilidad con dispositivos
Windows admite la capacidad de imprimir en impresoras de recibos conectadas a la red y mediante Bluetooth usando el lenguaje de control de impresora Epson ESC/POS. Para obtener más información sobre ESC/POS, consulta [Epson ESC/POS con formato](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting).

Si bien las clases, enumeraciones e interfaces expuestas en la API admiten impresoras de recibos, impresoras de boletos e impresoras de diarios, la interfaz de controlador solo admite impresoras de recibos. Si intentas usar una impresora de boletos o diarios en este momento, devolverá un estado de no implementada.

Actualmente, la compatibilidad está limitada a los modelos de dispositivo de red y Bluetooth que se enumeran en las tablas siguientes. En la actualidad, no se admiten impresoras conectadas mediante USB. Vuelve a consultar para obtener información sobre la compatibilidad que se agregue en el futuro.

### <a name="stationary-pos-printers-network-bluetooth"></a>Impresoras POS inmóviles (red, Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>Impresoras POS móviles (Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |

## <a name="examples"></a>Ejemplos
Consulta el ejemplo de impresora POS para ver un ejemplo de implementación.
+ [Ejemplo de impresora POS](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

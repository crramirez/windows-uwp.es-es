---
author: mukin
title: Caja registradora
description: "En este artículo se incluye información sobre la familia de dispositivos de punto de servicio de la caja registradora."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 
ms.openlocfilehash: 376272356cf720ddd9519f0077e771a1016abb1e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="cash-drawer"></a>Caja registradora

Permite a los desarrolladores de la aplicación interactuar con [cajas registradoras](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.cashdrawer).

## <a name="requirements"></a>Requisitos
Las aplicaciones que requieren este espacio de nombres requieren la adición del objeto "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) al manifiesto del paquete de aplicación.

## <a name="device-support"></a>Compatibilidad con dispositivos
La conexión directa a la caja registradora se puede establecer a través de la red o Bluetooth, según las funcionalidades de la unidad de caja registradora. Además, las cajas registradoras que no tienen funcionalidades de red o Bluetooth se pueden conectar a través del puerto DK en una impresora POS compatible o el accesorio Star Micronics DK-AirCash. En estos momentos, no hay compatibilidad con cajas registradoras conectadas mediante puerto USB o serie.

**Nota:**  Ponte en contacto con Star Micronics para obtener más información sobre AirCash DK.

### <a name="networkbluetooth"></a>Red o Bluetooth
| Fabricante |    Modelos |
|--------------|-----------|
| APG Cash Drawer |    NetPRO, BluePRO |

## <a name="examples"></a>Ejemplos
Consulta el ejemplo de caja registradora para obtener un ejemplo de implementación.
+    [Ejemplo de caja registradora](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)

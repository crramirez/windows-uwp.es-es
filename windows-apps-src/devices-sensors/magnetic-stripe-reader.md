---
author: mukin
title: "Lector de bandas magnéticas"
description: "En este artículo se incluye información sobre la familia de dispositivos de punto de servicio de lector de bandas magnéticas."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a11fe7a63c0444ac986e7bfe0d50472249e5196e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="magnetic-stripe-reader"></a>Lector de bandas magnéticas

Permite a los desarrolladores de aplicaciones acceder a [lectores de bandas magnéticas](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.magneticstripereader) para recuperar datos de tarjetas con banda magnética, tales como tarjetas de crédito o débito, tarjetas de fidelización, tarjetas de acceso, etc.

## <a name="requirements"></a>Requisitos
Las aplicaciones que requieren este espacio de nombres requieren la adición del objeto "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) al manifiesto del paquete de aplicación.

## <a name="device-support"></a>Compatibilidad con dispositivos
### <a name="usb"></a>USB
### <a name="supported-vendor-specific"></a>Específicos del proveedor admitidos
Windows ofrece compatibilidad con los siguientes lectores de bandas magnéticas de Magtek e IDTech en función de su identificador de proveedor e identificador producto (VID/PID).

| Fabricante |     Modelos |    Número de pieza |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>Específico de proveedor personalizado
Windows admite la implementación de controladores adicionales específicos del proveedor para la compatibilidad con lectores de bandas magnéticas adicionales. Ponte en contacto con el fabricante del lector de bandas magnéticas para conocer la disponibilidad.

## <a name="examples"></a>Ejemplos
Consulta el ejemplo de lector de bandas magnéticas para ver un ejemplo de implementación.
+    [Ejemplo de lector de bandas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)

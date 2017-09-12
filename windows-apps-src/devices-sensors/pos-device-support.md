---
author: mukin
title: Compatibilidad con dispositivos POS
description: "En este artículo se incluye información sobre la compatibilidad con dispositivos para cada familia de dispositivos POS"
ms.author: mukin
ms.date: 05/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 58018385082f7f7e49edba0351d919bc840ade05
ms.sourcegitcommit: 53930c9871461f6106f785ae4fabb2296eb359f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2017
---
# <a name="pos-device-support"></a>Compatibilidad con dispositivos POS

## <a name="barcode-scanner"></a>Escáner de códigos de barras
| Conectividad | Compatibilidad |
| -------------|-------------|
| USB          | <p>Windows contiene un controlador de clases integrado para escáneres de códigos de barras conectados por USB que se basa en la especificación la tabla de uso de escáneres HID (8c) definida por [USB.org](http://www.usb.org/developers/hidpage/). Consulta la siguiente tabla para obtener una lista de los dispositivos compatibles conocidos.  Consulta el manual del escáner de códigos de barras o ponte en contacto con el fabricante para determinar si se puede configurar en modo de escáner USB.HID.POS. </p><p>Windows también admite la implementación de controladores específicos del proveedor para admitir escáneres de códigos de barras adicionales que no admiten el estándar de escáner USB.HID.POS. Ponte en contacto con el fabricante del escáner de códigos de barras para información sobre la disponibilidad de controladores específicos del proveedor.</p>|
| Bluetooth    | <p>Windows admite escáneres de códigos de barras Bluetooth basados en SPP-SSI. Consulta la siguiente tabla para obtener una lista de los dispositivos compatibles conocidos.</p> |

### <a name="compatible-hardware"></a>Hardware compatible
| Categoría | Conectividad | Fabricante y modelo |
|--------------|-----------|-----------|
| **Lectores 1D de mano** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg (actualizable)|
| **Lectores 1D de mano** | **Bluetooth** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800 (anteriormente CHS 8Ci) <br/>|
|**Lectores 2D de mano** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg (actualizable)<br/>Honeywell Voyager 1602g<br/>Intermec SG20|
|**Lectores 2D de mano** | **Bluetooth** |Socket Mobile SocketScan S850 (anteriormente CHS 8Qi)|
| **Escáneres de presentación** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **Escáneres de mostrador** | **USB** |Honeywell Stratos 2700|
| **Motores de análisis** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Dispositivos WindowsMobile**| **Integrados** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Dispositivos WindowsMobile**| **Personalizados** | HP Elite X3 con bolsillo de escáner de códigos de barras |

## <a name="cash-drawer"></a>Caja registradora
| Conectividad | Compatibilidad |
| -------------|-------------|
| Red o Bluetooth | <p> La conexión directa a la caja registradora se puede establecer a través de la red o Bluetooth, según las funcionalidades de la unidad de caja registradora. </p>|
| Puerto DK | <p> Las cajas registradoras que no tienen funcionalidades de red o Bluetooth se pueden conectar a través del puerto DK en una impresora POS compatible o el accesorio Star Micronics DK-AirCash. </p>
| OPOS    | <p> Admite todos los dispositivos de caja registradora compatibles con controladores OPOS o proporciona objetos de servicio OPOS. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo en particular. Esto permite conexiones USB y otras según las especificaciones del fabricante. </p> |

**Nota:**  Ponte en contacto con Star Micronics para obtener más información sobre AirCash DK.

### <a name="networkbluetooth"></a>Red o Bluetooth
| Fabricante |    Modelos |
|--------------|-----------|
| APG Cash Drawer |    NetPRO, BluePRO |

## <a name="line-display"></a>Visualización de líneas
Admite todos los dispositivos de visualización de líneas compatibles con controladores OPOS o proporciona objetos de servicio OPOS. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo en particular.

## <a name="magnetic-stripe-reader"></a>Lector de bandas magnéticas

Windows contiene un controlador de clases integrado para lectores de bandas magnéticas conectados por USB que se basa en la especificación la tabla de uso de escáneres HID (8c) definida por [USB.org](http://www.usb.org/developers/hidpage/).

### <a name="vendor-specific-support"></a>Compatibilidad específica del proveedor
Windows ofrece compatibilidad con los siguientes lectores de bandas magnéticas de Magtek e IDTech en función de su identificador de proveedor e identificador producto (VID/PID).

| Fabricante |     Modelos |    Número de pieza |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>Específico de proveedor personalizado
Windows admite la implementación de controladores adicionales específicos del proveedor para la compatibilidad con lectores de bandas magnéticas adicionales. Ponte en contacto con el fabricante del lector de bandas magnéticas para conocer la disponibilidad.

## <a name="pos-printer"></a>Impresora POS
Windows admite la capacidad de imprimir en impresoras de recibos conectadas a la red y mediante Bluetooth usando el lenguaje de control de impresora Epson ESC/POS. Para obtener más información sobre ESC/POS, consulta [Epson ESC/POS con formato](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting).

Si bien las clases, enumeraciones e interfaces expuestas en la API admiten impresoras de recibos, impresoras de boletos e impresoras de diarios, la interfaz de controlador solo admite impresoras de recibos. Si intentas usar una impresora de boletos o diarios en este momento, devolverá un estado de no implementada.

| Conectividad | Compatibilidad |
| -------------|-------------|
| Red o Bluetooth | <p> La conexión directa a la impresora POS se puede establecer a través de la red o Bluetooth, según las funcionalidades de la unidad de impresora POS. </p>|
| OPOS    | <p> Admite todos los dispositivos impresora POS compatibles con los controladores OPOS o proporciona objetos de servicio OPOS. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo en particular. Esto permite conexiones USB y otras según las especificaciones del fabricante. </p> |

### <a name="stationary-pos-printers-networkbluetooth"></a>Impresoras POS inmóviles (red, Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>Impresoras POS móviles (Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |
---
author: TerryWarwick
title: Compatibilidad de hardware con punto de servicio (POS)
description: En este artículo se incluye información sobre la compatibilidad de hardware para cada clase de dispositivo con punto de servicio
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb2468497115c9595f6fd17ab61b30caed507ab
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832099"
---
# <a name="supported-point-of-service-peripherals"></a>Periféricos con puntos de servicio compatibles

## <a name="barcode-scanner"></a>Escáner de código de barras
| Conectividad | Compatibilidad |
| -------------|-------------|
| USB          | <p>Windows contiene un controlador de clases integrado para escáneres de códigos de barras conectados por USB que se basa en la especificación la tabla de uso de escáneres HID POS (8c) definida por [USB.org](http://www.usb.org/developers/hidpage/). Consulta la tabla siguiente para ver una lista de los dispositivos compatibles conocidos.  Consulta el manual del escáner de códigos de barras o ponte en contacto con el fabricante para determinar cómo configurar el escáner en modo **USB.HID.POS Scanner**. </p><p>Windows también admite la implementación de controladores específicos del proveedor para admitir escáneres de códigos de barras adicionales que no admiten el estándar de escáner USB.HID.POS. Ponte en contacto con el fabricante del escáner de códigos de barras para información sobre la disponibilidad de controladores específicos del proveedor.</p><p>Los fabricantes de escáneres de códigos de barras pueden consultar la [Guía de diseño de controladores de escáner de códigos de barras](https://aka.ms/pointofservice-drv) para obtener información sobre cómo crear un controlador de escáner de códigos de barras personalizado</p> |
| Bluetooth    | <p>Windows admite el protocolo de puerto serie: escáneres de códigos de barras de Bluetooth con interfaz serie única (SPP SSI). Consulta la siguiente tabla para obtener una lista de los dispositivos compatibles conocidos. Consulta el manual del escáner de código de barras o ponte en contacto con el fabricante para determinar cómo configurar el escáner en modo **SPP-SSI**.</p> |
| Cámara web       | <p>A partir de la versión 1803 de Windows 10, se pueden leer códigos de barras a través del objetivo de una cámara estándar desde una aplicación Universal de Windows. Se recomienda que uses una cámara con enfoque automático y una resolución mínima de 1920 x 1440.  Algunas cámaras de resolución más baja pueden leer códigos de barras estándar si la impresión es lo suficientemente grande.  Los códigos de barras con elementos más estrechos pueden requerir cámaras de mayor resolución.</p>| 
|


### <a name="compatible-barcode-scanners"></a>Escáneres de código de barras compatibles
| Categoría | Conectividad | Fabricante y modelo |
|--------------|-----------|-----------|
| **Lectores 1D de mano** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg (actualizable)|
| **Lectores 1D de mano** | **Bluetooth** |Socket Mobile CHS 7Ci<br/> Socket Mobile CHS 7Di<br/> Socket Mobile CHS 7Mi<br/> Socket Mobile CHS 7Pi<br/>Socket Mobile DuraScan D700<br/> Socket Mobile DuraScan D730<br/>Socket Mobile SocketScan S800 (anteriormente CHS 8Ci) <br/>|
|**Lectores 2D de mano** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg (actualizable)<br/>Honeywell Voyager 1602g<br/>Intermec SG20<br/>Zebra DS2278<br/>Zebra DS8108 ¹<hr><small>¹ firmware mínimo necesario: 016 (2018.01.18). Actualizable con [123Scan](http://www.zebra.com/123Scan)</small>|
|**Lectores 2D de mano** | **Bluetooth** |Socket Mobile SocketScan S850 (anteriormente CHS 8Qi)|
| **Escáneres de presentación** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **Escáneres de mostrador** | **USB** |Honeywell Stratos 2700|
| **Motores de análisis** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Dispositivos WindowsMobile**| **Integrados** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Dispositivos WindowsMobile**| **Personalizados** | HP Elite X3 con bolsillo de escáner de códigos de barras |


## <a name="cash-drawer"></a>Caja registradora
| Conectividad | Compatibilidad |
| -------------|-------------|
| Red o Bluetooth | <p> La conexión directa a la caja registradora se puede establecer a través de la red o Bluetooth, según las funcionalidades de la unidad de caja registradora. </p><p>Caja registradora APG : NetPRO, BluePRO</p> |
| Puerto DK | <p> Las cajas registradoras que no tienen funcionalidades de red o Bluetooth se pueden conectar a través del puerto DK en una impresora de recibos o en el accesorio Star Micronics DK-AirCash. </p>
| OPOS    | <p> Admite cualquier caja registradora compatible con OPOS a través de objetos de servicio OPOS proporcionados por el fabricante. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo. </p> |


## <a name="customer-display-linedisplay"></a>Pantalla del cliente (LineDisplay)
Admite cualquier pantalla de líneas compatible con OPOS a través de objetos de servicio OPOS proporcionados por el fabricante. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo.

## <a name="magnetic-stripe-reader"></a>Lector de bandas magnéticas
Windows ofrece compatibilidad con los siguientes lectores de bandas magnéticas de Magtek e IDTech en función de su identificador de proveedor e identificador de producto (VID/PID).

| Fabricante |    Modelos |  Número de pieza |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag (VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |  210730xx |
| | Dynamag (VID:0801 PID:0002) |   210401xx |

 Windows admite la implementación de controladores adicionales específicos de proveedores para la compatibilidad con lectores de bandas magnéticas adicionales. Ponte en contacto con el fabricante del lector de bandas magnéticas para conocer la disponibilidad. Los fabricantes de lectores de bandas magnéticas pueden consultar la [Guía de diseño de controladores de lectores de bandas magnéticas](https://aka.ms/pointofservice-drv) para obtener información sobre cómo crear un controlador de lector de bandas magnéticas personalizado.

## <a name="receipt-printer-posprinter"></a>Impresora de recibos (POSPrinter)
| Conectividad | Compatibilidad |
| -------------|-------------|
| Red y Bluetooth | <p>Windows admite impresoras de recibos conectadas a la red y por Bluetooth, usando el lenguaje de control de impresora Epson ESC/POS.  Las impresoras que se indican a continuación se detectan automáticamente con las API de POSPrinter. Las impresoras de recibos adicionales que proporcionen emulación ESC/POS también pueden funcionar, pero sería necesario asociarlas usando un proceso de [emparejamiento fuera de banda](https://aka.ms/pointofservice-oobpairing).</p><p>Nota: las estaciones de resguardos y diarios no se admiten a través de este método.</p> |
| OPOS    | <p> Admite las impresoras de recibos compatibles con OPOS a través de objetos de servicio OPOS. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Impresoras de recibos inmóviles (red, Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Impresoras de recibos móviles (Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |

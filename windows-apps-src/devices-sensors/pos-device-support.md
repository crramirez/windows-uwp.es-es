---
title: Compatibilidad de hardware con punto de servicio (POS)
description: En este artículo se incluye información sobre la compatibilidad de hardware para cada clase de dispositivo con punto de servicio
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6d67dd7bc7d2f6323679dd7c69a98df841b2848c
ms.sourcegitcommit: 769ec7811aaaa79fe521e3e984a2e1a2a9671caf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70057816"
---
# <a name="supported-point-of-service-peripherals"></a>Punto admitido de periféricos de servicio

## <a name="barcode-scanner"></a>Escáner de código de barras
| Conectividad | Soporte técnico |
| -------------|-------------|
| USB          | <p>Windows contiene un controlador de clase integrada para los escáneres de códigos de barras conectados USB, que se basa en la especificación de tabla de uso de escáneres de POS HID (8C) definida por [USB.org](https://www.usb.org/hid). Consulta la siguiente tabla para obtener una lista de los dispositivos compatibles conocidos.  Consulta el manual del escáner de códigos de barras o ponte en contacto con el fabricante para determinar cómo configurar el escáner en modo **USB.HID.POS Scanner**. </p><p>Windows también admite la implementación de controladores específicos del proveedor para admitir escáneres de códigos de barras adicionales que no admiten el estándar de escáner USB.HID.POS. Ponte en contacto con el fabricante del escáner de códigos de barras para información sobre la disponibilidad de controladores específicos del proveedor.</p><p>Los fabricantes de escáneres de códigos de barras pueden consultar la [Guía de diseño de controladores de escáner de códigos de barras](https://aka.ms/pointofservice-drv) para obtener información sobre cómo crear un controlador de escáner de códigos de barras personalizado</p> |
| Bluetooth    | <p>Windows admite el protocolo de puerto serie: escáneres de códigos de barras de Bluetooth con interfaz serie única (SPP SSI). Consulta la siguiente tabla para obtener una lista de los dispositivos compatibles conocidos. Consulta el manual del escáner de código de barras o ponte en contacto con el fabricante para determinar cómo configurar el escáner en modo **SPP-SSI**.</p> |
| Cámara web       | <p>A partir de la versión 1803 de Windows 10, se pueden leer códigos de barras a través del objetivo de una cámara estándar desde una aplicación Universal de Windows. Se recomienda que uses una cámara con enfoque automático y una resolución mínima de 1920 x 1440.  Algunas cámaras de resolución más baja pueden leer códigos de barras estándar si la impresión es lo suficientemente grande.  Los códigos de barras con elementos más estrechos pueden requerir cámaras de mayor resolución.</p>| 
|


| Fabricante  | Modelo                          | Capacidad | Conexión    | Type         | Modo                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Código          | Lector™ 950                    | 2D         | USB          | X30     | Escáner de PDV HID           |
| Código          | Lector™ 1021                   | 2D         | USB          | X30     | Escáner de PDV HID           |
| Código          | Lector™ 1421                   | 2D         | USB          | X30     | Escáner de PDV HID           |
| Código          | Lector™ 5000                   | 2D         | USB          | Presentación | Escáner de PDV HID           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | Presentación | Escáner de PDV HID           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | N5680                          | 2D         | Interno     | Componente    | Escáner de PDV HID           |
| Honeywell     | N3680                          | 2D         | Interno     | Componente    | Escáner de PDV HID           |
| Honeywell     | 7190g orbital                    | 2D         | USB          | Presentación | Escáner de PDV HID           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | En el contador   | Escáner de PDV HID           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Voyager 1202-BF                | 1D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Voyager 145Xg                  | 1D/2D<sup>1</sup>   | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Xenón 1900g                    | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Xenón 1902g                    | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Xenón 1902g-BF                 | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Xenón 1900h                    | 2D         | USB          | X30     | Escáner de PDV HID           |
| Honeywell     | Xenón 1902h                    | 2D         | USB          | X30     | Escáner de PDV HID           |
| HP            | Escáner de código de barras de valor (HR2150) | 2D         | USB          | X30     | Escáner de PDV HID           |
| Intermec      | SG20                           | 2D         | USB          | X30     | Escáner de PDV HID           |
| Socket Mobile | CHS 7Ci                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | CHS 7Di                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | CHS 7Mi                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | CHS 7Pi                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | CHS 8Ci                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | DuraScan D700                  | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | DuraScan D730                  | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | DuraScan D740                  | 2D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | SocketScan S700                | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | SocketScan S730                | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | SocketScan S740                | 2D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | SocketScan S800                | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket Mobile | SocketScan S850                | 2D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Cebra         | DS2208<sup>2</sup>                        | 2D         | USB          | X30     | Escáner de PDV HID           |
| Cebra         | DS2278                         | 2D         | USB          | X30     | Escáner de PDV HID           |
| Cebra         | DS8108<sup>3</sup>                        | 2D         | USB          | X30     | Escáner de PDV HID           |
| Cebra         | DS8178<sup>4</sup>                         | 2D         | USB          | X30     | Escáner de PDV HID           | 


<sup>1</sup> actualizable para admitir códigos de barras 2D a través de Honeywell <br/>
<sup>2</sup> firmware mínimo 009 (2018.07.09) obligatorio. Actualizable con Zebra [123Scan](http://www.zebra.com/123scan).<br/>
<sup>3</sup> se requiere el firmware mínimo 016 (2018.01.18). Actualizable con Zebra [123Scan](http://www.zebra.com/123scan).<br/> 
<sup>4</sup> firmware mínimo 023 (2019.03.11) obligatorio. Actualizable con Zebra [123Scan](http://www.zebra.com/123scan).<br/>

<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>Dispositivos Windows con escáner de código de barras integrado
| Fabricante   | Modelo | Sistema operativo |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>Dispositivos Windows Mobile con escáner de código de barras integrado
| Fabricante   | Modelo | Sistema operativo |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| Cebra          | TC700j | Windows Mobile   |
| HP             | Chaqueta de Elite X3 | Windows Mobile   |




## <a name="cash-drawer"></a>Caja registradora
| Conectividad | Soporte técnico |
| -------------|-------------|
| Red o Bluetooth | <p> La conexión directa a la caja registradora se puede establecer a través de la red o Bluetooth, según las funcionalidades de la unidad de caja registradora. </p><p>Cajón de caja de APG:  NetPRO, BluePRO</p> |
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
| Conectividad | Soporte técnico |
| -------------|-------------|
| Red y Bluetooth | <p>Windows admite impresoras de recibos conectadas a la red y por Bluetooth, usando el lenguaje de control de impresora Epson ESC/POS.  Las impresoras que se indican a continuación se detectan automáticamente con las API de POSPrinter. Las impresoras de recibos adicionales que proporcionen emulación ESC/POS también pueden funcionar, pero sería necesario asociarlas usando un proceso de [emparejamiento fuera de banda](https://aka.ms/pointofservice-oobpairing).</p><p>Nota: las estaciones de resguardos y diarios no se admiten a través de este método.</p> |
| OPOS    | <p> Admite las impresoras de recibos compatibles con OPOS a través de objetos de servicio OPOS. Instala los controladores OPOS según las instrucciones de instalación del fabricante del dispositivo. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Impresoras de recibos inmóviles (Red, Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Impresoras de recibos móviles (Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |

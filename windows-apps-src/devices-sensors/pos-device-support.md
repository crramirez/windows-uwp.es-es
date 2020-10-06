---
title: Soporte de hardware de punto de servicio
description: Este artículo contiene información sobre la compatibilidad de hardware para cada una de las clases de dispositivo de punto de servicio
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9e89011086e6c6d318d589226400789b41f8fe64
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750358"
---
# <a name="supported-point-of-service-peripherals"></a>Periféricos de punto de servicio admitidos

## <a name="barcode-scanner"></a>Escáner de código de barras
| Conectividad | Soporte técnico |
| -------------|-------------|
| USB          | <p>Windows contiene un controlador de clase integrada para los escáneres de códigos de barras conectados USB, que se basa en la especificación de tabla de uso de escáneres de POS HID (8C) definida por [USB.org](https://www.usb.org/hid). Vea la tabla siguiente para obtener una lista de dispositivos compatibles conocidos.  Consulte el manual del escáner de códigos de barras o póngase en contacto con el fabricante para determinar cómo configurar el escáner en **USB. HID. ** Modo de escáner de PDV. </p><p>Windows también admite la implementación de controladores específicos de proveedor para admitir escáneres de códigos de barras adicionales que no admiten USB. HID. Estándar de escáner de POS. Consulte al fabricante del escáner de códigos de barras para obtener la disponibilidad del controlador específico del proveedor.</p><p>Fabricantes de escáner de código de barras consulte la [Guía de diseño del controlador de escáner de código de barras](/windows-hardware/drivers/ddi/_pos/index) para obtener información sobre cómo crear un controlador de escáner de código de barras</p> |
| Bluetooth    | <p>Windows admite escáneres de códigos de barras Bluetooth basados en el protocolo de puerto serie (SPP-SSI). Vea la tabla siguiente para obtener una lista de dispositivos compatibles conocidos. Consulte el manual del escáner de códigos de barras o póngase en contacto con el fabricante para determinar cómo configurar el escáner en el modo **spp-SSI** .</p> |
| Cámara web       | <p>A partir de Windows 10, versión 1803, puede leer códigos de barras a través de una lente de cámara estándar desde una aplicación universal de Windows. Se recomienda usar una cámara que admita el foco automático y una resolución mínima de 1920 x 1440.  Algunas cámaras de menor resolución pueden leer códigos de barras estándar si el código de barras se imprime lo suficientemente grande.  Los códigos de barras con elementos más ligeros pueden requerir cámaras de mayor resolución.</p>| 
|


| Fabricante  | Modelo                          | Capacidad | Conexión    | Tipo         | Mode                      |
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
| Socket móvil | CHS 7Ci                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | CHS 7Di                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | CHS 7Mi                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | CHS 7Pi                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | CHS 8Ci                        | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | DuraScan D700                  | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | DuraScan D730                  | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | DuraScan D740                  | 2D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | SocketScan S700                | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | SocketScan S730                | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | SocketScan S740                | 2D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | SocketScan S800                | 1D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
| Socket móvil | SocketScan S850                | 2D         | Bluetooth    | X30     | Perfil de puerto serie (SPP) |
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




## <a name="cash-drawer"></a>Cajón de caja
| Conectividad | Soporte técnico |
| -------------|-------------|
| Red/Bluetooth | <p> La conexión directa con el cajón de caja puede realizarse a través de la red o a través de Bluetooth, en función de las capacidades de la unidad del cajón de caja. </p><p>Cajón de efectivo de APG: NetPRO, BluePRO</p> |
| Puerto DK | <p> Los alimentadores de efectivo que no tienen capacidades de red o Bluetooth se pueden conectar a través del puerto DK en una impresora de recepción admitida o en el accesorio Micronic DK-aeroefectivo de Micronics. </p>
| OPOS    | <p> Admite cualquier alimentador de efectivo compatible con OPOS a través de los objetos de servicio de OPOS proporcionados por el fabricante. Instale los controladores de OPOS según las instrucciones de instalación del fabricante del dispositivo. </p> |


## <a name="customer-display-linedisplay"></a>Pantalla de cliente (LineDisplay)
Admite cualquier pantalla compatible con OPOS que se muestre a través de los objetos de servicio de OPOS proporcionados por el fabricante. Instale los controladores de OPOS según las instrucciones de instalación del fabricante del dispositivo.

## <a name="magnetic-stripe-reader"></a>Lector de bandas magnéticas
Windows proporciona compatibilidad con los lectores de franjas magnéticas siguientes de Magtek y IDTech en función del identificador de proveedor y el ID. de producto (VID/PID).

| Fabricante |    Modelos |  Número de pieza |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID: PID de 0ACD: 2010) | IDRE-3x5xxxx |
| | MiniMag (VID: 0ACD PID: 0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID: 0801 PID: 0011) |  210730xx |
| | Dynamag (VID: 0801 PID: 0002) |   210401xx |

 Windows admite la implementación de controladores específicos de proveedor adicionales para admitir lectores de bandas magnéticas adicionales. Consulte al fabricante del lector de franjas magnéticas para obtener más disponibilidad. Fabricantes de lectores de bandas magnéticas consulte la [Guía de diseño del controlador del lector de franjas](/windows-hardware/drivers/ddi/_pos/index) magnéticas para obtener información sobre la creación de un controlador de lector de franjas magnéticas personalizado.

## <a name="receipt-printer-posprinter"></a>Impresora de recepción (POSPrinter)
| Conectividad | Soporte técnico |
| -------------|-------------|
| Red y Bluetooth | <p>Windows admite impresoras de recepción conectadas a través de Bluetooth y de red mediante el idioma de control de impresora ESC/POS de Epson.  Las impresoras que se enumeran a continuación se detectan automáticamente mediante las API de POSPrinter. Las impresoras de recepción adicionales que proporcionan una emulación ESC/POS también pueden funcionar, pero deben estar asociadas con un proceso [de emparejamiento fuera de banda](./point-of-service.md) .</p><p>Nota: la estación SLIP y las estaciones de diario no se admiten a través de este método.</p> |
| OPOS    | <p> Admite cualquier impresora de recepción compatible con OPOS a través de objetos de servicio de OPOS. Instale los controladores de OPOS según las instrucciones de instalación del fabricante del dispositivo. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Impresoras de recepción estacionales (red/Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Impresoras de recepción móvil (Bluetooth)
| Fabricante |    Modelos |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |
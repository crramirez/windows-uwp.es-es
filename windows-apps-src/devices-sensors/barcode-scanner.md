---
author: mukin
title: "Escáner de códigos de barras"
description: "En este artículo se incluye información sobre la familia de dispositivos de punto de servicio del escáner de códigos de barras."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8d9ef9bc08fa666c2af1348450f7a5fb0a0c7b65
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="barcode-scanner"></a>Escáner de códigos de barras
Permite a los desarrolladores de aplicaciones acceder a [escáneres de códigos de barras](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodescanner) para recuperar datos descodificados de una variedad de simbologías de códigos de barras, como UPC y códigos QR en función de la compatibilidad con el hardware. Consulta la clase [BarcodeSymbologies](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodesymbologies) para obtener una lista completa de simbologías compatibles.

## <a name="requirements"></a>Requisitos
Las aplicaciones que requieren este espacio de nombres requieren la adición del objeto "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) al manifiesto del paquete de la aplicación.

## <a name="device-support"></a>Compatibilidad con dispositivos

### <a name="usb"></a>USB

#### <a name="hidscanner"></a>HID.Scanner
Windows contiene un controlador de clase de escáner de códigos de barras que se basa en la página de uso HID.Scanner (8C) definida por USB.org. Este controlador de clase admitirá cualquier escáner de códigos de barras que implemente este estándar, como: Fabricante    Modelos Honeywell    1900GSR-2, 1200g-2 Intermec    SG20

Consulta el manual del escáner de códigos de barras o ponte en contacto con el fabricante para determinar si se puede configurar como USB.HID.Scanner.

#### <a name="hidvendor-specific"></a>Específico de HID.Vendor
Windows admite la implementación de controladores específicos del proveedor para admitir escáneres de códigos de barras adicionales. Ponte en contacto con el fabricante del escáner de códigos de barras para conocer la disponibilidad y saber si el dispositivo es compatible con USB.HID.Scanner de fábrica.

### <a name="bluetooth"></a>Bluetooth
#### <a name="serial-port-protocol-spp--simple-serial-interface-ssi"></a>Protocolo de puerto serie (SPP): interfaz serie sencilla (SSI)
Windows admite escáneres de códigos de barras Bluetooth basados en SPP-SSI.

| Fabricante |    Modelos |
|--------------|-----------|
| Socket Mobile |    **CHS 7 Series:** <br/> 7 Ci 1D Imager Barcode Scanner <br/> 7Di 1D Durable, Imager Barcode Scanner <br/> 7Mi 1D Laser Barcode Scanner <br/> 7Pi 1D Durable, LaserBarcode Scanner <br/> **DuraScan 700 Series:** <br/> D700 1D Imager Barcode Scanner <br/> D730 1D Laser Barcode Scanner <br/> **SocketScan 800 Series** <br/> S800 1D Imager Barcode Scanner (anteriormente CHS 8Ci) <br/> S850 2D Imager Barcode Scanner (anteriormente CHS 8Qi)

## <a name="examples"></a>Ejemplos
Consulta el ejemplo de escáner de códigos de barras ver una implementación de ejemplo.
+    [Ejemplo de escáner de códigos de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)

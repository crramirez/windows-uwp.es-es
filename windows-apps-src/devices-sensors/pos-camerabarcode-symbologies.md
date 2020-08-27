---
title: Simbologías del escáner de códigos de barras de la cámara
description: Vea códigos de barras de ejemplo para cada una de las simbologías admitidas por el descodificador de código de barras del software que se incluye con Windows 10.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: a9618402a6ee76a20ff5f95418ee7280b39db4a2
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943135"
---
# <a name="symbologies"></a>Simbologías

En este tema se proporcionan códigos de barras de ejemplo para cada una de las simbologías admitidas por el descodificador de código de barras de software que se suministra con Windows 10, incluidos: UPC/EAN, código 39, código 128, intercalado 2 de 5, barra de códigos omnidireccional, barra de códigos apilada, código QR y GS1DWCode.

Windows 10 usa una cámara de lente estándar combinada con un descodificador de software para generar un escáner de código de barras. En este artículo se hace referencia a las simbologías admitidas por el descodificador de software. Es posible que los dispositivos de escáner de código de barras dedicados que tienen descodificadores de hardware integrados admitan simbologías adicionales, póngase en contacto con el fabricante del escáner de códigos de barras para obtener más información. Las simbologías enumeradas se admiten en todas las ediciones de la compilación 17134 o posterior de Windows 10, a menos que se especifique lo contrario.

Use [GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync) para determinar las simbologías específicas admitidas por un escáner de códigos de barras.

> [!NOTE]
> [*Digimarc Corporation*](https://www.digimarc.com/)proporciona el descodificador de software integrado en Windows 10.

## <a name="1d-symbologies"></a>Simbologías 1D

### <a name="code-39"></a>Código 39
![Código de barras de ejemplo: código 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Código 128
![Código de barras de ejemplo: código 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>Databar omnidireccional
![Código de barras de ejemplo-DataBar omnidireccional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Barra de la barra apilada
![Código de barras de ejemplo-barra de códigos apilada](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![Código de barras de ejemplo EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![Código de barras de ejemplo EAN-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Entrelazado 2 de 5
![Código de barras de ejemplo-entrelazado 2 de 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![Código de barras de ejemplo-UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![Código de barras de ejemplo UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>Simbologías 2D
### <a name="qr-code"></a>Código QR
![Código de barras de ejemplo: código QR](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>Marca de agua digital
### <a name="gs1-dwcode"></a>GS1-DWCode

Digitalice la imagen de un paquete a continuación con la aplicación de escáner de códigos de barras de la cámara para ver GS1DWCode en acción.  La imagen está codificada con UPCA 856107006854.  Visite http://www.digimarc.com para obtener más información sobre las funcionalidades de GS1DWCode.

![Código de barras de ejemplo: GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>Consulte también

### <a name="samples"></a>Ejemplos

- [Ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)

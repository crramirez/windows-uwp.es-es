---
title: Requisitos del sistema para el escáner de código de barras basado en cámara
description: Este artículo enumeran los requisitos para usar el escáner de código de barras basado en cámara desde una aplicación para UWP.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243269"
---
# <a name="camera-barcode-scanner-system-requirements"></a>Requisitos del sistema para el escáner de código de barras basado en cámara
A partir de la versión 1803 de Windows 10, se pueden leer códigos de barras a través del objetivo de una cámara estándar desde una aplicación Universal de Windows.

## <a name="supported-windows-editions"></a>Versiones compatibles de Windows
- Windows 10 Professional en modo S
- Windows 10 Professional
- Windows 10 Enterprise
- Windows 10 IOT Core


## <a name="webcam-requirements"></a>Requisitos de la cámara web
| Category      | Recomendación           | Comentarios |
| ------------- | ------------------------ | -------- |
| Foco         | Enfoque automático               | No se recomienda el enfoque fijo |
| Resolución    | 1920 x 1440 o superior    | Hemos tenido la mejor experiencia con cámaras compatibles con una resolución de 1920 x 1440 o superior.  Algunas cámaras de resolución más baja pueden leer códigos de barras estándar si la impresión es lo suficientemente grande. Los códigos de barras con elementos más estrechos pueden requerir cámaras de mayor resolución. |
|

## <a name="see-also"></a>Vea también

### <a name="samples"></a>Muestras

- [Ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)

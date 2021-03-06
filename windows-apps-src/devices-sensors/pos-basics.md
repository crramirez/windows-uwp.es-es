---
title: Aspectos básicos del punto de servicio
description: Este artículo contiene información sobre cómo empezar a usar las API de Windows Runtime PointOfService.
ms.date: 12/3/2019
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: cb8acf5f7dce8e81d72850a4dbd097083b240864
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730046"
---
# <a name="point-of-service-basics"></a>Aspectos básicos del punto de servicio

Esta sección contiene temas que son comunes en todas las categorías de dispositivos de servicio.

|Tema |Descripción |
|------|------------|
| [Declaración de funcionalidad](pos-basics-capability.md)      | Obtenga información sobre cómo agregar la funcionalidad de **pointOfService** al manifiesto de aplicación.  Esta funcionalidad es necesaria para el uso del espacio de nombres Windows. Devices. PointOfService.  |
| [Enumeración de dispositivos](pos-basics-enumerating.md)        | Obtenga información acerca de cómo definir un selector de dispositivos que se utiliza para consultar los dispositivos disponibles en el sistema y usar este selector para enumerar los dispositivos de punto de servicio.  |
| [Crear un objeto de dispositivo](pos-basics-deviceobject.md)  | Obtenga información sobre cómo crear un objeto de dispositivo PointOfService que le proporcione acceso a las propiedades de solo lectura del periférico y solicite el uso exclusivo del periférico. |
| [Notificar y habilitar](pos-basics-claim.md)  | Obtenga información sobre cómo reservar un periférico PointOfService para uso exclusivo y habilitar para operaciones de e/s.  |
| [Compartir dispositivos periféricos con otras personas](pos-basics-sharing.md) | Obtenga información acerca de cómo compartir periféricos conectados a la red o Bluetooth con otros equipos en un entorno en el que varios equipos se basan en periféricos compartidos en lugar de periféricos dedicados conectados a cada equipo.
| [De extremo a extremo de PointOfService](pos-get-started.md)  | Este es un ejemplo de un extremo a otro sobre cómo interactuar con periféricos de PointOfService mediante los ejemplos anteriores. |
|

## <a name="see-also"></a>Vea también

| Tema   | Descripción |
|:--------|:------------|
| [Distribución de aplicaciones](../publish/distribute-lob-apps-to-enterprises.md) | Obtenga información sobre las opciones para distribuir la aplicación a los clientes empresariales. |
| [Ciclo de vida de aplicación](../launch-resume/app-lifecycle.md) | Obtenga información sobre el ciclo de vida de una aplicación de UWP y lo que sucede cuando Windows se inicia, suspende y reanuda la aplicación. |
| [Recursos de aplicación](../app-resources/index.md) | Aprenda a crear, empaquetar y usar los recursos de cadena, imagen y archivo de la aplicación. |
| [Enlace de datos](../data-binding/index.md) | Obtenga información sobre cómo usar el enlace de datos para Mostrar datos en la interfaz de usuario de la aplicación. |
| [Enumeración de dispositivos](enumerate-devices.md) | Aprenda a usar técnicas de enumeración avanzadas para encontrar periféricos.|
| [Aplicaciones adaptables de versión](../debug-test-perf/version-adaptive-apps.md) | Lean cómo diseñar la aplicación para que se ejecute en varias versiones de Windows 10.|
|


## <a name="sample-code"></a>Código de ejemplo
+ [Ejemplo de escáner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de cajón de caja]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de presentación de líneas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de franjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

---
title: Aspectos básicos del punto de servicio
description: En este artículo se incluye información sobre la introducción a las API de UWP de PointOfService.
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4749b66cc5ce2593aead75d65993f70106da7c8b
ms.sourcegitcommit: 2d709ddcc31f52d2a4ace1134aea45057d99a615
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782581"
---
# <a name="point-of-service-basics"></a>Aspectos básicos del punto de servicio

Esta sección contiene temas que son comunes a todas las categorías de dispositivos de punto de servicio.

|Tema |Descripción |
|------|------------|
| [Declaración de funcionalidad](pos-basics-capability.md)      | Obtén información sobre cómo agregar la funcionalidad **pointOfService** al manifiesto de aplicación.  Esta funcionalidad es necesaria para usar el espacio de nombres Windows.Devices.PointOfService.  |
| [Enumerar dispositivos](pos-basics-enumerating.md)        | Aprende cómo definir un selector de dispositivos que se usa para consultar los dispositivos disponibles para el sistema y cómo usar este selector para enumerar los dispositivos de punto de servicio.  |
| [Creación de un objeto de dispositivo](pos-basics-deviceobject.md)  | Obtén información sobre cómo crear un objeto de dispositivo PointOfService que te proporcionará acceso a las propiedades de solo lectura del periférico y reclamará el periférico para uso exclusivo. |
| [Notificar y habilitar](pos-basics-claim.md)  | Obtenga información sobre cómo reservar un periférico PointOfService para uso exclusivo y habilitar para operaciones de e/s.  |
| [Uso compartido de periféricos con otros usuarios](pos-basics-sharing.md) | Obtenga información acerca de cómo compartir periféricos conectados a la red o Bluetooth con otros equipos en un entorno en el que varios equipos se basan en periféricos compartidos en lugar de periféricos dedicados conectados a cada equipo.
| [De extremo a extremo de PointOfService](pos-get-started.md)  | Este es un ejemplo de un extremo a otro sobre cómo interactuar con periféricos de PointOfService mediante los ejemplos anteriores. |
|

## <a name="see-also"></a>Consulta también

| Tema   | Descripción |
|:--------|:------------|
| [Distribución de aplicaciones](../publish/distribute-lob-apps-to-enterprises.md) | Obtenga información sobre las opciones para distribuir la aplicación a los clientes empresariales. |
| [Ciclo de vida de la aplicación](../launch-resume/app-lifecycle.md) | Obtenga información sobre el ciclo de vida de una aplicación de UWP y lo que sucede cuando Windows se inicia, suspende y reanuda la aplicación. |
| [Recursos de la aplicación](../app-resources/index.md) | Aprenda a crear, empaquetar y usar los recursos de cadena, imagen y archivo de la aplicación. |
| [Enlace de datos](../data-binding/index.md) | Obtenga información sobre cómo usar el enlace de datos para Mostrar datos en la interfaz de usuario de la aplicación. |
| [Enumeración de dispositivos](enumerate-devices.md) | Aprenda a usar técnicas de enumeración avanzadas para encontrar periféricos.|
| [Aplicaciones adaptables de versión](../debug-test-perf/version-adaptive-apps.md) | Lean cómo diseñar la aplicación para que se ejecute en varias versiones de Windows 10.|
|


## <a name="sample-code"></a>Código de muestra
+ [Ejemplo de escáner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de cajón de caja]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de presentación de líneas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de franjas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

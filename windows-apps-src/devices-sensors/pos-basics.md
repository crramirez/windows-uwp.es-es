---
title: Tareas iniciales con punto de servicio
description: En este artículo se incluye información sobre la introducción a las API de UWP de PointOfService.
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 1b4ff9443c40cf44e171bf898b627de3e2819034
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656470"
---
# <a name="getting-started-with-point-of-service"></a>Tareas iniciales con punto de servicio

## <a name="pointofservice-basics"></a>Conceptos básicos de PointOfService

Esta sección contiene temas que son comunes a todas las categorías de dispositivos de punto de servicio.

|Tema |Descripción |
|------|------------|
| [Declaración de capacidad](pos-basics-capability.md)      | Obtén información sobre cómo agregar la funcionalidad **pointOfService** al manifiesto de aplicación.  Esta funcionalidad es necesaria para usar el espacio de nombres Windows.Devices.PointOfService.  |
| [Enumerar dispositivos](pos-basics-enumerating.md)        | Aprende cómo definir un selector de dispositivos que se usa para consultar los dispositivos disponibles para el sistema y cómo usar este selector para enumerar los dispositivos de punto de servicio.  |
| [Creación de un objeto de dispositivo](pos-basics-deviceobject.md)  | Obtén información sobre cómo crear un objeto de dispositivo PointOfService que te proporcionará acceso a las propiedades de solo lectura del periférico y reclamará el periférico para uso exclusivo. |
| [Notificación y habilitar ](pos-basics-claim.md)  | Obtenga información sobre cómo reservar un PointOfService periférico para uso exclusivo y habilitar para las operaciones de E/S.  |
| [Periféricos de uso compartido con otros usuarios](pos-basics-sharing.md) | Obtenga información sobre cómo compartir periféricos Bluetooth conectado o red con otros equipos en un entorno donde varios PC dependen de periféricos compartidos en lugar de dedicado periférico adjunto a cada equipo.
| [To-end PointOfService](pos-get-started.md)  | Se trata de un ejemplo completo de cómo interactuar con los periféricos PointOfService utilizando los ejemplos anteriores. |
|

## <a name="see-also"></a>Consulte también

| Tema   | Descripción |
|:--------|:------------|
| [Distribución de la aplicación](../publish/distribute-lob-apps-to-enterprises.md) | Obtenga información sobre las opciones para distribuir la aplicación a los clientes empresariales. |
| [Ciclo de vida de aplicación](../launch-resume/app-lifecycle.md) | Obtenga información sobre el ciclo de vida de una aplicación de UWP y lo que sucede cuando Windows se inician, suspenden y reanuda la aplicación. |
| [Recursos de la aplicación](../app-resources/index.md) | Aprenda a crear, paquete y usar la cadena de la aplicación, imagen y los recursos de archivos. |
| [Enlace de datos](../data-binding/index.md) | Obtenga información sobre cómo usar el enlace de datos para mostrar los datos en la interfaz de usuario de la aplicación. |
| [Enumeración de dispositivos](enumerate-devices.md) | Obtenga información sobre técnicas de enumeración de uso avanzada para encontrar los periféricos.|
| [Aplicaciones adaptables de versión](../debug-test-perf/version-adaptive-apps.md) | Obtenga información sobre cómo diseñar la aplicación para que se ejecute en varias versiones de Windows 10.|
|


## <a name="sample-code"></a>Código de ejemplo
+ [Ejemplo de escáner de código de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de cajón de caja]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de línea para mostrar](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de banda magnética](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)


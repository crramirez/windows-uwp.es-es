---
title: Tareas iniciales con punto de servicio
description: En este artículo se incluye información sobre la introducción a las API de UWP de PointOfService.
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 1b4ff9443c40cf44e171bf898b627de3e2819034
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7845006"
---
# <a name="getting-started-with-point-of-service"></a>Tareas iniciales con punto de servicio

## <a name="pointofservice-basics"></a>Conceptos básicos de PointOfService

Esta sección contiene temas que son comunes a todas las categorías de dispositivos de punto de servicio.

|Tema |Descripción |
|------|------------|
| [Declaración de funcionalidad](pos-basics-capability.md)      | Obtén información sobre cómo agregar la funcionalidad **pointOfService** al manifiesto de aplicación.  Esta funcionalidad es necesaria para usar el espacio de nombres Windows.Devices.PointOfService.  |
| [Enumeración de dispositivos](pos-basics-enumerating.md)        | Aprende cómo definir un selector de dispositivos que se usa para consultar los dispositivos disponibles para el sistema y cómo usar este selector para enumerar los dispositivos de punto de servicio.  |
| [Crear un objeto de dispositivo](pos-basics-deviceobject.md)  | Obtén información sobre cómo crear un objeto de dispositivo PointOfService que te proporcionará acceso a las propiedades de solo lectura del periférico y reclamará el periférico para uso exclusivo. |
| [Habilitar y notificación ](pos-basics-claim.md)  | Aprende a reservar un periférico PointOfService para uso exclusivo y habilitar para operaciones de E/S.  |
| [Compartir dispositivos periféricos con otras personas](pos-basics-sharing.md) | Obtén información sobre cómo compartir la red o periféricos Bluetooth conectado con otros equipos en un entorno donde varios equipos dependen de periféricos compartidos en lugar de dedicada periféricos conectados a cada equipo.
| [Principio a fin de PointOfService](pos-get-started.md)  | Este es un ejemplo de principio a fin de cómo interactuar con dispositivos periféricos de PointOfService para uso de los ejemplos anteriores. |
|

## <a name="see-also"></a>Ver también

| Tema   | Descripción |
|:--------|:------------|
| [Distribución de la aplicación](../publish/distribute-lob-apps-to-enterprises.md) | Obtén información sobre las opciones para distribuir la aplicación a los clientes de empresa. |
| [Ciclo de vida de aplicación](../launch-resume/app-lifecycle.md) | Obtén información sobre el ciclo de vida de una aplicación para UWP y lo que sucede cuando Windows inicia, suspende y reanuda la aplicación. |
| [Recursos de la aplicación](../app-resources/index.md) | Obtén información sobre cómo crear, empaquetar y consumir los recursos de archivo, imagen y cadena de la aplicación. |
| [Enlace de datos](../data-binding/index.md) | Aprende a usar el enlace de datos para mostrar los datos en la interfaz de usuario de la aplicación. |
| [Enumeración de dispositivos](enumerate-devices.md) | Obtén información sobre técnicas de enumeración de uso avanzado para buscar los periféricos.|
| [Aplicaciones adaptables de versión](../debug-test-perf/version-adaptive-apps.md) | Obtenga información sobre cómo diseñar la aplicación para que se ejecuta en varias versiones de Windows 10.|
|


## <a name="sample-code"></a>Código de muestra
+ [Ejemplo de escáner de códigos de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de caja registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de visualización de líneas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de bandas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de impresora de punto de servicio](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)


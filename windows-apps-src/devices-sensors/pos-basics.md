---
author: TerryWarwick
title: Tareas iniciales con punto de servicio
description: En este artículo se incluye información sobre la introducción a las API de UWP de PointOfService.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: a0583adbcef9e45dfe0b2e56e03ce7e0451ac5bb
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983548"
---
# <a name="getting-started-with-point-of-service"></a>Tareas iniciales con punto de servicio

Esta sección contiene temas que son comunes a todas las categorías de dispositivos de punto de servicio.

|Tema |Descripción |
|------|------------|
| [Declaración de funcionalidad](pos-basics-capability.md)      | Obtén información sobre cómo agregar la funcionalidad **pointOfService** al manifiesto de aplicación.  Esta funcionalidad es necesaria para usar el espacio de nombres Windows.Devices.PointOfService.  |
| [Enumeración de dispositivos](pos-basics-enumerating.md)        | Aprende cómo definir un selector de dispositivos que se usa para consultar los dispositivos disponibles para el sistema y cómo usar este selector para enumerar los dispositivos de punto de servicio.  |
| [Crear un objeto de dispositivo](pos-basics-deviceobject.md)  | Obtén información sobre cómo crear un objeto de dispositivo PointOfService que te proporcionará acceso a las propiedades de solo lectura del periférico y reclamará el periférico para uso exclusivo. |
| [Reclamar un dispositivo para uso exclusivo ](pos-basics-claim.md)  | Aprende a reservar un periférico PointOfService para uso exclusivo con el modelo de notificación PointOfService mientras se permite que otras aplicaciones del mismo equipo tengan acceso al periférico PointOfService cuando necesiten uso exclusivo.  |
|

## <a name="see-also"></a>Ver también
[Introducción a Windows.Devices.PointOfService](pos-get-started.md)


## <a name="sample-code"></a>Código de muestra
+ [Ejemplo de escáner de códigos de barras](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Ejemplo de caja registradora]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Ejemplo de visualización de líneas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Ejemplo de lector de bandas magnéticas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Ejemplo de impresora de punto de servicio](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)


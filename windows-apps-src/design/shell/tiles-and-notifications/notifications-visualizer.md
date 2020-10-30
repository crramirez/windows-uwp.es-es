---
description: Notifications Visualizer es una nueva aplicación de Windows que se puede encontrar en Store y que ayuda a los desarrolladores a diseñar iconos dinámicos adaptables para Windows 10.
title: Visualizador de notificaciones
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55ab7e22dc011555a8d98fd863b7dd9ff67de5fa
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033678"
---
# <a name="notifications-visualizer"></a>Visualizador de notificaciones

 


El visualizador de notificaciones es una nueva aplicación de Windows del [almacén](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ayuda a los desarrolladores a diseñar iconos dinámicos adaptables y notificaciones del sistema interactivas para Windows 10.


## <a name="overview"></a>Introducción

El visualizador de notificaciones proporciona vistas previas visuales instantáneas de la notificación del icono y del sistema a medida que edita la carga XML, de forma similar a la vista de diseño o editor XAML de Visual Studio. La aplicación también comprueba los errores, lo que garantiza que se crea una carga válida de la notificación del sistema o del icono.

Esta captura de pantalla de la aplicación muestra la carga de XML y cómo aparecen los tamaños de icono en un dispositivo determinado:

![captura de pantalla del editor de la aplicación Notifications Visualizer con código e iconos](images/notif-visualizer-001.png)

 

Con el visualizador de notificaciones, puede crear y probar cargas adaptables de iconos y notificaciones del sistema sin tener que editar e implementar su propia aplicación. Una vez que haya creado una carga con los resultados visuales ideales, puede integrarlo en la aplicación. Consulte [envío de una notificación de icono local](sending-a-local-tile-notification.md) y [envío de un sistema local](send-local-toast.md) para obtener más información.

**Nota:**   La simulación del visualizador de notificaciones del menú Inicio de Windows y las notificaciones del sistema no siempre es totalmente precisa y no admite algunas propiedades avanzadas de carga útil. Cuando tenga el icono o la notificación del sistema que quiera, pruébelo anclando el icono o sacando la notificación del sistema para comprobar que aparece según lo previsto.

 

## <a name="features"></a>Características

El visualizador de notificaciones incluye una serie de cargas de ejemplo para mostrar lo que es posible con los iconos dinámicos adaptables y los notificantes interactivos para ayudarle a empezar. Puedes experimentar con todas las opciones de texto, grupos/subgrupos, imágenes de fondo diferentes y ver cómo el icono se adapta a distintas pantallas y dispositivos. Una vez que hayas realizado cambios, puedes guardar la carga actualizada en un archivo para el uso futuro.

El editor proporciona advertencias y errores en tiempo real. Por ejemplo, si la carga es superior a 5 KB (una limitación de plataforma), el visualizador de notificaciones le advierte de que la carga supera ese límite. Te proporciona advertencias para los nombres de atributos o valores incorrectos, lo cual te ayuda a depurar problemas visuales.

Puede controlar las propiedades del icono como el nombre para mostrar, el color, los logotipos, ShowName y el valor del distintivo. Estas opciones te ayudan a comprender al instante cómo interactúan tus cargas de notificación de iconos y propiedades de los iconos, así como los resultados que producen.

Esta captura de pantalla de la aplicación muestra el editor de iconos:

![captura de pantalla del editor de Notifications Visualizer con iconos](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Temas relacionados

* [Obtener Notifications Visualizer en la Tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Crear iconos adaptables](create-adaptive-tiles.md)
* [Notificaciones interactivas](adaptive-interactive-toasts.md)

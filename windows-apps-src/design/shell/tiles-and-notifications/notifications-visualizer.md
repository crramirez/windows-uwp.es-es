---
author: andrewleader
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 59a2e93f7c09266b0e33d58bf6a2571e60c7a607
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6036665"
---
# <a name="notifications-visualizer"></a>Notifications Visualizer

 


Notifications Visualizer es una nueva aplicación de plataforma Universal de Windows (UWP) [en la tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ayuda a los desarrolladores diseñar iconos dinámicos adaptables y notificaciones del sistema interactivas para Windows 10.


## <a name="overview"></a>Introducción

Notifications Visualizer proporciona vistas previas instantáneas de tu icono y notificación del sistema a medida que editas la carga XML, de forma similar a la vista de editor/diseño de XAML de Visual Studio. La aplicación también comprueba si hay errores, lo cual garantiza que crees una carga de icono o notificación del sistema válida.

Esta captura de pantalla de la aplicación muestra la carga de XML y cómo aparecen los tamaños de icono en un dispositivo determinado:

![captura de pantalla del editor de la aplicación Notifications Visualizer con código e iconos](images/notif-visualizer-001.png)

 

Con Notifications Visualizer, puedes crear y probar cargas de notificaciones del sistema e iconos adaptables sin tener que editar e implementar su propia aplicación. Una vez creada una carga con resultados visuales ideales, puedes integrarla en tu aplicación. Consulta [Enviar una notificación de icono local](sending-a-local-tile-notification.md) y [Enviar una notificación del sistema local](send-local-toast.md) para obtener más información.

**Nota**  la simulación de Notifications Visualizer de las notificaciones del sistema y de menú Inicio de Windows no siempre es totalmente precisa y no es compatible con algunas propiedades de carga como avanzadas. Cuando tengas el icono o la notificación del sistema que desees, pruébalo anclando el icono o haciendo que aparezca la notificación del sistema para comprobar que aparece como deseas.

 

## <a name="features"></a>Características

Notifications Visualizer viene con varias cargas de muestra para mostrar lo que es posible con los iconos dinámicos adaptables y notificaciones del sistema interactivas para ayudarte a empezar. Puedes experimentar con todas las opciones de texto, grupos/subgrupos, imágenes de fondo diferentes y ver cómo el icono se adapta a distintas pantallas y dispositivos. Una vez que hayas realizado cambios, puedes guardar la carga actualizada en un archivo para el uso futuro.

El editor proporciona advertencias y errores en tiempo real. Por ejemplo, si la carga tiene un tamaño superior a 5KB (una limitación de plataforma), Notifications Visualizer te avisa de que la carga supera ese límite. Te proporciona advertencias para los nombres de atributos o valores incorrectos, lo cual te ayuda a depurar problemas visuales.

Puedes controlar las propiedades de los iconos como el nombre para mostrar, el color, los logotipos, ShowName y el valor del distintivo. Estas opciones te ayudan a comprender al instante cómo interactúan tus cargas de notificación de iconos y propiedades de los iconos, así como los resultados que producen.

Esta captura de pantalla de la aplicación muestra el editor de iconos:

![captura de pantalla del editor de Notifications Visualizer con iconos](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Temas relacionados

* [Obtener Notifications Visualizer en la Tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Crear iconos adaptables](create-adaptive-tiles.md)
* [Notificaciones del sistema interactivas](adaptive-interactive-toasts.md)
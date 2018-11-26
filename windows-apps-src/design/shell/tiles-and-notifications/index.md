---
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: Iconos, distintivos y notificaciones
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ea005dd33bbb5461921fa17eded8430d7648c87
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7718970"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>Iconos, distintivos y notificaciones para las aplicaciones para UWP
 

Aprende a usar iconos, distintivos, notificaciones del sistema y notificaciones para proporcionar puntos de entrada en la aplicación y mantener actualizados a los usuarios.

> **API importantes**: [paquete de NuGet del kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Un icono es una representación de la aplicación en el menú Inicio. Todas las aplicaciones para UWP tienen un icono. Asimismo, puedes habilitar distintos tamaños de icono (pequeño, mediano, grande y ancho).</p>

<p>Puedes usar una <em>notificación de icono</em> para actualizar el icono con el fin de comunicar información nueva al usuario, como titulares de noticias o el asunto del mensaje sin leer más reciente.</p>

<p>Puedes usar un <em>distintivo</em> en forma de glifo proporcionado por el sistema o un número de 1 a 99, para proporcionar información breve acerca del estado. Recuerda que los distintivos también pueden aparecer en el icono de la barra de tareas de una aplicación. </p>

<p>Una <em>notificación del sistema</em> es una notificación que la aplicación envía al usuario a través de un elemento emergente de la interfaz de usuario denominado <em>notificación del sistema</em> (o <em>mensaje emergente</em>). La notificación se puede ver esté el usuario en la aplicación o no.</p>
<p>Una <em>notificación de inserción</em> o <em>notificación sin procesar</em> es una notificación enviada a tu aplicación desde Servicios de notificaciones de inserción de Windows (WNS) o desde una tarea en segundo plano. Tu aplicación puede responder a estas notificaciones notificándole al usuario de que ha sucedido algo interesante (mediante una actualización de distintivo, una actualización de icono o una notificación del sistema) o puede responder de la manera que elijas.</p>

 
## <a name="tiles"></a>Iconos
| Artículo | Descripción |
| --- | --- |
| [Crear iconos](creating-tiles.md) | Personaliza el icono predeterminado de la aplicación y proporciona activos para diferentes tamaños de pantalla. |
| [Activos de icono de la aplicación](app-assets.md) | Los activos de icono de la aplicación, que aparecen en una amplia variedad de formas en todo el sistema operativo Windows 10, son las tarjetas de llamada de la aplicación para la Plataforma universal de Windows (UWP). Estas directrices detallan el lugar donde aparecen los recursos de icono de la aplicación en el sistema y proporcionan sugerencias de diseño detalladas sobre cómo crear los iconos más sofisticados. |
| [API de icono principal](primary-tile-apis.md) | Solicita anclar el icono principal de la aplicación y comprueba si el icono principal está anclado actualmente. |
| [Contenido de icono](create-adaptive-tiles.md) | Contenido de la notificación de icono se especifica con adaptable, una nueva característica de Windows 10, que te permite diseñar tu propio contenido con un lenguaje de marcado sencillo y flexible que se adapte a diferentes densidades de pantalla de la notificación de icono. En este artículo se explica cómo crear iconos dinámicos adaptables para la aplicación Plataforma universal de Windows (UWP). |
| [Esquema de contenido de icono](../tiles-and-notifications/tile-schema.md) | Estos son los elementos y atributos que usas para crear iconos adaptables. |
| [Plantillas de iconos especiales](special-tile-templates-catalog.md) | Las plantillas de iconos especiales son plantillas únicas que, o bien están animadas, o bien simplemente te permiten hacer cosas que no son posibles con los iconos adaptables. |
| [Enviar notificación de icono local](sending-a-local-tile-notification.md) | Aprende a enviar una notificación de icono local, agregando contenido dinámico enriquecido a tu icono dinámico. |


## <a name="notifications"></a>Notificaciones

| Artículo | Descripción |
| --- | --- |
| [Notificaciones del sistema](adaptive-interactive-toasts.md) | Las notificaciones del sistema adaptables e interactivas permiten crear notificaciones emergentes flexibles con más contenido, imágenes en línea opcionales e interacción del usuario opcional. |
| [Enviar una notificación de icono local](send-local-toast.md) | Aprende a enviar una notificación del sistema interactiva. |
| [Visualizador de notificaciones](notifications-visualizer.md) | Notifications que Visualizer es una nueva aplicación de plataforma Universal de Windows (UWP) de [la tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ayuda a los desarrolladores a diseñar adaptable iconos dinámicos para Windows 10. |
| [Elegir un método de entrega de notificaciones](choosing-a-notification-delivery-method.md) | En este artículo se abordan las cuatro opciones de notificación (local, programada, periódica y de inserción) que proporcionan actualizaciones de iconos y distintivos, así como contenido de notificaciones del sistema. |
| [Introducción a las notificaciones periódicas](periodic-notification-overview.md) | Las notificaciones periódicas, también denominadas notificaciones de sondeo, actualizan los iconos y los distintivos a intervalos fijos mediante la descarga de contenido desde un servicio de nube. |
| [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md) | Con los Servicios de notificaciones de inserción de Windows (WNS), los desarrolladores de terceros pueden enviar actualizaciones de notificaciones del sistema, de icono, de distintivo y sin procesar desde su propio servicio de nube. Esto proporciona un mecanismo para enviar nuevas actualizaciones a los usuarios de una manera segura y de bajo consumo. |
| [Código generado por el Asistente para notificaciones de inserción](the-code-generated-by-the-push-notification-wizard.md) | Usar un asistente en Visual Studio te permite generar notificaciones de inserción desde un servicio móvil creado con los Servicios móviles de Azure. El Asistente de Visual Studio genera código para ayudarte a empezar. En este tema se explica cómo el asistente modifica el proyecto, qué hace el código generado, cómo se usa este código y qué puedes hacer después para sacarle todo el partido a las notificaciones de inserción. Consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md). |
| [Introducción a las notificaciones sin procesar](raw-notification-overview.md) | Las notificaciones sin procesar son breves notificaciones de inserción de carácter general. Guardan un propósito estrictamente instructivo y no incluyen ningún componente de interfaz de usuario. Al igual que sucede con el resto de los tipos de notificaciones de inserción, la característica WNS entrega notificaciones sin procesar desde el servicio de nube a tu aplicación. |

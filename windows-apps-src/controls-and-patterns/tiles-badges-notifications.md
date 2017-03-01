---
author: mijacobs
Description: "Aprende a usar iconos, distintivos, notificaciones del sistema y notificaciones para proporcionar puntos de entrada en la aplicación y mantener actualizados a los usuarios."
title: Iconos, distintivos y notificaciones
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f8b063f45afadda50fa9ea091bf6cba71a25e8c1
ms.lasthandoff: 02/07/2017

---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>Iconos, distintivos y notificaciones para las aplicaciones para UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Aprende a usar iconos, distintivos, notificaciones del sistema y notificaciones para proporcionar puntos de entrada en la aplicación y mantener actualizados a los usuarios.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Un icono es una representación de la aplicación en el menú Inicio. Todas las aplicaciones para UWP tienen un icono. Asimismo, puedes habilitar distintos tamaños de icono (pequeño, mediano, grande y ancho).</p>

<p>Puedes usar una <em>notificación de icono</em> para actualizar el icono con el fin de comunicar información nueva al usuario, como titulares de noticias o el asunto del mensaje sin leer más reciente.</p>

<p>Puedes usar un <em>distintivo</em> en forma de glifo proporcionado por el sistema o un número de 1 a 99, para proporcionar información breve acerca del estado. Recuerda que los distintivos también pueden aparecer en el icono de la barra de tareas de una aplicación. </p>

<p>Una <em>notificación del sistema</em> es una notificación que la aplicación envía al usuario a través de un elemento emergente de la interfaz de usuario denominado <em>notificación del sistema</em> (o <em>mensaje emergente</em>). La notificación se puede ver esté el usuario en la aplicación o no.</p>
<p>Una <em>notificación de inserción</em> o <em>notificación sin procesar</em> es una notificación enviada a tu aplicación desde Servicios de notificaciones de inserción de Windows (WNS) o desde una tarea en segundo plano. Tu aplicación puede responder a estas notificaciones notificándole al usuario de que ha sucedido algo interesante (mediante una actualización de distintivo, una actualización de icono o una notificación del sistema) o puede responder de la manera que elijas.</p>

 
## <a name="tiles"></a>Iconos 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Crear iconos](tiles-and-notifications-creating-tiles.md)</p></td>
<td align="left"><p>Personaliza el icono predeterminado de la aplicación y proporciona activos para diferentes tamaños de pantalla.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md)</p></td>
<td align="left"><p>Las plantillas de iconos adaptables son una nueva característica de Windows 10, que te permite diseñar tu propio contenido de notificación de icono con un lenguaje de marcado sencillo y flexible que se adapta a diferentes densidades de pantalla. En este artículo se explica cómo crear iconos dinámicos adaptables para la aplicación Plataforma universal de Windows (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Esquema de iconos adaptables](tiles-and-notifications-adaptive-tiles-schema.md)</p></td>
<td align="left"><p>Estos son los elementos y atributos que usas para crear iconos adaptables.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Plantillas de iconos especiales](tiles-and-notifications-special-tile-templates-catalog.md)</p></td>
<td align="left"><p>Las plantillas de iconos especiales son plantillas únicas que, o bien están animadas, o bien simplemente te permiten hacer cosas que no son posibles con los iconos adaptables.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Activos de icono de la aplicación](tiles-and-notifications-app-assets.md)</p></td>
<td align="left"><p>Los activos de icono de la aplicación, que aparecen en una amplia variedad de formas en todo el sistema operativo Windows 10, son las tarjetas de llamada de la aplicación para la Plataforma universal de Windows (UWP). Estas directrices detallan el lugar donde aparecen los recursos de icono de la aplicación en el sistema y proporcionan sugerencias de diseño detalladas sobre cómo crear los iconos más sofisticados.</p></td>
</tr>
</tbody>
</table>

## <a name="notifications"></a>Notificaciones


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Notificaciones del sistema interactivas y adaptables](tiles-and-notifications-adaptive-interactive-toasts.md)</p></td>
<td align="left"><p>Las notificaciones del sistema adaptables e interactivas permiten crear notificaciones emergentes flexibles con más contenido, imágenes en línea opcionales e interacción del usuario opcional.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Notifications Visualizer](tiles-and-notifications-notifications-visualizer.md)</p></td>
<td align="left"><p>Notifications Visualizer es una nueva aplicación para la Plataforma universal de Windows (UWP) en [la Tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ayuda a los desarrolladores a diseñar iconos dinámicos adaptables para Windows 10.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Elegir un método de entrega de notificaciones](tiles-and-notifications-choosing-a-notification-delivery-method.md)</p></td>
<td align="left"><p>En este artículo se abordan las cuatro opciones de notificación (local, programada, periódica y de inserción) que proporcionan actualizaciones de iconos y distintivos, así como contenido de notificaciones del sistema.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Enviar una notificación de icono local](tiles-and-notifications-sending-a-local-tile-notification.md)</p></td>
<td align="left"><p>En este artículo se describe cómo enviar una notificación de icono local a un icono principal y un icono secundario con el uso de plantillas de iconos adaptables.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Introducción a las notificaciones periódicas](tiles-and-notifications-periodic-notification-overview.md)</p></td>
<td align="left"><p>Las notificaciones periódicas, también denominadas notificaciones de sondeo, actualizan los iconos y los distintivos a intervalos fijos mediante la descarga de contenido desde un servicio de nube.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</p></td>
<td align="left"><p>Con los Servicios de notificaciones de inserción de Windows (WNS), los desarrolladores de terceros pueden enviar actualizaciones de notificaciones del sistema, de icono, de distintivo y sin procesar desde su propio servicio de nube. Esto proporciona un mecanismo para enviar nuevas actualizaciones a los usuarios de una manera segura y de bajo consumo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Código generado por el Asistente para notificaciones de inserción](tiles-and-notifications-the-code-generated-by-the-push-notification-wizard.md)</p></td>
<td align="left"><p>Usar un asistente en Visual Studio te permite generar notificaciones de inserción desde un servicio móvil creado con los Servicios móviles de Azure. El Asistente de Visual Studio genera código para ayudarte a empezar. En este tema se explica cómo el asistente modifica el proyecto, qué hace el código generado, cómo se usa este código y qué puedes hacer después para sacarle todo el partido a las notificaciones de inserción. Consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Introducción a las notificaciones sin procesar](tiles-and-notifications-raw-notification-overview.md)</p></td>
<td align="left"><p>Las notificaciones sin procesar son breves notificaciones de inserción de carácter general. Guardan un propósito estrictamente instructivo y no incluyen ningún componente de interfaz de usuario. Al igual que sucede con el resto de los tipos de notificaciones de inserción, la característica WNS entrega notificaciones sin procesar desde el servicio de nube a tu aplicación.</p></td>
</tr>
</tbody>
</table>

 

 

 






---
Description: En este artículo se abordan las cuatro opciones de notificación (local, programada, periódica y de inserción) que proporcionan actualizaciones de iconos y distintivos, así como contenido de notificaciones del sistema.
title: Elegir un método de entrega de notificaciones
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: Elegir un método de entrega de notificaciones
template: detail.hbs
---

# Elegir un método de entrega de notificaciones


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este artículo se abordan las cuatro opciones de notificación (local, programada, periódica y de inserción) que proporcionan actualizaciones de iconos y distintivos, así como contenido de notificaciones del sistema. Un icono o una notificación del sistema pueden proporcionar información a tu usuario, incluso mientras el usuario no esté interactuando de forma directa con la aplicación. La naturaleza y el contenido de la aplicación y la información que quieras transmitir pueden ayudar a determinar qué métodos de notificación resultan más adecuados para tu escenario.

## <span id="Notification_delivery_methods__overview"></span><span id="notification_delivery_methods__overview"></span><span id="NOTIFICATION_DELIVERY_METHODS__OVERVIEW"></span>Introducción a los métodos de entrega de notificaciones


Existen cuatro mecanismos que puede usar una aplicación para entregar una notificación:

-   **Local**
-   **Programado**
-   **Periódico**
-   **De inserción**

Esta tabla resume los tipos de entrega de notificaciones.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método de entrega</th>
<th align="left">Uso con</th>
<th align="left">Descripción</th>
<th align="left">Ejemplos</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Local</td>
<td align="left">Icono, distintivo, notificación del sistema</td>
<td align="left">Un conjunto de llamadas a API que envía notificaciones mientras se ejecuta tu aplicación y actualiza directamente el icono o el distintivo, o envía una notificación del sistema.</td>
<td align="left"><ul>
<li>Una aplicación de música actualiza su icono para mostrar lo que se encuentra en &quot;Reproducción en curso&quot;.</li>
<li>Una aplicación de juego actualiza su icono con la puntuación más alta del usuario cuando el usuario abandona el juego.</li>
<li>Un distintivo cuyo glifo indica que hay nueva información en la aplicación se borra cuando se activa la aplicación.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Programado</td>
<td align="left">Icono, notificación del sistema</td>
<td align="left">Un conjunto de llamadas a API que programan una notificación anticipadamente para que se actualice en el momento que especifiques.</td>
<td align="left"><ul>
<li>Una aplicación de calendario establece un aviso de notificación del sistema para una reunión futura.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Periódico</td>
<td align="left">Icono, distintivo</td>
<td align="left">Notificaciones que actualizan regularmente los iconos y los distintivos a intervalos fijos mediante el sondeo del servicio de nube para detectar el contenido nuevo.</td>
<td align="left"><ul>
<li>Una aplicación de pronóstico del tiempo actualiza su icono cada 30 minutos para indicar el pronóstico del tiempo.</li>
<li>Un sitio de &quot;ofertas diarias&quot; actualiza la oferta del día todas las mañanas.</li>
<li>Un icono que muestra los días que faltan para un evento actualiza la cuenta atrás mostrada cada día a medianoche.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">De inserción</td>
<td align="left">Icono, distintivo, notificación del sistema, sin procesar</td>
<td align="left">Las notificaciones se envían desde un servidor de nube aunque la aplicación no se esté ejecutando.</td>
<td align="left"><ul>
<li>Una aplicación de compras envía una notificación del sistema para informar al usuario acerca una oferta en un artículo que está viendo.</li>
<li>Una aplicación de noticias actualiza su icono con noticias de último momento en cuanto se producen.</li>
<li>Una aplicación de deportes mantiene actualizado su icono durante el transcurso de un partido.</li>
<li>Una aplicación de comunicaciones proporciona alertas acerca de los mensajes o las llamadas telefónicas entrantes.</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Local_notifications"></span><span id="local_notifications"></span><span id="LOCAL_NOTIFICATIONS"></span>Notificaciones locales


La actualización del distintivo o del icono de la aplicación o la generación de una notificación del sistema mientras se está ejecutando la aplicación es el mecanismo más simple de entrega de notificaciones; solo requiere llamadas a API locales. Todas las aplicaciones pueden tener información útil o interesante para mostrar en el icono, incluso si el contenido cambia únicamente después de que el usuario inicie la aplicación e interactúe con esta. Las notificaciones locales también son un buen modo de mantener actualizado el icono de una aplicación, aunque también uses uno de los otros mecanismos de notificación. Por ejemplo, un icono de una aplicación de fotos puede mostrar las fotos de un álbum agregado recientemente.

Te recomendamos que tu aplicación actualice su icono de manera local en el primer inicio, o al menos inmediatamente después de que el usuario realice un cambio que tu aplicación normalmente reflejaría en el icono. La actualización no se verá hasta que el usuario salga de la aplicación, pero al hacer este cambio mientras la aplicación está siendo usada se garantiza que el icono ya está actualizado cuando el usuario sale de la aplicación.

Aunque las llamadas a las API son locales, las notificaciones pueden hacer referencia a imágenes web. Si la imagen web no está disponible para la descarga, está dañada o no cumple las especificaciones de imagen, los iconos y las notificaciones del sistema responden de modo distinto:

-   Iconos: la actualización no se muestra
-   Notificación del sistema: la notificación se muestra, pero con una imagen de marcador de posición

Aunque las notificaciones locales no expiran, se recomienda establecer una fecha de caducidad explícita.

Para obtener más información, consulta estos temas:

-   [Enviar una notificación de icono local](tiles-and-notifications-sending-a-local-tile-notification.md)
-   [Muestras de código de notificaciones de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Scheduled_notifications"></span><span id="scheduled_notifications"></span><span id="SCHEDULED_NOTIFICATIONS"></span>Notificaciones programadas


Las notificaciones programadas son el subconjunto de las notificaciones locales que pueden especificar el momento preciso en que se debe actualizar un icono o en que se debe mostrar una notificación del sistema. Las notificaciones programadas son ideales para las situaciones en las que el contenido que se debe actualizar se conoce con anticipación; por ejemplo, una invitación a una reunión. Si no conoces con anticipación el contenido de la notificación, debes usar una notificación periódica o de inserción.

De forma predeterminada, las notificaciones programadas expiran tres días después del momento en que se entregan. Si fuera necesario, puedes invalidar este valor predeterminado con un tiempo de caducidad explícito.

Para obtener más información, consulta estos temas:

-   [Directrices para notificaciones programadas](https://msdn.microsoft.com/library/windows/apps/hh761464)
-   [Muestras de código de notificaciones de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Periodic_notifications"></span><span id="periodic_notifications"></span><span id="PERIODIC_NOTIFICATIONS"></span>Notificaciones periódicas


Las notificaciones periódicas te proporcionan actualizaciones activas de icono con niveles mínimos de inversión de clientes y servicio de nube. También son un excelente método para distribuir el mismo contenido a una audiencia amplia. Tu código de cliente especifica la dirección URL de una ubicación de nube que Windows sondea en busca de actualizaciones de icono o de distintivo y, del mismo modo, especifica la frecuencia con la que se debe sondear la ubicación. En cada intervalo de sondeo, Windows se pone en contacto con la dirección URL para descargar el contenido XML especificado y mostrarlo en el icono.

Las notificaciones periódicas requieren que la aplicación hospede un servicio de nube, y este servicio se someterá a sondeo en los intervalos especificados por todos los usuarios que tienen instalada la aplicación. Ten en cuenta que las actualizaciones periódicas no se pueden usar para las notificaciones del sistema; las notificaciones del sistema se ejecutan mejor mediante notificaciones programadas o de inserción.

De forma predeterminada, las notificaciones periódicas expiran tres días después del momento en que se produce el sondeo. Si fuera necesario, puedes invalidar este valor predeterminado con un tiempo de caducidad explícito.

Para obtener más información, consulta estos temas:

-   [Introducción a las notificaciones periódicas](tiles-and-notifications-periodic-notification-overview.md)
-   [Muestras de código de notificaciones de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Push_notifications"></span><span id="push_notifications"></span><span id="PUSH_NOTIFICATIONS"></span>Notificaciones de inserción


Las notificaciones de inserción son ideales para comunicar datos en tiempo real o datos personalizados para tu usuario. Las notificaciones de inserción se usan para el contenido que se genera en momentos impredecibles, como las noticias de último momento, las actualizaciones de redes sociales o los mensajes instantáneos. Las notificaciones de inserción también son útiles para las situaciones en las que los datos están sujetos a limitación temporal de un modo inadecuado para notificaciones periódicas, como los resultados deportivos durante un partido.

Las notificaciones de inserción necesitan un servicio de nube que administre canales de notificaciones de inserción y que elija el momento en que se deben enviar notificaciones y a qué usuarios.

De forma predeterminada, las notificaciones de inserción expiran tres días después del momento en que los Servicios de notificaciones de inserción de Windows (WNS) las reciben. Si fuera necesario, puedes invalidar este valor predeterminado con un tiempo de caducidad explícito.

Para obtener más información, consulta lo siguiente:

-   [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
-   [Directrices para notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [Muestras de código de notificaciones de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

**Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <span id="related_topics"></span>Temas relacionados


* [Enviar una notificación de icono local](tiles-and-notifications-sending-a-local-tile-notification.md)
* [Directrices para notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Directrices para notificaciones programadas](https://msdn.microsoft.com/library/windows/apps/hh761464)
* [Directrices para notificaciones del sistema](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [Introducción a las notificaciones periódicas](tiles-and-notifications-periodic-notification-overview.md)
* [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
* [Muestras de código de notificaciones de la Plataforma universal de Windows (UWP) en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 






<!--HONumber=Mar16_HO1-->



---
Description: Windows Inserte Notification Services (WNS) permite a los desarrolladores de terceros enviar notificaciones del sistema, iconos, distintivos y sin formato desde su propio servicio en la nube. Hay muchas maneras de enviar las notificaciones en función de las necesidades de la aplicación.
title: Elegir el tipo adecuado de canal de notificación de inserción
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 326012a38f2d4a8cd7d5c406c160db5168c9877d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219228"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Elegir el tipo adecuado de canal de notificación de inserción

En este artículo se tratan los tres tipos de canales de notificaciones de entrega de Windows (principales, secundarios y alternativos) que le ayudarán a ofrecer contenido a su aplicación. 

(Para obtener más información sobre cómo crear notificaciones de envío, consulte la [Introducción a Windows Notification Services (WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md)). 

## <a name="types-of-push-channels"></a>Tipos de canales de envío 

Hay tres tipos de canales de inserciones que se pueden usar para enviar notificaciones a una aplicación de Windows. Son las siguientes: 

[Canal principal](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : el canal de instalación "tradicional". Cualquier aplicación de la tienda puede utilizarla para enviar notificaciones del sistema, de iconos, sin formato o de distintivo. [Obtenga más información aquí](windows-push-notification-services--wns--overview.md).

[Canal de icono secundario](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : se usa para enviar actualizaciones de iconos a un icono secundario. Solo se puede usar para enviar notificaciones de icono o distintivo a un icono secundario anclado en la pantalla Inicio del usuario.

[Canal alternativo](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : un nuevo tipo de canal agregado en Creators Update. Permite que se envíen notificaciones sin procesar a cualquier aplicación de Windows, incluidas las que no están registradas en el almacén. 

> [!NOTE]
> Independientemente del canal de inserciones que use, una vez que la aplicación se ejecute en el dispositivo, siempre podrá enviar notificaciones del sistema local, el icono o el distintivo. Puede enviar notificaciones locales desde los procesos de aplicación en primer plano o desde una tarea en segundo plano. 


## <a name="primary-channels"></a>Canales principales

Estos son los canales que se usan con más frecuencia en Windows ahora y son buenos para casi cualquier escenario en el que la aplicación se va a distribuir a través de la Microsoft Store. Permiten enviar todos los tipos de notificaciones a la aplicación. 

### <a name="what-do-primary-channels-enable"></a>¿Qué permiten los canales principales?

-   **Enviando actualizaciones de iconos o de notificaciones al icono principal.** Si el usuario ha elegido anclar el icono a la pantalla Inicio, es su oportunidad de mostrarlo. Envíe actualizaciones con información útil o recordatorios de experiencias en la aplicación. 
-   **Envío de notificaciones del sistema.** Las notificaciones del sistema son una oportunidad para obtener cierta información delante del usuario inmediatamente. Se dibujan por el shell sobre la mayoría de las aplicaciones y residen en el centro de actividades para que el usuario pueda volver atrás e interactuar con ellos más adelante. 
-   **Envío de notificaciones sin procesar para desencadenar una tarea en segundo plano.** A veces, desea realizar algún trabajo en nombre del usuario en función de una notificación. Las notificaciones sin procesar permiten la ejecución de las tareas en segundo plano de la aplicación 
-   **Cifrado de mensajes en tránsito proporcionado por Windows mediante TLS.** Los mensajes se cifran en la conexión y ambos entran en WNS y van al dispositivo del usuario.  

### <a name="limitations-of-primary-channels"></a>Limitaciones de los canales principales

-   Requiere el uso de la API de REST de WNS para notificaciones de envío, que no es estándar en todos los proveedores de dispositivos. 
-   Solo se puede crear un canal por aplicación 
-   Requiere que la aplicación se registre en el Microsoft Store

### <a name="creating-a-primary-channel"></a>Creación de un canal principal 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Canales de mosaicos secundarios

Se trata de canales que se pueden usar para enviar actualizaciones de iconos y notificaciones a un icono secundario. Las usan las aplicaciones para notificar a los usuarios de acciones o información interesantes con las que pueden interactuar en la aplicación, como los nuevos mensajes de un chat de grupo o una puntuación deportiva actualizada. 

### <a name="what-do-secondary-tile-channels-enable"></a>¿Qué habilitan los canales de iconos secundarios?

-   Envío de notificaciones de icono o distintivo a mosaicos secundarios. Los iconos secundarios son una excelente manera de volver a insertar usuarios en la aplicación. Son un vínculo profundo a la información que le interesan, y la información relevante sobre los mosaicos ayuda a volver a devolverlos.
-   Separación de los canales (y expira) entre varios mosaicos. Esto le permite separar la lógica del back-end entre los distintos tipos de iconos secundarios que un usuario podría anclar a su pantalla Inicio. 
-   Cifrado de mensajes en tránsito proporcionado por Windows mediante TLS. Los mensajes se cifran en la conexión y ambos entran en WNS y van al dispositivo del usuario.  

### <a name="limitations-of-secondary-tile-channels"></a>Limitaciones de los canales de mosaicos secundarios

-   No se permiten notificaciones de notificación o sin procesar. El sistema omite las notificaciones del sistema o las notificaciones sin procesar enviadas a un icono secundario.
-   Requiere que la aplicación se registre en el Microsoft Store


### <a name="creating-a-secondary-tile-channel"></a>Creación de un canal de icono secundario 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Canales alternativos

Los canales alternativos permiten que las aplicaciones envíen notificaciones de envío sin registrarse en el Microsoft Store o crear canales de inserciones fuera del principal que se usa para la aplicación. 
 
### <a name="what-do-alternate-channels-enable"></a>¿Qué habilitan los canales alternativos?
-   Enviar notificaciones de extracción sin formato a Windows que se ejecuta en cualquier dispositivo de Windows. Los canales alternativos solo permiten notificaciones sin procesar (sin embargo, puede activar una tarea en segundo plano para mostrar localmente notificaciones del sistema o de iconos).
-   Permite que las aplicaciones creen varios canales de inserciones sin procesar para diferentes características dentro de la aplicación. Una aplicación puede crear hasta 1000 canales alternativos y cada uno es válido durante 30 días. La aplicación puede administrar o revocar cada uno de estos canales por separado.
-   Se pueden crear canales de inserciones alternativos sin registrar una aplicación con el Microsoft Store. Si la aplicación va a instalarse en dispositivos sin registrarla en el Microsoft Store, seguirá pudiendo recibir notificaciones de envío.
-   Los servidores pueden enviar notificaciones de envío mediante las API de REST estándar del W3C y el protocolo VAPID. Los canales alternativos usan el protocolo estándar de W3C, lo que permite simplificar la lógica de servidor que debe mantenerse.
-   Cifrado completo de mensajes de un extremo a otro. Mientras que el canal principal proporciona cifrado mientras está en tránsito, si desea ser más seguro, los canales alternativos permiten que la aplicación pase a través de los encabezados de cifrado para proteger un mensaje. 

### <a name="limitations-of-alternate-channels"></a>Limitaciones de los canales alternativos
-   El servidor de la aplicación no puede enviar notificaciones de notificación de la notificación de envío, icono o tipo de distintivo. Solo se pueden enviar notificaciones de extracción sin procesar. La aplicación todavía puede enviar notificaciones locales desde la tarea en segundo plano. 
-   Requiere una API de REST diferente de la de los canales primarios o secundarios. El uso de la API de REST de W3C estándar significa que la aplicación necesitará una lógica diferente para enviar las actualizaciones de la notificación de envío o el icono.

### <a name="creating-an-alternate-channel"></a>Creación de un canal alternativo 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Comparación de tipos de canal
A continuación se muestra una comparación rápida entre los distintos tipos de canales:

<table>

<tr class="header">
<th align="left"><b>Tipo</b></th>
<th align="left"><b>¿La notificación de la inserciones?</b></th>
<th align="left"><b>¿Icono o distintivo de la inserciones?</b></th>
<th align="left"><b>Enviar notificaciones de extracción</b></th>
<th align="left"><b>Autenticación</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>¿Es necesario el registro de almacén?</b></th>
<th align="left"><b>Channels</b></th>
<th align="left"><b>Cifrado</b></th>
</tr>


<tr class="odd">
<td align="left">Principal</td>
<td align="left">Sí</td>
<td align="left">Sí: solo icono principal</td>
<td align="left">Sí</td>
<td align="left">OAuth</td>
<td align="left">API DE REST DE WNS</td>
<td align="left">Sí</td>
<td align="left">Una por aplicación</td>
<td align="left">En tránsito</td>
</tr>
<tr class="even">
<td align="left">Icono secundario</td>
<td align="left">No</td>
<td align="left">Sí: solo mosaico secundario</td>
<td align="left">No</td>
<td align="left">OAuth</td>
<td align="left">API DE REST DE WNS</td>
<td align="left">Sí</td>
<td align="left">Uno por icono secundario</td>
<td align="left">En tránsito</td>
</tr>
<tr class="odd">
<td align="left">Alternativa</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Sí</td>
<td align="left">VAPID</td>
<td align="left">Webinserte W3C Standard</td>
<td align="left">No</td>
<td align="left">1.000 por aplicación</td>
<td align="left">En tránsito + cifrado de extremo a extremo posible con el encabezado pass through (requiere código de aplicación)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Elección del canal adecuado

En general, se recomienda usar el canal principal en la aplicación, con algunas excepciones: 

1. Si va a insertar una actualización de icono en un icono secundario, use el canal de inserción de mosaicos secundarios.
2. Si va a pasar canales a otros servicios (por ejemplo, en el caso de un explorador), use el canal alternativo.
3. Si está creando una aplicación que no aparecerá en la lista de la tienda Windows (por ejemplo, una aplicación de LOB), use un canal alternativo.
4. Si tiene un código de inserciones Web en el servidor que desea reutilizar o necesita varios canales en el servicio back-end, use canales alternativos.

## <a name="related-articles"></a>Artículos relacionados

* [Enviar una notificación de icono local](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Notificaciones del sistema interactivas y adaptables](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Inicio rápido: Envío de una notificación de inserción](/previous-versions/windows/apps/hh868252(v=win.10))
* [Cómo actualizar un distintivo mediante notificaciones de inserción](/previous-versions/windows/apps/hh465450(v=win.10))
* [Cómo solicitar, crear y guardar un canal de notificación](/previous-versions/windows/apps/hh465412(v=win.10))
* [Cómo interceptar notificaciones para aplicaciones en ejecución](/previous-versions/windows/apps/hh465450(v=win.10))
* [Cómo autenticar con los Servicios de notificaciones de inserción de Windows (WNS)](/previous-versions/windows/apps/hh465407(v=win.10))
* [Solicitud de servicio de notificaciones de inserción y encabezados de respuesta](/previous-versions/windows/apps/hh465435(v=win.10))
* [Directrices y lista de comprobación de notificaciones de inserción](./windows-push-notification-services--wns--overview.md)
* [Notificaciones sin procesar](raw-notification-overview.md)

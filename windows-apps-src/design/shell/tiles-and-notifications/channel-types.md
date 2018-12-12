---
Description: Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. There are many ways to send the notifications depending on the needs of your application
title: Elegir el tipo adecuado de canal de notificación de inserción
ms.date: 07/07/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 075eaf5c02e5bddb4b87d7e4aaf931cbfde53cdd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8944300"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Elegir el tipo adecuado de canal de notificación de inserción

Este artículo cubre los tres tipos de canales de notificaciones de inserción UWP (principal, secundario y alternativo) que te ayudarán a entregar contenido a tu aplicación. 

(Para obtener más información sobre cómo crear las notificaciones de inserción, consulta la [información general de los servicios de notificaciones de inserción de Windows (WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md)). 

## <a name="types-of-push-channels"></a>Tipos de canales de inserción 

Existen tres tipos de canales de inserción que pueden usarse para enviar notificaciones a una aplicación para UWP. Estos son: 

[Canal principal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_): el canal de inserción "tradicional". Puede utilizarlo cualquier aplicación de la tienda para enviar notificaciones del sistema, iconos, sin procesar o de distintivos (vínculo a descripciones de notificaciones del sistema/de iconos/de distintivos)

[Canal de iconos secundario](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_): se usa para enviar actualizaciones de iconos para iconos secundarios. Solo puede usarse para enviar notificaciones de iconos o distintivos o a un icono secundario anclado en la pantalla de inicio del usuario

[Canal alternativo](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_): un nuevo tipo de canal que se agregó en la actualización Creators Update. Permite que las notificaciones sin procesar se envíen a cualquier aplicación para UWP, incluidas aquellas que no están registradas en la Tienda. 

> [!NOTE]
> Sin importar qué canal inserción uses, una vez que se ejecuta la aplicación en el dispositivo, siempre será capaz de enviar notificaciones locales del sistema local, de icono o de distintivos. Puede enviar notificaciones locales desde los procesos de la aplicación en primer plano o desde una tarea en segundo plano. 


## <a name="primary-channels"></a>Canales principales

Son los más usados actualmente en Windows y son adecuados para casi todos los escenarios donde la aplicación vaya a distribuirse a través de la Microsoft Store. Permiten enviar todos los tipos de notificaciones a la aplicación. 

### <a name="what-do-primary-channels-enable"></a>¿Qué permiten los canales principales?

-   **Enviar actualizaciones de iconos o distintivos para el icono principal.** Si el usuario ha elegido anclar tu icono a la pantalla de inicio, esta es tu oportunidad de que esté a la vista. Envía actualizaciones con información útil o avisos de experiencias dentro de la aplicación. 
-   **Envío de notificaciones del sistema.** Las notificaciones son una oportunidad de poner inmediatamente determinada información ante el usuario. Las hace mostrar el shell en la parte superior de la mayoría de las aplicaciones y, aparecen en vivo en el centro de actividades, por lo que el usuario puede volver atrás e interactuar con ellas más tarde. 
-   **Envío de notificaciones sin procesar para desencadenar una tarea en segundo plano.** A veces, quieres realizar algún trabajo en nombre del usuario, en función de una notificación. Las notificaciones sin procesar permiten que se ejecuten las tareas en segundo plano de tu aplicación 
-   **Cifrado de mensajes en tránsito proporcionado por Windows con TLS.** Los mensajes se cifran en la red tanto al llegar a WNS como al ir hacia el dispositivo del usuario.  

### <a name="limitations-of-primary-channels"></a>Limitaciones de los canales principales

-   Requiere el uso de la API WNS REST para notificaciones de inserción, que no es estándar para los proveedores de dispositivos. 
-   Se puede crear un solo canal por aplicación 
-   Requiere que tu aplicación esté registrada en la Microsoft Store.

### <a name="creating-a-primary-channel"></a>Creación de un canal principal 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Canales de Iconos secundarios

Son canales que pueden usarse para enviar actualizaciones de iconos y distintivos a un icono secundario. Los usan las aplicaciones para notificar a los usuarios sobre acciones o información de interés con que poder interactuar en la aplicación, como mensajes nuevos de un grupo de chat o unos resultados deportivos actualizados. 

### <a name="what-do-secondary-tile-channels-enable"></a>¿Qué permiten los canales secundarios de iconos?

-   Enviar notificaciones de iconos o distintivos a iconos secundarios Los iconos secundarios son una manera excelente de hacer volver a los usuarios a la aplicación. Son un vínculo profundo a la información que les interesa y el incluir información relevante en los iconos ayuda a hacerles volver una y otra vez.
-   Separación de canales (y caducados) entre diversos iconos. Esto te permite separar la lógica en el back-end entre los distintos tipos de iconos secundarios que un usuario puede anclar a la pantalla de inicio. 
-   Cifrado de mensajes en tránsito proporcionado por Windows con TLS. Los mensajes se cifran en la red tanto al llegar a WNS como al ir hacia el dispositivo del usuario.  

### <a name="limitations-of-secondary-tile-channels"></a>Limitaciones de los canales de secundarios de iconos

-   No se permiten notificaciones del sistema ni sin procesar. Las notificaciones del sistema o sin procesar enviadas a un icono secundario las ignora el sistema.
-   Requiere que tu aplicación esté registrada en la Microsoft Store.


### <a name="creating-a-secondary-tile-channel"></a>Creación de un canal secundario de iconos 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Canales alternativos

Los canales alternativos permiten a las aplicaciones enviar notificaciones de inserción sin registrarse en la Microsoft Store ni crear canales de inserción distintos del principal utilizado para la aplicación. 
 
### <a name="what-do-alternate-channels-enable"></a>¿Qué permiten los canales alternativos?
-   Enviar notificaciones de inserción sin procesar a una UWP que se ejecute en cualquier dispositivo Windows. Los canales alternativos solo permiten notificaciones sin procesar.
-   Permite que las aplicaciones creen varios canales de inserción sin procesar para diferentes funciones de la aplicación. Una aplicación puede crear hasta 1000 canales alternativos, siendo válido cada uno de ellos durante 30 días. Cada uno de estos canales puede administrado o revocarlo por separado la aplicación.
-   Los canales de inserción alternativos pueden crearse sin registrar una aplicación en la Microsoft Store. Si tu aplicación va a instalarse en dispositivos sin registrarla en la Microsoft Store., podrá seguir recibiendo notificaciones de inserción.
-   Los servidores pueden enviar notificaciones con el protocolo las API y VAPID REST de W3C estándar. Los canales alternativos usan el protocolo estándar W3C, lo que permite simplificar la lógica del servidor que haya que mantener.
-   Cifrado de mensaje completo, de principio a fin. Mientras el canal principal proporciona cifrado en tránsito, si quieres una máxima seguridad, los canales alternativos permiten a tu aplicación pasar a través de los encabezados de cifrado para proteger un mensaje. 

### <a name="limitations-of-alternate-channels"></a>Limitaciones de los canales alternativos
-   Las aplicaciones no pueden enviar notificaciones de tipos del sistema, iconos o distintivos. El canal alternativo limita la capacidad de enviar otros tipos de notificaciones. Tu aplicación seguirá pudiendo enviar notificaciones locales desde tareas en segundo plano. 
-   Requiere una API REST diferente a la de los canales primarios o secundarios de iconos. El uso de la API REST estándar de W3C significa que tu aplicación deberá tener una lógica diferente para enviar notificaciones de inserción del sistema o actualizaciones de iconos.

### <a name="creating-an-alternate-channel"></a>Creación de un canal alternativo 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Comparación de tipos de canal
Esta es una comparación rápida entre los distintos tipos de canales:

<table>

<tr class="header">
<th align="left"><b>Tipo</b></th>
<th align="left"><b>¿Notificación del sistema?</b></th>
<th align="left"><b>¿Notificación de icono, distintivo?</b></th>
<th align="left"><b>¿Notificaciones de inserción sin procesar?</b></th>
<th align="left"><b>Autenticación</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>¿Es obligatorio el registro en la Tienda?</b></th>
<th align="left"><b>Canales</b></th>
<th align="left"><b>Cifrado</b></th>
</tr>


<tr class="odd">
<td align="left">Principal</td>
<td align="left">Sí</td>
<td align="left">Sí, solo el icono principal</td>
<td align="left">Sí</td>
<td align="left">OAuth</td>
<td align="left">API REST WNS</td>
<td align="left">Sí</td>
<td align="left">Una por aplicación</td>
<td align="left">En transito</td>
</tr>
<tr class="even">
<td align="left">Icono secundario</td>
<td align="left">No</td>
<td align="left">Sí, solo iconos secundarios</td>
<td align="left">No</td>
<td align="left">OAuth</td>
<td align="left">API REST WNS</td>
<td align="left">Sí</td>
<td align="left">Uno por icono secundario</td>
<td align="left">En transito</td>
</tr>
<tr class="odd">
<td align="left">Alternativo</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Sí</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C estándar</td>
<td align="left">No</td>
<td align="left">1.000 por aplicación</td>
<td align="left">En tránsito + cifrado de principio a fin posible con paso a través de encabezado (requiere código de la aplicación)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Elección del canal adecuado

En general, te recomendamos que uses el canal principal en la aplicación, con unas pocas excepciones: 

1. Si vas a enviar una actualización de icono a un icono secundario, usa el canal de inserción secundario de iconos.
2. Si pasas canales a otros servicios (como en el caso de un navegador), usa el canal alternativo.
3. Si vas a crear una aplicación que no se enumerará en la Tienda Windows (por ejemplo, una aplicación LOB), usa un canal alternativo.
4. Si tienes código de inserción de web existente en el servidor y quieres reutilizarlo o necesitas múltiples canales en tu servicio back-end, usa canales alternativos.

## <a name="related-articles"></a>Artículos relacionados

* [Enviar una notificación de icono local](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Notificaciones del sistema adaptables e interactivas](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Inicio rápido: envío de una notificación de inserción](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Cómo actualizar un distintivo mediante notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Cómo solicitar, crear y guardar un canal de notificación](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Cómo interceptar notificaciones para aplicaciones en ejecución](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Cómo autenticar con los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Solicitud de servicio de notificaciones de inserción y encabezados de respuesta](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Directrices y lista de comprobación de notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Notificaciones sin procesar](raw-notification-overview.md)

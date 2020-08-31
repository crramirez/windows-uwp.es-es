---
description: Obtenga información acerca de cómo usar la creación de reflejo de la notificación, basado en el centro de actividades en la nube, para ver las notificaciones del sistema del teléfono en su equipo.
title: Reflejo de notificaciones
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, centro de actividades en la nube, creación de reflejo de notificaciones, notificación, dispositivo cruzado
ms.localizationpriority: medium
ms.openlocfilehash: 13e3e9f0b675ef0e5f9e0787f0544f87689cf74a
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054015"
---
# <a name="notification-mirroring"></a>Reflejo de notificaciones

La creación de reflejo de la notificación, con tecnología del centro de actividades en la nube, le permite ver las notificaciones del teléfono en su PC.

> [!IMPORTANT]
> **Requiere actualización de aniversario**: debe ejecutar la compilación 14393 o superior para ver el trabajo de creación de reflejo de la notificación. Si desea utilizar la creación de reflejo de notificaciones de la aplicación, debe tener como destino el SDK 14393 para tener acceso a las API de creación de reflejo.

Con la creación de reflejo de la notificación y Cortana, los usuarios pueden recibir y actuar en las notificaciones de su teléfono (Windows Mobile y Android) desde la comodidad de su PC. Como desarrollador, no tiene que hacer nada para habilitar la creación de reflejo de la notificación, ya que la creación de reflejo funciona automáticamente. Al hacer clic en los botones de la notificación del sistema reflejada, como las respuestas rápidas del mensaje, se redirigirá al teléfono, invocando la tarea en segundo plano o iniciando la aplicación en primer plano.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Los desarrolladores obtienen dos excelentes ventajas de la creación de reflejo de la notificación: las notificaciones reflejadas producen más interacción con el usuario con el servicio y también ayudan a los usuarios a detectar su aplicación de escritorio Microsoft Store. Es posible que los usuarios no sepan siquiera que tiene una aplicación impresionante para Windows disponible para su escritorio de Windows 10. Cuando los usuarios reciben la notificación reflejada desde su teléfono, los usuarios pueden hacer clic en la notificación del sistema para realizar la Microsoft Store, donde pueden instalar la aplicación de Windows.

La creación de reflejo funciona con Windows Phone y Android. Los usuarios deben iniciar sesión en Cortana tanto en el teléfono como en el escritorio para que funcione la creación de reflejo de la notificación.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>¿Qué ocurre si la aplicación está instalada en ambos dispositivos?

Si el usuario ya tiene la aplicación en su equipo, silenciaremos automáticamente la notificación de teléfono reflejado para que no vean las notificaciones duplicadas. Las notificaciones reflejadas se silenciarán automáticamente en función de los siguientes criterios...

1. Existe una aplicación en el equipo con el **mismo nombre para mostrar o el mismo pfn** (nombre de familia de paquete)
2. La aplicación de equipo ha enviado una notificación del sistema

Si la aplicación de PC todavía no ha enviado una notificación del sistema, todavía se mostrarán las notificaciones telefónicas, ya que es posible que el usuario no haya iniciado todavía la aplicación de PC).


## <a name="how-to-opt-out-of-mirroring"></a>Cómo no participar en la creación de reflejo

Los desarrolladores de aplicaciones de Windows, las empresas y los usuarios pueden optar por deshabilitar la creación de reflejo de la notificación.

> [!NOTE]
> Al deshabilitar la creación de reflejo, también se deshabilitará el [descartado universal](universal-dismiss.md).


### <a name="as-a-developer-opt-out-an-individual-notification"></a>Como desarrollador, rechazar una notificación individual

En ocasiones, es posible que tenga una notificación específica del dispositivo que no desea que se refleje en otros dispositivos. Puede evitar que una notificación específica se refleje estableciendo la propiedad de **creación de reflejo** en la notificación del sistema. Actualmente, esta propiedad de creación de reflejo solo se puede establecer en las notificaciones locales (no se puede especificar cuando se envía una notificación de extracción de WNS).

**Problema conocido**: la recuperación de la propiedad de creación de reflejo a través de la `ToastNotificationHistory.GetHistory()` API siempre devolverá el valor predeterminado (**permitido**) en lugar de la opción especificada. No se preocupe, todo es funcional, solo recupera el valor que se ha interrumpido.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>Como desarrollador, no participar completamente

Algunos desarrolladores pueden optar por elegir completamente su aplicación para la creación de reflejo de la notificación. Aunque creemos que todas las aplicaciones se beneficiarían de la creación de reflejo, hacemos que sea fácil rechazarla. Basta con llamar al método siguiente una vez y la aplicación se deshabilitará. Por ejemplo, puede colocar esta llamada en el constructor de la aplicación dentro de `App.xaml.cs` ...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>Como empresa, ¿cómo puedo dejar de participar?

Las empresas pueden optar por deshabilitar completamente la creación de reflejo de la notificación. Para ello, simplemente edite la directiva de grupo para desactivar la creación de reflejo de la notificación.


### <a name="as-a-user-how-do-i-opt-out"></a>Como usuario, ¿cómo puedo dejar de participar?

Los usuarios pueden dejar de participar en aplicaciones individuales o dejar de participar por completo deshabilitando la característica. Es posible que no quiera que las notificaciones de una aplicación específica se reflejen en el escritorio, por lo que puede deshabilitar simplemente esa aplicación específica. Puede encontrar estas opciones en la configuración de Cortana en su teléfono y PC.

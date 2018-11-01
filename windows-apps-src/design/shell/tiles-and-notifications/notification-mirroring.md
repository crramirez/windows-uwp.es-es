---
author: andrewleader
Description: Learn how to use notification mirroring on your toast notifications.
title: Reflejo de notificaciones
label: Notification mirroring
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, notificación del sistema, centro de actividades en la nube, reflejo de notificaciones, notificación, en todos los dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: c36b5ffcbb016e5b89fa1c960a7493767192296c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5869964"
---
# <a name="notification-mirroring"></a>Reflejo de notificaciones

El reflejo de notificaciones, con tecnología del Centro de actividades en la nube, te permite ver las notificaciones del teléfono en tu PC.

> [!IMPORTANT]
> **Requiere la Actualización de aniversario**: Debes estar ejecutando la compilación 14393 o superior para ver el trabajo de reflejo de notificaciones. Si quieres que tu aplicación no participe en el reflejo de notificaciones, debe estar destinada al SDK 14393 para tener acceso a las API de creación de reflejos.

Con el reflejo de notificaciones y Cortana, los usuarios pueden recibir y actuar sobre las notificaciones de su teléfono (Windows Mobile y Android) desde la comodidad de su PC. Como desarrollador, no tienes que hacer nada para habilitar el reflejo de notificaciones, ya que funciona automáticamente. Hacer clic en botones de la notificación del sistema reflejada, como respuestas rápidas a mensajes, se enrutará hacia el teléfono, invocando tu tarea en segundo plano o iniciando tu aplicación en primer plano.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Los desarrolladores obtienen dos grandes ventajas del reflejo de notificaciones: las notificaciones dan lugar mayor interacción del usuario con el servicio, y también ayudan a los usuarios descubrir tu aplicación de escritorio de Microsoft Store! Incluso es posible que tus usuarios no sepan que cuentas con una fantástica aplicación para UWP disponible para su escritorio de Windows 10. Cuando los usuarios reciben la notificación reflejada desde su teléfono, los usuarios hacer clic en la notificación del sistema para ir a la Microsoft Store, donde pueden instalar la aplicación de escritorio para UWP.

El reflejo funciona tanto con Windows Phone como con Android. Los usuarios deben haber iniciado sesión en Cortana tanto en el teléfono como en el escritorio para que funcione el reflejo de notificaciones.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>¿Qué ocurre si la aplicación está instalada en los dos dispositivos?

Si el usuario ya tiene la aplicación en su PC, silenciaremos automáticamente la notificación del teléfono reflejada para que no vean notificaciones duplicadas. Las notificaciones reflejadas se silenciarán automáticamente en función de los siguientes criterios...

1. Existe una aplicación en el PC con el **mismo nombre para mostrar o el mismo PFN** (Nombre de familia de paquete)
2. Esa aplicación de equipo ha enviado una notificación del sistema

Si la aplicación del PC no ha enviado una notificación del sistema todavía, seguiremos mostrando las notificaciones del teléfono, puesto que es posible que el usuario no haya iniciado realmente la aplicación del PC aún).


## <a name="how-to-opt-out-of-mirroring"></a>Cómo optar por no participar en el reflejo

Los usuarios, las empresas y los desarrolladores de la aplicaciones para UWP pueden deshabilitar el reflejo de notificaciones.

> [!NOTE]
> Al deshabilitar el reflejo también se deshabilitará [Descartar universal](universal-dismiss.md).


### <a name="as-a-developer-opt-out-an-individual-notification"></a>Como desarrollador, rechaza una notificación individual

En ocasiones, es posible que tengas una notificación específica de dispositivo que no quieras que se refleje en otros dispositivos. Para impedir que se refleje una notificación específica, establece la propiedad **Mirroring** en la notificación del sistema. Actualmente, esta propiedad de reflejo solo se puede establecer en notificaciones locales (no se puede especificar al enviar una notificación de inserción WNS).

**Problema conocido**: la recuperación de la propiedad Reflejo a través de las API de `ToastNotificationHistory.GetHistory()` siempre devolverá el valor predeterminado (**Allowed**) en lugar de la opción que especificaste. No te preocupes, todo es funcional; solo se trata de recuperar el valor que se ha roto.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>Como desarrollador, opta por no participar por completo

Algunos desarrolladores pueden elegir no participar por completo en el reflejo de notificaciones. Aunque creemos que todas las aplicaciones se beneficiarían del reflejo, facilitamos la no participación. Solo tienes que llamar al siguiente método una vez y tu aplicación no participará. Por ejemplo, puedes hacer esta llamada en el constructor de tu aplicación dentro de `App.xaml.cs`...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>Como empresa, ¿cómo hago para no participar?

Las empresas pueden elegir deshabilitar por completo el reflejo de notificaciones. Para ello, solo tienes que editar la directiva de grupo para desactivar el reflejo de notificaciones.


### <a name="as-a-user-how-do-i-opt-out"></a>Como usuario, ¿cómo hago para no participar?

Los usuarios pueden optar por no participar en aplicaciones individuales o completamente, deshabilitando la característica. Es posible que no quieras notificaciones de una aplicación específica reflejadas en tu escritorio, por lo que puedes deshabilitar simplemente esa aplicación específica. Puedes encontrar estas opciones de configuración de Cortana tanto en tu teléfono como en el PC.
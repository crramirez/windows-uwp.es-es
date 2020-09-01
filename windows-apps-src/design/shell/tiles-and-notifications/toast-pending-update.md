---
description: Obtenga información sobre cómo usar la notificación del sistema con la activación de actualizaciones pendientes para crear interacciones de varios pasos en las notificaciones del sistema.
title: Notificación del sistema con activación de actualización pendiente
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, actualización pendiente, pendingupdate, interactividad de varios pasos y interacciones de varios pasos
ms.localizationpriority: medium
ms.openlocfilehash: 00551414fbefe5591813731337653964bd2524f3
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054545"
---
# <a name="toast-with-pending-update-activation"></a>Notificación del sistema con activación de actualización pendiente

Puede usar **PendingUpdate** para crear interacciones de varios pasos en las notificaciones del sistema. Por ejemplo, como se muestra a continuación, puede crear una serie de notificaciones del sistema en las que las notificaciones posteriores dependen de las respuestas de las notificaciones del sistema anteriores.

![Notificación del sistema con actualización pendiente](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Requiere Desktop Fall Creators Update y 2.0.0 de la biblioteca de notificaciones**: debe estar ejecutando desktop Build 16299 o superior para ver el trabajo de actualización pendiente. Debe usar la versión 2.0.0 o posterior de la [biblioteca NuGet de notificaciones de Community Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para asignar **PendingUpdate** en sus botones. **PendingUpdate** solo se admite en el escritorio y se omitirá en otros dispositivos.


## <a name="prerequisites"></a>Prerrequisitos

En este artículo se supone que tiene conocimientos prácticos de...

- [Construir contenido del sistema](adaptive-interactive-toasts.md)
- [Enviar una notificación del sistema y controlar la activación en segundo plano](send-local-toast.md)


## <a name="overview"></a>Información general

Para implementar una notificación del sistema que usa la actualización pendiente como su comportamiento después de la activación...

1. En los botones de activación en segundo plano del sistema, especifique un **AfterActivationBehavior** de **PendingUpdate**

2. Asignar una **etiqueta** (y, opcionalmente, **agrupar**) al enviar la notificación del sistema

3. Cuando el usuario haga clic en el botón, se activará la tarea en segundo plano y la notificación del sistema se mantendrá en pantalla en estado de actualización pendiente.

4. En la tarea en segundo plano, envíe un nuevo aviso con el nuevo contenido, con la misma **etiqueta** y **Grupo**


## <a name="assign-pendingupdate"></a>Asignar PendingUpdate

En los botones de activación en segundo plano, establezca **AfterActivationBehavior** en **PendingUpdate**. Tenga en cuenta que esto solo funciona con los botones que tienen un **ActivationType** de **fondo**.

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>Usar una etiqueta en la notificación

Para reemplazar posteriormente la notificación, es necesario asignar la **etiqueta** (y, opcionalmente, el **Grupo**) en la notificación.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Reemplazar la notificación del sistema por contenido nuevo

En respuesta al usuario que hace clic en el botón, la tarea en segundo plano se desencadena y debe reemplazar el aviso por el nuevo contenido. Para reemplazar la notificación del sistema, solo tiene que enviar una nueva notificación con la misma **etiqueta** y **Grupo**.

Se recomienda encarecidamente **establecer el audio en modo silencioso** en los reemplazos en respuesta a un clic de botón, ya que el usuario ya está interactuando con la notificación del sistema.

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>Temas relacionados

- [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Enviar una notificación del sistema local y controlar la activación](send-local-toast.md)
- [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
- [Barra de progreso del sistema](toast-progress-bar.md)
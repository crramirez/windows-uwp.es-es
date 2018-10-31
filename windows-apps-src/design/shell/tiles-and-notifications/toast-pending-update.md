---
author: andrewleader
Description: Learn how to create multi-step interactions in your notifications.
title: Notificación del sistema con activación de actualización pendiente
label: Toast with pending update activation
template: detail.hbs
ms.author: mijacobs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, notificaciones del sistema, actualización pendiente, pendingupdate, interactividad de varios pasos, interacciones de varios pasos
ms.localizationpriority: medium
ms.openlocfilehash: 4d21c96676eec1b8b1a1f4397880af937dd8f4b6
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5835395"
---
# <a name="toast-with-pending-update-activation"></a>Notificación del sistema con activación de actualización pendiente

Puedes usar **PendingUpdate** para crear interacciones de varios pasos en tus notificaciones del sistema. Por ejemplo, como se muestra a continuación, puedes crear una serie de notificaciones del sistema donde las notificaciones del sistema posteriores dependen de las respuestas de las notificaciones del sistema anteriores.

![Notificación del sistema con actualización pendiente](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Requiere CreatorsUpdate de Escritorio y 2.0.0 de la biblioteca de notificaciones**: debes estar ejecutando Escritorio compilación 16299 o superior para ver la tarea de actualización pendiente. Debes usar la versión 2.0.0 o superior de la [Biblioteca NuGet de notificaciones del Kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para asignar **PendingUpdate** en tus botones. **PendingUpdate** solo se admite en Escritorio y se ignorará en otros dispositivos.


## <a name="prerequisites"></a>Requisitos previos

Este artículo supone un conocimiento práctico de...

- [Construir el contenido de la notificación del sistema](adaptive-interactive-toasts.md)
- [Enviar una notificación del sistema y controlar la activación en segundo plano](send-local-toast.md)


## <a name="overview"></a>Introducción

Para implementar una notificación del sistema que usa una actualización pendiente como suya tras el comportamiento de activación...

1. En los botones de activación en segundo plano de notificación del sistema, especifica un **AfterActivationBehavior** de **PendingUpdate**.

2. Asignar un **Etiqueta** (y, opcionalmente, **Grupo**) al enviar la notificación del sistema

3. Cuando el usuario hace clic en el botón, se activará la tarea en segundo plano y la notificación del sistema se mantendrá en pantalla en un estado de actualización pendiente.

4. En la tarea en segundo plano, envía una notificación del sistema nueva con el nuevo contenido, usando la misma **Etiqueta** y **Grupo**.


## <a name="assign-pendingupdate"></a>Asignar PendingUpdate

En los botones de activación en segundo plano, establece el **AfterActivationBehavior** en **PendingUpdate**. Ten en cuenta que esto solo funciona para los botones que tienen un **ActivationType** de **Segundo plano**.

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

Para poder reemplazar la notificación más adelante, debemos asignar la **Etiqueta** (y, opcionalmente, el **Grupo**) en la notificación.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Reemplazar la notificación del sistema por el nuevo contenido

En respuesta a la acción del usuario de hacer clic en el botón, se desencadena la tarea en segundo plano y necesita reemplazar la notificación del sistema por el nuevo contenido. Para remplaza la notificación del sistema, solo tiene que enviar una notificación del sistema nueva con la misma **Etiqueta** y **Grupo**.

Se recomienda **establecer el audio en silencio** en reemplazos en respuesta a una acción de clic en el botón, puesto que el usuario ya está interactuando con la notificación del sistema.

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
- [Enviar una notificación del sistema local y administrar la aplicación](send-local-toast.md)
- [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)
- [Barra de progreso de notificación del sistema](toast-progress-bar.md)
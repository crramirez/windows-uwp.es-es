---
author: mijacobs
Description: You can programmatically pin your app to the taskbar,  bnd you can check if it's currently pinned.
title: Anclar tu aplicación a la barra de tareas
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, barra de tareas, administrador de barra de tareas, anclar a la barra de tareas, icono principal
ms.localizationpriority: medium
ms.openlocfilehash: 47fcd1f9d090c49ecbd49e05696b33f789973160
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5815103"
---
# <a name="pin-your-app-to-the-taskbar"></a>Anclar tu aplicación a la barra de tareas

Mediante programación, puedes anclar tu propia aplicación a la barra de tareas, al igual que puedes [anclar tu aplicación al menú Inicio](tiles-and-notifications/primary-tile-apis.md). Y puedes comprobar si la aplicación está anclada actualmente y si la barra de tareas permite el anclaje. 

![Barra de tareas](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Requiere Fall Creators Update**: debes utilizar SDK 16299 y estar ejecutando la compilación 16299 o superior para usar las API de la barra de tareas.

> **API importantes**: [Clase TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>¿Cuándo deberías pedir al usuario que ancle tu aplicación en la barra de tareas? 

La [clase TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) te permite pedir al usuario que ancle la aplicación a la barra de tareas; el usuario debe aprobar la solicitud. Has invertido mucho esfuerzo en el diseño de una aplicación estrella y ahora tienes la oportunidad de pedir al usuario que la ancle a la barra de tareas. Pero antes de profundizar en el código, hay algunas cosas que debes tener en cuenta mientras estás diseñando tu experiencia:

* **Debes** crear una experiencia de usuario sin interrupciones y fácilmente descartable en tu aplicación con una clara llamada a la acción "Anclar a la barra de tareas". Evita el uso de cuadros de diálogo y controles flotantes para este propósito. 
* **Debes** explicar con claridad el valor de tu aplicación antes de pedirle al usuario que la ancle.
* **Debes evitar** pedir al usuario que ancle tu aplicación si el icono ya está anclado o el dispositivo no lo admite. (En este artículo se explica cómo determinar si se admite el anclaje).
* **Debes evitar** pedir repetidamente al usuario que ancle tu aplicación (probablemente eso le aburrirá).
* **Debes evitar** llamar a la API de ancla sin interacción explícita del usuario o cuando la aplicación está minimizada o no abierta.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Comprueba si existen las API necesarias

Si la aplicación admite versiones anteriores de Windows 10, debes comprobar si la clase TaskbarManager está disponible. Puedes usar el [método ApiInformation.IsTypePresent](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) para realizar esta comprobación. Si la clase TaskbarManager no está disponible, evita ejecutar llamadas a las API.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Comprueba si está presente la barra de tareas y si permite anclar

Las aplicaciones para UWP pueden ejecutarse en una amplia variedad de dispositivos; no todos ellos admiten la barra de tareas. Por ahora, solo los dispositivos de escritorio admiten la barra de tareas. 

Incluso si la barra de tareas está disponible, una directiva de grupo del equipo del usuario podría deshabilitar el anclaje a la barra de tareas. Por lo tanto, antes de intentar anclar la aplicación, deberás comprobar si se admite el anclaje de la barra de tareas. La [propiedad TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) devuelve true si la barra de tareas está presente y permite anclar. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Si no quieres anclar la aplicación a la barra de tareas y simplemente quieres saber si está disponible la barra de tareas, usa la [propiedad TaskbarManager.IsSupported](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Comprueba si la aplicación está anclada actualmente a la barra de tareas

Obviamente, no tiene sentido pedir al usuario que te permita anclar la aplicación a la barra de tareas si ya está anclada allí. Puedes usar el [método TaskbarManager.IsCurrentAppPinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) para comprobar si la aplicación ya está anclada antes de preguntar al usuario.

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. Ancla la aplicación

Si la barra de tareas está presente, se permite anclar y tu aplicación no está anclada en ese momento, podría interesarte mostrar una sugerencia sutil para avisar a los usuarios de que pueden anclar tu aplicación. Por ejemplo, podrías mostrar un icono de anclar en algún lugar en la interfaz de usuario, donde el usuario pueda hacer clic. 

Si el usuario hace clic en interfaz de usuario de la sugerencia de anclado, llama al [método TaskbarManager.RequestPinCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync). Este método muestra un diálogo que pide al usuario confirmar que quiere que la aplicación quede anclada a la barra de tareas.

> [!IMPORTANT]
> Esto se debe llamar desde un subproceso de interfaz de usuario en primer plano; de lo contrario, se generará una excepción.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![Diálogo de anclado](images/taskbar/pin-dialog.png)

Este método devuelve un valor booleano que indica si la aplicación está anclada en ese momento a la barra de tareas. Si tu aplicación ya estaba anclada, el método devuelve inmediatamente true sin mostrar el cuadro de diálogo al usuario. Si el usuario hace clic en "No" en el diálogo, o no se admite el anclaje de tu aplicación a la barra de tareas, el método devolverá false. En caso contrario, el usuario hizo clic en Sí y la aplicación quedó anclada, con lo que la API devolverá true.


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Clase TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Anclar una aplicación al menú Inicio](tiles-and-notifications/primary-tile-apis.md)
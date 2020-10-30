---
description: Puede anclar la aplicación mediante programación a la barra de tareas, BND puede comprobar si está anclada actualmente.
title: Anclar la aplicación a la barra de tareas
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, barra de tareas, administrador de la barra de tareas, anclar a la barra de tareas, icono principal
ms.localizationpriority: medium
ms.openlocfilehash: fa33725447da80b5c3295455f12a3851228a2756
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034168"
---
# <a name="pin-your-app-to-the-taskbar"></a>Anclar la aplicación a la barra de tareas

Puede anclar su propia aplicación mediante programación a la barra de tareas, igual que puede [anclar la aplicación al menú Inicio](tiles-and-notifications/primary-tile-apis.md). También puede comprobar si la aplicación está anclada actualmente y si la barra de tareas permite el anclaje. 

![Captura de pantalla de una barra de tareas de Windows 10 que muestra la aplicación anclada allí.](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Requiere Fall Creators Update** : debe tener como destino el SDK 16299 y ejecutar la compilación 16299 o superior para usar las API de la barra de tareas.

> **API importantes** : [clase TaskbarManager](/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>¿Cuándo se debe pedir al usuario que ancle la aplicación a la barra de tareas? 

La [clase TaskbarManager](/uwp/api/windows.ui.shell.taskbarmanager) permite pedirle al usuario que ancle la aplicación a la barra de tareas. el usuario debe aprobar la solicitud. Puede crear una gran cantidad de esfuerzo en la creación de una aplicación estelar y ahora tiene la oportunidad de pedirle al usuario que la ancle en la barra de tareas. Pero antes de profundizar en el código, aquí tiene algunas cosas que debe tener en cuenta al diseñar su experiencia:

* Cree una experiencia de **usuario no disruptiva** y fácilmente descartable en la aplicación con una llamada a la acción "anclar a la barra de tareas". Evite el uso de cuadros de diálogo y controles flotantes para este fin. 
* Explique **claramente el** valor de la aplicación antes de pedir al usuario que la ancle.
* **No** pida a un usuario que ancle su aplicación si el icono ya está anclado o el dispositivo no lo admite. (En este artículo se explica cómo determinar si se admite el anclaje).
* **No** pida al usuario que ancle su aplicación repetidamente (probablemente se molestará).
* **No** llame a la API de PIN sin interacción explícita del usuario o cuando la aplicación se minimice o no se abra.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Compruebe si existen las API necesarias

Si la aplicación es compatible con versiones anteriores de Windows 10, debe comprobar si la clase TaskbarManager está disponible. Puede usar el  [método ApiInformation. IsTypePresent](/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) para realizar esta comprobación. Si la clase TaskbarManager no está disponible, evite ejecutar llamadas a las API.

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


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Compruebe si la barra de tareas está presente y permite el anclaje

Las aplicaciones de Windows se pueden ejecutar en una amplia variedad de dispositivos. no todas ellas admiten la barra de tareas. En este momento, solo los dispositivos de escritorio admiten la barra de tareas. 

Incluso si la barra de tareas está disponible, es posible que una directiva de grupo en el equipo del usuario deshabilite el anclaje de la barra de tareas. Por lo tanto, antes de intentar anclar la aplicación, debe comprobar si se admite el anclaje en la barra de tareas. La [propiedad TaskbarManager. IsPinningAllowed](/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) devuelve true si la barra de tareas está presente y permite el anclaje. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Si no quiere anclar la aplicación a la barra de tareas y solo desea averiguar si la barra de tareas está disponible, use la [propiedad TaskbarManager. IsSupported](/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Compruebe si la aplicación está anclada actualmente a la barra de tareas

Obviamente, no hay ningún punto en pedir al usuario que le permita anclar la aplicación a la barra de tareas si ya está anclada allí. Puede usar el [método TaskbarManager. IsCurrentAppPinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) para comprobar si la aplicación ya está anclada antes de preguntar al usuario.

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


##  <a name="4-pin-your-app"></a>4. Ancle la aplicación

Si la barra de tareas está presente y se permite el anclaje y la aplicación no está anclada actualmente, es posible que quiera mostrar una sugerencia sutil para que los usuarios sepan que pueden anclar la aplicación. Por ejemplo, puede mostrar un icono de anclaje en algún lugar de la interfaz de usuario en el que el usuario pueda hacer clic. 

Si el usuario hace clic en la interfaz de usuario de sugerencia de PIN, llamará al [método TaskbarManager. RequestPinCurrentAppAsync](/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync). Este método muestra un cuadro de diálogo que pide al usuario que confirme que desea que la aplicación esté anclada a la barra de tareas.

> [!IMPORTANT]
> Se debe llamar a este método desde un subproceso de interfaz de usuario de primer plano; de lo contrario, se producirá una excepción.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![Cuadro de diálogo anclar](images/taskbar/pin-dialog.png)

Este método devuelve un valor booleano que indica si la aplicación está anclada ahora a la barra de tareas. Si la aplicación ya está anclada, el método devuelve inmediatamente true sin mostrar el cuadro de diálogo al usuario. Si el usuario hace clic en "no" en el cuadro de diálogo o no se permite anclar la aplicación a la barra de tareas, el método devuelve false. De lo contrario, el usuario hizo clic en sí y la aplicación estaba anclada, y la API devolverá True.


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Clase TaskbarManager](/uwp/api/windows.ui.shell.taskbarmanager)
* [Anclar una aplicación al menú Inicio](tiles-and-notifications/primary-tile-apis.md)

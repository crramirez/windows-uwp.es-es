---
Description: Puede anclar el icono principal de la aplicación mediante programación para que se inicie, al igual que puede anclar iconos secundarios. También puede comprobar si está anclado actualmente.
title: API de iconos principales
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, StartScreenManager, icono principal de PIN, API de iconos principales, comprobar si el icono está anclado, icono dinámico
ms.localizationpriority: medium
ms.openlocfilehash: 569ef5de9298a0fb9da58e4aaa88689c35b98c72
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172349"
---
# <a name="primary-tile-apis"></a>API de icono principal
 

Las API de iconos principales permiten comprobar si la aplicación está anclada al inicio y solicitar anclar el icono principal de la aplicación.

> [!IMPORTANT]
> **Requiere Creators Update**: debe tener como destino el SDK 15063 y ejecutar la compilación 15063 o superior para usar las API del icono principal.

> **API importantes**: [**clase StartScreenManager**](/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>Cuándo usar las API de iconos principales

Usted pone un gran esfuerzo en diseñar una gran experiencia para el icono principal de la aplicación y ahora tiene la oportunidad de pedirle al usuario que lo ancle a iniciar. Pero antes de profundizar en el código, aquí tiene algunas cosas que debe tener en cuenta a la vez que está diseñando su experiencia:

* Cree una experiencia de **usuario no disruptiva** y fácilmente descartable en la aplicación con una llamada clara a la acción.
* Explique claramente el valor del icono dinámico de la aplicación antes de pedir al usuario **que lo ancle** .
* **No** pida a un usuario que ancle el icono de la aplicación si el icono ya está anclado o el dispositivo no lo admite (más información a continuación).
* **No** pida al usuario repetidamente que ancle el icono de la aplicación (probablemente se molestará).
* **No** llame a la API de PIN sin interacción explícita del usuario o cuando la aplicación se minimice o no se abra.


## <a name="checking-whether-the-apis-exist"></a>Comprobando si existe la API

Si la aplicación es compatible con versiones anteriores de Windows 10, debe comprobar si estas API de iconos principales están disponibles. Para ello, use ApiInformation. Si las API de los iconos principales no están disponibles, evite ejecutar llamadas a las API.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>Comprobar si el inicio es compatible con la aplicación

Según el menú Inicio actual y el tipo de aplicación, es posible que no se admita el anclaje de la aplicación a la pantalla Inicio actual. Solo soporte técnico de escritorio y móvil anclando el icono principal al inicio. Por lo tanto, antes de mostrar cualquier interfaz de usuario de PIN o ejecutar cualquier código PIN, primero debe comprobar si la aplicación se admite incluso para la pantalla de inicio actual. Si no se admite, no se pide al usuario que ancle el icono.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Compruebe si está anclado actualmente

Para averiguar si el icono principal está anclado actualmente para iniciarse, use el método [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) .

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Anclar el icono principal

Si el icono principal no está anclado actualmente y el icono es compatible con el inicio, es posible que quiera mostrar una sugerencia a los usuarios para que puedan anclar el icono principal.

> [!NOTE]
> Se debe llamar a esta API desde un subproceso de interfaz de usuario mientras la aplicación está en primer plano y solo se debe llamar a esta API una vez que el usuario haya solicitado intencionadamente el icono principal (por ejemplo, después de que el usuario haga clic en sí para anclar el icono).

Si el usuario hace clic en el botón para anclar el icono principal, llamaría al método [RequestAddAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) para solicitar que el icono se inicie. Se mostrará un cuadro de diálogo que pide al usuario que confirme que desea que se inicie el icono.

Esto devolverá un valor booleano que representa si el icono está anclado al inicio. Si el icono ya estaba anclado, se devolverá inmediatamente true sin mostrar el cuadro de diálogo al usuario. Si el usuario hace clic en no en el cuadro de diálogo o no se admite el anclar el icono a Inicio, se devolverá FALSE. De lo contrario, el usuario hizo clic en sí y el icono se ancló, y la API devolverá True.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>Recursos

* [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [Anclar a la barra de tareas](../pin-to-taskbar.md)
* [Iconos, distintivos y notificaciones](index.md)
* [Documentación adaptable de iconos](create-adaptive-tiles.md)
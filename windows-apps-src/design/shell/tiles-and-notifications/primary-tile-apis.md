---
author: andrewleader
Description: You can programmatically pin your own app's primary tile to Start, just like you can pin secondary tiles. And you can check whether it's currently pinned.
title: API de icono principal
label: Primary tile API's
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, StartScreenManager, anclar icono principal, api de icono principal, comprobar si el icono está anclado, icono dinámico
ms.localizationpriority: medium
ms.openlocfilehash: 42b4c014dfd49c42497b8846e37e37af53cc3885
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4610273"
---
# <a name="primary-tile-apis"></a>API de icono principal
 

Las API de icono principal te permiten comprobar si tu aplicación está anclada actualmente a Inicio y solicitar anclar el icono principal de la aplicación.

> [!IMPORTANT]
> **Requiere la actualización Creators Update**: debes utilizar SDK 15063 y estar ejecutando la compilación 15063 o superior para usar las API de icono principal.

> **API importantes**: [**Clase StartScreenManager**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>Cuándo usar las API de icono principal

Inviertes mucho esfuerzo en el diseño de una gran experiencia para el icono principal de la aplicación y ahora tienes la oportunidad de pedir al usuario que la ancle a Inicio. Pero antes de profundizar en el código, hay algunas cosas que debes tener en cuenta mientras estás diseñando tu experiencia:

* **Cosas que hacer** Crea una experiencia de usuario sin interrupciones y fácilmente descartable en tu aplicación con una llamada a la acción "Anclar icono dinámico" clara.
* **Cosas que hacer** Explica con claridad el valor del icono dinámico de tu aplicación antes de pedirle al usuario que lo ancle.
* **Debes evitar** pedir al usuario que ancle el icono de tu aplicación si ya está anclado el icono o el dispositivo no lo admite (más información a continuación).
* **Debes evitar** pedir pidas varias veces al usuario que ancle el icono de la aplicación (probablemente acabará siendo una molestia para ellos).
* **Debes evitar** llamar a la API de ancla sin interacción explícita del usuario o cuando la aplicación está minimizada o no abierta.


## <a name="checking-whether-the-apis-exist"></a>Comprobar la existencia de la API

Si la aplicación admite versiones anteriores de Windows 10, debes comprobar si estas API de icono principal están disponibles. Haces esto con ApiInformation. Si las API de icono principal no están disponibles, evita ejecutar llamadas a las API.

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


## <a name="check-if-start-supports-your-app"></a>Comprueba si Inicio admite tu aplicación

En función del menú Inicio actual, y del tipo de aplicación, es posible que no se admita tu aplicación a la pantalla Inicio actual. Solo Escritorio y Móvil admiten anclar el icono principal a Inicio. Por lo tanto, antes de mostrar cualquier interfaz de usuario de anclaje o ejecutar cualquier código de anclaje, debes comprobar primero si la aplicación se admite incluso para la pantalla Inicio actual. Si no la admite, no pidas al usuario que ancle el icono.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Comprueba que estás anclado actualmente

Para averiguar si tu icono principal está anclado actualmente a Inicio, usa el método [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_).

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Anclar tu icono principal

Si actualmente tu icono principal no está anclado, y el icono es compatible con Inicio, es posible que quieras mostrar una sugerencia a los usuarios de que pueden anclar tu icono principal.

> [!NOTE]
> Debes llamar a esta API desde un subproceso de interfaz de usuario mientras la aplicación está en primer plano, y solo debes llamar a esta API después de que el usuario haya solicitado intencionadamente que se ancle el icono principal (por ejemplo, después de que el usuario haya hecho clic en Sí a la sugerencia sobre cómo anclar el icono).

Si el usuario hace clic en el botón para anclar el icono principal, a continuación llamaría al método [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) para solicitar que el icono se anclar a Inicio. Esta acción mostrará un cuadro de diálogo en el que se le pedirá al usuario que confirme que quiere que el icono quede anclado a Inicio.

Esto devolverá un valor booleano que indica si el icono está ahora anclado a Inicio. Si ya se ha anclado tu icono, este inmediatamente devolverá true sin mostrar el cuadro de diálogo al usuario. Si el usuario hace clic en No en el cuadro de diálogo, o no se admite el anclaje del icono a Inicio, devolverá false. De lo contrario, el usuario hizo clic en Sí, el icono se ancló y la API devolverá true.

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
* [Documentación de iconos adaptables](create-adaptive-tiles.md)

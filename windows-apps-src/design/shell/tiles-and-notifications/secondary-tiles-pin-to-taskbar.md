---
Description: Learn how to pin secondary tiles to taskbar.
title: Anclar iconos secundarios a la barra de tareas
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, uwp, anclar a la barra de tareas, iconos secundarios, anclar iconos secundarios a la barra de tareas, acceso directo
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8331764"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Anclar iconos secundarios a la barra de tareas

Al igual que el anclaje de iconos secundarios a Inicio, puedes anclar iconos secundarios a la barra de tareas, lo que da a los usuarios acceso rápido a contenido dentro de la aplicación.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API de acceso limitado**: esta API es una característica de acceso limitado. Para usar esta API, ponte en contacto con [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Requiere octubre de 2018 Update**: debes utilizar SDK 17763 y estar ejecutando la compilación 17763 o superior para anclar a la barra de tareas.


## <a name="guidance"></a>Guía

Un icono secundario proporciona una manera coherente y eficaz para que los usuarios acceder directamente a áreas específicas de una aplicación. Aunque el usuario elige si quiere "anclar" una ventana secundaria a la barra de tareas o no, las áreas puede anclables en una aplicación se determinan por el desarrollador. Para obtener más información, consulta las [instrucciones sobre los iconos secundarios](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. determinar si existe API y desbloquear acceso limitado

Dispositivos más antiguos no tienen la barra de tareas a la API de asignación (si te interesa versiones anteriores de Windows 10). Por lo tanto, no debe mostrar un botón de pin en estos dispositivos que no son capaces de anclaje.

Además, esta característica está bloqueada en acceso limitado. Para obtener acceso, ponte en contacto con Microsoft. Las llamadas de API **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** y **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** se producirá un error con una excepción de acceso denegado. Las aplicaciones no pueden usar esta API sin el permiso y la definición de la API puede cambiar en cualquier momento.

Usa el método [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) para determinar si las API están presentes. Y, a continuación, usar el **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API para intentar el desbloqueo de la API.

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. obtener la instancia de TaskbarManager

Las aplicaciones para UWP pueden ejecutarse en una amplia variedad de dispositivos; no todos ellos admiten la barra de tareas. Por ahora, solo los dispositivos de escritorio admiten la barra de tareas. Además, es posible que vienen presencia de la barra de tareas y se ve. Para comprobar si la barra de tareas está actualmente presente, llama al método **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** y comprobar que la instancia devuelta no es "null". No muestres un botón de pin si no está presente la barra de tareas.

Te recomendamos que sujeta la instancia de la duración de una sola operación, como anclaje y, a continuación, captura una nueva instancia de la próxima vez que debes hacer otra operación.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. comprueba si el icono está anclado actualmente a la barra de tareas

Si ya está anclado el icono, se debe mostrar un botón Desanclar en su lugar. Puedes usar el método **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** para comprobar si tu icono está anclado actualmente (los usuarios pueden sueltes en cualquier momento). En este método, se pasa el **TileId** de la ventana quieres saber está anclado.

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. comprueba si se permite la opción de asignación

Puede deshabilitar el anclaje a la barra de tareas mediante la directiva de grupo. La propiedad [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) le permite comprobar si se permite el anclaje.

Cuando el usuario hace clic en el botón de pin, debes comprobar esta propiedad y si es false, debe mostrar un cuadro de diálogo de mensaje que informa al usuario que no se permite la asignación en esta máquina.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. construir y anclar tu icono

El usuario ha hecho clic en el botón de pin y que hayas determinado que las API que están presentes, la barra de tareas está presente y anclaje puede … tiempo para anclar!

En primer lugar, construir tu icono secundario tal como lo harías al anclar a inicio. Puedes obtener más información sobre las propiedades del icono secundario mediante la lectura [Anclar iconos secundarios a inicio](secondary-tiles-pinning.md). Sin embargo, al anclar a la barra de tareas, además de las propiedades necesarias anteriormente, también es necesario Square44x44Logo (este es el logotipo de la barra de tareas). De lo contrario, se producirá una excepción.

A continuación, pasa el icono al método **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** . Dado que esto es bajo acceso limitado, esto no mostrará un cuadro de diálogo de confirmación y no requiere un subproceso de interfaz de usuario. Pero, en el futuro cuando esto se abre más allá de acceso limitado, los autores de llamadas acceso limitado, no utiliza recibirá un cuadro de diálogo y sea necesaria para usar el subproceso de interfaz de usuario.

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

Este método devuelve un valor booleano que indica si el icono está ahora anclado a la barra de tareas. Si ya se ha anclado tu icono, el método actualiza el icono existente y devuelve true. Si la asignación no se ha permitido o no se admite la barra de tareas, el método devuelve false.


## <a name="enumerate-tiles"></a>Enumerar los iconos

Para ver todas las ventanas que has creado y aún se anclan en algún lugar (inicio, la barra de tareas o ambos), usan **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Posteriormente, se puede comprobar si estos iconos están anclados a la barra de tareas o el inicio. Si no se admite la superficie, estos métodos devuelven falsos.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>Actualizar un icono

Para actualizar un icono ya anclado, puedes usar el método [**SecondaryTile.UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) tal como se describe en la [actualización de un icono secundario](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Desanclar un icono

La aplicación debe proporcionar un botón de Desanclar si el icono está anclado actualmente. Para desanclar el icono, basta con llamar a **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, pasando el **TileId** del icono secundario que te gustaría desanclado.

Este método devuelve un valor booleano que indica si el icono ya no está anclado a la barra de tareas. Si no se ha anclado tu icono en primer lugar, esto también devuelve true. Si no se ha permitido desanclen, esto devuelve false.

Dado que ya no está anclado en cualquier lugar, si solo se ha anclado tu icono a la barra de tareas, se eliminará el icono.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Eliminar un icono

Si quieres desanclar un icono en cualquier lugar (inicio, la barra de tareas), usa el método **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** .

Esto es apropiado para los casos donde el contenido anclado por el usuario ya no es aplicable. Por ejemplo, si la aplicación te permite anclar un bloc de notas a inicio y barra de tareas y, a continuación, el usuario elimina el Bloc de notas, simplemente debe eliminar el icono asociado con el Bloc de notas.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Desanclar solo desde el inicio

Si solo quieres desanclar un icono secundario desde Inicio dejando en la barra de tareas, llamar al método **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** . Del mismo modo se eliminará el icono si ya no está anclado a otras superficies.

Este método devuelve un valor booleano que indica si el icono ya no está anclado a inicio. Si no se ha anclado tu icono en primer lugar, esto también devuelve true. Si desanclen no estaba habilitado o no se admite el inicio, esto devuelve false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Recursos

* [Clase TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Anclar iconos secundarios a inicio](secondary-tiles-pinning.md)
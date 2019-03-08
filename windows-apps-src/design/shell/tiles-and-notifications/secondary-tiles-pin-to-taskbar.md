---
Description: Obtenga información sobre cómo anclar los iconos secundarios en la barra de tareas.
title: Anclar los iconos secundarios a la barra de tareas
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, uwp, anclar a la barra de tareas, el icono secundario, anclar los iconos secundarios en la barra de tareas, acceso directo
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591980"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Anclar los iconos secundarios a la barra de tareas

Al igual que anclar los iconos secundarios para que se inicie, puede anclar los iconos secundarios en la barra de tareas, proporcionar a los usuarios un acceso rápido al contenido dentro de la aplicación.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API de acceso limitado**: Esta API es una característica de acceso limitado. Para usar esta API, póngase en contacto con [ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Requiere la actualización de octubre de 2018**: Debe tener como destino el SDK 17763 y ejecutar compilación 17763 o posterior para anclar a la barra de tareas.


## <a name="guidance"></a>Instrucciones

Un mosaico secundario proporciona una manera coherente y eficaz para que los usuarios tener acceso directamente a áreas específicas dentro de una aplicación. Aunque un usuario elige "ancla" un icono secundario a la barra de tareas o no, las áreas en una aplicación anclables vienen determinadas por el desarrollador. Para obtener instrucciones, vea [orientación de mosaico secundario](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Determinar si existe API y desbloquear el acceso limitado

Los dispositivos más antiguos no tienen la barra de tareas anclar API (si tiene como destino versiones anteriores de Windows 10). Por lo tanto, no debería mostrar un botón de anclaje en estos dispositivos que no son capaces de anclaje.

Además, esta característica está bloqueada en acceso limitado. Para obtener acceso, póngase en contacto con Microsoft. Las llamadas API a  **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**,  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** y **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** se producirá una excepción de acceso denegado. Aplicaciones no pueden utilizar esta API sin permiso, y puede cambiar la definición de API en cualquier momento.

Use la [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) método para determinar si están presentes las API. Y, a continuación, utilice el **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API para intentar desbloquear la API.

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


## <a name="2-get-the-taskbarmanager-instance"></a>2. Obtener la instancia TaskbarManager

Las aplicaciones para UWP pueden ejecutarse en una amplia variedad de dispositivos; no todos ellos admiten la barra de tareas. Por ahora, solo los dispositivos de escritorio admiten la barra de tareas. Además, la presencia de la barra de tareas podría proceder y vaya. Para comprobar si la barra de tareas está presente actualmente, llame a la **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** método y compruebe que devuelve la instancia no es null. No mostrar un botón de anclaje si la barra de tareas no está presente.

Se recomienda la participación en la instancia para la duración de una sola operación, como anclado y, a continuación, tomar una nueva instancia de la próxima vez que necesite realizar otra operación.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Compruebe si el icono actualmente está anclado a la barra de tareas

Si ya está anclado el icono, debe mostrar un botón Desanclar en su lugar. Puede usar el **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** método para comprobar si actualmente está anclado el icono (los usuarios pueden Desanclar en cualquier momento). En este método, debe introducir el **TileId** está anclado del icono que desea saber.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. Compruebe si se permite la asignación

Anclar a la barra de tareas se puede deshabilitar mediante la directiva de grupo. El [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) propiedad le permite comprobar si se permite la asignación.

Cuando el usuario hace clic en el botón de anclaje, debe comprobar esta propiedad, y si es false, se debe mostrar un cuadro de diálogo de mensaje que informa al usuario que no se permite la asignación en esta máquina.

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


## <a name="5-construct-and-pin-your-tile"></a>5. Construir y anclar su icono

El usuario ha hecho clic el botón de anclaje y haya determinado que las API están presentes, barra de tareas está presente y se permite la asignación... hora de pin.

Primero, construya el icono secundario, tal como lo haría al anclar a inicio. Puede aprender más acerca de las propiedades del mosaico secundario mediante la lectura de [los iconos secundarios de anclar a inicio](secondary-tiles-pinning.md). Sin embargo, al anclar a la barra de tareas, además de las propiedades necesarias anteriormente, también es necesario Square44x44Logo (este es el logotipo de la barra de tareas). En caso contrario, se producirá una excepción.

A continuación, pase el icono para el **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** método. Puesto que se trata en acceso limitado, esto no mostrará un cuadro de diálogo de confirmación y no requiere que un subproceso de interfaz de usuario. Pero en el futuro cuando esto se abre más allá de acceso limitado, los llamadores no está utilizando acceso limitado recibirá un cuadro de diálogo y que tenga que usar el subproceso de interfaz de usuario.

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

Este método devuelve un valor booleano que indica si el icono ahora está anclado a la barra de tareas. Si ya se ancló el icono, el método actualiza el mosaico existente y devuelve true. Si la asignación no se permite o no se admite la barra de tareas, el método devuelve false.


## <a name="enumerate-tiles"></a>Enumerar los iconos

Para ver todos los iconos que ha creado y aún ancla en algún lugar (inicio, barra de tareas o ambos), use  **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Posteriormente, puede comprobar si estos iconos anclados a la barra de tareas o el inicio. Si no se admite la superficie, estos métodos devuelven falsos.

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

Para actualizar un mosaico ya anclado, puede usar el [ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) método tal como se describe en [actualizar un icono secundario](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Desanclar un mosaico

La aplicación debe proporcionar un botón Desanclar si actualmente está anclado el icono. Para desanclar el mosaico, basta con llamar a  **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, pasando el **TileId** del icono secundario que le gustaría desanclado.

Este método devuelve un valor booleano que indica si el icono ya no está anclado a la barra de tareas. Si el icono no se ha fijado en primer lugar, esto también devuelve true. Si no se puede Desanclar, devuelve false.

Si solo se ancló el icono en la barra de tareas, esta acción eliminará el icono puesto que ya no está anclado en cualquier lugar.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Eliminación de un icono

Si quiere desanclar un icono desde cualquier lugar (inicio, barra de tareas), use el **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** método.

Esto es apropiado para los casos donde el contenido al usuario anclado ya no es aplicable. Por ejemplo, si la aplicación le permite anclar a inicio y barra de tareas en un bloc de notas y, a continuación, el usuario elimina el Bloc de notas, simplemente debe eliminar el icono asociado con el Bloc de notas.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Desanclar solo desde el principio

Si solo desea desanclar un mosaico secundario desde el principio dejándolos en la barra de tareas, puede llamar a la **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** método. De forma similar, esta acción eliminará el icono si ya no está anclado a otras superficies.

Este método devuelve un valor booleano que indica si el icono ya no se ancla al inicio. Si el icono no se ha fijado en primer lugar, esto también devuelve true. Si no estaba permitido Desanclar o no se admite el inicio, se devuelve false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Recursos

* [Clase TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Iconos secundarios de anclar a inicio](secondary-tiles-pinning.md)
---
description: Obtenga información sobre cómo anclar iconos secundarios en la barra de tareas, lo que permite a los usuarios acceder rápidamente al contenido dentro de la aplicación.
title: Anclar iconos secundarios a la barra de tareas
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, UWP, anclar a la barra de tareas, icono secundario, anclar iconos secundarios a la barra de tareas, acceso directo
ms.localizationpriority: medium
ms.openlocfilehash: a57fa9c6a268b22df3c1772e0aec111c769d907b
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054195"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Anclar iconos secundarios a la barra de tareas

Al igual que anclar iconos secundarios al inicio, puede anclar iconos secundarios a la barra de tareas, lo que permite a los usuarios acceder rápidamente al contenido dentro de la aplicación.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API de acceso limitado**: esta API es una característica de acceso limitado. Para usar esta API, póngase en contacto con [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar) .

> **Requiere la actualización de octubre de 2018**: debe tener como destino el SDK 17763 y ejecutar la compilación 17763 o superior para anclar a la barra de tareas.


## <a name="guidance"></a>Instrucciones

Un icono secundario proporciona una forma coherente y eficaz de que los usuarios accedan directamente a áreas específicas de una aplicación. Aunque un usuario elige si "anclar" un icono secundario a la barra de tareas, el desarrollador determina las áreas ancladas de una aplicación. Para obtener más información, consulte la [Guía de iconos secundarios](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. determinar si existe la API y desbloquear el acceso limitado

Los dispositivos más antiguos no tienen las API de anclaje de la barra de tareas (si tiene como destino versiones anteriores de Windows 10). Por lo tanto, no debe mostrar un botón de anclaje en estos dispositivos que no sean capaces de anclar.

Además, esta característica está bloqueada con acceso limitado. Para obtener acceso, póngase en contacto con Microsoft. Las llamadas API a **[TaskbarManager. RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskbarManager. IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** y **[TaskbarManager. TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** producirán un error con una excepción de acceso denegado. Las aplicaciones no pueden usar esta API sin permiso y la definición de la API puede cambiar en cualquier momento.

Use el método [ApiInformation. IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) para determinar si las API están presentes. Y, a continuación, use la API **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** para probar el desbloqueo de la API.

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

Las aplicaciones de Windows se pueden ejecutar en una amplia variedad de dispositivos. no todas ellas admiten la barra de tareas. En este momento, solo los dispositivos de escritorio admiten la barra de tareas. Además, la presencia de la barra de tareas puede ser y go. Para comprobar si la barra de tareas está actualmente presente, llame al método **[TaskbarManager. GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** y compruebe que la instancia devuelta no sea NULL. No muestre un botón de anclaje si la barra de tareas no está presente.

Se recomienda mantener en la instancia mientras dure una sola operación, como anclar y, a continuación, capturar una nueva instancia la próxima vez que necesite realizar otra operación.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Compruebe si el icono está anclado a la barra de tareas.

Si el icono ya está anclado, debe mostrar un botón desanclar en su lugar. Puede usar el método **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** para comprobar si el icono está anclado actualmente (los usuarios pueden desanclarlo en cualquier momento). En este método, se pasa el **TileId** del icono que desea saber que está anclado.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. comprobar si se permite el anclaje

Directiva de grupo puede deshabilitar el anclaje en la barra de tareas. La propiedad [TaskbarManager. IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) le permite comprobar si se permite el anclaje.

Cuando el usuario hace clic en el botón anclar, debe activar esta propiedad y, si es false, debería mostrar un cuadro de diálogo de mensaje que informa al usuario de que no se permite el anclaje en este equipo.

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


## <a name="5-construct-and-pin-your-tile"></a>5. construir y anclar el icono

El usuario ha hecho clic en el botón anclar y ha determinado que las API están presentes, la barra de tareas está presente y se permite el anclaje... tiempo para anclar

En primer lugar, construya el icono secundario tal como lo haría al anclar a Inicio. Puede obtener más información sobre las propiedades de los iconos secundarios leyendo los [iconos secundarios de PIN para iniciar](secondary-tiles-pinning.md). Sin embargo, al anclar a la barra de tareas, además de las propiedades necesarias anteriormente, también se requiere Square44x44Logo (este es el logotipo que usa la barra de tareas). De lo contrario, se producirá una excepción.

A continuación, pase el icono al método **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** . Como esto está en acceso limitado, no se mostrará un cuadro de diálogo de confirmación y no se requiere un subproceso de interfaz de usuario. Pero en el futuro, cuando se abre más allá del acceso limitado, los autores de la llamada que no usen acceso limitado recibirán un cuadro de diálogo y serán necesarios para usar el subproceso de la interfaz de usuario.

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

Este método devuelve un valor booleano que indica si el icono ahora está anclado a la barra de tareas. Si el icono ya estaba anclado, el método actualiza el icono existente y devuelve true. Si no se permite el anclaje o no se admite la barra de tareas, el método devuelve false.


## <a name="enumerate-tiles"></a>Enumerar iconos

Para ver todos los mosaicos que creó y siguen anclados en algún lugar (Inicio, barra de tareas o ambos), use **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Posteriormente, puede comprobar si estos mosaicos están anclados a la barra de tareas o iniciar. Si no se admite la superficie, estos métodos devuelven false.

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

Para actualizar un icono ya anclado, puede usar el método [**SecondaryTile. UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) tal y como se describe en [actualizar un icono secundario](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Desanclar un icono

La aplicación debe proporcionar un botón desanclar si el icono está anclado actualmente. Para desanclar el icono, simplemente llame a **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, pasando el **TileId** del icono secundario que le gustaría desanclar.

Este método devuelve un valor booleano que indica si el icono ya no está anclado a la barra de tareas. Si el icono no se ancló en primer lugar, también devuelve true. Si no se permite desanclar, se devuelve false.

Si el icono solo está anclado a la barra de tareas, se eliminará el icono, ya que ya no está anclado en ningún lugar.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Eliminar un icono

Si desea desanclar un icono desde cualquier lugar (Inicio, barra de tareas), use el método **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** .

Esto es adecuado para los casos en los que el contenido anclado por el usuario ya no es aplicable. Por ejemplo, si la aplicación le permite anclar un bloc de notas a Inicio y a la barra de tareas, y el usuario elimina el cuaderno, simplemente debe eliminar el icono asociado al cuaderno.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Desanclar solo de inicio

Si solo desea desanclar un icono secundario de Start mientras lo deja en la barra de tareas, puede llamar al método **[StartScreenManager. TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** . De igual forma, se eliminará el icono si deja de estar anclado a otras superficies.

Este método devuelve un valor booleano que indica si el icono ya no está anclado al inicio. Si el icono no se ancló en primer lugar, también devuelve true. Si no se permite el desanclado o no se admite el inicio, devuelve false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Recursos

* [Clase TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Anclar iconos secundarios a Inicio](secondary-tiles-pinning.md)

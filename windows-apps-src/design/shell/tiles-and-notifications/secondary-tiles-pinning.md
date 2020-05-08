---
Description: Obtenga información sobre cómo anclar un icono secundario para que se inicie desde la aplicación de Windows.
title: Anclar iconos secundarios al inicio
label: Pin secondary tiles to Start
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, iconos secundarios, PIN, anclaje, Inicio rápido, ejemplo de código, ejemplo, secondarytile
ms.localizationpriority: medium
ms.openlocfilehash: 8c535e7e2abaf68212cb0a2f6daac8741b6548a5
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971050"
---
# <a name="pin-secondary-tiles-to-start"></a>Anclar iconos secundarios al inicio


Este tema le guía por los pasos necesarios para crear un icono secundario para su aplicación de Windows y anclarlo al menú Inicio.

![Captura de pantalla de iconos secundarios](images/secondarytiles.png)

Para obtener más información sobre los iconos secundarios, consulte la [información general sobre los mosaicos secundarios](secondary-tiles.md).


## <a name="add-namespace"></a>Agregar espacio de nombres

El espacio de nombres Windows. UI. StartScreen incluye la clase SecondaryTile.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Inicializar el icono secundario

Los mosaicos secundarios se componen de algunos componentes clave...

* **TileId**: un identificador único que le permite identificar el icono entre los otros mosaicos secundarios.
* **DisplayName**: el nombre que desea que se muestre en el icono.
* **Arguments**: los argumentos que desea que se devuelvan a la aplicación cuando el usuario haga clic en el icono.
* **Square150x150Logo**: el logotipo necesario, que se muestra en el icono de tamaño medio (y cuyo tamaño se ha cambiado a mosaico pequeño si no se ha proporcionado ningún logotipo pequeño).

**Debe** proporcionar valores inicializados para todas las propiedades anteriores; de lo contrario, se producirá una excepción.

Hay una variedad de constructores que se pueden usar, pero usar el constructor que toma tileId, displayName, Arguments, square150x150Logo y desiredSize ayuda a garantizar que se establezcan todas las propiedades necesarias.

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>Opcional: agregar compatibilidad con tamaños de icono mayores

Si va a mostrar las notificaciones de icono enriquecidas en el icono secundario, es probable que desee permitir que el usuario cambie el tamaño del icono a ancho o grande, para que pueda ver incluso más contenido.

Para habilitar tamaños de iconos anchos y grandes, debe proporcionar *Wide310x150Logo* y *Square310x310Logo*. Además, si es posible, debe proporcionar el valor de *Square71x71Logo* para el tamaño de mosaico pequeño (de lo contrario, se reducir la Square150x150Logo necesaria para el icono pequeño).

También puede proporcionar un *Square44x44Logo*único, que se muestra opcionalmente en la esquina inferior derecha cuando hay una notificación. En su lugar, si no se proporciona ninguno, se usará el *Square44x44Logo* del icono principal.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Opcional: habilitar la visualización del nombre para mostrar

De forma predeterminada, no se mostrará el nombre para mostrar. Para mostrar el nombre para mostrar en medio/ancho/grande, agregue el código siguiente.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Opcional: iconos secundarios 3D
Puede mejorar el icono secundario para Windows Mixed Reality agregando recursos 3D. Los usuarios pueden colocar iconos 3D directamente en su página principal de Windows Mixed Reality en lugar de en el menú Inicio cuando se usa la aplicación en un entorno de realidad mixta. Por ejemplo, puede crear fotoesferas de 360 ° que se vinculan directamente a una aplicación de visor de fotos de 360 °, o bien permitir que los usuarios coloquen un modelo 3D de una silla de un catálogo de mobiliario que abra una página de detalles sobre las opciones de precios y de color de ese objeto cuando se seleccionan. Para empezar, consulte la documentación para [desarrolladores de realidad mixta](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Anclar el icono secundario

Por último, solicite el anclaje del icono. Tenga en cuenta que se debe llamar a este método desde un subproceso de interfaz de usuario. En el escritorio, aparecerá un cuadro de diálogo que le pide al usuario que confirme si desea anclar el icono.

> [!IMPORTANT]
> Si es una aplicación de escritorio de Windows que usa el puente de escritorio, primero debe realizar un paso adicional, como se describe en [anclar desde la aplicación de escritorio](secondary-tiles-desktop-pinning.md) .

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Comprobar si existe un icono secundario

Si el usuario visita una página de la aplicación que ya está anclada para iniciarse, querrá mostrar un botón "desanclar".

Por lo tanto, al elegir qué botón Mostrar, debe comprobar primero si el icono secundario está anclado actualmente.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Desanclar un icono secundario

Si el icono está anclado actualmente y el usuario hace clic en el botón desanclar, querrá desanclar (eliminar) el icono.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Actualización de un icono secundario

Si necesita actualizar los logotipos, el nombre para mostrar o cualquier otro elemento en el icono secundario, puede usar *RequestUpdateAsync*.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Enumerar todos los mosaicos secundarios anclados

Si necesita detectar todos los iconos anclados por el usuario, en lugar de usar *SecondaryTile. exists*, también puede usar *SecondaryTile. FindAllAsync ()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Enviar una notificación de icono

Para obtener información sobre cómo mostrar contenido enriquecido en el icono a través de notificaciones de icono, consulte [enviar una notificación de icono local](sending-a-local-tile-notification.md).


## <a name="related"></a>Temas relacionados

* [Información general sobre mosaicos secundarios](secondary-tiles.md)
* [Guía de mosaicos secundarios](secondary-tiles-guidance.md)
* [Activos en mosaico](app-assets.md)
* [Documentación de contenido de mosaico](create-adaptive-tiles.md)
* [Enviar una notificación de icono local](sending-a-local-tile-notification.md)

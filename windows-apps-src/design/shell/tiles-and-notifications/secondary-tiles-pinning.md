---
author: andrewleader
Description: Learn how to pin a secondary tile from your UWP app.
title: Anclar iconos secundarios
label: Pin secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, uwp, iconos secundarios, anclar, anclado, inicio rápido, ejemplo de código, ejemplo, secondarytile
ms.localizationpriority: medium
ms.openlocfilehash: 7fcea65a43ec3ca3d7e29056bec129e0f03d4229
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6647249"
---
# <a name="pin-secondary-tiles"></a>Anclar iconos secundarios


Este tema te guiará por pasos a dar para crear un icono secundario para tu aplicación para UWP y anclarlo al menú Inicio.

![Captura de pantalla de iconos secundarios](images/secondarytiles.png)

Para obtener más información sobre iconos secundarios, consulta [Introducción a los iconos secundarios](secondary-tiles.md).


## <a name="add-namespace"></a>Añadir espacio de nombres

El espacio de nombres Windows.UI.StartScreen incluye la clase SecondaryTile.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Inicializar el icono secundario

Los iconos secundarios se componen de algunos componentes clave...

* **TileId**: identificador único que permite identificar el icono entre los demás iconos secundarios.
* **DisplayName**: nombre que quieras que se muestre en el icono.
* **Arguments**: copia de los argumentos que quieras devolver a la aplicación cuando el usuario haga clic en el icono.
* **Square150x150Logo**: logotipo obligatorio, mostrado en el icono de tamaño mediano (y cambiado de tamaño para iconos de tamaño pequeño si no se proporciona un logotipo pequeño).

**DEBES** proporcionar valores inicializados para todas las propiedades anteriores, ya que en caso contrario obtendrás una excepción.

Hay una gran variedad de constructores que puedes usar, pero usar el constructor que toma tileId, displayName, arguments, square150x150Logo y desiredSize ayuda a garantizar que establecerás todas las propiedades necesarias.

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


## <a name="optional-add-support-for-larger-tile-sizes"></a>Opcional: Agregar compatibilidad para tamaños de icono mayores

Si vas a mostrar notificaciones de icono enriquecidas en el icono secundario, probablemente querrás que el usuario pueda cambiar el tamaño de su ventana para que sea ancha o grande, para poder ver más parte de tu contenido.

Para habilitar los tamaños de icono ancho y grande, debes proporcionar *Wide310x150Logo* y *Square310x310Logo*. Además, si es posible, debes proporcionar el *Square71x71Logo* para el tamaño de icono pequeño (de lo contrario, se reducirá el tamaño de tu Square150x150Logo requerido para adecuarse al icono pequeño).

También puedes proporcionar un único *Square44x44Logo*, que, opcionalmente, se muestra en la esquina inferior derecha cuando está presente una notificación. Si no proporcionas uno, se usará en su lugar el *Square44x44Logo* de tu icono principal.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Opcional: Habilitar presentar el nombre para mostrar

De manera predeterminada, NO se presentará el nombre para mostrar. Para presentar el nombre para mostrar en modo mediano/ancho/grande, agrega el siguiente código.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Opcional: iconos secundarios 3D
Puedes mejorar tu icono secundario para Windows Mixed Reality al agregar los activos 3D. Los usuarios pueden colocar iconos 3D directamente en su página principal de Windows Mixed Reality en lugar del menú Inicio al usar tu aplicación en un entorno de realidad mixta. Por ejemplo, puedes crear fotoesferas de 360° que vinculen directamente a una aplicación de visualizador de fotos de 360° o bien, dejar que los usuarios coloquen un modelo 3D de una silla de un catálogo de muebles que abra una página de detalles sobre las opciones de precios y colores para ese objeto cuando se selecciona. Para empezar, consulta la [documentación para desarrolladores de realidad mixta](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Anclar el icono secundario

Por último, solicita anclar el icono. Ten en cuenta que debe llamarse desde un subproceso de interfaz de usuario. En el escritorio, aparecerá un cuadro de diálogo pidiendo al usuario que confirme si quiere anclar el icono.

> [!IMPORTANT]
> Si tienes una aplicación de escritorio de Windows que usa el Puente de dispositivo de escritorio, primero debes seguir un paso adicional, conforme se describe en [Anclar desde aplicación de escritorio](secondary-tiles-desktop-pinning.md)

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Comprobar si existe un icono secundario

Si el usuario visita una página de la aplicación que ya ha anclado a Inicio, querrás mostrar un botón de "Desanclar" en lugar del anterior.

Por lo tanto, al elegir qué botón mostrar, deberás primero comprobar si el icono secundario está anclado actualmente.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Desanclaje de un icono secundario

Si el icono está actualmente anclado y el usuario hace clic en el botón de desanclar, querrás desanclar (eliminar) el icono.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Actualización de un icono secundario

Si necesitas actualizar los logotipos, el nombre para mostrar o cualquier otra cosa del icono secundario, puedes usar *RequestUpdateAsync*.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Enumerar todos los iconos secundarios anclados

Si es necesario conocer todos los iconos que el usuario ha anclado, en lugar de usar *SecondaryTile.Exists*, puedes usar como alternativa *SecondaryTile.FindAllAsync()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Enviar una notificación de icono

Para obtener información sobre cómo mostrar contenido enriquecido en el icono mediante notificaciones de icono, consulta [Enviar una notificación de icono local](sending-a-local-tile-notification.md).


## <a name="related"></a>Relacionados

* [Información general sobre iconos secundarios](secondary-tiles.md)
* [Instrucciones sobre iconos secundarios](secondary-tiles-guidance.md)
* [Activos de icono](app-assets.md)
* [Documentación del contenido de iconos](create-adaptive-tiles.md)
* [Enviar una notificación de icono local](sending-a-local-tile-notification.md)

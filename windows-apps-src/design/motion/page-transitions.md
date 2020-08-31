---
title: Transiciones de página
description: Obtenga información sobre cómo usar las transiciones de páginas de Plataforma universal de Windows (UWP) para proporcionar a los usuarios información sobre la relación entre las páginas de la aplicación.
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c77f99e170bdfe6689a9bfd4e8d8075ec2154d28
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094682"
---
# <a name="page-transitions"></a>Transiciones de página

Las transiciones de página navegan por los usuarios entre las páginas de una aplicación y proporcionan comentarios como la relación entre las páginas. Las transiciones de página ayudan a los usuarios a entender si están en la parte superior de una jerarquía de navegación, se mueven entre páginas relacionadas o navegan más en la jerarquía de la página.

Se proporcionan dos animaciones diferentes para la navegación entre las páginas de una aplicación, la *actualización de páginas* y la *obtención de detalles*, y se representan mediante subclases de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene instalada la aplicación de <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> , haga clic aquí para <a href="xamlcontrolsgallery:/item/PageTransition">abrir la aplicación y ver las transiciones de página en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>Actualización de página

La actualización de página es una combinación de una animación de deslizamiento hacia arriba y una animación de fundido para el contenido entrante. Use la actualización de página cuando el usuario se desplace a la parte superior de una pila de navegación, como navegar entre las pestañas o los elementos de navegación a la izquierda.

Lo que se desea es que el usuario haya empezado a trabajar.

![animación de actualización de página](images/page-refresh.gif)

La animación de actualización de página se representa mediante [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Nota**: un [**marco**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) usa automáticamente [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) para animar la navegación entre dos páginas. De forma predeterminada, la animación es la actualización de la página.

## <a name="drill"></a>Detalles

Usar la obtención de detalles cuando los usuarios navegan más en una aplicación, como mostrar más información después de seleccionar un elemento.

Lo que se desea es que el usuario haya profundizado en la aplicación.

![animación de obtención de detalles](images/drill.gif)

La animación de obtención de detalles se representa mediante la clase [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) .

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Diapositiva horizontal

Use la diapositiva horizontal para mostrar que las páginas relacionadas aparecen una junto a la otra. El control [NavigationView](../controls-and-patterns/navigationview.md) usa automáticamente esta animación para la navegación superior, pero si va a crear su propia experiencia de navegación horizontal, puede implementar la diapositiva horizontal con SlideNavigationTransitionInfo.

Lo que se desea es que el usuario navegue entre las páginas que se encuentran junto entre sí. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Suprimir

Para evitar la reproducción de cualquier animación durante la navegación, use [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) en lugar de otros subtipos de **NavigationTransitionInfo** .

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

Suprimir la animación resulta útil si va a crear su propia transición mediante [animaciones conectadas](connected-animation.md) o animaciones de mostrar u ocultar implícitas.

## <a name="backwards-navigation"></a>Navegación hacia atrás

Puede usar `Frame.GoBack(NavigationTransitionInfo)` para reproducir una transición específica al navegar hacia atrás.

Esto puede ser útil cuando se modifica el comportamiento de navegación dinámicamente basándose en el tamaño de la pantalla. por ejemplo, en un escenario de maestro y detalles con capacidad de respuesta.

## <a name="related-topics"></a>Temas relacionados

- [Navegar entre dos páginas](../basics/navigate-between-two-pages.md)
- [Movimiento en aplicaciones de UWindowsWP](index.md)

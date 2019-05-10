---
Description: Obtenga información sobre cómo usar las transiciones de página en sus aplicaciones para UWP.
title: Transiciones de página en aplicaciones para UWP
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9b3244c24ff4fa8e3c85ee9970536b1b35d8efd5
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444191"
---
# <a name="page-transitions"></a>Transiciones de página

Las transiciones de página sirven para que los usuarios se desplacen entre las páginas de una aplicación, proporcionando comentarios como la relación entre las páginas. Las transiciones de página ayudan a los usuarios a entender si están en la parte superior de una jerarquía de navegación, moviéndose entres las páginas del mismo nivel o adentrándose en la jerarquía de páginas.

Se proporcionan dos animaciones diferentes para la navegación entre páginas en una aplicación, *Actualización de página* y *Drill*, y se representan mediante subclases de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/PageTransition">abra la aplicación y ver las transiciones de página en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>Actualización de página

La actualización de página es una combinación de una animación deslizante y una atenuación en animación del contenido entrante. Usa la actualización de página cuando el usuario sea llevado a la parte superior de una pila de navegación, como la navegación entre los elementos tabs o left-nav.

La sensación deseada es que el usuario ha comenzado de nuevo.

![animación de actualización de página](images/page-refresh.gif)

La animación de actualización de página se representa mediante [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Nota**: Un [ **marco** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) usa automáticamente [ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) para animar la navegación entre las dos páginas. De manera predeterminada, la animación tiene actualización de página.

## <a name="drill"></a>Drill

Usa drill cuando los usuarios se adentren en una aplicación, como mostrar más información después de seleccionar un elemento.

La sensación deseada es que el usuario se ha adentrado en la aplicación.

![animación de drill](images/drill.gif)

La animación de drill se representa mediante la clase [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Presentación horizontal

Utilice diapositiva horizontal para mostrar que las páginas relacionadas aparecen junto a la otra. El [NavigationView](../controls-and-patterns/navigationview.md) control usa automáticamente esta animación de navegación superior, pero si va a crear su propia experiencia de navegación horizontal, puede implementar diapositiva horizontal con SlideNavigationTransitionInfo.

El sentimiento deseado es que el usuario se desplaza entre las páginas que están cerca entre sí. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Suprimir

Para evitar reproducir cualquier animación durante la navegación, usa [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) en lugar de otros subtipos **NavigationTransitionInfo**.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

Suprimir la animación es útil si vas a crear tu propia transición con [animaciones conectadas](connected-animation.md) o animaciones de mostrar u ocultar implícitas.

## <a name="backwards-navigation"></a>Navegación hacia atrás

Puedes usar `Frame.GoBack(NavigationTransitionInfo)` para reproducir una transición específica al volver atrás.

Esto puede ser útil cuando modifiques el comportamiento de la navegación de forma dinámica en función del tamaño de la pantalla, por ejemplo, en un escenario maestro/de detalles con capacidad de respuesta.

## <a name="related-topics"></a>Temas relacionados

- [Desplazarse entre las dos páginas](../basics/navigate-between-two-pages.md)
- [Movimiento de las aplicaciones para UWP](index.md)

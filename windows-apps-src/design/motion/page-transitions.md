---
author: serenaz
Description: Learn how to use page transitions in your UWP apps.
title: Transiciones de página en aplicaciones para UWP
template: detail.hbs
ms.author: sezhen
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: cba05cd9106d64f443e87b1e8373b2501d0ce451
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842242"
---
# <a name="page-transitions"></a>Transiciones de página

Las transiciones de página sirven para que los usuarios se desplacen entre las páginas de una aplicación, proporcionando comentarios como la relación entre las páginas. Las transiciones de página ayudan a los usuarios a entender si están en la parte superior de una jerarquía de navegación, moviéndose entres las páginas del mismo nivel o adentrándose en la jerarquía de páginas.

Se proporcionan dos animaciones diferentes para la navegación entre páginas en una aplicación, *Actualización de página* y *Drill*, y se representan mediante subclases de [**NavigationTransitionInfo**](/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="page-refresh"></a>Actualización de página

La actualización de página es una combinación de una animación deslizante y una atenuación en animación del contenido entrante. Usa la actualización de página cuando el usuario sea llevado a la parte superior de una pila de navegación, como la navegación entre los elementos tabs o left-nav.

La sensación deseada es que el usuario ha comenzado de nuevo.

![animación de actualización de página](images/page-refresh.gif)

La animación de actualización de página se representa mediante [**EntranceNavigationTransitionInfoClass**](/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Nota**: Un [**Frame**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) usa automáticamente [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) para animar la navegación entre dos páginas. De manera predeterminada, la animación tiene actualización de página.

## <a name="drill"></a>Drill

Usa drill cuando los usuarios se adentren en una aplicación, como mostrar más información después de seleccionar un elemento.

La sensación deseada es que el usuario se ha adentrado en la aplicación.

![animación de drill](images/drill.gif)

La animación de drill se representa mediante la clase [**DrillInNavigationTransitionInfo**](/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>Suprimir

Para evitar reproducir cualquier animación durante la navegación, usa [**SuppressNavigationTransitionInfo**](/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) en lugar de otros subtipos **NavigationTransitionInfo**.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

Suprimir la animación es útil si vas a crear tu propia transición con [animaciones conectadas](connected-animation.md) o animaciones de mostrar u ocultar implícitas.

## <a name="backwards-navigation"></a>Navegación hacia atrás

Puedes usar `Frame.GoBack(NavigationTransitionInfo)` para reproducir una transición específica al volver atrás.

Esto puede ser útil cuando modifiques el comportamiento de la navegación de forma dinámica en función del tamaño de la pantalla, por ejemplo, en un escenario maestro/de detalles con capacidad de respuesta.

## <a name="related-topics"></a>Temas relacionados

- [Navegar entre dos páginas](../basics/navigate-between-two-pages.md)
- [Movimiento en las aplicaciones para UWP](index.md)
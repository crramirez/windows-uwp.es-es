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
ms.openlocfilehash: dc42199eba00071f5dbabd83a4ae524298a619ee
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818553"
---
# <a name="page-transitions"></a>Transiciones de página

Las transiciones de página son animaciones que se reproducen para que los usuarios se desplacen entre las páginas de una aplicación, proporcionando comentarios como la relación entre las páginas. Las transiciones de página ayudan a los usuarios a entender si están en la parte superior de una jerarquía de navegación, moviéndose entres las páginas del mismo nivel o adentrándose en la jerarquía de páginas.

Se proporcionan dos animaciones diferentes para la navegación entre páginas en una aplicación, *actualización de página* y *drill*, y se representan mediante subclases de [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="page-refresh"></a>Actualización de página

La actualización de página es una combinación de una animación deslizante y una atenuación en animación del contenido entrante. La sensación deseada es que el usuario ha comenzado de nuevo.

Usa la actualización de página cuando el usuario sea llevado a la parte superior de una pila de navegación, como la navegación entre los elementos [tabs](../controls-and-patterns/tabs-pivot.md) o [left-nav](../controls-and-patterns/navigationview.md). De manera predeterminada, [Frame.Navigate ()](/uwp/api/windows.ui.xaml.controls.frame.navigate) usa la actualización de página.

![animación de actualización de página](images/page-refresh.gif)

La animación de actualización de página se representa mediante [EntranceNavigationTransitionInfoClass](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());
```

## <a name="drill"></a>Drill

Usa drill cuando los usuarios se adentren en una aplicación, como mostrar más información después de seleccionar un elemento.

La sensación deseada es que el usuario se ha adentrado en la aplicación.

![animación de drill](images/drill.gif)

La animación de drill se representa mediante la clase [DrillInNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>Suprimir

Suprimir la animación es útil si vas a crear tu propia transición con [animaciones conectadas](connected-animation.md) o animaciones de mostrar u ocultar implícitas.

Para evitar reproducir cualquier animación durante la navegación, usa [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) en lugar de otros subtipos [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

## <a name="backwards-navigation"></a>Navegación hacia atrás

De manera predeterminada, [Frame.GoBack()](/uwp/api/windows.ui.xaml.controls.frame.goback) reproduce la correspondiente animación "volver" en función de la animación reproducida para ir a la página. Por ejemplo, una aplicación que usa drill para ir a una página verá un drill out cuando los usuarios vuelvan atrás.

Para reproducir una transición específica al volver atrás, usa `Frame.GoBack(NavigationTransitionInfo)`. Esto puede ser útil cuando modifiques el comportamiento de la navegación de forma dinámica en función del tamaño de la pantalla, por ejemplo, en un escenario maestro/de detalles con capacidad de respuesta.

## <a name="related-topics"></a>Artículos relacionados

- [Navegar entre dos páginas](../basics/navigate-between-two-pages.md)
- [Movimiento en las aplicaciones para UWP](index.md)

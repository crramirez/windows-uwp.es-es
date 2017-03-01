---
author: Jwmsft
title: Vista en dos paneles
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido."
label: Split view
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b4f0c84c4fdf273e7ddf2c16e3323ad0c4e52359
ms.lasthandoff: 02/07/2017

---
# <a name="split-view-control"></a>Control de vista en dos paneles

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Clase SplitView**](https://msdn.microsoft.com/library/windows/apps/dn864360)</li>
</ul>
</div>

Este es un ejemplo de la aplicación Microsoft Edge con SplitView para mostrar su hub.

![Ejemplo de vista en dos paneles de Microsoft Edge](images/split_view_Edge.png)


 Un área de contenido de vista en dos paneles siempre está visible. El panel puede expandirse y contraerse, o permanecer en un estado abierto, y puede presentarse desde el lado izquierdo o desde el lado derecho de una ventana de la aplicación. El panel tiene cuatro modos:

-   **Superposición**

    El panel está oculto hasta que se abre. Cuando se abre, el panel se superpone al área de contenido.

-   **En línea**

    El panel siempre está visible y no se superpone al área de contenido. Las áreas del panel y el contenido dividen la superficie disponible de la pantalla.

-   **CompactOverlay**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48 px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, se superpondrá al área de contenido.

-   **CompactInline**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48 px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, reducirá el espacio disponible para el contenido para apartarlo de su camino.

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

El control de vista en dos paneles puede usarse para crear un [panel de navegación](nav-pane.md). Para crear este patrón, agrega un botón de expandir/contraer (botón de "hamburguesa") y una vista de lista que represente los elementos de navegación.

El control de vista en dos paneles también puede usarse para crear cualquier experiencia de "cajón" donde los usuarios puedan abrir y cerrar el panel complementario.

## <a name="create-a-split-view"></a>Crear una vista en dos paneles

Este es un control SplitView con un panel abierto que aparece en línea junto al contenido.
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```



## <a name="related-topics"></a>Temas relacionados
* [Patrón de panel de navegación](nav-pane.md)
* [Vista de lista](lists.md)
 

 


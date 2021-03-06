---
title: Vista en dos paneles
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido.
label: Split view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c942c5748ef1d62dc86f6fa3d041bae651167f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173909"
---
# <a name="split-view-control"></a>Control de vista en dos paneles

Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido.

> **API importantes**: [Clase SplitView](/uwp/api/Windows.UI.Xaml.Controls.SplitView)

Este es un ejemplo de la aplicación Microsoft Edge con SplitView para mostrar su hub.

![Ejemplo de vista en dos paneles de Microsoft Edge](images/split_view_Edge.png)


 Un área de contenido de vista en dos paneles siempre está visible. El panel puede expandirse y contraerse, o permanecer en un estado abierto, y puede presentarse desde el lado izquierdo o desde el lado derecho de una ventana de la aplicación. El panel tiene cuatro modos:

-   **Overlay**

    El panel está oculto hasta que se abre. Cuando se abre, el panel se superpone al área de contenido.

-   **Inline**

    El panel siempre está visible y no se superpone al área de contenido. Las áreas del panel y el contenido dividen la superficie disponible de la pantalla.

-   **CompactOverlay**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48 px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, se superpondrá al área de contenido.

-   **CompactInline**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48 px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, reducirá el espacio disponible para el contenido para apartarlo de su camino.

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

El control de vista dividida se puede usar para crear cualquier experiencia de "cajón" donde los usuarios puedan abrir y cerrar el panel complementario. Por ejemplo, puedes usar SplitView para generar el patrón [maestro y detalles](master-details.md).

Si quieres crear un menú de navegación con un botón de expandir/contraer y una lista de elementos de navegación, utiliza el control [NavigationView](navigationview.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/SplitView">abrirla y ver SplitView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

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

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados
- [Patrón de panel de navegación](navigationview.md)
- [Vista de lista](lists.md)
- [Maestro/detalles](master-details.md)
---
author: Jwmsft
title: Vista en dos paneles
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido."
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 7fae1477b997508ade92a5bbb977c1d6530a181f

---
# Control de vista en dos paneles

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn864360"><strong>Clase SplitView (XAML)</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn919970"><strong>Objeto SplitView (HTML)</strong></a></li>
</ul>

</div>
</div>




 Un área de contenido de vista en dos paneles siempre está visible. El panel puede expandirse y contraerse, o permanecer en un estado abierto, y puede presentarse desde el lado izquierdo o desde el lado derecho de una ventana de la aplicación. El panel tiene cuatro modos:

-   **Superposición**

    El panel está oculto hasta que se abre. Cuando se abre, el panel se superpone al área de contenido.

-   **En línea**

    El panel siempre está visible y no se superpone al área de contenido. Las áreas del panel y el contenido dividen la superficie disponible de la pantalla.

-   **CompactOverlay**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, se superpondrá al área de contenido.

-   **CompactInline**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, reducirá el espacio disponible para el contenido para apartarlo de su camino.

## ¿Es este el control adecuado?

El control de vista en dos paneles puede usarse para crear un [panel de navegación](nav-pane.md). Para crear este patrón, agrega un botón de expandir/contraer (botón de "hamburguesa") y una vista de lista que represente los elementos de navegación.

El control de vista en dos paneles también puede usarse para crear cualquier experiencia de "cajón" donde los usuarios puedan abrir y cerrar el panel complementario.

## Ejemplos

El control de vista en dos paneles en su forma predeterminada es un contenedor básico. Este es un ejemplo de la aplicación Microsoft Edge con SplitView para mostrar su hub.

![Ejemplo de vista en dos paneles de Microsoft Edge](images/split_view_Edge.png)



## Temas relacionados


* [Patrón de panel de navegación](nav-pane.md)
* [Vista de lista](lists.md)
 

 



<!--HONumber=Aug16_HO3-->



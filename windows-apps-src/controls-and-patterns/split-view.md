---
author: Jwmsft
title: Vista en dos paneles
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido."
label: Split view
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 391bfdbbf09474ad707dbbf306d4997825fa8386

---

# Directrices para el control SplitView



**API importantes**

-   [**Clase SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objeto SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido. El área de contenido siempre está visible. El panel puede expandirse y contraerse, o permanecer en un estado abierto, y puede presentarse desde el lado izquierdo o desde el lado derecho de una ventana de la aplicación. El panel tiene cuatro modos:

-   **Superposición**

    El panel está oculto hasta que se abre. Cuando se abre, el panel se superpone al área de contenido.

-   **En línea**

    El panel siempre está visible y no se superpone al área de contenido. Las áreas del panel y el contenido dividen la superficie disponible de la pantalla.

-   **CompactOverlay**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, se superpondrá al área de contenido.

-   **CompactInline**

    Una parte estrecha del panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos. El ancho del panel cerrado predeterminado es de 48px, que se puede modificar con `CompactPaneLength`. Si el panel se abre, reducirá el espacio disponible para el contenido para apartarlo de su camino.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>¿Es este el control adecuado?

El control de vista en dos paneles puede usarse para crear un [panel de navegación](nav-pane.md). Para crear este patrón, agrega un botón de expandir/contraer (botón de "hamburguesa") y una vista de lista que represente los elementos de navegación.

El control de vista en dos paneles también puede usarse para crear cualquier experiencia de "cajón" donde los usuarios puedan abrir y cerrar el panel complementario.

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Ejemplos

El control de vista en dos paneles en su forma predeterminada es un contenedor básico. Este es un ejemplo de la aplicación Microsoft Edge con SplitView para mostrar su hub.

![Ejemplo de vista en dos paneles de Microsoft Edge](images/split_view_Edge.png)



## <span id="related_topics"></span>Temas relacionados


* [Patrón de panel de navegación](nav-pane.md)
* [Vista de lista](lists.md)
 

 



<!--HONumber=Jun16_HO4-->



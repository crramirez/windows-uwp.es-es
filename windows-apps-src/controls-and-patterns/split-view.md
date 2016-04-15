---
title: Vista en dos paneles
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido.
label: Vista en dos paneles
template: detail.hbs
---

# Directrices para el control de vista en dos paneles


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Clase SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objeto SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Un control de vista en dos paneles tiene un panel expandible y contraíble y un área de contenido. El área de contenido siempre está visible. El panel puede expandirse y contraerse, o permanecer en un estado abierto, y puede presentarse desde el lado izquierdo o desde el lado derecho de una ventana de la aplicación. El panel tiene tres modos:

-   **Superposición**

    El panel está oculto hasta que se abre. Cuando se abre, el panel se superpone al área de contenido.

-   **En línea**

    El panel siempre está visible y no se superpone al área de contenido. Las áreas del panel y el contenido dividen la superficie disponible de la pantalla.

-   **Compacto**

    El panel siempre está visible en este modo, que es lo suficientemente ancho como para mostrar iconos (normalmente de 48 epx de ancho). Las áreas del panel y el contenido dividen la superficie disponible de la pantalla. Aunque el modo compacto estándar no se superpone el área de contenido, puede transformarse en un panel más amplio para mostrar más contenido que se superponga al área de contenido.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>¿Es este el control adecuado?


El control de vista en dos paneles puede usarse para crear un [patrón de panel de navegación](nav-pane.md). Para crear este patrón, agregar un botón de expandir y contraer (el botón de "hamburguesa") y una vista de lista al control de vista en dos paneles.

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Ejemplos


El control de vista en dos paneles en su forma predeterminada es un contenedor básico. Con un botón y una vista de lista agregados, el control de vista en dos paneles está listo como un menú de navegación. Estos son ejemplos de la vista en dos paneles como un menú de navegación, en los modos expandido y compacto.

![ejemplo de un menú de vista en dos paneles en modo de superposición y en modo compacto](images/controls-splitview-menu01.png)
## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Recomendaciones


-   Al usar la vista en dos paneles para un menú de navegación, recomendamos colocar en el panel controles de navegación que permitan el acceso a otras áreas de la aplicación. El uso del panel de navegación ofrece una experiencia de usuario coherente. Además, esta implementación de menú permite a los usuarios familiarizarse con todas las partes de una aplicación, proporciona acceso rápido a la página principal de la aplicación y puede animar a los usuarios a explorar más áreas de la aplicación.

\[Este artículo contiene información específica de las aplicaciones para la Plataforma universal de Windows (UWP) y Windows 10. Para obtener instrucciones sobre Windows 8.1, descarga el [PDF sobre las directrices para Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743)\].

## <span id="related_topics"></span>Temas relacionados


* [Patrón de panel de navegación](nav-pane.md)
* [Vista de lista](lists.md)
 

 






<!--HONumber=Mar16_HO1-->



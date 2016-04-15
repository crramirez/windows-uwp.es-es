---
Description: Proporciona navegación de nivel superior mientras conserva el espacio de la pantalla.
title: Directrices sobre paneles de navegación
ms.assetid: 8FB52F5E-8E72-4604-9222-0B0EC6A97541
label: Nav pane
template: detail.hbs
---

Paneles de navegación
=============================================================================================
Un panel de navegación (o "nav pane" abreviado en inglés), es un patrón que admite muchos elementos de navegación de nivel superior a la vez que se conserva el espacio de la pantalla. El panel de navegación se usa ampliamente en aplicaciones móviles, pero también funciona bien en pantallas más grandes. Cuando se usa como una superposición, el panel permanece contraído y fuera de la vista hasta que el usuario presiona el botón, lo que es útil para pantallas más pequeñas. Cuando se usa en su modo acoplado, el panel permanece abierto, lo que te permite aprovechar mejor la pantalla, si hay suficiente espacio en la misma.

![Ejemplo de un panel de navegación](images/NAV_PANE_EXAMPLE.png)

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objeto SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

<<<<<<< ENCABEZADO

=======

>>>>>>> origen

<span id="Is_this_the_right_pattern_"></span><span id="is_this_the_right_pattern_"></span><span id="IS_THIS_THE_RIGHT_PATTERN_"></span>¿Es este el patrón adecuado?
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

El panel de navegación es adecuado para:

-   Aplicaciones con muchos elementos de navegación de nivel superior que se encuentren en la misma clase; por ejemplo, una aplicación de deportes con categorías como fútbol, béisbol, baloncesto, etc.
-   Proporcionar una experiencia de navegación coherente en las aplicaciones, siempre que solo se coloquen los elementos de navegación dentro del panel.
-   Un número de medio a alto (5-10 +) de categorías de navegación de nivel superior.
-   Conservar el espacio en pantalla (como una superposición).
-   Elementos de navegación a los que se accede con poca frecuencia. (como una superposición).
-   Escenarios de arrastrar y soltar (cuando se acopla).

<span id="Building_a_nav_pane"></span><span id="building_a_nav_pane"></span><span id="BUILDING_A_NAV_PANE"></span>Crear un panel de navegación
-------------------------------------------------------------------------------------------------------------------------------------

El patrón de panel de navegación consta de un botón, un panel de categorías de navegación y un área de contenido. La forma más sencilla de crear un panel de navegación es con un [control de vista dividida](split-view.md), que viene con un panel vacío y un área de contenido que siempre está visible. El panel puede estar visible u oculto, y puede presentarse desde el lado izquierdo o desde el lado derecho de una ventana de la aplicación.

Si quieres crear un panel de navegación sin el control de vista en dos paneles, necesitarás tres componentes principales: un botón, un panel y un área de contenido. El botón permite al usuario abrir y cerrar el panel. El panel es un contenedor de elementos de navegación. En el área de contenido se muestra información del elemento de navegación seleccionado. El panel de navegación también puede existir en un modo acoplado, en el que siempre se muestra el panel, por lo que un botón no sería necesario en ese caso.

### <span id="Button"></span><span id="button"></span><span id="BUTTON"></span>Botón

El botón del panel de navegación se visualiza, de manera predeterminada, como tres líneas horizontales apiladas y normalmente se conoce como el botón "hamburguesa". El botón permite al usuario abrir y cerrar el panel cuando sea necesario y no se mueve con el panel. Se recomienda colocar el botón en la esquina superior izquierda de la aplicación. El botón no se mueve con el panel.

![Ejemplo del botón del panel de navegación](images/NAVPANE_BUTTONONLY.png)

El botón suele estar asociado a una cadena de texto. En el nivel superior de la aplicación, se puede mostrar el título de la aplicación junto al botón. En los niveles inferiores de la aplicación, la cadena de texto puede asociarse con la página en la que se encuentra el usuario.

### <span id="Pane"></span><span id="pane"></span><span id="PANE"></span>Panel

Los encabezados de las categorías de navegación van en el panel. Los puntos de entrada a la configuración de la aplicación y la administración de cuentas, si procede, también se incluyen en el panel. Los encabezados de navegación pueden ser de nivel superior, o anidados de nivel superior y segundo.

![Ejemplo del panel de un panel de navegación](images/NAVPANE_PANE.png)

### <span id="Content_area"></span><span id="content_area"></span><span id="CONTENT_AREA"></span>Área de contenido

El área de contenido es donde se muestra información de la ubicación de navegación seleccionada. Puede contener elementos individuales o navegación de niveles inferiores.

<span id="Nav_pane_variations"></span><span id="nav_pane_variations"></span><span id="NAV_PANE_VARIATIONS"></span>Variaciones del panel de navegación
-------------------------------------------------------------------------------------------------------------------------------------

El panel de navegación tiene dos variaciones principales: superposición y acoplado. La superposición se contrae y se expande según sea necesario. El panel acoplado permanece abierto de forma predeterminada.

### <span id="Overlay"></span><span id="overlay"></span><span id="OVERLAY"></span>Superposición

-   La superposición puede usarse en cualquier tamaño de pantalla y en orientación vertical u horizontal. En su estado predeterminado (contraído), la superposición no ocupa ningún estado real, solo se muestra el botón.
-   Ofrece una navegación a petición que ahorra superficie en pantalla. Ideal para aplicaciones en teléfonos y tabléfonos.
-   El panel está oculto de forma predeterminada, solo con el botón visible.
-   Al presionar el botón del panel de navegación, se abre y se cierra la superposición.
-   El estado expandido es transitorio y se descarta cuando se realiza una selección, cuando se usa el botón Atrás, o cuando el usuario pulsa fuera del panel.
-   La superposición se dibuja encima del contenido y no redistribuye el contenido.

### <span id="Docked"></span><span id="docked"></span><span id="DOCKED"></span>Acoplado

-   El panel de navegación permanece abierto. Este modo es más apropiado para pantallas más grandes, por lo general de tabletas y superiores.
-   En la orientación horizontal, el ancho de pantalla mínima que se puede aprovechar del estado acoplado es 720 epx. El estado acoplado en este tamaño puede requerir atención especial para el escalado de contenido.
-   Admite los escenarios de arrastrar y colocar hacia y desde el panel.
-   El botón del panel de navegación no es necesario para este estado. Si se usa el botón, el área de contenido se mueve fuera y se redistribuye el contenido dentro de esa área.
-   La selección debe mostrarse en los elementos de lista para resaltar en qué lugar del árbol de navegación se encuentra el usuario.
-   Cuando el dispositivo es demasiado estrecho para mostrar el panel acoplado cuando está en vertical, se recomienda establecer el siguiente comportamiento cuando se gire el dispositivo:
    -   De horizontal a vertical. El panel se contrae al estado de superposición o al estado minimizado.
    -   De vertical a horizontal. El panel vuelve a aparecer.

<span id="related_topics"></span>Temas relacionados
-----------------------------------------------

* [Control de vista en dos paneles](split-view.md)
* [Maestro y detalles](master-details.md)
* [Conceptos básicos de navegación](https://msdn.microsoft.com/library/windows/apps/dn958438)
 

 


<!--HONumber=Mar16_HO4-->



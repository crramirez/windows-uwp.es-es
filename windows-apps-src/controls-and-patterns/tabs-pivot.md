---
Description: Las pestañas y las tablas dinámicas permiten a los usuarios navegar entre contenido al que se accede con frecuencia.
title: Pestañas y tablas dinámicas
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Pestañas y tablas dinámicas
template: detail.hbs
---
# Pestañas y tablas dinámicas

Las pestañas y los controles dinámicos se usan para navegar por categorías de contenidos distintas y a las que se accede con frecuencia. El patrón de la pestaña/tabla dinámica se compone de dos o más paneles de contenido que tienen los encabezados de categoría correspondientes. Los encabezados se mantienen en pantalla y tienen un estado de selección que se muestra con claridad, para que los usuarios conozcan siempre en qué categoría están.
![Ejemplos de pestañas](images/HIGSecOne_Tabs.png)

Las pestañas y las tablas dinámicas son efectivamente el mismo patrón y ambos se crean usando el control [**dinámico**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). La funcionalidad básica del control dinámico se describe más adelante en este artículo.

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase Tabla dinámica**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## Patrón de pestañas/tablas dinámicas

Al crear una aplicación con el patrón de pestañas/tablas dinámicas, hay algunas variables de diseño clave a tener en cuenta en función del conjunto de funciones configurables del patrón.

- **Colocación del encabezado.**   Los encabezados pueden colocarse en la parte superior o inferior de la pantalla.
    
    **Nota**&nbsp;&nbsp;La colocación de los encabezados en la parte inferior de la pantalla requiere nuevas plantillas del control dinámico.
- **Etiquetas de encabezado.**  Los encabezados pueden tener un icono con texto, solo texto o solo un icono.
- **Alineación de encabezados.**  Los encabezados pueden dejarse centrados o alineados a la izquierda.
- **Navegación de nivel inferior o de nivel superior.**  Las pestañas/controles dinámicos pueden usarse para cualquier nivel de navegación y se pueden apilar en un patrón de nivel superior/inferior. Cuando hay dos niveles de las pestañas o tablas dinámicas, los encabezados de nivel superior y de nivel secundario deben tener suficiente diferenciación visual para que los usuarios puedan separar claramente los dos.
- **Compatibilidad con gestos táctiles.**  En los dispositivos que admiten los gestos táctiles, puedes usar uno de los dos conjuntos de interacción para navegar entre las categorías de contenido:
    1. Toca un encabezado de pestaña/tabla dinámica para navegar a esa categoría o pasa el dedo por el área de contenido para navegar a la categoría adyacente.
    2. Toca un encabezado de pestaña/tabla dinámica para navegar a esa categoría (sin pasar el dedo).

### Configuraciones de patrón

La disposición óptima del patrón de pestaña/tabla dinámica depende del escenario de interacción y los dispositivos en los que aparecerá la aplicación. En esta tabla se describen algunos de los escenarios y configuraciones de patrón principales.

Escenario de interacción|Configuración recomendada
--------------------|-------------------------
Desplazarse lateralmente entre 2 y 5 categorías de contenido de la vista de cuadrícula o de lista de nivel superior en un teléfono o en un tabléfono|Pestaña/tablas dinámicas: se colocan en la parte superior de la pantalla, centradas
|Etiquetas de encabezado: iconos y texto
|Pasa el dedo por el área de contenido: habilitado
Desplazamiento entre una variedad de categorías de contenido en un teléfono o un tabléfono en el que pasar el dedo por un área de contenido no es práctico para la navegación|Pestaña/tablas dinámicas: se colocan en la parte inferior de la pantalla, centradas
|Etiquetas de encabezado: iconos y texto
|Pasa el dedo por el área de contenido: deshabilitado
Navegación de nivel superior con un mouse y un teclado|Pestaña/tablas dinámicas: se colocan en la parte superior de la pantalla, alineadas a la izquierda
 *o bien*|Etiquetas de encabezado: solo texto
 Navegación de nivel de página en un dispositivo táctil|Pasa el dedo por el área de contenido: deshabilitado

## Ejemplos

Este diseño de una aplicación de camiones de venta de alimentos muestra el aspecto que puede tener el colocar encabezados de pestañas/tablas dinámicas en la parte superior o inferior. En los dispositivos móviles, colocarlos en la parte inferior funciona bien para la accesibilidad.

![Ejemplo de pestañas en un dispositivo móvil](images/uap_foodtruck_phone_320_tabsboth.png)

Diseño de aplicación de camiones de venta de alimentos en encabezados de solo texto de funciones de portátiles o equipos de escritorio. El uso de iconos con texto para los encabezados ayuda con la selección táctil del destino, pero para el mouse y el teclado, los encabezados de solo texto funcionan correctamente.

![Ejemplo de pestañas en el escritorio](images/uap_foodtruck_desktop_home_700.png)

## Crear un control dinámico

El patrón de navegación de la pestaña/tabla dinámica se crea mediante el control [**dinámico**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). El control incluye la funcionalidad básica que se describe en esta sección.

Este XAML crea un control dinámico básico con 3 secciones de contenido.

```xaml
<Pivot x:Name="rootPivot" Title="PIVOT TITLE">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

**Elementos de tabla dinámica**

La tabla dinámica es un [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), de modo que puede contener una colección de elementos de cualquier tipo. Cualquier elemento que agregues a la tabla dinámica que no sea explícitamente un [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) se encapsula implícitamente en un PivotItem. Dado que una tabla dinámica a menudo se usa para navegar entre páginas de contenido, es común para rellenar la colección [**Elementos**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directamente con los elementos de la interfaz de usuario de XAML. O puedes establecer la propiedad [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) en un origen de datos. Los elementos enlazados en ItemsSource pueden ser de cualquier tipo, pero si no son explícitamente PivotItems, debes definir una [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) y [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar cómo se muestran los elementos.

Puedes usar la propiedad [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obtener o configurar el elemento activo de la tabla dinámica. Usa la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obtener o configurar el índice del elemento activo. 

**Encabezados dinámicos**

Puedes usar las propiedades [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) y [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para agregar otros controles al encabezado dinámico. 

### Interacción dinámica

El control ofrece estas interacciones de gestos táctiles:

-   Tocar un encabezado permite navegar al contenido de la sección de dicho encabezado.
-   Pasar el dedo a la izquierda o la derecha en un encabezado permite navegar a la sección/encabezado adyacente.
-   Pasar el dedo a la izquierda o la derecha en el contenido de una sección permite navegar a la sección/encabezado adyacente.

El control tiene dos modos:

**Inmóvil**

-   Las tablas dinámicas están inmóviles cuando todos los encabezados de tablas dinámicas caben en el espacio permitido.
-   Al tocar una etiqueta de tabla dinámica se navega a la página correspondiente, aunque no se moverá la propia tabla dinámica. La tabla dinámica activa se destacará.

**Carrusel**

-   Las tablas dinámicas giran cuando no caben todos los encabezados de tabla dinámica dentro del espacio permitido.
-   Al tocar una etiqueta de tabla dinámica se navega a la página correspondiente y, a continuación, la etiqueta de la tabla dinámica girará a la primera posición.

El control tiene funcionalidad integrada de punto de interrupción, que se basa en el número de encabezados y la longitud de cadena de las etiquetas.

## Recomendaciones

-   Basar la alineación de los encabezados de la pestaña/tabla dinámica en el tamaño de la pantalla. Para tamaños de pantalla inferiores a 720 epx, normalmente funciona mejor la alineación al centro, mientras que en la mayoría de casos se recomienda la alineación a la izquierda para anchos de pantalla superiores a 720 epx.
-   Durante el escalado de una ventana, una vez que el número de encabezados de pestañas/tablas dinámicas supere el espacio disponible, empieza a insertar encabezados en el área de desbordamiento.
-   Las pestañas/tablas dinámicas se pueden usar en cualquier orientación de la pantalla, pero asegúrate de mantener el mismo número total de encabezados (visibles y ocultos) en las orientaciones horizontal y vertical.
-   Evita usar más de 5 encabezados cuando uses el modo de carrusel (ida y vuelta), ya que tener más de 5 puede provocar que el usuario pierda la orientación.
-   En los dispositivos móviles, colocar pestañas/tablas dinámicas en la parte inferior es adecuado para la accesibilidad, si se usa la acción de pasar el dedo en otra parte de la interfaz de usuario y para evitar una interfaz de usuario pesada.
-   Cuando se implementa el teclado en pantalla, los encabezados pueden salir de la pantalla para conservar espacio.

\[Este artículo contiene información específica de las aplicaciones para la Plataforma universal de Windows (UWP) y Windows 10. Para obtener instrucciones sobre Windows 8.1, descarga el [PDF sobre las directrices para Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743).\]

## Temas relacionados

[Conceptos básicos de diseño de la navegación](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=Mar16_HO1-->



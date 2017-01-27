---
author: Jwmsft
Description: "Las pestañas y los controles dinámicos permiten a los usuarios navegar entre contenido al que se accede con frecuencia."
title: "Pestañas y controles dinámicos"
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a652189d0595bb6dffbc5228a91a68261eb7e5ef
ms.openlocfilehash: 865969a7c965a502acd9fe24df28128399efd380

---
# <a name="pivot-and-tabs"></a>Control dinámico y pestañas

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Los controles dinámicos y el patrón de pestañas relacionado se usan para navegar por categorías de contenido distintas a las que se accede con frecuencia. Los elementos dinámicos permiten la navegación entre dos o más paneles de contenido y se basan en los encabezados de texto para articular las diferentes secciones de contenido.

![Ejemplos de pestañas](images/pivot_Hero_main.png)

Las pestañas son una variante visual de los elementos dinámicos que usan una combinación de iconos y texto o solo iconos para articular el contenido de las secciones. Las pestañas se crean mediante el control [**dinámico**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). La [**muestra de control dinámico**](http://go.microsoft.com/fwlink/p/?LinkId=619903) indica cómo personalizar el control dinámico en el patrón de pestañas.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Clase Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241)</li>
</ul>
</div>


## <a name="the-pivot-pattern"></a>Patrón de controles dinámicos

Al crear una aplicación con control dinámico, hay que tener en cuenta algunas variables de diseño importantes.

- **Etiquetas de encabezado.**  Los encabezados pueden tener un icono con texto, solo texto o solo un icono.
- **Alineación de encabezados.**  Los encabezados pueden dejarse centrados o alineados a la izquierda.
- **Navegación de nivel inferior o de nivel superior.**  Los controles dinámicos se pueden usar para cualquier nivel de navegación. Opcionalmente, el [panel de navegación](nav-pane.md) puede servir como nivel principal con controles dinámicos que actúen como secundarios.
- **Compatibilidad con gestos táctiles.**  En los dispositivos que admiten los gestos táctiles, puedes usar uno de los dos conjuntos de interacción para navegar entre las categorías de contenido:
    1. Toca un encabezado de pestaña/control dinámico para navegar a esa categoría.
    2. Desplázate hacia la izquierda o la derecha en el área de contenido para navegar a la categoría adyacente.

## <a name="examples"></a>Ejemplos

Control dinámico en teléfono.

![Un ejemplo de control dinámico](images/pivot_example.png)

Patrón de pestañas en la aplicación Alarmas y reloj.

![Ejemplo de patrón de pestañas en Alarmas y reloj](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>Crear un control dinámico

El control [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) incluye la funcionalidad básica que se describe en esta sección.

Este XAML crea un control dinámico básico con 3 secciones de contenido.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
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

### <a name="pivot-items"></a>Elementos de tabla dinámica

La tabla dinámica es un [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), de modo que puede contener una colección de elementos de cualquier tipo. Cualquier elemento que agregues a la tabla dinámica que no sea explícitamente un [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) se encapsula implícitamente en un PivotItem. Dado que una tabla dinámica a menudo se usa para navegar entre páginas de contenido, es común para rellenar la colección [**Elementos**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directamente con los elementos de la interfaz de usuario de XAML. O puedes establecer la propiedad [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) en un origen de datos. Los elementos enlazados en ItemsSource pueden ser de cualquier tipo, pero si no son explícitamente PivotItems, debes definir una [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) y [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar cómo se muestran los elementos.

Puedes usar la propiedad [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obtener o configurar el elemento activo de la tabla dinámica. Usa la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obtener o configurar el índice del elemento activo.

### <a name="pivot-headers"></a>Encabezados dinámicos

Puedes usar las propiedades [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) y [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para agregar otros controles al encabezado dinámico.

### <a name="pivot-interaction"></a>Interacción dinámica

El control ofrece estas interacciones de gestos táctiles:

-   Tocar un encabezado de elemento de control dinámico permite navegar al contenido de la sección de dicho encabezado.
-   Pasar el dedo a la izquierda o la derecha en un encabezado de elemento de control dinámico permite navegar a la sección adyacente.
-   Pasar el dedo a la izquierda o la derecha en el contenido de una sección permite navegar a la sección adyacente.
![Ejemplo de pasar el dedo a la izquierda en el contenido de la sección](images/pivot_w_hand.png)

El control tiene dos modos:

**Inmóvil**

-   Los controles dinámicos están inmóviles cuando todos los encabezados de control dinámico caben en el espacio permitido.
-   Al tocar una etiqueta de control dinámico, se navega a la página correspondiente, aunque el propio control dinámico no se moverá. El control dinámico activo se destacará.

<div class="microsoft-internal-note">
Especialmente, te recomendamos evitar elementos en una vista de carrusel en un entorno de 10 pies. Configura la nueva propiedad `IsHeaderItemsCarouselEnabled` en False si la aplicación se ejecutará en Xbox.
</div>

**Carrusel**

-   Los controles dinámicos giran cuando no caben todos los encabezados de control dinámico dentro del espacio permitido.
-   Al tocar una etiqueta de control dinámico se navega a la página correspondiente y, después, la etiqueta de control dinámico activa gira a la primera posición.
-   Elementos de control dinámico en un bucle de carrusel de la última a la primera sección de control dinámico.

<div class="microsoft-internal-note">
### Foco de control dinámico

De manera predeterminada, el foco del teclado en un encabezado de control dinámico se representa con un subrayado.

![El foco predeterminado subraya el encabezado seleccionado](images/pivot_focus_selectedHeader.png)

Las aplicaciones que tienen un control dinámico e incorporan el subrayado en elementos visuales de la selección de encabezado pueden usar la nueva propiedad `HeaderFocusVisualPlacement` para cambiar el valor predeterminado. Cuando `HeaderFocusVisualPlacement=\"ItemHeaders\"`, el foco se dibuja alrededor de todo el panel de encabezado.

![La opción ItemsHeader dibuja el rectángulo de foco alrededor de todos los encabezados dinámicos](images/pivot_focus_headers.png)
</div>

## <a name="recommendations"></a>Recomendaciones

-   Basa la alineación de los encabezados de pestaña/control dinámico en el tamaño de la pantalla. Para tamaños de pantalla inferiores a 720 epx, normalmente funciona mejor la alineación al centro, mientras que en la mayoría de casos se recomienda la alineación a la izquierda para anchos de pantalla superiores a 720 epx.
-   Evita usar más de 5 encabezados cuando uses el modo de carrusel (ida y vuelta), ya que un bucle de más de 5 puede resultar confuso.
-   Usa el patrón de pestañas solo si los elementos de control dinámico tienen iconos diferentes.
-   Incluye texto en los encabezados de elementos de control dinámico para ayudar a los usuarios a comprender el significado de cada sección de control dinámico. Los iconos no son necesariamente descriptivos para todos los usuarios.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo
- [Muestra de control dinámico](http://go.microsoft.com/fwlink/p/?LinkId=619903)<br/>
    Aprende a personalizar el control dinámico en el patrón de pestañas.
- [Muestra de conceptos básicos de una interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Ve todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados
- [Conceptos básicos de diseño de la navegación](../layout/navigation-basics.md)
- [**Muestra de control dinámico**](http://go.microsoft.com/fwlink/p/?LinkId=619903)



<!--HONumber=Dec16_HO1-->



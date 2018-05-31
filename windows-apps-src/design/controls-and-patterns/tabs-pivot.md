---
author: serenaz
Description: Tabs and pivots enable users to navigate between frequently accessed content.
title: Pestañas y controles dinámicos
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 09e88fd070f7e7517e55d6f57563dd7608696770
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675482"
---
# <a name="pivot-and-tabs"></a>Control dinámico y pestañas



Los controles dinámicos y el patrón de pestañas relacionado se usan para navegar por categorías de contenido distintas a las que se accede con frecuencia. Los elementos dinámicos permiten la navegación entre dos o más paneles de contenido y se basan en los encabezados de texto para etiquetar las diferentes secciones de contenido.

> **API importantes**: [Clase Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)

![Un ejemplo del control dinámico](images/pivot_Hero_main.png)

Las pestañas son una variante visual de los elementos dinámicos que usan una combinación de iconos y texto o solo iconos para articular el contenido de las secciones. Las pestañas se crean mediante el control [Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). La [Muestra de control dinámico](http://go.microsoft.com/fwlink/p/?LinkId=619903) indica cómo personalizar el control dinámico en el patrón de pestañas.

![Un ejemplo del patrón de pestañas](images/tabs.png) 

## <a name="the-pivot-control"></a>El control dinámico

Al crear una aplicación con control dinámico, hay que tener en cuenta algunas variables de diseño importantes.

- **Etiquetas de encabezado.**  Los encabezados pueden tener un icono con texto, solo texto o solo un icono.
- **Alineación de encabezados.**  Los encabezados pueden dejarse centrados o alineados a la izquierda.
- **Navegación de nivel inferior o de nivel superior.**  Los controles dinámicos se pueden usar para cualquier nivel de navegación. Opcionalmente, la [vista de navegación](navigationview.md) puede servir como nivel principal con controles dinámicos que actúen como secundarios.
- **Compatibilidad con gestos táctiles.**  En los dispositivos que admiten los gestos táctiles, puedes usar uno de los dos conjuntos de interacción para navegar entre las categorías de contenido:
    1. Toca un encabezado de pestaña/control dinámico para navegar a esa categoría.
    2. Desplázate hacia la izquierda o la derecha en el área de contenido para navegar a la categoría adyacente.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Pivot">abrir la aplicación y ver el control dinámico en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Control dinámico en teléfono.

![Un ejemplo de control dinámico](images/pivot_example.png)

Patrón de pestañas en la aplicación Alarmas y reloj.

![Ejemplo de patrón de pestañas en Alarmas y reloj](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>Crear un control dinámico

El control [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) incluye la funcionalidad básica que se describe en esta sección.

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

La tabla dinámica es un [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), de modo que puede contener una colección de elementos de cualquier tipo. Cualquier elemento que agregues a la tabla dinámica que no sea explícitamente un [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) se encapsula implícitamente en un PivotItem. Dado que una tabla dinámica a menudo se usa para navegar entre páginas de contenido, es común para rellenar la colección [Elementos](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directamente con los elementos de la interfaz de usuario de XAML. O puedes establecer la propiedad [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) en un origen de datos. Los elementos enlazados en ItemsSource pueden ser de cualquier tipo, pero si no son explícitamente PivotItems, debes definir una [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) y [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar cómo se muestran los elementos.

Puedes usar la propiedad [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obtener o configurar el elemento activo de la tabla dinámica. Usa la propiedad [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obtener o configurar el índice del elemento activo.

### <a name="pivot-headers"></a>Encabezados dinámicos

Puedes usar las propiedades [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) y [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para agregar otros controles al encabezado dinámico.

Por ejemplo, puedes agregar un [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) en RightHeader del control dinámico.

![Un ejemplo de una barra de comandos en el encabezado de derecho del control dinámico](images/PivotHeader.png)

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>Interacción dinámica

El control ofrece estas interacciones de gestos táctiles:

-   Tocar un encabezado de elemento de control dinámico permite navegar al contenido de la sección de dicho encabezado.
-   Pasar el dedo a la izquierda o la derecha en un encabezado de elemento de control dinámico permite navegar a la sección adyacente.
-   Pasar el dedo a la izquierda o la derecha en el contenido de una sección permite navegar a la sección adyacente.

![Ejemplo de pasar el dedo a la izquierda en el contenido de la sección](images/pivot_w_hand.png)

El control tiene dos modos:

**Inmóvil**

-   Los controles dinámicos están inmóviles cuando todos los encabezados de control dinámico caben en el espacio permitido.
-   Al tocar una etiqueta de control dinámico, se navega a la página correspondiente, aunque el propio control dinámico no se moverá. La tabla dinámica activa se destacará.

**Carrusel**

-   Los controles dinámicos giran cuando no caben todos los encabezados de control dinámico dentro del espacio permitido.
-   Al tocar una etiqueta de control dinámico se navega a la página correspondiente y, después, la etiqueta de control dinámico activa gira a la primera posición.
-   Elementos de control dinámico en un bucle de carrusel de la última a la primera sección de control dinámico.

> **Nota:** los encabezados dinámicos no se ven en carrusel en un [entorno de 10 pies](../devices/designing-for-tv.md). Establece la propiedad [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) en **false** si la aplicación se ejecuta en Xbox.

### <a name="pivot-focus"></a>Foco de control dinámico

De manera predeterminada, el foco del teclado en un encabezado de control dinámico se representa con un subrayado.

![El foco predeterminado subraya el encabezado seleccionado](images/pivot_focus_selectedHeader.png)

Las aplicaciones que tienen un control dinámico e incorporan el subrayado en elementos visuales de la selección de encabezado pueden usar la propiedad [HeaderFocusVisualPlacement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.HeaderFocusVisualPlacement) para cambiar el valor predeterminado. Cuando `HeaderFocusVisualPlacement="ItemHeaders"`, el foco se dibuja alrededor de todo el panel de encabezado.

![La opción ItemsHeader dibuja el rectángulo de foco alrededor de todos los encabezados dinámicos](images/pivot_focus_headers.png)

## <a name="recommendations"></a>Recomendaciones

- Basa la alineación de los encabezados de pestaña/control dinámico en el tamaño de la pantalla. Para tamaños de pantalla inferiores a 720 epx, normalmente funciona mejor la alineación al centro, mientras que en la mayoría de casos se recomienda la alineación a la izquierda para anchos de pantalla superiores a 720 epx.
- Evita usar más de 5 encabezados cuando uses el modo de carrusel (ida y vuelta), ya que un bucle de más de 5 puede resultar confuso.
- Usa el patrón de pestañas solo si los elementos de control dinámico tienen iconos diferentes.
- Incluye texto en los encabezados de elementos de control dinámico para ayudar a los usuarios a comprender el significado de cada sección de control dinámico. Los iconos no son necesariamente descriptivos para todos los usuarios.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Muestra de control dinámico](http://go.microsoft.com/fwlink/p/?LinkId=619903): demuestra cómo personalizar el control dinámico en el patrón de pestañas.
- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Artículos relacionados

- [Clase Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Conceptos básicos de diseño de la navegación](../basics/navigation-basics.md)

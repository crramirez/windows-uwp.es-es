---
author: QuinnRadich
Description: The Pivot control enables touch-swiping between a small set of content sections.
title: Dinámica
template: detail.hbs
ms.author: quradic
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 386fba3cec00de6c443daa60409fe3bb74621fa1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5701865"
---
# <a name="pivot"></a>Dinámica

El control [dinámico](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permite la entrada táctil de deslizar rápidamente entre un conjunto reducido de las secciones de contenido.

> **API importantes**: [clase Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [clase NavigationView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación de la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> , haz clic aquí para <a href="xamlcontrolsgallery:/item/Pivot">Abrir la aplicación y ver el control dinámico en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El control dinámico, al igual que [NavigationView](navigationview.md), subraya el elemento seleccionado.

![El foco predeterminado subraya el encabezado seleccionado](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Para lograr la navegación superior comunes y patrones de pestañas, te recomendamos que uses [NavigationView](navigationview.md), que se adapta a diferentes tamaños de pantalla y permite la personalización mayor automáticamente.

Sin embargo, si la navegación requiere Deslizar rápidamente la entrada táctil, se recomienda usar el control dinámico.

Las otras diferencias clave entre los controles de NavigationView y controles dinámicos son el comportamiento de desbordamiento predeterminada y la API de navegación:

- Desbordamiento carruseles elementos, mientras NavigationView usa un menú desplegable de desbordamiento para que los usuarios pueden ver todos los elementos de control dinámico.
- Control dinámico controla la navegación entre las secciones de contenido, mientras que NavigationView permite tener más control sobre el comportamiento de navegación.

## <a name="use-navigationview-instead-of-pivot"></a>Usar NavigationView en lugar de control dinámico

Si la interfaz de usuario de la aplicación usa el control dinámico, a continuación, puedes convertir tabla dinámica a NavigationView con el siguiente código.

Este código XAML crea un NavigationView con 3 secciones de contenido, como en el ejemplo de control dinámico en [crear un control dinámico](#create-a-pivot-control).

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView ofrece más control sobre la personalización de navegación y requiere correspondiente de código subyacente. Para anexarlos el código XAML anterior, usa el siguiente código subyacente:

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

Este código imita la experiencia de exploración integrada del control dinámico, menos la experiencia de deslizar rápidamente táctil entre las secciones de contenido. Sin embargo, como puedes ver, también podría personalizar varios puntos, incluida la transición animada, los parámetros de navegación y las funcionalidades de la pila.

## <a name="create-a-pivot-control"></a>Crear un control dinámico

Este código crea un control dinámico básico con 3 secciones de contenido.

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Elementos de tabla dinámica

La tabla dinámica es un [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), de modo que puede contener una colección de elementos de cualquier tipo. Cualquier elemento que agregues a la tabla dinámica que no sea explícitamente un [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) se encapsula implícitamente en un PivotItem. Dado que una tabla dinámica a menudo se usa para navegar entre páginas de contenido, es común para rellenar la colección [Elementos](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directamente con los elementos de la interfaz de usuario de XAML. O puedes establecer la propiedad [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) en un origen de datos. Los elementos enlazados en ItemsSource pueden ser de cualquier tipo, pero si no son explícitamente PivotItems, debes definir una [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) y [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar cómo se muestran los elementos.

Puedes usar la propiedad [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obtener o configurar el elemento activo de la tabla dinámica. Usa la propiedad [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obtener o configurar el índice del elemento activo.

### <a name="pivot-headers"></a>Encabezados dinámicos

Puedes usar las propiedades [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) y [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para agregar otros controles al encabezado dinámico.

Por ejemplo, puedes agregar un [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) en RightHeader del control dinámico.

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
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

- Tocar un encabezado de elemento de control dinámico permite navegar al contenido de la sección de dicho encabezado.
- Pasar el dedo a la izquierda o la derecha en un encabezado de elemento de control dinámico permite navegar a la sección adyacente.
- Pasar el dedo a la izquierda o la derecha en el contenido de una sección permite navegar a la sección adyacente.

El control tiene dos modos:

**Inmóvil**

- Los controles dinámicos están inmóviles cuando todos los encabezados de control dinámico caben en el espacio permitido.
- Al tocar una etiqueta de control dinámico, se navega a la página correspondiente, aunque el propio control dinámico no se moverá. La tabla dinámica activa se destacará.

**Carrusel**

- Los controles dinámicos giran cuando no caben todos los encabezados de control dinámico dentro del espacio permitido.
- Al tocar una etiqueta de control dinámico se navega a la página correspondiente y, después, la etiqueta de control dinámico activa gira a la primera posición.
- Elementos de control dinámico en un bucle de carrusel de la última a la primera sección de control dinámico.

> **Nota:** los encabezados dinámicos no se ven en carrusel en un [entorno de 10 pies](../devices/designing-for-tv.md). Establece la propiedad [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) en **false** si la aplicación se ejecuta en Xbox.

## <a name="recommendations"></a>Recomendaciones

- Evita usar más de 5 encabezados cuando uses el modo de carrusel (ida y vuelta), ya que un bucle de más de 5 puede resultar confuso.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Artículos relacionados

- [Clase Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Conceptos básicos de diseño de la navegación](../basics/navigation-basics.md)
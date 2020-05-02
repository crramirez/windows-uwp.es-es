---
Description: El control Pivot permite el deslizamiento táctil rápido entre un pequeño conjunto de secciones de contenido.
title: Pivot
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6d55c24dbf84e16e0bb4dedc9eaf2b89982e2993
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081618"
---
# <a name="pivot"></a>Pivot

El control [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permite el deslizamiento táctil rápido entre un pequeño conjunto de secciones de contenido.

![El foco predeterminado subraya el encabezado seleccionado](images/pivot_focus_selectedHeader.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](/windows/uwp/design/style/rounded-corner). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones para UWP. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library ](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma**: [Clase Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [Clase NavigationView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Pivot">abrir la aplicación y ver PivotControl en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El control Pivot, al igual que [NavigationView](navigationview.md), subraya el elemento seleccionado.

![El foco predeterminado subraya el encabezado seleccionado](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Para lograr patrones comunes de navegación superior y de pestañas, se recomienda utilizar [NavigationView](navigationview.md), que se adapta automáticamente a diferentes tamaños de pantalla y permite una mayor personalización.

Sin embargo, si la navegación requiere el deslizamiento táctil rápido, se recomienda usar Pivot.

Otras diferencias importantes entre los controles NavigationView y Pivot son el comportamiento de desbordamiento predeterminado y la API de navegación:

- Pivot muestra los elementos de desbordamiento en vistas de carrusel, mientras que NavigationView utiliza un menú desplegable de desbordamiento para que los usuarios puedan ver todos los elementos.
- Pivot controla la navegación entre las secciones de contenido, mientras que el control NavigationView permite más control sobre el comportamiento de navegación.

## <a name="use-navigationview-instead-of-pivot"></a>Usar el control NavigationView en lugar de Pivot

Si la interfaz de usuario de la aplicación usa el control Pivot, puedes convertir Pivot en NavigationView con el código siguiente.

Este lenguaje XAML crea un control NavigationView con tres secciones de contenido, como en el ejemplo de Pivot en [Crear un control Pivot](#create-a-pivot-control).

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

NavigationView ofrece mayor control sobre la personalización de la navegación y requiere el correspondiente código subyacente. Para acompañar el lenguaje XAML anterior, utiliza el siguiente código subyacente:

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

Este código simula la experiencia de navegación integrada del control Pivot, menos la experiencia de deslizamiento táctil rápido entre secciones de contenido. Sin embargo, como puedes ver, también puedes personalizar varios aspectos, entre los que se incluyen el comportamiento de transición animada, los parámetros de navegación y las funcionalidades de la pila.

## <a name="create-a-pivot-control"></a>Crear un control dinámico

Este código crea un control Pivot básico con tres secciones de contenido.

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

Pivot es un [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl), por lo que puede contener una colección de elementos de cualquier tipo. Cualquier elemento que agregues a Pivot que no sea explícitamente un [PivotItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PivotItem) se encapsula implícitamente en un PivotItem. Dado que Pivot suele utilizarse para navegar entre páginas de contenido, es habitual rellenar la colección [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) directamente con los elementos de la interfaz de usuario de XAML. También puedes establecer la propiedad [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) en un origen de datos. Los elementos enlazados en ItemsSource pueden ser de cualquier tipo, pero si no son explícitamente PivotItems, debes definir las propiedades [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) y [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) para especificar cómo se muestran los elementos.

Puedes usar la propiedad [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) para obtener o establecer el elemento activo de Pivot. Usa la propiedad [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) para obtener o establecer el índice del elemento activo.

### <a name="pivot-headers"></a>Encabezados dinámicos

Puedes usar las propiedades [LeftHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.leftheader) y [RightHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.rightheader) para agregar otros controles al encabezado de Pivot.

Por ejemplo, puedes agregar una clase [CommandBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/app-bars) en la propiedad RightHeader de Pivot.

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
- Al tocar una etiqueta de control dinámico, se navega a la página correspondiente, aunque el propio control dinámico no se moverá. El control dinámico activo se destacará.

**Carrusel**

- Los controles dinámicos giran cuando no caben todos los encabezados de control dinámico dentro del espacio permitido.
- Al tocar una etiqueta de control dinámico se navega a la página correspondiente y, después, la etiqueta de control dinámico activa gira a la primera posición.
- Elementos de control dinámico en un bucle de carrusel de la última a la primera sección de control dinámico.

> **Nota:** Los encabezados de tabla dinámica no deben mostrarse en carrusel en un [entorno de 10 pies](../devices/designing-for-tv.md). Establece la propiedad [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) en **false** si la aplicación se ejecuta en Xbox.

## <a name="recommendations"></a>Recomendaciones

- Evita usar más de 5 encabezados cuando uses el modo de carrusel (ida y vuelta), ya que un bucle de más de 5 puede resultar confuso.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados

- [Clase Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Conceptos básicos de diseño de la navegación](../basics/navigation-basics.md)
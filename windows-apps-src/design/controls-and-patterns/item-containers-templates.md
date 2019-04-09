---
Description: Usar plantillas para modificar la apariencia de los elementos en controles ListView o GridView.
title: Plantillas y contenedores de elementos
label: Item containers and templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5b628c4d473c2a74eb63a17c12b17ade43c11964
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244421"
---
# <a name="item-containers-and-templates"></a>Plantillas y contenedores de elementos

 

Los controles **ListView** y **GridView** administran la organización de sus elementos (horizontal, vertical, ajuste, etc.) y la interacción del usuario con los elementos, pero no la visualización de elementos individuales en la pantalla. Administran la visualización de elementos los contenedores de elementos. Cuando agregas elementos a una vista de lista, estos se colocan automáticamente en un contenedor. El contenedor de elementos predeterminado de ListView es [ListViewItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listviewitem.aspx); para GridView, es [GridViewItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridviewitem.aspx).

> **API importantes**: [Clase de ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [clase GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), [propiedad ItemTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx), [propiedad ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx)


> [!NOTE]
> ListView y GridView derivan ambos de la clase [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), por lo que tienen la misma funcionalidad, pero muestran los datos de manera diferente. En este artículo, cuando hablamos de la vista de lista, la información corresponde a ambos controles, ListView y GridView, a menos que se indique lo contrario. Podemos hacer mención a clases como ListView o ListViewItem, pero el prefijo *List* se puede reemplazar por *Grid* para el equivalente de cuadrícula correspondiente (GridView o GridViewItem). 

Estos controles de contenedor constan de dos partes importantes que se combinan para crear los elementos visuales finales que se muestran para un elemento: la *plantilla de datos* y la *plantilla de control*.

- **Plantilla de datos**: debe asignar una clase [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx) a la propiedad [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) de la vista de lista para especificar cómo se muestran los elementos de datos individuales.
- **Plantilla de control**: la plantilla de control proporciona la parte de la visualización de elementos de la que el marco es responsable, como los estados visuales. Puedes usar la propiedad [ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) para modificar la plantilla de control. Por lo general, puedes hacerlo para modificar los colores de la vista de lista de modo que coincidan con tu personalización de marca, o bien para cambiar cómo se muestran los elementos seleccionados.

Esta imagen muestra cómo se combinan la plantilla de control y la plantilla de datos para crear la visualización final de un elemento.

![Plantillas de control y de datos de la vista de lista](images/listview-visual-parts.png)

Este es el código XAML que crea este elemento. Explicaremos las plantillas más adelante.

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="14" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```
 
## <a name="prerequisites"></a>Requisitos previos

- Damos por hecho que sabes cómo usar un control de vista de lista. Para obtener más información, consulta el artículo [ListView y GridView](listview-and-gridview.md).
- También se supone que comprendes las plantillas y los estilos de control, incluido el uso de un estilo en línea como un recurso. Para obtener más información, consulta [Aplicación de estilos a controles](xaml-styles.md) y [Plantillas de control](control-templates.md).

## <a name="the-data"></a>Datos

Antes de profundizar en cómo mostrar elementos de datos en una vista de lista, es necesario comprender los datos que se van a mostrar. En este ejemplo, se crea un tipo de datos denominado `NamedColor`. Combina un nombre de color, un valor de color y una clase **SolidColorBrush** para el color, que se exponen como las tres propiedades siguientes: `Name`, `Color` y `Brush`.
 
A continuación, se rellena una **List** con un objeto `NamedColor` para cada color con nombre en la clase [Colors](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors.aspx). La lista se establece como la propiedad [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) de la vista de lista.

Este es el código para definir la clase y rellenar la lista `NamedColors`.

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>Plantilla de datos

Debes especificar una plantilla de datos para indicar a la vista de lista cómo debe mostrarse el elemento de datos. 

De manera predeterminada, un elemento de datos se muestra en la vista de lista como una representación de cadena del objeto de datos con el que está enlazado. Si muestras los datos de 'NamedColors' en una vista de lista sin indicar a la vista de lista cómo deben mostrarse, mostrará lo que el método **ToString** devuelva, como en el ejemplo siguiente.

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![vista de lista que muestra la representación de cadena de los elementos](images/listview-no-template.png)

Para mostrar la representación de cadena de una propiedad determinada del elemento de datos, establece [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) en dicha propiedad. Aquí, debes establecer DisplayMemberPath en la propiedad `Name` del elemento `NamedColor`.

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

La vista de lista muestra ahora los elementos por nombre, como se muestra aquí. Es más útil, pero no es muy interesante y deja una gran cantidad de información oculta.

![Vista de lista que muestra la representación de cadena de una propiedad del elemento](images/listview-display-member-path.png)

Seguramente quieras mostrar una presentación más enriquecida de los datos. Para especificar con exactitud cómo se mostrarán los elementos en la vista de lista, debes crear una clase [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx). El lenguaje XAML de la clase DataTemplate define el diseño y la apariencia de los controles usados para mostrar un elemento individual. Los controles del diseño se pueden enlazar a las propiedades de un objeto de datos o pueden tener contenido estático definido en línea. Debes asignar la clase DataTemplate a la propiedad [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) del control de lista.

> [!IMPORTANT]
> No puedes usar **ItemTemplate** y **DisplayMemberPath** al mismo tiempo. Si se establecen ambas propiedades, se produce una excepción.

Aquí, debes definir una clase DataTemplate que muestre un objeto [Rectangle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.aspx) con el color del elemento, junto con el nombre del color y los valores RGB. 

> [!NOTE]
> Cuando uses la [extensión de marcado x:Bind](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) en DataTemplate, debes especificar DataType (`x:DataType`) en DataTemplate.

**XAML**
```XAML
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Esta es la apariencia de los elementos de datos cuando se muestran con esta plantilla de datos.

![Elementos de la vista de lista con una plantilla de datos](images/listview-data-template-0.png)

Es posible que quieras mostrar los datos en un control GridView. Esta es otra plantilla de datos que muestra los datos de manera más adecuada para un diseño de cuadrícula. Esta vez, la plantilla de datos está definida como un recurso en lugar de en línea con el código XAML del control GridView.


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

Cuando los datos se muestran en una cuadrícula con esta plantilla de datos, tienen esta apariencia.

![Elementos de la vista de cuadrícula con una plantilla de datos](images/gridview-data-template.png)

### <a name="performance-considerations"></a>Consideraciones de rendimiento

Las plantillas de datos son la forma principal de definir la apariencia de la vista de lista. También pueden tener un impacto significativo en el rendimiento si la lista muestra una gran cantidad de elementos. 

Se crea una instancia de cada elemento XAML en una plantilla de datos para cada elemento de la vista de lista. Por ejemplo, la plantilla de cuadrícula del ejemplo anterior tiene 10 elementos XAML (1 Grid, 1 Rectangle, 3 Border, 5 TextBlock). Un control GridView que muestre 20 elementos en pantalla con esta plantilla de datos creará al menos 200 elementos (20*10 = 200). Reducir el número de elementos en una plantilla de datos puede reducir considerablemente el número total de elementos creados para la vista de lista. Para obtener más información, consulte [ListView y GridView UI optimización: Reducción del recuento de elemento por elemento](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview#element-reduction-per-item).

 Ten en cuenta esta sección de la plantilla de datos de cuadrícula. Veamos algunas de las cosas que reducen el recuento de elementos.

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - En primer lugar, el diseño usa un solo elemento Grid. Podrías tener un elemento Grid de una sola columna y colocar estos tres elementos TextBlock en una clase StackPanel, pero en una plantilla de datos que se crea muchas veces, debes buscar maneras de evitar la incrustación de paneles de diseño en otros paneles de diseño.
 - En segundo lugar, puedes usar un control Border para representar un fondo sin colocar elementos realmente dentro del elemento Border. Un elemento Border puede tener solo un elemento secundario, por lo que deberías agregar un panel de diseño adicional para hospedar los tres elementos TextBlock dentro del elemento Border en XAML. Si los elementos TextBlock no se convierten en elementos secundarios del elemento Border, se elimina la necesidad de un panel que contenga los elementos TextBlock.
 - Por último, podrías colocar los elementos TextBlock dentro de un elemento StackPanel y establecer las propiedades de borde en el elemento StackPanel, en lugar de usar un elemento Border explícito. Sin embargo, el elemento Border es un control más ligero que un elemento StackPanel, por lo que tiene un impacto mucho menor en el rendimiento cuando se representa.

## <a name="control-template"></a>Plantilla de control
La plantilla de control de un elemento contiene los elementos visuales que muestran el estado, como la selección, el paso del puntero y el foco. Estos elementos visuales se representan encima o debajo de la plantilla de datos. Aquí se muestran algunos de los elementos visuales predeterminados comunes que se dibujan en la plantilla del control ListView.

- Mantenimiento del mouse: rectángulo gris claro que se dibuja debajo de la plantilla de datos.  
- Selección: rectángulo azul claro que se dibuja debajo de la plantilla de datos. 
- Foco del teclado: borde de puntos blanco y negro que se dibuja encima de plantilla de elemento. 

![Elementos visuales de estado de vista de lista](images/listview-state-visuals.png)

La vista de lista combina los elementos de la plantilla de datos y la plantilla de control para crear los elementos visuales finales representados en la pantalla. Aquí se muestran los elementos visuales de estado en el contexto de una vista de lista.

![Vista de lista con los elementos de diferentes estados](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

Como se mencionó anteriormente acerca de las plantillas de datos, el número de elementos XAML que se crean para cada elemento puede tener un impacto significativo en el rendimiento de una vista de lista. Dado que la plantilla de datos y la plantilla de control se combinan para mostrar cada elemento, el número real de elementos necesarios para mostrar un elemento incluye los elementos de ambas plantillas.

Los controles ListView y GridView están optimizados para reducir el número de elementos XAML creados por elemento. Los elementos visuales **ListViewItem** los crea la clase [ListViewItemPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx), que es un elemento XAML especial que muestra elementos visuales complejos de foco, selección y otros estados visuales, sin la sobrecarga de numerosas clases UIElement.
 
> [!NOTE]
> En las aplicaciones para UWP para Windows 10, tanto **ListViewItem** como **GridViewItem** usan la clase **ListViewItemPresenter**; la clase GridViewItemPresenter está en desuso y no debes usarla. Los elementos ListViewItem y GridViewItem establecen diferentes valores de propiedad en la clase ListViewItemPresenter para conseguir distintas apariencias predeterminadas).

Para modificar la apariencia del contenedor de elementos, usa la propiedad [ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) y proporciona una clase [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx) con la propiedad [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.targettype.aspx) establecida en **ListViewItem** o **GridViewItem**.

En este ejemplo, debes agregar espaciado interno al elemento ListViewItem para crear algo de espacio entre los elementos en la lista.

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Ahora la vista de lista tendrá esta apariencia con espacio entre los elementos.

![Elementos de la vista de lista con espaciado interno aplicado](images/listview-data-template-1.png)

En el estilo predeterminado ListViewItem, la propiedad **ContentMargin** de ListViewItemPresenter tiene una extensión [TemplateBinding](https://msdn.microsoft.com/windows/uwp/xaml-platform/templatebinding-markup-extension) para la propiedad **Padding** de (`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`). Cuando se establece la propiedad Padding, ese valor se pasa realmente a la propiedad ContentMargin de ListViewItemPresenter.

Para modificar otras propiedades de ListViewItemPresenter que no están vinculadas mediante plantilla a las propiedades de ListViewItem, debes volver a crear la plantilla de ListViewItem con una nueva clase ListViewItemPresenter en la que puedas modificar las propiedades. 

> [!NOTE]
> Los estilos predeterminados ListViewItem y GridViewItem establecen muchas propiedades en la clase ListViewItemPresenter. Debes comenzar siempre con una copia del estilo predeterminado y modificar solo las propiedades que necesites. De lo contrario, es probable que los elementos visuales no se muestren según lo previsto porque algunas propiedades no se establecerán correctamente.

**Para crear una copia de la plantilla predeterminada en Visual Studio**
 
1. Abre el panel Esquema del documento (**Ver > Otras ventanas > Esquema del documento**).
2. Selecciona el elemento de lista o cuadrícula que quieres modificar. En este ejemplo, se modifica el elemento `colorsGridView`.
3. Haz clic con el botón derecho y selecciona **Edit Additional Templates > Edit Generated Item Container (ItemContainerStyle) > Editar una copia**.
    ![Editor de Visual Studio](images/listview-itemcontainerstyle-vs.png)
4. En el cuadro de diálogo Create Style Resource, escribe un nombre para el estilo. En este ejemplo, se usa `colorsGridViewItemStyle`.
    ![Cuadro de diálogo Create Style Resource de Visual Studio (images/listview-style-resource-vs.png)

Se agrega una copia del estilo predeterminado a la aplicación como un recurso y la propiedad **GridView.ItemContainerStyle** se establece en ese recurso, como se muestra en este código XAML. 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

Ahora puedes modificar las propiedades de la clase ListViewItemPresenter para controlar la casilla de selección, el posicionamiento de los elementos y los colores del pincel para los estados visuales. 

#### <a name="inline-and-overlay-selection-visuals"></a>Elementos visuales de selección Inline y Overlay

ListView y GridView indican los elementos seleccionados de diferentes maneras según el control y la propiedad [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx). Para obtener más información sobre la selección de la vista de lista, consulta [ListView y GridView](listview-and-gridview.md). 

Si **SelectionMode** está establecido en **Multiple**, se muestra una casilla de selección como parte de la plantilla de control del elemento. Puedes usar la propiedad [SelectionCheckMarkVisualEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) para desactivar la casilla de selección en el modo de selección Multiple. Sin embargo, esta propiedad se omite en otros modos de selección, por lo que no se puede activar la casilla en el modo de selección Extended o Single.

Puedes establecer la propiedad [CheckMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode.aspx) para especificar si la casilla debe mostrarse con el estilo en línea o el estilo de superposición.

- **Inline**: Este estilo muestra la casilla situada a la izquierda del contenido y los colores de fondo del elemento contenedor para indicar la selección. Este es el estilo predeterminado del control ListView.
- **Superposición**: Este estilo muestra la casilla de verificación en la parte superior del contenido y sólo el borde del contenedor de elementos para indicar la selección de colores. Este es el estilo predeterminado del control GridView.

En esta tabla se muestran los elementos visuales predeterminados usados para indicar la selección.

SelectionMode:&nbsp;&nbsp; | Single/Extended | Multiple
---------------|-----------------|---------
Inline | ![Selección única o extendida en línea](images/listview-single-selection.png) | ![Selección múltiple en línea](images/listview-multi-selection.png)
Overlay | ![Selección única o extendida de superposición](images/gridview-single-selection.png) | ![Selección múltiple de superposición](images/gridview-multi-selection.png)

> [!NOTE]
> En este ejemplo y en los siguientes, los elementos de datos de cadena simple se muestran sin plantillas de datos para enfatizar los elementos visuales que ofrece la plantilla de control.

También se incluyen varias propiedades de pincel para cambiar los colores de la casilla. Veamos estos elementos y otras propiedades de pincel.

#### <a name="brushes"></a>Pinceles 

Muchas de las propiedades especifican los pinceles usados para los distintos estados visuales. Es posible que quieras modificarlas para que coincidan con el color de tu marca. 

En esta tabla se muestran los estados visuales Common y Selection de ListViewItem, así como los pinceles usados para representar los elementos visuales de cada estado. Las imágenes muestran los efectos de los pinceles en ambos estilos visuales de selección en línea y de superposición.

> [!NOTE]
> En esta tabla, los valores de color modificados de los pinceles son colores con nombre codificados de forma rígida y los colores están seleccionados para ser más patentes cuando se apliquen en la plantilla. Estos no son los colores predeterminados de los estados visuales. Si modificas los colores predeterminados de la aplicación, debes usar los recursos de pincel para modificar los valores de color, como se muestra en la plantilla predeterminada.

Nombre del estado o pincel | Estilo en línea | Estilo de superposición
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush="Red"</b></li></ul> | ![Selección de elemento en línea normal](images/listview-item-normal.png) | ![Selección de elemento de superposición normal](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground="DarkOrange"</b></li><li><b>PointerOverBackground="MistyRose"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Selección de elemento en línea con el puntero encima](images/listview-item-pointerover.png) | ![Selección de elemento de superposición con el puntero encima](images/gridview-item-pointerover.png)
<b>Pressed</b><ul><li><b>PressedBackground="LightCyan"</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![Selección de elemento en línea presionada](images/listview-item-pressed.png) | ![Selección de elemento de superposición presionada](images/gridview-item-pressed.png)
<b>Selected</b><ul><li><b>SelectedForeground="Navy"</b></li><li><b>SelectedBackground="Khaki"</b></li><li><b>CheckBrush="Green"</b></li><li>CheckBoxBrush="Red" (solo en línea)</li></ul> | ![Selección de elemento en línea seleccionada](images/listview-item-selected.png) | ![Selección de elemento de superposición seleccionada](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground="Lavender"</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (solo superposición)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (solo en línea)</li></ul> | ![Selección de elemento en línea con el puntero encima seleccionada](images/listview-item-pointeroverselected.png) | ![Selección de elemento de superposición con el puntero encima seleccionada](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground="MediumTurquoise"</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (solo superposición)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (solo en línea)</li></ul> | ![Selección de elemento en línea presionada seleccionada](images/listview-item-pressedselected.png) | ![Selección de elemento de superposición presionada seleccionada](images/gridview-item-pressedselected.png)
<b>Focused</b><ul><li><b>FocusBorderBrush="Crimson"</b></li><li><b>FocusSecondaryBorderBrush="Gold"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Selección de elemento en línea enfocada](images/listview-item-focused.png) | ![Selección de elemento de superposición enfocada](images/gridview-item-focused.png)

ListViewItemPresenter tiene otras propiedades de pincel para los marcadores de posición de datos y los estados de arrastre. Si usas la carga incremental o la función de arrastrar y colocar en la vista de lista, debes considerar si también es necesario modificar estas propiedades de pincel adicionales. Consulta la clase ListViewItemPresenter para obtener una lista completa de propiedades que se pueden modificar. 

### <a name="expanded-xaml-item-templates"></a>Plantillas de elementos XAML expandidas

Si necesitas realizar más modificaciones de las que se permiten en las propiedades de **ListViewItemPresenter** (por ejemplo, si necesitas cambiar la posición de la casilla), puedes usar la las plantillas *ListViewItemExpanded* o *GridViewItemExpanded*. Estas plantillas se incluyen con los estilos predeterminados en generic.xaml. Siguen el patrón de XAML estándar de compilación de todos los elementos visuales a partir de propiedades UIElement individuales.

Como se mencionó anteriormente, el número de propiedades UIElement de una plantilla de elemento tiene un impacto significativo en el rendimiento de la vista de lista. Reemplazar ListViewItemPresenter por las plantillas XAML expandidas aumenta enormemente el recuento de elementos, y no se recomienda si la vista de lista va a mostrar un gran número de elementos o si el rendimiento es una preocupación.

> [!NOTE]
> **ListViewItemPresenter** se admite solo cuando la propiedad [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) de la vista de lista es [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx) o [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx). Si cambias la propiedad ItemsPanel para usar [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.variablesizedwrapgrid.aspx), [WrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.wrapgrid.aspx) o [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx), la plantilla de elemento cambiará automáticamente a la plantilla XAML expandida. Para obtener más información, consulta [Optimización de interfaz de usuario de ListView y GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

Para personalizar una plantilla XAML expandida, debes realizar una copia de esta en la aplicación y establecer la propiedad **ItemContainerStyle** en la copia.

**Para copiar la plantilla expandida**
1. Establece la propiedad ItemContainerStyle como se muestra aquí para el control ListView o GridView.
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. En el panel Propiedades de Visual Studio, expande la sección Varios y busca la propiedad ItemContainerStyle. (Asegúrate de que esté seleccionado ListView o GridView).
3. Haz clic en el marcador de propiedad de la propiedad ItemContainerStyle. (Es el pequeño cuadro junto al cuadro de texto. Su coloreed verde para mostrar que se establece en un StaticResource.) Se abre el menú de propiedad.
4. En el menú de propiedades, haz clic en **Convertir en nuevo recurso**. 
    
    ![Menú de propiedades de Visual Studio](images/listview-convert-resource-vs.png)
5. En el cuadro de diálogo Create Style Resource, escribe un nombre para el recurso y haz clic en Aceptar.

Se crea una copia de la plantilla expandida de generic.xaml en tu aplicación, que puedes modificar según sea necesario.


## <a name="related-articles"></a>Artículos relacionados

- [Listas](lists.md)
- [ListView y GridView](listview-and-gridview.md)


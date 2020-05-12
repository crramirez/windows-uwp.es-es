---
Description: Muestra una a una las imágenes de una colección, como las fotos de un álbum o los elementos de una página de detalles de un producto.
title: Directrices para controles de vista invertida
ms.assetid: A4E05D92-1A0E-4CDD-84B9-92199FF8A8A3
label: Flip view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f4871b4a43284052a89d0fd37412528840e1ea5a
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968740"
---
# <a name="flip-view"></a>Inversión de la vista

Usa la vista invertida para explorar una a una las imágenes u otros elementos de una colección, como las fotos de un álbum o los elementos de una página con los detalles de un producto. Si usas dispositivos táctiles, puedes desplazarte por la colección deslizando el dedo por los elementos. En cambio, si usas un mouse, aparecerán botones de navegación al pasar el mouse por encima. En el caso del teclado, te puedes desplazar por la colección con las teclas de dirección.

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](/windows/uwp/design/style/rounded-corner). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma:** [Clase FlipView](/uwp/api/windows.ui.xaml.controls.flipview), [Propiedad ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [Propiedad ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

La vista invertida es ideal para examinar imágenes de colecciones pequeñas o medianas (de hasta 25 elementos, más o menos). Algunos ejemplos de estas colecciones son elementos de la página de detalles de un producto o las fotos de un álbum de fotos. Aunque no recomendamos la vista para alternar para colecciones más grandes, es un control habitual para ver imágenes individuales en un álbum de fotos.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/FlipView">abrirla y ver FlipView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un diseño típico de la vista para alternar es la navegación horizontal, comenzando por el extremo izquierdo y desplazándose hacia la derecha. Este diseño funciona bien tanto en la orientación vertical como horizontal en todos los dispositivos:

![Ejemplo de diseño de la vista para alternar horizontal](images/controls_flipview_horizonal.jpg)

Asimismo, también puedes examinar verticalmente una vista invertida:

![Ejemplo de vista invertida vertical](images/controls_flipview_vertical.jpg)

## <a name="create-a-flip-view"></a>Crear una vista invertida nueva

La clase FlipView forma parte de [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol), de modo que puede contener una colección de elementos de cualquier tipo. Para rellenar la vista, agrega elementos a la colección [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) o establece la propiedad [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) en un origen de datos.

De manera predeterminada, un elemento de datos se muestra en la vista invertida como una representación de cadena del objeto de datos al que está enlazado. Para especificar exactamente cómo se muestran los elementos en la vista invertida, puedes crear una clase [**DataTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) para definir el diseño de los controles que se usan para mostrar un elemento individual. Los controles del diseño se pueden enlazar a las propiedades de un objeto de datos o tener contenido definido en línea. A continuación, asigna la clase DataTemplate a la propiedad [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) de la clase FlipView.

### <a name="add-items-to-the-items-collection"></a>Agregar elementos a la colección Items

Puedes agregar elementos a la colección [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) mediante lenguaje XAML o código. Normalmente, los elementos se agregan de esta forma si tienes un pequeño número de elementos que no cambian y que se definen fácilmente en XAML, o si generas los elementos en el código en tiempo de ejecución. Esta es una vista invertida con elementos definidos en línea.

```xaml
<FlipView x:Name="flipView1">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

```csharp
// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.Items.Add("Item 1");
flipView1.Items.Add("Item 2");

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

Cuando agregas elementos a una vista invertida, se colocan automáticamente en un contenedor [**FlipViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipViewItem). Para cambiar la manera en que se muestra un elemento, puedes configurar la propiedad [**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) para aplicar un estilo al contenedor de elementos. 

Cuando defines los elementos en XAML, se agregan automáticamente a la colección Items.

### <a name="set-the-items-source"></a>Establecer el origen de los elementos

Por lo general, se usa una vista invertida para mostrar los datos de un origen como una base de datos o Internet. Para rellenar una vista invertida desde un origen de datos, debes establecer la propiedad [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) en una colección de elementos de datos.

Aquí, la propiedad ItemsSource de la vista invertida está establecida en el código, directamente en la instancia de una colección.

```csharp
// Data source.
List<String> itemsList = new List<string>();
itemsList.Add("Item 1");
itemsList.Add("Item 2");

// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.ItemsSource = itemsList;
flipView1.SelectionChanged += FlipView_SelectionChanged;

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

También puedes enlazar la propiedad ItemsSource a una colección en XAML. Para obtener más información, consulta el tema [Enlace de datos con XAML](../../data-binding/data-binding-quickstart.md).

En este ejemplo, ItemsSource está enlazado a una clase [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) denominada `itemsViewSource`. 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**Nota**&nbsp;&nbsp;Puedes rellenar una vista invertida agregando elementos a su colección Items o estableciendo su propiedad ItemsSource, pero no puedes usar ambos métodos al mismo tiempo. Si estableces la propiedad ItemsSource y agregas un elemento en XAML, el elemento agregado se omite. Si estableces la propiedad ItemsSource y decides agregar un elemento a la colección Items en código, se inicia una excepción.

### <a name="specify-the-look-of-the-items"></a>Especificar el aspecto de los elementos

De manera predeterminada, un elemento de datos se muestra en la vista invertida como una representación de cadena del objeto de datos al que está enlazado. Por lo general, preferirás mostrar una presentación más completa de los datos. Para especificar con exactitud cómo se mostrarán los elementos en la vista invertida, debes crear una clase [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). El lenguaje XAML de la clase DataTemplate define el diseño y la apariencia de los controles usados para mostrar un elemento individual. Los controles del diseño se pueden enlazar a las propiedades de un objeto de datos o tener contenido definido en línea. La clase DataTemplate se asigna a la propiedad [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) del control FlipView.

En este ejemplo, la clase ItemTemplate de un control FlipView se define en línea. Asimismo, se agrega una superposición a la imagen para mostrar el nombre de la imagen. 

```XAML
<FlipView x:Name="flipView1" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

Esta es la apariencia del diseño definido por la plantilla de datos.

Plantilla de datos de la vista invertida.

### <a name="set-the-orientation-of-the-flip-view"></a>Establecer la orientación de la vista invertida

De manera predeterminada, la vista invertida se voltea de forma horizontal. Para voltearla de forma vertical, usa un panel StackPanel que tenga orientación vertical, al igual que la propiedad [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) de la vista invertida.

Este ejemplo muestra cómo usar un panel StackPanel con orientación vertical como la propiedad ItemsPanel de un control FlipView.

```XAML
<FlipView x:Name="flipViewVertical" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    
    <!-- Use a vertical stack panel for vertical flipping. -->
    <FlipView.ItemsPanel>
        <ItemsPanelTemplate>
            <VirtualizingStackPanel Orientation="Vertical"/>
        </ItemsPanelTemplate>
    </FlipView.ItemsPanel>
    
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

Esta es la apariencia de la vista invertida con orientación vertical.

![Ejemplo de vista invertida vertical](images/controls_flipview_vertical.jpg)

## <a name="adding-a-context-indicator"></a>Agregar un indicador de contexto

Un indicador de contexto en una vista invertida proporciona un punto de referencia útil. Los puntos de un indicador de contexto estándar no son interactivos. Tal como se muestra en este ejemplo, la mejor ubicación es, por lo general, centrada y debajo de la galería:

![Ejemplo de un indicador de página](images/controls_pageindicator.png)

En el caso de colecciones más grandes (de 10 a 25 elementos), considera la posibilidad de usar un indicador que proporcione más contexto, como una tira de imágenes de miniaturas. A diferencia de un indicador de contexto que usa puntos simples, cada miniatura en la tira de imágenes muestra una versión pequeña de la imagen correspondiente y debería ser seleccionable:

![Ejemplo de indicador de contexto](images/controls_contextindicator.jpg)

Para obtener un ejemplo de código que muestre cómo agregar un indicador de contexto a FlipView, consulta [Muestra de FlipView XAML](https://code.msdn.microsoft.com/windowsapps/XAML-FlipView-control-0ae45312).

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

-   Las vistas invertidas funcionan mejor en colecciones de hasta 25 elementos aproximadamente.
-   Evita el uso de un control de vista para alternar para colecciones más grandes, ya que el movimiento repetitivo de desplazamiento entre los elementos puede ser tedioso. Una excepción serían los álbumes de fotos, que a menudo tienen cientos o miles de imágenes. Los álbumes de fotos casi siempre cambian a una vista para alternar una vez que se ha seleccionado una foto en el diseño de la vista de cuadrícula. Para otras colecciones grandes, considera la posibilidad de usar una [vista de lista o de cuadrícula](lists.md).
-   En cuanto a los indicadores de contexto:
    -   El orden de los puntos (o sea cual sea el marcador visual que elijas) funciona mejor cuando están centrados y debajo de la galería de movimiento panorámico horizontal.
    -   Si quieres usar un indicador de contexto en una galería de movimiento panorámico vertical, este funciona mejor si se coloca centrado y a la derecha de las imágenes.
    -   El punto resaltado indica el elemento actual. Normalmente el punto resaltado es blanco y los otros puntos son grises.
    -   El número de puntos puede variar, pero no coloques demasiados porque el usuario tendría dificultades para encontrar lo que necesita. Con un máximo de 10 puntos suele ser suficiente.

## <a name="globalization-and-localization-checklist"></a>Lista de comprobación Globalización y localización

<table>
<tr>
<th>Consideraciones bidireccionales</th><td>Usa la creación de reflejos estándar para los idiomas de derecha a izquierda. Los controles para ir atrás y adelante deben basarse en la dirección del idioma, por lo que para los idiomas de derecha a izquierda, el botón derecho debe navegar hacia atrás y el botón izquierdo debe navegar hacia delante.</td>
</tr>

</table>

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Directrices sobre listas](lists.md)
- [**Clase FlipView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

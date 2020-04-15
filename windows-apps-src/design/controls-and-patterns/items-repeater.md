---
Description: ItemsRepeater es un control ligero para generar y presentar una colección de elementos.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5782c6e9ba42fed07c2b1382f2d17b1d311d0a13
ms.sourcegitcommit: 1b06c27e7fa4726fd950cbeaf05206c0a070e3c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80893465"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Usa una clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para crear experiencias de colección personalizadas con un sistema de diseño flexible, vistas personalizadas y virtualización.

A diferencia de [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no proporciona una experiencia de usuario final completa, ya que no tiene una interfaz de usuario predeterminada y no proporciona directivas para el foco, la selección o la interacción del usuario. En su lugar, es un bloque de creación que puedes usar para crear tus propios controles personalizados y experiencias únicas basadas en la colección. Aunque no cuenta con ninguna directiva integrada, te permite asociar directivas para compilar la experiencia que necesites. Por ejemplo, puedes definir el diseño que se va a usar, la directiva de teclado, la directiva de selección, etc.

En términos conceptuales, [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) podría considerarse un panel controlado por datos, en lugar de un control completo, como ListView. Tú te encargas de especificar una colección de elementos de datos para su visualización, una plantilla de elemento que genere un elemento de interfaz de usuario para cada elemento de datos y un diseño que determine el tamaño y la colocación de los elementos. ItemsRepeater generará elementos secundarios en función del origen de datos y los mostrará según lo que especifiquen la plantilla de elemento y el diseño. No hace falta que los elementos mostrados sean homogéneos, ya que ItemsRepeater puede cargar contenido para representar los elementos de datos según los criterios que especifiques en un selector de plantillas de datos.

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control **ItemsRepeater** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library ](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de la biblioteca de interfaz de usuario de Windows**: [clase ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
>
> **API de plataforma:** [Clase ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa una clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) con el fin de crear visualizaciones personalizadas para las colecciones de datos. Aunque se puede usar para presentar un conjunto básico de elementos, a menudo la usarás como elemento de visualización en la plantilla de un control personalizado.

Si necesitas un control predeterminado para mostrar datos en una lista o cuadrícula con una personalización mínima, considera la posibilidad de usar una clase [ListView](/uwp/api/windows.ui.xaml.controls.listview) o [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

ItemsRepeater no tiene una colección de elementos integrada. Si necesitas proporcionar una colección de elementos directamente, en lugar de enlazar a un origen de datos independiente, es probable que requieras una experiencia con directivas más elevadas y que debas usar [ListView](/uwp/api/windows.ui.xaml.controls.listview) o [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

Tanto [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) como ItemsRepeater permiten experiencias de colección personalizables, pero ItemsRepeater admite la virtualización de diseños de la interfaz de usuario, a diferencia de ItemsControl. Se recomienda usar ItemsRepeater en lugar de ItemsControl, ya sea para simplemente presentar algunos elementos de los datos o para compilar un control de colección personalizado.

> [!NOTE]
> Si te encuentras en una situación en la que ItemsControl satisface tus necesidades y ItemsRepeater no lo hace, háznoslo saber con un comentario en el [proyecto de GitHub de la biblioteca de interfaz de usuario de Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para abrir la aplicación y ver <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Desplazamiento con ItemsRepeater

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no se deriva de [**Control**](/uwp/api/windows.ui.xaml.controls.control), por lo que no tiene una plantilla de control. Por lo tanto, no tiene desplazamiento integrado, a diferencia de ListView o de otros controles de colección.

Cuando uses **ItemsRepeater**, debes proporcionar funcionalidad de desplazamiento. Para ello, inclúyela en un control [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer).

> [!NOTE]
> Si la aplicación se va a ejecutar en versiones anteriores de Windows (publicadas *antes* de Windows 10, versión 1809), también tendrás que hospedar el control **ScrollViewer** dentro de [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost). 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> Si la aplicación solo se va a ejecutar en versiones recientes de Windows 10, versión 1809 y posteriores, no es necesario usar [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost).
>
> Antes de Windows 10, versión 1809, **ScrollViewer** no implementaba la interfaz [**IScrollAnchorProvider**](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) que **ItemsRepeater** requiere.  **ItemsRepeaterScrollHost** permite que **ItemsRepeater** se coordine con **ScrollViewer** en versiones anteriores para conservar correctamente la ubicación visible de los elementos que está viendo el usuario.  En caso contrario, podría parecer que los elementos se mueven o desaparecen repentinamente cuando se cambian los elementos de la lista o se modifica el tamaño de la aplicación.

## <a name="create-an-itemsrepeater"></a>Creación de ItemsRepeater

Para usar una clase [**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), debes proporcionarle los datos que se van a mostrar. Para ello, establece la propiedad **ItemsSource**. Después, indícale cómo debe mostrar los elementos mediante la propiedad [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate).

### <a name="itemssource"></a>ItemsSource

Para rellenar la vista, establece la propiedad [**ItemsSource**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) en una colección de elementos de datos. Aquí, la propiedad **ItemsSource** está establecida en el código, directamente en una instancia de una colección.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

También puedes enlazar la propiedad **ItemsSource** a una colección en XAML. Para obtener más información sobre el enlace de datos, consulta [Introducción al enlace de datos](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
Para especificar cómo se visualiza un elemento de datos, establece la propiedad [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) en una clase [**DataTemplate**](/uwp/api/windows.ui.xaml.datatemplate) o [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) que hayas definido. La plantilla de datos define cómo se visualizan los datos. De manera predeterminada, el elemento se muestra en la vista con un **TextBlock** que usa una representación de cadena del objeto de datos.

Pero seguramente te interese mostrar una presentación más enriquecida de los datos mediante una plantilla que defina el diseño y la apariencia de uno o varios controles que usarás para mostrar un elemento individual. Los controles que uses en la plantilla se pueden enlazar a las propiedades de un objeto de datos o pueden tener contenido estático definido en línea.

#### <a name="datatemplate"></a>DataTemplate
En este ejemplo, el objeto de datos es una cadena simple. La clase **DataTemplate** incluye una imagen a la izquierda del texto y aplica estilo al **TextBlock** para mostrar la cadena en un color verde azulado.

> [!NOTE]
> Cuando uses la [extensión de marcado x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) en **DataTemplate**, debes especificar DataType (`x:DataType`) en DataTemplate.

```xaml
<DataTemplate x:DataType="x:String">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="47"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/placeholder.png" Width="32" Height="32"
               HorizontalAlignment="Left"/>
        <TextBlock Text="{x:Bind}" Foreground="Teal"
                   FontSize="15" Grid.Column="1"/>
    </Grid>
</DataTemplate>
```

Aquí puedes ver el aspecto que tendrán los elementos cuando se muestren con esta clase **DataTemplate**.

![Elementos mostrados con una plantilla de datos](images/listview-itemstemplate.png)

El número de elementos que se usan en la clase **DataTemplate** para un elemento puede afectar considerablemente al rendimiento en caso de que la vista muestre una gran cantidad de elementos. Para obtener más información y ejemplos de cómo usar la clase **DataTemplate** para definir la apariencia de los elementos de la lista, consulta [Plantillas y contenedores de elementos](item-containers-templates.md).

> [!TIP]
> Para mayor comodidad si quieres declarar la plantilla insertada en lugar de referenciada como un recurso estático, puedes especificar la clase **DataTemplate** o **DataTemplateSelector** como el elemento secundario directo de **ItemsRepeater**.  Se asignará como el valor de la propiedad **ItemTemplate**. Por ejemplo, esto es válido:
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> A diferencia de **ListView** y otros controles de colección, **ItemsRepeater** no encapsula los elementos de **DataTemplate** con un contenedor de elementos adicionales que incluya la directiva predeterminada, como los márgenes, el relleno, los objetos visuales de selección o un puntero encima del estado visual. En su lugar, **ItemsRepeater** solo presenta lo que se define en la clase **DataTemplate**. Si quieres que los elementos tengan el mismo aspecto que un elemento de la vista de lista, puedes incluir explícitamente un contenedor, como **ListViewItem**, en la plantilla de datos. **ItemsRepeater** mostrará los objetos visuales de **ListViewItem**, pero no usará automáticamente otras funcionalidades, como la selección o la visualización de la casilla de selección múltiple.
>
> De forma similar, si la colección de datos es una colección de controles reales, como **Button** (`List<Button>`), puedes colocar un **ContentPresenter** en la clase **DataTemplate** para mostrar el control.

#### <a name="datatemplateselector"></a>DataTemplateSelector

No es necesario que los elementos que muestres en la vista sean del mismo tipo. Puedes proporcionar a la propiedad [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) una clase [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) para seleccionar distintas clases **DataTemplate** según los criterios que especifiques.

En este ejemplo se da por supuesto que se ha definido una clase **DataTemplateSelector** que decide entre dos diferentes clases **DataTemplate** para representar un elemento grande y pequeño.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Al definir una clase **DataTemplateSelector** para usarla con **ItemsRepeater**, solo tienes que implementar una invalidación para el método [**SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_). Para obtener más información y ejemplos, consulta [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Una alternativa a las clases **DataTemplate** para administrar la creación de elementos en escenarios más avanzados consiste en implementar tu propio [**Windows.UI.Xaml.Controls.IElementFactory**](/uwp/api/windows.ui.xaml.controls.ielementfactory) para usarlo como **ItemTemplate**.  Se encargará de generar contenido cuando se solicite.

## <a name="configure-the-data-source"></a>Configuración del origen de datos

Usa la propiedad [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) para especificar la colección que se va a emplear para generar el contenido de los elementos. Puedes establecer la propiedad ItemsSource en cualquier tipo que implemente **IEnumerable**. Las interfaces de colección adicionales que implemente el origen de datos determinarán qué funcionalidades están a disposición de ItemsRepeater para interactuar con los datos.

En esta lista se muestran las interfaces disponibles y cuándo se debe considerar la posibilidad de usarlas.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Puede usarse para conjuntos de datos pequeños y estáticos.

    Como mínimo, el origen de datos debe implementar la interfaz IEnumerable/IIterable. Si esto es todo lo que admite, el control iterará a través de todo el contenido una vez para crear una copia que pueda usar con el fin de acceder a los elementos a través de un valor de índice.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Puede usarse para conjuntos de datos estáticos y de solo lectura.

    Permite al control acceder a los elementos por índice y evita la copia interna redundante.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Puede usarse para conjuntos de datos estáticos.

    Permite al control acceder a los elementos por índice y evita la copia interna redundante.

    **Advertencia**: Los cambios realizados en la lista/vector sin implementar [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) no se reflejarán en la interfaz de usuario.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Recomendada para admitir la notificación de cambios.

    Permite al control observar los cambios y reaccionar ante ellos en el origen de datos, así como reflejar dichos cambios en la interfaz de usuario.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Admite la notificación de cambios.

    Al igual que la interfaz **INotifyCollectionChanged**, permite al control observar los cambios en el origen de datos y reaccionar ante ellos.

    **Advertencia**: Windows.Foundation.IObservableVector\<T> no es compatible con una acción de tipo "Mover". Esto puede hacer que la interfaz de usuario de un elemento pierda su estado visual.  Por ejemplo, un elemento que actualmente está seleccionado o que tiene el foco donde el movimiento se consigue mediante una acción "Eliminar" seguida de una acción "Agregar" perderá el foco y dejará de estar seleccionado.

    Platform.Collections.Vector\<T> usa IObservableVector\<T> y tiene esta misma limitación. Si es necesaria la compatibilidad con una acción de tipo "Mover", usa la interfaz **INotifyCollectionChanged**.  La clase .NET ObservableCollection\<T> usa **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Cuando se puede asociar un identificador único con cada elemento.  Se recomienda al usar "Restablecer" como acción de cambio de la colección.

    Permite al control recuperar de manera muy eficaz la interfaz de usuario existente después de recibir una acción de restablecimiento completo como parte de un evento **INotifyCollectionChanged** o **IObservableVector**. Después de recibir un restablecimiento, el control usará el identificador único proporcionado para asociar los datos actuales con los elementos que ya ha creado. Sin la asignación de clave a índice, el control daría por supuesto que tiene que volver a empezar desde cero al crear la interfaz de usuario para los datos.

Las interfaces enumeradas anteriormente, excepto IKeyIndexMapping, proporcionan el mismo comportamiento en ItemsRepeater que en ListView y GridView.


Las interfaces siguientes en una propiedad ItemsSource habilitan funcionalidades especiales en los controles ListView y GridView, pero actualmente no tienen ningún efecto en ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Queremos sus comentarios. Nos gustaría conocer tu opinión sobre el [proyecto de GitHub de la biblioteca de interfaz de usuario de Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues). No dudes en exponer lo que piensas sobre las propuestas existentes, como la [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): Agregar compatibilidad con la carga incremental para ItemsRepeater.

Un enfoque alternativo para cargar incrementalmente los datos cuando el usuario se desplaza hacia arriba o hacia abajo consiste en observar la posición de la ventanilla de ScrollViewer y cargar más datos a medida que la ventanilla se acerca a su tamaño.

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>Cambiar el diseño de elementos

Los elementos que muestra [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) se organizan mediante un objeto [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) que administra el tamaño y la posición de sus elementos secundarios. Cuando se usa con ItemsRepeater, el objeto Layout permite la virtualización de la interfaz de usuario. Los diseños proporcionados son [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) y [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). De forma predeterminada, ItemsRepeater usa una clase StackLayout con orientación vertical.

### <a name="stacklayout"></a>StackLayout

La clase [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) organiza los elementos en una sola línea que se puede orientar horizontal o verticalmente.

Puedes establecer la propiedad [Spacing](/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) para ajustar la cantidad de espacio entre los elementos. El espaciado se aplica en la dirección de la propiedad [Orientation](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation) del diseño.

![Espaciado del diseño de pila](images/stack-layout.png)

En este ejemplo se muestra cómo establecer la propiedad ItemsRepeater.Layout en una clase StackLayout con orientación horizontal y un espaciado de 8 píxeles.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

La clase [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) coloca los elementos secuencialmente en un diseño de ajuste. Los elementos se disponen en orden de izquierda a derecha cuando la propiedad [Orientation](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) es **Horizontal** y de arriba a abajo cuando es **Vertical**. Todos los elementos tienen el mismo tamaño.

![Espaciado de diseño de cuadrícula uniforme](images/uniform-grid-layout.png)

El número de elementos de cada fila de un diseño horizontal viene determinado por el ancho mínimo del elemento. El número de elementos de cada columna de un diseño vertical viene determinado por el alto mínimo del elemento.

- Puedes proporcionar explícitamente un tamaño mínimo para su uso mediante las propiedades [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) y [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth).
- Si no especificas un tamaño mínimo, el tamaño medido del primer elemento se considera el tamaño mínimo por elemento.

También puedes establecer un espacio mínimo para el diseño que debe incluirse entre las filas y columnas mediante las propiedades [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) y [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing).

![Tamaño y espaciado de cuadrícula uniformes](images/uniform-grid-sizing-spacing.png)

Una vez que se ha determinado el número de elementos de una fila o columna en función del tamaño y el espaciado mínimos del elemento, podría quedar espacio sin usar después del último elemento de la fila o columna (como se muestra en la imagen anterior). Puedes especificar si se ignora el espacio adicional, si se usa para aumentar el tamaño de cada elemento o si se emplea para crear más espacio entre los elementos. Esto se controla mediante las propiedades [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) y [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification).

Puedes establecer la propiedad [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) para especificar cómo se aumenta el tamaño del elemento para rellenar el espacio sin usar.

En esta lista se muestran los valores disponibles. Las definiciones dan por supuesto que el valor predeterminado de **Orientation** es **Horizontal**.

- **None**: el espacio adicional se deja sin usar al final de la fila. Este es el valor predeterminado.
- **Fill**: los elementos obtienen un ancho adicional para usar el espacio disponible (en caso de orientación vertical, obtienen un alto adicional).
- **Uniform**: los elementos obtienen un ancho adicional para usar el espacio disponible y un alto adicional para mantener la relación de aspecto (el alto y el ancho se intercambian en caso de orientación vertical).

En esta imagen se muestra el efecto de los valores de **ItemsStretch** en un diseño horizontal.

![Ampliación de los elementos de cuadrícula uniforme](images/uniform-grid-item-stretch.png)

Cuando **ItemsStretch** es **None**, puedes establecer la propiedad [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) para especificar cómo se usa el espacio adicional para alinear los elementos.

En esta lista se muestran los valores disponibles. Las definiciones dan por supuesto que el valor predeterminado de **Orientation** es **Horizontal**.

- **Start**: los elementos se alinean con el inicio de la fila. El espacio adicional se deja sin usar al final de la fila. Este es el valor predeterminado.
- **Center**: los elementos se alinean en el centro de la fila. El espacio adicional se reparte por igual al principio y al final de la fila.
- **End**: los elementos se alinean con el final de la fila. El espacio adicional se deja sin usar al principio de la fila.
- **SpaceAround**: los elementos se distribuyen de manera uniforme. Se agrega la misma cantidad de espacio antes y después de cada elemento.
- **SpaceBetween**: los elementos se distribuyen de manera uniforme. Se agrega la misma cantidad de espacio entre cada elemento. No se agrega espacio ni al principio ni al final de la fila.
- **SpaceEvenly**: los elementos se distribuyen de manera uniforme con la misma cantidad de espacio entre cada elemento, así como al principio y al final de la fila.

En esta imagen se muestra el efecto de los valores de **ItemsStretch** en un diseño vertical (se aplica a columnas en lugar de a filas).

![Justificación de los elementos de cuadrícula uniforme](images/uniform-grid-item-justification.png)

> [!TIP]
> La propiedad **ItemsStretch** afecta al pase de _medición_ del diseño. La propiedad **ItemsJustification** afecta al pase de _organización_ del diseño.

En este ejemplo se muestra cómo establecer la propiedad **ItemsRepeater.Layout** en **UniformGridLayout**.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>Eventos de ciclo de vida

Al hospedar elementos en una clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), es posible que debas realizar alguna acción cuando se muestra o se deja de mostrar un elemento, como iniciar una descarga asincrónica de algo de contenido, asociar el elemento con un mecanismo para llevar un seguimiento de la selección o detener alguna tarea en segundo plano.

En un control de virtualización, no puedes depender de los eventos Loaded y Unloaded, ya que el elemento podría no haberse quitado del árbol visual dinámico al reciclarse. En su lugar, se proporcionan otros eventos para administrar el ciclo de vida de los elementos. En este diagrama se muestra el ciclo de vida de un elemento en una clase ItemsRepeater y el momento en que se producen los eventos relacionados.

![Diagrama de eventos del ciclo de vida](images/items-repeater-lifecycle.png)

- [**ElementPrepared**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) se produce cada vez que un elemento se prepara para su uso. Esto sucede para los elementos recién creados y para los elementos que ya existen y que van a volver a usarse desde la cola de reciclaje.
- [**ElementClearing**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) se produce de inmediato cada vez que un elemento se envía a la cola de reciclaje, como cuando queda fuera del intervalo de los elementos ejecutados.
- [**ElementIndexChanged**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) se produce para cada UIElement ejecutado cuando el índice del elemento que representa ha cambiado. Por ejemplo, cuando se agrega o se elimina otro elemento en el origen de datos, el índice de los elementos que vienen después en la ordenación reciben este evento.

En este ejemplo se muestra cómo podrías usar estos eventos para adjuntar un servicio de selección personalizado con el fin de llevar un seguimiento de la selección de elementos en un control personalizado que usa ItemsRepeater para mostrar los elementos.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>Ordenación, filtrado y restablecimiento de los datos

Al realizar acciones como filtrar u ordenar el conjunto de datos, lo más probable es que compares el conjunto de datos anterior con los datos nuevos y, luego, emitas notificaciones pormenorizadas de cambios mediante [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). Pero a menudo resulta más fácil reemplazar por completo los datos antiguos por los datos nuevos y desencadenar una notificación de cambios en la colección mediante la acción de [restablecimiento](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction).

Por lo general, un restablecimiento hace que un control libere los elementos secundarios existentes y vuelva a empezar, para lo que compila la interfaz de usuario desde el principio en la posición de desplazamiento 0, ya que no sabe exactamente cómo han cambiado los datos durante el restablecimiento.

Aun así, si la colección asignada como ItemsSource admite identificadores únicos mediante la implementación de la interfaz [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping), la clase ItemsRepeater puede identificar rápidamente:

- Los elementos de tipo UIElements reutilizables para los datos que existían antes y después del restablecimiento
- Los elementos previamente visibles que se quitaron
- Los elementos nuevos agregados que serán visibles

De este modo, ItemsRepeater evitará empezar de nuevo desde la posición de desplazamiento 0. Esto también le permitirá restaurar rápidamente los elementos de tipo UIElements para los datos que no han cambiado en un restablecimiento, lo que mejora el rendimiento.

En este ejemplo se ilustra cómo mostrar una lista de elementos en una pila vertical donde _MyItemsSource_ es un origen de datos personalizado que encapsula una lista de elementos subyacentes. Expone una propiedad _Data_ que se puede usar para volver a asignar una nueva lista que se empleará como origen de los elementos, que después desencadenará un restablecimiento.

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>Creación de un control de colección personalizado

Puedes usar la clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para crear un control de colección personalizado con su propio tipo de control para presentar cada elemento.

> [!NOTE]
> Esto es similar al uso de **ItemsControl**, pero en lugar de derivar de **ItemsControl** y colocar un elemento **ItemsPresenter** en la plantilla de control, derivas de **Control** e insertas una clase **ItemsRepeater** en la plantilla de control. El control de colección personalizado "tiene" una clase **ItemsRepeater**, no "es" una clase **ItemsControl**. Esto implica que debes elegir explícitamente qué propiedades se exponen, en lugar de seleccionar qué propiedades heredadas no se admiten.

En este ejemplo se muestra cómo se coloca una clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) en la plantilla de un control personalizado denominado _MediaCollectionView_ y cómo se exponen sus propiedades.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>Visualización de elementos agrupados

Puedes anidar una clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) en la propiedad [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) de otra clase ItemsRepeater para crear diseños de virtualización anidados. El marco hará un uso eficaz de los recursos, ya que minimiza la ejecución innecesaria de los elementos que no son visibles o no están cerca de la ventanilla actual.

En este ejemplo se muestra cómo puedes mostrar una lista de elementos agrupados en una pila vertical. La clase ItemsRepeater externa genera cada grupo. En la plantilla de cada grupo, otra clase ItemsRepeater genera los elementos.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->

<Page.Resources>
    <muxc:StackLayout x:Key="MyGroupLayout"/>
    <muxc:StackLayout x:Key="MyItemLayout" Orientation="Horizontal"/>
</Page.Resources>

<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          <TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```
En la imagen siguiente se muestra el diseño básico creado usando como pauta el ejemplo anterior.

![Diseño anidado con ItemsRepeater](images/items-repeater-nested-layout.png)

En este ejemplo se muestra un diseño para una aplicación con varias categorías que se pueden cambiar con las preferencias del usuario y que se presentan como listas de desplazamiento horizontal. El diseño de este ejemplo también se representa con la imagen anterior.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>Colocación de un elemento a la vista

El marco XAML ya controla la colocación de un elemento de tipo FrameworkElement a la vista cuando recibe el foco del teclado o del Narrador. Puede haber otros casos en los que necesites colocar un elemento explícitamente a la vista. Por ejemplo, en respuesta a una acción del usuario o para restaurar el estado de la interfaz de usuario después de la navegación por una página.

El hecho de colocar a la vista un elemento virtualizado implica lo siguiente:
1. Ejecutar un elemento de tipo UIElement para un elemento
2. Ejecutar el diseño para asegurarse de que el elemento tiene una posición válida
3. Iniciar una solicitud para colocar a la vista el elemento ejecutado

En el ejemplo siguiente se muestran estos pasos como parte de la restauración de la posición de desplazamiento de un elemento en una lista plana y vertical después de la navegación por una página. En el caso de los datos jerárquicos mediante clases ItemsRepeaters anidadas, el enfoque es básicamente el mismo, pero debe realizarse en cada nivel de la jerarquía.

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

     protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        // retrieve saved offset + index(es) of the tracked element and then bring it into view.
        // ... 
        
        var element = repeater.GetOrCreateElement(index);

        // ensure the item is given a valid position
        element.UpdateLayout();

        element.StartBringIntoView(new BringIntoViewOptions()
        {
            VerticalOffset = relativeVerticalOffset
        });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualSize.X, anchor.ActualSize.Y));
        relativeVerticalOffset = this.scrollviewer.VerticalOffset - anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>Habilitación de la accesibilidad

La clase [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no proporciona una experiencia predeterminada de accesibilidad. La documentación incluida en [Facilidad de uso para aplicaciones para UWP](/windows/uwp/design/usability) contiene gran cantidad de información que te ayudará a garantizar que la aplicación proporcione una experiencia de usuario inclusiva. Si usas la clase ItemsRepeater para crear un control personalizado, asegúrate de consultar la documentación incluida en [Automatización del mismo nivel personalizada](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Teclado
La compatibilidad de teclado mínima para el movimiento del foco que proporciona la clase ItemsRepeater se basa en la [navegación direccional 2D para teclado](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard) de XAML.

![Navegación direccional](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

El [modo XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) de ItemsRepeater está _habilitado_ de forma predeterminada. En función de la experiencia deseada, considera la posibilidad de agregar compatibilidad con [interacciones de teclado](/windows/uwp/design/input/keyboard-interactions) comunes, como Inicio, Fin, Retroceder Página y Avanzar Página.

La clase ItemsRepeater se asegura automáticamente de que el orden de tabulación predeterminado para sus elementos (ya sean virtualizados o no) sigue el orden en el que se proporcionan los elementos en los datos. De forma predeterminada, la clase ItemsRepeater tiene la propiedad [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) establecida en [Once](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode), en lugar de _Local_ (valor predeterminado habitual).

> [!NOTE]
> ItemsRepeater no recuerda automáticamente el último elemento enfocado.  Esto significa que, cuando un usuario usa Mayús+Tab, es posible que se le lleve al último elemento ejecutado.

### <a name="announcing-item-_x_-of-_y_-in-screen-readers"></a>Anuncio de "Elemento _X_ de _Y_" en lectores de pantalla

Debes administrar el establecimiento de las propiedades de automatización adecuadas, como los valores de **PositionInSet** y **SizeOfSet**, y asegurarte de que permanecen actualizadas al agregar elementos, moverlos, quitarlos, etc.

En algunos diseños personalizados podría no haber una secuencia obvia para el orden visual.  Los usuarios esperan como mínimo que los valores de las propiedades PositionInSet y SizeOfSet usadas por los lectores de pantalla coincidan con el orden en que los elementos aparecen en los datos (desplazamiento de 1 para que coincidan con el recuento natural, frente al basado en 0).

La mejor manera de conseguirlo consiste en que la automatización del mismo nivel del elemento controle la implementación de los métodos [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) y [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) y notifique la posición del elemento en el conjunto de datos representado por el control. El valor solo se calcula en tiempo de ejecución cuando se accede a él mediante una tecnología de asistencia y mantenerlo actualizado no supone un problema. El valor coincide con el orden de los datos.

En este ejemplo se muestra cómo podrías hacerlo al presentar un control personalizado denominado _CardControl_.

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Listas](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)

---
Description: ItemsRepeater es un control ligero para generar y presentar una colección de elementos.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 93a81501b524826484111419899675fbb99b86fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364755"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Use un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) crear una colección personalizada experiencias con un sistema de diseño flexible, vistas personalizadas y virtualización.

A diferencia de [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no proporciona una experiencia completa para el usuario final, no se tiene ninguna interfaz de usuario predeterminada y no se proporciona ninguna directiva en torno a la interacción del usuario, la selección o foco. En su lugar, es un bloque de creación que puede usar para crear sus propias experiencias únicas basado en recopilación y controles personalizados. Mientras no tiene ninguna directiva integrada, permite asociar directiva para crear la experiencia que necesita. Por ejemplo, puede definir el diseño para usar la directiva de usuario, la directiva de selección, etcetera.

Puede pensar en [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) conceptualmente como un panel controladas por datos, en lugar de como un control completo, como ListView. Especifique una colección de elementos de datos que se mostrará una plantilla de elemento que genera un elemento de interfaz de usuario para cada elemento de datos y un diseño que determina cómo se tamaño y coloca los elementos. A continuación, ItemsRepeater genera elementos secundarios en función del origen de datos y los muestra según lo especificado por la plantilla de elemento y el diseño. Los elementos mostrados no es necesario ser homogéneo porque ItemsRepeater pueden cargar contenido para representar los elementos de datos según los criterios especificados en un selector de plantillas de datos.

| **Obtención de la biblioteca de interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y características de interfaz de usuario para aplicaciones UWP. Para obtener más información, incluidas las instrucciones de instalación, consulte el [Introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API importantes**: [Clase ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), [clase ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Use un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) crear pantallas personalizadas para las colecciones de datos. Aunque se puede usar para presentar un conjunto básico de elementos, a menudo puede usar como el elemento de visualización en la plantilla de un control personalizado.

Si necesita un control de casilla para mostrar datos en una lista o una cuadrícula con una mínima personalización, considere el uso de un [ListView](/uwp/api/windows.ui.xaml.controls.listview) o [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

ItemsRepeater no tiene una colección de elementos integrada. Si tiene que proporcionar una colección de elementos directamente, en lugar de enlazarse a un origen de datos independiente, entonces es probable que precisan una experiencia más alto-policy y debe usar [ListView](/uwp/api/windows.ui.xaml.controls.listview) o [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) y ItemsRepeater ambos habilitar experiencias completas de la colección personalizable, pero ItemsRepeater es compatible con virtualización de los diseños de interfaz de usuario, mientras que ItemsControl no. Se recomienda usar ItemsRepeater en lugar de ItemsControl, si su para simplemente presentar algunos elementos de datos o creación de un control de la colección personalizada.

> [!NOTE]
> Si tienes una situación donde piensa ItemsControl satisfaga sus necesidades y no ItemsRepeater, dejar comentarios en el [proyecto de GitHub de biblioteca de interfaz de usuario de Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues) e infórmenos.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para abrir la aplicación y ver el <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Desplazamiento con ItemsRepeater

[**ItemsRepeater** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no se deriva de [ **Control**](/uwp/api/windows.ui.xaml.controls.control), por lo que no tiene una plantilla de control. Por lo tanto, no contiene cualquier integrada desplazamiento como un ListView o realizar otros controles de la colección.

Cuando se usa un **ItemsRepeater**, debe proporcionar funcionalidad de desplazamiento incluyéndolo en una [ **ScrollViewer** ](/uwp/api/windows.ui.xaml.controls.scrollviewer) control.

> [!NOTE]
> Si la aplicación se ejecutará en versiones anteriores de Windows - aquellos publicado *antes* Windows 10, versión 1809 - también tendrá que hospedar el **ScrollViewer** dentro de la [  **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost). 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> Si la aplicación solo se ejecutará en las versiones recientes de Windows 10, versión 1809 y versiones posterior:, no es necesario utilizar el [ **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost).
>
> Antes de Windows 10, versión 1809, **ScrollViewer** no implementó la [ **IScrollAnchorProvider** ](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) interfaz que el **ItemsRepeater**necesita.  El **ItemsRepeaterScrollHost** permite la **ItemsRepeater** coordinarse con **ScrollViewer** en versiones anteriores para conservar correctamente la ubicación de los elementos visible el usuario está viendo.  En caso contrario, es posible que aparecen los elementos mover o, de repente, desaparecen cuando se cambian los elementos de la lista o se cambia el tamaño de la aplicación.

## <a name="create-an-itemsrepeater"></a>Crear un ItemsRepeater

Para usar un [ **ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), debe proporcionarle los datos que se va a mostrar estableciendo el **ItemsSource** propiedad. A continuación, indique cómo mostrar los elementos estableciendo los [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propiedad.

### <a name="itemssource"></a>ItemsSource

Para rellenar la vista, establecer el [ **ItemsSource** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) propiedad a una colección de elementos de datos. En este caso, el **ItemsSource** se establece en el código directamente a una instancia de una colección.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

También puede enlazar el **ItemsSource** propiedad a una colección en XAML. Para obtener más información sobre el enlace de datos, consulta [Introducción al enlace de datos](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
Para especificar cómo se visualiza un elemento de datos, establezca el [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propiedad a un [ **DataTemplate** ](/uwp/api/windows.ui.xaml.datatemplate) o [  **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector) ha definido. La plantilla de datos define cómo se visualizan los datos. De forma predeterminada, el elemento se muestra en la vista con un **TextBlock** la usa la representación de cadena del objeto de datos.

Sin embargo, normalmente desea mostrar una presentación más enriquecida de los datos mediante una plantilla que define el diseño y la apariencia de uno o más controles que va a usar para mostrar un elemento individual. Los controles que usar en la plantilla se pueden enlazar a las propiedades del objeto de datos o tienen insertada definido de contenido estático.

#### <a name="datatemplate"></a>DataTemplate
En este ejemplo, el objeto de datos es una cadena simple. El **DataTemplate** incluye una imagen a la izquierda del texto y estilos el **TextBlock** para mostrar la cadena en un color verde azulado.

> [!NOTE]
> Cuando se usa el [extensión de marcado x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) en un **DataTemplate**, tendrá que especificar el tipo de datos (`x:DataType`) en la plantilla de datos.

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

Aquí es cómo los elementos aparecerá cuando se muestra con este **DataTemplate**.

![Elementos que se muestran con una plantilla de datos](images/listview-itemstemplate.png)

El número de elementos que se usan en el **DataTemplate** para un elemento puede tener un impacto significativo en el rendimiento si la vista muestra un gran número de elementos. Para obtener más información y ejemplos de cómo usar **DataTemplate**para definir la apariencia de los elementos en la lista, consulte [contenedores y las plantillas de elemento](item-containers-templates.md).

> [!TIP]
> Para mayor comodidad cuando quiere declarar la plantilla insertada en lugar de al que hace referencia como un recurso estático, puede especificar el **DataTemplate** o **DataTemplateSelector** como elemento secundario directo de la **ItemsRepeater**.  Se asignará como el valor de la **ItemTemplate** propiedad. Por ejemplo, esto es válido:
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> A diferencia de **ListView** y otros controles de la colección, el **ItemsRepeater** no se ajusta los elementos de un **DataTemplate** con un contenedor de elementos adicionales que incluye directiva predeterminada, como los márgenes, relleno, los objetos visuales de selección o un puntero a través de estado visual. En su lugar, **ItemsRepeater** sólo presenta lo que se define en el **DataTemplate**. Si desea que los elementos que tienen el mismo aspecto que un elemento de vista de lista, se puede incluir explícitamente un contenedor, como **ListViewItem**, en la plantilla de datos. **ItemsRepeater** mostrará el **ListViewItem** objetos visuales, pero no convierte automáticamente el uso de otras funcionalidades, como la selección o que se muestra la casilla de selección múltiple.
>
> De forma similar, si la recopilación de datos es una colección de controles reales, como **botón** (`List<Button>`), puede colocar un **ContentPresenter** en su **DataTemplate** a Mostrar el control.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Los elementos que mostrar en la vista no debe ser del mismo tipo. Puede proporcionar el [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propiedad con un [ **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector) para seleccionar distintos  **DataTemplate**en función de los criterios que especifique.

En este ejemplo se da por supuesto un **DataTemplateSelector** se ha definido que decide entre dos diferentes **DataTemplate**s para representar un elemento grande y pequeño.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Al definir un **DataTemplateSelector** para usar con **ItemsRepeater** sólo tiene que implementar una invalidación para el [ **SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_) método. Para obtener más información y ejemplos, vea [ **DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Una alternativa a **DataTemplate**s para administrar cómo se crean elementos en escenarios más avanzados consiste en implementar su propio [ **Windows.UI.Xaml.Controls.IElementFactory** ](/uwp/api/windows.ui.xaml.controls.ielementfactory)que se usará como el **ItemTemplate**.  Será responsable de generar el contenido cuando se solicita.

## <a name="configure-the-data-source"></a>Configurar el origen de datos

Utilice la [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) propiedad para especificar la colección utilizada para generar el contenido de elementos. Puede establecer el ItemsSource a cualquier tipo que implementa **IEnumerable**. Interfaces de colección adicionales implementadas por el origen de datos determinan qué funcionalidad está disponible para el ItemsRepeater para interactuar con los datos.

Esta lista muestra las interfaces disponibles y cuándo se debe considerar el uso de cada uno de ellos.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(. NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Puede usarse para conjuntos de datos pequeños y estáticos.

    Como mínimo, el origen de datos debe implementar la interfaz IEnumerable / interfaz IIterable. Si esto es todo lo que es compatible con el control iterará a través de todo el contenido una vez para crear una copia que puede usar para tener acceso a los elementos a través de un valor de índice.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(. NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Puede usarse para conjuntos de datos estáticos y de solo lectura.

    Habilita el control de acceder a los elementos por índice y evita la copia interna redundante.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(. NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Puede usarse para conjuntos de datos estáticos.

    Habilita el control de acceder a los elementos por índice y evita la copia interna redundante.

    **Advertencia**: Cambios en el vector de lista sin implementar [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) no se reflejarán en la interfaz de usuario.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Se recomienda para admitir la notificación de cambio.

    Habilita el control de observar y reaccionar ante los cambios en el origen de datos y reflejar dichos cambios en la interfaz de usuario.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Admite la notificación de cambio

    Al igual que el **INotifyCollectionChanged** interfaz, esto permite que el control de observar y reaccionar ante los cambios en el origen de datos.

    **Advertencia**: El Windows.Foundation.IObservableVector\<T > no es compatible con una acción "Mover". Esto puede causar la interfaz de usuario para un elemento pierde su estado visual.  Por ejemplo, un elemento que está seleccionado actualmente o que tiene el foco donde el movimiento se consigue mediante 'Remove' seguido de un 'Add' pierde el foco y ya no estará seleccionado.

    El Platform.Collections.Vector\<T > usa IObservableVector\<T > y tiene esta limitación de la misma. Si el soporte técnico para una acción "Mover" se requiere, a continuación, usar el **INotifyCollectionChanged** interfaz.  ObservableCollection .NET\<T > usa la clase **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Cuando un identificador único puede asociarse con cada elemento.  Se recomienda cuando se usa "Reset" como la acción de cambio de la colección.

    Habilita el control de manera muy eficaz recuperar la interfaz de usuario existente después de recibir una acción de disco dura "Restablecer" como parte de un **INotifyCollectionChanged** o **IObservableVector** eventos. Después de recibir un restablecimiento el control utilizará el identificador único proporcionado para asociar los datos actuales de los elementos que ya había creado. Sin la clave de asignación de índice, el control tendría que suponga que necesita para volver a empezar desde cero en la creación de la interfaz de usuario para los datos.

Las interfaces enumeradas anteriormente, que no sea IKeyIndexMapping, proporcionan el mismo comportamiento en ItemsRepeater como lo hacen en ListView y GridView.


Las interfaces siguientes en ItemsSource habilitar funcionalidad especial en los controles ListView y GridView, pero actualmente no tienen ningún efecto en un ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Queremos sus comentarios. Nos gustaría conocer su opinión sobre la [proyecto de GitHub de biblioteca de interfaz de usuario de Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues). Considere la posibilidad de agregar sus opiniones sobre propuestas existentes como [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): Agregar compatibilidad con la carga incremental para ItemsRepeater.

Es un enfoque alternativo para cargar incrementalmente los datos como el usuario se desplaza hacia arriba o hacia abajo observar la posición de la ventanilla de ScrollViewer y cargar más datos a medida que la extensión aproxima a la ventanilla.

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

Elementos mostrados por la [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) se organizan por un [diseño](/uwp/api/microsoft.ui.xaml.controls.layout) objeto que administra la posición y dimensionamiento de sus elementos secundarios. Cuando se usa con un ItemsRepeater, el objeto de diseño permite la virtualización de interfaz de usuario. Los diseños proporcionados son [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) y [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). De forma predeterminada, ItemsRepeater utiliza un StackLayout con orientación vertical.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) organiza los elementos en una sola línea que se puede orientar horizontal o verticalmente.

Puede establecer el [espaciado](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) propiedad que se va a ajustar la cantidad de espacio entre los elementos. Se aplica el espaciado en el sentido de que el diseño [orientación](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation).

![Espaciado de diseño de pila](images/stack-layout.png)

En este ejemplo se muestra cómo establecer la propiedad ItemsRepeater.Layout en un StackLayout con orientación horizontal y el espaciado de 8 píxeles.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

El [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) coloca los elementos secuencialmente en un diseño de ajuste. Los elementos son disponer en orden de izquierda a derecha cuando el [orientación](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) es **Horizontal**y disponer de arriba a abajo cuando la orientación es **Vertical**. Cada elemento es el mismo tamaño.

![Espaciado de diseño de cuadrícula uniforme](images/uniform-grid-layout.png)

El número de elementos de cada fila de un diseño horizontal viene determinado por el ancho mínimo del elemento. El número de elementos de cada columna de un diseño vertical se ve influenciado por el alto mínimo del elemento.

- Puede proporcionar explícitamente un tamaño mínimo para usar estableciendo el [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) y [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) propiedades.
- Si no especifica un tamaño mínimo, el tamaño del primer elemento medido se considera el tamaño mínimo por cada elemento.

También puede establecer un espacio mínimo para el diseño debe incluir entre las filas y columnas estableciendo el [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) y [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) propiedades.

![Espaciado y el tamaño de cuadrícula uniforme](images/uniform-grid-sizing-spacing.png)

Después de que el número si se ha determinado los elementos de una fila o columna en función de tamaño mínimo del elemento y el espaciado, podría haber dejado después del último elemento de la fila o columna (como se muestra en la imagen anterior) de espacio no utilizado. Puede especificar si cualquier espacio adicional se omite, usa para aumentar el tamaño de cada elemento o usado para crear espacio adicional entre los elementos. Esto se controla mediante el [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) y [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) propiedades.

Puede establecer el [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) propiedad para especificar cómo se aumenta el tamaño del elemento para rellenar el espacio no utilizado.

Esta lista muestra los valores disponibles. Las definiciones suponen el valor predeterminado **orientación** de **Horizontal**.

- **Ninguna**: El espacio adicional se quede sin usar al final de la fila. Este es el valor predeterminado.
- **Rellenar**: Los elementos tienen un ancho adicional para usar el espacio disponible (alto si vertical).
- **Uniforme**: Los elementos son el ancho adicional para usar el espacio disponible y especificados alto adicional para mantener la relación de aspecto (ancho y alto cambian si vertical).

Esta imagen muestra el efecto de la **ItemsStretch** valores en un diseño horizontal.

![Estiramiento de elemento de cuadrícula uniforme](images/uniform-grid-item-stretch.png)

Cuando **ItemsStretch** es **ninguno**, puede establecer el [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) propiedad para especificar cómo extra espacio se usa para alinear los elementos.

Esta lista muestra los valores disponibles. Las definiciones suponen el valor predeterminado **orientación** de **Horizontal**.

- **iniciar**: Los elementos se alinean con el inicio de la fila. El espacio adicional se quede sin usar al final de la fila. Este es el valor predeterminado.
- **Center**: Los elementos se alinean en el centro de la fila. El espacio adicional se divide por igual al principio y al final de la fila.
- **End**: Los elementos se alinean con el final de la fila. El espacio adicional se quede sin usar del principio de la fila.
- **SpaceAround**: Los elementos se distribuyen uniformemente. Se agrega una cantidad igual de espacio antes y después de cada elemento.
- **SpaceBetween**: Los elementos se distribuyen uniformemente. Se agrega una cantidad igual de espacio entre cada elemento. No hay espacio se agrega al principio y al final de la fila.
- **SpaceEvenly**: Los elementos se distribuyen uniformemente con una cantidad igual de espacio entre cada elemento tanto al principio y al final de la fila.

Esta imagen muestra el efecto de la **ItemsStretch** valores en un diseño vertical (se aplica a columnas en lugar de filas).

![Justificación de elemento de cuadrícula uniforme](images/uniform-grid-item-justification.png)

> [!TIP]
> El **ItemsStretch** afecta a la propiedad el _medida_ pase de diseño. El **ItemsJustification** afecta a la propiedad el _organizar_ pase de diseño.

En este ejemplo se muestra cómo establecer el **ItemsRepeater.Layout** propiedad a un **UniformGridLayout**.

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

Cuando hospeda elementos en una [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), es posible que deba realizar alguna acción cuando se muestra o detiene se muestra un elemento, tales como iniciar una descarga asincrónica de parte del contenido, asociación el elemento con un mecanismo para realizar un seguimiento de selección, o Detener alguna tarea en segundo plano.

En un control de la virtualización, no puede confiar en los eventos Loaded y Unloaded porque no se puede quitar el elemento desde el árbol visual dinámico cuando ha reciclado. En su lugar, se proporcionan otros eventos para administrar el ciclo de vida de los elementos. Este diagrama muestra el ciclo de vida de un elemento en un ItemsRepeater y, cuando se producen los eventos relacionados.

![Diagrama de eventos de ciclo de vida](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) se produce cada vez que un elemento se prepara para su uso. Esto se produce para un elemento recién creado así como un elemento que ya existe y es que se va a volver a utilizar desde la cola de reciclaje.
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) se produce inmediatamente producidas de cada vez que un elemento se ha enviado a la cola de reciclaje, como cuando se encuentra fuera del intervalo de elementos.
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) se produce para cada uno de ellos realizadas UIElement donde ha cambiado el índice del elemento representa. Por ejemplo, cuando se agrega otro elemento o se quitan del origen de datos, el índice de elementos que van detrás en la ordenación recibir este evento.

En este ejemplo se muestra cómo podría utilizar estos eventos para adjuntar un servicio de selección personalizada para realizar un seguimiento de selección de elementos en un control personalizado que utiliza ItemsRepeater para mostrar los elementos.

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

## <a name="sorting-filtering-and-resetting-the-data"></a>Ordenar, filtrar y restablecer los datos

Al realizar acciones como filtrar ni ordenar el conjunto de datos, que tradicionalmente es posible que tiene en comparación con el conjunto de datos a los nuevos datos anterior y luego emite notificaciones de cambio específico a través de [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). Sin embargo, a menudo resulta más fácil de reemplazar los datos antiguos con los nuevos datos completamente y desencadenar una notificación de cambio de colección mediante el [restablecer](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) acción en su lugar.

Normalmente, un restablecimiento hace que un control liberar los elementos secundarios existentes y volver a empezar, creación de la interfaz de usuario desde el principio en la posición de desplazamiento 0, ya que tiene no conoce exactamente cómo han cambiado los datos durante el restablecimiento.

Sin embargo, si la colección asignado como el ItemsSource admite identificadores únicos implementando la [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) interfaz, a continuación, puede identificar rápidamente el ItemsRepeater:

- elementos de IU reutilizable para los datos que existían antes y después el restablecimiento
- elementos visibles previamente que se quitaron
- nuevos elementos agregados que serán visibles

Esto permite la ItemsRepeater evitar empezar de nuevo desde la posición de desplazamiento 0. También se permite restaurar rápidamente el UIElements para los datos que no ha cambiado en un restablecimiento, lo que mejora el rendimiento.

En este ejemplo se muestra cómo mostrar una lista de elementos en una pila vertical donde _MyItemsSource_ es un origen de datos personalizado que contiene una lista de elementos subyacente. Expone un _datos_ propiedad que se puede usar para volver a asignar una nueva lista que se usará como el origen de los elementos, que, a continuación, desencadena un restablecimiento.

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

## <a name="create-a-custom-collection-control"></a>Crear un control de la colección personalizada

Puede usar el [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para crear un control de la colección personalizada junto con su propio tipo de control para presentar cada elemento.

> [!NOTE]
> Esto es similar a usar **ItemsControl**, pero en lugar de derivar desde **ItemsControl** y colocar un **ItemsPresenter** en la plantilla de control, deriva de  **Control** e inserte un **ItemsRepeater** en la plantilla de control. El control de la colección personalizada "tiene un" **ItemsRepeater** frente a "es un" **ItemsControl**. Esto implica que debe elegir explícitamente qué propiedades se exponen, en lugar de que hereda las propiedades de no admitir.

En este ejemplo se muestra cómo colocar un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) en la plantilla de un control personalizado denominado _MediaCollectionView_ y exponer sus propiedades.

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

## <a name="display-grouped-items"></a>Mostrar los elementos agrupados

Puede anidar un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) en el [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) de otro ItemsRepeater crear anidada virtualizar los diseños. El marco de trabajo hará un uso eficaz de recursos, ya que minimiza la realización innecesaria de los elementos que no son visibles o cerca de la ventanilla actual.

En este ejemplo se muestra cómo puede mostrar una lista de elementos agrupados en una pila vertical. El ItemsRepeater externa genera cada grupo. En la plantilla para cada grupo, otro ItemsRepeater genera los elementos.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
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

En este ejemplo se muestra un diseño de una aplicación que tiene varias categorías que se pueden cambiar con la preferencia de usuario y se presentan como desplazarse horizontalmente por listas, como se muestra aquí.

![Diseño anidado con elementos repeater](images/items-repeater-nested-layout.png)

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

## <a name="bringing-an-element-into-view"></a>Poner un elemento en la vista

Framework el XAML controla ya aportan un FrameworkElement a la vista cuando (1) recibe el foco de teclado o (2) recibe el foco de Narrador. Puede haber otros casos en que necesite para poner explícitamente un elemento en la vista. Por ejemplo, en respuesta a una acción del usuario o para restaurar el estado de la interfaz de usuario después de una navegación de página.

Poner un elemento virtualizado en la vista implica lo siguiente:
1. Tenga en cuenta un elemento UIElement sea para un elemento
2. Ejecute el diseño para asegurarse de que el elemento tiene una posición válida
3. Iniciar una solicitud para mostrar el elemento producido

El ejemplo siguiente muestra estos pasos como parte de la restauración de la posición de desplazamiento de un elemento en una lista plana, vertical después de una navegación de página. En el caso de los datos jerárquicos mediante ItemsRepeaters anidada el enfoque es básicamente el mismo, pero debe realizarse en cada nivel de la jerarquía.

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
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>Habilitar la accesibilidad

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no proporciona una experiencia de la accesibilidad predeterminada. La documentación sobre [facilidad de uso para aplicaciones UWP](/windows/uwp/design/usability) proporciona una gran cantidad de información que le ayudarán a garantizar que su aplicación proporciona una experiencia de usuario inclusive. Si usas el ItemsRepeater para crear un control personalizado, a continuación, asegúrese de ver la documentación en [automatizaciones personalizado](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Teclado
La compatibilidad de usuario mínima para el movimiento de foco proporciona ItemsRepeater se basa en el XAML [2D navegación direccional para Keyboarding](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard).

![Dirección de navegación](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

El ItemsRepeater [XYFocusKeyboardNavigation modo](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) es _habilitado_ de forma predeterminada. Dependiendo de la experiencia deseada, considere la posibilidad de agregar compatibilidad para common [interacciones de teclado](/windows/uwp/design/input/keyboard-interactions) como inicio, fin, RePág y AvPág.

El ItemsRepeater automáticamente Asegúrese de que el orden de tabulación predeterminada para sus elementos (ya sea virtualizados o no) sigue el mismo orden que se proporcionan los elementos en los datos. De forma predeterminada tiene la ItemsRepeater su [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) propiedad establecida en [una vez](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) en lugar del predeterminado común de _Local_.

> [!NOTE]
> El ItemsRepeater no recuerda el último elemento con foco automáticamente.  Esto significa que cuando un usuario usa Mayús + Tab puede dejarse hasta el último producidas elemento.

### <a name="announcing-item-x-of-y-in-screen-readers"></a>Anuncio de "elemento _X_ de _Y_" en los lectores de pantalla

Debe administrar la configuración de las propiedades de automatización apropiado, como los valores de **PositionInSet** y **SizeOfSet**y asegúrese de que permanecen actualizados cuando se agregan elementos, mover, quitar, etcetera.

En algunos diseños personalizados no puede haber una secuencia obvia para el orden visual.  Como mínimo, los usuarios esperan que los valores de las propiedades de PositionInSet y SizeOfSet usadas por los lectores de pantalla coincidirá con el orden de que los elementos aparecen en los datos (desplazamiento en 1 para que coincida con natural de recuento en comparación con la que se está basado en 0).

La mejor manera de lograr esto es la automatización del mismo nivel para la implementación del control de elemento de tener la [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) y [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) métodos y la posición del elemento en el conjunto de datos de informe representado por el control. El valor solo se calcula en tiempo de ejecución cuando se tiene acceso mediante una tecnología de asistencia y mantenerlo actualizado se convierte en un problema. El valor coincide con el orden de los datos.

En este ejemplo se muestra cómo podría hacerlo al presentar un control personalizado denominado _CardControl_.

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

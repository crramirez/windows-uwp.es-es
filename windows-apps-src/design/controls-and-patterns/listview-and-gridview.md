---
Description: Usa los controles ListView and GridView para mostrar y manipular conjuntos de datos, como una galería de imágenes o un conjunto de mensajes de correo electrónico.
title: Vista de lista y vista de cuadrícula
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c130505ec79ca83698fd79df26464969afe79c36
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80893475"
---
# <a name="list-view-and-grid-view"></a>Vista de lista y vista de cuadrícula

La mayoría de las aplicaciones manipulan y muestran conjuntos de datos, como una galería de imágenes o un conjunto de mensajes de correo electrónico. El marco de trabajo de la interfaz de usuario de XAML proporciona los controles ListView y GridView que hacen más fácil mostrar y manipular datos en la aplicación.  

> **API importantes**: [clase ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [clase GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [propiedad ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [propiedad Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> Las clases ListView y GridView se derivan ambas de la clase [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase), por lo que tienen la misma funcionalidad, pero muestran los datos de manera diferente. En este artículo, cuando hablamos de la vista de lista, la información corresponde a ambos controles, ListView y GridView, a menos que se indique lo contrario. Podemos hacer mención a clases como ListView o ListViewItem, pero el prefijo *List* se puede reemplazar por *Grid* para el equivalente de cuadrícula correspondiente (GridView o GridViewItem). 

Los controles ListView y GridView ofrecen muchas ventajas para trabajar con colecciones. Son fáciles de implementar y proporcionan una interfaz de usuario básica, interacción y desplazamiento, además de poderse personalizar con facilidad. Los controles ListView y GridView se pueden enlazar a orígenes de datos dinámicos existentes o a datos codificados de forma rígida que se proporcionan en el propio XAML o en el código subyacente. 

Estos dos controles son flexibles para muchos casos de uso, pero funcionan mejor con las colecciones en las que todos los elementos deben tener la misma estructura básica y apariencia, así como el mismo comportamiento de interacción; es decir, deben realizar la misma acción cuando se hace clic en ellos (abrir un vínculo, navegar, etc.).


## <a name="differences-between-listview-and-gridview"></a>Diferencias entre ListView y GridView

### <a name="listview"></a>ListView
ListView muestra los datos apilados verticalmente en una sola columna. ListView funciona mejor para los elementos que tienen texto como punto focal y para las colecciones que se van a leer de arriba abajo (es decir, están ordenadas alfabéticamente). Algunos casos de uso habituales de ListView son las listas de mensajes y los resultados de la búsqueda. Las colecciones que deben mostrarse en varias columnas o en un formato de tabla _no_ deben usar ListView, pero deben buscar usar [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) en su lugar.

![Vista de lista con datos agrupados](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView presenta una colección de elementos en filas y columnas por las que podemos desplazarnos verticalmente. Se datos se apilan horizontalmente hasta que rellenan las columnas y luego se continúa con la fila siguiente. GridView funciona mejor para los elementos que tienen imágenes como punto focal y para las colecciones que se pueden leer de lado a lado o que no están en un orden específico. Un caso de uso habitual de GridView es una galería de productos o de fotos.

![Ejemplo de una biblioteca de contenido](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>¿Qué control de colección debes usar? Comparación con ItemsRepeater

ListView y GridView son controles que funcionan de serie para mostrar cualquier colección, con su propia interfaz de usuario y experiencia de usuario integradas. El control [ItemsRepeater](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) también se usa para mostrar colecciones, pero se generó como bloque de creación para crear un control personalizado que se ajuste a las necesidades exactas de tu interfaz de usuario. A continuación, se indican las diferencias más importantes que afectan a la elección del control que vas a usar:

-   ListView y GridView son controles con abundantes características que requieren poca personalización, pero ofrecen mucho. ItemsRepeater es un bloque de creación que sirve para crear tu propio control de diseño y no tiene las mismas características y funcionalidad integradas: requiere que implementes las características o interacciones necesarias.
-   Debes usar ItemsRepeater si tienes una interfaz de usuario muy personalizada que no se puede crear con ListView o GridView o si tienes un origen de datos que requiere un comportamiento muy diferente para cada elemento.


Para obtener más información sobre ItemsRepeater, lee la documentación sobre las [directivas](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) y la [documentación sobre la API](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para abrir la aplicación y ver <a href="xamlcontrolsgallery:/item/ListView">ListView</a> o <a href="xamlcontrolsgallery:/item/GridView">GridView</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>Crear un control ListView o GridView

ListView y GridView son de tipo [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol), por lo que pueden contener una colección de elementos de cualquier tipo. Un control ListView o GridView debe tener elementos en su colección [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) para poder mostrar algo en la pantalla. Para rellenar la vista, puedes agregar elementos directamente a la colección [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) o establecer la propiedad [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) en un origen de datos. 

> [!IMPORTANT]
> Puedes usar Items o ItemsSource para rellenar la lista, pero no ambos al mismo tiempo. Si estableces la propiedad ItemsSource y agregas un elemento en XAML, el elemento agregado se omite. Si estableces la propiedad ItemsSource y decides agregar un elemento a la colección Items en código, se inicia una excepción.

Para que resulte más sencillo, en muchos de los ejemplos de este artículo, la colección `Items` se rellena directamente. Sin embargo, es más habitual que los elementos en una lista provengan de un origen dinámico, como una lista de libros de una base de datos en línea. Se usa la propiedad `ItemsSource` para tales fines. 

### <a name="add-items-to-a-listview-or-gridview"></a>Agregar elementos a un control ListView o GridView

Puedes agregar elementos a la colección [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) de ListView o GridView mediante XAML o código para producir el mismo resultado. Normalmente, se agregan elementos mediante XAML si se tienen pocos elementos, que no cambian y se definen fácilmente, o si se generan los elementos en el código en tiempo de ejecución. 

<u>Método 1: agregar elementos a la colección Items</u>
#### <a name="option-1-add-items-through-xaml"></a>Opción 1: agregar elementos mediante XAML
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>Opción 2: agregar elementos mediante C#

##### <a name="c-code"></a>Código C#:
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>Código XAML correspondiente:
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
Las dos opciones anteriores darán como resultado el mismo ListView, que se muestra a continuación:

![Una vista de lista simple](images/listview-basic-code-example2.png)
<br/>
<u> Método 2: agregar elementos estableciendo ItemsSource</u>

Por lo general, se usa un control ListView o GridView para mostrar los datos de un origen, como una base de datos o Internet. Para rellenar un control ListView o GridView desde un origen de datos, debes establecer la propiedad [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) en una colección de elementos de datos. Este método funciona mejor si ListView o GridView van a contener objetos de clase personalizada, como se muestra en los ejemplos siguientes.

#### <a name="option-1-set-itemssource-in-c"></a>Opción 1: establecer ItemsSource en C#
Aquí, la propiedad ItemsSource de la vista de lista está establecida en el código, directamente en una instancia de una colección. 

##### <a name="c-code"></a>Código C#:
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>Código XAML:
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>Opción 2: establecer ItemsSource en XAML
También puedes enlazar la propiedad ItemsSource a una colección en XAML. Aquí, ItemsSource está enlazado a una propiedad pública denominada `Contacts`, que expone la colección de datos privados de Page, `_contacts`.

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

Las dos opciones anteriores darán como resultado el mismo ListView, que se muestra a continuación. ListView solo muestra la representación de cadena de cada elemento porque no se proporcionó ninguna plantilla de datos.

![Vista de lista simple con ItemsSource establecido](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> Si no se ha definido ninguna plantilla de datos, los objetos de clase personalizada solo aparecerán en el control ListView con su valor de cadena si tienen definido un método [ToString() ](https://docs.microsoft.com/uwp/api/windows.foundation.istringable.tostring).

 En la siguiente sección se detalla cómo representar visual y correctamente elementos de clase simple y personalizada en un control ListView o GridView.

Para obtener más información sobre el enlace de datos, consulta [Introducción al enlace de datos](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).

> [!NOTE]
> Si necesitas mostrar datos agrupados en el control ListView, debes enlazar a una clase [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource). CollectionViewSource actúa como proxy para la clase de colección en XAML y habilitar la compatibilidad con la agrupación. Para más información, consulta [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource).

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>Personalización de la apariencia de los elementos con una clase DataTemplate

Una plantilla de datos en un control ListView o GridView define cómo se visualizan los elementos o los datos. De manera predeterminada, un elemento de datos se muestra en el control ListView como una representación de cadena del objeto de datos con el que está enlazado. Para mostrar la representación de cadena de una propiedad determinada del elemento de datos, establece [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) en dicha propiedad.

No obstante, seguramente quieras mostrar una presentación más enriquecida de los datos. Para especificar con exactitud cómo se muestran los elementos en los controles ListView o GridView, debes crear una clase [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). El lenguaje XAML de la clase DataTemplate define el diseño y la apariencia de los controles usados para mostrar un elemento individual. Los controles del diseño se pueden enlazar a las propiedades de un objeto de datos o pueden tener contenido estático definido en línea. 

> [!NOTE]
> Cuando uses la [extensión de marcado x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) en una clase DataTemplate, debes especificar DataType (`x:DataType`) en dicha clase.

#### <a name="simple-listview-data-template"></a>Plantilla de datos simple de ListView
En este ejemplo, el elemento de datos es una cadena simple. La clase DataTemplate se define dentro de la definición de ListView para agregar una imagen a la izquierda de la cadena y mostrar la cadena en verde azulado. Es el mismo control ListView creado con el método 1 y la opción 1 mostrado anteriormente.

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

Este es el aspecto de los elementos de datos cuando se muestran con esta plantilla de datos en un control ListView:

![Elementos de ListView con una plantilla de datos](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>Plantilla de datos de ListView para objetos de clase personalizada
En este ejemplo, el elemento de datos es un objeto de contacto. Una clase DataTemplate se define dentro de la definición de ListView para agregar la imagen de contacto a la izquierda del nombre de contacto y la compañía. Este control ListView se creó con el método 2 y la opción 2 mencionados anteriormente.
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Este es el aspecto de los elementos de datos cuando se muestran con esta plantilla de datos en un control ListView:

![Elementos de clase personalizada de ListView con una plantilla de datos](images/listview-customclass-datatemplate-final.png)

Las plantillas de datos son la forma principal de definir la apariencia del control ListView. También pueden tener un impacto significativo en el rendimiento si la lista contiene una gran cantidad de elementos.  

La plantilla de datos se puede definir dentro de la definición de ListView o GridView (tal como se muestra más arriba) o de forma independiente en una sección de recursos. Si la clase DataTemplate se define fuera del propio control ListView o GridView, se debe proporcionar un atributo [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) y asignarla a la propiedad [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) del control ListView o GridView con esa clave.

Para obtener más información y ejemplos de cómo usar las plantillas de datos y los contenedores de elementos para definir la apariencia de los elementos de la lista o cuadrícula, consulta [Plantillas y contenedores de elementos](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Cambiar el diseño de elementos

Al agregar elementos a un control ListView o GridView, el control encapsula automáticamente cada elemento en un contenedor de elemento y luego expone todos los contenedores de elementos. La manera en que se disponen estos contenedores de elementos depende de la propiedad [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) del control.  
- De manera predeterminada, **ListView** usa una clase [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel), lo cual genera una lista vertical, como esta.

![Una vista de lista simple](images/listview-simple.png)

- **GridView** usa una clase [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid), que agrega los elementos horizontalmente, y se ajusta y desplaza verticalmente, como se muestra a continuación.

![Una vista de cuadrícula simple](images/gridview-simple.png)

Para modificar el diseño de elementos, puedes ajustar las propiedades en el panel de elementos o puedes reemplazar el panel predeterminado por otro panel.

> [!NOTE]
> Ten cuidado de no deshabilitar la virtualización si cambias la propiedad ItemsPanel. Tanto **ItemsStackPanel** como **ItemsWrapGrid** admiten la virtualización, por lo que son seguras. Si usas otro panel, es posible que deshabilites la virtualización y ralentices el rendimiento de la vista de lista. Para obtener más información, consulta los artículos sobre la vista de lista en [Performance](https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui) (Rendimiento). 

En este ejemplo se muestra cómo hacer que una clase **ListView** disponga sus contenedores de elementos en una lista horizontal al cambiar la propiedad [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) de la clase **ItemsStackPanel**.
Dado que la vista de lista se desplaza verticalmente de manera predeterminada, también debes ajustar algunas propiedades de la clase [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) interna de la vista de lista para que se desplace horizontalmente.
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) en **Enabled** o **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) en **Auto** 
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) en **Disabled** 
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) en **Hidden** 

> [!IMPORTANT]
> En estos ejemplos se muestran con el ancho de la vista de lista sin ninguna restricción, por lo que no se muestran las barras de desplazamiento horizontal. Si ejecutas este código, puedes establecer `Width="180"` en el control ListView para que muestre las barras de desplazamiento.

**XAML**
```xml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

La lista resultante tiene este aspecto.

![Una vista de lista horizontal](images/listview-horizontal2-final.png)

 En el siguiente ejemplo, el control **ListView** dispone los elementos en una lista de ajuste vertical, con una clase **ItemsWrapGrid** en lugar de una clase **ItemsStackPanel**. 
 
> [!IMPORTANT]
> El alto de la vista de lista debe restringirse para forzar que el control encapsule los contenedores.

**XAML**
```xml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

La lista resultante tiene este aspecto.

![Una vista de lista con diseño de cuadrícula](images/listview-itemswrapgrid2-final.png)

Si muestras datos agrupados en la vista de lista, la propiedad ItemsPanel determina cómo se disponen los grupos de elementos, no cómo se disponen los elementos individuales. Por ejemplo, si la clase ItemsStackPanel horizontal que se mostró anteriormente se usa para mostrar datos agrupados, los grupos se organizan de forma horizontal, pero los elementos de cada grupo aún se apilan verticalmente, como se muestra aquí.

![Una vista de lista horizontal agrupada](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Selección e interacción de elementos

Puedes elegir entre varias maneras para permitir que el usuario interactúe con una vista de lista. De manera predeterminada, un usuario puede seleccionar un único elemento. Puedes cambiar la propiedad [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) para habilitar la selección múltiple o para deshabilitar la selección. Puedes establecer la propiedad [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) de modo que un usuario haga clic en un elemento para invocar una acción (por ejemplo, un botón) en lugar de seleccionar el elemento.

> **Nota**&nbsp;&nbsp;Tanto ListView como GridView usan la enumeración [ListViewSelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewselectionmode) en sus propiedades SelectionMode. IsItemClickEnabled es **False** de manera predeterminada, por lo que deberás establecerla únicamente para permitir el modo de clic.

En esta tabla se muestran las maneras en que un usuario puede interactuar con una vista de lista, y cómo se puedes responder a la interacción.

Para habilitar esta interacción: | Usa esta configuración: | Controla este evento: | Usa esta propiedad para obtener el elemento seleccionado:
----------------------------|---------------------|--------------------|--------------------------------------------
Sin interacción | [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **None**, [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | N/A | N/A 
Selección única | SelectionMode = **Single**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem), [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
Selección múltiple | SelectionMode = **Multiple**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Selección extendida | SelectionMode = **Extended**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Haga clic en | SelectionMode = **None**, IsItemClickEnabled = **True** | [ItemClick](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | N/A 

> **Nota**&nbsp;&nbsp;A partir de Windows 10, puedes habilitar IsItemClickEnabled para generar un evento ItemClick mientras SelectionMode también está establecido en Single, Multiple o Extended. Si haces esto, primero se genera el evento ItemClick y luego se genera el evento SelectionChanged. En algunos casos, como si navegaras a otra página en el controlador de eventos ItemClick, no se genera el evento SelectionChanged y no se selecciona el elemento.

Puedes establecer estas propiedades en XAML o en el código, como se muestra aquí.

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>Solo lectura

Puedes establecer la propiedad SelectionMode en **ListViewSelectionMode.None** para deshabilitar la selección de elementos. Esto coloca el control en modo de solo lectura, para que se use para mostrar los datos, pero no para interactuar con ellos. El control en sí no está deshabilitado, solo está deshabilitada la selección de elementos.

### <a name="single-selection"></a>Selección única

En esta tabla se describen las interacciones del teclado, el mouse y de las funciones táctiles cuando SelectionMode es **Single**.

Tecla modificadora | Interacción
-------------|------------
Ninguno | <li>Un usuario puede seleccionar un solo elemento con la barra espaciadora, un clic del mouse o una entrada táctil.</li>
Ctrl | <li>Un usuario puede anular la selección un solo elemento con la barra espaciadora, un clic del mouse o una entrada táctil.</li><li>Con las teclas de dirección, un usuario puede mover el foco independientemente de la selección.</li>

Cuando SelectionMode es **Single**, puedes obtener el elemento de datos seleccionado desde la propiedad [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem). Puedes obtener el índice de la colección del elemento seleccionado con la propiedad [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex). Si no se selecciona ningún elemento, SelectedItem es **null** y SelectedIndex es -1. 
 
Si intentas establecer un elemento que no está en la colección **Items** como la propiedad **SelectedItem**, la operación se ignora y SelectedItem es**null**. Sin embargo, si intentas establecer la propiedad **SelectedIndex** en un índice que está fuera del intervalo de **Items** en la lista, se produce una excepción **System.ArgumentException**. 

### <a name="multiple-selection"></a>Selección múltiple

En esta tabla se describen las interacciones del teclado, el mouse y de las funciones táctiles cuando SelectionMode es **Multiple**.

Tecla modificadora | Interacción
-------------|------------
Ninguno | <li>Un usuario puede seleccionar varios elementos con la barra espaciadora, un clic del mouse o una entrada táctil para alternar la selección del elemento especificado.</li><li>Con las teclas de dirección, un usuario puede mover el foco independientemente de la selección.</li>
Mayús | <li>Un usuario puede seleccionar varios elementos contiguos haciendo clic o pulsando el primer elemento de la selección y luego el último elemento de la selección.</li><li>Con las teclas de dirección, un usuario puede crear una selección contigua empezando por el elemento seleccionado mientras se mantiene presionada la tecla Mayús.</li>

### <a name="extended-selection"></a>Selección extendida

En esta tabla se describen las interacciones del teclado, el mouse y de las funciones táctiles cuando SelectionMode es **Extended**.

Tecla modificadora | Interacción
-------------|------------
Ninguno | <li>El comportamiento es el mismo que la selección **Single**.</li>
Ctrl | <li>Un usuario puede seleccionar varios elementos con la barra espaciadora, un clic del mouse o una entrada táctil para alternar la selección del elemento especificado.</li><li>Con las teclas de dirección, un usuario puede mover el foco independientemente de la selección.</li>
Mayús | <li>Un usuario puede seleccionar varios elementos contiguos haciendo clic o pulsando el primer elemento de la selección y luego el último elemento de la selección.</li><li>Con las teclas de dirección, un usuario puede crear una selección contigua empezando por el elemento seleccionado mientras se mantiene presionada la tecla Mayús.</li>

Cuando SelectionMode es **Multiple** o **Extended**, puedes obtener los elementos de datos seleccionados desde la propiedad [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems). 

Las propiedades **SelectedIndex**, **SelectedItem** y **SelectedItems** están sincronizadas. Por ejemplo, si estableces SelectedIndex en -1, SelectedItem se establece en **null** y SelectedItems está vacía; si estableces SelectedItem en **null**, SelectedIndex se establece en -1 y SelectedItems está vacía.

En el modo de selección múltiple, **SelectedItem** contiene el elemento que se selecciona en primer lugar, y **SelectedIndex** contiene el índice del elemento que se seleccionó primero. 

### <a name="respond-to-selection-changes"></a>Responder a los cambios de selección

Para responder a cambios de selección en una vista de lista, controla el evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). En el código del controlador de eventos, puedes obtener la lista de elementos seleccionados a partir de la propiedad [SelectionChangedEventArgs.AddedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Puedes obtener los elementos cuya selección se anuló a partir de la propiedad [SelectionChangedEventArgs.RemovedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems). Las colecciones AddedItems y RemovedItems contienen al menos 1 artículo, a menos que el usuario mantenga presionada la tecla Mayús para seleccionar un intervalo de elementos.

En este ejemplo se muestra cómo controlar el evento **SelectionChanged** y acceder a las distintas colecciones de elementos.

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>Modo de clic

Puedes cambiar una vista de lista para que un usuario haga clic en elementos, como botones, en lugar de seleccionarlos. Por ejemplo, resulta útil si tu aplicación navega a una página nueva cuando el usuario hace clic en un elemento de una lista o cuadrícula. Para habilitar este comportamiento:
- Establece **SelectionMode** en **None**.
- Establece **IsItemClickEnabled** en **true**.
- Controla el evento **ItemClick** para que haga algo cuando el usuario haga clic en un elemento.

Aquí tienes una vista de lista con elementos en los que se puede hacer clic. El código en el controlador de eventos ItemClick navega a una página nueva.

**XAML**
```xml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>Seleccionar un intervalo de elementos mediante programación

A veces, es necesario manipular la selección de un elemento de la vista de lista mediante programación. Por ejemplo, es posible que tengas un botón **Seleccionar todo** para permitir al usuario seleccionar todos los elementos en una lista. En este caso, normalmente no resulta muy eficaz agregar y quitar elementos de la colección SelectedItems uno por uno. Cada cambio en un elemento produce un evento SelectionChanged, y cuando trabajas con los elementos directamente en lugar de trabajar con los valores de índice, el elemento se desvirtualiza.

Los métodos [SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectall), [SelectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange) y [DeselectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) proporcionan una manera más eficaz de modificar la selección que el uso de la propiedad SelectedItems. Estos métodos seleccionan o anulan la selección mediante los intervalos de índices de elementos. Los elementos que están virtualizados permanecen virtualizados, porque solo se usa el índice. Se seleccionan todos los elementos en el intervalo especificado (o se anula la selección) independientemente de su estado de selección original. El evento SelectionChanged se produce solo una vez para cada llamada a estos métodos.

> [!IMPORTANT]
> Debes llamar a estos métodos solo cuando la propiedad SelectionMode esté establecida en Multiple o Extended. Si llamas a SelectRange cuando SelectionMode es Single o None, se produce una excepción.

Cuando seleccionas elementos mediante intervalos de índices, usa la propiedad [SelectedRanges](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) para obtener todos los intervalos seleccionados en la lista.

Si ItemsSource implementa [IItemsRangeInfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.iitemsrangeinfo) y usas estos métodos para modificar la selección, las propiedades **AddedItems** y **RemovedItems** no se establecen en la propiedad SelectionChangedEventArgs. Establecer estas propiedades requiere la desvirtualización del objeto del elemento. En su lugar, usa la propiedad **SelectedRanges** para obtener estos elementos.

Para seleccionar todos los elementos en una colección, puedes llamar al método SelectAll. Sin embargo, no hay ningún método correspondiente para anular la selección de todos los elementos. Para anular la selección de todos los elementos, puedes llamar a DeselectRange y pasar una clase [ItemIndexRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange) con un valor [FirstIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) de 0 y un valor [Length](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.length) igual al número de elementos de la colección. Esto se muestra en el ejemplo siguiente, junto con una opción para seleccionar todos los elementos.

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

Para obtener información sobre cómo cambiar la apariencia de los elementos seleccionados, consulta [Plantillas y contenedores de elementos](item-containers-templates.md).

### <a name="drag-and-drop"></a>Arrastrar y colocar

Los controles ListView y GridView permiten arrastrar y colocar elementos en sí mismos, entre sí y en otros controles ListView y GridView. Para obtener más información acerca de cómo implementar el patrón de arrastrar y colocar, consulta [Arrastrar y colocar](../input/drag-and-drop.md).

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Ejemplo de ListView y GridView en XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView): muestra los controles ListView y GridView.
- [Ejemplo de arrastrar y colocar en XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop): muestra cómo arrastrar y colocar con el control ListView.
- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Listas](lists.md)
- [Plantillas y contenedores de elementos](item-containers-templates.md)
- [Arrastrar y colocar](../input/drag-and-drop.md)

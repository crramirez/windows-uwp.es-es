---
description: Puedes crear una vista de árbol expandible mediante el enlace de ItemsSource a un origen de datos jerárquicos, o bien puedes crear y administrar objetos TreeViewNode tú mismo.
title: Vista de árbol
label: Tree view
template: detail.hbs
ms.date: 07/24/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: 8e18455a39441d46e13e5a9a72291c9cd379c310
ms.sourcegitcommit: 9effd88952bd26611f7b0a0e7baa68aba7d0ee8d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68616531"
---
# <a name="treeview"></a>TreeView

El control [TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) de XAML habilita una lista jerárquica con nodos que se expanden y se contraen, y que contienen elementos anidados. Puede usarse para ilustrar una estructura de carpetas o relaciones anidadas en la interfaz de usuario.

Las API de **TreeView** admiten las siguientes características:

- Anidamiento de n niveles
- Selección de uno o varios nodos
- Enlace de datos a la propiedad **ItemsSource** en las clases **TreeView** y [TreeViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewitem)
- **TreeViewItem** como raíz de la plantilla de elementos de **TreeView**
- Tipos arbitrarios de contenido en **TreeViewItem**
- Función Arrastrar y colocar entre vistas de árbol

| **Obtener la biblioteca de interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información, incluidas instrucciones sobre la instalación, consulta la [introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plataforma** | **API de la biblioteca de interfaz de usuario de Windows** |
| - | - |
| [Clase TreeView](/uwp/api/windows.ui.xaml.controls.treeview), [clase TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode), [propiedad TreeView.ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [Clase TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview), [clase TreeViewNode](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [propiedad TreeView.ItemsSource](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

En este documento, se usará el alias **muxc** en XAML para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado lo siguiente a nuestro elemento [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page):

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

En el código subyacente, se usará el alias **muxc** en C# para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado esta instrucción **using** en la parte superior del archivo:

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

- Usa una clase **TreeView** cuando los elementos tengan elementos de lista anidados y si es importante ilustrar la relación jerárquica de los elementos respecto de los elementos del mismo nivel y los nodos.

- Evita usar **TreeView** si resaltar la relación anidada de un elemento no es una prioridad. En la mayoría de los escenarios de detalles, es adecuado usar una vista de lista normal.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TreeView">abrir la aplicación y ver TreeView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>Interfaz de usuario de TreeView

La vista de árbol usa una combinación de sangría e iconos para representar la relación anidada entre los nodos primarios y los nodos secundarios. Los nodos contraídos usan una comilla angular que señala hacia la derecha, mientras que los expandidos usan una comilla angular que señala hacia abajo.

![Icono de comilla angular en TreeView](images/treeview-simple.png)

Puedes incluir un icono en la plantilla de datos del elemento de vista de árbol para representar los nodos. Por ejemplo, si muestras una jerarquía de sistema de archivos, podrías usar iconos de carpeta para los nodos primarios e iconos de archivo para los nodos hoja.

![Iconos de comilla angular y de carpeta juntos en una vista de árbol](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>Creación de una vista de árbol

Puedes crear una vista de árbol mediante el enlace de la propiedad [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) a un origen de datos jerárquicos, o bien puedes crear y administrar objetos **TreeViewNode** tú mismo.

Para crear una vista de árbol, usa un control [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) y una jerarquía de objetos [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode). Crea la jerarquía de nodos mediante la adición de uno o varios nodos raíz a la colección **RootNodes** del control [TreeView](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes). Después, se pueden agregar más nodos a la colección **Children** de cada objeto [TreeViewNode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewnode.children). Puedes anidar nodos de vista de árbol con cualquier profundidad necesaria.

Puedes enlazar un origen de datos jerárquicos a la propiedad [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) para proporcionar el contenido de la vista de árbol, tal y como harías con **ItemsSource** de [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview). De forma similar, usa la propiedad [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (y la propiedad opcional [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)) para proporcionar una clase [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) que represente el elemento.

> [!IMPORTANT]
> La propiedad **ItemsSource** y sus API relacionadas requieren Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk) o posterior), o bien la [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).
>
> **ItemsSource** es un mecanismo alternativo para que la propiedad **TreeView.RootNodes** coloque contenido en el control **TreeView**. No se pueden establecer **ItemsSource** y **RootNodes** al mismo tiempo. Cuando usas la propiedad **ItemsSource**, los nodos se crean automáticamente y puedes tener acceso a ellos desde la propiedad **TreeView.RootNodes**.

Este es un ejemplo de una vista de árbol sencilla que se ha declarado en XAML. Normalmente se agregan los nodos en el código, pero te mostramos aquí la jerarquía XAML, porque puede resultar útil para visualizar cómo se crea la jerarquía de nodos.

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

En la mayoría de los casos, la vista de árbol muestra datos de un origen de datos, por lo que suele declararse el control **TreeView** raíz en XAML, pero agrega los objetos **TreeViewNode** en el código o mediante el enlace de datos.

### <a name="bind-to-a-hierarchical-data-source"></a>Enlace a un origen de datos jerárquicos

Para crear una vista de árbol mediante el enlace de datos, establece una colección jerárquica en la propiedad **TreeView.ItemsSource**. Después, en **ItemTemplate**, establece la colección de elementos secundarios en la propiedad **TreeViewItem.ItemsSource**.

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

Consulta [Vista de árbol con enlace de datos](#tree-view-using-data-binding) para obtener el código completo.

#### <a name="items-and-item-containers"></a>Elementos y contenedores de elementos

Si usas la propiedad **TreeView.ItemsSource**, estas API están disponibles para obtener el nodo o el elemento de datos del contenedor y viceversa.

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | Obtiene el elemento de datos del contenedor de **TreeViewItem** especificado. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | Obtiene el contenedor de **TreeViewItem** del elemento de datos especificado. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | Obtiene el objeto **TreeViewNode** del contenedor **TreeViewItem** especificado. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | Obtiene el contenedor **TreeViewItem** del objeto **TreeViewNode** especificado. |

### <a name="manage-tree-view-nodes"></a>Administración de los nodos de la vista de árbol

Esta vista de árbol es la misma que se creó anteriormente en XAML, pero en este caso los nodos se crean en código.

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Estas API están disponibles para administrar la jerarquía de datos de la vista de árbol.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Una vista de árbol puede tener uno o varios nodos raíz. Agrega un objeto **TreeViewNode** a la colección **RootNodes** para crear un nodo raíz. El valor **Parent** de un nodo raíz es siempre **null**. El valor **Depth** de un nodo raíz es 0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Agrega objetos **TreeViewNode** a la colección **Children** de un nodo primario para crear la jerarquía del nodo. Un nodo es el elemento **Parent** de todos los nodos de su colección **Children**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | **true** si el nodo ha ejecutado los elementos secundarios. **false** indica una carpeta vacía o un elemento. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Usa esta propiedad si estás rellenando nodos cuando están expandidos. Consulta [Rellenar un nodo cuando está en expansión](#fill-a-node-when-its-expanding) más adelante en este artículo. |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Indica a qué distancia está un nodo secundario del nodo raíz. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Obtiene el objeto **TreeViewNode** que posee la colección **Children** de la que forma parte este nodo. |

La vista de árbol usa las propiedades **HasChildren** y **HasUnrealizedChildren** para determinar si se muestra el icono de expandir/contraer. Si cualquiera de estas propiedades es **true**, se muestra el icono; de lo contrario, no se muestra.

## <a name="tree-view-node-content"></a>Contenido del nodo de vista de árbol

Puedes almacenar el elemento de datos que representa un nodo de la vista de árbol en su propiedad [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

En los ejemplos anteriores, el contenido es un valor de cadena simple. Aquí, un nodo de la vista de árbol representa la carpeta **Imágenes** del usuario; por lo tanto, se asigna la biblioteca de imágenes [StorageFolder](/uwp/api/windows.storage.storagefolder) a la propiedad **Content** del nodo.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> Para obtener acceso a la carpeta **Imágenes**, tienes que especificar la funcionalidad **Biblioteca imágenes** en el manifiesto de la aplicación. Para más información, consulte [Declaraciones de funcionalidad de las aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

Puedes proporcionar una clase [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) para especificar cómo se muestra el elemento de datos en la vista de árbol.

> [!NOTE]
> En Windows 10, versión 1803, debes volver a crear la plantilla del control **TreeView** y especificar una propiedad **ItemTemplate** personalizada si el contenido no es una cadena. En versiones posteriores, establece la propiedad **ItemTemplate**. Para obtener más información, consulta [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate).

### <a name="item-container-style"></a>Estilo del elemento de contenedor

Tanto si usas la propiedad **ItemsSource** como la propiedad **RootNodes**, los elementos reales empleados para mostrar cada nodo (denominado "contenedor") son un objeto [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem). Puedes modificar las propiedades de **TreeViewItem** para aplicar estilos al contenedor mediante las propiedades [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) o [ItemContainerStyleSelector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector) del control **TreeView**.

En este ejemplo se muestra cómo cambiar los glifos expandidos o contraídos por signos +/- naranjas. En la plantilla **TreeViewItem** predeterminada, los glifos se establecen para usar la fuente `Segoe MDL2 Assets`. Puedes establecer la propiedad **Setter.Value** si proporcionas el valor de carácter Unicode en el formato que usa XAML, de la siguiente manera: `Value="&#xE948;"`.

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>Selectores de plantillas de elementos

De forma predeterminada, [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) muestra la representación de cadena del elemento de datos de cada nodo. Puedes establecer la propiedad [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) para cambiar lo que se muestra en todos los nodos. O bien, puedes usar un [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector) para elegir otra [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) para los elementos de la vista de árbol en función del tipo de elemento o de algún otro criterio que especifiques.

Por ejemplo, en una aplicación de explorador de archivos, podrías usar una plantilla de datos para las carpetas y otra para los archivos.

![Carpetas y plantillas que usan plantillas de datos diferentes](images/treeview-icons.png)

Este es un ejemplo de cómo crear y usar un selector de plantillas de elementos.  Para obtener más información, consulta la clase [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Este código forma parte de un ejemplo más grande y no funcionará por sí solo. Para ver el ejemplo completo, que incluye el código que define `ExplorerItem`, consulta el [repositorio Xaml-Controls-Gallery](https://github.com/microsoft/Xaml-Controls-Gallery) en GitHub. [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) y [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) contienen el código correspondiente.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

El tipo de objeto que se pasa al método [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) depende de si creas la vista de árbol al establecer la propiedad **ItemsSource** o al crear y administrar objetos **TreeViewNode**.

- Si se establece **ItemsSource**, el objeto será del mismo tipo que el elemento de datos. En el ejemplo anterior, el objeto era `ExplorerItem`, por lo que podría usarse después de una conversión simple a `ExplorerItem`: `var explorerItem = (ExplorerItem)item;`.
- Si no se establece **ItemsSource** y administras los nodos de la vista de árbol, el objeto que se ha pasado a **SelectTemplateCore** es **TreeViewNode**. En este caso, puedes obtener el elemento de datos de la propiedad [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Este es un selector de plantillas de datos del ejemplo [Vista de árbol de las bibliotecas Imágenes y Música](#pictures-and-music-library-tree-view) que se muestra más adelante. El método **SelectTemplateCore** recibe un **TreeViewNode**, que puede tener un [StorageFolder](/uwp/api/windows.storage.storagefolder) o un [StorageFile](/uwp/api/windows.storage.storagefile) como su contenido. En función del contenido, puedes devolver una plantilla predeterminada o una plantilla específica para la carpeta de música, la carpeta de imágenes, los archivos de música o los archivos de imágenes.

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>Interacción con una vista de árbol

Puedes configurar una vista de árbol para permitir al usuario interactuar con ella de diversas formas:

- Expandir o contraer nodos
- Elementos de selección única o múltiple
- Clic para invocar un elemento

### <a name="expandcollapse"></a>Expandir o contraer

Siempre puede expandirse o contraerse cualquier nodo de vista de árbol que tenga elementos secundarios. Para ello, es necesario hacer clic en el glifo de expandir/contraer. También puedes expandir o contraer un nodo mediante programación y responder cuando un nodo cambie de estado.

#### <a name="expandcollapse-a-node-programmatically"></a>Expandir/contraer un nodo mediante programación

Existen dos formas de expandir o contraer un nodo de vista de árbol en el código.

- La clase [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) tiene los métodos [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) y [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand). Cuando se llama a estos métodos, se pasa la clase **TreeViewNode** que se quiere expandir o contraer.

- Cada [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) tiene la propiedad [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Puedes usar esta propiedad para comprobar el estado de un nodo, o bien configurarla para cambiar el estado. También puedes establecer esta propiedad en XAML para indicar el estado inicial de un nodo.

### <a name="fill-a-node-when-its-expanding"></a>Rellenar un nodo cuando está en expansión

Es posible que debas mostrar un gran número de nodos en la vista de árbol, o que no sepas con antelación cuántos nodos tendrá. El control **TreeView** no se virtualiza, por lo que puedes administrar los recursos si rellenas cada nodo cuando se expande y si quitas los nodos secundarios cuando se contrae.

Manipula el evento [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) y usa la propiedad [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) para agregar elementos secundarios a un nodo cuando se expande. La propiedad **HasUnrealizedChildren** indica si el nodo debe rellenarse o si ya se ha rellenado su colección **Children**. Es importante recordar que **TreeViewNode** no establece este valor; hay que administrarlo en el código de la aplicación.

Este es un ejemplo de estas API en uso. Consulta el código de ejemplo completo al final de este artículo para buscar contexto, incluida la implementación de **FillTreeNode**.

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

No es necesario, pero es posible que quieras manipular también el evento [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) y quitar los nodos secundarios cuando se cierra el nodo primario. Esto puede ser importante si la vista de árbol tiene varios nodos, o si los datos del nodo usan muchos recursos. Debes tener en cuenta el impacto que tiene en el rendimiento rellenar un nodo cada vez que se abre, en lugar de salir de los elementos secundarios en un nodo cerrado. La mejor opción dependerá de la aplicación.

Este es un ejemplo de un controlador para el evento **Collapsed**.

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Invocación de un elemento

Un usuario puede invocar una acción (considerando el elemento como un botón) en lugar de seleccionar el elemento. Se controla el evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) para responder a esta interacción del usuario.

> [!NOTE]
> A diferencia de la clase **ListView**, que tiene la propiedad [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled), la invocación de un elemento siempre está habilitada en la vista de árbol. Aun así, puedes elegir si quieres controlar el evento.

**Clase [TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)**

Los argumentos del evento **ItemInvoked** dan acceso al elemento invocado. La propiedad [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) tiene el nodo que se ha invocado. Puedes transmitirlo a **TreeViewNode** y obtener el elemento de datos de la propiedad **TreeViewNode.Content**.

Este es un ejemplo de un controlador de eventos **ItemInvoked**. El elemento de datos es un [IStorageItem](/uwp/api/windows.storage.istorageitem). En este ejemplo solo se muestra información sobre el archivo y el árbol. Además, si el nodo es un nodo de carpeta, expande o contrae el nodo al mismo tiempo. De lo contrario, el nodo expande o contrae solo cuando se hace clic en la comilla angular.

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>Selección de elementos

El control **TreeView** admite tanto la selección única como la múltiple. De manera predeterminada, la selección de nodos está desactivada, pero puedes establecer la propiedad [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) para permitir la selección de nodos. Los valores [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) son **None**, **Single** y **Multiple**.

#### <a name="multiple-selection"></a>Selección múltiple

Cuando se habilita la selección múltiple, se muestra una casilla junto a cada nodo de la vista de árbol y se resaltan los elementos seleccionados. Un usuario puede seleccionar o anular la selección de un elemento mediante la casilla; al hacer clic en el elemento, se le invoca.

Si se selecciona un nodo primario (o se anula su selección), se seleccionarán todos los elementos secundarios bajo ese nodo (o se anulará su selección). Si se seleccionan algunos elementos secundarios bajo un nodo primario, pero no todos, la casilla del nodo primario se mostrará como indeterminada (rellenada con una caja negra).

![Selección múltiple en una vista de árbol](images/treeview-selection.png)

Los nodos seleccionados se agregan a la colección [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) de la vista de árbol. Puedes llamar al método [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) para seleccionar todos los nodos de una vista de árbol.

> [!NOTE]
> Si llamas a **SelectAll**, se seleccionan todos los nodos ejecutados, independientemente de la propiedad **SelectionMode**. Para proporcionar una experiencia de usuario coherente, solo debes llamar a **SelectAll** si el valor de **SelectionMode** es **Multiple**.

#### <a name="selection-and-realizedunrealized-nodes"></a>Selección y nodos ejecutados/no ejecutados

Si la vista de árbol tiene nodos no ejecutados, no se tienen en cuenta para la selección. Estas son algunas cosas que debes tener en cuenta respecto a la selección con nodos no ejecutados.

- Si un usuario selecciona un nodo primario, también se seleccionan todos los elementos secundarios bajo ese elemento principal. Del mismo modo, si se seleccionan todos los nodos secundarios, también se selecciona el primario.
- El método **SelectAll** solo agrega nodos ejecutados a la colección **SelectedNodes**.
- Si se selecciona un nodo primario con elementos secundarios no ejecutados, se seleccionarán los elementos secundarios a medida que se ejecutan.

## <a name="code-examples"></a>Ejemplos de código

Los siguientes ejemplos de código muestran las distintas características del control de vista de árbol.

### <a name="tree-view-using-xaml"></a>Vista de árbol en XAML

En este ejemplo se muestra cómo crear una sencilla estructura de vista de árbol en XAML. La vista de árbol muestra ingredientes y sabores de helado entre los que el usuario puede elegir, organizados en categorías. Se habilita la selección múltiple y, cuando el usuario hace clic en un botón, se muestran los elementos seleccionados en la interfaz de usuario de la aplicación principal.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all" Click="SelectAllButton_Click"/>
                <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
                <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>Vista de árbol con enlace de datos

En este ejemplo se muestra cómo crear la misma vista de árbol del ejemplo anterior, pero en lugar de crear la jerarquía de datos en XAML, los datos se crean en código y se enlazan a la propiedad **ItemsSource** de la vista de árbol. (Los controladores de eventos de botón que se muestran en el ejemplo anterior también se aplican a este ejemplo).

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>Vista de árbol de las bibliotecas Imágenes y Música

En este ejemplo se indica cómo crear una vista de árbol que muestra el contenido y la estructura de las bibliotecas **Imágenes** y **Música** del usuario. No se puede conocer con antelación el número de elementos, por lo que cada nodo se rellena cuando se expande y se vacía cuando se contrae.

Se usa una plantilla de elemento personalizada para mostrar los elementos de datos, que son del tipo [IStorageItem](/uwp/api/windows.storage.istorageitem).

> [!IMPORTANT]
> El código de este ejemplo requiere las funcionalidades **picturesLibrary** y **musicLibrary**. Para obtener más información sobre el acceso de archivo, consulta [Permisos de acceso de archivos](../../files/file-access-permissions.md), [Enumerar y consultar archivos y carpetas](../../files/quickstart-listing-files-and-folders.md) y [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            // A TreeView can have more than 1 root node. The Pictures library
            // and the Music library will each be a root node in the tree.
            // Get Pictures library.
            StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
        {
            // Get the contents of the folder represented by the current tree node.
            // Add each item as a new child node of the node that's being expanded.

            // Only process the node if it's a folder and has unrealized children.
            StorageFolder folder = null;

            if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
            {
                folder = node.Content as StorageFolder;
            }
            else
            {
                // The node isn't a folder, or it's already been filled.
                return;
            }

            IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

            if (itemsList.Count == 0)
            {
                // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
                // that the chevron appears, but don't try to process children that aren't there.
                return;
            }

            foreach (var item in itemsList)
            {
                var newNode = new muxc.TreeViewNode();
                newNode.Content = item;

                if (item is StorageFolder)
                {
                    // If the item is a folder, set HasUnrealizedChildren to true.
                    // This makes the collapsed chevron show up.
                    newNode.HasUnrealizedChildren = true;
                }
                else
                {
                    // Item is StorageFile. No processing needed for this scenario.
                }

                node.Children.Add(newNode);
            }

            // Children were just added to this node, so set HasUnrealizedChildren to false.
            node.HasUnrealizedChildren = false;
        }

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

            if (node.Content is IStorageItem item)
            {
                FileNameTextBlock.Text = item.Name;
                FilePathTextBlock.Text = item.Path;
                TreeDepthTextBlock.Text = node.Depth.ToString();

                if (node.Content is StorageFolder)
                {
                    node.IsExpanded = !node.IsExpanded;
                }
            }
        }

        private void RefreshButton_Click(object sender, RoutedEventArgs e)
        {
            sampleTreeView.RootNodes.Clear();
            InitializeTreeView();
        }
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
        ' Get the contents of the folder represented by the current tree node.
        ' Add each item as a new child node of the node that's being expanded.

        ' Only process the node if it's a folder and has unrealized children.
        Dim folder As StorageFolder = Nothing
        If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
            folder = TryCast(node.Content, StorageFolder)
        Else
            ' The node isn't a folder, or it's already been filled.
            Return
        End If

        Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
        If itemsList.Count = 0 Then
            ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
            ' that the chevron appears, but don't try to process children that aren't there.
            Return
        End If

        For Each item In itemsList
            Dim newNode As New muxc.TreeViewNode With {
            .Content = item
        }
            If TypeOf item Is StorageFolder Then
                ' If the item is a folder, set HasUnrealizedChildren to True.
                ' This makes the collapsed chevron show up.
                newNode.HasUnrealizedChildren = True
            Else
                ' Item is StorageFile. No processing needed for this scenario.
            End If
            node.Children.Add(newNode)
        Next

        ' Children were just added to this node, so set HasUnrealizedChildren to False.
        node.HasUnrealizedChildren = False
    End Sub

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
        Dim item = TryCast(node.Content, IStorageItem)
        If item IsNot Nothing Then
            FileNameTextBlock.Text = item.Name
            FilePathTextBlock.Text = item.Path
            TreeDepthTextBlock.Text = node.Depth.ToString()
            If TypeOf node.Content Is StorageFolder Then
                node.IsExpanded = Not node.IsExpanded
            End If
        End If
    End Sub

    Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
        sampleTreeView.RootNodes.Clear()
        InitializeTreeView()
    End Sub

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>Arrastrar y colocar elementos entre vistas de árbol

En el siguiente ejemplo se muestra cómo crear dos vistas de árbol cuyos elementos se pueden arrastrar y colocar entre ellas. Cuando se arrastra a la otra vista de árbol, se agrega al final de la lista. No obstante, los elementos pueden reorganizarse dentro de una vista de árbol. En este ejemplo también se tienen en cuenta las vistas de árbol de un nodo raíz.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Clase TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [Clase ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [ListView y GridView](listview-and-gridview.md)

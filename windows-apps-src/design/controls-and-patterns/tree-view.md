---
author: Jwmsft
description: Usa el código de ejemplo de la vista de árbol para crear un árbol expansible.
title: Vista de árbol
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.openlocfilehash: 41e17d299e9bac34e58f3c8ffdffecff19ddac18
ms.sourcegitcommit: e020e9a4d947368a68e4eeba1eea65e9b3a725af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2018
ms.locfileid: "1924398"
---
# <a name="treeview"></a>TreeView

El control TreeView XAML permite una lista jerárquica con nodos que se expanden y se contraen, y que contienen elementos anidados. Puede usarse para ilustrar una estructura de carpetas o relaciones anidadas en la interfaz de usuario.

> **API importantes**: [clase TreeView](/uwp/api/windows.ui.xaml.controls.treeview), [clase TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)

Las API TreeView admiten las siguientes características:

- Anidamiento de n niveles
- Expansión/contracción de nodos
- Selección de uno o varios nodos

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

- Usa una vista de árbol cuando los elementos cuenten con elementos de lista anidados y si es importante ilustrar la relación jerárquica de los elementos respecto a los elementos del mismo nivel y los nodos.

- Evita usar una vista de árbol si resaltar la relación anidada de un elemento no es una prioridad. En la mayoría de los escenarios de desglose, una vista de lista normal es adecuada

## <a name="treeview-ui"></a>Interfaz de usuario de la vista de árbol

La vista de árbol utiliza una combinación de sangría e iconos para representar la relación anidada entre los nodos de carpeta/primarios y los nodos que no son de carpeta/secundarios. Los nodos contraídos utilizan comillas angulares que señalan hacia la derecha, mientras que los expandidos usan comillas angulares que señalan hacia abajo.

![El icono de comillas angulares en la vista de árbol](images/treeview_chevron.png)

Puedes incluir un icono en la plantilla de datos del elemento de vista de árbol para representar los nodos. Si haces esto, debes usar un icono de carpeta solo para los nodos que representan carpetas literales, como la estructura de carpetas en un disco.

![Los iconos de comillas angulares y de carpeta juntos en una vista de árbol](images/treeview_chevron_folder.png)

## <a name="create-a-tree-view"></a>Crear una vista de árbol

Para crear una vista de árbol, usa un control [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) y una jerarquía de objetos [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode). Crea la jerarquía de nodos agregando uno o varios nodos de raíz a la colección RootNodes del control de TreeView. A continuación, cada TreeViewNode puede tener más nodos agregados a su colección de elementos secundarios. Puedes anidar nodos de vista de árbol con cualquier profundidad necesaria.

Este es un ejemplo de una vista de árbol sencilla que se ha declarado en XAML. Normalmente se agregan los nodos en el código, pero te mostramos aquí la jerarquía XAML, porque puede resultar útil para visualizar cómo se crea la jerarquía de nodos.

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

En la mayoría de los casos, la vista de árbol muestra datos de un origen de datos, por lo que suele declararse el control TreeView de raíz en XAML, pero agrega los objetos TreeViewNode en el código.

Esta vista de árbol es la misma que se creó anteriormente en XAML, pero los nodos se crean en código en su lugar.

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New TreeViewNode With {.Content = "Vanilla"})
        .Add(New TreeViewNode With {.Content = "Strawberry"})
        .Add(New TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Estas API están disponibles para administrar la jerarquía de datos de su vista de árbol.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Una vista de árbol puede tener uno o varios nodos de raíz. Agrega un objeto TreeViewNode a la colección RootNodes para crear un nodo raíz. El **Parent** de un nodo raíz es siempre **null**. El valor de **Depth** de un nodo raíz es 0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Agrega objetos TreeViewNode a la colección de elementos secundarios de un nodo primario para crear la jerarquía del nodo. Un nodo es el elemento **Parent** de todos los nodos en su colección **Children**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | **true** si el nodo ha realizado los elementos secundarios. **false** indica una carpeta vacía o un elemento. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Usa esta propiedad si estás rellenando nodos cuando se expanden. Consulta _Rellenar un nodo cuando está en expansión_ más adelante en este artículo. |
| [Profundidad](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Indica a qué distancia está un nodo secundario del nodo raíz. |
| [Primario](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Obtiene la TreeViewNode que posee la colección **Children** de la que forma parte este nodo. |

La vista de árbol utiliza las propiedades **HasChildren** y **HasUnrealizedChildren** para determinar si se muestra el icono de expandir/contraer. Si cualquiera de estas propiedades es **true**, se muestra el icono; de lo contrario, no se muestra.

## <a name="tree-view-node-content"></a>Contenido de nodo de vista de árbol

Puedes almacenar el elemento de datos que representa un nodo de la vista de árbol en su propiedad [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

En los ejemplos anteriores, el contenido es un valor de cadena simple. Aquí, un nodo de la vista de árbol representa la carpeta Imágenes del usuario, por lo tanto, se asignan la biblioteca de imágenes [StorageFolder](/uwp/api/windows.storage.storagefolder) a la propiedad Content del nodo.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New TreeViewNode With {.Content = picturesFolder}
```

Puedes proporcionar una clase [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) para especificar cómo se muestra el elemento de datos en la vista de árbol.

> [!NOTE]
> En Windows 10, versión 1803, tienes que volver a crear la plantilla del control de vista de árbol y especificar un ItemTemplate personalizado si el contenido no es una cadena. Para obtener más información, vea el ejemplo completo al final de este artículo.

## <a name="interacting-with-a-tree-view"></a>Interactuar con una vista de árbol

Puedes configurar una vista de árbol para permitir al usuario interactuar con ella de diversas formas:

- expandir o contraer nodos
- elementos de selección única o múltiple
- haga clic para invocar un elemento

### <a name="expandcollapse"></a>Expandir o contraer

Siempre puede expandirse o contraerse cualquier nodo de vista de árbol que tenga elementos secundarios haciendo clic en el glifo de expandir/contraer. También puede expandir o contraer un nodo mediante programación y responder cuando un nodo cambie de estado.

#### <a name="expandcollapse-a-node-programmatically"></a>Expandir/contraer un nodo mediante programación

Existen 2 formas de expandir o contraer un nodo de vista de árbol en el código.

- La clase [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) tiene los métodos [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) y [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand). Cuando se llama a estos métodos, se pasa el TreeViewNode que se quiere expandir o contraer.

- Cada [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) tiene la propiedad [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Puedes usar esta propiedad para comprobar el estado de un nodo, o configurarla para cambiar el estado. También puedes establecer esta propiedad en XAML para establecer el estado inicial de un nodo.

### <a name="fill-a-node-when-its-expanding"></a>Rellenar un nodo cuando está en expansión

Es posible que debas mostrar un gran número de nodos en la vista de árbol, o que no sepas con antelación cuántos nodos tendrá. El control TreeView no se virtualiza, para que puedas administrar recursos rellenado cada nodo mientras se expande y quitando los nodos secundarios cuando se contrae.

Manipula el evento [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) y usa la propiedad [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) para agregar elementos secundarios a un nodo cuando se expande. La propiedad HasUnrealizedChildren indica si el nodo debe rellenarse, o si ya se ha rellenado su colección de elementos secundarios. Es importante recordar que el TreeViewNode no establece este valor, hay que administrarlo en el código de la aplicación.

Este es un ejemplo de estos API en uso. Consulte el código de ejemplo completo al final de este artículo para buscar contexto, incluyendo la implementación de 'FillTreeNode'.

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

No es necesario, pero es posible que quieras manipular también el evento [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) y quitar los nodos secundarios cuando se cierra el nodo primario. Esto puede ser importante si la vista de árbol tiene varios nodos, o si los datos del nodo usan muchos recursos. Debes tener en cuenta el impacto de rendimiento de rellenar un nodo cada vez que se abre en lugar de salir de los elementos secundarios en un nodo cerrado. La mejor opción dependerá de la aplicación.

Aquí hay un ejemplo de un controlador para el evento Collapsed.

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Invocar un elemento

Un usuario puede invocar una acción (considerando al elemento como un botón) en lugar de seleccionar el elemento. Se controla el evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) para responder a esta interacción del usuario.

> [!NOTE]
> A diferencia de ListView, que tiene la propiedad [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled), invocando un elemento siempre habilitado en la vista de árbol. Aún puede elegir si desea controlar el evento o no.

**Clase [TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)**

Los argumentos del evento ItemInvoked dan acceso al elemento invocado. La propiedad [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) tiene el nodo que se haya invocado. Puedes transmitirlo a TreeViewNode y obtener el elemento de datos de la propiedad TreeViewNode.Content.

Este es un ejemplo de un controlador de eventos ItemInvoked. El elemento de datos es un [IStorageItem](/uwp/api/windows.storage.istorageitem) y este ejemplo solo muestra información sobre el archivo y el árbol. Además, si el nodo es un nodo de carpeta, expande o contrae el nodo al mismo tiempo. De lo contrario, el nodo expande o contrae solo cuando se hace clic en las comillas angulares.

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
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
Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
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

El control TreeView admite tanto la selección única como la múltiple. De manera predeterminada, la selección de nodos está desactivada, pero puedes establecer la propiedad [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) para permitir la selección de nodos. Los valores [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) son **None**, **Single** y **Multiple**.

Cuando se habilita la selección, se muestra una casilla junto a cada nodo de la vista de árbol, y se resaltan los elementos seleccionados. Un usuario puede seleccionar o anular la selección de un elemento usando la casilla; al hacer clic en el elemento se le sigue invocando.

Los nodos seleccionados se agregan a la colección [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) de la vista de árbol. Puedes llamar al método [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) para seleccionar todos los nodos de una vista de árbol.

> [!NOTE]
> Si llamas a **SelectAll**, se seleccionan todos los nodos ejecutados, independientemente de la propiedad SelectionMode. Para proporcionar una experiencia de usuario coherente, solo debes llamar a SelectAll si SelectionMode es **Multiple**.

#### <a name="selection-and-realizedunrealized-nodes"></a>Selección y nodos ejecutados/no ejecutados

Si la vista de árbol tiene nodos no ejecutados, no se tienen en cuenta para la selección. Estas son algunas cosas que debes tener en cuenta respecto a la selección con nodos no ejecutados.

- Si un usuario selecciona un nodo primario, también se seleccionan todos los elementos secundarios bajo ese elemento principal. Del mismo modo, si se seleccionan todos los nodos secundarios, también se selecciona el primario.
- El método SelectAll solo agrega nodos ejecutados a la colección SelectedNodes.
- Si se selecciona un nodo primario con elementos secundarios no ejecutados, se seleccionarán los elementos secundarios a medida que se ejecutan.

## <a name="code-examples"></a>Ejemplos de código

### <a name="tree-view-with-selection-enabled"></a>Vista de árbol con selección habilitada

En este ejemplo se muestra cómo crear una sencilla estructura de vista de árbol en XAML. La vista de árbol muestra ingredientes y sabores de helado entre los que el usuario puede elegir, organizados en categorías. Se habilita la selección múltiple, y cuando el usuario hace clic en un botón, se muestran SelectedItems en la interfaz de usuario de la aplicación principal.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
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
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
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
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="pictures-and-music-library-tree-view"></a>Vista de árbol de las bibliotecas de música e imágenes

Este ejemplo muestra cómo crear una vista de árbol que muestra el contenido y la estructura de las bibliotecas de Imágenes y Música de los usuarios. No se puede conocer el número de elementos antes de tiempo, por lo que cada nodo se rellena cuando se expande y se vacía cuando se contrae.

Se utiliza una plantilla de elemento personalizada para mostrar los elementos de datos, que son del tipo [IStorageItem](/uwp/api/windows.storage.istorageitem).

> [!IMPORTANT]
> El código de este ejemplo requiere las capacidades picturesLibrary y musicLibrary. Para obtener más información acerca del acceso de archivo, consulta [Permisos de acceso de archivos](../../files/file-access-permissions.md), [Enumerar y consultar archivos y carpetas](../../files/quickstart-listing-files-and-folders.md) y [Archivos y carpetas en las bibliotecas de música, imágenes y vídeos](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
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
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
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
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
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
        var newNode = new TreeViewNode();
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

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
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
```

```vb
Public Sub New()
    InitializeComponent()
    InitializeTreeView()
End Sub

Private Sub InitializeTreeView()
    ' A TreeView can have more than 1 root node. The Pictures library
    ' and the Music library will each be a root node in the tree.
    ' Get Pictures library.
    Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
    Dim pictureNode As New TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(pictureNode)
    FillTreeNode(pictureNode)

    ' Get Music library.
    Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
    Dim musicNode As New TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(musicNode)
    FillTreeNode(musicNode)
End Sub

Private Async Sub FillTreeNode(node As TreeViewNode)
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
        Dim newNode As New TreeViewNode With {
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

Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub

Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub

Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
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
```

## <a name="related-articles"></a>Artículos relacionados

- [Clase TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [Clase ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView y GridView](listview-and-gridview.md)
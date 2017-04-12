---
author: Jwmsft
Description: "Usa el código de ejemplo de la vista de árbol para crear un árbol expansible."
title: "Vista de árbol"
label: Tree view
template: detail.hbs
ms.openlocfilehash: 88e3e79b7ebdf06c200f3525095d7685f7e3e6dc
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="hierarchical-layout-with-treeview"></a>Diseño jerárquico con TreeView
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div class="microsoft-internal-note">
Las líneas rojas de TreeView están en el almacén de diseño: http://designdepotweb1/DesignDepot.FrontEnd/#/Dashboard/856
</div>

TreeView es un patrón de lista jerárquica con nodos que se expanden y se contraen, y que contienen elementos anidados. Los elementos anidados pueden ser nodos adicionales o elementos normales de una lista. Puedes usar una clase [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) para crear una vista de árbol con el fin de ilustrar una estructura de carpetas o relaciones anidadas en la interfaz de usuario.

La [muestra de TreeView](http://go.microsoft.com/fwlink/?LinkId=785018) es una implementación de referencia creada con **ListView**. No es un control independiente. La vista de árbol que se muestra en el panel de favoritos del navegador Microsoft Edge usa esta implementación de referencia.

La muestra admite:
- Anidamiento de n niveles
- Expansión/contracción de nodos
- Arrastrar y colocar nodos dentro de la vista de árbol
- Accesibilidad integrada

![La vista de árbol en la muestra de referencia](images/tree-view-sample.png) | ![La vista de árbol en el navegador Edge](images/tree-view-edge.png)
-- | --
Muestra de referencia de TreeView | TreeView en el navegador Edge

## <a name="is-this-the-right-pattern"></a>¿Es este el patrón adecuado?

- Usa una vista de árbol cuando los elementos cuenten con elementos de lista anidados y si es importante ilustrar la relación jerárquica de los elementos respecto a los elementos del mismo nivel y los nodos.

- Evita usar una vista de árbol si resaltar la relación anidada de un elemento no es una prioridad. En la mayoría de los escenarios de desglose, una vista de lista normal es adecuada

## <a name="treeview-ui-structure"></a>Estructura de la interfaz de usuario de una vista de árbol

Puedes usar iconos para representar los nodos en una vista de árbol. Puede usarse una combinación de sangría e iconos para representar la relación anidada entre los nodos de carpeta/primarios y los nodos que no son de carpeta/secundarios. Aquí te ofrecemos indicaciones sobre cómo hacerlo.

### <a name="icons"></a>Iconos

Usa iconos para indicar que un elemento es un nodo, así como el estado en el que está dicho nodo (expandido o contraído).

#### <a name="chevron"></a>Comillas angulares

Por motivos de coherencia, los nodos contraídos deben usar comillas angulares que señalen hacia la derecha y los nodos expandidos deben usar comillas angulares que señalen hacia abajo.

![Uso del icono de las comillas angulares en la vista de árbol](images/treeview_chevron.png)

#### <a name="folder"></a>Carpeta

Usa un icono de carpeta únicamente para representaciones literales de carpetas.

![Uso del icono de la carpeta en la vista de árbol](images/treeview_folder.png)

#### <a name="chevron-and-folder"></a>Comillas angulares y carpetas

Solo debe usarse una combinación de comillas angulares y una carpeta si los elementos de lista que no son nodo en la vista de árbol también tienen iconos.

![Uso de los iconos de comillas angulares y carpeta juntos en una vista de árbol](images/treeview_chevron_folder.png)

#### <a name="redlines-for-indentation-of-folders-and-non-folder-nodes"></a>Líneas rojas para la sangría de los nodos de carpeta y que no son carpetas

Usa las líneas rojas de la captura de pantalla de debajo para la sangría de los nodos de carpeta y que no son de carpeta.

![Líneas rojas para la sangría de los nodos de carpeta y que no son carpetas](images/treeview_chevron_folder_indent_rl.png)

## <a name="building-a-treeview"></a>Crear una vista de árbol

TreeView tiene las siguientes clases principales. Todas se definen y se incluyen en la implementación de referencia.

> **Nota**&nbsp;&nbsp;TreeView se implementa como un [componente de Windows Runtime](https://msdn.microsoft.com/windows/uwp/winrt-components/index) escrito en C++, por lo que cualquier aplicación para UWP escrita en uno de los lenguajes admitidos puede hacer referencia a ella. En la muestra, el código de TreeView se encuentra en la carpeta *cpp/Control*. No hay ninguna carpeta *cs/Control* correspondiente para C#.

- La clase `TreeNode` implementa el diseño jerárquico para TreeView. También contiene los datos que se vincularán a ella en la plantilla de elementos.
- La clase `TreeView` implementa eventos para ItemClick, expandir o contraer carpetas e iniciar el arrastre.
- La clase `TreeViewItem` implementa los eventos de la operación de colocación.
- La clase `ViewModel` aplana la lista TreeViewItems para que ciertas operaciones, como la navegación por teclado y arrastrar y colocar, se puedan heredar de ListView.

## <a name="create-a-data-template-for-your-treeviewitem"></a>Crear una plantilla de datos para TreeViewItem

Esta es la sección de XAML que configura la plantilla de datos para los elementos de carpeta y que no son de carpeta.
- Para especificar un ListViewItem como una carpeta, deberás establecer explícitamente la propiedad [AllowDrop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.allowdrop.aspx) como **true** en dicho ListViewItem. Este XAML muestra una manera de hacerlo.
- Para especificar un ListViewItem como que no es de carpeta, no es necesario especificar ninguna propiedad en el propio ListViewItem. Solo tienes que establecer la propiedad AllowDrop como True en ListView.
- Puedes usar iconos de carpeta expandidos/contraídos o comillas angulares para indicar visualmente si una carpeta está expandida o contraída.
- Puedes usar convertidores para elegir los diferentes iconos necesarios para el estado expandido en comparación con el estado contraído, tal como se muestra en este ejemplo.

```xaml
<!-- MainPage.xaml -->
<DataTemplate x:Key="TreeViewItemDataTemplate">
    <StackPanel Orientation="Horizontal" Height="40" Margin="{Binding Depth, Converter={StaticResource IntToIndConverter}}" AllowDrop="{Binding Data.IsFolder}">
        <FontIcon x:Name="expandCollapseChevron"
                  Glyph="{Binding IsExpanded, Converter={StaticResource expandCollapseGlyphConverter}}"
                  Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"                           
                  FontSize="12"
                  Margin="12,8,12,8"
                  FontFamily="Segoe MDL2 Assets"                          
                  />
        <Grid>
            <FontIcon x:Name ="expandCollapseFolder"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderGlyphConverter}}"
                      Foreground="#FFFFE793"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="nonFolderIcon"
                      Glyph="&#xE160;"
                      Foreground="{ThemeResource SystemControlForegroundBaseLowBrush}"
                      FontSize="12"
                      Margin="20,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource inverseBooleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="expandCollapseFolderOutline"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderOutlineGlyphConverter}}"
                      Foreground="#FFECC849"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"/>
        </Grid>

        <TextBlock Text="{Binding Data.Name}"
                   HorizontalAlignment="Stretch"
                   VerticalAlignment="Center"  
                   FontWeight="Medium"
                   FontFamily="Segoe MDL2 Assests"                           
                   Style="{ThemeResource BodyTextBlockStyle}"/>
    </StackPanel>
</DataTemplate>
```

## <a name="set-up-the-data-in-your-treeview"></a>Configurar los datos en TreeView

Este es el código que configura los datos en la muestra de TreeView.

```csharp
 public MainPage()
 {
     this.InitializeComponent();

     TreeNode workFolder = CreateFolderNode("Work Documents");
     workFolder.Add(CreateFileNode("Feature Functional Spec"));
     workFolder.Add(CreateFileNode("Feature Schedule"));
     workFolder.Add(CreateFileNode("Overall Project Plan"));
     workFolder.Add(CreateFileNode("Feature Resource allocation"));
     sampleTreeView.RootNode.Add(workFolder);

     TreeNode remodelFolder = CreateFolderNode("Home Remodel");
     remodelFolder.IsExpanded = true;
     remodelFolder.Add(CreateFileNode("Contactor Contact Information"));
     remodelFolder.Add(CreateFileNode("Paint Color Scheme"));
     remodelFolder.Add(CreateFileNode("Flooring woodgrain types"));
     remodelFolder.Add(CreateFileNode("Kitchen cabinet styles"));

     TreeNode personalFolder = CreateFolderNode("Personal Documents");
     personalFolder.IsExpanded = true;
     personalFolder.Add(remodelFolder);

     sampleTreeView.RootNode.Add(personalFolder);
 }

 private static TreeNode CreateFileNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) };
 }

 private static TreeNode CreateFolderNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) { IsFolder = true } };
 }
```

Cuando hayas terminado los pasos anteriores, tendrá un diseño de vista de árbol o jerárquico totalmente relleno con anidamiento de n niveles, compatibilidad con la expansión o contracción de carpetas, arrastrar y colocar entre las carpetas y accesibilidad integrada.

Para proporcionar al usuario la posibilidad de agregar o quitar elementos de la vista de árbol, te recomendamos que agregues un menú contextual para exponer las opciones para el usuario.


## <a name="related-articles"></a>Artículos relacionados

- [Muestra de TreeView](http://go.microsoft.com/fwlink/?LinkId=785018)
- [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView y GridView](listview-and-gridview.md)

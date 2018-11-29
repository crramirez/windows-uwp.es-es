---
ms.assetid: 26DF15E8-2C05-4174-A714-7DF2E8273D32
title: Optimización de la interfaz de usuario de ListView y GridView
description: Mejora el rendimiento y el tiempo de inicio de ListView y GridView mediante la virtualización de la interfaz de usuario, la reducción de elementos y la actualización progresiva de elementos.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5f8ddbdd1e8079e4b5bf945455bfa2efe7094203
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8191093"
---
# <a name="listview-and-gridview-ui-optimization"></a>Optimización de la interfaz de usuario de ListView y GridView


**Nota**  para obtener más información, consulta la sesión de //build/ [Considerablemente aumentar el rendimiento cuando los usuarios interactúan con grandes cantidades de datos de ListView y GridView](https://channel9.msdn.com/events/build/2013/3-158).

Mejorar el rendimiento y el tiempo de inicio de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) mediante la virtualización de la interfaz de usuario, la reducción de elementos y la actualización progresiva de elementos. Para más información sobre técnicas de virtualización de datos, consulta [la virtualización de datos de ListView y GridView](listview-and-gridview-data-optimization.md).

## <a name="two-key-factors-in-collection-performance"></a>Dos factores clave en el rendimiento de las colecciones

La manipulación de colecciones es un escenario común. Un visualizador de fotos tiene colecciones de fotos, un lector tiene colecciones de artículos, libros e historias, y una aplicación de compras tiene colecciones de productos. Este tema muestra lo que puedes hacer para que la aplicación sea eficaz en la manipulación de colecciones.

Existen dos factores clave en el rendimiento cuando se trata de colecciones: el primero es el tiempo invertido por el subproceso de la interfaz de usuario al crear elementos; el segundo es la memoria usada por el conjunto de datos sin procesar junto con los elementos de la interfaz de usuario usados para representar esos datos.

Para obtener unos movimientos panorámicos/desplazamientos suaves, es fundamental que el subproceso de la interfaz de usuario realice un trabajo eficaz e inteligente de crear una instancia, enlazar los datos y diseñar los elementos.

## <a name="ui-virtualization"></a>Virtualización de interfaz de usuario

La virtualización de la interfaz de usuario es la mejora más importante que puedes hacer. Esto significa que los elementos de la interfaz de usuario representen los elementos se crean a petición. Para controlar los elementos enlazados a una colección de 1 000 elementos, sería un desperdicio de recursos crear la interfaz de usuario de todos los elementos al mismo tiempo, porque no todos pueden mostrarse al mismo tiempo. [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) (y otros controles derivados estándar de [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803)) realizan la virtualización de la interfaz de usuario por ti. Cuando los elementos están a punto de desplazarse hacia la vista (a una páginas de distancia), el marco de trabajo genera la interfaz de usuario de los elementos y los almacena. Asimismo, cuando sea improbable que los elementos se muestren de nuevo, el marco de trabajo recupera la memoria.

Si proporcionas una plantilla del panel de elementos personalizada (consulta [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)), asegúrate de usar un panel de virtualización, como [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) o [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795). Si usas las clases [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227651), [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227717) o [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635), no obtendrás la virtualización. Además, los siguientes eventos [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) se generan únicamente cuando se usa una clase [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) o [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795): [**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer), [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) y [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging).

El concepto de una ventanilla es fundamental para la virtualización de la interfaz de usuario, porque el marco debe crear los elementos que es probable que se muestren. En general, la ventanilla de una clase [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) es del tamaño del control lógico. Por ejemplo, la ventanilla de un control [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) tiene el ancho y el alto del elemento **ListView**. Algunos paneles permiten que los elementos secundarios tengan espacio ilimitado, como [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) y [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), los cuales cuentan con columnas o filas de tamaño automático. Cuando una clase **ItemsControl** virtualizada se coloca en un panel como ese, necesita bastante espacio para mostrar todos sus elementos, lo que inhabilita la virtualización. Restaura la virtualización estableciendo un ancho y alto en **ItemsControl**.

## <a name="element-reduction-per-item"></a>Reducción del elemento por elemento

Reduce el número de elementos de la interfaz de usuario usada para representar los elementos, a un mínimo razonable.

Cuando un control de elementos se muestra por primera vez, se crean todos los elementos necesarios para representar una ventanilla llena completa de elementos. Además, a medida que los elementos se acercan a la ventanilla, el marco de trabajo actualiza los elementos de la interfaz de usuario en las plantillas de elementos almacenadas en caché con los objetos de datos enlazados. Minimizar la complejidad del marcado dentro de las plantillas se compensa en la memoria y en el tiempo invertido en el subproceso de la interfaz de usuario, lo que mejora la capacidad de respuesta especialmente durante el movimiento panorámico o en el desplazamiento. Las plantillas en cuestión son, la plantilla de elementos (consulta [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)) y la plantilla de control de una clase [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) o una clase [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) (la plantilla de control de elementos o [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)). La ventaja de tener incluso una pequeña reducción en el recuento de elementos, se multiplica por el número de elementos mostrados.

Para ver ejemplos de reducción de elementos, consulta [Optimizar el marcado XAML](optimize-xaml-loading.md).

Las plantillas de control predeterminadas para [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) y [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) contienen un elemento [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/Dn298500). Este presentador es un solo elemento optimizado que muestra los efectos visuales complejos de foco, selección y otros estados visuales. Si ya tienes plantillas de control de elementos personalizadas ([**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)) o si en el futuro editas una copia de una plantilla de control de elementos, te recomendamos que uses un elemento **ListViewItemPresenter**, ya que este elemento te dará un equilibrio óptimo entre el rendimiento y la capacidad de personalización en la mayoría de los casos. Personaliza este presentador estableciendo las propiedades en él. Por ejemplo, tenemos aquí el marcado que quita la marca que aparece de manera predeterminada cuando se selecciona un elemento y cambia el color de fondo del elemento seleccionado a naranja.

```xml
...
<ListView>
 ...
 <ListView.ItemContainerStyle>
 <Style TargetType="ListViewItem">
 <Setter Property="Template">
 <Setter.Value>
 <ControlTemplate TargetType="ListViewItem">
 <ListViewItemPresenter SelectionCheckMarkVisualEnabled="False" SelectedBackground="Orange"/>
 </ControlTemplate>
 </Setter.Value>
 </Setter>
 </Style>
 </ListView.ItemContainerStyle>
</ListView>
<!-- ... -->
```

Existen aproximadamente 25 propiedades con nombres descriptivos similares a [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) y [**SelectedBackground**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedbackground.aspx). En el caso de que los tipos de presentador resultaran no ser lo suficientemente personalizables para tu caso de uso, puedes editar una copia de la plantilla de control `ListViewItemExpanded` o `GridViewItemExpanded`. Estas se encuentran en `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`. Ten en cuenta que el uso de estas plantillas significa conceder algo de rendimiento para el aumento de la personalización.

<span id="update-items-incrementally"/>

## <a name="update-listview-and-gridview-items-progressively"></a>Actualizar los elementos ListView and GridView de forma progresiva

Si estás usando la virtualización de datos, puedes conseguir que la capacidad de respuesta de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) siga siendo alta, configurando el control para representar elementos de la interfaz de usuario temporales de los elementos que aún se estén descargando o cargando. Los elementos temporales se reemplazan progresivamente con la interfaz de usuario real a medida que se cargan los datos.

Igualmente, e independientemente de dónde estés cargando los datos (desde el disco local, la red o la nube), un usuario puede realizar un movimiento panorámico o de desplazamiento de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) o [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) tan rápido, que probablemente no te sea posible representar cada elemento con total fidelidad y conservando el movimiento panorámico o de desplazamiento suave. Para conservar el movimiento panorámico/desplazamiento suave, puedes representar un elemento en varias fases además de usar marcadores de posición.

Un ejemplo de estas técnicas suele verse en las aplicaciones de visualización de fotos: aunque no todas las imágenes se haya cargado visualizado, el usuario puede seguir realizando el movimiento panorámico/desplazamiento e interactuando con la colección. O bien, para un elemento de "película", puedes mostrar el título en la primera fase, la clasificación en la segunda fase y una imagen del póster en la tercera fase. El usuario ve los datos más importantes sobre cada elemento tan pronto como sea posible, lo que significa que podrá realizar una acción al mismo tiempo. A continuación, la información menos importante se rellena según lo permita el tiempo. Estas son las características de plataforma que puedes usar para implementar estas técnicas.

### <a name="placeholders"></a>Marcadores de posición

La característica temporal de elementos visuales de marcador de posición está activada de forma predeterminada y se controla con la propiedad [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders). Durante el movimiento panorámico o de desplazamiento rápido, esta característica proporciona al usuario una indicación visual de que quedan elementos por mostrarse completamente y, al mismo tiempo, se conserva la suavidad de movimientos. Si usas una de las técnicas siguientes, puedes establecer **ShowsScrollingPlaceholders** en "false" si prefieres que el sistema no represente marcadores de posición.

**Actualizaciones progresivas de plantillas de datos con x:Phase**

Aquí mostramos cómo usar el [atributo x:Phase](https://msdn.microsoft.com/library/windows/apps/Mt204790) con enlaces [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) para implementar las actualizaciones de plantilla de datos progresivas.

1.  Este es el aspecto que tiene el origen del enlace (es decir, el origen al que enlazaremos).

    ```csharp
    namespace LotsOfItems
    {
        public class ExampleItem
        {
            public string Title { get; set; }
            public string Subtitle { get; set; }
            public string Description { get; set; }
        }

        public class ExampleItemViewModel
        {
            private ObservableCollection<ExampleItem> exampleItems = new ObservableCollection<ExampleItem>();
            public ObservableCollection<ExampleItem> ExampleItems { get { return this.exampleItems; } }

            public ExampleItemViewModel()
            {
                for (int i = 1; i < 150000; i++)
                {
                    this.exampleItems.Add(new ExampleItem(){
                        Title = "Title: " + i.ToString(),
                        Subtitle = "Sub: " + i.ToString(),
                        Description = "Desc: " + i.ToString()
                    });
                }
            }
        }
    }
    ```
2.  Este es el marcado que contiene `DeferMainPage.xaml`. La vista de cuadrícula contiene una plantilla de elementos, que consta de elementos enlazados a las propiedades **Title**, **Subtitle** y **Description** de la clase **MyItem**. Ten en cuenta que **x:Phase** está establecido de forma predeterminada en 0. En este ejemplo, los elementos se representarán inicialmente con solo el título visible. El elemento de subtítulo serán datos enlazados y visibles para todos los elementos y así sucesivamente, hasta que se hayan procesado todas las fases.
    ```xml
    <Page
        x:Class="LotsOfItems.DeferMainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Text="{x:Bind Subtitle}" x:Phase="1"/>
                            <TextBlock Text="{x:Bind Description}" x:Phase="2"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```

3.  Si ejecutas la aplicación ahora y realizas rápidamente el movimiento panorámico o de desplazamiento a través de la vista de cuadrícula, verás que, a medida que aparece un elemento nuevo en la pantalla, este se representa en primer lugar como un rectángulo gris oscuro (debido a que la propiedad [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) está establecida de forma predeterminada en **true**) y que, a continuación, aparece el título, seguido del subtítulo, seguido de la descripción.

**Actualizaciones progresivas de la plantilla de datos mediante ContainerContentChanging**

La estrategia general del evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) es usar la propiedad **Opacity** para ocultar los elementos que no deben ser visibles inmediatamente. Cuando los elementos se reciclan, conservan sus valores antiguos, por que queremos ocultar esos elementos hasta que hayamos actualizado esos valores desde el elemento de datos nuevo. Para ello, usaremos la propiedad **Phase** en los argumentos del evento para determinar qué elementos actualizar y mostrar. Si se necesitan fases adicionales, se registra una devolución de llamada.

1.  Usaremos el mismo origen de enlace que para **x:Phase**.

2.  Este es el marcado que contiene `MainPage.xaml`. La vista de cuadrícula declara un controlador para su evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) y contiene una plantilla de elementos con elementos que se usan para mostrar las propiedades **Title**, **Subtitle** y **Description** de la clase **MyItem**. Para obtener los beneficios que aporta conseguir el máximo rendimiento **ContainerContentChanging**, no usaremos enlaces en el marcado; en su lugar, asignaremos valores mediante programación. La excepción aquí es el elemento que muestra el título, que consideramos que está en fase 0.
    ```xml
    <Page
        x:Class="LotsOfItems.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}" ContainerContentChanging="GridView_ContainerContentChanging">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Opacity="0"/>
                            <TextBlock Opacity="0"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```
3.  Por último, esta es la implementación del controlador de eventos [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging). Asimismo, este código también muestra cómo agregar una propiedad de tipo **RecordingViewModel** a **MainPage** para exponer la clase del origen de enlace, desde la clase que representa nuestra página de marcado. Siempre y cuando no haya ningún enlace [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) en la plantilla de datos, marca el objeto de argumentos de eventos como controlados en la primera fase del controlador para sugerir al elemento que no necesita establecer un contexto de datos.
    ```csharp
    namespace LotsOfItems
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new ExampleItemViewModel();
            }

            public ExampleItemViewModel ViewModel { get; set; }

            // Display each item incrementally to improve performance.
            private void GridView_ContainerContentChanging(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 0)
                {
                    throw new System.Exception("We should be in phase 0, but we are not.");
                }

                // It's phase 0, so this item's title will already be bound and displayed.

                args.RegisterUpdateCallback(this.ShowSubtitle);

                args.Handled = true;
            }

            private void ShowSubtitle(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 1)
                {
                    throw new System.Exception("We should be in phase 1, but we are not.");
                }

                // It's phase 1, so show this item's subtitle.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[1] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Subtitle;
                textBlock.Opacity = 1;

                args.RegisterUpdateCallback(this.ShowDescription);
            }

            private void ShowDescription(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 2)
                {
                    throw new System.Exception("We should be in phase 2, but we are not.");
                }

                // It's phase 2, so show this item's description.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[2] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Description;
                textBlock.Opacity = 1;
            }
        }
    }
    ```

4.  Si ejecutas la aplicación ahora y realizas rápidamente un movimiento panorámico o de desplazamiento a través de la vista de cuadrícula, verás el mismo comportamiento que para **x:Phase**.

## <a name="container-recycling-with-heterogeneous-collections"></a>Reciclaje de contenedores con colecciones heterogéneas

En algunas aplicaciones, debes tener una interfaz de usuario distinta para los diferentes tipos de elementos de una colección. Esto puede crear una situación en la que los paneles de virtualización no puedan reutilizar o reciclar los elementos visuales usados para mostrar los elementos. Recrear los elementos visuales de un elemento durante el movimiento panorámico elimina muchas de las ganancias de rendimiento que proporciona la virtualización. Sin embargo, con un poco de planeación se puede hacer que los paneles de virtualización puedan reutilizar los elementos. Los desarrolladores tienen dos opciones en función de su escenario: el evento [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) o un selector de plantilla de elementos. El enfoque de **ChoosingItemContainer** tiene un mejor rendimiento.

**El evento ChoosingItemContainer**

[**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) es un evento que te permite proporcionar un elemento (**ListViewItem**/**GridViewItem**) a los controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)/[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) cada vez que se necesita un nuevo elemento durante el inicio o el reciclado. Puedes crear un contenedor basado en el tipo de elemento de datos que mostrará el contenedor (puedes verlo en el siguiente ejemplo). **ChoosingItemContainer** es la mejor manera de conseguir el mayor rendimiento al usar diferentes plantillas de datos para distintos elementos. El almacenamiento en caché del contenedor se puede conseguir mediante **ChoosingItemContainer**. Por ejemplo, si tienes cinco plantillas diferentes y usas una de ellas más a menudo que las demás, entonces, el elemento ChoosingItemContainer no solo te permite crear elementos en las proporciones necesarias, sino también mantener un número apropiado de los elementos almacenados en caché y disponibles para su reciclaje. [**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer) proporciona la misma funcionalidad para encabezados de grupo.

```csharp
// Example shows how to use ChoosingItemContainer to return the correct
// DataTemplate when one is available. This example shows how to return different 
// data templates based on the type of FileItem. Available ListViewItems are kept
// in two separate lists based on the type of DataTemplate needed.
private void ListView_ChoosingItemContainer
    (ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    // Determines type of FileItem from the item passed in.
    bool special = args.Item is DifferentFileItem;

    // Uses the Tag property to keep track of whether a particular ListViewItem's 
    // datatemplate should be a simple or a special one.
    string tag = special ? "specialFiles" : "simpleFiles";

    // Based on the type of datatemplate needed return the correct list of 
    // ListViewItems, this could have also been handled with a hash table. These 
    // two lists are being used to keep track of ItemContainers that can be reused.
    List<UIElement> relevantStorage = special ? specialFileItemTrees : simpleFileItemTrees;

    // args.ItemContainer is used to indicate whether the ListView is proposing an 
    // ItemContainer (ListViewItem) to use. If args.Itemcontainer, then there was a 
    // recycled ItemContainer available to be reused.
    if (args.ItemContainer != null)
    {
        // The Tag is being used to determine whether this is a special file or 
        // a simple file.
        if (args.ItemContainer.Tag.Equals(tag))
        {
            // Great: the system suggested a container that is actually going to 
            // work well.
        }
        else
        {
            // the ItemContainer's datatemplate does not match the needed 
            // datatemplate.
            args.ItemContainer = null;
        }
    }

    if (args.ItemContainer == null)
    {
        // see if we can fetch from the correct list.
        if (relevantStorage.Count > 0)
        {
            args.ItemContainer = relevantStorage[0] as SelectorItem;
        }
        else
        {
            // there aren't any (recycled) ItemContainers available. So a new one 
            // needs to be created.
            ListViewItem item = new ListViewItem();
            item.ContentTemplate = this.Resources[tag] as DataTemplate;
            item.Tag = tag;
            args.ItemContainer = item;
        }
    }
}
```

**Selector de plantillas de elementos**

Un selector de plantillas de elementos ([**DataTemplateSelector**](https://msdn.microsoft.com/library/windows/apps/BR209469)) permite que una aplicación devuelva una plantilla de elementos diferente en tiempo de ejecución, en función del tipo de elemento de datos que se mostrará. Gracias a ello, el desarrollo es más productivo, pero la virtualización de la interfaz de usuario es más compleja ya que no todas las plantillas de elementos pueden volver a usarse en cada elemento de datos.

Al reciclar un elemento (**ListViewItem**/**GridViewItem**), el marco debe decidir si los elementos que están disponibles para su uso en la cola de reciclaje (la cola de reciclaje es una memoria caché de elementos que no se usan actualmente para mostrar datos) tienen una plantilla de elemento que coincida con la que necesita el elemento de datos actual. Si no hay elementos en la cola de reciclaje con la plantilla de elemento apropiada, entonces se crea un nuevo elemento, así como la instancia de la plantilla de elemento adecuada para él. Si, por otro lado, la cola de reciclaje contiene un elemento con la plantilla de elemento adecuada, dicho elemento se quita de la cola de reciclaje y se usa para el elemento de datos actual. Un selector de plantilla de elemento funciona en situaciones donde solo se usa un reducido número de plantillas de elemento y hay una distribución plana en toda la colección de elementos que usan las diferentes plantillas de elemento.

Cuando hay una distribución desigual de los elementos que usan plantillas de elemento diferentes, entonces es probable que se tengan que crear nuevas plantillas de elemento durante el movimiento panorámico, lo cual elimina muchas de las mejoras proporcionadas por la virtualización. Además, un selector de plantilla de elemento solo tiene en cuenta cinco posibles candidatos al evaluar si se puede reutilizar un contenedor particular para el elemento de datos actual. Por lo tanto, debes pensar detenidamente si tus datos son apropiados para usarlos con un selector de plantilla de elemento antes de usar uno en tu aplicación. Si tu colección es principalmente homogénea, el selector devuelve el mismo tipo la mayoría del tiempo (posiblemente todo el tiempo). Solo debes tener en cuenta el esfuerzo que te supone tener excepciones poco frecuentes en esa homogeneidad y decidir si prefieres usar [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) (o dos controles de elementos).

 


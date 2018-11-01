---
author: muhsinking
Description: A semantic zoom control allows the user to zoom between two different semantic views of the same data set.
title: Zoom semántico
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 17391b48b7b75209321f35ffd20610e17e3935c4
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5877117"
---
# <a name="semantic-zoom"></a>Zoom semántico

 

El zoom semántico permite que el usuario cambie entre dos vistas distintas del mismo contenido para poder navegar rápidamente a través de un gran conjunto de datos agrupados.
 
- La vista ampliada es la vista principal del contenido. Esta es la vista principal donde se muestran los elementos de datos individuales. 
- La vista alejada es un nivel superior del mismo contenido. Por lo general, en esta vista se muestran los encabezados de grupo para un conjunto de datos agrupados. 

Por ejemplo, al ver una libreta de direcciones, el usuario puede alejarse para pasar rápidamente a la letra "W" y ampliar para ver los nombres con esa letra. 

> **API importantes**: [Clase SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601), [Clase ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx), [Clase GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx)

**Características**:

-   El tamaño de la vista alejada está restringido por los límites del control del zoom semántico.
-   Al pulsar en un encabezado de grupo, se alterna entre las vistas. Se puede habilitar el gesto de reducir como una forma de alternar entre las vistas.
-   Los encabezados activos cambian entre vistas.

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control **SemanticZoom** si es necesario para mostrar un conjunto de datos agrupado lo suficientemente grande como para que no se pueda mostrar en una o dos páginas.

No confundas el zoom semántico con el zoom óptico. Aunque comparten la misma interacción y el comportamiento básico (muestran más o menos detalle en función de un factor de zoom), el zoom óptico hace referencia al ajuste de ampliación de un área de contenido o un objeto, como una fotografía. Para obtener información sobre un control que aplique el zoom óptico, consulta el control [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/SemanticZoom">abrir la aplicación y ver SemanticZoom en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Aplicación Fotos**

Este es un zoom semántico usado en la aplicación Fotos. Las fotos se agrupan por mes. Al seleccionar un encabezado de mes en la vista de cuadrícula predeterminada, se aleja a la vista de lista de mes para obtener una navegación más rápida.

![Un zoom semántico en la aplicación Fotos](images/control-examples/semantic-zoom-photos.png)

**Libreta de direcciones**

Una libreta de direcciones es otro ejemplo de un conjunto de datos por el que puede ser mucho más fácil navegar con un zoom semántico. Puedes usar la vista alejada para saltar rápidamente a la letra que quieras (imagen de la izquierda), mientras que la vista ampliada muestra los elementos de datos individuales (imagen de la derecha).

![Ejemplo de zoom semántico en una lista de contactos](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>Creación de un zoom semántico

El control **SemanticZoom** no tiene una representación visual de sí mismo. Es un control de host que administra la transición entre otros 2 controles que proporcionan las vistas de tu contenido, normalmente los controles **ListView** o **GridView**.  Puedes establecer los controles de vista en las propiedades de [ZoomedInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx) y [ZoomedOutView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx) del elemento SemanticZoom.

Los 3 elementos que necesitas para un zoom semántico son:
- Un origen de datos agrupados
- Una vista ampliada que muestre los datos a nivel de elemento.
- Una vista alejada que muestre los datos a nivel de grupo.

Antes de usar el zoom semántico, debes saber cómo usar una vista de lista con los datos agrupados. Para obtener más información, consulta [Vista de lista y vista de cuadrícula](listview-and-gridview.md) y [Agrupación de elementos en una lista](). 

> **Nota**&nbsp;&nbsp;Para definir la vista ampliada y la vista alejada del control SemanticZoom, puedes usar cualquiera de los dos controles que implementan la interfaz [ISemanticZoomInformation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.isemanticzoominformation.aspx). El marco XAML proporciona 3 controles que implementan esta interfaz: ListView, GridView y Hub.
 
 Este XAML muestra la estructura del control SemanticZoom. Puedes asignar otros controles a las propiedades ZoomedOutView y ZoomedInView.
 
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed in view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed out view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Estos ejemplos se han tomado de la página SemanticZoom del [Ejemplo de conceptos básicos de interfaz de usuario de XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992). Puedes descargar el ejemplo para ver el código completo, incluyendo el origen de datos. El zoom semántico usa un control GridView para proporcionar la vista ampliada y un control ListView para la vista alejada.
  
**Definición de la vista ampliada**

Este es el control GridView para la vista ampliada. La vista ampliada muestra los elementos de datos individuales en grupos. En este ejemplo se muestra cómo visualizar los elementos en una cuadrícula con una imagen y texto. 

```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
El aspecto de los encabezados de grupo se define en el recurso `ZoomedInGroupHeaderTemplate`. La apariencia de los elementos se define en el recurso `ZoomedInTemplate`. 

```xaml
<DataTemplate x:Key="ZoomedInGroupHeaderTemplate" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**Definir la vista alejada**

Este código XAML define un control ListView para la vista alejada. Este ejemplo muestra cómo mostrar los encabezados de grupo como texto en una lista.

```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 El aspecto se define en el recurso `ZoomedOutTemplate`.
 
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Sincronizar las vistas**

La vista alejada y la vista ampliada deberían estar sincronizadas, de modo que si un usuario sincroniza un grupo en la vista alejada, los detalles de ese mismo grupo se muestran en la vista ampliada. Puedes usar una clase [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) o agregar código para sincronizar las vistas.

Todos los controles que vínculos a la misma clase CollectionViewSource tendrán siempre el mismo elemento actual. Si las dos vistas usan el mismo CollectionViewSource que su origen de datos, la clase CollectionViewSource sincroniza las vistas automáticamente. Para obtener más información, consulta [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

Si no usas una clase [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx) para sincronizar las vistas, deberías controlar el evento ViewChangeStarted y sincronizar los elementos del controlador de eventos, tal como se muestra aquí.

```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## <a name="recommendations"></a>Recomendaciones

-   Al usar el zoom semántico en la aplicación, asegúrate de que el diseño de los elementos y la dirección del movimiento panorámico no cambian en función del nivel de zoom. Las interacciones de movimiento panorámico y de diseño deben ser predecibles y coherentes en todos los niveles de zoom.
-   El zoom semántico permite al usuario saltar rápidamente al contenido, de forma que se limita el número de páginas y pantallas a tres en el modo alejado. Demasiado movimiento panorámico disminuye la utilidad del zoom semántico.
-   Evita usar el zoom semántico para cambiar el ámbito del contenido. Por ejemplo, un álbum de fotos no debería cambiar a una vista de carpeta en el Explorador de archivos.
-   Usa una estructura y una semántica que sean esenciales para la vista.
-   Usa los nombres de los grupos para elementos en una colección agrupada.
-   Usa criterios de ordenación de una colección que esté sin agrupar, pero ordenada, como orden cronológico de fechas u orden alfabético de una lista de nombres.


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.


## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos de diseño de la navegación](../basics/navigation-basics.md)
- [Vista de lista y vista de cuadrícula](listview-and-gridview.md)
- [Plantillas y contenedores de elementos](item-containers-templates.md)






---
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: Optimiza tu diseño XAML
description: El diseño puede ser una parte costosa de una aplicación XAML, \#tanto por la sobrecarga de memoria como por el uso de la CPU. A continuación te mostramos algunos sencillos pasos para mejorar el rendimiento de diseño de la aplicación XAML.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29bf53560759b8e691ac016987efb0226ace9309
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154179"
---
# <a name="optimize-your-xaml-layout"></a>Optimiza tu diseño XAML


**API importantes**

-   [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)

El diseño es el proceso de definir la estructura visual de la interfaz de usuario. El mecanismo principal para describir el diseño en XAML son los paneles, que son objetos contenedores que te permiten colocar y organizar los elementos de interfaz de usuario dentro de ellos. El diseño puede ser una parte costosa de una aplicación XAML; tanto en la sobrecarga de memoria como en el uso de la CPU. A continuación te mostramos algunos sencillos pasos para mejorar el rendimiento de diseño de la aplicación XAML.

## <a name="reduce-layout-structure"></a>Reducir la estructura de diseño

La mayor mejora en el rendimiento de diseño proviene de simplificar la estructura jerárquica del árbol de los elementos de la interfaz de usuario. Los paneles existen en el árbol visual, pero son elementos estructurales, no *elementos productores de píxeles* como sería una clase [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) o [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). El hecho de simplificar el árbol reduciendo el número de elementos no productores de píxeles, proporciona, por lo general, un aumento significativo del rendimiento.

Muchas interfaces de usuario se implementan anidando paneles que dan como resultado árboles de paneles y elementos profundos y complejos. Anidar paneles es práctico pero, en muchos casos, la misma interfaz de usuario se puede lograr con un panel simple más complejo. Usar un panel simple proporciona un mejor rendimiento.

### <a name="when-to-reduce-layout-structure"></a>Cuándo reducir la estructura del diseño

Reducir la estructura del diseño de una manera trivial como, por ejemplo, reducir un panel anidado desde la página de nivel superior, no tiene un efecto perceptible.

La mayor mejora que puedes conseguir en el rendimiento, es mediante la reducción de la estructura de diseño que se repite en la interfaz de usuario, como sucede en la clase [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) o [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView). Estos elementos [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) usan una clase [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate), que define un subárbol de elementos de interfaz de usuario del que se crean instancias varias veces. Cuando el mismo subárbol se duplica varias veces en la aplicación, cualquier mejora en el rendimiento de ese subárbol tiene un efecto multiplicativo en el rendimiento general de la aplicación.

### <a name="examples"></a>Ejemplos

Ten en cuenta la siguiente interfaz de usuario.

![Ejemplo de diseño de formulario](images/layout-perf-ex1.png)

Estos ejemplos muestran 3 formas de implementar la misma interfaz de usuario. Cada opción de implementación tiene como resultado píxeles casi idénticos en la pantalla, pero varía considerablemente en los detalles de implementación.

Opción 1: Elementos [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) anidados

Aunque este es el modelo más sencillo, usa 5 elementos de panel y produce una sobrecarga significativa.

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

Opción 2: Un solo control [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)

La clase [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) agrega cierta complejidad, pero solo usa un elemento de panel simple.

```xml
<Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

Opción 3: Un solo control [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)

Este panel simple también resulta un poco más complejo que usar paneles anidados, pero puede ser más fácil de comprender y mantener que una clase [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid).

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

Tal como se muestra en estos ejemplos, existen muchas formas de lograr la misma interfaz de usuario. Debes elegir teniendo en cuenta todos los inconvenientes, incluido el rendimiento, la legibilidad y el mantenimiento.

## <a name="use-single-cell-grids-for-overlapping-ui"></a>Usar cuadrículas de una sola celda para superponer la interfaz de usuario

Un requisito común de la interfaz de usuario es tener un diseño en el que los elementos se superpongan entre sí. Por lo general el espaciado interno, los márgenes, las alineaciones y las transformaciones se usan para ubicar los elementos de esta forma. El control [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) de XAML está optimizado para mejorar el rendimiento de diseño de los elementos que se superponen.

**Importante**  Para ver la mejora, usa un objeto [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) de una sola celda. No definas las propiedades [**RowDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) ni [**ColumnDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.columndefinitions).

### <a name="examples"></a>Ejemplos

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![Texto superpuesto en un círculo](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![Dos bloques de texto en una cuadrícula](images/layout-perf-ex3.png)

## <a name="use-a-panels-built-in-border-properties"></a>Usar las propiedades de borde integradas de un panel

Los controles [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel), [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) y [**ContentPresenter**](/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter) tienen propiedades de borde integradas que te permiten dibujar un borde alrededor de ellos sin tener que agregar un elemento [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) adicional al XAML. Las nuevas propiedades que admiten el borde integrado son: **BorderBrush**, **BorderThickness**, **CornerRadius** y **Padding**. Cada una de ellas es una clase [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty), por lo que puedes usarlas con enlaces y animaciones. Además, están diseñadas para reemplazar completamente otro elemento **Border**.

Si tu interfaz de usuario tiene elementos [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) alrededor de estos paneles, usa el borde integrado en su lugar, ya que guarda un elemento adicional en la estructura de diseño de la aplicación. Tal como se mencionó anteriormente, esto puede suponer un ahorro significativo, especialmente en el caso de interfaces de usuario repetidas.

### <a name="examples"></a>Ejemplos

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## <a name="use-sizechanged-events-to-respond-to-layout-changes"></a>Usar eventos **SizeChanged** para responder a cambios de diseño

La clase [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) expone dos eventos similares para responder a los cambios de diseño: [**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) y [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Puede que estés usando uno de estos eventos para recibir una notificación cuando cambia el tamaño de un elemento durante el diseño. La semántica de los dos eventos es diferente y hay aspectos importantes de rendimiento para elegir entre ellos.

Para lograr un buen rendimiento, [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) es casi siempre la opción adecuada. **SizeChanged** tiene una semántica intuitiva. Se genera durante el diseño una vez que se actualiza el tamaño de la clase [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement).

[**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) también se genera durante el diseño, pero tiene una semántica global que se genera en todos los elementos cada vez que se actualiza cualquiera de ellos. Es habitual usarla para realizar solo procesamiento local en el controlador de eventos, en cuyo caso el código se ejecuta con más frecuencia de la necesaria. Usa **LayoutUpdated** solo si necesitas saber cuándo cambia la posición de un elemento sin que haya cambiado el tamaño (que es poco habitual).

## <a name="choosing-between-panels"></a>Elegir entre los paneles

El rendimiento no suele ser algo a tener en cuenta al elegir entre los paneles individuales. Esa elección se suele realizar teniendo en cuenta qué panel proporciona el comportamiento de diseño más cercano a la interfaz de usuario que estás implementando. Por ejemplo, si estás eligiendo entre [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) y [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), debes elegir el panel que proporcione la asignación más cercana al modelo de la implementación que tenías en mente.

Cada panel XAML está optimizado para lograr un buen rendimiento y todos los paneles proporcionan un rendimiento similar para interfaces de usuario similares.
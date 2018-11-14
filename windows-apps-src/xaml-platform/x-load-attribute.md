---
author: jwmsft
title: atributo xLoad
description: xLoad permite la creación dinámica y la destrucción de un elemento y sus elementos secundarios, reducir el uso de memoria y el tiempo de inicio. 
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d9659d183c020c579aa0a21fe179a69c1d9997c5
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "6143021"
---
# <a name="xload-attribute"></a>Atributo x:Load

Puedes usar **x: Load** para optimizar el inicio, la creación de árbol visual y el uso de memoria de la aplicación XAML. Usar **x: Load** tiene un efecto visual similar a la **visibilidad**, salvo que cuando el elemento no se carga, su memoria se libera e internamente un marcador de posición pequeño se usa para marcar su lugar en el árbol visual.

Puede ser el elemento de interfaz de usuario el atributo x: Load cargando y descargado a través de código o con una expresión [x: Bind](x-bind-markup-extension.md) . Esto es útil para reducir los costes de los elementos que no se muestran a menudo o que se muestran de forma condicional. Cuando usas x: Load en un contenedor, como de cuadrícula o StackPanel, el contenedor y todos sus elementos secundarios se carga o descarga como un grupo.

El seguimiento de los elementos aplazados por el marco XAML agrega alrededor de 600 bytes al uso de memoria de cada elemento atribuida con x: Load, para tener en cuenta el marcador de posición. Por lo tanto, es posible exceso este atributo en la medida en que el rendimiento disminuirá realmente. Te recomendamos que solo usan en los elementos que deben estar oculto. Si usas x: Load en un contenedor, la sobrecarga se paga solo para el elemento con el atributo x: Load.

> [!IMPORTANT]
> El atributo x: Load está disponible a partir de Windows 10, versión 1703 (Creators Update). Para poder usar x:Load, la versión mínima del proyecto de Visual Studio debe ser *Windows10 Creators Update (10.0, compilación 15063)*.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Carga de elementos

Hay varias formas diferentes para cargar los elementos:

- Usa una expresión [x: Bind](x-bind-markup-extension.md) para especificar el estado de carga. La expresión debería devolver **true** para cargar y **false** para descargar el elemento.
- Llama al método [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) con el nombre definido en el elemento.
- Llama al método [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) con el nombre definido en el elemento.
- En un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), usa una animación [**establecedor**](https://msdn.microsoft.com/library/windows/apps/br208817) o el **guión gráfico** que esté orientado al elemento x: Load.
- Como destino el elemento descargado en cualquier **guión gráfico**.

> NOTA: una vez iniciada la creación de instancias de un elemento, se crea en el subproceso de la interfaz de usuario, lo que podría provocar problemas de estabilidad en la interfaz de usuario si se crean demasiados al mismo tiempo.

Una vez creado el elemento diferido con cualquiera de los métodos enumerados anteriormente, sucederán varias cosas:

- Se generará el evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) en el elemento.
- Se establece el campo para x: Name.
- Todos los enlaces de x: Bind en el elemento se evaluarán.
- Si la aplicación se ha registrado para recibir notificaciones de cambios de propiedad en la propiedad que contiene los elementos diferidos, se mostrará una notificación.

## <a name="unloading-elements"></a>Elementos de la descarga

Para descargar un elemento:

- Usa una expresión x: Bind para especificar el estado de carga. La expresión debería devolver **true** para cargar y **false** para descargar el elemento.
- En una página o un UserControl, llama a **UnloadObject** y pasar la referencia de objeto
- Llama a **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** y pasar la referencia de objeto

Cuando se ha descargado un objeto, se reemplazará con un marcador de posición en el árbol. La instancia del objeto permanecerá en la memoria hasta que se han publicado todas las referencias. La API de UnloadObject en un página o UserControl está diseñada para liberar las referencias mantenidas por codegen para x: Name y x: Bind. Si mantienes referencias adicionales en el código de la aplicación también deberás a que se publique.

Cuando un elemento se ha descargado, todos los Estados asociados con el elemento se descartarán, por lo tanto, si con x: Load como una versión optimizada de visibilidad, a continuación, asegúrate de que todas de estado se aplica a través de enlaces o se vuelve a aplicar al código cuando se desencadena el evento Loaded.

## <a name="restrictions"></a>Restricciones

Las restricciones para usar **x: Load** son:

- Debes definir un [x: Name](x-name-attribute.md)para el elemento, ya que es necesario tener una manera de encontrar el elemento más adelante.
- Solo puedes usar x: Load en tipos que deriven de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) o [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- No puedes usar x: Load en los elementos raíz en una [**página**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), un [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)o una [**clase DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
- No puedes usar x: Load en los elementos de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
- No puedes usar x: Load en XAML sueltos cargados con [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Mover un elemento primario borrará todos los elementos que no se han cargado.

## <a name="remarks"></a>Observaciones

Puedes usar x: Load en elementos anidados, pero que deben llevarse a cabo desde el elemento más externo. Si intentas crear un elemento secundario antes de crear el elemento primario, se producirá una excepción.

Por lo general, te recomendamos que aplaces aquellos elementos que no sean visibles en el primer fotograma.Una buena opción para encontrar candidatos para aplazar es buscar elementos que se vayan a crear con una propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) contraída. Asimismo, la interfaz de usuario que se desencadena debido a la interacción del usuario es un buen lugar para buscar elementos que puedes aplazar.

Ten cuidado con el aplazamiento de elementos en escenarios [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), ya que se reducirá el tiempo de inicio; aunque también puede disminuir el rendimiento del movimiento panorámico en función de lo que vayas a crear. Si buscas aumentar el rendimiento del movimiento panorámico, consulta la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y del [atributo x:Phase](x-phase-attribute.md).

Si se usa el [atributo x: Phase](x-phase-attribute.md) junto con **x: Load** a continuación, cuando se crea un elemento o un árbol elementos, los enlaces se aplicarán hasta la fase actual incluida. La fase especificada para **x: Phase** afectará ni controlará el estado de carga del elemento. Cuando un elemento de lista se recicla como parte del movimiento panorámico, realizado los elementos comportarán de la misma manera como otros elementos activos, y enlaces compilados (enlaces **{X: Bind}** ) se procesarán mediante las mismas reglas, incluidos el escalonamiento.

Como regla general, se recomienda evaluar el rendimiento de la aplicación antes y después, para asegurarte de que vas a obtener el rendimiento deseado.

## <a name="example"></a>Ejemplo

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```


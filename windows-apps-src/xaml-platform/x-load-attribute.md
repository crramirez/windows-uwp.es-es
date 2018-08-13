---
author: jwmsft
title: atributo xLoad
description: xLoad permite la creación dinámica y destruir un elemento y sus elementos secundarios, lo que reduce el uso de memoria y tiempo de inicio.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8f34ea43b981de65c81e9cce2b8a896c3577e595
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "610790"
---
# <a name="xload-attribute"></a>Atributo x:Load

Puede usar **X: carga** para optimizar el inicio, la creación del árbol visual y el uso de memoria de la aplicación XAML. Uso de **X: carga** tiene un efecto visual similar a la **visibilidad**, excepto que cuando el elemento no se carga, su memoria se libera e internamente un pequeño marcador de posición se usa para marcar su lugar en el árbol visual.

El elemento de la interfaz de usuario que se expresarán con x: carga puede ser cargado y descargado a través de código, o con una expresión [X: enlazar](x-bind-markup-extension.md) . Esto es útil para reducir los costes de los elementos que no se muestran a menudo o que se muestran de forma condicional. Cuando use x: carga en un contenedor como cuadrícula o StackPanel, el contenedor y todos sus elementos secundarios se cargan o descargan como un grupo.

El seguimiento de los elementos diferidos por el marco de XAML agrega aproximadamente 600 bytes para el uso de memoria para cada elemento que se expresarán con x: carga, para tener en cuenta para el marcador de posición. Por lo tanto, es posible uso excesivo de este atributo en la medida en que realmente disminuye el rendimiento. Se recomienda que lo use sólo en los elementos que necesitan que esté oculto. Si usa x: carga en un contenedor, la sobrecarga se paga sólo para el elemento con el atributo x: carga.

> [!IMPORTANT]
> El atributo x: carga está disponible a partir de Windows 10, versión 1703 (los creadores de actualización). Para poder usar x:Load, la versión mínima del proyecto de Visual Studio debe ser *Windows10 Creators Update (10.0, compilación 15063)*.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Carga de elementos

Hay varias maneras de cargar los elementos:

- Use una expresión [X: enlazar](x-bind-markup-extension.md) para especificar el estado de carga. La expresión debe devolver **true** para cargar y **false** para descargar el elemento.
- Llama al método [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) con el nombre definido en el elemento.
- Llama al método [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) con el nombre definido en el elemento.
- En un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), use una animación de [**Set**](https://msdn.microsoft.com/library/windows/apps/br208817) o **guión gráfico** que se dirige el elemento x: carga.
- El elemento descargado en cualquier **guión gráfico**de destino.

> NOTA: una vez iniciada la creación de instancias de un elemento, se crea en el subproceso de la interfaz de usuario, lo que podría provocar problemas de estabilidad en la interfaz de usuario si se crean demasiados al mismo tiempo.

Una vez creado el elemento diferido con cualquiera de los métodos enumerados anteriormente, sucederán varias cosas:

- Se generará el evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) en el elemento.
- Se establece el campo para x: Name.
- Se evalúan los enlaces de x: enlazar en el elemento.
- Si la aplicación se ha registrado para recibir notificaciones de cambios de propiedad en la propiedad que contiene los elementos diferidos, se mostrará una notificación.

## <a name="unloading-elements"></a>Descargar elementos

Para descargar un elemento:

- Use una expresión x: enlazar para especificar el estado de carga. La expresión debe devolver **true** para cargar y **false** para descargar el elemento.
- En una página o UserControl, llame a **UnloadObject** y pasar en la referencia de objeto
- Llame a **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** y pase la referencia de objeto

Cuando se descarga un objeto, se reemplazará con un marcador de posición en el árbol. La instancia del objeto permanecerá en la memoria hasta que se han liberado todas las referencias. La API UnloadObject en un página o UserControl está diseñada para liberar las referencias llevadas a cabo por codegen para x: Name y x: enlazar. Si tiene referencias adicionales en el código de la aplicación también tendrá que liberar.

Cuando un elemento se ha descargado, todos los Estados asociados con el elemento se descartarán, por lo que si usa x: carga como una versión optimizada de visibilidad, a continuación, asegúrese de que todos los de estado se aplica a través de los enlaces o se vuelve a aplicar por código cuando se desencadena el evento Loaded.

## <a name="restrictions"></a>Restricciones

Las restricciones para el uso de **X: carga** son:

- Debes definir un atributo [x:Name](x-name-attribute.md) para el elemento, ya que es necesario tener una manera de encontrar ese elemento más adelante.
- Sólo se puede utilizar x: carga en tipos que se derivan de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) o [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- No puede usar x: carga en los elementos raíz de una [**página**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), un [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)o una [**plantilla de datos**](https://msdn.microsoft.com/library/windows/apps/br242348).
- No puede usar x: carga en los elementos de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
- No puede usar x: carga en XAML dinámico cargado con [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Mover un elemento primario, se borrará el todos los elementos que no se han cargado.

## <a name="remarks"></a>Observaciones

Puede usar x: carga en los elementos anidados, sin embargo que deben llevarse a cabo desde el elemento más exterior.  Si intentas crear un elemento secundario antes de crear el elemento primario, se producirá una excepción.

Por lo general, te recomendamos que aplaces aquellos elementos que no sean visibles en el primer fotograma. Una buena opción para encontrar candidatos para aplazar es buscar elementos que se vayan a crear con una propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) contraída. Asimismo, la interfaz de usuario que se desencadena debido a la interacción del usuario es un buen lugar para buscar elementos que puedes aplazar.

Ten cuidado con el aplazamiento de elementos en escenarios [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), ya que se reducirá el tiempo de inicio; aunque también puede disminuir el rendimiento del movimiento panorámico en función de lo que vayas a crear. Si buscas aumentar el rendimiento del movimiento panorámico, consulta la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y del [atributo x:Phase](x-phase-attribute.md).

Si se utiliza el [atributo x: fase](x-phase-attribute.md) junto con **X: carga** , a continuación, cuando se lleva a cabo un elemento o un árbol de elementos, se aplican los enlaces hasta e incluyendo la fase actual. La fase de especificado para **X: fase** afectan a o controlar el estado de carga del elemento. Cuando un elemento de lista se recicle como parte de panorámica, obtenidas elementos comportan de la misma manera que los demás elementos activos y enlaces compilados (**{X: enlazar}** enlaces) se procesan utilizando las mismas reglas, incluida la eliminación progresiva.

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


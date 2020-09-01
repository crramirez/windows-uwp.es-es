---
title: atributo xLoad
description: xLoad permite la creación y destrucción dinámicas de un elemento y sus elementos secundarios, lo que reduce el tiempo de inicio y el uso de memoria.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b110091c1a1f208ad06ea52f5c84dc7daad56c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161689"
---
# <a name="xload-attribute"></a>Atributo x:Load

Puede usar **x:Load** para optimizar el inicio, la creación de árboles visuales y el uso de memoria de la aplicación XAML. El uso de **x:Load** tiene un efecto visual similar a la **visibilidad**, salvo que cuando el elemento no se carga, su memoria se libera y internamente se usa un marcador de posición pequeño para marcar su lugar en el árbol visual.

El elemento de la interfaz de usuario con atributos x:Load se puede cargar y descargar mediante código o mediante una expresión [x:Bind](x-bind-markup-extension.md) . Esto resulta útil para reducir los costos de los elementos que se muestran con poca frecuencia o de manera condicional. Cuando se usa x:Load en un contenedor como Grid o StackPanel, el contenedor y todos sus elementos secundarios se cargan o descargan como un grupo.

El seguimiento de los elementos diferidos por el marco XAML agrega aproximadamente 600 bytes al uso de memoria para cada elemento con el atributo x:Load, para tener en cuenta el marcador de posición. Por lo tanto, es posible utilizar este atributo en la medida en que el rendimiento disminuye realmente. Se recomienda usarlo únicamente en los elementos que deban ocultarse. Si usa x:Load en un contenedor, la sobrecarga solo se paga por el elemento con el atributo x:Load.

> [!IMPORTANT]
> El atributo x:Load está disponible a partir de Windows 10, versión 1703 (Creators Update). La versión mínima de destino de su proyecto de Visual Studio debe ser *Windows 10 Creators Update (10,0, compilación 15063)* para poder usar x:Load.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Cargar elementos

Hay varias maneras de cargar los elementos:

- Use una expresión [x:Bind](x-bind-markup-extension.md) para especificar el estado de carga. La expresión debe devolver **true** para cargar y **false** para descargar el elemento.
- Llame a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) con el nombre que ha definido en el elemento.
- Llame a [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) con el nombre que ha definido en el elemento.
- En un [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), use un [**establecedor**](/uwp/api/Windows.UI.Xaml.Setter) o una animación de **guion gráfico** que tenga como destino el elemento x:Load.
- El destino es el elemento descargado en cualquier **guion gráfico**.

> NOTA: una vez iniciada la creación de instancias de un elemento, se crea en el subproceso de la interfaz de usuario, lo que podría provocar problemas de estabilidad en la interfaz de usuario si se crean demasiados al mismo tiempo.

Una vez que se crea un elemento diferido en cualquiera de las formas enumeradas anteriormente, suceden varias cosas:

- Se genera el evento [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) en el elemento.
- Se establece el campo de x:Name.
- Se evalúa cualquier enlace x:Bind del elemento.
- Si se ha registrado para recibir notificaciones de cambio de propiedad en la propiedad que contiene los elementos aplazados, se genera la notificación.

## <a name="unloading-elements"></a>Descargar elementos

Para descargar un elemento:

- Use una expresión x:Bind para especificar el estado de carga. La expresión debe devolver **true** para cargar y **false** para descargar el elemento.
- En una página o UserControl, llame a **UnloadObject** y pase la referencia de objeto.
- Llamar a **Windows. UI. Xaml. Markup. XamlMarkupHelper. UnloadObject** y pasar la referencia de objeto

Cuando se descarga un objeto, se reemplazará en el árbol con un marcador de posición. La instancia del objeto permanecerá en la memoria hasta que se hayan liberado todas las referencias. La API de UnloadObject en una página/UserControl está diseñada para liberar las referencias contenidas por CODEGEN para x:Name y x:Bind. Si contiene referencias adicionales en el código de la aplicación, también necesitarán liberarse.

Cuando se descarga un elemento, se descarta todo el estado asociado al elemento, por lo que si se usa x:Load como una versión optimizada de visibilidad, se garantiza que todos los Estados se apliquen a través de los enlaces o que el código vuelva a aplicarlo cuando se desencadene el evento Loaded.

## <a name="restrictions"></a>Restricciones

Las restricciones para el uso de **x:Load** son las siguientes:

- Debe definir un [x:Name](x-name-attribute.md)   para el elemento, ya que debe haber una manera de encontrar el elemento más adelante.
- Solo se puede usar x:Load en tipos que derivan de [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) o [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- No se puede usar x:Load en los elementos raíz de una página, un [**control**](/uwp/api/windows.ui.xaml.controls.page) [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)o un [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate).
- No se puede usar x:Load en los elementos de un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- No se puede usar x:Load en el código XAML dinámico cargado con [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Al mover un elemento primario, se borrarán todos los elementos que no se hayan cargado.

## <a name="remarks"></a>Observaciones

Puede usar x:Load en elementos anidados, sin embargo, se deben realizar desde el elemento más externo en. Si intenta obtener un elemento secundario antes de que se haya realizado el elemento primario, se produce una excepción.

Normalmente, se recomienda aplazar los elementos que no se pueden ver en el primer fotograma.Una buena opción para encontrar candidatos a ser aplazados es buscar elementos que se vayan a crear con [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) contraída. Además, la interfaz de usuario desencadenada por la interacción con el usuario es un buen lugar para buscar los elementos que se pueden aplazar.

Tenga cuidado al aplazar elementos en un [**control ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView), ya que disminuirá el tiempo de inicio, pero también puede reducir el rendimiento de la panorámica en función de lo que esté creando. Si desea aumentar el rendimiento de la panorámica, consulte la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y el [atributo x:Phase](x-phase-attribute.md) .

Si el [atributo x:Phase](x-phase-attribute.md) se usa junto con **x:Load** , cuando se realiza un elemento o un árbol de elementos, los enlaces se aplican hasta la fase actual, inclusive. La fase especificada para **x:Phase** afecta o controla el estado de carga del elemento. Cuando un elemento de lista se recicla como parte de la panorámica, los elementos realizados se comportarán de la misma manera que otros elementos activos y los enlaces compilados (**{x:Bind}** bindings) se procesan con las mismas reglas, incluido el escalonamiento.

Una directriz general es medir el rendimiento de la aplicación antes y después para asegurarse de que obtiene el rendimiento que desea.

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
---
title: Atributo xLoad
description: El atributo xLoad te permite crear y eliminar de forma dinámica un elemento y sus elementos secundarios, para reducir el tiempo de inicio y el uso de memoria.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d85051aabdb7631c5bdb84e08d6d10a0f70d6ede
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372292"
---
# <a name="xload-attribute"></a>Atributo x:Load

Puedes usar **x:Load** para optimizar el inicio, la creación de árboles visuales y el uso de memoria de la aplicación XAML. Al usar **x:Load** obtendrás los mismos efectos visuales que al usar **Visibility**, solo que cuando no se carga el elemento, se libera la memoria y se usa un pequeño marcador de posición de forma interna para marcar su lugar en el árbol visual.

Puedes cargar o descargar mediante código o con una expresión [x:Bind](x-bind-markup-extension.md) el elemento de interfaz de usuario asignado con x:Load. Esto es útil para reducir los costes de los elementos que no se muestran a menudo o que se muestran de forma condicional. Al usar x:Load en un contenedor como Grid o StackPanel, se carga o descarga ese contenedor y todos sus elementos secundarios como un grupo.

El seguimiento que el marco XAML realiza a los elementos aplazados, agrega unos 600 bytes al uso de memoria por cada elemento asignado con x:Load, para tener en cuenta el marcador de posición. Por lo tanto, es posible usar en exceso este atributo, aunque el rendimiento disminuirá. Te recomendamos que solo lo uses en elementos que deban ocultarse. Si usas x:Load en un contenedor, la sobrecarga recaerá solo en el elemento que tenga el atributo x:Load.

> [!IMPORTANT]
> El atributo x: Load está disponible a partir de Windows 10, versión 1703 (Creator Update). Para poder usar x:Load, la versión mínima del proyecto de Visual Studio debe ser *Windows 10 Creators Update (10.0, compilación 15063)* .

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Carga de elementos

Existen varias maneras de cargar elementos:

- Usa una expresión [x:Bind](x-bind-markup-extension.md) para especificar el estado de carga. La expresión debe devolver el valor **true** para cargar el elemento y **false** para descargarlo.
- Llama al método [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) con el nombre definido en el elemento.
- Llama al método [**GetTemplateChild**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) con el nombre definido en el elemento.
- En la clase [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState), usa la animación [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) o **Storyboard** que tiene por destino el elemento x:Load.
- Establece como destino el elemento descargado en cualquier **Storyboard**.

> Nota: Una vez que se ha iniciado la creación de instancias de un elemento, se crea en el subproceso de interfaz de usuario, por lo que podría provocar que la interfaz de usuario parpadeen si demasiado que gran parte se crea al mismo tiempo.

Una vez creado el elemento diferido con cualquiera de los métodos enumerados anteriormente, sucederán varias cosas:

- Se generará el evento [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) en el elemento.
- Se establece el campo de x:Name.
- Todos los enlaces x:Bind del elemento se evaluarán.
- Si la aplicación se ha registrado para recibir notificaciones de cambios de propiedad en la propiedad que contiene los elementos diferidos, se mostrará una notificación.

## <a name="unloading-elements"></a>Descarga de elementos

Para descargar un elemento:

- Usa una expresión x:Bind para especificar el estado de carga. La expresión debe devolver el valor **true** para cargar el elemento y **false** para descargarlo.
- En un elemento Page o UserControl, llama a **UnloadObject** y pasa la referencia del objeto.
- Llama a **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** y pasa la referencia del objeto.

Cuando un objeto se descarga, se reemplazará en el árbol con un marcador de posición. La instancia del objeto permanecerá en la memoria hasta que se hayan publicado todas las referencias. La API UnloadObject de un elemento Page o UserControl, está diseñada para proporcionar referencias que contiene la generación de código para x:Name y x:Bind. Si tienes referencias adicionales en el código de la aplicación, estas también se proporcionarán.

Ten en cuenta que al descargar un elemento todos los estados asociados al mismo se descartarán; por lo tanto, si usas x:Load como una versión optimizada de Visibility, asegúrate de que se aplican todos los estados mediante enlaces o que el código los vuelve a aplicar al iniciar el evento Loaded.

## <a name="restrictions"></a>Restricciones

Las restricciones por usar **x:Load** son las siguientes:

- Debe definir un [x: Name](x-name-attribute.md) para el elemento, como debe ser una forma de encontrar el elemento más adelante.
- Solo puedes usar x:Load en tipos que deriven de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) o [**FlyoutBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- No puedes usar x:Load en elementos raíz de un control [**Page**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), [**UserControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol), o de una clase [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate).
- No puedes usar x:Load en elementos de [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- No puedes usar x:Load en XAML dinámico que se haya cargado con [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Si mueves un elemento primario se borrarán todos los elementos que no se hayan cargado.

## <a name="remarks"></a>Comentarios

Puedes usar x:Load en elementos anidados; sin embargo, deben llevarse a cabo desde el elemento más externo.  Si intentas crear un elemento secundario antes de crear el elemento primario, se producirá una excepción.

Por lo general, te recomendamos que aplaces aquellos elementos que no sean visibles en el primer fotograma. Una buena opción para encontrar candidatos a ser aplazados es buscar elementos que se vayan a crear con [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) contraída. Asimismo, la interfaz de usuario que se desencadena debido a la interacción del usuario es un buen lugar para buscar elementos que puedes aplazar.

Ten cuidado con el aplazamiento de elementos en escenarios [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), ya que se reducirá el tiempo de inicio; aunque también puede disminuir el rendimiento del movimiento panorámico en función de lo que vayas a crear. Si buscas aumentar el rendimiento del movimiento panorámico, consulta la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y del [atributo x:Phase](x-phase-attribute.md).

Si el [atributo x:Phase](x-phase-attribute.md) se usa en combinación con **x:Load**, a continuación, cuando se crea un elemento o un árbol de elementos, los enlaces se aplicarán hasta la fase actual incluida. La fase especificada para **x:Phase** afectará o controlará al estado de carga del elemento. Cuando un elemento de lista se recicla como parte del movimiento panorámico, los elementos creados se comportarán de la misma manera que otros elementos activos, y los enlaces compilados (enlaces **{x:Bind}** ) se procesarán mediante las mismas reglas, incluidos el ajuste de fase.

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


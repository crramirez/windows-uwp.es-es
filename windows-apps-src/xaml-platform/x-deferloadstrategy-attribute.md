---
author: jwmsft
title: Atributo xDeferLoadStrategy
description: El atributo xDeferLoadStrategy retrasa la creación de un elemento y sus elementos secundarios, lo que disminuye el tiempo de inicio aunque incrementa ligeramente el uso de memoria. Cada elemento afectado agrega alrededor de 600 bytes de uso de memoria.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
---

# Atributo x:DeferLoadStrategy

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**x:DeferLoadStrategy="Lazy"** retrasa la creación de un elemento y sus elementos secundarios, lo que disminuye el tiempo de inicio aunque incrementa ligeramente el uso de memoria. Cada elemento afectado agrega alrededor de 600 bytes de uso de memoria. Cuanto mayor sea del árbol de elementos aplazado, mayor será el ahorro de tiempo de inicio, aunque a cambio de un mayor superficie de memoria. Por lo tanto, es posible usar en exceso este atributo en la medida en que disminuye el rendimiento.

## Uso del atributo XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## Observaciones

Las restricciones de el de uso **x: DeferLoadStrategy** son:

-   Requiere definir el atributo [x:Name](x-name-attribute.md) , ya que debe haber una forma de encontrar el elemento más adelante.
-   Solo se puede marcar un elemento [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) como diferido, a excepción de los tipos derivados de la clase [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
-   No se pueden aplazar elementos raíz en las clases [**Page**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page), [**UserControls**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.usercontrol) y [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
-   Los elementos de la clase [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) no se puede aplazar.
-   No funciona con atributos XAML sueltos cargados con el método [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
-   Mover un elemento primario borrará todos los elementos que no se hayan realizado.

Existen varias maneras de obtener los elementos aplazados:

-   Llama al método [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) con el nombre definido en el elemento.
-   Llama al método [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) con el nombre definido en el elemento.
-   En la clase [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), usa la animación [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) o **Storyboard** que tiene por destino el elemento diferido.
-   Establece como destino el elemento diferido en cualquier **Storyboard**.
-   Usa un enlace que esté orientado al elemento diferido.
-   NOTA: una vez iniciada la creación de instancias de un elemento, se crea en el subproceso de la interfaz de usuario, lo que podría provocar problemas de estabilidad en la interfaz de usuario si se crean demasiados al mismo tiempo.

Una vez creado el elemento diferido con cualquiera de los métodos enumerados anteriormente, sucederán varias cosas:

-   Se generará el evento [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) en el elemento.
-   Todos los enlaces del elemento se evaluarán.
-   Si la aplicación se ha registrado para recibir notificaciones de cambios de propiedad en la propiedad que contiene los elementos diferidos, se mostrará una notificación.

Puedes anidar elementos diferidos; sin embargo, deben llevarse a cabo desde el elemento más externo.  Si intentas crear un elemento secundario antes de crear el elemento primario, se producirá una excepción.

En general, se recomienda aplazar elementos que no sean visibles en el primer fotograma.  Una buena opción para encontrar candidatos a ser aplazados es buscar elementos que se vayan a crear con [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) contraída.  La interfaz de usuario ocasional (es decir, la interfaz de usuario que se desencadena debido a la interacción del usuario) también es un buen lugar para buscar elementos aplazados.  

Ten cuidado con el aplazamiento de elementos en escenarios [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), ya que se reducirá el tiempo de inicio, aunque también puede disminuir el rendimiento del movimiento panorámico en función de lo que vayas a crear.  Si buscas aumentar el rendimiento del movimiento panorámico, consulta la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y del [atributo x:Phase](x-phase-attribute.md) .

Si el [atributo x:Phase](x-phase-attribute.md) se usa en combinación con **x:DeferLoadStrategy**, a continuación, cuando se realiza un elemento o un árbol de elementos, los enlaces se aplicarán hasta la fase actual incluida. La fase especificada para **x:Phase** no afectará ni controlará el aplazamiento del elemento. Cuando un elemento de lista se recicla como parte del movimiento panorámico, los elementos realizados se comportarán de la misma manera que otros elementos activos, y los enlaces compilados (enlaces **{x:Bind}**) se procesarán mediante las mismas reglas, incluidos el ajuste de fase.

Como regla general, se recomienda evaluar la aplicación antes y después para asegurarte de que vas a obtener el rendimiento deseado.

## Ejemplo

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```



<!--HONumber=May16_HO2-->



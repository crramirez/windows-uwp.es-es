---
title: Atributo xDeferLoadStrategy
description: El atributo xDeferLoadStrategy retrasa la creación de un elemento y sus elementos secundarios, lo que disminuye el tiempo de inicio aunque incrementa ligeramente el uso de memoria. Cada elemento afectado agrega alrededor de 600 bytes de uso de memoria.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f445b186c42095bfdf6d10fa7960b78691ced792
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371092"
---
# <a name="xdeferloadstrategy-attribute"></a>Atributo x:DeferLoadStrategy

> [!IMPORTANT]
> A partir de la versión 1703 de Windows 10 (Creators Update) **x:DeferLoadStrategy** ha sido reemplazado por el [**atributo x:Load**](x-load-attribute.md). Es igual usar `x:Load="False"` que `x:DeferLoadStrategy="Lazy"`, pero aquí se proporciona la capacidad de descargar la interfaz de usuario si fuera necesario. Para obtener más información, consulta el [atributo x:Load](x-load-attribute.md).

Puedes usar **x:DeferLoadStrategy="Lazy"** para optimizar el inicio o el rendimiento de la creación de árboles de la aplicación XAML. Al usar **x:DeferLoadStrategy="Lazy"** , se retrasa la creación de un elemento y sus elementos secundarios; debido a ello, se reduce el tiempo de inicio y el costo de la memoria. Esto es útil para reducir los costes de los elementos que no se muestran a menudo o que se muestran de forma condicional. El elemento se ejecutará en el momento en que se le haga referencia desde el código o desde VisualStateManager.

Sin embargo, el seguimiento de los elementos aplazados por el marco XAML agrega alrededor de 600 bytes al uso de memoria de cada elemento afectado. Cuanto mayor sea del árbol de elementos aplazado, mayor será el ahorro de tiempo de inicio, aunque a cambio de un mayor superficie de memoria. Por lo tanto, es posible usar en exceso este atributo, aunque el rendimiento disminuirá.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Comentarios

Las restricciones de el de uso **x: DeferLoadStrategy** son:

- Debe definir un [x: Name](x-name-attribute.md) para el elemento, como debe ser una forma de encontrar el elemento más adelante.
- Solo puedes postergar los tipos que se derivan de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) o [**FlyoutBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- No se pueden postergar los elementos raíz en las clases [**Page**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), [**UserControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) o [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate).
- No puedes postergar los elementos de la clase [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) no.
- No puedes postergar XAML dinámicos que se hayan cargado con [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Mover un elemento primario borrará todos los elementos que no se hayan realizado.

Existen varias maneras de obtener los elementos aplazados:

- Llama al método [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) con el nombre definido en el elemento.
- Llama al método [**GetTemplateChild**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) con el nombre definido en el elemento.
- En la clase [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState), usa la animación [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) o **Storyboard** que tiene por destino el elemento diferido.
- Establece como destino el elemento diferido en cualquier **Storyboard**.
- Usa un enlace que esté orientado al elemento diferido.

> Nota: Una vez que se ha iniciado la creación de instancias de un elemento, se crea en el subproceso de interfaz de usuario, por lo que podría provocar que la interfaz de usuario parpadeen si demasiado que gran parte se crea al mismo tiempo.

Una vez creado el elemento diferido con cualquiera de los métodos enumerados anteriormente, sucederán varias cosas:

- Se generará el evento [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) en el elemento.
- Todos los enlaces del elemento se evaluarán.
- Si la aplicación se ha registrado para recibir notificaciones de cambios de propiedad en la propiedad que contiene los elementos diferidos, se mostrará una notificación.

Puedes anidar elementos diferidos; sin embargo, deben llevarse a cabo desde el elemento más externo.  Si intentas crear un elemento secundario antes de crear el elemento primario, se producirá una excepción.

Por lo general, te recomendamos que aplaces aquellos elementos que no sean visibles en el primer fotograma. Una buena opción para encontrar candidatos a ser aplazados es buscar elementos que se vayan a crear con [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) contraída. Asimismo, la interfaz de usuario que se desencadena debido a la interacción del usuario es un buen lugar para buscar elementos que puedes aplazar.

Ten cuidado con el aplazamiento de elementos en escenarios [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), ya que se reducirá el tiempo de inicio; aunque también puede disminuir el rendimiento del movimiento panorámico en función de lo que vayas a crear. Si buscas aumentar el rendimiento del movimiento panorámico, consulta la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y del [atributo x:Phase](x-phase-attribute.md).

Si el [atributo x:Phase](x-phase-attribute.md) se usa en combinación con **x:DeferLoadStrategy**, a continuación, cuando se crea un elemento o un árbol de elementos, los enlaces se aplicarán hasta la fase actual incluida. La fase especificada para **x:Phase** no afectará ni controlará el aplazamiento del elemento. Cuando un elemento de lista se recicla como parte del movimiento panorámico, los elementos realizados se comportarán de la misma manera que otros elementos activos, y los enlaces compilados (enlaces **{x:Bind}** ) se procesarán mediante las mismas reglas, incluidos el ajuste de fase.

Como regla general, se recomienda evaluar el rendimiento de la aplicación antes y después, para asegurarte de que vas a obtener el rendimiento deseado.

## <a name="example"></a>Ejemplo

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
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```

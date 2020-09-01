---
title: Atributo xDeferLoadStrategy
description: El atributo xDeferLoadStrategy retrasa la creación de un elemento y sus elementos secundarios, lo que disminuye el tiempo de inicio aunque incrementa ligeramente el uso de memoria.Cada elemento afectado agrega alrededor de 600 bytes de uso de memoria.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f37c04af132e742a54df8e88e5c875a32fa94beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173779"
---
# <a name="xdeferloadstrategy-attribute"></a>Atributo x:DeferLoadStrategy

> [!IMPORTANT]
> A partir de Windows 10, versión 1703 (Creators Update), **x:DeferLoadStrategy** se sustituye por el [**atributo x:Load**](x-load-attribute.md). El uso de `x:Load="False"` es equivalente a `x:DeferLoadStrategy="Lazy"` , pero proporciona la capacidad de descargar la interfaz de usuario si es necesario. Vea el [atributo x:Load](x-load-attribute.md) para obtener más información.

Puede usar **x:DeferLoadStrategy = "Lazy"** para optimizar el rendimiento de la creación de árboles o el inicio de la aplicación XAML. Cuando se usa **x:DeferLoadStrategy = "Lazy"**, se retrasa la creación de un elemento y sus elementos secundarios, lo que reduce el tiempo de inicio y los costos de memoria. Esto resulta útil para reducir los costos de los elementos que se muestran con poca frecuencia o de manera condicional. El elemento se realizará cuando se hace referencia a él desde código o VisualStateManager.

Sin embargo, el seguimiento de los elementos diferidos por el marco XAML agrega aproximadamente 600 bytes al uso de memoria para cada elemento afectado. Cuanto mayor sea del árbol de elementos aplazado, mayor será el ahorro de tiempo de inicio, aunque a cambio de un mayor superficie de memoria. Por lo tanto, es posible utilizar este atributo en la medida en que disminuya el rendimiento.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Observaciones

Las restricciones de el de uso **x: DeferLoadStrategy** son:

- Debe definir un [x:Name](x-name-attribute.md)   para el elemento, ya que debe haber una manera de encontrar el elemento más adelante.
- Solo se pueden aplazar los tipos que derivan de [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) o [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- No se pueden aplazar los elementos raíz de una página, un [**control**](/uwp/api/windows.ui.xaml.controls.page) [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)o un [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate).
- No se pueden aplazar elementos en un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- No se puede diferir el código XAML dinámico cargado con [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Mover un elemento primario borrará todos los elementos que no se hayan realizado.

Existen varias maneras de obtener los elementos aplazados:

- Llame a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) con el nombre que ha definido en el elemento.
- Llame a [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) con el nombre que ha definido en el elemento.
- En un [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), use un [**establecedor**](/uwp/api/Windows.UI.Xaml.Setter) o una animación de **guion gráfico** que tenga como destino el elemento aplazado.
- Establece como destino el elemento diferido en cualquier **Storyboard**.
- Use un enlace que tenga como destino el elemento diferido.

> NOTA: una vez iniciada la creación de instancias de un elemento, se crea en el subproceso de la interfaz de usuario, lo que podría provocar problemas de estabilidad en la interfaz de usuario si se crean demasiados al mismo tiempo.

Una vez que se crea un elemento diferido en cualquiera de las formas enumeradas anteriormente, suceden varias cosas:

- Se genera el evento [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) en el elemento.
- Se evalúa cualquier enlace del elemento.
- Si se ha registrado para recibir notificaciones de cambio de propiedad en la propiedad que contiene los elementos aplazados, se genera la notificación.

Puedes anidar elementos diferidos; sin embargo, deben llevarse a cabo desde el elemento más externo. Si intenta obtener un elemento secundario antes de que se haya realizado el elemento primario, se produce una excepción.

Normalmente, se recomienda aplazar los elementos que no se pueden ver en el primer fotograma.Una buena opción para encontrar candidatos a ser aplazados es buscar elementos que se vayan a crear con [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) contraída. Además, la interfaz de usuario desencadenada por la interacción con el usuario es un buen lugar para buscar los elementos que se pueden aplazar.

Tenga cuidado al aplazar elementos en un [**control ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView), ya que disminuirá el tiempo de inicio, pero también puede reducir el rendimiento de la panorámica en función de lo que esté creando. Si desea aumentar el rendimiento de la panorámica, consulte la documentación de la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) y el [atributo x:Phase](x-phase-attribute.md) .

Si el [atributo x:Phase](x-phase-attribute.md) se usa junto con **x:DeferLoadStrategy** , cuando se realiza un elemento o un árbol de elementos, los enlaces se aplican hasta la fase actual, inclusive. La fase especificada para **x:Phase** no afecta ni controla el aplazamiento del elemento. Cuando un elemento de lista se recicla como parte de la panorámica, los elementos realizados se comportan de la misma manera que otros elementos activos y los enlaces compilados (**{x:Bind}** bindings) se procesan con las mismas reglas, incluido el escalonamiento.

Una directriz general es medir el rendimiento de la aplicación antes y después para asegurarse de que obtiene el rendimiento que desea.

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
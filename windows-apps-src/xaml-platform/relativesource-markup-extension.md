---
description: Proporciona un medio para especificar el origen de un enlace en términos de una relación relativa en el gráfico de objetos en tiempo de ejecución.
title: Extensión de marcado RelativeSource
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 716ca61fc9925846377157d215ca3326191915b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371146"
---
# <a name="relativesource-markup-extension"></a>Extensión de marcado {RelativeSource}


Proporciona un medio para especificar el origen de un enlace en términos de una relación relativa en el gráfico de objetos en tiempo de ejecución.

## <a name="xaml-attribute-usage-self-mode"></a>Uso del atributo XAML (modo Self)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>Uso del atributo XAML (modo TemplatedParent)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| {RelativeSource Self} | Produce un valor [<strong>Mode</strong>](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode) de <strong>Self</strong>. El elemento de destino debe usarse como origen para este enlace. Esto resulta útil para enlazar una propiedad de un elemento con otra propiedad del mismo elemento. |
| {RelativeSource TemplatedParent} | Produce una [<strong>ControlTemplate</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) que se aplica como el origen de este enlace. Esto resulta útil para aplicar la información en tiempo de ejecución a enlaces en el nivel de plantilla. | 

## <a name="remarks"></a>Comentarios

Un [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) puede establecer [**Binding.RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) como un atributo en un elemento de objeto **Binding** o como un componente en una [extensión de marcado {Binding}](binding-markup-extension.md). Por eso se muestran dos sintaxis de XAML diferentes.

**RelativeSource** es similar a [Extensión de marcado {Binding}](binding-markup-extension.md).  Se trata de una extensión de marcado capaz de devolver instancias de sí misma y que admite una construcción basada en cadenas que fundamentalmente pasa un argumento al constructor. En este caso, el argumento que se pasa es el valor [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode).

El modo **Self** es útil para enlazar una propiedad de un elemento con otra propiedad del mismo elemento y es una variación en el enlace de [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname), pero no requiere la definición de nombres y que luego el elemento haga referencia a sí mismo. Si vinculas una propiedad de un elemento a otra propiedad del mismo elemento, las propiedades deben usar el mismo tipo de propiedad, o debes usar también un [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) en el enlace para convertir los valores. Por ejemplo, podrías usar [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) como origen de [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sin conversión, pero necesitarías un convertidor para usar [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) como un origen de [**Visibility**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility).

A continuación te mostramos un ejemplo. Este [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) usa una [extensión de marcado {Binding}](binding-markup-extension.md) para que su [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) siempre sean iguales y se represente como un cuadrado. Solo Height se establece como un valor fijo. Para este **Rectangle**, su valor predeterminado [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) es **null**, no **this**. Por eso, para establecer que el origen del contexto de datos sea el propio objeto (y permitir el enlace con sus otras propiedades), empleamos el argumento `RelativeSource={RelativeSource Self}` en el uso de la extensión de marcado {Binding}.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Otro uso de `RelativeSource={RelativeSource Self}` es como una forma de establecer el [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) de un objeto en sí mismo.  Por ejemplo, puede ver esta técnica en algunos de los ejemplos SDK donde el [ **página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) se ha ampliado la clase con una propiedad personalizada que ya proporciona un modelo de vista de lista para usar su propio enlace de datos Por ejemplo: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Tenga en cuenta**  uso el XAML para **RelativeSource** muestra sólo el uso que se dirige: establecer un valor para [ **Binding.RelativeSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)en XAML como parte de una expresión de enlace. Teóricamente, otros usos son posibles si se configura una propiedad donde el valor sea [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource).

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [Extensión de marcado {binding}](binding-markup-extension.md)
* [**enlace**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource)


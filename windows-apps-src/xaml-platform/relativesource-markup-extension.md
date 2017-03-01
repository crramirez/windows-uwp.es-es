---
author: jwmsft
description: "Proporciona un medio para especificar el origen de un enlace en términos de una relación relativa en el gráfico de objetos en tiempo de ejecución."
title: "Extensión de marcado RelativeSource"
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 46b48e8e1ef1efbff7248ddf54c22e5a8bc29deb
ms.lasthandoff: 02/07/2017

---

# <a name="relativesource-markup-extension"></a>Extensión de marcado {RelativeSource}

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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
| {RelativeSource Self} | Produce un valor [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) de <strong>Self</strong>. El elemento de destino debe usarse como origen para este enlace. Esto resulta útil para enlazar una propiedad de un elemento con otra propiedad del mismo elemento. |
| {RelativeSource TemplatedParent} | Produce una [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) que se aplica como el origen de este enlace. Esto resulta útil para aplicar la información en tiempo de ejecución a enlaces en el nivel de plantilla. | 

## <a name="remarks"></a>Observaciones

Un [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) puede establecer [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) como un atributo en un elemento de objeto **Binding** o como un componente en una [extensión de marcado {Binding}](binding-markup-extension.md). Por eso se muestran dos sintaxis de XAML diferentes.

**RelativeSource** es similar a [Extensión de marcado {Binding}](binding-markup-extension.md).  Se trata de una extensión de marcado capaz de devolver instancias de sí misma y que admite una construcción basada en cadenas que fundamentalmente pasa un argumento al constructor. En este caso, el argumento que se pasa es el valor [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915).

El modo **Self** es útil para enlazar una propiedad de un elemento con otra propiedad del mismo elemento y es una variación en el enlace de [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), pero no requiere la definición de nombres y que luego el elemento haga referencia a sí mismo. Si vinculas una propiedad de un elemento a otra propiedad del mismo elemento, las propiedades deben usar el mismo tipo de propiedad, o debes usar también un [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) en el enlace para convertir los valores. Por ejemplo, podrías usar [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) como origen de [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) sin conversión, pero necesitarías un convertidor para usar [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) como un origen de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

A continuación te mostramos un ejemplo. Este [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) usa una [extensión de marcado {Binding}](binding-markup-extension.md) para que su [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) y [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) siempre sean iguales y se represente como un cuadrado. Solo Height se establece como un valor fijo. Para este **Rectangle**, su valor predeterminado [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) es **null**, no **this**. Por eso, para establecer que el origen del contexto de datos sea el propio objeto (y permitir el enlace con sus otras propiedades), empleamos el argumento `RelativeSource={RelativeSource Self}` en el uso de la extensión de marcado {Binding}.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Otro uso de `RelativeSource={RelativeSource Self}` es como una forma de establecer el [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) de un objeto en sí mismo.  Por ejemplo, esta técnica puede verse en algunos ejemplos del SDK, en los que la clase [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) se ha extendido con una propiedad personalizada que ya proporciona un modelo de vista listo para el enlace de sus propios datos, como, por ejemplo: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Nota**  El uso de XAML para **RelativeSource** solo muestra el uso para el que fue creado: establecer un valor para [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) en XAML como parte de una expresión de enlace. Teóricamente, otros usos son posibles si se configura una propiedad donde el valor sea [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Extensión de marcado {Binding}](binding-markup-extension.md)
* [**Enlace**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)



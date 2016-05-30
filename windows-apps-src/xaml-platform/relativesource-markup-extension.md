---
author: jwmsft
description: Proporciona un medio para especificar el origen de un enlace en términos de una relación relativa en el gráfico de objetos en tiempo de ejecución.
title: Extensión de marcado RelativeSource
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
---

# Extensión de marcado {RelativeSource}

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Proporciona un medio para especificar el origen de un enlace en términos de una relación relativa en el gráfico de objetos en tiempo de ejecución.

## Uso del atributo XAML (modo Self)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## Uso del atributo XAML (modo TemplatedParent)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## Valores de XAML

| Término | Descripción | | {RelativeSource Self} | Produce un valor [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) igual a <strong>Self</strong>. El elemento de destino debe usarse como origen para este enlace. Esto resulta útil para vincular una propiedad de un elemento a otra propiedad del mismo elemento. | | {RelativeSource TemplatedParent} | Produce una [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) que se aplica como el origen de este enlace. Esto resulta útil para aplicar la información en tiempo de ejecución a enlaces en el nivel de plantilla. | 

## Observaciones

Un [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) puede establecer [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) como un atributo en un elemento de objeto **Binding** o como un componente en una [extensión de marcado {Binding}](binding-markup-extension.md). Es por ello que se muestran dos sintaxis XAML diferentes.

**RelativeSource** es similar a la [extensión de marcado {Binding}](binding-markup-extension.md) en que se trata de una extensión de marcado capaz de devolver instancias de sí misma, y admite una construcción basada en cadenas que fundamentalmente pasa un argumento al constructor. En este caso, el argumento que se pasa es el valor [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915).

El modo **Self** resulta útil para casos en los que se debe usar el mismo elemento como objeto de origen y objeto de destino para un enlace, pero el origen y el destino son propiedades diferentes. Esto sirve para enlazar una propiedad de un elemento a otra propiedad del mismo elemento, y es una variación en el enlace de [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) que no requiere asignación de nombres y hace que el elemento haga referencia a sí mismo. Si vinculas una propiedad de un elemento a otra propiedad del mismo elemento, las propiedades deben usar el mismo tipo de propiedad, o debes usar también un [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) en el enlace para convertir los valores. Por ejemplo, podrías usar [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) como origen de [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) sin conversión, pero necesitarías un convertidor para usar [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) como un origen de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

A continuación te mostramos un ejemplo. Este [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) usa una [extensión de marcado {Binding}](binding-markup-extension.md) para que su [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) y [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) siempre sean iguales y se represente como un cuadrado. Solo Height se establece como un valor fijo. Para este **Rectangle**, su valor predeterminado [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) es **null**, no **this**. Por eso, para establecer que el origen del contexto de datos sea el propio objeto (y permitir el enlace a sus otras propiedades), usamos el argumento `RelativeSource={RelativeSource Self}` en el uso de la extensión de marcado {Binding}.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Otra técnica que puede resultar útil es usar `RelativeSource={RelativeSource Self}` como una manera de establecer el [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) de un objeto en sí mismo; la clase [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) se ha extendido con una propiedad personalizada que ya proporciona un modelo de vista listo para el enlace a sus propios datos. Esta técnica se ve en algunos ejemplos del SDK: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Nota** El uso de XAML para **RelativeSource** solo muestra el uso para el cual fue creado: configurar un valor para [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) en XAML como parte de una expresión de enlace. Teóricamente, otros usos son posibles si se configura una propiedad donde el valor sea [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Extensión de marcado {Binding}](binding-markup-extension.md)
* [**Enlace**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)



<!--HONumber=May16_HO2-->



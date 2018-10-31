---
author: jwmsft
description: Proporciona un valor para cualquier atributo XAML mediante la evaluación de una referencia a un recurso ya definido. Los recursos se definen en un ResourceDictionary, y el uso de StaticResource hace referencia a la clave de ese recurso en el ResourceDictionary.
title: Extensión de marcado StaticResource
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 83919cc46694279bc35e046c97acf27c64a196f5
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5836470"
---
# <a name="staticresource-markup-extension"></a>Extensión de marcado {StaticResource}


Proporciona un valor para cualquier atributo XAML mediante la evaluación de una referencia a un recurso ya definido. Los recursos se definen en un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) y el uso de **StaticResource** hace referencia a la clave de ese recurso en el **ResourceDictionary**.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| key | La clave del recurso solicitado. Esta clave se asigna inicialmente por [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Una clave de recurso puede ser cualquier cadena que se defina con la gramática XamlName. |

## <a name="remarks"></a>Observaciones

**StaticResource** es una técnica para obtener valores para un atributo XAML que se definen en otra parte de un diccionario de recursos XAML. Los valores podrían colocarse en un diccionario de recursos porque están destinados a ser compartidos por varios valores de propiedad, o porque se usa un diccionario de recursos XAML como técnica de empaquetado o factorización de XAML. Un ejemplo de una técnica de empaquetado de XAML es el diccionario de temas de un control. Otro ejemplo son los diccionarios de recursos combinados que se usan para la reserva de recursos.

**StaticResource** toma un argumento, que especifica la clave del recurso solicitado. Una clave de recursos es siempre una cadena en XAML de Windows Runtime. Para obtener más información acerca de cómo especificar inicialmente la clave de recurso, consulta [Atributo x:Key](x-key-attribute.md).

Las reglas por las que un **StaticResource** resuelve un elemento en un diccionario de recursos no se describen en este tema. Eso depende de si la referencia y el recurso existen en una plantilla, de si se usan diccionarios de recursos combinados, etc. Para obtener más información sobre cómo definir recursos y usar correctamente un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), incluida una muestra de código, consulta [Referencias a ResourceDictionary y a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

**Importante**  **StaticResource** no debe intentar hacer referencia adelantada a un recurso que se define léxicamente dentro del archivo XAML. Este intento no se admite. Aunque la referencia adelantada no genere un error, intentar llevarla a cabo conlleva una penalización de rendimiento. Para obtener los mejores resultados, ajusta la composición de tus diccionarios de recursos de manera que se eviten las referencias adelantadas.

Intentar especificar un **StaticResource** en una clave que no puede resolverse inicia una excepción de análisis XAML en tiempo de ejecución. Las herramientas de diseño también pueden ofrecer advertencias o errores.

En la implementación del procesador XAML de Windows Runtime no hay una representación de clase de respaldo para la funcionalidad de **StaticResource**. **StaticResource** se usa exclusivamente en XAML. El equivalente más parecido en el código consiste en usar la API de colección de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), por ejemplo, una llamada a [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) o [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139).

La [extensión de marcado {ThemeResource}](themeresource-markup-extension.md) es una extensión de marcado similar que hace referencia a recursos con nombre en otra ubicación. La diferencia es que la extensión de marcado {ThemeResource} puede devolver diferentes recursos según el tema del sistema que esté activo. Para obtener más información, consulta [Extensión de marcado {ThemeResource}](themeresource-markup-extension.md).

**StaticResource** es una extensión de marcado. Las extensiones de marcado generalmente se implementan cuando es necesario que los valores de atributo de escape no sean valores literales o nombres de controlador y el requisito sea más global que simplemente colocar convertidores de tipos en ciertos tipos o propiedades. Todas las extensiones de marcado en XAML usan los caracteres "\{" y "\}" en su sintaxis de atributo, que es la convención mediante la cual un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

### <a name="an-example-staticresource-usage"></a>Ejemplo de uso de {StaticResource}

Este ejemplo de XAML se ha extraído de la [Muestra de enlace de datos XAML](http://go.microsoft.com/fwlink/p/?linkid=226854).

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

Este ejemplo concreto crea un objeto respaldado por una clase personalizada y lo crea como un recurso en un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Para que sea un recurso válido, este elemento `local:S2Formatter` también debe tener un valor de atributo **x:Key**. El valor del atributo se establece en "GradeConverter".

A continuación, se solicita el recurso en un nivel más profundo del XAML, donde puedes ver `{StaticResource GradeConverter}`.

Observa cómo el uso de la extensión de marcado {StaticResource} establece una propiedad de otra extensión de marcado [extensión de marcado {Binding](binding-markup-extension.md), de modo que hay dos usos de extensiones de marcado anidadas. La interior se evalúa primero, de tal forma que el recurso se obtiene en primer lugar y puede usarse como un valor. Este mismo ejemplo se muestra en la extensión de marcado {Binding}.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>Compatibilidad con herramientas en tiempo de diseño para la extensión de marcado **{StaticResource}**

Microsoft Visual Studio2013 puede incluir posibles valores de clave en los menús desplegables de Microsoft IntelliSense cuando uses la extensión de marcado **{StaticResource}** en una página XAML. Por ejemplo, cuando escribes "{StaticResource", aparece cualquiera de las claves de recurso del ámbito de la consulta actual en los menús desplegables de IntelliSense. Además de los recursos típicos que tendrías en el nivel de página ([**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)) y el nivel de aplicación ([**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338)), también verás [recursos de tema XAML](https://msdn.microsoft.com/library/windows/apps/mt187274) y recursos de cualquier extensión que use el proyecto.

Cuando exista una clave de recurso como parte del uso de cualquier **{StaticResource}**, la característica **Ir a definición** (F12) puede resolver ese recurso y mostrar el diccionario en el que se define. Para los recursos de tema, se dirige a generic.xaml para el tiempo de diseño.

## <a name="related-topics"></a>Temas relacionados

* [Referencias a ResourceDictionary y a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [Atributo x:Key](x-key-attribute.md)
* [Extensión de marcado {ThemeResource}](themeresource-markup-extension.md)


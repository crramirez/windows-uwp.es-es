---
author: jwmsft
description: "Vincula el valor de una propiedad en una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. TemplateBinding solo se puede usar dentro de una definición de ControlTemplate en XAML."
title: "Extensión de marcado TemplateBinding"
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 1a8006e58391c41568731810d9b1901474e8d18f

---

# Extensión de marcado {TemplateBinding}

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Vincula el valor de una propiedad en una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. **TemplateBinding** solo se puede usar dentro de una definición de [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) en XAML.

## Uso del atributo XAML

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## Uso del atributo XAML (para la propiedad Setter en la plantilla o estilo)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## Valores de XAML

| Término | Descripción |
|------|-------------|
| propertyName | El nombre de la propiedad que se establece en la sintaxis de setter. Debe ser una propiedad de dependencia. |
| sourceProperty | El nombre de otra propiedad de dependencia que existe en el tipo al que se aplica la plantilla. |

## Observaciones

El uso de **TemplateBinding** es una parte fundamental de la definición de una plantilla de control, tanto si eres un autor de control personalizado como si reemplazas una plantilla de control para los controles existentes. Para obtener más información, consulta [Inicio rápido: plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

Es bastante común que *propertyName* y *targetProperty* usen el mismo nombre de propiedad. En este caso, un control podría definir una propiedad en sí misma y reenviar la propiedad a una propiedad existente y con un nombre asignado intuitivamente de una de sus partes componentes. Por ejemplo, un control que incorpore un objeto [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) en su composición, que se usa para mostrar la propiedad **Texto** del control, podría incluir este código XAML como parte de la plantilla del control: `<TextBlock Text="{TemplateBinding Text}" .... />`

Los tipos usados como valor para la propiedad de origen y la propiedad de destino deben coincidir. No existe la posibilidad de introducir un convertidor al usar **TemplateBinding**. Si no coinciden los valores, se produce un error al analizar el XAML. Si necesitas un convertidor, puedes usar la sintaxis detallada para un enlace de plantillas como: `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Al intentar usar un **TemplateBinding** fuera de una definición de [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) en XAML se generará un error del analizador.

Puedes usar **TemplateBinding** en casos en los que el valor principal de la plantilla también se aplace como otro enlace. La evaluación de **TemplateBinding** no puede esperar que algún enlace necesario en tiempo de ejecución tenga valores.

Un **TemplateBinding** siempre es un enlace unidireccional. Ambas propiedades implicadas deben ser propiedades de dependencia.

**TemplateBinding** es una extensión de marcado. Las extensiones de marcado generalmente se implementan cuando es necesario que los valores de atributo de escape no sean valores literales o nombres de controlador y el requisito sea más global que simplemente colocar convertidores de tipos en ciertos tipos o propiedades. Todas las extensiones de marcado en XAML usan los caracteres "{" y "}" en su sintaxis de atributo, que es la convención mediante la cual un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

**Nota** En la implementación del procesador XAML de Windows Runtime no hay una representación de clase de respaldo para **TemplateBinding**. **TemplateBinding** se usa exclusivamente en marcado XAML. No hay una forma directa de reproducir el comportamiento en el código.

## Temas relacionados

* [Inicio rápido: Plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [Introducción a XAML](xaml-overview.md)
* [Introducción a las propiedades de dependencia](dependency-properties-overview.md)
 




<!--HONumber=Jun16_HO4-->



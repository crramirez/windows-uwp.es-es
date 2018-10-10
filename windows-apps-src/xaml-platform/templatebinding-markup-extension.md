---
author: jwmsft
description: Vincula el valor de una propiedad en una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. TemplateBinding solo se puede usar dentro de una definición de ControlTemplate en XAML.
title: Extensión de marcado TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c1812adc9d5610fffd6f9d275b4e093a4fa96e6
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "4501058"
---
# <a name="templatebinding-markup-extension"></a>Extensión de marcado {TemplateBinding}


Vincula el valor de una propiedad en una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. **TemplateBinding** solo se puede usar dentro de una definición de [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) en XAML.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>Uso del atributo XAML (para la propiedad Setter en la plantilla o estilo)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| propertyName | El nombre de la propiedad que se establece en la sintaxis de setter. Debe ser una propiedad de dependencia. |
| sourceProperty | El nombre de otra propiedad de dependencia que existe en el tipo al que se aplica la plantilla. |

## <a name="remarks"></a>Observaciones

El uso de **TemplateBinding** es una parte fundamental de la definición de una plantilla de control, tanto si eres un autor de control personalizado como si reemplazas una plantilla de control para los controles existentes. Para obtener más información, consulta [Inicio rápido: plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

Es bastante común que *propertyName* y *targetProperty* usen el mismo nombre de propiedad. En este caso, un control podría definir una propiedad en sí misma y reenviar la propiedad a una propiedad existente y con un nombre asignado intuitivamente de una de sus partes componentes. Por ejemplo, un control que incorpore un objeto [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) en su composición, que se usa para mostrar la propiedad **Texto** del control, podría incluir este código XAML como parte de la plantilla del control: `<TextBlock Text="{TemplateBinding Text}" .... />`

Los tipos usados como valor para la propiedad de origen y la propiedad de destino deben coincidir. No existe la posibilidad de introducir un convertidor al usar **TemplateBinding**. Si no coinciden los valores, se produce un error al analizar el XAML. Si necesitas un convertidor, puedes usar la sintaxis detallada para un enlace de plantillas como:  `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Al intentar usar un **TemplateBinding** fuera de una definición de [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) en XAML se generará un error del analizador.

Puedes usar **TemplateBinding** en casos en los que el valor principal de la plantilla también se aplace como otro enlace. La evaluación de **TemplateBinding** no puede esperar que algún enlace necesario en tiempo de ejecución tenga valores.

Un **TemplateBinding** siempre es un enlace unidireccional. Ambas propiedades implicadas deben ser propiedades de dependencia.

**TemplateBinding** es una extensión de marcado. Las extensiones de marcado generalmente se implementan cuando es necesario que los valores de atributo de escape no sean valores literales o nombres de controlador y el requisito sea más global que simplemente colocar convertidores de tipos en ciertos tipos o propiedades. Todas las extensiones de marcado en XAML usan los caracteres "{" y "}" en su sintaxis de atributo, que es la convención mediante la cual un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

**Nota** En la implementación del procesador XAML de Windows Runtime no hay una representación de clase de respaldo para **TemplateBinding**. **TemplateBinding** se usa exclusivamente en marcado XAML. No hay una forma directa de reproducir el comportamiento en el código.

### <a name="xbind-in-controltemplate"></a>x: Bind en ControlTemplate

A partir de la siguiente actualización importante a Windows 10, puedes usar extensión de marcado **x: Bind** en cualquier lugar que usaste **TemplateBinding** en [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). 

La propiedad [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype#Windows_UI_Xaml_Controls_ControlTemplate_TargetType) será necesario (no opcional) en [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) al usar **x: Bind**.

Con el soporte de **x: Bind** , ahora puedes usar ambos [enlaces de función](../data-binding/function-bindings.md) como enlaces bidireccionales bien como en [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)

En el siguiente ejemplo, el TextBlock.Text se evalúa como Button.Content.ToString(). TargetType en ControlTemplate actúa como el origen de datos y realiza el mismo resultado que TemplateBinding al elemento primario.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content}" />
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [Introducción a XAML](xaml-overview.md)
* [Introducción a las propiedades de dependencia](dependency-properties-overview.md)
 


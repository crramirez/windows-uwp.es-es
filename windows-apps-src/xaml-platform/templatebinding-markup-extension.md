---
description: Vincula el valor de una propiedad en una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. TemplateBinding solo se puede usar dentro de una definición de ControlTemplate en XAML.
title: Extensión de marcado TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e784b14c30222a28a0e10f8ba0bcf96e6c7f9afd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372315"
---
# <a name="templatebinding-markup-extension"></a>Extensión de marcado {TemplateBinding}

Vincula el valor de una propiedad en una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. **TemplateBinding** solo se puede usar dentro de una definición de [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) en XAML.

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

## <a name="remarks"></a>Comentarios

El uso de **TemplateBinding** es una parte fundamental de la definición de una plantilla de control, tanto si eres un autor de control personalizado como si reemplazas una plantilla de control para los controles existentes. Para obtener más información, consulte [inicio rápido: Plantillas de control](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

Es bastante común que *propertyName* y *targetProperty* usen el mismo nombre de propiedad. En este caso, un control podría definir una propiedad en sí misma y reenviar la propiedad a una propiedad existente y con un nombre asignado intuitivamente de una de sus partes componentes. Por ejemplo, un control que incorpore un [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) en su composición, que se usa para mostrar la propiedad **Texto**, podría incluir este código XAML como parte de la plantilla del control: `<TextBlock Text="{TemplateBinding Text}" .... />`

Los tipos usados como valor para la propiedad de origen y la propiedad de destino deben coincidir. No existe la posibilidad de introducir un convertidor al usar **TemplateBinding**. Si no coinciden los valores, se produce un error al analizar el XAML. Si necesitas un convertidor, puedes usar la sintaxis detallada para un enlace de plantillas como: `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Al intentar usar un **TemplateBinding** fuera de una definición de [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) en XAML se generará un error del analizador.

Puedes usar **TemplateBinding** en casos en los que el valor principal de la plantilla también se aplace como otro enlace. La evaluación de **TemplateBinding** no puede esperar que algún enlace necesario en tiempo de ejecución tenga valores.

Un **TemplateBinding** siempre es un enlace unidireccional. Ambas propiedades implicadas deben ser propiedades de dependencia.

**TemplateBinding** es una extensión de marcado. Las extensiones de marcado generalmente se implementan cuando es necesario que los valores de atributo de escape no sean valores literales o nombres de controlador y el requisito sea más global que simplemente colocar convertidores de tipos en ciertos tipos o propiedades. Todas las extensiones de marcado en XAML usan los caracteres "{" y "}" en su sintaxis de atributo, que es la convención mediante la cual un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

**Tenga en cuenta**  implementación del procesador en el XAML en tiempo de ejecución de Windows, no hay ninguna representación de clase de respaldo para **TemplateBinding**. **TemplateBinding** se usa exclusivamente en marcado XAML. No hay una forma directa de reproducir el comportamiento en el código.

### <a name="xbind-in-controltemplate"></a>x: Bind en ControlTemplate

> [!NOTE]
> Uso de x: Bind en un ControlTemplate requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o una versión posterior. Para obtener más información sobre las versiones de destino, consulta [Version adaptive code (Código adaptativo para versiones)](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

A partir de Windows 10, versión 1809, puede usar el **x: Bind** extensión de marcado en cualquier parte use **TemplateBinding** en un [ **ControlTemplate** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate). 

El [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) propiedad es necesaria (no es opcional) en [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) al usar **x: Bind**.

Con **x: Bind** admite, puede usar ambos [función enlaces](../data-binding/function-bindings.md) , así como los enlaces bidireccionales en un [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate).

En este ejemplo, el **TextBlock.Text** propiedad se evalúa en **Button.Content.ToString**. TargetType de ControlTemplate actúa como el origen de datos y lleva a cabo el mismo resultado que TemplateBinding al elemento primario.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Plantillas de control](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))
* [Enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [Introducción a XAML](xaml-overview.md)
* [Información general sobre las propiedades de dependencia](dependency-properties-overview.md)
 


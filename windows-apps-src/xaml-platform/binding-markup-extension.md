---
description: La extensión de marcado Binding se convierte en tiempo de carga XAML en una instancia de la clase Binding.
title: Extensión de marcado Binding
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6f11aae7d08f25e9dffaee12e24d1486cf9de581
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72998609"
---
# <a name="binding-markup-extension"></a>Extensión de marcado {Binding}


> [!NOTE]
> Hay un nuevo mecanismo de enlace disponible para Windows 10, que está optimizado para el rendimiento y la productividad de los desarrolladores. Consulta [Extensión de marcado {x:Bind}](x-bind-markup-extension.md).

> [!NOTE]
> Para obtener información general sobre el uso del enlace de datos en la aplicación con **{Binding}** (y para una comparación completa entre **{X:Bind}** y **{Binding}** ), consulte [enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

La extensión de marcado **{Binding}** se usa para enlazar las propiedades de los controles a los valores procedentes de un origen de datos como el código. En el tiempo de carga de XAML, la extensión de marcado **{Binding}** se convierte en una instancia de la clase [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Este objeto de enlace obtiene un valor de propiedad en un origen de datos y lo inserta en la propiedad que está en el control. Opcionalmente, el objeto de enlace puede configurarse para observar cambios en el valor de la propiedad de origen de datos y se actualiza en función de dichos cambios. Opcionalmente, también puede configurarse para insertar los cambios en el valor del control de nuevo en la propiedad de origen. La propiedad que es el destino de un enlace de datos debe ser una propiedad de dependencia. Para obtener más información, consulta [Introducción a las propiedades de dependencia](dependency-properties-overview.md).

**{Binding}** tiene la misma prioridad de propiedad de dependencia que un valor local, y definir un valor local en código imperativo quita el efecto de cualquier **{Binding}** definido en el marcado.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| Término | Descripción |
|------|-------------|
| *propertyPath* | Una cadena que especifica la ruta de acceso de la propiedad para el enlace. Para obtener más información, consulta la sección [Ruta de acceso de propiedades](#property-path) que aparece más adelante. |
| *bindingProperties* | *propname*=*valor*\[, *propName*=*valor*\]*<br/>Una o más propiedades de enlace que se especifican con una sintaxis de par de nombre-valor. |
| *propName* | El nombre de cadena de la propiedad que se establecerá en el objeto [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Por ejemplo, "Converter". |
| *value* | El valor en el que se establecerá la propiedad. La sintaxis del argumento depende de la propiedad de la sección [Propiedades de la clase Binding que se pueden establecer con {Binding}](#properties-of-the-binding-class-that-can-be-set-with-binding) que aparece más adelante. |

## <a name="property-path"></a>Ruta de acceso de propiedades

[**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) describe la propiedad a la que está enlazando (la propiedad Source). Path es un parámetro de posición, lo que significa que puedes usar el nombre del parámetro explícitamente (`{Binding Path=EmployeeID}`), o bien, puede especificarlo como el primer parámetro sin nombre (`{Binding EmployeeID}`).

El tipo de [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) es una ruta de acceso de propiedad, que es una cadena que se evalúa como una propiedad o una subpropiedad de su tipo personalizado o de un tipo de marco. El tipo puede ser (pero no tiene por qué serlo) un [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Los pasos de las rutas de acceso de propiedad están delimitados por puntos (.) y puedes incluir varios delimitadores para desviar subpropiedades sucesivas. Usa puntos como delimitadores independientemente del lenguaje de programación que uses para implementar el objeto al que se está enlazando.

Por ejemplo, para enlazar la interfaz de usuario a la propiedad del nombre de un objeto de un empleado, tu ruta de acceso de propiedad podría ser "Employee.FirstName". Si estás enlazando un control de elemento a una propiedad que contiene las personas a cargo del empleado, la ruta de acceso de la propiedad podría ser "Employee.Dependents" y la plantilla del elemento del control de elementos se ocuparía de mostrar los elementos en "Dependents".

Si el origen de datos es una colección, una ruta de acceso de propiedad puede especificar los elementos de la colección por su posición o índice. Por ejemplo, "Teams\[0\]. Reproductores ", donde el literal"\[\]"incluye el" 0 "que especifica el primer elemento de una colección.

Al usar un enlace [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) para un [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) existente, puedes usar propiedades adjuntas como parte de la ruta de acceso de la propiedad. Para desambiguar una propiedad adjunta de manera que el punto intermedio en el nombre de la propiedad adjunta no se considere un paso dentro de la ruta de acceso de la propiedad, coloca paréntesis alrededor del nombre de la propiedad adjunta calificado por el propietario; por ejemplo, `(AutomationProperties.Name)`.

Un objeto intermedio de ruta de acceso de propiedad se almacena como un objeto [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) en una representación en tiempo de ejecución, pero en la mayoría de escenarios no es necesario interactuar con un objeto **PropertyPath** en el código. Normalmente puedes especificar la información de enlace necesaria mediante XAML.

Para obtener más información sobre la sintaxis de cadenas de una ruta de acceso de propiedades, rutas de acceso de propiedades en áreas de características de animaciones y la construcción de un objeto [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath), consulta [Sintaxis de Property-path](property-path-syntax.md).

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>Propiedades de la clase de enlace que se pueden establecer con {Binding}


**{Binding}** se explica a través de la sintaxis de marcadores de posición *bindingProperties* porque hay muchas propiedades de lectura y escritura de [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) que se pueden definir en la extensión de marcado. Las propiedades pueden establecerse en cualquier orden con pares separados por comas *propName*=*value*. Algunas de las propiedades requieren tipos que no tengan una conversión de tipos, por lo que requieren sus propias extensiones de marcado anidadas dentro de **{Binding}** .

| Propiedad | Descripción |
|----------|-------------|
| [**Camino**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) | Consulta la sección [Ruta de acceso de propiedades](#property-path) anterior. |
| [**Convertidor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) | Especifica un objeto convertidor al que el motor de enlace llama. El convertidor puede establecerse en el marcado mediante la [extensión de marcado {StaticResource}](staticresource-markup-extension.md) para hacer referencia a ese objeto desde un diccionario de recursos. |
| [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) | Especifica la referencia cultural que usará el convertidor. (Si estableces [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)). La cultura se establece como un identificador basado en estándares. Para obtener más información, consulta [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) | Especifica un parámetro del convertidor que se puede usar en la lógica del convertidor. (Si estableces [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)). La mayoría de convertidores usan una lógica simple que obtiene toda la información que necesitan del valor pasado para convertir y no necesitan ningún valor de **ConverterParameter**. El parámetro **ConverterParameter** es para las implementaciones de convertidor más complejas, que disponen de una lógica condicional que controla lo que se pasa en **ConverterParameter**. Puedes escribir un convertidor que use valores que no sean cadenas, aunque no es lo habitual. Consulta las observaciones de **ConverterParameter** para obtener más información. |
| [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) | Especifica un origen de datos haciendo referencia a otro elemento en la misma construcción XAML que tenga la propiedad **Name** o el [atributo x:Name](x-name-attribute.md). A menudo, se usa para compartir valores relacionados o usar subpropiedades de un elemento de interfaz de usuario para proporcionar un valor específico para otro elemento, por ejemplo, en una plantilla de control XAML. |
| [**FallbackValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) | Especifica un valor que se mostrará cuando no se puede resolver el origen o la ruta de acceso. |
| [**Modo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) | Especifica el modo de enlace como uno de los siguientes valores: "OneTime", "OneWay" o "TwoWay". Estos se corresponden con los nombres de constante de la enumeración de [**BindingMode**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingMode). El valor predeterminado es "OneWay". Ten en cuenta que esto difiere del valor predeterminado de **{x:Bind}** , que es "OneTime". | 
| [**RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) | Para especificar un origen de datos, describe la posición del origen del enlace con relación a la posición del destino de enlace. Esto suele usarse en los enlaces dentro de plantillas de control XAML, estableciendo la [extensión de marcado {RelativeSource}](relativesource-markup-extension.md). |
| [**Fuentes**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) | Especifica el origen de datos del objeto. Dentro de la extensión de marcado **Binding**, la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) requiere una referencia de objeto, como una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md). Si no se especifica esta propiedad, el contexto de datos que actúa especifica el origen. Es más habitual no especificar un valor Source en enlaces individuales y, en lugar de ello, basarse en la propiedad **DataContext** compartida para enlaces múltiples. Para más información, consulta [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) o [Enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth). |
| [**TargetNullValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.targetnullvalue) | Especifica un valor que se mostrará cuando se resuelva el valor de origen, pero es explícitamente **null**. |
| [**UpdateSourceTrigger**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.updatesourcetrigger) | Especifica el tiempo de las actualizaciones de origen de enlace. Si no se especifica, el valor predeterminado es **Default**. |

**Nota**  si está convirtiendo el marcado de **{X:Bind}** a **{Binding}** , tenga en cuenta las diferencias en los valores predeterminados de la propiedad **mode** .

[**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) y **ConverterLanguage** están relacionados con el escenario de conversión de un valor o tipo del origen de enlace en un tipo o valor que es compatible con la propiedad de destino de enlace. Para obtener más información y ejemplos, consulta la sección “Conversiones de datos” de [Enlaces de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

> [!NOTE]
> A partir de la versión 1607 de Windows 10, el marco XAML proporciona un valor booleano integrado para el convertidor Visibility. El convertidor asigna **true** al valor de la enumeración **Visible** y **false** a **Collapsed**, para que puedas enlazar una propiedad Visibility a un valor booleano sin necesidad de crear un convertidor. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No puedes usarlo si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Código adaptativo para versiones](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

[**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source), [**RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)y [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) especifican un origen de enlace, por lo que son mutuamente excluyentes.

**Sugerencia**  si tiene que especificar una sola llave para un valor, como en [**path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) o [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), debe ir precedida de una barra diagonal inversa: `\{`. Como alternativa, escribe la cadena completa que contiene las llaves que necesitan escape entre un conjunto de comillas secundario, por ejemplo, `ConverterParameter='{Mix}'`.

## <a name="examples"></a>Ejemplos

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

El segundo ejemplo establece cuatro propiedades de [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) distintas: [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname), [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path), [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) y [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter). **Path** en este caso se muestra explícitamente como una propiedad de **Binding**. **Path** se evalúa en un origen de enlace de datos que es otro objeto del mismo árbol de objetos en tiempo de ejecución, un [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) denominado `sliderValueConverter`.

Observa que el valor de la propiedad [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) usa otra extensión de marcado, [extensión de marcado {StaticResource}](staticresource-markup-extension.md), de forma que hay dos usos de extensiones de marcado anidadas. La interior se evalúa primero de tal forma que, una vez obtenido el recurso, hay un [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) práctico (una clase personalizada de la cual el elemento `local:S2Formatter` de los recursos crea una instancia) que puede usarse en el enlace.

## <a name="tools-support"></a>Compatibilidad con herramientas

Microsoft IntelliSense en Microsoft Visual Studio muestra las propiedades del contexto de datos durante la creación de **{Binding}** en el editor de marcado XAML. Tan pronto como escribes "{Binding", se muestran las propiedades de contexto de datos adecuadas para [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) en la lista desplegable. IntelliSense también ayuda con las otras propiedades de [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Para que funcione, debes tener el contexto de datos o el contexto de datos en tiempo de diseño establecido en la página de marcado. **Ir a definición** (F12) también funciona con **{Binding}** . Como alternativa, puedes usar el cuadro de diálogo de enlace de datos.

 

---
author: jwmsft
description: "La extensión de marcado Binding se convierte en tiempo de carga XAML en una instancia de la clase Binding."
title: "Extensión de marcado Binding"
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: 740110809845220d919c6ba3c90b1393dbc8ae94

---

# Extensión de marcado {Binding}

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Nota**  Un nuevo mecanismo de enlace está disponible para Windows 10. Está optimizado para el rendimiento y la productividad del desarrollador. Consulta [Extensión de marcado {x:Bind}](x-bind-markup-extension.md).

**Nota**  Para obtener información general sobre el uso de enlace de datos en la aplicación con **{Binding}** (y para realizar una comparación total entre **{x:Bind}** y **{Binding}**), consulta el tema [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

La extensión de marcado **{Binding}** se convierte en tiempo de carga XAML en una instancia de la clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Este objeto de enlace obtiene un valor de propiedad en un origen de datos. Opcionalmente, el objeto de enlace puede configurarse para observar cambios en el valor de la propiedad de origen de datos y se actualiza en función de los cambios. Opcionalmente, también puede configurarse para insertar los cambios en su propio valor de nuevo en la propiedad de origen. La propiedad que es el destino de un enlace de datos debe ser una propiedad de dependencia. Para obtener más información, consulta [Introducción a las propiedades de dependencia](dependency-properties-overview.md).

**{Binding}** tiene la misma prioridad de propiedad de dependencia que un valor local, y definir un valor local en código imperativo quita el efecto de cualquier **{Binding}** definido en el marcado.

**Aplicaciones de ejemplo que muestran {Binding}**

-   Descarga la aplicación [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Descarga la aplicación [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

## Uso del atributo XAML


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
| *bindingProperties* | *propName*
            =
            *value*\[, *propName*=*value*\]*<br/>Una o más propiedades de enlace que se especifican con una sintaxis de par de nombre-valor. |
| *propName* | El nombre de cadena de la propiedad que se establecerá en el objeto [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Por ejemplo, "Converter". | 
| *value* | El valor en el que se establecerá la propiedad. La sintaxis del argumento depende de la propiedad de la sección [Propiedades de la clase Binding que se pueden establecer con {Binding}](#properties-of-binding) a continuación. |

## Ruta de acceso de propiedades

*PropertyPath* define el valor de [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), que es la propiedad a la que estás enlazando (la propiedad de origen). Puedes mencionar el nombre de la propiedad explícitamente: `{Binding Path=...}`. O puedes omitirlo: `{Binding ...}`.

El tipo de [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) es una ruta de acceso de propiedad, que es una cadena que se evalúa como una propiedad o una subpropiedad de su tipo personalizado o de un tipo de marco. El tipo puede ser (pero no tiene por qué serlo) un [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Los pasos de las rutas de acceso de propiedad están delimitados por puntos (.) y puedes incluir varios delimitadores para desviar subpropiedades sucesivas. Usa puntos como delimitadores independientemente del lenguaje de programación que uses para implementar el objeto al que se está enlazando.

Por ejemplo, para enlazar la interfaz de usuario a la propiedad del nombre de un objeto de un empleado, tu ruta de acceso de propiedad podría ser "Employee.FirstName". Si estás enlazando un control de elemento a una propiedad que contiene las personas a cargo del empleado, la ruta de acceso de la propiedad podría ser "Employee.Dependents" y la plantilla del elemento del control de elementos se ocuparía de mostrar los elementos en "Dependents".

Si el origen de datos es una colección, una ruta de acceso de propiedad puede especificar los elementos de la colección por su posición o índice. Por ejemplo, "Teams\[0\].Players", donde se escribe el literal "\[\]" alrededor del "0" que especifica el primer elemento en una colección.

Al usar un enlace [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) para un [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) existente, puedes usar propiedades adjuntas como parte de la ruta de acceso de la propiedad. Para desambiguar una propiedad adjunta de manera que el punto intermedio en el nombre de la propiedad adjunta no se considere un paso dentro de la ruta de acceso de la propiedad, coloca paréntesis alrededor del nombre de la propiedad adjunta calificado por el propietario; por ejemplo, `(AutomationProperties.Name)`.

Un objeto intermedio de ruta de acceso de propiedad se almacena como un objeto [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) en una representación en tiempo de ejecución, pero en la mayoría de escenarios no es necesario interactuar con un objeto **PropertyPath** en el código. Normalmente puedes especificar la información de enlace necesaria mediante XAML.

Para obtener más información sobre la sintaxis de cadenas de una ruta de acceso de propiedades, rutas de acceso de propiedades en áreas de características de animaciones y la construcción de un objeto [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259), consulta [Sintaxis de Property-path](property-path-syntax.md).

## Propiedades de la clase de enlace que se pueden establecer con {Binding}


**{Binding}** se explica a través de la sintaxis de marcadores de posición *bindingProperties* porque hay muchas propiedades de lectura y escritura de [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) que se pueden definir en la extensión de marcado. Las propiedades pueden establecerse en cualquier orden con pares separados por comas *propName*=*value*. Algunas de las propiedades requieren tipos que no tengan una conversión de tipos, por lo que requieren sus propias extensiones de marcado anidadas dentro de **{Binding}**.

| Propiedad | Descripción |
|----------|-------------|
| [**Ruta de acceso**](https://msdn.microsoft.com/library/windows/apps/br209830) | Consulta la sección [Ruta de acceso de propiedades](#property-path) anterior. |
| [**Convertidor**](https://msdn.microsoft.com/library/windows/apps/br209826) | Especifica el objeto convertidor que llama el motor de enlace. El convertidor puede establecerse en XAML, pero solo si se hace referencia a una instancia de objeto que hayas asignado en una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md) a ese objeto en el diccionario de recursos. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | Especifica la referencia cultural que usará el convertidor. (Si estableces [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)). La cultura se establece como un identificador basado en estándares. Para obtener más información, consulta **ConverterLanguage** | 
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | Especifica el parámetro del convertidor que se puede usar en la lógica del convertidor. (Si estableces [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)). La mayoría de convertidores usan una lógica simple que obtiene toda la información que necesitan del valor pasado para convertir, y no necesitan un valor **ConverterParameter**. El parámetro **ConverterParameter** es para las implementaciones de convertidor moderadamente avanzado con más de una lógica que controla lo que se pasa en **ConverterParameter**. Puedes escribir un convertidor que use valores que no sean cadenas, aunque no es lo habitual. Consulta las observaciones de **ConverterParameter** para obtener más información. |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | Especifica un origen de datos haciendo referencia a otro elemento en la misma construcción XAML que tenga la propiedad **Name** o el [atributo x:Name](x-name-attribute.md). A menudo, se usa para compartir valores relacionados o usar subpropiedades de un elemento de interfaz de usuario para proporcionar un valor específico para otro elemento, por ejemplo, en una plantilla de control XAML. |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | Especifica un valor que se mostrará cuando no se puede resolver el origen o la ruta de acceso. | 
| [**Modo**](https://msdn.microsoft.com/library/windows/apps/br209829) | Especifica el modo de enlace como una de las siguientes cadenas: "OneTime", "OneWay" o "TwoWay". Las cadenas corresponden a los nombres de constante de la enumeración de [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822). El valor predeterminado depende del destino de enlace, pero en la mayoría de los casos es "OneWay". Ten en cuenta que esto difiere del valor predeterminado de **{x:Bind}**, que es "OneTime". | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | Para especificar un origen de datos, describe la posición del origen del enlace con relación a la posición del destino de enlace. Esto se expresa en términos del gráfico de objetos en tiempo de ejecución, por ejemplo, especificando el elemento primario del objeto. Establecer la [extensión de marcado {RelativeSource}](relativesource-markup-extension.md). |
| [**Origen**](https://msdn.microsoft.com/library/windows/apps/br209832) | Especifica el origen de datos del objeto. Dentro de la extensión de marcado **Binding**, la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) requiere una referencia de objeto, como una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md). Si no se especifica esta propiedad, el contexto de datos que actúa especifica el origen. Es más habitual no especificar un valor Source en enlaces individuales y, en lugar de ello, basarse en la propiedad **DataContext** compartida para enlaces múltiples. Para más información, consulta [**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx) o [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946). |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | Especifica un valor que se mostrará cuando se resuelva el valor de origen, pero es explícitamente **null**. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | Especifica el tiempo de las actualizaciones de origen de enlace. Si no se especifica, el valor predeterminado es **Default**. |

**Nota**  Si quieres convertir el marcado de **{x:Bind}** a **{Binding}**, ten en cuenta las diferencias en los valores predeterminados de la propiedad **Mode**.

[
              **Converter**
            ](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) y **ConverterLanguage** tienen relación con la conversión de un valor o tipo del origen de enlace en un valor o tipo que sea compatible con la propiedad de destino de enlace. Para más información y ejemplos, consulta la sección “Conversiones de datos” de [Enlaces de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

[
              **Source**
            ](https://msdn.microsoft.com/library/windows/apps/br209832), [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) y [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) especifican un origen de enlace, por lo que son mutuamente excluyentes.

**Sugerencia**  Si tienes que especificar una llave para un valor, como en [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) o [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), debe ir precedido por una barra diagonal inversa: `\{`. Como alternativa, escribe la cadena completa que contiene las llaves que necesitan escape entre un conjunto de comillas secundario, por ejemplo, `ConverterParameter='{Mix}'`.

## Ejemplos

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

El segundo ejemplo establece cuatro propiedades de [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) distintas: [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) y [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826). **Path** en este caso se muestra explícitamente como una propiedad **Binding**. **Path** se evalúa en un origen de enlace de datos que es otro objeto del mismo árbol de objetos en tiempo de ejecución, un [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) denominado `sliderValueConverter`.

Observa que el valor de la propiedad [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) usa otra extensión de marcado, [extensión de marcado {StaticResource}](staticresource-markup-extension.md), de forma que hay dos usos de extensiones de marcado anidadas. La interior se evalúa primero de tal forma que, una vez obtenido el recurso, hay un [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) práctico (una clase personalizada de la cual el elemento `local:S2Formatter` de los recursos crea una instancia) que puede usarse en el enlace.

## Compatibilidad con herramientas

Microsoft IntelliSense en Microsoft Visual Studio muestra las propiedades del contexto de datos durante la creación de **{Binding}** en el editor de marcado XAML. Tan pronto como escribes "{Binding", se muestran las propiedades de contexto de datos adecuadas para [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) en la lista desplegable. IntelliSense también ayuda con las otras propiedades de [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Para que funcione, debes tener el contexto de datos o el contexto de datos en tiempo de diseño establecido en la página de marcado. **Ir a definición** (F12) también funciona con **{Binding}**. Como alternativa, puedes usar el cuadro de diálogo de enlace de datos.

 




<!--HONumber=Jun16_HO4-->



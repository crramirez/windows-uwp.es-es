---
description: Proporciona un valor para cualquier atributo XAML mediante la evaluación de una referencia a un recurso, con lógica del sistema adicional que recupera diferentes recursos en función del tema activo en ese momento.
title: Extensión de marcado ThemeResource
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 89ae286fbc29628e8d81899265019ccdaaa038a3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161709"
---
# <a name="themeresource-markup-extension"></a>Extensión de marcado {ThemeResource}

Proporciona un valor para cualquier atributo XAML mediante la evaluación de una referencia a un recurso, con lógica del sistema adicional que recupera diferentes recursos en función del tema activo en ese momento. De forma similar a la [extensión de marcado {StaticResource}](staticresource-markup-extension.md), los recursos se definen en un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) y el uso de **ThemeResource** hace referencia a la clave de ese recurso en el **ResourceDictionary**.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| key | Clave del recurso solicitado. El [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)asigna inicialmente esta clave. Una clave de recurso puede ser cualquier cadena que se defina con la gramática XamlName. |

## <a name="remarks"></a>Observaciones

Un **ThemeResource** es una técnica para obtener valores para un atributo XAML que se definen en otra parte de un diccionario de recursos XAML. La extensión de marcado tiene el mismo propósito básico que la [extensión de marcado {StaticResource}](staticresource-markup-extension.md). La diferencia de comportamiento respecto a la extensión de marcado {StaticResource} es que una referencia a **ThemeResource** puede usar diferentes diccionarios de forma dinámica como ubicación de búsqueda principal, en función del tema que esté usando el sistema en ese momento.

Cuando la aplicación se inicia por primera vez, las referencias a recursos realizadas por una referencia a **ThemeResource** se evalúan en función del tema que se esté usando en el inicio. Pero si el usuario cambia después el tema activo en tiempo de ejecución, el sistema volverá a evaluar todas las referencias a **ThemeResource**, recuperará un recurso específico del tema que podría ser diferente, y volverá a mostrar la aplicación con los nuevos valores de recurso en los lugares correspondientes del árbol visual. Un **StaticResource** se determina en el momento de la carga del código XAML o del inicio de la aplicación y no se vuelve a evaluar en tiempo de ejecución. (Hay otras técnicas, como los estados visuales, que vuelven a cargar el código XAML dinámicamente, pero trabajan en un nivel superior al de la evaluación básica de recursos que permite la [extensión de marcado {StaticResource}](staticresource-markup-extension.md)).

**ThemeResource** toma un argumento, que especifica la clave del recurso solicitado. Una clave de recursos es siempre una cadena en XAML de Windows Runtime. Para obtener más información acerca de cómo especificar inicialmente la clave de recurso, consulta [Atributo x:Key](x-key-attribute.md).

Para obtener más información sobre cómo definir recursos y usar correctamente un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary), incluida una muestra de código, consulta [Referencias a ResourceDictionary y a recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).

**Importante** Como con **StaticResource**, un **ThemeResource** no debe intentar realizar una referencia adelantada a un recurso que se define léxicamente en el archivo XAML. Este intento no se admite. Aunque la referencia adelantada no genere un error, intentar llevarla a cabo conlleva una penalización de rendimiento. Para obtener los mejores resultados, ajusta la composición de tus diccionarios de recursos de manera que se eviten las referencias adelantadas.

Intentar especificar un **ThemeResource** en una clave que no puede resolverse inicia una excepción de análisis XAML en tiempo de ejecución. Las herramientas de diseño también pueden ofrecer advertencias o errores.

En la implementación del procesador XAML de Windows Runtime no hay una representación de clase de respaldo para la funcionalidad de **ThemeResource**. El equivalente más parecido en el código consiste en usar la API de colección de un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary), por ejemplo, una llamada a [**Contains**](/uwp/api/windows.ui.xaml.resourcedictionary.contains) o [**TryGetValue**](/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue).

**ThemeResource** es una extensión de marcado. Las extensiones de marcado se suelen implementar cuando se necesita que los valores de los atributos de escape no sean valores literales o nombres de controladores, y este requisito es de índole más global que limitarse a colocar los convertidores de tipos en determinados tipos o propiedades. Todas las extensiones de marcado en XAML usan los caracteres "{" y "}" en su sintaxis de atributo, que es la convención mediante la cual un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>Cuánto y cómo usar {ThemeResource} en lugar de {StaticResource}

Las reglas por las que un **ThemeResource** se resuelve en un elemento de un diccionario de recursos suelen ser las mismas que en el caso de **StaticResource**. Una búsqueda de **ThemeResource** puede extenderse a los archivos de [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) a los que se hace referencia en una colección [**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries), pero un **StaticResource** también puede hacerlo. La diferencia es que un **ThemeResource** puede volver a evaluarse en tiempo de ejecución, y un **StaticResource** no.

El conjunto de claves de cada diccionario de temas debe proporcionar el mismo conjunto de recursos con clave independientemente del tema que esté activo. Si en el diccionario de temas **HighContrast** hay un recurso con clave determinado, también habrá otro recurso con ese nombre en **Light** y **Default**. De lo contrario, la búsqueda de recursos producirá un error cuando el usuario cambie los temas y la aplicación no se verá bien. Sin embargo, es posible que un diccionario de temas pueda contener recursos con clave a los que solo se haga referencia desde el mismo ámbito para proporcionar subvalores; estos no tienen que ser equivalentes en todos los temas.

Por lo general, debes colocar estos recursos en diccionarios de temas y hacer referencia a ellos usando **ThemeResource** solo cuando esos valores puedan cambiar de un tema a otro o sean compatibles con valores que cambian. Esto es apropiado para los siguientes tipos de escenarios:

-   Pinceles, en especial colores de [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Suponen alrededor del 80 % de los usos de **ThemeResource** en las plantillas de control XAML predeterminadas (generic.xaml).
-   Valores de píxeles para bordes, desplazamiento, márgenes, rellenos, etc.
-   Propiedades de fuentes, como **FontFamily** o **FontSize**.
-   Plantillas completas para un número limitado de controles a los que normalmente el sistema aplica el estilo y que se usan para presentaciones dinámicas, como [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) y [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem).
-   Estilos de presentación de texto (normalmente para cambiar el color de la fuente, el fondo y quizás el tamaño).

Windows Runtime proporciona un conjunto de recursos que están diseñados específicamente para que **ThemeResource** haga referencia a ellos. Todos estos recursos se enumeran como parte del archivo XAML themeresources.xaml, que está disponible en la carpeta include/winrt/xaml/design del Kit de desarrollo de software de Windows (SDK). Para obtener documentación sobre los pinceles de temas y otros estilos adicionales definidos en themeresources.xaml, consulta [Referencia a recursos de temas en XAML](../design/controls-and-patterns/xaml-theme-resources.md). Los pinceles se documentan en una tabla que indica el valor de color que tiene cada pincel para los tres posibles temas activos.

Las definiciones XAML de los estados visuales en una plantilla de control deben usar referencias a **ThemeResource** siempre que haya un recurso subyacente que pueda cambiar debido a un cambio de tema. Normalmente, un cambio de tema del sistema no provocará un cambio del estado visual. En este caso, los recursos deben usar referencias a **ThemeResource** para que se puedan volver a evaluar los valores para el estado visual aún activo. Por ejemplo, si tienes un estado visual que cambia un color de pincel de una parte determinada de la interfaz de usuario y una de sus propiedades, y ese color de pincel es diferente de un tema a otro, debes usar una referencia a **ThemeResource** para proporcionar el valor de esa propiedad en la plantilla predeterminada así como todas las modificaciones del estado visual en esa plantilla predeterminada.

Los usos de **ThemeResource** se podrían ver en una serie de valores dependientes. Por ejemplo, un valor de [**Color**](/uwp/api/Windows.UI.Color) usado por un [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) que también es un recurso con clave podría usar una referencia a **ThemeResource**. Sin embargo, las propiedades de la interfaz de usuario que usan el recurso **SolidColorBrush** con clave también usarían una referencia a **ThemeResource**, por lo que es cada propiedad del tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) la que habilita específicamente un cambio de valor dinámico cuando el tema cambia.

**Nota:**   `{ThemeResource}` y la evaluación de recursos en tiempo de ejecución en la conmutación de temas se admite en Windows 8.1 XAML, pero no se admite en XAML para las aplicaciones destinadas a Windows 8.

### <a name="system-resources"></a>Recursos del sistema

Algunos recursos de temas hacen referencia a valores de recursos del sistema como un subvalor subyacente. Un recurso del sistema es un valor de recurso especial que no se encuentra en ningún diccionario de recursos XAML. El comportamiento de estos valores se basa en que el XAML de Windows Runtime permite reenviar valores desde el propio sistema y representarlos de forma que un recurso XAML pueda hacer referencia a ellos. Por ejemplo, hay un recurso del sistema denominado "SystemColorButtonFaceColor" que representa un color RGB. Este color deriva de los aspectos de los colores y temas del sistema que no son específicos solo de Windows Runtime y de las aplicaciones de Windows Runtime.

Con frecuencia, los recursos del sistema son los valores subyacentes de un tema de contraste alto. El usuario controla las opciones de color de su tema de contraste alto y selecciona estas opciones usando características del sistema que tampoco son específicas de las aplicaciones de Windows Runtime. Al hacer referencia a los recursos del sistema como referencias a **ThemeResource**, el comportamiento predeterminado de los temas de contraste alto en las aplicaciones de Windows Runtime puede usar estos valores específicos del tema que son controlados por el usuario y expuestos por el sistema. Además, las referencias ahora quedan marcadas para volverlas a evaluar si el sistema detecta un cambio de tema en tiempo de ejecución.

### <a name="an-example-themeresource-usage"></a>Ejemplo de uso de {ThemeResource}

Este es un ejemplo de XAML tomado de los archivos generic.xaml y themeresources.xaml predeterminados para mostrar cómo usar **ThemeResource**. Solo veremos una plantilla (la predeterminada [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)) y cómo se declaran dos propiedades ([**Background**](/uwp/api/windows.ui.xaml.controls.control.background) y [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground)) para que respondan a los cambios de tema.

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

Aquí, las propiedades toman un valor [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush), y la referencia a los recursos [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) denominados `ButtonBackgroundThemeBrush` y `ButtonForegroundThemeBrush` se realiza usando **ThemeResource**.

Algunos de los estados visuales de un [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) también ajustan estas mismas propiedades. Lo más destacable es que el color de fondo cambia cuando se hace clic en un botón. Y aquí, las animaciones [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) y [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) en el guión gráfico del estado visual usan objetos [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) y referencias a pinceles con **ThemeResource** como valor de marco con clave.

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

Cada uno de estos pinceles se define antes en generic.xaml: deben estar definidos antes de que ninguna plantilla pueda usarlos para evitar referencias adelantadas a XAML. Estas son las definiciones para el diccionario de temas "Default".

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

Después, cada uno de los demás diccionarios de temas tiene definidos también estos pinceles, por ejemplo:

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

Aquí, el valor de [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) es otra referencia a **ThemeResource** a un recurso del sistema. Si haces referencia a un recurso del sistema y quieres cambiarlo en respuesta a un cambio de tema, debes usar **ThemeResource** para crear la referencia.

## <a name="windows8-behavior"></a>Comportamiento de Windows 8

Windows 8 no era compatible con la extensión de marcado **ThemeResource**. Esta está disponible a partir de Windows 8.1. Además, Windows 8 no admitía la conmutación dinámica de recursos relacionados por tema para una aplicación de Windows Runtime. La aplicación tenía que reiniciarse para elegir el cambio de tema de las plantillas y estilos de XAML. Esto no es una buena experiencia de usuario, por lo que se recomienda volver a compilar las aplicaciones para Windows 8.1, para que puedan usar estilos con usos de **ThemeResource** y puedan conmutar dinámicamente temas cuando lo haga el usuario. Las aplicaciones compiladas para Windows 8 que se ejecuten en Windows 8.1 siguen usando el comportamiento de Windows 8.

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>Compatibilidad con herramientas en tiempo de diseño para la extensión de marcado **{ThemeResource}**

Microsoft Visual Studio 2013 puede incluir posibles valores de clave en los menús desplegables de Microsoft IntelliSense cuando uses la extensión de marcado **{ThemeResource}** en una página XAML. Por ejemplo, cuando escribes "{ThemeResource", aparece cualquiera de las claves de recurso de los [recursos de tema XAML](../design/controls-and-patterns/xaml-theme-resources.md).

Cuando exista una clave de recurso como parte del uso de cualquier **{ThemeResource}**, la característica **Ir a definición** (F12) puede resolver ese recurso y mostrar el generic.xaml para tiempo de diseño, donde se define el recurso de tema. Puesto que los recursos de tema se definen más de una vez (por tema) **Ir a definición** te lleva a la primera definición que se encuentre en el archivo, que es la definición de **Default**. Si quieres tener las otras definiciones, puedes buscar el nombre de clave dentro del archivo y buscar las definiciones de los otros temas.

## <a name="related-topics"></a>Temas relacionados

* [Referencias a ResourceDictionary y a los recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [Recursos de temas XAML](../design/controls-and-patterns/xaml-theme-resources.md)
* [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [Atributo x:Key](x-key-attribute.md)
 
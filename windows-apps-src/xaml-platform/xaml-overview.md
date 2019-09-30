---
description: Presenta el lenguaje XAML y los conceptos XAML al público del desarrollador de aplicaciones Windows Runtime y describe las distintas formas de declarar objetos y establecer atributos en XAML, ya que se usa para crear una aplicación Windows Runtime.
title: Introducción a XAML
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 6d45e70afa5b0dc6e903dbd253c09042bb2046c2
ms.sourcegitcommit: f7ef7e894d7b7fc24483b4485605686abf8f2e93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679209"
---
# <a name="xaml-overview"></a>Introducción a XAML

En este artículo se presentan el lenguaje XAML y los conceptos de XAML al público del desarrollador de aplicaciones Windows Runtime y se describen las distintas formas de declarar objetos y establecer atributos en XAML, ya que se usan para crear una aplicación Windows Runtime.

## <a name="what-is-xaml"></a>¿Qué es XAML?

El lenguaje de marcado de aplicaciones extensible (XAML) es un lenguaje declarativo. En concreto, XAML puede inicializar objetos y establecer las propiedades de los objetos mediante una estructura de lenguaje que muestra las relaciones jerárquicas entre varios objetos y una Convención de tipos de respaldo que admite la extensión de tipos. Puedes crear elementos visibles de la interfaz de usuario en el marcado XAML declarativo. A continuación, puedes asociar un archivo de código subyacente distinto para cada archivo XAML que puede responder a eventos y manipular los objetos que originalmente declares en XAML.

El lenguaje XAML admite el intercambio de orígenes entre diferentes herramientas y roles en el proceso de desarrollo, como intercambiar orígenes XAML entre herramientas de diseño y un entorno de desarrollo interactivo (IDE) o entre desarrolladores y localización principales. Developer. Al usar XAML como formato de intercambio, los roles de diseñador y de desarrollador se pueden mantener separados o juntos, y los diseñadores y desarrolladores pueden iterar durante la producción de una aplicación.

Cuando los veas como parte de tus proyectos de aplicaciones de Windows Runtime, los archivos XAML son archivos XML con la extensión de nombre de archivo .xaml.

## <a name="basic-xaml-syntax"></a>Sintaxis XAML básica

XAML posee una sintaxis básica que se basa en XML y, por definición, un código XAML válido debe ser un código XML válido. Sin embargo, XAML también tiene conceptos de sintaxis que se asignan a un significado diferente y más completo mientras siguen siendo válidos en XML según la especificación XML 1,0. Por ejemplo, XAML admite *la sintaxis de elemento de propiedad*, en la que se pueden establecer valores de propiedad dentro de los elementos, en vez de valores de cadena en atributos o como contenido. Para XML normal, un elemento de propiedad XAML es un elemento con un punto en su nombre, por lo que es válido para XML normal, pero no tiene el mismo significado.

## <a name="xaml-and-visual-studio"></a>XAML y Visual Studio

Microsoft Visual Studio te ayuda a producir una sintaxis XAML válida, tanto en el editor de texto XAML como en la superficie de diseño de XAML de orientación más gráfica. Al escribir XAML para la aplicación con Visual Studio, no se preocupe demasiado sobre la sintaxis con cada pulsación de tecla. El IDE fomenta la sintaxis XAML válida proporcionando sugerencias de autocompletar, mostrando sugerencias en listas y desplegables de Microsoft IntelliSense, mostrando bibliotecas de elementos de interfaz de usuario en la ventana **cuadro de herramientas** u otras técnicas. Si esta es su primera experiencia con XAML, puede que siga siendo útil conocer las reglas de sintaxis y, en particular, la terminología que se usa a veces para describir las restricciones o opciones al describir la sintaxis de XAML en referencia u otros temas. Los puntos de la sintaxis XAML se describen en un tema independiente, [Guía de sintaxis de XAML](xaml-syntax-guide.md).

## <a name="xaml-namespaces"></a>Espacios de nombres XAML

En programación general, un espacio de nombres es un concepto de organización que determina la forma en que se interpretan los identificadores para entidades de programación. Con los espacios de nombres, un marco de programación puede separar los identificadores declarados por el usuario de los declarados por el marco, desambiguar los identificadores mediante cualificaciones del espacio de nombres y aplicar reglas para el control de nombres, etc. XAML tiene su propio concepto de espacio de nombres XAML que se usa para este fin en el lenguaje XAML. Así es cómo XAML aplica y extiende los conceptos de espacio de nombres del lenguaje XML:

- XAML usa el atributo XML reservado **xmlns** para declaraciones de espacio de nombres. El valor del atributo suele ser un identificador uniforme de recursos (URI), que es una convención heredada de XML.
- XAML usa prefijos en declaraciones para declarar espacios de nombres no predeterminados, y el uso de prefijos en elementos y atributos hace referencia a ese espacio de nombres.
- XAML tiene un concepto de un espacio de nombres predeterminado, que es el espacio de nombres usado cuando no existe un prefijo en un uso o una declaración. El espacio de nombres predeterminado se puede definir de forma diferente para cada marco de programación XAML.
- Las definiciones de espacio de nombres se heredan en un constructor o archivo XAML, de elementos primarios a elementos secundarios. Por ejemplo, si define un espacio de nombres en el elemento raíz de un archivo XAML, todos los elementos de ese archivo heredan esa definición de espacio de nombres. Si un elemento más profundo de la página vuelve a definir el espacio de nombres, los descendientes de ese elemento heredarán la nueva definición.
- Los atributos de un elemento heredan los espacios de nombres del elemento. No es nada común ver prefijos en atributos XAML.

Un archivo XAML casi siempre declara un espacio de nombres XAML predeterminado en su elemento raíz. El espacio de nombres XAML predeterminado define qué elementos se pueden declarar sin cualificarlos con un prefijo. En el caso de proyectos de aplicaciones de Windows Runtime normales, el espacio de nombres predeterminado contiene todos los atributos y elementos integrados en el vocabulario XAML para Windows Runtime: esto es todo el conjunto de controles predeterminados, elementos de texto, animaciones y elementos gráficos XAML, tipos de compatibilidad de estilo y enlace de datos, etc. Así, la mayoría del código XAML que escribas para aplicaciones de Windows Runtime podrá evitar el uso de prefijos y espacios de nombres XAML cuando te refieras a elementos comunes de la interfaz de usuario.

Este es un fragmento de código que muestra una raíz <xref:Windows.UI.Xaml.Controls.Page> creada por una plantilla de la página inicial de una aplicación (que solo muestra la etiqueta de apertura y simplificada). Declara el espacio de nombres predeterminado y, también, el espacio de nombres **x** (que explicaremos a continuación).

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>Espacio de nombres XAML del lenguaje XAML

Un espacio de nombres XAML particular que se declara en casi todos los archivos XAML de Windows en tiempo de ejecución es el espacio de nombres del lenguaje XAML. Este espacio de nombres incluye elementos y conceptos que se definen mediante la especificación del lenguaje XAML. Por convención, al espacio de nombres XAML del lenguaje XAML se le asigna el prefijo "x". Las plantillas de archivo y proyecto predeterminadas para los proyectos de aplicaciones de Windows Runtime siempre definen tanto el espacio de nombres XAML predeterminado (sin prefijo, simplemente `xmlns=`) como el espacio de nombres XAML del lenguaje XAML (prefijo "x") como parte del elemento raíz.

El espacio de nombres XAML del lenguaje XAML o prefijo "x" contiene varias construcciones de programación que suelen usarse en XAML. Estas son algunas de las más habituales:

| Término | Descripción |
|------|-------------|
| [x:Key](x-key-attribute.md) | Establece una clave única definida por el usuario para cada recurso de un XAML <xref:Windows.UI.Xaml.ResourceDictionary>. La cadena de token de la clave es el argumento de la extensión de marcado **StaticResource** y, más adelante, se usa esta clave para recuperar el recurso XAML de otro uso de XAML en cualquier otro punto del XAML de la aplicación. |
| [x:Class](x-class-attribute.md) | Especifica el espacio de nombres del código y el nombre de clase del código para la clase que proporciona el código subyacente de una página XAML. Asigna un nombre a la clase creada o unida mediante acciones de compilación cuando se compila la aplicación. Estas acciones de compilación admiten el compilador de marcado XAML y combinan el marcado y el código subyacente cuando se compila la aplicación. Debes contar con una clase como esta para admitir código subyacente para una página XAML. <xref:Windows.UI.Xaml.Window.Content%2A?displayProperty=nameWithType> en el modelo de activación Windows Runtime predeterminado. |
| [x:Name](x-name-attribute.md) | Especifica un nombre de objeto en tiempo de ejecución para la instancia que existe en el código en tiempo de ejecución después de procesar un elemento de objeto definido en XAML. Puedes equiparar la definición de **x:Name** en XAML a declarar una variable con nombre en código. Como verás más adelante, eso es exactamente lo que ocurre cuando se carga tu XAML como componente de una aplicación de Windows Runtime. <br/><div class="alert">**Tenga en cuenta** <xref:Windows.UI.Xaml.FrameworkElement.Name%2A> es una propiedad similar en el marco de trabajo, pero no todos los elementos lo admiten. Use **x:Name** para la identificación de elementos siempre que no se admita **FrameworkElement.Name** en ese tipo de elemento. |
| [x:Uid](x-uid-directive.md) | Identifica los elementos que deben usar recursos localizados para algunos de sus valores de propiedad. Para obtener más información sobre cómo usar **x:UID**, vea [Quickstart: Trasladar recursos de la interfaz de usuario @ no__t-0. |
| [Tipos de datos intrínsecos de XAML](xaml-intrinsic-data-types.md) | Estos tipos pueden especificar valores para tipos valor simple cuando sean necesarios para un atributo o un recurso. Estos tipos intrínsecos se corresponden con tipos de valores simples que suelen definirse como parte de las definiciones intrínsecas de cada lenguaje de programación. Por ejemplo, puede que necesite un objeto que represente un valor booleano **true** para usarlo en un estado visual de guion gráfico de @no__t 1. Para ese valor en XAML, usaría el tipo intrínseco **x:Boolean** como elemento de objeto, como el siguiente: <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> |

Existen otras construcciones de programación en el espacio de nombres XAML del lenguaje XAML, pero no son tan comunes.

## <a name="mapping-custom-types-to-xaml-namespaces"></a>Asignación de tipos personalizados a espacios de nombres XAML

Uno de los aspectos más importantes de XAML como lenguaje es que es fácil extender el vocabulario XAML para aplicaciones de Windows en tiempo de ejecución. Puedes definir tus propios tipos personalizados en el lenguaje de programación de tus aplicaciones y, luego, hacer referencia a esos tipos personalizados en el marcado XAML. La compatibilidad con la extensión mediante tipos personalizados está fundamentalmente integrada en el funcionamiento del lenguaje XAML. Los desarrolladores de aplicaciones o los marcos son los responsables de crear los objetos de apoyo a los que XAML hace referencia. Ninguno de los marcos ni el desarrollador de la aplicación están limitados por las especificaciones de lo que representan los objetos de sus vocabularios o lo hacen más allá de las reglas básicas de sintaxis XAML. (Hay algunas expectativas de lo que deben hacer los tipos de espacio de nombres XAML del lenguaje XAML, pero el Windows Runtime proporciona toda la compatibilidad necesaria).

Si usas XAML para tipos procedentes de bibliotecas que no sean los metadatos ni las bibliotecas principales de Windows Runtime, debes declarar y asignar un espacio de nombres XAML a un prefijo. Usa ese prefijo en los usos de elementos para hacer referencia a los tipos que se definieron en la biblioteca. Declaras las asignaciones de prefijo como atributos **xmlns**, normalmente en un elemento raíz junto con las otras definiciones de espacio de nombres XAML.

Para crear tu propia definición de espacio de nombres que haga referencia a tipos personalizados, primero debes especificar la palabra clave **xmlns:** y, luego, el prefijo que quieras. El valor de ese atributo debe contener la palabra clave **using:** como primera parte del valor. El resto del valor es un token de cadena que hace referencia al espacio de nombres con respaldo de código específico que contiene tus tipos personalizados, por su nombre.

El prefijo define el token de marcado que se usa para hacer referencia a ese espacio de nombres XAML en el resto del marcado de ese archivo XAML. Se escribe un signo de dos puntos (:) entre el prefijo y la entidad a la que se va a hacer referencia en el espacio de nombres XAML.

Por ejemplo, la sintaxis de atributo para asignar un prefijo `myTypes` al espacio de nombres `myCompany.myTypes` es: `    xmlns:myTypes="using:myCompany.myTypes"` y un uso representativo del elemento es: `<myTypes:CustomButton/>`.

Para obtener más información sobre la asignación de espacios de nombres XAML para tipos personalizados, incluidas consideraciones especiales para las extensiones de componente de Visual C++ (C++/CX) consulta [Espacios de nombres XAML y asignación de espacios de nombres](xaml-namespaces-and-namespace-mapping.md).

## <a name="other-xaml-namespaces"></a>Otros espacios de nombres XAML

A menudo ves archivos XAML que definen los prefijos "d" (para el espacio de nombres del diseñador) y "mc" (para la compatibilidad del marcado). Por lo general, son para la compatibilidad con la infraestructura o para habilitar escenarios en una herramienta en tiempo de diseño. Para obtener más información, consulta la [sección "Otros espacios de nombres XAML" en el tema Espacios de nombres XAML](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces).

## <a name="markup-extensions"></a>Extensiones de marcado

Las extensiones de marcado son un concepto del lenguaje XAML que suele usarse en la implementación de XAML de Windows Runtime. Las extensiones de marcado suelen representar algún tipo de "método abreviado" que permite a un archivo XAML tener acceso a un valor o comportamiento que no declare simplemente elementos basados en tipos de respaldo. Algunas extensiones de marcado pueden establecer propiedades con cadenas sencillas o con más elementos anidados, para simplificar la sintaxis o los factores entre distintos archivos XAML.

En la sintaxis de los atributos XAML, las llaves "{" y "}" indican el uso de una extensión de marcado XAML. Este uso indica al procesamiento de XAML que abandone el tratamiento general de los valores de atributo como una cadena literal o un valor convertible directamente en cadena. En su lugar, un analizador XAML llama a un código que proporciona un comportamiento para esa extensión de marcado específica. Ese código proporciona un objeto o un resultado de comportamiento alternativo que el analizador de XAML necesita. Las extensiones de marcado pueden tener argumentos, que siguen el nombre de extensión de marcado y también se encierran entre llaves. Normalmente, una extensión de marcado evaluada proporciona el valor devuelto de un objeto. Durante el análisis, ese valor devuelto se inserta en la posición del árbol de objetos, donde estaba el uso de la extensión de marcado en el XAML de origen.

El lenguaje XAML de Windows Runtime admite estas extensiones de marcado que se definen en el espacio de nombres XAML predeterminado y que el analizador XAML de Windows Runtime comprende:

- [{x:Bind}](x-bind-markup-extension.md): admite el enlace de datos, que aplaza la evaluación de la propiedad hasta el tiempo de ejecución mediante la ejecución de código de propósito especial, que se genera en tiempo de compilación. Esta extensión de marcado admite una amplia gama de argumentos.
- [{Binding}](binding-markup-extension.md): admite el enlace de datos, el cual aplaza la evaluación de propiedades hasta el tiempo de ejecución mediante la ejecución de la inspección de objetos en tiempo de ejecución de propósito general. Esta extensión de marcado admite una amplia gama de argumentos.
- [{StaticResource}](staticresource-markup-extension.md): admite la referencia a valores de recursos que se definen en un <xref:Windows.UI.Xaml.ResourceDictionary>. Estos recursos pueden estar en otro archivo XAML pero, en última instancia, el analizador XAML debe poder encontrarlos en el tiempo de carga. El argumento de un uso de `{StaticResource}` identifica la clave (el nombre) de un recurso con clave en un <xref:Windows.UI.Xaml.ResourceDictionary>.
- [{ThemeResource}](themeresource-markup-extension.md): Es parecido a [{StaticResource}](staticresource-markup-extension.md), pero puede responder a cambios de tema en tiempo de ejecución. {ThemeResource} aparece con bastante frecuencia en las plantillas XAML predeterminadas de Windows en tiempo de ejecución, ya que la mayoría de esas plantillas se han diseñado para que admitan que el usuario cambie de tema mientras la aplicación se ejecuta.
- [{TemplateBinding}](templatebinding-markup-extension.md): un caso especial de [{Binding}](binding-markup-extension.md) que admite plantillas de control en XAML y su posterior uso en tiempo de ejecución.
- [{RelativeSource}](relativesource-markup-extension.md): habilita una forma determinada de enlace de plantilla en la que los valores provienen de un primario en plantilla.
- [{CustomResource}](customresource-markup-extension.md): para escenarios de búsqueda avanzada de recursos.

Windows Runtime también admite la extensión de marcado [{x:Null}](x-null-markup-extension.md). Se usa para establecer valores [**Nullable**](/dotnet/api/system.nullable-1) en **null** en XAML. Por ejemplo, podría utilizar esto en una plantilla de control para un <xref:Windows.UI.Xaml.Controls.CheckBox>, que interpreta **null** como un estado de comprobación indeterminado (desencadenando el estado visual "indeterminado").

Normalmente, una extensión de marcado devuelve una instancia existente de otra parte del gráfico de objetos de la aplicación o pospone un valor a tiempo de ejecución. Dado que puedes usar una extensión de marcado como valor de atributo, lo que es el uso típico, verás a menudo extensiones de marcado que proporcionan valores para propiedades de tipo de referencia que, de lo contrario, podrían haber necesitado la sintaxis de un elemento de propiedad.

Por ejemplo, esta es la sintaxis para hacer referencia a un @no__t reutilizable de un <xref:Windows.UI.Xaml.ResourceDictionary>: `<Button Style="{StaticResource SearchButtonStyle}"/>`. Un <xref:Windows.UI.Xaml.Style> es un tipo de referencia, no un valor simple, por lo que sin el uso de `{StaticResource}`, habría necesitado un elemento de propiedad `<Button.Style>` y una definición de `<Style>` en él para establecer la propiedad <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType>.

Con las extensiones de marcado, todas las propiedades que se pueden establecer en XAML se podrían establecer en la sintaxis de atributo. Puedes usar la sintaxis de atributo para proporcionar valores de referencia para una propiedad aunque no admita una sintaxis de atributo para la creación directa de instancias de objeto. O bien, puedes habilitar un comportamiento específico que aplace el requisito general por el que las propiedades XAML deben rellenarse con tipos de valor o con tipos de referencia recién creados.

En el siguiente ejemplo de XAML se establece el valor de la propiedad <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> de un <xref:Windows.UI.Xaml.Controls.Border> mediante la sintaxis de atributo. La propiedad <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> toma una instancia de la clase <xref:Windows.UI.Xaml.Style?displayProperty=nameWithType>, un tipo de referencia que de forma predeterminada no se pudo crear mediante una cadena de sintaxis de atributo. Pero, en este caso, el atributo hace referencia a una extensión de marcado determinada, [StaticResource](staticresource-markup-extension.md). Cuando se procesa la extensión de marcado, devuelve una referencia a un elemento **Style** que definiste anteriormente como un recurso con clave en un diccionario de recursos.

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

Puedes anidar extensiones de marcado. Primero se evalúa la extensión de marcado más interna.

Debido a las extensiones de marcado, necesitarás una sintaxis especial para un valor "{" literal en un atributo. Para obtener más información, consulta la [Guía de sintaxis XAML](xaml-syntax-guide.md).

## <a name="events"></a>Events

XAML es un lenguaje declarativo para objetos y sus propiedades, pero también incluye una sintaxis para adjuntar controladores de eventos a los objetos del marcado. La sintaxis de evento de XAML puede integrar los eventos declarados mediante XAML con el modelo de programación de Windows Runtime. El nombre del evento se especifica como un nombre de atributo en el objeto donde se controla el evento. Para el valor de atributo, debes especificar el nombre de una función de controlador de eventos que definas en el código. El procesador de XAML usa este nombre para crear una representación de delegado en el árbol de objetos cargados, y agrega el controlador especificado a una lista de controladores internos. La mayoría de las aplicaciones de Windows Runtime se define mediante marcado y orígenes de código subyacente.

Este es un ejemplo sencillo. La clase <xref:Windows.UI.Xaml.Controls.Button> admite un evento denominado <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click>. Puedes escribir un controlador para **Click** que ejecute un código que debe invocarse después de que el usuario haga clic en el **Button**. En XAML, **Click** se especifica como un atributo en el **Button**. Para el valor del atributo, proporciona una cadena que sea el nombre de método de tu controlador.

```xml
<Button Click="showUpdatesButton_Click">Show updates</Button>
```

A la hora de compilar, el compilador espera que haya un método denominado `showUpdatesButton_Click` definido en el archivo de código subyacente, en el espacio de nombres declarado en el valor [x:Class](x-class-attribute.md) de la página XAML. Además, ese método debe cumplir el contrato de delegado para el evento <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click>. Por ejemplo:

```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```

```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton_Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```

```cppwinrt
namespace winrt::App1::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        ...
        void showUpdatesButton_Click(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);
    };
}
```

```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton_Click(Object^ sender, RoutedEventArgs^ e);
    };
}
```

Dentro de un proyecto, el código XAML se escribe como un archivo .xaml y se usa el lenguaje que prefieras (C#, Visual Basic, C++/CX) para escribir un archivo de código subyacente. Cuando se compila el marcado de un archivo XAML como parte de la acción de compilación del proyecto, la ubicación del archivo de código subyacente XAML para cada página XAML se identifica especificando un espacio de nombres y una clase como el atributo [x:Class](x-class-attribute.md) del elemento raíz de la página XAML. Para obtener más información sobre cómo funcionan estos mecanismos en XAML y cómo se relacionan con los modelos de programación y aplicación, consulta [Introducción a eventos y eventos enrutados](events-and-routed-events-overview.md).

> [!NOTE]
> Para C++/CX hay dos archivos de código subyacente: uno es un encabezado (. Xaml. h) y el otro es la implementación (. Xaml. cpp). La implementación hace referencia al encabezado y es técnicamente el encabezado el que representa el punto de entrada para la conexión de código subyacente.

## <a name="resource-dictionaries"></a>Diccionarios de recursos

Crear un <xref:Windows.UI.Xaml.ResourceDictionary> es una tarea común que normalmente se realiza mediante la creación de un diccionario de recursos como un área de una página XAML o un archivo XAML independiente. Los diccionarios de recursos y cómo usarlos es un amplio campo conceptual que queda fuera del alcance de este tema. Para obtener más información, consulta [Referencias a ResourceDictionary y a recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).

## <a name="xaml-and-xml"></a>XAML y XML

El lenguaje XAML se basa fundamentalmente en el lenguaje XML. Sin embargo, XAML amplia XML de forma significativa. En particular, trata el concepto de esquema de forma muy diferente debido a su relación con el concepto de tipo de respaldo, y agrega elementos del lenguaje como miembros adjuntos y extensiones de marcado. **xml:lang** es válido en XAML, aunque influye en el tiempo de ejecución y no en el comportamiento del análisis, y suele tener un alias para una propiedad en el nivel de marco. Para obtener más información, vea <xref:Windows.UI.Xaml.FrameworkElement.Language%2A?displayProperty=nameWithType>. **xml:base** es válido en el marcado, pero los analizadores lo ignoran. **xml:space** es válido, pero solo es relevante para los escenarios que se describen en el tema [XAML y espacio en blanco](xaml-and-whitespace.md). El atributo **encoding** es válido en XAML. Solo se admiten las codificaciones UTF-8 y UTF-16. No se admite la codificación UTF-32.

###  <a name="case-sensitivity-in-xaml"></a>Distinción de mayúsculas y minúsculas en XAML

XAML distingue mayúsculas de minúsculas. Esta es otra consecuencia de que XAML se base en XML, que distingue mayúsculas de minúsculas. Los nombres de los elementos y de los atributos XAML distinguen entre mayúsculas y minúsculas. El valor de un atributo puede distinguir entre mayúsculas y minúsculas; dependerá de cómo se controle el valor del atributo para propiedades determinadas. Por ejemplo, si el valor de atributo declara el nombre de un miembro de una enumeración, el comportamiento integrado que convierte el tipo de una cadena de nombre de miembro para devolver el valor de miembro de la enumeración no distingue entre mayúsculas y minúsculas. En cambio, el valor de la propiedad **Name** y los métodos de utilidades para trabajar con objetos basados en el nombre que declara la propiedad **Name** tratan la cadena de nombre como si distinguiera entre mayúsculas y minúsculas.

## <a name="xaml-namescopes"></a>Ámbitos de nombres XAML

El lenguaje XAML define un concepto de ámbito de nombres XAML. El concepto de ámbito de nombres XAML influye en la forma en que los procesadores XAML deben tratar el valor de **x:Name** o **Name** aplicado a elementos XAML, especialmente los ámbitos en los que se depende de que los nombres sean identificadores únicos. Los ámbitos de nombres XAML se tratan con más detalle en un tema aparte. Consulta [Ámbitos de nombres XAML](xaml-namescopes.md)

## <a name="the-role-of-xaml-in-the-development-process"></a>El rol de XAML en el proceso de desarrollo

XAML tiene varios roles importantes en el proceso de desarrollo de aplicaciones.

- XAML es el formato principal para declarar la interfaz de usuario de una aplicación y los elementos de dicha interfaz de usuario, si estás programando con C#, Visual Basic o C++/CX. Por lo general, al menos un archivo XAML del proyecto representa una metáfora de página en la aplicación para la interfaz de usuario que se muestra inicialmente. Otros archivos XAML podrían declarar otras páginas para la interfaz de usuario de navegación. Y otros archivos XAML pueden declarar recursos, como plantillas o estilos.
- El formato XAML se usa para declarar los estilos y las plantillas que se aplican a los controles y la interfaz de usuario de una aplicación.
- Podrías usar estilos y plantillas para aplicar plantillas a controles existentes, o si defines un control que suministra una plantilla predeterminada como parte de un paquete de controles. Cuando se usa para definir estilos y plantillas, el código XAML correspondiente se suele declarar como un archivo XAML discreto con una raíz <xref:Windows.UI.Xaml.ResourceDictionary>.
- XAML es el formato común que se usa para crear interfaces de usuario de aplicaciones e intercambiar el diseño de la interfaz de usuario entre diferentes aplicaciones de diseño. Lo más destacable es que el código XAML de la aplicación puede intercambiarse entre diferentes herramientas de diseño XAML (o ventanas de diseño de las herramientas).
- Existen otras tecnologías que también definen la interfaz de usuario básica en XAML. En relación con XAML de Windows Presentation Foundation (WPF) y XAML de Microsoft Silverlight, el código XAML para una aplicación de Windows Runtime usa el mismo URI para el espacio de nombres XAML predeterminado compartido. El vocabulario XAML de una aplicación de Windows Runtime coincide en gran medida con el vocabulario XAML para interfaz de usuario usado también por Silverlight y, en menor medida, por WPF. Por consiguiente, XAML ofrece una ruta de migración eficaz para las interfaces de usuario originalmente definidas para tecnologías precursoras que también usaban XAML.
- XAML define la apariencia visual de una interfaz de usuario, y un archivo de código subyacente asociado define la lógica. Puedes ajustar el diseño de la interfaz de usuario sin realizar cambios en la lógica, en el código subyacente. XAML simplifica el flujo de trabajo entre diseñadores y desarrolladores.
- Gracias a la extensa compatibilidad con diseñadores visuales y superficies de diseño del lenguaje XAML, XAML permite crear rápidamente prototipos de interfaces de usuario en las primeras fases de desarrollo.

En función de tu rol dentro del proceso de desarrollo, quizás no interactúes mucho con XAML. El grado de interacción con los archivos XAML también depende del entorno de desarrollo que uses, de si usas características de entorno de diseño interactivas, como cajas de herramientas y editores de propiedades, y del ámbito y la finalidad de tu aplicación de Windows en tiempo de ejecución. No obstante, es probable que durante el desarrollo de la aplicación edites un archivo XAML en el nivel de elemento, mediante un editor de texto o un editor XML. Con esta información, puedes editar código XAML con confianza en una representación de texto o XML y mantener la validez de las declaraciones de ese archivo XAML y su finalidad cuando sea usado por herramientas, operaciones de compilación de marcado o la fase de tiempo de ejecución de tu aplicación de Windows en tiempo de ejecución.

## <a name="optimize-your-xaml-for-load-performance"></a>Optimiza el rendimiento de la carga de tu código XAML

Estas son algunas sugerencias para definir los elementos de la interfaz de usuario en XAML siguiendo los procedimientos recomendados para rendimiento. Muchas de estas sugerencias hacen relación a cómo usar los recursos XAML, pero se incluyen en la descripción general de XAML por conveniencia. Para obtener más información sobre los recursos XAML, consulta [Referencias a ResourceDictionary y a recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md). Para ver algunos consejos más sobre el rendimiento, incluido XAML que demuestra deliberadamente algunas de las prácticas de bajo rendimiento que debes evitar en tu código XAML, consulta [Optimizar tu marcado XAML](../debug-test-perf/optimize-xaml-loading.md).

- Si usa el mismo pincel de color a menudo en el código XAML, defina un <xref:Windows.UI.Xaml.Media.SolidColorBrush> como un recurso en lugar de usar un color con nombre como valor de atributo cada vez.
- Si usa el mismo recurso en más de una página de la interfaz de usuario, considere la posibilidad de definirla en <xref:Windows.UI.Xaml.Application.Resources%2A> en lugar de en cada página. Por el contrario, si un recurso se usa solo en una página, no lo definas en **Application.Resources**; defínelo solo para la página que lo necesita. Esto es bueno para la factorización de XAML durante el diseño de tu aplicación y para el rendimiento durante el análisis de XAML.
- En el caso de los recursos que tu aplicación empaqueta, comprueba si hay recursos que no se usan (cuando un recurso tiene una clave pero en la aplicación no hay ninguna referencia [StaticResource](staticresource-markup-extension.md) que la use). Quítalos de tu código XAML antes de lanzar la aplicación.
- Si utiliza archivos XAML independientes que proporcionan recursos de diseño (<xref:Windows.UI.Xaml.ResourceDictionary.MergedDictionaries%2A>), considere la posibilidad de comentar o quitar recursos no usados de estos archivos. Incluso si tienes un punto de inicio de XAML compartido que uses en más de una aplicación o que ofrezca recursos comunes para todas tus aplicaciones, sigue siendo tu aplicación la que empaqueta los recursos XAML cada vez y que, potencialmente, tiene que cargarlos.
- No definas elementos de la interfaz de usuario que no necesites para la composición. Usa las plantillas de control predeterminadas siempre que sea posible (estas plantillas ya se han probado y comprobado para ver el rendimiento de la carga).
- Use contenedores como <xref:Windows.UI.Xaml.Controls.Border> en lugar de desdibujaciones deliberadas de los elementos de la interfaz de usuario. Básicamente, no dibujes el mismo píxel varias veces. Para obtener más información sobre cómo sobredibujar y cómo comprobarlo, vea <xref:Windows.UI.Xaml.DebugSettings.IsOverdrawHeatMapEnabled?displayProperty=nameWithType>.
- Usar las plantillas de elementos predeterminados para <xref:Windows.UI.Xaml.Controls.ListView> o <xref:Windows.UI.Xaml.Controls.GridView>; tienen una lógica especial del **presentador** que resuelve los problemas de rendimiento al compilar el árbol visual para un gran número de elementos de lista.

## <a name="debug-xaml"></a>Depurar XAML

Debido a que XAML es un lenguaje de marcado, no dispone de algunas de las estrategias típicas de depuración de Microsoft Visual Studio. Por ejemplo, no hay ninguna manera de establecer un punto de interrupción dentro de un archivo XAML. Sin embargo, hay otras técnicas que pueden ayudarte a depurar errores con las definiciones de la interfaz de usuario u otro marcado XAML mientras estás desarrollando tu aplicación.

Cuando hay problemas con un archivo XAML, lo más normal es que el sistema o tu aplicación inicie una excepción de análisis XAML. Siempre que se produce una excepción de análisis XAML, el código XAML cargado por el analizador XAML no pudo crear un árbol de objetos válido. En algunos casos, como cuando el código XAML representa la primera "página" de tu aplicación que se carga como elemento visual raíz, la excepción de análisis XAML es irrecuperable.

El código XAML suele editarse con un IDE como Visual Studio, y una de sus superficies de diseño XAML. Normalmente, Visual Studio puede proporcionar comprobación de errores y validación en tiempo de diseño de un código fuente XAML mientras lo estás editando. Por ejemplo, podría mostrar "garabatos" en el editor de texto XAML en cuanto escribas un valor de atributo incorrecto, y no tendrás que esperar a compilar el código XAML para ver que hay algo incorrecto en tu definición de la interfaz de usuario.

Cuando la aplicación se ejecuta realmente, si se han pasado por alto errores de análisis XAML en tiempo de diseño, estos aparecerán en el informe de Common Language Runtime (CLR) como [**XamlParseException**](https://docs.microsoft.com/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0). Para obtener más información sobre lo que podrías hacer con una **XamlParseException** en tiempo de ejecución, consulta [Control de excepciones para aplicaciones de Windows Runtime con C# o Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10)).

> [!NOTE]
> Las aplicaciones que C++usan/CX para el código no obtienen el [XamlParseException](https://docs.microsoft.com/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0)específico. pero el mensaje de la excepción aclara que el origen del error está relacionado con el código XAML e incluye información de contexto como los números de línea de un archivo XAML, igual que hace **XamlParseException**.

Para obtener más información sobre la depuración de una aplicación de Windows Runtime, consulta [Iniciar una sesión de depuración](/visualstudio/debugger/start-a-debugging-session-for-a-store-app-in-visual-studio-vb-csharp-cpp-and-xaml?view=vs-2015).

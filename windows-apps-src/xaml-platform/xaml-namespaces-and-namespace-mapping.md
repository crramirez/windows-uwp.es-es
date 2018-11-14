---
author: jwmsft
description: En este tema se explican las asignaciones de espacios de nombres XML/XAML (xmlns) tal y como se encuentran en el elemento raíz de la mayoría de los archivos XAML. También describe cómo producir asignaciones similares para tipos y ensamblados personalizados.
title: Espacios de nombres XAML y asignación de espacios de nombres
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1aebe3d9aac460d444a5dffcd63142300c022b7
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6448895"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>Espacios de nombres XAML y asignación de espacios de nombres


En este tema se explican las asignaciones de espacios de nombres XML/XAML (**xmlns**) tal y como se encuentran en el elemento raíz de la mayoría de los archivos XAML. También describe cómo producir asignaciones similares para tipos y ensamblados personalizados.

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>Cómo se relacionan los espacios de nombres XAML con la definición del código y las bibliotecas de tipos

Tanto en su propósito general como en su aplicación a la programación de aplicaciones de Windows en tiempo de ejecución, el lenguaje XAML sirve para declarar objetos, las propiedades de dichos objetos y las relaciones entre objetos y propiedades expresadas como jerarquías. Los objetos que declares en XAML están respaldados por bibliotecas de tipos u otras representaciones que se definen con otras técnicas y lenguajes de programación. Estas bibliotecas pueden ser:

-   El conjunto integrado de objetos de Windows en tiempo de ejecución. Esto es un conjunto fijo de objetos, y el acceso a estos objetos desde XAML usa lógica de activación y asignación de tipos interna.
-   Bibliotecas distribuidas, proporcionadas por Microsoft u otros proveedores.
-   Bibliotecas que representan la definición de un control de otro proveedor que tu aplicación incorpora y tu paquete redistribuye.
-   Tu propia biblioteca, que es parte de tu proyecto y que guarda todas tus definiciones de código de usuario o parte de ellas.

La información del tipo de respaldo se asocia a definiciones de espacios de nombres XAML determinadas. Los marcos de XAML, como Windows Runtime, pueden agregar varios ensamblados y espacios de nombres de código para asignarse a un único espacio de nombres XAML. Esto nos da el concepto de vocabulario XAML, que abarca un marco o tecnología de programación mayor. Un vocabulario XAML puede ser bastante amplio; por ejemplo, la mayoría del código XAML documentado para aplicaciones de Windows Runtime de esta referencia constituye un solo vocabulario XAML. Un vocabulario XAML también se puede ampliar; para ello, agrega tipos a las definiciones de código de respaldo asegurándote de incluir los tipos en espacios de nombres de código que ya se usan como orígenes de espacio de nombres asignados del vocabulario XAML.

Un procesador XAML puede consultar los tipos y los miembros en los ensamblados de respaldo asociados a ese espacio de nombres XAML cuando cree una representación de objetos en tiempo de ejecución. Por este motivo, XAML resulta útil como medio para formalizar e intercambiar definiciones de comportamiento de construcción de objetos, y se usa como técnica de definición de interfaz de usuario para una aplicación de UWP.

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>Espacios de nombres XAML en un uso de marcado XAML típico

Un archivo XAML casi siempre declara un espacio de nombres XAML predeterminado en su elemento raíz. El espacio de nombres XAML predeterminado define qué elementos se pueden declarar sin cualificarlos con un prefijo. Por ejemplo, si declaras un elemento `<Balloon />`, un analizador XAML esperará que exista un elemento **Balloon** y que sea válido en el espacio de nombres XAML predeterminado. Por el contrario, si **Balloon** no es el espacio de nombres XAML predeterminado definido, debes cualificar ese nombre de elemento con un prefijo como, por ejemplo, `<party:Balloon />`. El prefijo indica que el elemento existe en un espacio de nombres XAML que no es el predeterminado, y debes asignar un espacio de nombres XAML al prefijo **party** antes de poder usar este elemento. Los espacios de nombres XAML se aplican al elemento específico en el que se declaran, así como a cualquier elemento que esté contenido en ese elemento en la estructura XAML. Por este motivo, los espacios de nombres XAML casi siempre se declaran en elementos raíz de un archivo XAML para aprovechar las ventajas de la herencia.

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>Declaraciones de espacio de nombres predeterminado y del lenguaje XAML

En el elemento raíz de la mayoría de archivos XAML, hay dos declaraciones **xmlns**. La primera asigna un espacio de nombres XAML como predeterminado:  `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

Este es el mismo identificador de espacio de nombres XAML que se usaba en varias tecnologías de Microsoft anteriores que también usan XAML como formato de marcado para la definición de la interfaz de usuario. El uso del mismo identificador es deliberado y resulta útil cuando se migra una interfaz de usuario previamente definida a una aplicación de Windows en tiempo de ejecución con C++, C# o Visual Basic.

La segunda declaración asigna un espacio de nombres XAML diferente para los elementos del lenguaje definidos por XAML, y le asigna (normalmente) el prefijo "x":  `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

Este valor de **xmlns** y el prefijo x: al que está asignado también son idénticos a las definiciones que se usaban en varias tecnologías anteriores de Microsoft que usan XAML.

La relación entre estas declaraciones es que XAML es una definición de lenguaje, y Windows Runtime es una implementación que usa XAML como lenguaje y define un vocabulario específico a cuyos tipos se hace referencia en XAML.

El lenguaje XAML especifica determinados elementos de lenguaje, cada uno de los cuales debe ser accesible mediante implementaciones del procesador XAML que trabajan con el espacio de nombres XAML. Las plantillas de proyecto, el código de muestra y la documentación de las características del lenguaje siguen la convención por la que se asigna "x:" al espacio de nombres XAML del lenguaje XAML. El espacio de nombres del lenguaje XAML define varias características de uso común, incluso para aplicaciones básicas de Windows en tiempo de ejecución con C++, C# o Visual Basic. Por ejemplo, para unir un código subyacente con un archivo XAML mediante una clase parcial, debes asignar un nombre a esa clase como el [atributo x:Class](x-class-attribute.md) en el elemento raíz del archivo XAML correspondiente. O bien, cualquier elemento que se defina en una página XAML como un recurso con clave de un [ResourceDictionary y referencias a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273) debe tener establecido el [atributo x:Key](x-key-attribute.md) en el elemento de objeto en cuestión.

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>Otros espacios de nombres XAML

Además del espacio de nombres predeterminado y del espacio de nombres XAML del lenguaje XAML "x:", puedes ver otros espacios de nombres XAML asignados en el código XAML predeterminado inicial para aplicaciones generadas por Microsoft Visual Studio.

### **<a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>d: (`http://schemas.microsoft.com/expression/blend/2008`)**

El espacio de nombres XAML "d:" se usa para la compatibilidad con diseñadores, concretamente para la compatibilidad con diseñadores en las superficies de diseño de XAML de Microsoft Visual Studio. El espacio de nombres XAML "d:" permite el uso de atributos de diseñador o en tiempo de diseño en elementos XAML. Estos atributos de diseñador solo afectan a los aspectos de diseño relativos al comportamiento de XAML. Los atributos de diseñador se ignoran cuando el analizador XAML de Windows en tiempo de ejecución carga el mismo código XAML al ejecutarse una aplicación. Por lo general, los atributos de diseñador son válidos en cualquier elemento XAML pero, en la práctica, hay determinados escenarios en los que resulta apropiado aplicar uno mismo un atributo de diseñador. En particular, muchos de los atributos de diseñador están destinados a proporcionar una mejor experiencia de interacción con contextos de datos y orígenes de datos mientras desarrollas código XAML y otro código que use enlace de datos.

-   **Atributos d:DesignHeight y d:DesignWidth:** a veces, estos atributos se aplican a la raíz de un archivo XAML que Visual Studio u otra superficie de diseñador XAML crean automáticamente. Por ejemplo, estos atributos se establecen en la raíz de [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) del XAML que se crea al agregar un nuevo **UserControl** al proyecto de la aplicación. Estos atributos facilitan el diseño de la composición del contenido XAML, de forma que se pueden anticipar las limitaciones de contenido que podrían existir al usar el contenido XAML en una instancia de control o en otra parte de una página de interfaz de usuario mayor.

   **Nota**si migras el XAML desde Microsoft Silverlight estos atributos podrían encontrarse en los elementos raíz que representan una página de la interfaz de usuario completa. En ese caso, es posible que quieras quitar los atributos. Otras características de los diseñadores de XAML, como el simulador, son probablemente más útiles para crear diseños de página que controlen la escala y muestren los estados mejor que en un diseño de página de tamaño fijo con **d:DesignHeight** y **d:DesignWidth**.

-   **Atributo d:DataContext:** puedes establecer este atributo en una raíz de página o en un control para invalidar cualquier elemento [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) explícito o heredado que tenga el objeto.
-   **Atributo d:DesignSource:** especifica un origen de datos en tiempo de diseño para un elemento [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833), lo que invalida la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835).
-   **Extensiones de marcado d:DesignInstance y d:DesignData:** estas extensiones de marcado se usan para proporcionar recursos de datos en tiempo de diseño para **d:DataContext** o **d:DesignSource**. En este tema no explicaremos en detalle cómo usar los recursos de datos en tiempo de diseño. Para obtener más información, consulta [Atributos en tiempo de diseño](http://go.microsoft.com/fwlink/p/?LinkId=272504). Para obtener algunos ejemplos de uso, consulta [Datos de muestra sobre la superficie de diseño y para la creación de prototipos](https://msdn.microsoft.com/library/windows/apps/mt517866).

### **<a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

" mc:" indica y admite un modo de compatibilidad de marcado para leer XAML. Normalmente, el prefijo "d:" se asocia al atributo **mc:Ignorable**. Esta técnica permite a los analizadores XAML en tiempo de ejecución omitir los atributos de diseño en "d:".

### <a name="local-and-common"></a>**local:** y **common:**

"local:" es un prefijo que se suele asignar automáticamente en las páginas XAML a un proyecto de aplicación de uWP con plantilla. Se asigna para que haga referencia al mismo espacio de nombres que se ha creado para contener el [atributo x:Class](x-class-attribute.md) y el código de todos los archivos XAML, incluido app.xaml. Mientras definas cualquier clase personalizada que quieras usar en XAML en este mismo espacio de nombres, puedes usar el prefijo **local:** para referirte a tus tipos personalizados en XAML. Un prefijo relacionado que procede de un proyecto de aplicación de UWP con plantilla es **common:**. Este prefijo hace referencia a un espacio de nombres "Common" anidado que contiene clases de utilidad como conversores y comandos; encontrarás las definiciones en la carpeta Common de la vista **Explorador de soluciones**.

### **<a name="vsm"></a>vsm:**

No los uses. "vsm:" es un prefijo que a veces se ve en plantillas XAML más antiguas importadas de otras tecnologías de Microsoft. El espacio de nombres en principio solucionaba un problema de utillaje de los espacios de nombres heredados. Deberías eliminar las definiciones de espacios de nombres XAML de "vsm:" en el código XAML que uses para Windows Runtime, y cambiar el uso de prefijos de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) y otros objetos relacionados para usar en su lugar el espacio de nombres XAML predeterminado. Para obtener más información sobre la migración de XAML, consulta [Migrar código XAML de WPF o Silverlight a una aplicación de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/br229571).

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>Asignación de tipos personalizados a espacios de nombres XAML y prefijos

Puedes asignar un espacio de nombres XAML con el fin de usar XAML para acceder a tus propios tipos personalizados. En otras palabras, asignas un espacio de nombres de código tal y como existe en una representación de código que define el tipo personalizado, y le asignas un espacio de nombres XAML con un prefijo de uso. Los tipos personalizados para XAML pueden definirse con un lenguaje de Microsoft .NET (C# o Microsoft Visual Basic) o en C++. La asignación se realiza mediante la definición de un prefijo **xmlns**. Por ejemplo, `xmlns:myTypes` define un nuevo espacio de nombres XAML al que se accede incluyendo prefijos en todos los usos con el token `myTypes:`.

Una definición de **xmlns** incluye un valor, así como el nombre del prefijo. El valor es una cadena entrecomillada y seguida del signo igual. Una convención habitual en XML es asociar el espacio de nombres XML con un Identificador uniforme de recursos (URI), de manera que hay una convención para la exclusividad y la identificación. Esta convención también se observa en el espacio de nombres XAML predeterminado y en el espacio de nombres XAML del lenguaje XAML, así como en algunos espacios de nombres XAML de uso menos frecuente usados por el lenguaje XAML de Windows en tiempo de ejecución. Sin embargo, en el caso de un espacio de nombres XAML que asigna tipos personalizados, en lugar de especificar un URI, la definición del prefijo comienza por el token "using:". Después del token "using:", se asigna un nombre al espacio de nombres de código.

Por ejemplo, para asignar un prefijo "custom1" que permita hacer referencia a un espacio de nombres "CustomClasses" y usar las clases de dicho espacio de nombres o ensamblado como elementos de objeto en XAML, la página XAML debe incluir la siguiente asignación en el elemento raíz:  `xmlns:custom1="using:CustomClasses"`

No es necesario asignar las clases parciales del mismo ámbito de página. Por ejemplo, no necesitas prefijos para hacer referencia a los controladores de eventos que has definido para controlar los eventos de la definición de la interfaz de usuario XAML de tu página. Además, muchas de las páginas XAML iniciales precedentes de proyectos generados en Visual Studio para una aplicación de Windows Runtime con C++, C# o Visual Basic ya asignan un prefijo "local:" que hace referencia al espacio de nombres predeterminado especificado por el proyecto y el espacio de nombres usado por las definiciones de clases parciales.

### <a name="clr-language-rules"></a>Reglas del lenguaje CLR

Si escribes tu código de respaldo en un lenguaje de .NET (C# o Microsoft Visual Basic), es posible que estés usando convenciones que incluyen un punto (".") como parte del nombre de los espacios de nombres para crear una jerarquía conceptual de espacios de nombres de código. Si tu definición de espacio de nombres contiene un punto, este debe formar parte del valor que especifiques después del token "using:".

Si el archivo de código subyacente o el archivo de definiciones de código es un archivo de C++, existen ciertas convenciones que aún siguen el formato del lenguaje Common Language Runtime (CLR) para que no haya diferencias en la sintaxis de XAML. Si declaras espacios de nombres anidados en C++, el separador entre las sucesivas cadenas de espacios de nombres anidados debe ser "." en lugar de "::" cuando especifiques el valor que sigue al token "using:".

No uses tipos anidados (como es el caso de anidar una enumeración en una clase) cuando definas el código para usarlo con XAML. Los tipos anidados no se pueden evaluar. El analizador de XAML no tiene forma de distinguir que un punto forma parte del nombre de un tipo anidado, y no del nombre del espacio de nombres.

## <a name="custom-types-and-assemblies"></a>Tipos y ensamblados personalizados

En la asignación no se especifica el nombre del ensamblado que define los tipos de respaldo de un espacio de nombres XAML. La lógica por la que los ensamblados están disponibles se controla en el nivel de definición de la aplicación y forma parte de los principios básicos de implementación y seguridad de una aplicación. Declara los ensamblados que quieras incluir como origen de definiciones de código para XAML como ensamblados dependientes en la configuración del proyecto. Para obtener más información, consulta [Creación de componentes de Windows en tiempo de ejecución en C# y Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx).

Si haces referencia a tipos personalizados en la definición de aplicación o en las definiciones de página de la aplicación principal, esos tipos están disponibles sin necesidad de otra configuración de ensamblado dependiente, aunque aún debes asignar el espacio de nombres de código que contiene esos tipos. Una convención habitual es asignar el prefijo "local" al espacio de nombres de código predeterminado de cualquier página XAML dada. Esta convención se suele incluir en las plantillas de proyecto de inicio para proyectos de XAML.

## <a name="attached-properties"></a>Propiedades adjuntas

Si haces referencia a propiedades adjuntas, el fragmento del tipo de propietario del nombre de la propiedad adjunta debe estar en el espacio de nombres XAML predeterminado, o bien incluir un prefijo. Incluir prefijos en atributos con independencia de sus elementos no es muy habitual, pero en este caso a veces es necesario, sobre todo si se trata de una propiedad adjunta personalizada. Para obtener más información, consulta [Propiedades adjuntas personalizadas](custom-attached-properties.md).

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Guía de sintaxis XAML](xaml-syntax-guide.md)
* [Creación de componentes de Windows Runtime en C# y Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Plantillas de proyecto C#, VB y C++ para aplicaciones de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Migrar de Silverlight o código XAML en WPF a una aplicación de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/br229571)
 


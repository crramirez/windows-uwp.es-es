---
author: jwmsft
description: En este tema se explica el sistema de propiedades de dependencia de que dispones cuando escribes una aplicación de Windows Runtime con C++, C#, o Visual Basic junto con definiciones XAML para la interfaz de usuario.
title: Introducción a las propiedades de dependencia
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fbf6e7ee8a224a6957428fddd11a2922adbecf4
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7161251"
---
# <a name="dependency-properties-overview"></a>Introducción a las propiedades de dependencia

En este tema se explica el sistema de propiedades de dependencia de que dispones cuando escribes una aplicación de Windows Runtime con C++, C#, o Visual Basic junto con definiciones XAML para la interfaz de usuario.

## <a name="what-is-a-dependency-property"></a>¿Qué es una propiedad de dependencia?

Una propiedad de dependencia es un tipo especializado de propiedad. Se trata de una propiedad para la cual un sistema de propiedades dedicado que forma parte de Windows Runtime hace un seguimiento de su valor e influye en él.

Para poder admitir una propiedad de dependencia, el objeto que define la propiedad debe ser un [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) (dicho de otro modo, una clase que tenga la clase base **DependencyObject** en algún lugar de su herencia). Muchos de los tipos que usas para las definiciones de interfaz de usuario para una aplicación para UWP con XAML serán una subclase **DependencyObject** y admitirán propiedades de dependencia. Pero cualquier tipo que provenga de un espacio de nombres de Windows Runtime que no tenga "XAML" en su nombre no admitirá propiedades de dependencia. Las propiedades de estos tipos son propiedades normales que no sufrirán el comportamiento de dependencia de un sistema de propiedades.

El propósito de las propiedades de dependencia es proporcionar un modo sistémico de calcular el valor de una propiedad según otras entradas (otras propiedades, eventos y estados que se produzcan en tu aplicación mientras se ejecuta). Estas otras entradas podrían ser:

- Entradas externas, como una preferencia del usuario
- Mecanismos de determinación de propiedades oportunos, como enlace de datos, animaciones y guiones gráficos
- Plantillas de usos múltiples como recursos y estilos
- Valores que se conocen a través de relaciones de elementos principales y secundarios con otros elementos del árbol de objetos

Una propiedad de dependencia representa o admite una característica específica del modelo de programación para definir una aplicación de Windows Runtime con XAML para las extensiones de componentes de la interfaz de usuario y C#, Microsoft Visual Basic o VisualC ++ (C++ / CX) para el código. Entre estas funciones se incluyen:

- Enlace de datos
- Estilos
- Animaciones con guion gráfico
- Comportamiento "PropertyChanged"; se puede implementar una propiedad de dependencia para proporcionar devoluciones de llamadas que puedan propagar cambios a otras propiedades de dependencia
- Uso de un valor predeterminado que proviene de los metadatos de la propiedad
- Utilidad del sistema de propiedades general como [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) y búsqueda de metadatos

## <a name="dependency-properties-and-windows-runtime-properties"></a>Propiedades de dependencia y propiedades de Windows Runtime

Las propiedades de dependencia extienden la funcionalidad de propiedades básica de Windows Runtime. Para ello, ofrecen un almacén de propiedades global e interno que incluye todas las propiedades de dependencia de una aplicación en tiempo de ejecución. Es una alternativa al patrón estándar de respaldo de una propiedad con un campo privado que es privado en la clase de definición de la propiedad. Este almacén de propiedades interno es un conjunto de identificadores y valores de propiedades que existen para cualquier objeto en particular (siempre que sea un [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)). En lugar de identificarse por el nombre, cada propiedad del almacén se identifica mediante una instancia de [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362). Pero el sistema de propiedades oculta en su mayoría los detalles de esta implementación: las propiedades de dependencia suelen ser accesibles mediante el uso de un nombre simple: el nombre de la propiedad en el lenguaje de código que uses o el nombre de un atributo mientras escribes XAML.

El tipo base que ofrece la base del sistema de propiedades de dependencia es [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). **DependencyObject** define métodos que pueden acceder a la propiedad de dependencia, y las instancias de una clase derivada de **DependencyObject** admiten internamente el concepto de almacén de propiedades que mencionamos anteriormente.

Este es un resumen de la terminología que usamos en esta documentación cuando se tratan las propiedades de dependencia:

| Término | Descripción |
|------|-------------|
| Propiedad de dependencia | Una propiedad que existe en un identificador [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) (consulta más adelante). Generalmente, este identificador se encuentra disponible como miembro estático de la clase derivada **DependencyObject** de definición. |
| Identificador de propiedad de dependencia | Un valor constante para identificar la propiedad; normalmente es público y de solo lectura. |
| Contenedor de propiedades | Las implementaciones **get** y **set** que se pueden llamar de una propiedad de Windows Runtime. O bien, la proyección específica del lenguaje de la definición original. Una implementación del contenedor de propiedades **get** llama a [**GetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.getvalue.aspx) y pasa el identificador de la propiedad de dependencia correspondiente. |

El contenedor de propiedades no solo es práctico para los llamadores, sino que también expone la propiedad de dependencia a cualquier proceso, herramienta o proyección que use definiciones de Windows Runtime para propiedades.

El siguiente ejemplo define una propiedad de dependencia personalizada "IsSpinning" definida para C# y muestra la relación del identificador de propiedad de dependencia con el contenedor de propiedades.

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty =
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

> [!NOTE]
> El ejemplo anterior no se ha diseñado como el ejemplo completo de cómo crear una propiedad de dependencia personalizada. Pretende mostrar conceptos de propiedades de dependencia para cualquiera que prefiera aprender conceptos mediante código. Para obtener un ejemplo más completo, consulta [Propiedades de dependencia personalizadas](custom-dependency-properties.md).

## <a name="dependency-property-value-precedence"></a>Prioridad de valores de la propiedad de dependencia

Cuando obtienes el valor de una propiedad de dependencia, obtienes un valor que se determinó en esa propiedad a través de una de las entradas que participan en el sistema de propiedades de Windows Runtime. La prioridad de valores de la propiedad de dependencia existe para que el sistema de propiedades de Windows Runtime pueda calcular valores de un modo predecible, y es importante que estés familiarizado con el orden de prioridad básico. De lo contrario, podría ocurrir que intentaras establecer una propiedad en un nivel de prioridad, pero algo (el sistema, llamadores de terceros, tu propio código) la configurara en otro nivel, por lo que llegarías a frustrarte intentando averiguar qué valor de propiedad se usa y de dónde procede.

Por ejemplo, el propósito de los estilos y las plantillas es ser un punto de inicio compartido para establecer los valores de las propiedades y, por consiguiente, las apariencias de un control. Pero es posible que en la instancia de un control en particular quieras cambiar su valor con respecto al valor en la plantilla común, por ejemplo, dando al control un color de fondo diferente o una cadena de texto diferente como contenido. El sistema de propiedades de Windows Runtime considera los valores locales con una prioridad mayor que los valores que proporcionan los estilos y las plantillas. Esto permite el escenario de hacer que los valores específicos de una aplicación sobrescriban las plantillas, para que los controles sean útiles para que los utilices personalmente en la interfaz de usuario de la aplicación.

### <a name="dependency-property-precedence-list"></a>Lista de prioridades de las propiedades de dependencia

El siguiente es el orden definitivo que el sistema de propiedades usa para asignar el valor de tiempo de ejecución de una propiedad de dependencia. La prioridad mayor es la primera de la lista. Al final de esta lista, encontrarás explicaciones más detalladas.

1. **Valores animados:** animaciones activas, animaciones de estado visual o animaciones con un comportamiento [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306). Para lograr un efecto práctico, una animación aplicada a una propiedad debe tener prioridad sobre el valor base (no animado), aunque ese valor se haya establecido localmente.
1. **Valor local:** un valor local podría establecerse a través del conveniente contenedor de propiedades, que también equivale a establecer como atributo o un elemento de propiedad en XAML, o bien al llamar al método [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) mediante la propiedad de una instancia específica. Si estableces un valor local con un enlace o un recurso estático, cada uno de ellos actúa en el orden de prioridad como si se estableciera un valor local, y los enlaces y referencias a recursos se borran si se establece un valor local nuevo.
1. **Propiedades con plantilla:** un elemento las tiene si se creó como parte de una plantilla (a partir de una clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) o [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)).
1. **Establecedores de estilo:** valores de una clase [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) incluidos en los estilos de los recursos de la página o la aplicación.
1. **Valor predeterminado:** una propiedad de dependencia puede tener un valor predeterminado como parte de sus metadatos.

### <a name="templated-properties"></a>Propiedades con plantilla

Las propiedades con plantilla como elemento de prioridad no se aplican a las propiedades de un elemento que declares directamente en el marcado de página XAML. El concepto de propiedad con plantilla existe solamente para los objetos que se crean cuando Windows Runtime aplica una plantilla XAML a un elemento de la interfaz para definir sus vistas.

Todas las propiedades configuradas desde una plantilla de control tienen valores de algún tipo. Estos valores son prácticamente como un conjunto extendido de valores predeterminados para el control y a menudo se asocian con valores que pueden restablecerse posteriormente configurando los valores de la propiedad directamente. Por lo tanto, los valores de conjunto de plantilla deben ser diferenciables de un valor local verdadero, de tal modo que cualquier valor local nuevo pueda sobrescribirlo.

> [!NOTE]
> En algunos casos, la plantilla podría reemplazar incluso valores locales si la plantilla no puede exponer referencias de la [extensión de marcado {TemplateBinding}](templatebinding-markup-extension.md) para propiedades que deberían haberse configurado en instancias. Esto se suele hacer solamente si la propiedad no está realmente pensada para su configuración en instancias, por ejemplo, si es solo relevante para el comportamiento de plantillas y vistas, y no para la función o la lógica en tiempo de ejecución pretendida del control que usa la plantilla.

### <a name="bindings-and-precedence"></a>Enlaces y prioridad

Las operaciones de enlace tienen la prioridad apropiada para cualquier ámbito para el que se usen. Por ejemplo, una [extensión de marcado {Binding}](binding-markup-extension.md) aplicada a un valor local actúa como valor local, y una [extensión de marcado {TemplateBinding}](templatebinding-markup-extension.md) para un establecedor de propiedades se aplica igual que un establecedor de estilo. Como los enlaces deben esperar hasta el tiempo de ejecución para poder obtener valores de orígenes de datos, el proceso de determinación de la prioridad de los valores de cualquier propiedad se extiende también hasta el tiempo de ejecución.

Los enlaces no solo operan con la misma prioridad que un valor local, realmente son un valor local, donde el enlace es un marcador de posición para un valor que está aplazado. Si tienes un enlace vigente para un valor de propiedad, y estableces un valor local en tiempo de ejecución, este reemplazará todo el enlace. De forma similar, si llamas a [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) para definir un enlace que solo existe en tiempo de ejecución, estás reemplazando cualquier valor local que hayas aplicado en XAML o con código ejecutado anteriormente.

### <a name="storyboarded-animations-and-base-value"></a>Animaciones con guion gráfico y valor base

Las animaciones con guión gráfico actúan en un concepto de un *valor base*. El valor base es el valor que viene determinado por el sistema de propiedades mediante el uso de la prioridad, pero omite el último paso de buscar animaciones. Por ejemplo, un valor base puede venir de una plantilla de control, o puede venir de la configuración de un valor local en una instancia de un control. De cualquier forma, la aplicación de una animación sobrescribirá este valor base y aplicará el valor animado mientras la animación siga ejecutándose.

En una propiedad animada, el valor base puede tener un efecto sobre el comportamiento de la animación, si esa animación no especifica de forma explícita **From** y **To**, o si la animación revierte la propiedad a su valor base cuando se completa. En estos casos, cuando la animación deja de ejecutarse, el resto de la prioridad se usa de nuevo.

No obstante, una animación que especifica un valor **To** con un comportamiento [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) puede anular un valor local hasta que se elimine la animación, aun cuando visualmente parezca que se detuvo. Conceptualmente, esto es como una animación que se ejecuta para siempre, incluso cuando no hay ninguna animación visual en la interfaz.

Se pueden aplicar varias animaciones a una sola propiedad. Cada una de estas animaciones puede estar definida para sustituir valores base que vienen de diferentes puntos en la prioridad de valores. Pero estas animaciones se ejecutarán todas a la vez en tiempo de ejecución, y esto suele significar que deben combinar sus valores porque cada animación tiene la misma influencia en el valor. Esto depende exactamente de la cantidad de animaciones que se definan y del tipo de valor que se anime.

Para obtener más información, consulta [Animaciones con guion gráfico](https://msdn.microsoft.com/library/windows/apps/mt187354).

### <a name="default-values"></a>Valores predeterminados

El establecimiento del valor predeterminado para una propiedad de dependencia con un valor [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) se explica más detalladamente en el tema [Propiedades de dependencia personalizadas](custom-dependency-properties.md).

Las propiedades de dependencia siguen teniendo valores predeterminados, incluso aunque esos valores predeterminados no se definieran en los metadatos de esa propiedad. A menos que los metadatos los hayan cambiado, los valores predeterminados de las propiedades de dependencia de Windows Runtime suelen ser alguno de los siguientes:

- Una propiedad que use un objeto de tiempo de ejecución o el tipo **Object** básico (un *tipo de referencia*) tiene un valor predeterminado de **null**. Por ejemplo, [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) es **null** hasta que se establezca o se herede de forma deliberada.
- Una propiedad que use un valor básico como números o un valor booleano (un *tipo de valor*) usa un valor predeterminado esperado. Por ejemplo, 0 para números enteros y de punto flotante, y **false** para un valor booleano.
- Una propiedad que use una estructura de Windows Runtime tiene un valor predeterminado que se obtiene de llamar al constructor predeterminado implícito de esa estructura. Este constructor usa los valores predeterminados de cada campo de valor básico de la estructura. Por ejemplo, un valor predeterminado de un valor [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) se inicializa con sus valores **X** e **Y** en 0.
- Una propiedad que use una enumeración tiene un valor predeterminado del primer miembro definido de esa enumeración. Comprueba la referencia de las enumeraciones específicas para ver el valor predeterminado.
- Una propiedad que use una cadena ([**System.String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) para .NET, [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) para C++/CX) tiene un valor predeterminado de una cadena vacía (**""**).
- Las propiedades de colección normalmente se implementan como propiedades de dependencia, por razones que se tratan más a fondo en este tema. Pero si implementas una propiedad de colección personalizada y quieres que sea una propiedad de dependencia, asegúrate de que evitas que se produzca un *singleton no intencionado* como se describe hacia el final del tema [Propiedades de dependencia personalizadas](custom-dependency-properties.md).

## <a name="property-functionality-provided-by-a-dependency-property"></a>Funcionalidad de la propiedad suministrada por una propiedad de dependencia

### <a name="data-binding"></a>Enlace de datos

Se puede establecer el valor de una propiedad de dependencia mediante la aplicación de un enlace de datos. El enlace de datos usa la sintaxis de [extensión de marcado {Binding}](binding-markup-extension.md) en XAML, la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) o la clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) en el código. Para una propiedad de enlace de datos, la determinación del valor de la propiedad final se aplaza hasta el tiempo de ejecución. En ese momento, el valor se obtiene de un origen de datos. El papel que el sistema de propiedades de dependencia juega aquí es habilitar un comportamiento de marcador de posición para operaciones como cargar XAML cuando el valor aún no es conocido y, luego, suministrar el valor en tiempo de ejecución mediante la interacción con el motor de enlace de datos de Windows Runtime.

En el siguiente ejemplo se establece el valor [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) de un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) mediante un enlace en XAML. El enlace usa un contexto de datos heredados y un origen de datos de objeto. (Ninguno de ellos se muestra en el ejemplo abreviado; para obtener una muestra más completa que muestre el contexto y el código fuente, consulta [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)).

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

También puedes establecer enlaces con códigos y no con XAML. Consulta [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257).

> [!NOTE]
> Los enlaces como este se tratan como un valor local para fines de prioridad de valores de propiedad de dependencia. Si estableces otro valor local para una propiedad que originalmente tenía un valor [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820), sobrescribirás el enlace por completo, no solo el valor en tiempo de ejecución del enlace. Los enlaces {x: Bind} se implementan mediante código generado que establecerá un valor local para la propiedad. Si estableces un valor local para una propiedad que está usando {x: Bind}, ese valor se reemplazará la próxima vez que se evalúe el enlace, por ejemplo, cuando observe que una propiedad cambia en su objeto de origen.

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>Orígenes de enlace, destinos de enlace, el rol de FrameworkElement

Para ser el origen de un enlace, no es necesario que una propiedad sea de dependencia; por lo general, puedes usar cualquier propiedad como origen de enlace, aunque esto depende del lenguaje de programación y cada uno tiene sus particularidades. Sin embargo, para ser el destino de una [extensión de marcado {Binding}](binding-markup-extension.md) o de una clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820), la propiedad debe ser de dependencia. {x: Bind} no tiene este requisito, ya que usa código generado para aplicar sus valores de enlace.

Si estás creando un enlace en código, ten en cuenta que la API [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) solo se define para [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). No obstante, puedes crear una definición de enlace usando [**BindingOperations**](https://msdn.microsoft.com/library/windows/apps/br209823) en su lugar y, por lo tanto, que haga referencia a cualquier propiedad [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356).

Tanto en código como en XAML, recuerda que [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) es una propiedad [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). Mediante el uso de una forma de herencia de propiedad principal/secundaria (normalmente establecida en marcado XAML), el sistema de enlace puede resolver un **DataContext** que exista en un elemento principal. Esta herencia puede evaluar incluso si el objeto secundario (que tiene la propiedad de destino) no es un **FrameworkElement** y, por lo tanto, no guarda su propio valor de **DataContext**. Pero el elemento principal que se hereda debe ser un **FrameworkElement** para poder establecer y guardar el **DataContext**. Otra opción es definir el enlace de manera tal que pueda funcionar con un valor **null** para **DataContext**.

Conectar el enlace no es lo único que se necesita en la mayoría de casos de enlace de datos. Para que el enlace unidireccional o bidireccional sea efectivo, la propiedad de origen debe admitir notificaciones de cambio que se propaguen al sistema de enlace y, por consiguiente, al destino. En el caso de orígenes de enlace personalizados, esto significa que la propiedad debe ser una propiedad de dependencia o el objeto debe admitir [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx). Las colecciones deben admitir [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Ciertas clases admiten estas interfaces en sus implementaciones para que sean útiles como clases base en escenarios de enlace de datos; un ejemplo de dicha clase es [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx). Para obtener más información sobre el enlace de datos y la forma en que se relaciona con el sistema de propiedades, consulta [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

> [!NOTE]
> Los orígenes de datos de Microsoft .NET de tipos que se enumeran aquí admiten. Los orígenes de datos de C++/CX usan interfaces diferentes para las notificaciones de cambios o un comportamiento observable. Consulta la sección [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

### <a name="styles-and-templates"></a>Estilos y plantillas

Los estilos y las plantillas son dos de los escenarios en los que las propiedades se definen como propiedades de dependencia. Los estilos son útiles para establecer propiedades que definen la interfaz de usuario de la aplicación. Los estilos se definen como recursos en XAML, ya sea como una entrada en una colección de [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) o en archivos XAML independientes, como los diccionarios de recursos de tema. Los estilos interactúan con el sistema de propiedades porque contienen establecedores para propiedades. La propiedad más importante que se establece de esta manera es la propiedad [**Control.Template**](https://msdn.microsoft.com/library/windows/apps/br209465) de un [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390), que define la mayor parte de la apariencia visual y del estado visual de un **Control**. Para obtener más información sobre los estilos y algún ejemplo de XAML que define un [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) y usa establecedores, consulta [Inicio rápido: controles de estilo](https://msdn.microsoft.com/library/windows/apps/mt210950).

Los valores que provienen de estilos o plantillas son valores aplazados, similares a los enlaces. Esto sirve para que los usuarios de controles puedan volver a crear plantillas de los controles o redefinir estilos. Y es por eso que los establecedores de propiedades en los estilos solo pueden actuar en propiedades de dependencia, no en propiedades normales.

### <a name="storyboarded-animations"></a>Animaciones con guion gráfico

Puedes animar el valor de una propiedad de dependencia con una animación con guion gráfico. Las animaciones con guion gráfico de Windows Runtime no son simples decoraciones visuales. Es más útil pensar en las animaciones como una técnica de máquina de estado que puede establecer los valores de propiedades individuales o de todas las propiedades y vistas de un control, y cambiar estos valores con el tiempo.

Para que pueda animarse, la propiedad de destino de la animación debe ser una propiedad de dependencia. Además, para que sea animado, el tipo de valor de la propiedad de destino debe ser compatible con uno de los tipos de animación derivados de [**Timeline**](https://msdn.microsoft.com/library/windows/apps/br210517) existentes. Los valores de [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723), [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) y [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) se pueden animar con técnicas de interpolación o de fotograma clave. El resto de valores, en su mayoría, se pueden animar con fotogramas clave de **Object** discretos.

Cuando se aplica una animación y está en funcionamiento, el valor animado opera con una prioridad mayor que cualquier otro valor (como un valor local) que la propiedad tenga. Las animaciones también tienen un comportamiento [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) opcional que puede hacer las que las animaciones se apliquen a valores de propiedad aún si visualmente parece que la animación se detuvo.

El principio de máquina de estado se expresa mediante el uso de animaciones con guion gráfico como parte del modelo de estado [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021) de los controles. Para obtener más información sobre animaciones con guion gráfico, consulta [Animaciones con guion gráfico](https://msdn.microsoft.com/library/windows/apps/mt187354). Para obtener más información sobre **VisualStateManager** y definir estados visuales para los controles, consulta [Animaciones con guion gráfico para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808) o [Plantillas de control](../design/controls-and-patterns/control-templates.md).

### <a name="property-changed-behavior"></a>Comportamiento modificado por la propiedad

El comportamiento modificado por la propiedad es uno de los principales motivos de la parte "de dependencia" del término propiedad de dependencia. Mantener valores válidos para una propiedad cuando otra propiedad puede influir en el valor de la primera es un problema de desarrollo difícil en muchas plataformas. En el sistema de propiedades de Windows Runtime, cada propiedad de dependencia puede especificar que se invoque una devolución de llamada siempre que cambie el valor de su propiedad. Esta devolución de llamada se puede usar para notificar o cambiar los valores relacionados con la propiedad de una manera generalmente sincrónica. Muchas propiedades de dependencia existentes tienen un comportamiento modificado por la propiedad. También puedes agregar un comportamiento de devolución de llamada similar a las propiedades de dependencia personalizadas e implementar tus propias devoluciones de llamadas modificadas por la propiedad. Consulta un ejemplo en [Propiedades de dependencia personalizadas](custom-dependency-properties.md).

Windows 10 incorpora el método [**RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx). Esto permite que el código de la aplicación registre notificaciones de cambio cuando se cambia la propiedad de dependencia especificada en una instancia de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.aspx).

### <a name="default-value-and-clearvalue"></a>Valor predeterminado y **ClearValue**

Una propiedad de dependencia puede tener un valor predeterminado definido como parte de los metadatos de la propiedad. En el caso de una propiedad de dependencia, su valor predeterminado no se convierte en irrelevante después de establecer la propiedad por primera vez. El valor predeterminado se puede aplicar de nuevo en tiempo de ejecución siempre que desaparezca algún otro determinante en la prioridad de valores. (La prioridad de valores de la propiedad de dependencia se analiza en la siguiente sección). Por ejemplo, podrías quitar deliberadamente un valor de estilo o una animación que se aplique a una propiedad, pero desear que, después de hacerlo, el valor sea un valor predeterminado razonable. El valor predeterminado de la propiedad de dependencia puede proporcionar este valor, sin necesidad de establecer específicamente cada valor de la propiedad como paso adicional.

Puedes establecer deliberadamente una propiedad en el valor predeterminado incluso después de establecerla con un valor local. Para restablecer el valor predeterminado y para que otros participantes anteriores puedan invalidar el valor predeterminado pero no un valor local, llama al método [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) (referencia a la propiedad para borrar como parámetro de método). No siempre querrás que la propiedad use literalmente el valor predeterminado, pero borrar el valor local y revertir al valor predeterminado puede habilitar otro elemento anterior que quieras que actúe ahora, como usar el valor que venía de un establecedor de estilo en una plantilla de control.

## <a name="dependencyobject-and-threading"></a>**DependencyObject** y subprocesos

Todas las instancias de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) deben crearse en el subproceso de interfaz de usuario asociado a [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) actual que muestra la aplicación de Windows Runtime. Aunque cada **DependencyObject** debe crearse en el subproceso de interfaz de usuario principal, se puede acceder a los objetos mediante una referencia de distribuidor desde otros subprocesos, accediendo a la propiedad [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616). Posteriormente se puede llamar a métodos como [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) en el objeto [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) y ejecutar el código dentro de las reglas de restricción de subprocesos en el subproceso de la interfaz de usuario.

Los aspectos de subprocesos de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) son relevantes porque, por lo general, significa que solo el código que se ejecuta en el subproceso de interfaz de usuario puede cambiar o incluso leer el valor de una propiedad de dependencias. Los problemas de subprocesos normalmente se pueden evitar en el código típico de la interfaz de usuario que haga un uso correcto de los patrones **async** y de los subprocesos de trabajo en segundo plano. Normalmente, solo te encontrarás con problemas de subprocesos relacionados con **DependencyObject** si vas a definir tipos de **DependencyObject** e intentas usarlos para los orígenes de datos u otros escenarios donde un **DependencyObject** no es necesariamente lo adecuado.

## <a name="related-topics"></a>Temas relacionados

### <a name="conceptual-material"></a>Material conceptual

- [Propiedades de dependencia personalizadas](custom-dependency-properties.md)
- [Introducción a las propiedades adjuntas](attached-properties-overview.md)
- [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)
- [Animaciones con guion gráfico](https://msdn.microsoft.com/library/windows/apps/mt187354)
- [Creación de componentes de Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
- [Muestra de controles de usuario y controles personalizados de XAML](http://go.microsoft.com/fwlink/p/?linkid=238581)

## <a name="apis-related-to-dependency-properties"></a>API relacionadas con las propiedades de dependencia

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)


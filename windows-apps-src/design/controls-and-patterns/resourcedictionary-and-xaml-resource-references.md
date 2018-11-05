---
author: Jwmsft
Description: Explains how to define a ResourceDictionary element and keyed resources, and how XAML resources relate to other resources that you define as part of your app or app package.
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referencias a recursos de ResourceDictionary y XAML
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8b5d2a55610b6cec2f9026a5834b00ad7015a9c6
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032367"
---
# <a name="resourcedictionary-and-xaml-resource-references"></a>Referencias a ResourceDictionary y a los recursos XAML

 

Puedes definir la interfaz de usuario o los recursos de la aplicación con XAML. Los recursos suelen ser definiciones de algún objeto que esperas usar más de una vez. Para consultar un recurso XAML más adelante, debes especificar una clave de un recurso que actúe como su nombre. Puedes hacer referencia a un recurso a través de una aplicación o desde cualquier página XAML de esta. Puedes definir los recursos con un elemento [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) desde el XAML de Windows Runtime. Después, puedes hacer referencia a tus recursos mediante una [extensión de marcado StaticResource](../../xaml-platform/staticresource-markup-extension.md) o una [extensión de marcado ThemeResource](../../xaml-platform/themeresource-markup-extension.md).

Los elementos XAML que quizás quieras declarar con más frecuencia como recursos XAML son [Style](https://msdn.microsoft.com/library/windows/apps/br208849), [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391), componentes de animación y subclases [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush). Aquí se explica cómo definir un elemento [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) y recursos con clave, y cómo los recursos XAML se relacionan con otros recursos que definas como parte de tu aplicación o paquete de la aplicación. También se explican las características avanzadas del diccionario de recursos, como [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) y [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807).

**Requisitos previos**

Damos por hecho que conoces el marcado XAML y que has leído la [introducción a XAML](https://msdn.microsoft.com/library/windows/apps/mt185595).

## <a name="define-and-use-xaml-resources"></a>Definir y usar recursos de XAML

Los recursos XAML son objetos a los que se hace referencia desde el marcado más de una vez. Los recursos se definen en un objeto [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), normalmente en un archivo independiente o en la parte superior de la página de marcado, como esta.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

En este ejemplo:

-   `<Page.Resources>…</Page.Resources>` - Define el diccionario de recursos.
-   `<x:String>` - Define el recurso con la clave "greeting".
-   `{StaticResource greeting}` - Busca el recurso con la clave "greeting", que se asigna a la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/br209676) del objeto [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652).

> **Nota**&nbsp;&nbsp;No confundas los conceptos relativos a [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) con la acción de compilación **Resource**, los archivos de recursos (.resw) u otros "recursos" que se describen en el contexto de la estructuración del proyecto de código que genera el paquete de la aplicación.

Los recursos no tienen que ser cadenas, pueden ser cualquier objeto susceptible de compartirse, como estilos, plantillas, pinceles y colores. Sin embargo, los controles, las formas y otros objetos [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) no se pueden compartir, por lo que no se pueden declarar como recursos reutilizables. Para más información sobre el uso compartido, consulta la sección [Los recursos XAML deben ser compartibles](#xaml-resources-must-be-shareable), más adelante en este tema.

Aquí, un pincel y una cadena se declaran como recursos y los controles los usan en una página.

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

Todos los recursos deben tener una clave. Por lo general, esa clave es una cadena definida con `x:Key=”myString”`. Sin embargo, existen otras maneras de especificar una clave:

-   [Style](https://msdn.microsoft.com/library/windows/apps/br208849) y [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) necesitan un elemento **TargetType**, y usarán el elemento **TargetType** como clave si no se especifica [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787). En este caso, la clave es el objeto Tipo en sí, no una cadena. (Ver ejemplos abajo)
-   Los recursos [DataTemplate](https://msdn.microsoft.com/library/windows/apps/br242348) que tengan un elemento **TargetType** usarán este elemento **TargetType** como clave si no se especifica [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787). En este caso, la clave es el objeto Tipo en sí, no una cadena.
-   [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788) puede usarse en lugar de [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787). Sin embargo, x:Name también genera un campo de código subyacente para el recurso. Como resultado, x:Name es menos eficiente que x:Key porque ese campo debe inicializarse cuando se carga la página.

La [extensión de marcado StaticResource](../../xaml-platform/staticresource-markup-extension.md) puede recuperar recursos solo con un nombre de cadena ([x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) o [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788)). Sin embargo, el marco XAML también busca los recursos de estilo implícito (aquellos que usan **TargetType** en lugar de x:Key o x:Name) cuando decide qué estilo y plantilla usar para un control que no haya establecido las propiedades [Style](https://msdn.microsoft.com/library/windows/apps/br208743) y [ContentTemplate](https://msdn.microsoft.com/library/windows/apps/br209369) o [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/br242830).

Aquí, el objeto [Style](https://msdn.microsoft.com/library/windows/apps/br208849) tiene una clave implícita de **typeof(Button)**, y dado que el objeto [Button](https://msdn.microsoft.com/library/windows/apps/br209265) situado en la parte inferior de la página no especifica una propiedad [Style](https://msdn.microsoft.com/library/windows/apps/br208743), busca un estilo con clave de **typeof(Button)**:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Background" Value="Red"/>
        </Style>
    </Page.Resources>
    <Grid>
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
    </Grid>
</Page>
```

Para obtener más información sobre los estilos implícitos y cómo funcionan, consulta [Controles de aplicación de estilos](xaml-styles.md) y [Plantillas de control](control-templates.md).

## <a name="look-up-resources-in-code"></a>Buscar recursos en el código

El acceso a los miembros del diccionario de recursos es igual que el acceso a cualquier otro diccionario.

> [!WARNING]
> Al realizar una búsqueda de recursos en el código, solo los recursos en el `Page.Resources` se busca diccionario. A diferencia de la [extensión de marcado StaticResource](../../xaml-platform/staticresource-markup-extension.md), el código no vuelve al diccionario `Application.Resources` si los recursos no se encuentran en el primer diccionario.

 

Este ejemplo muestra cómo recuperar el recurso `redButtonStyle` del diccionario de recursos de una página:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```

Para buscar recursos de toda la aplicación desde el código, usa **Application.Current.Resources** para obtener el diccionario de recursos de la aplicación, como se muestra aquí.

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```

También puedes agregar un recurso de aplicación en el código.

Hay dos cosas a tener en cuenta al hacerlo.

-   En primer lugar, debes agregar los recursos antes de que cualquier página intente usar el recurso.
-   En segundo lugar, no puedes agregar recursos al constructor de la aplicación.

Puedes evitar ambos problemas si agregas el recurso en el método [Application.OnLaunched](https://msdn.microsoft.com/library/windows/apps/br242335) de este modo.

```CSharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```

## <a name="every-frameworkelement-can-have-a-resourcedictionary"></a>Cada FrameworkElement puede tener un ResourceDictionary

[FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) es una clase base de la que heredan los controles y tiene una propiedad [Resources](https://msdn.microsoft.com/library/windows/apps/br208740). Por lo tanto, puedes agregar un diccionario de recursos locales a cualquier objeto **FrameworkElement**.

Aquí, tanto [Page](https://msdn.microsoft.com/library/windows/apps/br227503) como [Border](https://msdn.microsoft.com/library/windows/apps/br209250) tienen diccionarios de recursos, y ambos tienen un recurso denominado "greeting". El [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) denominado 'textBlock2' es dentro del **borde**, por lo que la búsqueda de recursos busca primera del **borde**de los recursos, a continuación, la **página**de recursos y, a continuación, los recursos de la [aplicación](https://msdn.microsoft.com/library/windows/apps/br242324) . En el objeto **TextBlock** se pondrá "Hola mundo".

Para acceder a los recursos de ese elemento desde el código, usa la propiedad [Resources](https://msdn.microsoft.com/library/windows/apps/br208740) de ese elemento. Al acceder a los recursos de un [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706) en el código, en lugar de XAML, solo buscará en ese diccionario, no en los diccionarios del elemento primario.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>
    
    <StackPanel>
        <!-- Displays "Hello world" -->
        <TextBlock x:Name="textBlock1" Text="{StaticResource greeting}"/>

        <Border x:Name="border">
            <Border.Resources>
                <x:String x:Key="greeting">Hola mundo</x:String>
            </Border.Resources>
            <!-- Displays "Hola mundo" -->
            <TextBlock x:Name="textBlock2" Text="{StaticResource greeting}"/>
        </Border>

        <!-- Displays "Hola mundo", set in code. -->
        <TextBlock x:Name="textBlock3"/>
    </StackPanel>
</Page>

```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            textBlock3.Text = (string)border.Resources["greeting"];
        }
    }
```

## <a name="merged-resource-dictionaries"></a>Diccionarios de recursos combinados

Un *diccionario de recursos combinado* combina un diccionario de recursos en otro, que está normalmente en otro archivo.

> **Sugerencia**&nbsp;&nbsp;Puedes crear un archivo de diccionario de recursos en Microsoft Visual Studio mediante la opción **Agregar &gt; Nuevo elemento… &gt; Diccionario de recursos** del menú **Proyecto**.

En este apartado, debes definir un diccionario de recursos en un archivo XAML independiente denominado Dictionary1.xaml.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

Para usar ese diccionario, combínalo con el de tu página:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

En este ejemplo puedes ver lo que ocurre. En `<Page.Resources>`, declaras `<ResourceDictionary>`. El marco XAML te crea implícitamente un diccionario de recursos cuando agregas recursos al objeto `<Page.Resources>`. Sin embargo, en este caso no quieres cualquier diccionario de recursos, sino uno que contenga diccionarios combinados.

Por tanto, declaras `<ResourceDictionary>` y luego agregas cosas a su colección `<ResourceDictionary.MergedDictionaries>`. Cada una de esas entradas adopta la forma de `<ResourceDictionary Source="Dictionary1.xaml"/>`. Para agregar más de un diccionario, simplemente agrega una entrada `<ResourceDictionary Source="Dictionary2.xaml"/>` después de la primera entrada.

Después de `<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>`, si quieres puedes colocar recursos adicionales en el diccionario principal. Los recursos de un diccionario combinado se usan igual que los de un diccionario normal. En el ejemplo anterior, `{StaticResource brush}` busca el recurso en el diccionario secundario/combinado (Dictionary1.xaml), mientras que `{StaticResource greeting}` lo busca en el diccionario de la página principal.

En la secuencia de búsqueda de recursos, un diccionario [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) solo se comprueba después de comprobar todos los recursos con clave de ese [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794). Después de buscar en ese nivel, la búsqueda llega a los diccionarios combinados y se comprueba cada elemento de **MergedDictionaries**. Si hay varios diccionarios combinados, estos se comprueban en orden inverso al que se declararon en la propiedad **MergedDictionaries**. En el ejemplo siguiente, si tanto Dictionary2.xaml como Dictionary1.xaml declaran la misma clave, primero se usa la clave de Dictionary2.xaml porque es la última del conjunto de **MergedDictionaries**.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

En el ámbito de cualquier [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), se comprueba que la clave del diccionario sea única. Sin embargo, ese ámbito no se extiende a los diferentes elementos de los distintos archivos de [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801).

Puedes usar la combinación de la secuencia de búsqueda y la falta de obligatoriedad de una clave única en los ámbitos de diccionarios combinados para crear una secuencia de valores de reserva de los recursos de [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794). Por ejemplo, podrías almacenar las preferencias de usuario para un determinado color de pincel en el último diccionario de recursos combinado en la secuencia, mediante un diccionario de recursos que se sincronice con el estado de tu aplicación y los datos de preferencia del usuario. Sin embargo, si aún no existen preferencias del usuario, puedes definir esa misma cadena de clave para un recurso de **ResourceDictionary** en el archivo [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) inicial, y puede servir como valor de reserva. Recuerda que siempre se comprueban todos los valores que proporcionas en un diccionario de recursos principal antes de comprobar los diccionarios combinados; por lo tanto, si quieres usar la técnica de reserva, no definas ese recurso en un diccionario de recursos principal.

## <a name="theme-resources-and-theme-dictionaries"></a>Recursos de temas y diccionarios de temas

Un [ThemeResource](../../xaml-platform/themeresource-markup-extension.md) es similar a un [StaticResource](../../xaml-platform/staticresource-markup-extension.md), pero la búsqueda de recursos se reevalúa cuando cambia el tema.

En este ejemplo, establece el primer plano de un [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) en un valor del tema actual.

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

Un diccionario de temas es un tipo especial de diccionario combinado que contiene los recursos que varían en función del tema que esté usando el usuario en su dispositivo. Por ejemplo, el tema "Claro" podría usar un pincel blanco mientras que el tema "Oscuro" podría usar un pincel de color oscuro. El pincel cambia el recurso en el que se resuelve, aunque la composición de un control que usa el pincel como recurso podría ser la misma. Para reproducir el comportamiento de cambio de tema en tus propias plantillas y estilos, en lugar de usar [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) como la propiedad para combinar elementos en los diccionarios principales, mejor usa la propiedad [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807).

Cada elemento [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) de [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) debe tener un valor [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787). Ese valor es una cadena que asigna un nombre al tema correspondiente, por ejemplo: "Default", "Dark", "Light" o "HighContrast". Por lo general, los objetos `Dictionary1` y `Dictionary2` definirán recursos que tengan los mismos nombres pero valores diferentes.

Aquí, usa texto rojo para el tema claro y texto azul para el tema oscuro.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!-- Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

En este ejemplo, establece el primer plano de un [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) en un valor del tema actual.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

Para los diccionarios de temas, el diccionario activo que se usará para la búsqueda de recursos cambia dinámicamente, siempre que se use la [extensión de marcado ThemeResource](../../xaml-platform/themeresource-markup-extension.md) para crear la referencia y el sistema detecte un cambio de tema. El comportamiento de la búsqueda realizada por el sistema se basa en la asignación del tema activo al objeto [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) de un diccionario de temas específico.

Puede resultar útil examinar cómo se estructuran los diccionarios de temas en los recursos de diseño predeterminados de XAML, que siguen las plantillas que Windows Runtime usa de manera predeterminada para sus controles. Abre los archivos XAML en \\(Archivos de programa)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;versión del SDK&gt;\\Generic, mediante un editor de texto o el IDE. Observa cómo se definen primero los diccionarios de temas en generic.xaml y cómo cada diccionario de temas define las mismas claves. Después, los elementos de composición de los diversos elementos con clave que están fuera de los diccionarios de temas, y que se definen más adelante en el XAML, hacen referencia a cada una de estas claves. También hay un archivo themeresources.xaml independiente para el diseño que solo contiene recursos de temas y plantillas adicionales, en lugar de las plantillas de control predeterminadas. Las áreas de temas son duplicados de lo que verías en generic.xaml.

Cuando usas herramientas de diseño XAML para editar copias de estilos y plantillas, estas herramientas extraen secciones de los diccionarios de recursos de diseño XAML y las colocan como copias locales de los elementos del diccionario XAML que forman parte de la aplicación y el proyecto.

Para más información y una lista de los recursos del sistema y específicos de temas que hay a disposición de tus aplicaciones, consulta [Recursos de temas XAML](xaml-theme-resources.md).

## <a name="lookup-behavior-for-xaml-resource-references"></a>Comportamiento de la búsqueda de referencias a recursos XAML

*Comportamiento de búsqueda* es el término que describe la manera en la que el sistema de recursos XAML intenta buscar un recurso XAML. La búsqueda se realiza cuando se hace referencia a una clave como una referencia de recursos XAML desde algún lugar del XAML de la aplicación. En primer lugar, el sistema de recursos tiene un comportamiento predecible en cuanto al lugar en el que buscará si existe un recurso según el ámbito. Si un recurso no se encuentra en el ámbito inicial, este último se expande. El comportamiento de búsqueda continúa a través de las ubicaciones y los ámbitos donde podría encontrarse un recurso XAML según la definición de una aplicación o del sistema. Si no se encuentra el recurso en todos los intentos de búsqueda posibles, se suele producir un error. Por lo general, se pueden eliminar estos errores durante el proceso de desarrollo.

El comportamiento de búsqueda para las referencias a recursos XAML comienza con el objeto al que se aplica el uso real y su propiedad [Resources](https://msdn.microsoft.com/library/windows/apps/br208740). Si allí existe un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), se comprueba si en este **ResourceDictionary** hay un elemento con la clave solicitada. Este primer nivel de búsqueda rara vez es relevante porque normalmente no defines un recurso y luego haces referencia a él en el mismo objeto. De hecho, suele ocurrir que aquí no existe una propiedad **Resources**. Puedes hacer referencias a los recursos del XAML desde casi cualquier parte del XAML; no estás limitado a las propiedades de las subclases de [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/br208706).

A continuación, la secuencia de búsqueda comprueba el siguiente objeto primario del árbol de objetos en tiempo de ejecución de la aplicación. Si existe un [FrameworkElement.Resources](https://msdn.microsoft.com/library/windows/apps/br208740) y contiene un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), se solicita el elemento de diccionario con la clave especificada. Si se encuentra el recurso, la secuencia de búsqueda se detiene y el objeto se proporciona a la ubicación donde se creó la referencia. En caso contrario, el comportamiento de búsqueda continúa con el siguiente nivel primario hacia la raíz del árbol de objetos. La búsqueda continúa recursivamente hacia arriba hasta llegar al elemento raíz del XAML, agotando así la búsqueda de todas las ubicaciones de recursos inmediatos posibles.

> **Nota**&nbsp;&nbsp;Es una práctica habitual definir todos los recursos inmediatos en el nivel raíz de una página, tanto para aprovechar este comportamiento de búsqueda de recursos como por una convención de estilo de marcado de XAML.

 

Si no se encuentra el recurso solicitado en los recursos inmediatos, el siguiente paso de la búsqueda es comprobar la propiedad [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338). **Application.Resources** es el mejor lugar para guardar los recursos específicos de la aplicación a los que varias páginas de la estructura de navegación de tu aplicación hacen referencia.

Las plantillas de control tienen otra posible ubicación para la búsqueda de recursos: los diccionarios de temas. Un diccionario de temas es un único archivo XAML que tiene un elemento [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) como raíz. El diccionario de temas podría ser un diccionario combinado de [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338). El diccionario de temas también podría ser el diccionario de temas específico de un control personalizado con plantilla.

Por último, se realiza una búsqueda de recursos en los recursos de la plataforma. Los recursos de la plataforma incluyen las plantillas de control definidas para cada uno de los temas de la interfaz de usuario del sistema y que definen la apariencia predeterminada de todos los controles que usas para la interfaz de usuario en una aplicación de Windows en tiempo de ejecución. También incluyen un conjunto de recursos con nombre relacionados con la apariencia y los temas de todo el sistema. Estos recursos son técnicamente un elemento [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801) y, por lo tanto, están disponibles para la búsqueda del XAML o de código cuando la aplicación se ha cargado. Por ejemplo, entre los recursos de tema del sistema encontramos uno llamado "SystemColorWindowTextColor" que ofrece una definición de [Color](https://msdn.microsoft.com/library/windows/apps/hh673723) que permite que el color del texto de la aplicación sea el mismo que el del texto de una ventana del sistema procedente del sistema operativo y las preferencias del usuario. Otros estilos de XAML para tu aplicación pueden hacer referencia a este estilo, o tu código puede obtener un valor de búsqueda de recurso (y convertirlo en **Color** en el caso de ejemplo).

Para más información y una lista de los recursos del sistema y específicos de temas que hay a disposición de una aplicación UWP que use XAML, consulta [Recursos de temas XAML](xaml-theme-resources.md).

Si tampoco se encuentra la clave solicitada en ninguna de estas ubicaciones, se produce una excepción o error de análisis de XAML. En determinadas circunstancias, la excepción de análisis de XAML puede ser una excepción en tiempo de ejecución que no es detectada por una acción de compilación de marcado de XAML ni por un entorno de diseño de XAML.

Debido al comportamiento de búsqueda por niveles de los diccionarios de recursos, puedes definir deliberadamente varios elementos de recurso, cada uno de ellos con el mismo valor de cadena que la clave, siempre que cada recurso se defina en un nivel diferente. En otras palabras, aunque las claves deben ser únicas en cualquier [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) dado, este requisito no se aplica a toda la secuencia de comportamiento de búsqueda. Durante la búsqueda, solo el primer objeto que se recupere correctamente se usará para la referencia del recurso XAML y, a continuación, se detendrá la búsqueda. Puedes usar este comportamiento para solicitar el mismo recurso XAML por clave en varias posiciones del código XAML de la aplicación y obtener resultados distintos, en función del ámbito desde el que se hizo referencia al recurso XAML y el comportamiento de esa búsqueda específica.

##  <a name="forward-references-within-a-resourcedictionary"></a>Referencias adelantadas en un ResourceDictionary


Las referencias a recursos XAML en un diccionario de recursos determinado deben hacer referencia a un recurso que se ya haya definido con una clave, y ese recurso debe aparecer léxicamente antes que la referencia al recurso. Las referencias adelantadas no se pueden resolver mediante una referencia a recurso XAML. Por este motivo, si usas referencias a recursos XAML desde otro recurso, debes diseñar la estructura del diccionario de recursos de manera que en el diccionario de recursos se definan primero aquellos recursos que son usados por otros recursos.

Los recursos definidos en el nivel de la aplicación no pueden hacer referencia a recursos inmediatos. Esto equivale a intentar una referencia adelantada, porque los recursos de la aplicación se procesan en primer lugar (cuando la aplicación se inicia por primera vez y antes de que se cargue cualquier contenido de página de navegación). Sin embargo, los recursos inmediatos pueden hacer referencia a un recurso de la aplicación, lo que puede resultar una técnica útil para evitar situaciones de referencias adelantadas.

## <a name="xaml-resources-must-be-shareable"></a>Los recursos XAML deben ser compartibles


Para que un objeto exista en un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), dicho objeto debe ser *compartible*.

Es necesario que sean compartibles porque cuando se construye el árbol de objetos de una aplicación y se usa en tiempo de ejecución, los objetos no pueden existir en varias ubicaciones del árbol. El sistema de recursos crea copias a nivel interno de los valores de recursos que se usarán en el gráfico de objetos de la aplicación cuando se solicite cada recurso XAML.

Un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) y el XAML de Windows Runtime normalmente admiten estos objetos para compartirlos:

-   Estilos y plantillas ([Style](https://msdn.microsoft.com/library/windows/apps/br208849) y clases derivadas de [FrameworkTemplate](https://msdn.microsoft.com/library/windows/apps/br208753))
-   Pinceles y colores (clases derivadas de los valores [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) y [Color](https://msdn.microsoft.com/library/windows/apps/hh673723))
-   Tipos de animación, incluido [Storyboard](https://msdn.microsoft.com/library/windows/apps/br210490)
-   Transformaciones (clases derivadas de [GeneralTransform](https://msdn.microsoft.com/library/windows/apps/br210034))
-   [Matrix](https://msdn.microsoft.com/library/windows/apps/br210127) y [Matrix3D](https://msdn.microsoft.com/library/windows/apps/br243266)
-   Valores de [Point](https://msdn.microsoft.com/library/windows/apps/br225870)
-   Otras estructuras relacionadas con la interfaz de usuario, como [Thickness](https://msdn.microsoft.com/library/windows/apps/br208864) y [CornerRadius](https://msdn.microsoft.com/library/windows/apps/br242343)
-   [Tipos de datos intrínsecos de XAML](https://msdn.microsoft.com/library/windows/apps/mt186448)

También puedes usar tipos personalizados como un recurso compartible si sigues los patrones de implementación necesarios. Tienes que definir estas clases en el código de respaldo (o en los componentes en tiempo de ejecución que incluyas) y, después, crear instancias de estas clases en XAML como si fueran un recurso. Algunos ejemplos son orígenes de datos de objeto e implementaciones de [IValueConverter](https://msdn.microsoft.com/library/windows/apps/br209903) para el enlace de datos.

Los tipos personalizados deben tener un constructor predeterminado, porque eso es lo que usa un analizador XAML para crear una instancia de una clase. Los tipos personalizados que se usan como recursos no pueden tener la clase [UIElement](https://msdn.microsoft.com/library/windows/apps/br208911) en su herencia, porque un **UIElement** nunca se puede compartir (siempre está destinado a representar exactamente un elemento de la interfaz de usuario que existe en una posición en el gráfico de objetos de la aplicación en tiempo de ejecución).

## <a name="usercontrol-usage-scope"></a>Ámbito de uso de UserControl


Un elemento [UserControl](https://msdn.microsoft.com/library/windows/apps/br227647) es un caso especial del comportamiento de la búsqueda de recursos porque tiene los conceptos inherentes de un ámbito de definición y un ámbito de uso. Un **UserControl** que hace referencia a un recurso XAML desde su ámbito de definición debe poder admitir la búsqueda de dicho recurso en su propia secuencia de búsqueda del ámbito de definición; es decir, no puede acceder a los recursos de la aplicación. Desde el ámbito de uso de un **UserControl**, la referencia a un recurso se trata como si se realizara desde dentro de la secuencia de búsqueda hacia la raíz de la página de uso (igual que cualquier otra referencia a recursos que se realiza desde un objeto en un árbol de objetos cargados) y puede acceder a los recursos de la aplicación.

## <a name="resourcedictionary-and-xamlreaderload"></a>ResourceDictionary y XamlReader.Load

Un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) se puede usar como raíz o como parte de la entrada XAML para el método [XamlReader.Load](https://msdn.microsoft.com/library/windows/apps/br228048). También puedes incluir referencias a recursos XAML en ese código XAML si todas estas referencias están completamente autocontenidas en el código XAML para su carga. **XamlReader.Load** analiza el código XAML en un contexto que no tiene constancia de otros objetos **ResourceDictionary**, ni siquiera de la propiedad [Application.Resources](https://msdn.microsoft.com/library/windows/apps/br242338). Además, no uses `{ThemeResource}` desde el código XAML enviado a **XamlReader.Load**.

## <a name="using-a-resourcedictionary-from-code"></a>Uso de ResourceDictionary desde el código

La mayoría de los escenarios para un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) se controla exclusivamente en XAML. Declaras el contenedor **ResourceDictionary** y los recursos que incluye como un archivo XAML o un conjunto de nodos XAML en un archivo de definición de interfaz de usuario. Y, a continuación, usas referencias a recursos XAML para solicitar esos recursos de otras partes del código XAML. Aun así, hay ciertos casos en que tu aplicación podría querer ajustar el contenido de un **ResourceDictionary** con código que se ejecute mientras se ejecute la aplicación, o, al menos, consultar el contenido de un **ResourceDictionary** para ver si ya se definió un recurso. Las llamadas a este código se realizan en una instancia de **ResourceDictionary**, por lo que primero tienes que recuperar una, bien un **ResourceDictionary** inmediato en algún lugar del árbol de objetos mediante la obtención de [FrameworkElement.Resources](https://msdn.microsoft.com/library/windows/apps/br208740), o bien, `Application.Current.Resources`.

En el código de C\# o Microsoft Visual Basic, puedes hacer referencia a un recurso en un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) determinado con el indexador ([Item](https://msdn.microsoft.com/library/windows/apps/jj603134)). Un **ResourceDictionary** es un diccionario de claves de cadena, por lo que el indexador usa la clave de cadena en lugar de un índice de entero. En extensiones de componentes VisualC ++ (C++ / CX), usa la [búsqueda](https://msdn.microsoft.com/library/windows/apps/br208800).

Cuando usas código para examinar o cambiar un objeto [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), el comportamiento de las API, como [Lookup](https://msdn.microsoft.com/library/windows/apps/br208800) o [Item](https://msdn.microsoft.com/library/windows/apps/jj603134), no se desvía de los recursos inmediatos a los recursos de la aplicación, ya que ese es un comportamiento del analizador XAML que solo tiene lugar al cargar las páginas XAML. En tiempo de ejecución, el ámbito de las claves es independiente en la instancia de **ResourceDictionary** que se usa en ese momento, aunque sí se extiende a [MergedDictionaries](https://msdn.microsoft.com/library/windows/apps/br208801).

Además, si solicitas una clave que no existe en el [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794), puede que no se produzca un error, sino que el valor devuelto sea simplemente **null**. Pero aún obtendrás un error si intentas usar el **null** devuelto como valor. El error se originará en el establecedor de la propiedad, no en la llamada de **ResourceDictionary**. La única forma de evitar el error es si la propiedad acepta **null** como valor válido. Este comportamiento contrasta con el comportamiento de búsqueda del XAML en tiempo de análisis del XAML: si no se puede resolver la clave proporcionada desde el XAML en tiempo de análisis, se produce un error de análisis del XAML, incluso aunque la propiedad haya aceptado el valor **null**.

Los diccionarios de recursos combinados se incluyen en el ámbito de índice del diccionario de recursos principal que hace referencia al diccionario combinado en tiempo de ejecución. En otras palabras, puedes usar los elementos **Item** o [Lookup](https://msdn.microsoft.com/library/windows/apps/br208800) del diccionario principal para encontrar los objetos que se definieron en el diccionario combinado. En este caso, el comportamiento de la búsqueda se asemeja al comportamiento de la búsqueda de XAML en tiempo de análisis: si hay varios objetos en diccionarios combinados, todos con la misma clave, se devuelve el objeto del último diccionario agregado.

Está permitido agregar elementos a un objeto [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) existente con una llamada a **Add** (C\# o Visual Basic) o a [Insert](https://msdn.microsoft.com/library/windows/apps/br208799) (C++/CX). Puedes agregarlos los elementos a recursos inmediatos o a recursos de la aplicación. Todas estas llamadas a API requieren una clave, lo que satisface el requisito de que cada elemento de un objeto **ResourceDictionary** tenga una clave. Sin embargo, los elementos que agregues a un objeto **ResourceDictionary** en tiempo de ejecución no son importantes para las referencias a recursos XAML. La búsqueda necesaria de referencias a recursos XAML se produce cuando ese XAML se analiza primero según se carga la aplicación (o se detecta un cambio de tema). Los recursos agregados a colecciones en tiempo de ejecución no estaban disponibles entonces, y la alteración del **ResourceDictionary** no invalida un recurso ya recuperado, incluso si cambias el valor de ese recurso.

También puedes quitar elementos de un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) en tiempo de ejecución, crear copias de algunos o de todos los elementos, y otras operaciones. La lista de miembros de **ResourceDictionary** indica qué API están disponibles. Ten en cuenta que como **ResourceDictionary** tiene una API proyectada para admitir las interfaces de la colección subyacentes, las opciones de tu API diferirán según si usas C\# o Visual Basic respecto a C++/CX.

## <a name="resourcedictionary-and-localization"></a>ResourceDictionary y localización


Inicialmente, un [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) de XAML podría contener cadenas localizables. En este caso, guarda estas cadenas como recursos del proyecto en lugar de en un **ResourceDictionary**. Extrae las cadenas del código XAML y, en su lugar, asigna al elemento propietario un valor de [directiva x:Uid](https://msdn.microsoft.com/library/windows/apps/mt204791). Luego, define un recurso en un archivo de recursos. Proporciona un nombre de recurso con el formato *XUIDValue*.*PropertyName* y un valor de recurso de la cadena que debe localizarse.

## <a name="custom-resource-lookup"></a>Búsqueda de recursos personalizada

Para los escenarios avanzados, puedes implementar una clase que pueda tener un comportamiento distinto que el comportamiento de búsqueda de referencias a recursos XAML descrito en este tema. Para ello, implementa la clase [CustomXamlResourceLoader](https://msdn.microsoft.com/library/windows/apps/br243327) y, a continuación, podrás acceder a ese comportamiento mediante el uso de la [extensión de marcado CustomResource](https://msdn.microsoft.com/library/windows/apps/mt185580) para referencias de recursos en vez de usar [StaticResource](../../xaml-platform/staticresource-markup-extension.md) o [ThemeResource](../../xaml-platform/themeresource-markup-extension.md). La mayoría de las aplicaciones no tendrán escenarios que requieran esto. Para obtener más información, consulta [CustomXamlResourceLoader](https://msdn.microsoft.com/library/windows/apps/br243327).

 
## <a name="related-topics"></a>Temas relacionados

* [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)
* [Introducción a XAML](https://msdn.microsoft.com/library/windows/apps/mt185595)
* [Extensión de marcado StaticResource](../../xaml-platform/staticresource-markup-extension.md)
* [Extensión de marcado ThemeResource](../../xaml-platform/themeresource-markup-extension.md)
* [Recursos de temas XAML](xaml-theme-resources.md)
* [Controles de estilo](xaml-styles.md)
* [Atributo x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)

 

 




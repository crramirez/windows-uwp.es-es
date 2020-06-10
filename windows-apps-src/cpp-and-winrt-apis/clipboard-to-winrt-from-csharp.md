---
title: Portabilidad del ejemplo de Clipboard a C++/WinRT desde C# (un caso práctico)
description: En este tema se presenta un caso práctico de portabilidad de las [aplicaciones para la Plataforma universal de Windows (UWP) de ejemplo](https://github.com/microsoft/Windows-universal-samples) de [C#](/visualstudio/get-started/csharp) a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
ms.date: 04/13/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, proyección, portar, migrar, C#, ejemplo, clipboard, caso, práctico
ms.localizationpriority: medium
ms.openlocfilehash: 660eac0cb2b0679815d628f60b77bc5ac01d042f
ms.sourcegitcommit: 8eae7aec4c4ffb8a0c30e9d03744942fb23958d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84334240"
---
# <a name="porting-the-clipboard-sample-tocwinrtfromcmdasha-case-study"></a>Portabilidad del ejemplo de Clipboard a C++/WinRT desde C#&mdash;un caso práctico

En este tema se presenta un caso práctico de portabilidad de las [aplicaciones para la Plataforma universal de Windows (UWP) de ejemplo](https://github.com/microsoft/Windows-universal-samples) de [C#](/visualstudio/get-started/csharp) a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Para practicar y ganar experiencias de portabilidad, puedes seguir el tutorial y portar el ejemplo por tu cuenta a medida que avanzas.

Para obtener un catálogo completo de los detalles técnicos implicados en la migración a C++/WinRT desde C#, consulta el tema complementario [Migrar a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp).

## <a name="a-brief-preface-about-c-and-c-source-code-files"></a>Breve introducción sobre los archivos de código fuente de C# y C++

En un proyecto de C#, los archivos de código fuente son archivos `.cs` principalmente. Al pasar a C++, observarás que hay más tipos de archivos de código fuente con los que familiarizarse. La razón tiene que ver con la diferencia entre los compiladores, la forma en que se reutiliza el código de C++ y las nociones de *declaración* y *definición* de un tipo y sus funciones (sus métodos).

Una *declaración* de función describe solo la *firma* de la función (tipo de valor devuelto, nombre, y tipos y nombres de parámetros). Una *definición* de función incluye el *cuerpo* de la función (implementación).

Es un poco diferente cuando se trata de tipos. Para *definir* un tipo, se proporciona el nombre y, como mínimo, se *declaran* todas sus funciones miembro (y otros miembros). Es decir, puedes *definir* un tipo aunque no definas sus funciones miembro.

- Los archivos de código fuente de C++ comunes son `.h` y `.cpp`. Un archivo `.h` es un archivo de *encabezado*, que define uno o varios tipos. Aunque *puedes* definir funciones miembro en un encabezado, esta suele ser la finalidad de un archivo `cpp`. Por lo tanto, para un tipo de archivo de C++ **MyClass** hipotético, definirías **MyClass** en `MyClass.h` y sus funciones miembro, en `MyClass.cpp`. Para que otros desarrolladores vuelvan a usar las clases, debes compartir solo los archivos `.h` y el código de objeto. Debes conservar el secreto de los archivos `.cpp`, ya que la implementación constituye tu propiedad intelectual.
- Encabezado precompilado (`pch.h`). Normalmente, hay un conjunto de archivos de encabezado que se incluyen en la aplicación y que establecen cambios solo en raras ocasiones. Por lo tanto, en lugar de procesar el contenido de ese conjunto de encabezados cada vez que realizas las compilación, puedes agregar esos encabezados en uno, compilarlos una vez y, a continuación, utilizar la salida de ese paso de precompilación cada vez que realices la compilación. Esto se hace a través de un archivo de *encabezado precompilado* (normalmente denominado `pch.h`).
- Archivos `.idl`. Estos archivos contienen lenguaje de definición de interfaz (IDL). Puedes considerar IDL como los archivos de encabezado de tipos de Windows Runtime. Hablaremos más sobre IDL en la sección [IDL para el tipo **MainPage**](#idl-for-the-mainpage-type).

## <a name="download-and-test-the-clipboard-sample"></a>Descarga y prueba del ejemplo de Clipboard

Visita la página web del [ejemplo de Clipboard](/samples/microsoft/windows-universal-samples/clipboard/) y haz clic en **Descargar archivo ZIP**. Descomprime el archivo descargado y echa un vistazo a la estructura de carpetas.

- La versión de C# del código fuente de ejemplo se encuentra en la carpeta denominada `cs`.
- La versión de C++/WinRT del código fuente de ejemplo se encuentra en la carpeta denominada `cppwinrt`.
- Otros archivos&mdash;utilizados por la versión de C# y la versión de C++/WinRT&mdash;pueden encontrarse en las carpetas `shared` y `SharedContent`.

En el tutorial de este tema se muestra cómo puedes volver a crear la versión de C++/WinRT de la muestra de Clipboard si la portas desde el código fuente de C#. De este modo, puedes ver cómo puede portar tus propios proyectos de C# a C++/WinRT.

Para hacerte una idea de lo que hace el ejemplo, abre la solución de C# (`\Clipboard_sample\cs\Clipboard.sln`), cambia la configuración según corresponda (quizás a *x64*), compila y ejecuta. La propia interfaz de usuario (UI) del ejemplo te guía a través de sus diversas características, paso a paso.

## <a name="create-a-blank-app-cwinrt-named-clipboard"></a>Creación de una aplicación en blanco (C++/WinRT) denominada Clipboard

> [!NOTE]
> Para más información sobre cómo instalar y usar la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Comienza el proceso de portabilidad creando un nuevo proyecto de C++/WinRT en Microsoft Visual Studio. Crea un nuevo proyecto con la plantilla de proyecto **de aplicación en blanco (C++/WinRT)** . Establece el nombre en *Clipboard* y, para que la estructura de carpetas coincida con el tutorial, asegúrate de que la opción **Colocar la solución y el proyecto en el mismo directorio** esté desactivada.

Simplemente para tener una base de referencia, asegúrate de que este nuevo proyecto vacío se compile y ejecute.

## <a name="packageappxmanifest-and-asset-files"></a>Activos Package.appxmanifest y Assets

Si las versiones de C# y C++/WinRT del ejemplo no necesitan instalarse en paralelo en el mismo equipo, los archivos de origen del manifiesto del paquete de la aplicación de los dos proyectos (`Package.appxmanifest`) pueden ser idénticos. En ese caso, solo tienes que copiar `Package.appxmanifest` del proyecto de C# en el proyecto de C++/WinRT y estarás listo.

Para que coexistan las dos versiones del ejemplo, necesitan identificadores diferentes. En ese caso, en el proyecto de C++/WinRT, abre el archivo `Package.appxmanifest` en un editor XML y toma nota de estos tres valores.

- En el elemento **/Package/Identity**, anota el valor del atributo **Name**. Este es el *nombre del paquete*. Para un proyecto recién creado, el proyecto le asignará un valor inicial de un GUID exclusivo.
- En el elemento **/Package/Applications/Application**, anota el valor del atributo **Id**. Este es el *id. de la aplicación*.
- En el elemento **/Package/mp:PhoneIdentity**, anota el valor del atributo **PhoneProductId**. De nuevo, para un proyecto recién creado, se establecerá en el mismo GUID en el que se ha establecido el nombre del paquete.

Luego, copia `Package.appxmanifest` del proyecto de C# en el proyecto de C++/WinRT. Por último, puedes restaurar los tres valores que anotaste. O bien, puedes editar los valores copiados para que sean exclusivos o adecuados para la aplicación y para tu organización (como los haría normalmente para un proyecto nuevo). Por ejemplo, en este caso, en lugar de restaurar el valor del nombre del paquete, solo podemos cambiar el valor copiado de *Microsoft.SDKSamples.Clipboard.CS* a *Microsoft.SDKSamples.Clipboard.CppWinRT*. Y podemos dejar el identificador de la aplicación establecido *App*. Siempre que el nombre del paquete *o* el identificador de la aplicación sean diferentes, las dos aplicaciones tendrán identificadores de modelo del usuario de la aplicación (AUMID) diferentes.

A los efectos de este tutorial, tiene sentido hacer algunos otros cambios en `Package.appxmanifest`. Hay tres apariciones de la cadena *Clipboard C# Sample*. Cámbiala a *Clipboard C++/WinRT Sample*.

En el proyecto de C++/WinRT, el archivo `Package.appxmanifest` y el proyecto ahora están sin sincronizar con respecto a los archivos de recursos a los que hacen referencia. Para solucionarlo, quita primero los recursos del proyecto de C++/WinRT; para ello, selecciona todos los archivos de la carpeta `Assets` (en Explorador de soluciones en Visual Studio) y quítalos (elige **Eliminar** en el cuadro de diálogo).

El proyecto de C# hace referencia a los archivos de recursos de una carpeta compartida. Puedes hacer lo mismo en el proyecto de C++/WinRT o puedes copiar los archivos como haremos en este tutorial.

Ve a la carpeta `\Clipboard_sample\SharedContent\media`. Selecciona los siete archivos que incluye el proyecto de C# (de `microsoft-sdk.png` a `windows-sdk.png`), cópialos y pégalos en la carpeta `\Clipboard\Clipboard\Assets` del nuevo proyecto.

Haz clic con el botón derecho en la carpeta `Assets` (en el Explorador de soluciones en el proyecto de C++/WinRT) > **Agregar** > **Elemento existente…** y ve hasta `\Clipboard\Clipboard\Assets`. En el selector de archivos, selecciona los siete archivos y haz clic en **Agregar**.

`Package.appxmanifest` ahora vuelve a estar sincronizado con los archivos de recursos del proyecto.

## <a name="mainpage-including-the-functionality-that-configures-the-sample"></a>**MainPage**, incluida la funcionalidad que configura la muestra.

El ejemplo de Clipboard &mdash;como todos los [ejemplos de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/microsoft/Windows-universal-samples)&mdash; se compone de una colección de escenarios que el usuario puede recorrer de uno en uno. La colección de escenarios de un ejemplo dado se configura en el código fuente del ejemplo. Cada escenario de la colección es un elemento de datos que almacena un título, así como el tipo de la clase en el proyecto que implementa el escenario.

En la versión de C# del ejemplo, si buscas en el archivo de código fuente `SampleConfiguration.cs`, verás dos clases. La mayor parte de la lógica de configuración se encuentra en la clase **MainPage**, que es una clase parcial (forma una clase completa cuando se combina con el marcado en `MainPage.xaml` y el código imperativo en `MainPage.xaml.cs`). La otra clase de este archivo de código fuente es **Scenario**, con sus propiedades **Title** y **ClassType**.

En los siguientes apartados, veremos cómo portar **MainPage** y **Scenario**.

### <a name="idl-for-the-mainpage-type"></a>IDL para el tipo **MainPage**

Comencemos esta sección hablando brevemente sobre el lenguaje de definición de interfaz (IDL) y cómo nos ayuda a programar con C++/WinRT. IDL es un tipo de código fuente que describe la superficie de un tipo Windows Runtime a la que se puede llamar. La superficie de un tipo a la que se puede llamar (o pública) se *proyecta* al mundo, lo que permite consumir el tipo. Esa parte *proyectada* del tipo contrasta con la implementación interna real del tipo, que no es pública y a la que no se puede llamar. Solo es la parte proyectada que definimos en IDL.

En el código fuente IDL creado (dentro de un archivo `.idl`), puedes compilar IDL en archivos de metadatos legibles por máquina (también conocidos como metadatos de Windows). Estos archivos de metadatos tienen la extensión `.winmd`. A continuación se indican algunos de sus usos.

- Un archivo `.winmd` puede describir los tipos de Windows Runtime en un componente. Cuando se hace referencia a un componente de Windows Runtime (WRC) desde un proyecto de aplicación, el proyecto de aplicación lee los metadatos de Windows que pertenecen a WRC (esos metadatos pueden estar en un archivo independiente o empaquetados en el mismo archivo que el propio WRC) para que pueda usar los tipos de WRC desde la aplicación.
- Un archivo `.winmd` puede describir los tipos de Windows Runtime de una parte de la aplicación para que se puedan usar en una parte diferente de la misma aplicación. Por ejemplo, un tipo de Windows Runtime que se usa en una página XAML de la misma aplicación.
- Para facilitar el consumo de tipos de Windows Runtime (integrado o de terceros), el sistema de compilación de C++/WinRT utiliza archivos `.winmd` para generar tipos de contenedor para representar las partes proyectadas de esos tipos de Windows Runtime.
- Para que sea más fácil implementar sus propios tipos de Windows Runtime, el sistema de compilación de C++/WinRT convierte el IDL en un archivo `.winmd` y, a continuación, lo usa para generar contenedores para la proyección, así como códigos auxiliares en los que basar la implementación (hablaremos más sobre estos códigos auxiliares más adelante en este tema).

La versión específica de IDL que usamos con C++/WinRT es [Lenguaje de definición de interfaz de Microsoft 3.0](/uwp/midl-3/intro). En el resto de esta sección, examinaremos el tipo **MainPage** de C# con cierto detalle. Decidiremos qué partes deben estar en la *proyección* del tipo de **MainPage** de C++/WinRT (es decir, en su superficie pública o a la que se puede llamar), y cuáles solo pueden ser parte de su implementación. Esta distinción es importante porque, cuando lleguemos a la creación de IDL (lo que haremos en la sección siguiente), solo definiremos las partes a las que se puede llamar.

Los archivos de código fuente de C# que, en conjunto, implementan el tipo **MainPage** son: `MainPage.xaml` (que portaremos pronto, al copiarlo), `MainPage.xaml.cs` y `SampleConfiguration.cs`.

En la versión de C++/WinRT, factorizaremos el tipo **MainPage** en los archivos de código fuente de una manera similar. Tomaremos la lógica de `MainPage.xaml.cs` y la traduciremos en su mayor parte a `MainPage.h` y `MainPage.cpp`. Y para la lógica de `SampleConfiguration.cs`, la traduciremos a `SampleConfiguration.h` y `SampleConfiguration.cpp`.

Las clases de una aplicación para la Plataforma universal de Windows (UWP) de C# son, por supuesto, tipos de Windows Runtime. Sin embargo, cuando creas un tipo en una aplicación de C++/WinRT, puedes elegir si ese tipo es un tipo de Windows Runtime o una clase, estructura o enumeración de C++ normal.

Cualquier página XAML de nuestro proyecto debe ser un tipo de Windows Runtime, por lo que **MainPage** debe ser un tipo de Windows Runtime. En el proyecto de C++/WinRT, **MainPage** ya es un tipo de Windows Runtime, por lo que no es necesario cambiar ese aspecto. En concreto, se trata de una *clase en tiempo de ejecución*.

- Para obtener más información sobre si debes crear una clase en tiempo de ejecución para un tipo determinado, consulta el tema [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).
- En C++/WinRT, la implementación interna de una clase en tiempo de ejecución y sus partes proyectadas (públicas) existen en el formulario de dos clases diferentes. Se conocen como *tipo de implementación* y *tipo proyectado*. Puedes obtener información sobre ellos en el tema mencionado en el punto anterior y también en [Consumo de API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis).
- Para obtener más información sobre la conexión entre clases en tiempo de ejecución e IDL (archivos `.idl`), lee y sigue con el tema [Controles de XAML; enlazar a una propiedad de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property). En ese tema encontrarás una guía por el proceso de creación de una clase en tiempo de ejecución, cuyo primer paso es agregar un nuevo elemento **Archivo Midl (.idl)** al proyecto.

Para **MainPage**, ya tenemos el archivo `MainPage.idl` necesario en el proyecto de C++/WinRT. Esto se debe a que la plantilla de proyecto lo creó automáticamente. Pero más adelante en este tutorial, agregaremos más archivos `.idl` al proyecto.

Pronto veremos una lista de qué IDL se debe agregar exactamente al archivo `MainPage.idl` existente. Antes de eso, tenemos que razonar algunas cosas sobre qué hay que incluir, y qué no, en el IDL.

Para decidir qué miembros de **MainPage** tenemos que declarar en `MainPage.idl` (de modo que se vuelvan parte de la clase en tiempo de ejecución **MainPage**) y cuáles pueden ser simplemente miembros del tipo de implementación **MainPage**, vamos a crear una lista de los miembros de la clase **MainPage** de C#. Para encontrar esos miembros, buscaremos en `MainPage.xaml.cs` y en `SampleConfiguration.cs`.

Encontramos un total de doce campos y métodos `protected` y `private`. Y encontramos los siguientes miembros de `public`.

- El constructor predeterminado `MainPage()`.
- Los campos estáticos **Current** y **FEATURE_NAME**.
- Las propiedades **IsClipboardContentChangedEnabled** y **Scenarios**.
- Los métodos **BuildClipboardFormatsOutputString**, **DisplayToast**, **EnableClipboardContentChangedNotifications** y **NotifyUser**.

Esos miembros de `public` son los candidatos para declarar en `MainPage.idl`. Vamos a examinar cada uno y ver si es necesario que formen parte de la clase en tiempo de ejecución **MainPage** o si solo tienen que formar parte de su implementación.

- El constructor predeterminado `MainPage()`. En el caso de una clase **Page** de XAML, es normal declarar un constructor predeterminado en su IDL. De este modo, el marco de trabajo de la interfaz de usuario de XAML puede activar el tipo.
- El campo estático **Current** se usa desde las páginas de XAML del escenario individual para acceder a la instancia de **MainPage** de la aplicación. Puesto que **Current** no se usa para interoperar con el marco de XAML (ni se usa en unidades de compilación), podríamos reservarlo para que sea únicamente miembro del tipo de implementación. Con tus propios proyectos en casos como este, puede que decidas hacerlo. Pero como el campo es una instancia del tipo proyectado, resulta lógico declararlo en el IDL. Eso es lo que haremos aquí (y hacerlo también hace que el código esté ligeramente más despejado).
- Es un caso similar para el campo estático **FEATURE_NAME**, al que se accede desde el tipo **MainPage**. Una vez más, si eliges declararlo en el IDL, el código estará ligeramente más despejado.
- La propiedad **IsClipboardContentChangedEnabled** solo se usa en la clase **OtherScenarios**. Por lo tanto, durante la portación, simplificaremos un poco las cosas y la convertiremos en un campo privado de la clase en tiempo de ejecución **OtherScenarios**. De modo que no se incluirá en el IDL.
- La propiedad **Scenarios** es una colección de objetos de tipo **Scenario** (un tipo que mencionamos antes). Hablaremos sobre **Scenario** en el siguiente apartado, así que también vamos a dejar la propiedad **Scenarios** para luego.
- Los métodos **BuildClipboardFormatsOutputString**, **DisplayToast** y **EnableClipboardContentChangedNotifications** son funciones de utilidad que tienen más que ver con el estado general del ejemplo que con la página principal. Por lo tanto, durante la portación, vamos a refactorizar estos tres métodos en un nuevo tipo de utilidad denominado **SampleState** (que no es necesario que sea un tipo de Windows Runtime). Por ese motivo, estos tres métodos no se incluirán en el IDL.
- Se llama al método **NotifyUser** desde las páginas de XAML del escenario individual en la instancia de **MainPage** que se devuelve del campo de estático *Current*. Dado que (como ya se ha indicado) **Current** es una instancia del tipo proyectado, es necesario declarar **NotifyUser** en el IDL. **NotifyUser** toma un parámetro de tipo **NotifyType**. Hablaremos sobre esto en el siguiente apartado.

Cualquier miembro con el que quieras enlazar datos también se debe declarar en IDL (tanto si usas `{x:Bind}` como `{Binding}`). Para obtener más información, consulta [Enlace de datos](/windows/uwp/data-binding/).

Estamos progresando: estamos desarrollando una lista de los miembros que vamos a agregar al archivo `MainPage.idl`, y de los que no. Pero todavía tenemos que analizar los la propiedad **Scenarios** y el tipo **NotifyType**. Así que hagámoslo ahora.

### <a name="idl-for-the-scenario-and-notifytype-types"></a>IDL para los tipos **Scenario** y **NotifyType**

La clase de **Scenario** se define en `SampleConfiguration.cs`. Tenemos que tomar la decisión de cómo portar esa clase a C++/WinRT. De forma predeterminada, probablemente la convertiríamos en un `struct` de C++ común. Pero si **Scenario** se usa en archivos binarios o para interoperar con el marco de XAML, hay que declararlo en el IDL como un tipo de Windows Runtime.

Si analizamos el código fuente de C#, encontramos que **Scenario** se usa en este contexto.

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

Una colección de objetos de **Scenario** se asigna a la propiedad [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) de un **ListBox** (que es un control de elementos). Dado que **Scenario** *sí* debe interoperar con XAML, debe ser un tipo de Windows Runtime. Por lo tanto, debe definirse en el IDL. Definir el tipo **Scenario** en el IDL hace que el sistema de compilación de C++/WinRT genere automáticamente una definición de código fuente de **Scenario** en un archivo de encabezado en segundo plano (cuyo nombre y ubicación no son importantes para este tutorial).

Además, recordarás que **MainPage.Scenarios** es una colección de objetos de **Scenario**, que acabamos de decir que tiene que incluirse en el IDL. Por ese motivo, **MainPage.Scenarios** también tiene que declararse en el IDL.

**NotifyType** es un `enum` declarado en `MainPage.xaml.cs` de C#. Dado que pasamos **NotifyType** a un método que pertenece a la clase en tiempo de ejecución **MainPage**, **NotifyType** también tiene que ser un tipo de Windows Runtime; y tiene que definirse en `MainPage.idl`.

Ahora vamos a agregar al archivo `MainPage.idl` los nuevos tipos y el nuevo miembro de **Mainpage** que hemos decidido declarar en el IDL. Al mismo tiempo, quitaremos del IDL los miembros del marcador de posición de **Mainpage** que nos proporcionó la plantilla de proyecto de Visual Studio.

Por lo tanto, en el proyecto de C++/WinRT, abre `MainPage.idl` y edítalo para que se parezca a la lista siguiente. Ten en cuenta que una de las modificaciones consiste en cambiar el nombre del espacio de nombres de **Clipboard** a **SDKTemplate**. Si quieres, simplemente, puedes reemplazar todo el contenido de `MainPage.idl` por el siguiente código. Otro retoque a tener en cuenta es que estamos cambiando el nombre de **Scenario::ClassType** a **Scenario::ClassName**.

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> Para más información sobre el contenido de un archivo `.idl` en un proyecto de C++/WinRT, consulta [Lenguaje de definición de interfaz de Microsoft 3.0](/uwp/midl-3/).

Con tu propio trabajo de portabilidad, es posible que no quieras ni necesites cambiar el nombre del espacio de nombres como hicimos anteriormente. Lo hacemos aquí solo porque el espacio de nombres predeterminado del proyecto de C# que estamos portando es **SDKTemplate**, mientras que el nombre del proyecto y del ensamblado es **Clipboard**.

Pero, a medida que avancemos con la portación en este tutorial, cambiaremos en el código fuente cada aparición del nombre del espacio de nombres **Clipboard** a **SDKTemplate**. También hay un lugar en las propiedades del proyecto de C++/WinRT donde aparece el nombre del espacio de nombres **Clipboard**, así que aprovecharemos para cambiarlo ahora.

En Visual Studio, para el proyecto de C++/WinRT, establece la propiedad  **Common Properties** \> **C++/WinRT** \> **Root Namespace** del proyecto en el valor *SDKTemplate*.

### <a name="save-the-idl-and-re-generate-stub-files"></a>Almacenamiento del IDL y regeneración de los archivos de código auxiliar

El tema [Controles XAML; enlazar a una propiedad de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property) presenta la noción de *archivos de código auxiliar* y muestra un tutorial de estos en acción. También mencionamos los archivos de código auxiliar anteriormente en este tema al explicar que el sistema de compilación de C++/WinRT convierte el contenido de los archivos `.idl` en metadatos de Windows y, a continuación, a partir de esos metadatos, una herramienta denominada `cppwinrt.exe` genera archivos de código auxiliar en los que puede basar la implementación.

Cada vez que se agregas, quitas o cambias algo en tu IDL y en la compilación, el sistema de compilación actualiza las implementaciones de código auxiliar en esos archivos de código auxiliar. Por lo tanto, cada vez que cambies tu IDL y la compilación, se recomienda que veas esos archivos de código auxiliar, copies todas las firmas modificadas y las pegues en el proyecto. Ofreceremos más detalles y ejemplos de cómo hacerlo dentro de un momento. Pero la ventaja de hacerlo es proporcionarte una forma sin errores de saber en todo momento cuál debe ser la forma del tipo de implementación y cuál debe ser la firma de sus métodos.

En este punto del tutorial, hemos terminado de editar el archivo `MainPage.idl` por el momento, así que deberías guardarlo ahora. El proyecto no se compilará hasta su finalización en este momento, pero crear una compilación ahora es útil porque vuelve a generar los archivos de código auxiliar de **MainPage**.

En este proyecto de C++/WinRT, los archivos de código auxiliar se generan en la carpeta `\Clipboard\Clipboard\Generated Files\sources`. Podrás encontrarlos allí después de que se haya completado la compilación parcial (de nuevo, como se espera, la compilación no se completará correctamente. Pero el paso que nos interesa&mdash;generar archivos de código auxiliar&mdash;*sí* se habrá completado correctamente). Los archivos que nos interesan son `MainPage.h` y `MainPage.cpp`.

En esos dos archivos de código auxiliar, verás nuevas implementaciones de código auxiliar de los miembros de **MainPage** que hemos agregado al IDL (**Current** y **FEATURE_NAME**, por ejemplo). Podrás copiar esas implementaciones de código auxiliar en los archivos `MainPage.h` y `MainPage.cpp` que ya están en el proyecto. Al mismo tiempo, al igual que hicimos con el IDL, quitaremos de los archivos existentes los miembros del marcador de posición **Mainpage** que nos ha proporcionado la plantilla de proyecto de Visual Studio (la propiedad ficticia denominada **MyProperty**y el controlador de eventos denominado **ClickHandler**).

De hecho, el único miembro de la versión actual de **MainPage** que queremos conservar es el constructor.

Una vez que hayas copiado los nuevos miembros de los archivos de código auxiliar, hayas eliminado los miembros que no queremos y hayas actualizado el espacio de nombres, los archivos `MainPage.h` y `MainPage.cpp` del proyecto deberían tener un aspecto como el de las listas de código siguientes. Observa que hay dos tipos de **MainPage**. Uno en el espacio de nombres de **implementación** y otro en el espacio de nombres **factory_implementation**. El único cambio que hemos hecho en **factory_implementation** es agregar **SDKTemplate** a su espacio de nombres.

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

En el caso de las cadenas, C# usa **System.String**. Consulta el método **MainPage.NotifyUser** para obtener un ejemplo. En el IDL, declaramos una cadena con **String** y, cuando la herramienta `cppwinrt.exe` genera el código de C++/WinRT automáticamente, usa el tipo [**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring). Cada vez que nos encontremos con una cadena en el código de C#, la portaremos a **winrt::hstring**. Para más información, consulta [Control de cadenas en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings).

Para una explicación de los parámetros de `const&` en las firmas de método, consulte [Paso de parámetros](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing).

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>Actualización de todas las declaraciones/referencias restantes del espacio de nombres y compilación

Antes de compilar el proyecto de C++/WinRT, busca las declaraciones (y las referencias) del espacio de nombres **Clipboard** y cámbialas a **SDKTemplate**.

- `MainPage.xaml` y `App.xaml`. El espacio de nombres aparece en los valores de los atributos `x:Class` y `xmlns:local`.
- `App.idl`.
- `App.h`.
- `App.cpp`. Hay dos directivas de `using namespace` (busca la subcadena `using namespace Clipboard`) y dos calificaciones del tipo **MainPage** (busca `Clipboard::MainPage`). Tienes que cambiarlas.

Dado que hemos quitado el controlador de eventos de **MainPage**, también ve a `MainPage.xaml` y elimina el elemento **Button** del marcado.

Guarda todos los archivos. Limpia la solución (**Build** > **Clean Clipboard**) y, luego, compílala. La compilación se completará correctamente si los cambios que has hecho hasta ahora son válidos.

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>Implementación de los miembros de **MainPage** que se han declarado en el IDL

#### <a name="the-constructor-current-and-feature_name"></a>El constructor, **Current**, y **FEATURE_NAME**

Este es el código pertinente (del proyecto de C#) que tenemos que portar.

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

Pronto, volveremos a reutilizar `MainPage.xaml` en su totalidad (al copiarlo). Por ahora, vamos a agregar temporalmente un elemento **TextBlock**, con el nombre adecuado, en el archivo `MainPage.xaml` del proyecto de C++/WinRT.

**FEATURE_NAME** es un campo estático de **MainPage** (un campo `const` de C# tiene, en esencia, un comportamiento estático), definido en `SampleConfiguration.cs`. En el caso de C++/WinRT, en lugar de un campo (estático), lo convertiremos en la expresión de una propiedad (estática) de solo lectura de C++/WinRT. La forma en que C++/WinRT expresa un captador de propiedad es como una función que devuelve el valor de la propiedad y no toma ningún parámetro (un descriptor de acceso). Por lo tanto, el campo estático **FEATURE_NAME** de C# se convierte en la función de descriptor de acceso estático **FEATURE_NAME** de C++/WinRT (en este caso, devuelve el literal de cadena).

Por cierto, haríamos lo mismo si estuviésemos portando una propiedad de solo lectura de C#. En el caso de una propiedad grabable de C#, la manera en que C++/WinRT expresa un establecedor de propiedad es como una función `void` que toma el valor de la propiedad como parámetro (un mutador). En todo caso, si el campo o la propiedad de C# son estáticos, también lo son el descriptor de acceso o el mutador de C++/WinRT.

**Current** es un campo estático (no una constante) de **MainPage**. Una vez más, lo convertiremos en (la expresión de C++/WinRT de) una propiedad de solo lectura y lo volveremos a hacer estático. Donde **FEATURE_NAME** es constante, **Current** no lo es. Por lo tanto, en C++/WinRT se necesitará un campo de respaldo, y el descriptor de acceso devolverá eso. De este modo, en el proyecto de C++/WinRT, declararemos en `MainPage.h` un campo estático privado denominado **current**, definiremos/inicializaremos **current** en `MainPage.cpp` (porque tiene una duración de almacenamiento estática) y accederemos a él a través de una función de descriptor de acceso estática pública denominada **Current**.

El propio constructor realiza un par de asignaciones, que son sencillas de portar.

En el proyecto de C++/WinRT, agrega un nuevo elemento **Visual C++**  > **Code** > **C++ File (.cpp)** con el nombre `SampleConfiguration.cpp`.

Edita `MainPage.xaml`, `MainPage.h`, `MainPage.cpp` y `SampleConfiguration.cpp` para que coincidan con las listas a continuación.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

Además, asegúrate de eliminar los cuerpos de función existentes de `MainPage.cpp` para **MainPage::Current()** y **MainPage::FEATURE_NAME()** , porque ahora definimos esos métodos en otra parte.

Como puedes ver, **MainPage::current** se declara como de tipo **SDKTemplate::MainPage**, que es el tipo proyectado. No es de tipo **SDKTemplate::implementation::MainPage**, que es el tipo de implementación. El tipo proyectado es el que se ha diseñado para usarse dentro del proyecto para la interoperación de XAML, o a través de archivos binarios. El tipo de implementación es lo que usas para implementar las funciones que has expuesto en el tipo proyectado. Dado que la declaración de **MainPage::current** (en `MainPage.h`) aparece dentro del espacio de nombres de implementación (**winrt::SDKTemplate::implementation**), un **MainPage** no calificado habría hecho referencia al tipo de implementación. Por lo tanto, calificamos con **SDKTemplate::** para que quede claro que queremos que **MainPage::current** sea una instancia del tipo proyectado **winrt::SDKTemplate::MainPage**.

En el constructor, hay algunos puntos relacionados con `MainPage::current = *this;` que merecen una explicación.
- Cuando usas el puntero `this` dentro de un miembro del tipo de implementación, el puntero `this` es evidentemente *un puntero al tipo de implementación*.
- Para convertir el puntero `this` en el tipo proyectado correspondiente, tienes que desreferenciarlo. Siempre que generes el tipo de implementación a partir del IDL (como lo hemos hecho aquí), el tipo de implementación tiene un operador de conversión que lo convierte a su tipo proyectado. Este es el motivo por el que funciona la asignación en este caso.

Para más información sobre estos detalles, consulta [Crear instancias y devolver tipos de implementación e interfaces](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces).

Además, el constructor es `SampleTitle().Text(FEATURE_NAME());`. La parte `SampleTitle()` es una llamada a una función de descriptor de acceso simple denominada **SampleTitle**, que devuelve el elemento **TextBlock** que agregamos al XAML. Siempre que `x:Name` un elemento XAML, el compilador XAML genera automáticamente un descriptor de acceso que se denomina para el elemento. La parte `.Text(...)` llama a la función **Text** del mutador en el objeto **TextBlock** que devolvió el descriptor de acceso **SampleTitle**. Y `FEATURE_NAME()` llama a nuestra función estática de descriptor de acceso **MainPage::FEATURE_NAME** para devolver el literal de cadena. En conjunto, esa línea de código establece la propiedad **Text** del objeto **TextBlock** denominado *SampleTitle*.

Ten en cuenta que, puesto que las cadenas son anchas en Windows Runtime, para portar un literal de cadena le agregaremos el prefijo de codificación de carácter ancho `L`. Por lo tanto, cambiamos (por ejemplo) "un literal de cadena" a L"un literal de cadena". Consulta también [Literales de cadena anchos](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals).

#### <a name="scenarios"></a>**Escenarios**

Este es el código de C# pertinente que tenemos que portar.

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

A partir de nuestra investigación anterior, sabemos que esta colección de objetos de **Scenario** se muestra en un control **ListBox**. En C++/WinRT, hay límites al *tipo de colección* que podemos asignar a la propiedad **ItemsSource** de un control de elementos. La colección tiene que ser un vector o un vector observable, y sus elementos deben ser uno de los siguientes.

- Clases en tiempo de ejecución, o bien
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).

En el caso de **IInspectable**, si los elementos no son en sí mismos clases en tiempo de ejecución, esos elementos deben ser de un tipo al que se puede aplicar la conversión boxing y la conversión unboxing desde [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Y eso significa que deben ser tipos de Windows Runtime (consulta [Conversión boxing y unboxing de valores escalares a IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing)).

En este caso práctico, no hicimos que **Scenario** fuese una clase en tiempo de ejecución. Sin embargo, sigue siendo una opción razonable. Y habrá casos en tu propio trabajo de portabilidad en el que una clase en tiempo de ejecución será sin dudas el procedimiento adecuado. Por ejemplo, si necesitas hacer que el tipo de elemento sea *observable* (consulta [Controles de XAML; enlazar a una propiedad de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property)), o si el elemento necesita tener métodos por cualquier otro motivo, y es más que un simple conjunto de miembros de datos.

Dado que en este tutorial *no* asignamos una clase en tiempo de ejecución para el tipo **Scenario**, debemos pensar en la conversión boxing. Si hubiéramos hecho que **Scenario** fuese un `struct` de C++ normal, no podríamos aplicarle la conversión boxing. Pero hemos declarado **Scenario** como `struct` en IDL, por lo que *podemos* aplicarle la conversión boxing.

Nos queda la opción de aplicar la conversión boxing a **Scenario** de antemano, o esperar hasta que estemos a punto de asignarlo a la propiedad **ItemsSource** y aplicarle la conversión boxing según Just-In-Time. He aquí algunas consideraciones sobre estas dos opciones.

- Conversión boxing por anticipado. Para esta opción, nuestro miembro de datos es una colección de **IInspectable** lista para asignarse a la interfaz de usuario. Durante la inicialización, aplicamos la conversión boxing a los objetos **Scenario** en ese miembro de datos. Solo se necesita una copia de esa colección, pero debemos aplicar una conversión unboxing a un elemento cada vez que tenemos que leer sus campos.
- Conversión boxing Just-In-Time. Para esta opción, nuestro miembro de datos es una colección de **Scenario**. Cuando llegue el momento de asignarse a la interfaz de usuario, aplicamos la conversión boxing a los objetos **Scenario** del miembro de datos en una nueva colección de **IInspectable**. Podemos leer los campos de los elementos del miembro de datos sin la conversión unboxing, pero se necesitan dos copias de la colección.

Como puedes ver, en el caso de una colección pequeña como esta, las ventajas y desventajas hacen que sea un empate. Por lo tanto, en este caso práctico, vamos a usar la opción Just-In-Time.

El miembro **scenarios** es un campo de **MainPage**, definido e inicializado en `SampleConfiguration.cs`. Y **Scenarios** es una propiedad de solo lectura de **MainPage**, que se define en `MainPage.xaml.cs` (y se implementa para devolver simplemente el campo **scenarios**). Haremos algo parecido en el proyecto de C++/WinRT; pero haremos que los dos miembros sean estáticos (ya que solo necesitamos una instancia en la aplicación y para que podamos acceder a ellos sin necesidad de una instancia de clase). Y los denominaremos *scenariosInner* y *scenarios*, respectivamente. Declararemos *scenariosInner* en `MainPage.h`. Y, dado que tiene una duración de almacenamiento estática, la definiremos o inicializaremos en un archivo `.cpp` (en este caso,`SampleConfiguration.cpp`).

Edita `MainPage.h` y `SampleConfiguration.cpp` para que coincidan con las listas a continuación.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

Además, asegúrate de eliminar el cuerpo de la función existente de `MainPage.cpp` para **MainPage::scenarios()** , porque ahora vamos a definir ese método en el archivo de encabezado.

Como puedes ver, en `SampleConfiguration.cpp`, inicializamos el miembro de datos estático *scenariosInner* llamando a una función auxiliar de C++/WinRT denominada [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector). Esta función crea un nuevo objeto de colección de Windows Runtime automáticamente y lo devuelve como una interfaz [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_). Puesto que, en este ejemplo, la colección no es *observable* (no es necesario que lo sea, porque no agrega ni quita elementos después de la inicialización), podríamos haber optado por llamar a [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector). Esa función devuelve la colección como una interfaz [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_).

Para más información sobre las colecciones y cómo enlazarlas, consulta [Controles de elementos de XAML; enlazar a una colección C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection) y [Colecciones con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/collections).

El código de inicialización que acabas de agregar hace referencia a tipos que todavía no están en el proyecto (por ejemplo, **winrt::SDKTemplate::CopyText**. Para solucionarlo, vamos a agregar al proyecto cinco nuevas páginas XAML en blanco.

#### <a name="add-five-new-blank-xaml-pages"></a>Adición de cinco nuevas páginas XAML en blanco

Agrega un nuevo elemento **XAML** > **Blank Page (C++/WinRT)**   al proyecto (asegúrate de que sea la plantilla de elementos **Blank Page (C++/WinRT)** y no la **Blank Page**). Asígnale el nombre  `CopyText`. La nueva página XAML se define en el espacio de nombres **SDKTemplate**, que es lo que queremos.

Repite el proceso anterior otras cuatro veces y denomina las páginas XAML `CopyImage`, `CopyFiles`, `HistoryAndRoaming`y `OtherScenarios`.

Ahora podrás volver a compilar si lo quieres.

#### <a name="notifyuser"></a>**NotifyUser**

En el proyecto de C#, encontrarás la implementación del método **MainPage.NotifyUser** en `MainPage.xaml.cs`. **MainPage.NotifyUser** tiene una dependencia en **MainPage.UpdateStatus**y, a su vez, ese método tiene dependencias de elementos de XAML que todavía no se han portado. Por lo tanto, por ahora solo vamos a crear código auxiliar para un método **UpdateStatus** en el proyecto de C++/WinRT y vamos a portarlo más adelante.

Este es el código de C# pertinente que tenemos que portar.

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser** usa la enumeración [**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority). En C++/WinRT, siempre que quieras usar un tipo de un espacio de nombres de Windows, tienes que incluir el archivo de encabezado del espacio de nombres de Windows de C++/WinRT correspondiente (para más información sobre esto, consulta [Introducción a C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started)). En este caso, como verás en la lista de código a continuación, el encabezado es `winrt/Windows.UI.Core.h` y lo incluiremos en `pch.h`.

**UpdateStatus** es privado. Así que lo haremos un método privado en el tipo de implementación **MainPage**. **UpdateStatus** no está diseñado para que se lo llame en la clase en tiempo de ejecución, así que no se declarará en el IDL.

Después de portar **MainPage.NotifyUser** y crear el código auxiliar para **MainPage.UpdateStatus**, esto es lo que tenemos en el proyecto de C++/WinRT. Después de esta lista de código, examinaremos algunos de los detalles.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

En C#, puedes usar la notación de puntos para aplicar *dot into* a las propiedades anidadas. Por lo tanto, el tipo **MainPage** de C#  puede acceder a su propia propiedad **Dispatcher** con la sintaxis `Dispatcher`. Y C# puede, además, aplicar *dot into* a ese valor con una sintaxis como `Dispatcher.HasThreadAccess`. En C++/WinRT, las propiedades se implementan como funciones de descriptor de acceso, por lo que la sintaxis difiere solo en que agregas paréntesis para cada llamada de función.

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

Cuando la versión de C# de **NotifyUser** llama a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), implementa el delegado de devolución de llamada asincrónico como una función lambda. La versión de C++/WinRT hace lo mismo, pero la sintaxis es un poco diferente. En C++/WinRT, *capturamos* los dos parámetros que vamos a usar, así como el puntero `this` (ya que vamos a llamar a una función miembro). Hay más información sobre la implementación de delegados como expresiones lambda y ejemplos de código en el tema [Control de eventos mediante delegados en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). Además, podemos descartar la parte `var task =` en este caso concreto. No estamos esperando el objeto asincrónico devuelto, así que no es necesario almacenarlo. 

### <a name="implement-the-remaining-mainpage-members"></a>Implementación de los miembros de **MainPage** restantes

Vamos a crear una lista completa de los miembros de **MainPage** (implementados entre `MainPage.xaml.cs` y `SampleConfiguration.cs`) para poder ver cuáles hemos portado hasta ahora y cuáles están pendientes todavía.

|Miembro|Acceso|Estado|
|-|-|-|
|Constructor **MainPage**|`public`|Portado|
|Propiedad **Current**|`public`|Portado|
|Propiedad **FEATURE_NAME**|`public`|Portado|
|Propiedad **IsClipboardContentChangedEnabled**|`public`|Sin iniciar|
|Propiedad **Scenarios**|`public`|Portado|
|Método **BuildClipboardFormatsOutputString**|`public`|Sin iniciar|
|Método **DisplayToast**|`public`|Sin iniciar|
|Método **EnableClipboardContentChangedNotifications**|`public`|Sin iniciar|
|Método **NotifyUser**|`public`|Portado|
|Método **OnNavigatedTo**|`protected`|Sin iniciar|
|Campo **isApplicationWindowActive**|`private`|Sin iniciar|
|Campo **needToPrintClipboardFormat**|`private`|Sin iniciar|
|Campo **scenarios**|`private`|Portado|
|Método **Button_Click**|`private`|Sin iniciar|
|Método **DisplayChangedFormats**|`private`|Sin iniciar|
|Método **Footer_Click**|`private`|Sin iniciar|
|Método **HandleClipboardChanged**|`private`|Sin iniciar|
|Método **OnClipboardChanged**|`private`|Sin iniciar|
|Método **OnWindowActivated**|`private`|Sin iniciar|
|Método **ScenarioControl_SelectionChanged**|`private`|Sin iniciar|
|Método **UpdateStatus**|`private`|Creado código auxiliar|

Entonces, hablaremos sobre los miembros que todavía no hemos portado en los siguientes apartados.

> [!NOTE]
> De vez en cuando, vamos a encontrar referencias en el código fuente a los elementos de la interfaz de usuario en el marcado XAML (en `MainPage.xaml`). A medida que lleguemos a estas referencias, las solucionaremos temporalmente agregando elementos de marcador de posición simples en el código XAML. De este modo, el proyecto se seguirá compilando después de cada apartado. La alternativa es resolver las referencias copiando ahora *todo* el contenido de `MainPage.xaml` del proyecto de C# al proyecto de C++/WinRT. Pero si hacemos eso, pasará mucho tiempo hasta que podamos hacer una pausa y volver a compilar (con lo que posiblemente quedaran ocultos los errores tipográficos u otros que cometamos durante el proceso).
>
> Una vez que hayamos terminado de portar el código imperativo para la clase **MainPage**, *luego* copiaremos el contenido del archivo XAML y estaremos seguros de que el proyecto aún se compilará.

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

Se trata de una propiedad get-set de C# cuyo valor predeterminado es `false`. Es miembro de **MainPage** y se define en `SampleConfiguration.cs`.

En el caso de C++/WinRT, necesitaremos una función de descriptor de acceso, una función de mutador y un miembro de datos de respaldo como campo. Dado que **IsClipboardContentChangedEnabled** representa el estado de uno de los escenarios en el ejemplo, en lugar del estado de **MainPage** mismo, crearemos los nuevos miembros en un nuevo tipo de utilidad denominado **SampleState**. E implementaremos eso en el archivo de código fuente `SampleConfiguration.cpp`, y haremos que los miembros sean `static` (ya que solo necesitamos una instancia en la aplicación y para que podamos acceder a ellos sin necesidad de una instancia de clase).

Para acompañar a `SampleConfiguration.cpp`, en el proyecto de C++/WinRT, agrega un nuevo elemento **Visual C++**  > **Code** > **Header File (.h)** con el nombre `SampleConfiguration.h`. Edita `SampleConfiguration.h` y `SampleConfiguration.cpp` para que coincidan con las listas a continuación.

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

De nuevo, un campo con almacenamiento `static` (como **SampleState:: isClipboardContentChangedEnabled**) debe definirse una vez en la aplicación, y un archivo `.cpp` es un buen lugar para ello (en este caso, `SampleConfiguration.cpp`).

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

Este método es un miembro público de **MainPage** y se define en `SampleConfiguration.cs`.

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

En C++/WinRT, haremos que **BuildClipboardFormatsOutputString** sea un método estático público de **SampleState**. Podemos hacerlo `static` porque no tiene acceso a ningún miembro de la instancia.

Para usar los tipos **Clipboard** y **DataPackageView** en C++/WinRT, es necesario incluir el archivo de encabezado de espacio de nombres de Windows de C++/WinRT `winrt/Windows.ApplicationModel.DataTransfer.h`.

En C#, la propiedad **DataPackageView.AvailableFormats** es una **IReadOnlyList**, así que podemos acceder a su propiedad **Count**. En C++/WinRT, la función de descriptor de acceso **DataPackageView::AvailableFormats** devuelve una **IVectorView**, que tiene una función de descriptor de acceso **Size** a la que podemos llamar.

Para portar el uso del tipo **System.Text.StringBuilder** de C#, aprovecharemos el tipo estándar de C++ [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream). Ese tipo es un flujo de salida para cadenas anchas (y, para usarlo, es necesario incluir el archivo de encabezado `sstream`). En lugar de usar un método **Append** como sucede con **StringBuilder**, usa el [operador de inserción](/cpp/standard-library/using-insertion-operators-and-controlling-format) (`<<`) con un flujo de salida, como **wostringstream**. Para más información, consulta [Programación con iostream](/cpp/standard-library/iostream-programming) y [Formato de cadenas en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings#formatting-strings).

El código de C# crea un **StringBuilder** con la palabra clave `new`. En C#, los objetos son tipos de referencia de forma predeterminada, que se declaran en el montón con `new`. En C++ estándar moderno, los objetos son tipos de valor de forma predeterminada, que se declaran en la pila (sin usar `new`). Por lo tanto, portamos `StringBuilder output = new StringBuilder();` a C++/WinRT simplemente con `std::wostringstream output;`.

La palabra clave `var` de C# pide al compilador que infiera un tipo. Portas `var` a `auto` en C++/WinRT. Pero, en C++/WinRT, hay casos en los que (para evitar copias), quieres una *referencia* a un tipo inferido (o deducido) y expresas una referencia lvalue a un tipo deducido con `auto&`. También hay casos en los que quieres un tipo especial de referencia que se enlace correctamente, tanto si se inicializa con *lvalue* como con *rvalue*. Y lo expresas con `auto&&`. Esta es la forma que se usa en el bucle `for` en el código portado a continuación. Para ver una introducción a *lvalues* y *rvalues*, consulta [Categorías de valor y referencias a ellas](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories).

Edita `pch.h`, `SampleConfiguration.h` y `SampleConfiguration.cpp` para que coincidan con las listas a continuación.

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> La sintaxis de la línea de código `DataPackageView clipboardContent{ Clipboard::GetContent() };` usa una característica de C++ estándar moderno llamada *inicialización uniforme*, con su uso característico de llaves en lugar de un signo de `=`. Esa sintaxis deja claro que se está llevando a cabo la inicialización, en lugar de la asignación. Si prefieres la forma de sintaxis que se *parece* a la asignación (pero realmente no lo es), puedes reemplazar la sintaxis anterior por su equivalente: `DataPackageView clipboardContent = Clipboard::GetContent();`. Sin embargo, resulta conveniente familiarizarse con ambas formas de expresar la inicialización, ya que es probable que veas que ambas se usan con frecuencia en los códigos con que te encuentres.

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast** es un método estático público de la clase **MainPage** de C# y encontrarás que se define en `SampleConfiguration.cs`. En C++/WinRT, lo convertiremos en un método estático público de **SampleState**.

Ya nos hemos encontrado con la mayoría de los detalles y técnicas que son pertinentes para portar este método. Un nuevo elemento que debes tener en cuenta es que vas a portar un literal de cadena textual de C# (`@`) a un [literal de cadena sin formato](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11) estándar de C++ (`LR`).

Además, al hacer referencia a los tipos [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) y [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) en C++/WinRT, puedes calificarlos por nombre de espacio de nombres o puedes editar `SampleConfiguration.cpp` y agregar directivas `using namespace` como en el ejemplo siguiente.

```cppwinrt
using namespace Windows::UI::Notifications;
```

Tienes la misma opción al hacer referencia al tipo [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) y siempre que hagas referencia a cualquier otro tipo de Windows Runtime.

Además de esos elementos, simplemente sigue las mismas instrucciones que antes para completar los pasos siguientes.

- Declara el método en `SampleConfiguration.h` y defínelo en `SampleConfiguration.cpp`.
- Edita `pch.h` para incluir los archivos de encabezado de espacio de nombres de Windows de C++/WinRT necesarios.
- Construye objetos de C++/WinRT en la pila, no en el montón.
- Reemplaza las llamadas a los descriptores de acceso de la propiedad get con la sintaxis de llamada de función (`()`).

Una causa muy común de errores del compilador o del enlazador es olvidar incluir los archivos de encabezado del espacio de nombres de Windows de C++/WinRT que necesitas. Para más información sobre un posible error, consulta [¿Por qué el enlazador me da un error "LNK2019: símbolo externo sin resolver"?](/windows/uwp/cpp-and-winrt-apis/faq#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)

Si quiere seguir con el tutorial y portar **DisplayToast** usted mismo, puede comparar sus resultados con el código de la versión de C++/WinRT del archivo ZIP del código fuente de [ejemplo de Clipboard](/samples/microsoft/windows-universal-samples/clipboard/) que ha descargado.

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications** es un método estático público de la clase **MainPage** de C# y se define en `SampleConfiguration.cs`.

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

En C++/WinRT, lo convertiremos en un método estático público de **SampleState**.

En C#, usas la sintaxis de operador `+=` y `-=` para registrar y revocar los delegados de control de eventos. En C++/WinRT, tienes varias opciones sintácticas para registrar o revocar un delegado, tal como se describe en [Control de eventos mediante delegados en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). Pero la forma general es que registras y revocas mediante llamadas a un par de funciones designadas para el evento. Para registrar, pasas el delegado a la función de registro y, a cambio, recuperas un token de revocación ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). Para revocar, pasas ese token a la función de revocación. En este caso, el controlador es estático y (como puedes ver en la lista de código siguiente) la sintaxis de la llamada de función es sencilla.

Los tokens similares *se* usan realmente, en segundo plano, en C#. Pero el lenguaje hace que el detalle esté implícito. C++/WinRT lo hace explícito.

El tipo **object** aparece en las firmas del controlador de eventos de C#. En el lenguaje C#, **object** es un [alias](/dotnet/csharp/language-reference/builtin-types/reference-types) para el tipo [**System.Object**](/dotnet/api/system.object) de .NET. El equivalente en C++/WinRT es [**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Por lo tanto, verás **IInspectable** en los controladores de eventos de C++/WinRT.

Edita `SampleConfiguration.h` y `SampleConfiguration.cpp` para que coincidan con las listas a continuación.

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

Deja los delegados de control de eventos (**OnClipboardChanged** y **OnWindowActivated**) como código auxiliar por ahora. Ya están en la lista de miembros para portar, por lo que los abordaremos en apartados posteriores.

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo** es un método protegido de la clase **MainPage** de C# y se define en `MainPage.xaml.cs`. Aquí está, junto con **ListBox** de XAML al que hace referencia.

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

Es un método importante e interesante, porque aquí es donde la colección de objetos **Scenario** se asignan a la interfaz de usuario. El código en C# crea una clase [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1) de objetos **Scenario** y la asigna a la propiedad [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) de **ListBox** (que es un control de elementos). Y, en C#, usamos [interpolación de cadenas](/dotnet/csharp/language-reference/tokens/interpolated) para generar el título de cada objeto **Scenario** (ten en cuenta el uso del carácter especial `$`).

En C++/WinRT, haremos que **OnNavigatedTo** sea un método público de **MainPage**. Y agregaremos un elemento **ListBox** auxiliar a XAML para que la compilación se complete correctamente. Después de la lista de código, examinaremos algunos de los detalles.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

Una vez más, llamamos a la función [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector), pero esta vez para crear una colección de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Esto era parte de la decisión que tomamos para realizar la conversión boxing de los objetos **Scenario** según Just-In-Time.

Y, en lugar de este uso de la [interpolación de cadenas](/dotnet/csharp/language-reference/tokens/interpolated) en C#, usamos una combinación de la función [**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) función y el [operador de concatenación](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator) de **winrt::hstring**.

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

En C#, **isApplicationWindowActive** es un campo privado simple `bool` que pertenece a la clase **MainPage** y se define en `SampleConfiguration.cs`. Se establece de forma predeterminada en `false`. En C++/WinRT, lo convertiremos en un campo estático público de **SampleState** (por los motivos que ya hemos descrito) en los archivos `SampleConfiguration.h` y `SampleConfiguration.cpp`, con el mismo valor predeterminado.

Ya hemos visto cómo declarar, definir e inicializar un campo estático. Como recordatorio, repasa lo que hicimos con el campo **isClipboardContentChangedEnabled** y haz lo mismo con **isApplicationWindowActive**.

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

El mismo patrón que **isApplicationWindowActive** (consulta el encabezado inmediatamente anterior a este).

#### <a name="button_click"></a>**Button_Click**

**Button_Click** es un método privado (de control de eventos) de la clase **MainPage** de C# y se define en `MainPage.xaml.cs`. Aquí está, junto con el elemento **SplitView** XAML al que hace referencia y el elemento **ToggleButton** que lo registra.

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
<ToggleButton Click="Button_Click" .../>
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

Y este es el equivalente, portado a C++/WinRT. Ten en cuenta que en la versión de C++/WinRT, el controlador de eventos es `public` (como puedes ver, lo declaras *antes* de las declaraciones de `private:`). Esto se debe a que un controlador de eventos registrado en el marcado XAML, como este, debe estar definido como `public` en C++/WinRT para que el marcado XAML pueda acceder a él. Si registras un controlador de eventos en código imperativo (como hicimos en **MainPage::EnableClipboardContentChangedNotifications** anteriormente), no es necesario que el controlador de eventos tenga el valor `public`.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

En C#, **DisplayChangedFormats** es un método privado que pertenece a la clase **MainPage** y se define en `SampleConfiguration.cs`.

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

En C++/WinRT, lo convertiremos en un campo estático privado de **SampleState** (no accede a ningún miembro de la instancia), en los archivos `SampleConfiguration.h` y `SampleConfiguration.cpp`. El código en C# de este método no usa **System.Text.StringBuilder**; pero tiene un formato de cadena suficiente como para que la versión de C++/WinRT sea otro buen lugar para usar **std::wostringstream**.

En lugar de la propiedad estática [**System.Environment.NewLine**](/dotnet/api/system.environment.newline), que se usa en el código de C#, insertaremos `std::endl` estándar de C++ (un carácter de nueva línea) en el flujo de salida.

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

Hay una pequeña ineficacia en el diseño de la versión de C++/WinRT anterior. En primer lugar, creamos **std::wostringstream**. Pero también llamamos al método **BuildClipboardFormatsOutputString** (que portamos antes). Ese método crea su propio **std::wostringstream**. Y convierte su flujo en **winrt::hstring** y devuelve eso. Llamamos a la función [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) para devolver al tipo **hstring** devuelta en una cadena de estilo C y, luego, insertamos eso en el flujo. Sería más eficiente crear solo una **std::wostringstream** y pasar (una referencia a) eso, de modo que los métodos puedan insertar cadenas allí directamente.

Eso es lo que hacemos en la versión de C++/WinRT del código fuente del [ejemplo de Clipboard](/samples/microsoft/windows-universal-samples/clipboard/) (del archivo ZIP que ha descargado). En ese código fuente, hay un nuevo método estático privado denominado **SampleState::AddClipboardFormatsOutputString**, que toma y opera con una referencia a un flujo de salida. Y, luego, los métodos **SampleState::DisplayChangedFormats** y **SampleState::BuildClipboardFormatsOutputString** se refactorizan para llamar a ese nuevo método. Es funcionalmente equivalente a las listas de código de este tema, pero es más eficiente.

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click** es un controlador de eventos asincrónico que pertenece a la clase **MainPage** de C# y se define en `MainPage.xaml.cs`. La siguiente lista de código es funcionalmente equivalente al método en el código fuente que descargaste. Pero aquí lo he desempaquetado de una línea a cuatro, para que sea más fácil ver lo que hace y, por consiguiente, cómo deberíamos portarlo.

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

Si bien técnicamente el método es asincrónico, no hace nada después de `await`, por lo que no necesita `await` (ni la palabra clave `async`). Probablemente los usa para evitar el mensaje de IntelliSense en Visual Studio.

El método de C++/WinRT equivalente también será asincrónico (porque llama a [**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)). Pero no es necesario `co_await`, ni devolver un objeto asincrónico. Para información sobre `co_await` y los objetos asincrónicos, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

Ahora hablemos sobre lo que hace el método. Dado que se trata de un controlador de eventos para el evento **Click** de un **HyperlinkButton**, el objeto denominado *sender* es realmente un **HyperlinkButton**. Por lo tanto, la conversión de tipos es segura (también podríamos haber expresado esta conversión como `sender as HyperlinkButton`). Luego, recuperamos el valor de la propiedad **Tag** (si observas el marcado XAML en el proyecto de C#, verás que se establece en una cadena que representa una dirección URL web). Si bien la propiedad **FrameworkElement.Tag** (**HyperlinkButton** es un **FrameworkElement**) es de tipo **object**, en C# podemos convertirlo a cadena con [**Object.ToString**](/dotnet/api/system.object.tostring). En la cadena resultante, construimos un objeto **Uri**. Y, por último, con la ayuda del shell, iniciamos un explorador y navegamos a la dirección URL.

Este es el método que se portó a C++/WinRT (de nuevo, expandido para mayor claridad), después del cual hay una descripción de los detalles.

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

Como siempre, hacemos que el controlador de eventos sea `public`. Usamos la función [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) en el objeto *sender* para convertirlo en **HyperlinkButton**. En C++/WinRT, la propiedad **Tag** es una interfaz [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (el equivalente de [**Object**](/dotnet/api/system.object)). Pero no hay ningún **ToString** en **IInspectable**. En su lugar, es necesario realizar una conversión unboxing de **IInspectable** a un valor escalar (en este caso, una cadena). De nuevo, para más información sobre las conversiones boxing y unboxing, consulta [Conversión boxing y unboxing de valores escalares a IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing).

Las dos últimas líneas repiten los patrones de portabilidad que hemos visto antes y, en gran medida, repiten lo visto en la versión de C#.

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

La portabilidad de este método no conlleva nada nuevo. Puede comparar las versiones de C# y C++/WinRT en el archivo ZIP del código fuente del [ejemplo de Clipboard](/samples/microsoft/windows-universal-samples/clipboard/) que ha descargado.

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** y **OnWindowActivated**

Hasta ahora solo tenemos códigos auxiliares vacíos para estos dos controladores de eventos. Pero portarlos es sencillo y no genera nada nuevo que tratar.

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

Se trata de otro controlador de eventos privado que pertenece a la clase **MainPage** de C# y que se define en `MainPage.xaml.cs`. En C++/WinRT, lo haremos público y lo implementaremos en `MainPage.h` y `MainPage.cpp`.

Para este método, necesitamos **MainPage::Navigating**, que es un campo booleano privado, inicializado en `false`. Y necesitarás un **Frame** en `MainPage.xaml`, denominado *ScenarioFrame*. Pero, aparte de estos detalles, portar este método no revela nuevas técnicas.

#### <a name="updatestatus"></a>**UpdateStatus**

Hasta el momento, solo tenemos un código auxiliar de **MainPage.UpdateStatus**. Nuevamente, portar su implementación no aporta nada nuevo. Un nuevo punto a tener en cuenta es que, mientras que en C# podemos comparar **string** con **String.Empty**, en C++/WinRT, en su lugar, llamamos a la función [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function). Otro es que `nullptr` es el equivalente estándar de C++ de `null` de C#.

Puedes realizar el resto de la portabilidad con las técnicas que ya hemos abordado. Esta es una lista de los tipos de cosas que tienes que hacer antes de que se compile la versión portada de este método.

- Para `MainPage.xaml`, agrega un **Border** denominado *StatusBorder*.
- Para `MainPage.xaml`, agrega un **TextBlock** denominado *StatusBlock*.
- Para `MainPage.xaml`, agrega un **StackPanel** denominado *StatusPanel*.
- Para `pch.h`, agrega `#include "winrt/Windows.UI.Xaml.Media.h"`.
- Para `pch.h`, agrega `#include "winrt/Windows.UI.Xaml.Automation.Peers.h"`.
- Para `MainPage.cpp`, agrega `using namespace winrt::Windows::UI::Xaml::Media;`.
- Para `MainPage.cpp`, agrega `using namespace winrt::Windows::UI::Xaml::Automation::Peers;`.

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>Copia de XAML y estilos necesarios para terminar de portar **MainPage**

Para XAML, el caso ideal es que puedas usar *el mismo* marcado XAML en un proyecto de C# y de C++/WinRT. Y el ejemplo Clipboard es uno de estos casos.

En el archivo `Styles.xaml`, el ejemplo Clipboard tiene un **ResourceDictionary** de estilos de XAML, que se aplican a los botones, menús y otros elementos de UI a través de la interfaz de usuario de la aplicación. La página `Styles.xaml` se combina en `App.xaml`. Y, luego, hay el punto de partida de `MainPage.xaml` estándar para la interfaz de usuario, que ya hemos visto brevemente. Ahora podemos reutilizar esos tres archivos `.xaml`, sin cambios, en la versión de C++/WinRT del proyecto.

Al igual que con los archivos de recursos, puedes optar por hacer referencia a los mismos archivos XAML compartidos desde varias versiones de la aplicación. En este tutorial, solo por motivos de simplicidad, copiaremos los archivos en el proyecto de C++/WinRT y los agregaremos de este modo.

Ve a la carpeta `\Clipboard_sample\SharedContent\xaml`, selecciona y copia `App.xaml` y `MainPage.xaml` y, luego, pega esos dos archivos en la carpeta `\Clipboard\Clipboard` del proyecto de C++/WinRT, y elige si quieres reemplazar los archivos cuando se te pregunte.

Agrega una nueva carpeta al proyecto de C++/WinRT, inmediatamente debajo del nodo del proyecto y con el nombre `Styles`. Ve a la carpeta `\Clipboard_sample\SharedContent\xaml`, selecciona y copia `Styles.xaml` y pégalo en la carpeta `\Clipboard\Clipboard\Styles` del proyecto de C++/WinRT. Haz clic con el botón derecho en la carpeta `Styles` (en el Explorador de soluciones en el proyecto de C++/WinRT) > **Agregar** > **Elemento existente…** y ve hasta `\Clipboard\Clipboard\Styles`. En el selector de archivos, selecciona `Styles` y haz clic en **Agregar**.

Hemos terminado de portar **MainPage** y, si has estado siguiendo los pasos, el proyecto de C++/WinRT ahora se compilará y ejecutará.

## <a name="consolidate-your-idl-files"></a>Consolidación de los archivos `.idl`

Además del punto de partida de `MainPage.xaml` estándar para la interfaz de usuario, el ejemplo Clipboard tiene otras cinco páginas XAML específicas del escenario, junto con sus archivos de código subyacente correspondientes. Volveremos a usar el marcado XAML real de todas estas páginas, sin cambios, en la versión de C++/WinRT del proyecto. Y veremos cómo portar el código subyacente en las próximas secciones principales. Pero antes de eso, hablemos sobre IDL.

Consolidar las clases en tiempo de ejecución en un único archivo IDL (consulta [Factorizar clases en tiempo de ejecución en archivos Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)) tiene su valor. Por lo tanto, a continuación consolidaremos el contenido de `CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` y `OtherScenarios.idl` moviendo ese IDL a un único archivo denominado `Project.idl` (y, luego, eliminando los archivos originales).

Mientras hacemos eso, también vamos a quitar la propiedad ficticia generada automáticamente (`Int32 MyProperty;` y su implementación) de cada uno de esos cinco tipos de página XAML.

En primer lugar, agrega un nuevo elemento de **archivo Midl (.idl)** al proyecto de C++/WinRT. Asígnale el nombre `Project.idl`. Reemplaza todo el contenido de `Project.idl` por el código siguiente.

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

Como puedes ver, se trata simplemente de una copia del contenido de los archivos `.idl` individuales, todos dentro de un espacio de nombres, y donde se ha quitado `MyProperty` de cada clase en tiempo de ejecución.

En el Explorador de soluciones en Visual Studio, selecciona todos los archivos IDL originales (`CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` y `OtherScenarios.idl`) y selecciona **Editar** > **Quitar** esos archivos (elige **Eliminar** en el cuadro de diálogo).

Por último &mdash;y para completar la eliminación de `MyProperty`&mdash; en los archivos `.h` y `.cpp` de cada uno de los cinco tipos de página XAML, elimina las declaraciones y definiciones del descriptor de acceso `int32_t MyProperty()` y las funciones del mutador `void MyProperty(int32_t)`.

A propósito, siempre es aconsejable que el nombre de los archivos XAML coincida con el nombre de la clase que representan. Por ejemplo, si tienes `x:Class="MyNamespace.MyPage"` en un archivo de marcado XAML, ese archivo se debería denominar `MyPage.xaml`. Aunque no es un requisito técnico, no tener que trabajar con distintos nombres para el mismo artefacto hará que el proyecto sea más comprensible y fácil de mantener, y que resulte más fácil trabajar con él.

## <a name="copyfiles"></a>**CopyFiles**

En el proyecto de C#, el tipo de página XAML **CopyFiles** se implementa en los archivos de código fuente `CopyFiles.xaml` y `CopyFiles.xaml.cs`. Echemos un vistazo a cada uno de los miembros de **CopyFiles** uno a la vez.

### <a name="rootpage"></a>**rootPage**

Este es un campo privado.

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

En C++/WinRT, podemos definirlo e inicializarlo de este modo.

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

De nuevo (al igual que con **MainPage::Current**), **CopyFiles::rootPage** se declara como de tipo **SDKTemplate::MainPage**, que es el tipo proyectado, y no el tipo de implementación.

### <a name="copyfiles-the-constructor"></a>**CopyFiles** (el constructor)

En el proyecto de C++/WinRT, el tipo **CopyFiles** ya tiene un constructor que contiene el código que queremos (simplemente llama a **InitializeComponent**).

### <a name="copybutton_click"></a>**CopyButton_Click**

El método **CopyButton_Click** de C# es un controlador de eventos y, a partir de la palabra clave `async` en su firma, podemos decir que el método realiza un trabajo asincrónico. En C++/WinRT, implementamos un método asincrónico como *corrutina*. Para ver una introducción a la simultaneidad en C++/WinRT, junto con una descripción de lo que es una *corrutina*, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

Es habitual que quieras programar más trabajo después de que se complete una corrutina y, para estos casos, la corrutina devolvería algún tipo de objeto asincrónico que se pueda esperar y que, opcionalmente, informe del progreso. Sin embargo, estas consideraciones normalmente no se aplican a un controlador de eventos. Por lo tanto, cuando tengas un controlador de eventos que realiza operaciones asincrónicas, puedes implementarlo como corrutina que devuelva **winrt::fire_and_forget**. Para más información, consulta [Desencadenamiento y olvido](/windows/uwp/cpp-and-winrt-apis/concurrency-2#fire-and-forget).

Aunque la idea de una corrutina de desencadenamiento y olvido es que no te interesa cuándo se completa, el trabajo aún continúa (o se suspende, esperando la reanudación) en segundo plano. Puedes ver en la implementación de C# que **CopyButton_Click** depende del puntero `this` (accede al miembro de datos de instancia `rootPage`). Por tanto, tenemos que asegurarnos de que el puntero `this` (un puntero a un objeto **CopyFiles**) sobreviva a la corrutina **CopyButton_Click**. En una situación como esta aplicación de ejemplo, donde el usuario navega entre páginas de UI, no podemos controlar directamente la duración de esas páginas. En caso de que se destruya la página **CopyFiles** (al salir de ella) mientras **CopyButton_Click** todavía está en curso en un subproceso en segundo plano, no será seguro acceder a `rootPage`. Para que la corrutina sea correcta, tiene que obtener una referencia fuerte al puntero `this` y mantener esa referencia durante la vigencia de la corrutina. Para más información, consulta [Referencias fuertes y débiles de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references).

Si observas la versión de C++/WinRT del ejemplo, en **CopyFiles::CopyButton_Click**, verás que se ha hecho con una simple declaración en la pila.

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

Echemos un vistazo a los demás aspectos del código portado que vale la pena mencionar.

En el código, creamos una instancia de un objeto [**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker) y dos líneas más adelante accedemos a la propiedad [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) de ese objeto. El tipo de valor devuelto de esa propiedad implementa un **IVector** de cadenas. Y en ese **IVector**, llamamos al método [IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall). El aspecto interesante es el valor que pasamos a ese método, donde se espera una matriz. He aquí la línea de código.

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

El valor que pasamos (`{ L"*" }`) es una *lista de inicializadores* de C++ estándar. Contiene un único objeto, en este caso, pero una lista de inicializadores puede contener cualquier cantidad de objetos separados por comas. Los elementos de C++/WinRT que te permiten pasar una lista de inicializadores a un método como este se explican en [Listas de inicializadores estándares](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists).

Portamos la palabra clave `await` de C# a `co_await` en C++/WinRT. Este es el ejemplo del código.

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

Luego, ten en cuenta esta línea de código de C#.

```csharp
dataPackage.SetStorageItems(storageItems);
```

C# es capaz de convertir implícitamente la **IReadOnlyList<StorageFile>** representada por *storageItems* en la **IEnumerable<IStorageItem>** que espera [**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems). Pero en C++/WinRT, es necesario convertir explícitamente de **IVectorView<StorageFile>** a **IIterable<IStorageItem>** . Y, por lo tanto, tenemos otro ejemplo de la función [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) en acción.

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

Cuando usamos la palabra clave `null` en C# (por ejemplo, `Clipboard.SetContentWithOptions(dataPackage, null)`), usamos `nullptr` en C++/WinRT (por ejemplo, `Clipboard::SetContentWithOptions(dataPackage, nullptr)`).

### <a name="pastebutton_click"></a>**PasteButton_Click**

Se trata de otro controlador de eventos en forma de corrutina de desencadenamiento y olvido. Echemos un vistazo a los aspectos del código portado que vale la pena mencionar.

En la versión de C# del ejemplo, detectamos excepciones con `catch (Exception ex)`. En el código de C++/WinRT portado, verás la expresión `catch (winrt::hresult_error const& ex)`. Para más información sobre [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) y cómo trabajar con ella, consulta [Control de errores con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/error-handling).

Un ejemplo de comprobación de si un objeto de C# es `null` o no es `if (storageItems != null)`. En C++/WinRT, podemos confiar en un operador de conversión a `bool`, que hace la prueba en `nullptr` internamente.

Esta es una versión ligeramente simplificada de un fragmento de código de la versión de C++/WinRT portada del ejemplo.

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

Construir **std::wstring_view** a partir de una **winrt::hstring** de este modo ilustra una alternativa a llamar a la función [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) (para convertir **winrt::hstring** en una cadena de estilo C). Esta alternativa funciona gracias al [operador de conversión de **hstring**en **std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view).

Ten en cuenta este fragmento de C#.

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

Para portar la palabra clave `as` de C# a C++/WinRT, hasta ahora hemos visto que la función [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) se ha usado un par de veces. Esa función devuelve una excepción si se produce un error en la conversión de tipos. Pero si queremos que la conversión devuelva `nullptr` si se produce un error (para que podamos controlar esa condición en el código), en su lugar, usamos la función [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function).

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>Copia del XAML necesario para terminar de portar **CopyFiles**

Ahora puedes seleccionar todo el contenido del archivo `CopyFiles.xaml` del proyecto de C# y pegarlo en el archivo de `CopyFiles.xaml` en el proyecto de C++/WinRT (reemplazando el contenido existente de ese archivo en el proyecto de C++/WinRT).

Por último, edita `CopyFiles.h` y `.cpp`, y elimina la función ficticia **ClickHandler**, ya que simplemente sobrescribimos el marcado XAML correspondiente.

Hemos terminado de portar **CopyFiles** y, si has estado siguiendo los pasos, el proyecto de C++/WinRT ahora se compilará y ejecutará, y el escenario de **CopyFiles** estará operativo.

## <a name="copyimage"></a>**CopyImage**

Para portar el tipo de página **CopyImage** de XAML, sigue el mismo proceso que para **CopyFiles**. Al portar **CopyImage**, verás que se usa la [*instrucción using*](/dotnet/csharp/language-reference/keywords/using-statement) de C#, lo que garantiza que los objetos que implementan la interfaz [**IDisposable**](/dotnet/api/system.idisposable) se eliminen correctamente.

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

La interfaz equivalente en C++/WinRT es [**IClosable**](/uwp/api/windows.foundation.iclosable), con su único método **Close**. Este es el equivalente de C++/WinRT del código de C# anterior.

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

Los objetos de C++/WinRT implementan **IClosable** principalmente para beneficio de los lenguajes que carecen de finalización determinista. C++/WinRT cuenta con finalización determinista, por lo que a menudo no es necesario llamar a **IClosable::Close** al escribir en C++/WinRT. Pero hay ocasiones en las que es conveniente llamarlo, y este es uno de esos momentos. Aquí, el identificador *imageStream* es un contenedor con recuento de referencias alrededor de un objeto de Windows Runtime subyacente (en este caso, un objeto que implementa [**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype)). Aunque podemos determinar que el finalizador de *imageStream* (su destructor) se ejecutará al final del ámbito de inclusión (los corchetes), no podemos tener la certeza de que el finalizador llamará a **Close**. Esto se debe a que pasamos *imageStream* a otras API, y es posible que estas sigan contribuyendo al recuento de referencias del objeto de Windows Runtime subyacente. Por lo tanto, este es un caso donde se recomienda llamar a **Close** de manera explícita. Para más información, consulta [¿Tengo que llamar a IClosable::Close en las clases en tiempo de ejecución que consumo?](/windows/uwp/cpp-and-winrt-apis/faq#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume)

A continuación, considera la expresión `(uint)(imageDecoder.OrientedPixelWidth * 0.5)` de C#, que encontrarás en el controlador de eventos **OnDeferredImageRequestedHandler**. Esa expresión multiplica `uint` por `double`, lo que da `double`. Después convierte el resultado en `uint`. En C++/WinRT, *podríamos* usar una conversión de estilo C de aspecto similar (`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`), pero es preferible dejar claro exactamente qué tipo de conversión queremos y, en este caso, lo haríamos con `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)`.

La versión de C# de **CopyImage.OnDeferredImageRequestedHandler** tiene una cláusula `finally`, pero no una cláusula `catch`. Avanzamos apenas algo más en la versión de C++/WinRT y hemos implementado una cláusula `catch` para poder informar si la representación diferida se completó correctamente o no.

Portar el resto de esta página XAML no produce nada nuevo que tengamos que tratar. No olvide eliminar la función ficticia **ClickHandler**. Y, al igual que con **CopyFiles**, el último paso de la portabilidad es seleccionar todo el contenido de `CopyImage.xaml` y pegarlo en el mismo archivo en el proyecto de C++/WinRT.

## <a name="copytext"></a>**CopyText**

Puedes portar `CopyText.xaml` y `CopyText.xaml.cs` mediante las técnicas que ya hemos tratado.

## <a name="historyandroaming"></a>**HistoryAndRoaming**

Hay algunos puntos interesantes que surgen al portar el tipo de página XAML **HistoryAndRoaming**.

En primer lugar, echa un vistazo al código fuente de C# y sigue el flujo de control de **OnNavigatedTo** a través del controlador de eventos **OnHistoryEnabledChanged** y, por último, a la función asincrónica **CheckHistoryAndRoaming** (a la que no se espera, así que básicamente es de desencadenamiento y olvido). Dado que **CheckHistoryAndRoaming** es asincrónico, debemos tener cuidado en C++/WinRT con la duración del puntero `this`. Puedes ver el resultado si observas la implementación en el archivo de código fuente `HistoryAndRoaming.cpp`. En primer lugar, al conectar delegados a los eventos **Clipboard::HistoryEnabledChanged** y **Clipboard::RoamingEnabledChanged**, solo tomamos una referencia débil al objeto de página **HistoryAndRoaming**. Para ello, creamos el delegado con una dependencia en el valor devuelto desde [**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function), en lugar de una dependencia en el puntero `this`. Esto significa que el propio delegado, que finalmente llama a código asincrónico, no mantiene activa la página **HistoryAndRoaming**, en caso de que salgamos de ella.

En segundo lugar, cuando finalmente llegamos a la nuestra corrutina de desencadenamiento y olvido **CheckHistoryAndRoaming**, lo primero que hacemos es tomar una referencia fuerte a `this` para garantizar que la página **HistoryAndRoaming** dure al menos hasta que la corrutina se complete finalmente. Para más información sobre los dos aspectos recién descritos, consulta [Referencias fuertes y débiles de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references).

Otro punto de interés se encuentra al portar **CheckHistoryAndRoaming**. Contiene código para actualizar la interfaz de usuario; por lo tanto, tenemos que estar seguros de que lo estamos haciendo en el subproceso de la UI principal. El subproceso que llama inicialmente a un controlador de eventos es el subproceso principal de la interfaz de usuario. Pero, normalmente, un método asincrónico puede ejecutarse o reanudarse en cualquier subproceso arbitrario. En C#, la solución consiste en llamar a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) y actualizar la UI desde dentro de la función lambda. En C++/WinRT, podemos usar la función [**WinRT::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) junto con la propiedad [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) del puntero `this` para suspender la corrutina y reanudarla inmediatamente en el subproceso de la UI principal.

La expresión pertinente es `co_await winrt::resume_foreground(Dispatcher());`. Como alternativa, aunque menos clara, podríamos expresar esto simplemente como `co_await Dispatcher();`. La versión más corta se logra gracias a un operador de conversión proporcionado por C++/WinRT.

Portar el resto de esta página XAML no produce nada nuevo que tengamos que tratar. No olvide eliminar la función ficticia **ClickHandler** y copiar el marcado XAML.

## <a name="otherscenarios"></a>**OtherScenarios**

Puedes portar `OtherScenarios.xaml` y `OtherScenarios.xaml.cs` mediante las técnicas que ya hemos tratado.

## <a name="conclusion"></a>Conclusión

Espero que en este tutorial hayas encontrado la información y las técnicas de portabilidad suficientes para que ahora puedas seguir y portar tus propias aplicaciones de C# a C++/WinRT. Como recordatorio, puedes seguir consultando las versiones anterior (C#) y posterior (C++/WinRT) del código fuente en el ejemplo de Clipboard, y compararlas en paralelo para ver la correspondencia.

## <a name="related-topics"></a>Temas relacionados
* [Migrar a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)

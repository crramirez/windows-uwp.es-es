---
description: En este tema se catalogan de forma exhaustiva los detalles técnicos implicados en la migración del código fuente de un proyecto de [C#](/visualstudio/get-started/csharp) a su equivalente de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Migrar a C++/WinRT desde C#
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migrar, C#
ms.localizationpriority: medium
ms.openlocfilehash: 804c22b782dada9c0bde3c379ebfe5a37f1dcff9
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81759938"
---
# <a name="move-to-cwinrt-from-c"></a>Migrar a C++/WinRT desde C#

En este tema se catalogan de forma exhaustiva los detalles técnicos implicados en la migración del código fuente de un proyecto de [C#](/visualstudio/get-started/csharp) a su equivalente de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

Para ver un caso práctico de migración de uno de los ejemplos de aplicaciones de la Plataforma universal de Windows (UWP), consulta el tema complementario [Migración del ejemplo de Clipboard a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp). Para ganar práctica y experiencia en la migración, puedes seguir ese tutorial y portar el ejemplo por tu cuenta a medida que avanzas.

## <a name="how-to-prepare-and-what-to-expect"></a>Cómo realizar la preparación y qué esperar

En el caso práctico del tema [Migración del ejemplo de Clipboard a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp), se muestran ejemplos de los tipos de decisiones de diseño de software que deberás tomar al portar un proyecto a C++/WinRT. Por lo tanto, te recomendamos que, para prepararte para la migración, obtengas conocimientos detallados sobre cómo funciona el código existente. De este modo, contarás con información general adecuada sobre la funcionalidad de la aplicación y la estructura del código y, luego, las decisiones que tomes siempre te ayudarán a avanzar en la dirección adecuada.

En lo que se refiere a los tipos de cambio en la migración que debes esperar, puedes agruparlos en cuatro categorías.

- [**Migración de la proyección del lenguaje**](#port-the-language-projection). Windows Runtime (WinRT) se *proyecta* en varios lenguajes de programación. Cada una de esas proyecciones de lenguaje está diseñada para que suene idiomática en el lenguaje de programación en cuestión. En C#, algunos tipos de Windows Runtime se proyectan como tipos .NET. Por ejemplo, traducirás [**System.Collections.Generic.IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1) de nuevo a [**Windows.Foundation.Collections.IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1). También en C#, algunas operaciones de Windows Runtime se proyectan como características de lenguaje adecuadas de C#. Un ejemplo sería el uso de la sintaxis de operador `+=` en C# para registrar un delegado de control de eventos. Por lo tanto, traducirás características del lenguaje; por ejemplo, para volver a la operación fundamental que se está realizando (el registro de eventos, en este caso).
- [**Migración de la sintaxis del lenguaje**](#port-language-syntax). Muchos de estos cambios son transformaciones mecánicas simples, en las que se reemplaza un símbolo por otro. Por ejemplo, se cambia un punto (`.`) por dos signos de dos puntos (`::`).
- [**Procedimiento de migración del lenguaje**](#port-language-procedure). Algunos de estos pueden ser cambios sencillos y repetitivos (como de `myObject.MyProperty` a `myObject.MyProperty()`). Otros requieren cambios más complejos (por ejemplo, portar un procedimiento que implique el uso de **System.Text.StringBuilder** a uno que implique el uso de **std::wostringstream**).
- [**Migración de tareas específicas de C++/WinRT**](#porting-tasks-that-are-specific-to-cwinrt). C# se encarga de completar determinados detalles de Windows Runtime de manera implícita y en segundo plano. Estos detalles se realizan explícitamente en C++/WinRT. Un ejemplo sería el uso de un archivo `.idl` para definir las clases en tiempo de ejecución.

El resto de este tema está estructurado según esa taxonomía.

## <a name="port-the-language-projection"></a>Migración de la proyección del lenguaje

||C#|C++/WinRT|Consulta también|
|-|-|-|-|
|Objeto sin tipo|`object` o [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[Migración del método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Espacios de nombres de proyección|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|Tamaño de una colección|`collection.Count`|`collection.Size()`|[Migración del método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Colección de solo lectura|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[Migración del método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Delegado del controlador de eventos como miembro de clase|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[Migración del método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Delegado del controlador de eventos de revocación|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[Migración del método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Contenedor asociativo|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|Acceso de miembro vectorial|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>Registro/revocación de un controlador de eventos

En C++/WinRT, tienes varias opciones sintácticas para registrar o revocar un delegado del controlador de eventos, tal como se describe en [Control de eventos mediante delegados en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). Consulta también el tema [Migración del método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications).

A veces, por ejemplo, cuando el destinatario de un evento (un objeto que controla un evento) esté a punto de destruirse, te recomendamos que revoques un controlador de eventos para que el origen del evento (el objeto que provoca el evento) no llame a un objeto destruido. Consulta [Revocación de un delegado registrado](/windows/uwp/cpp-and-winrt-apis/handle-events#revoke-a-registered-delegate). En estos casos, crea una variable de miembro **event_token** para los controladores de eventos. Para ver un ejemplo, consulta el tema [Migración del método **EnableClipboardContentChangedNotifications**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications).

También puedes registrar un controlador de eventos en el marcado XAML.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

En C#, el método **OpenButton_Click** puede ser privado, y XAML aún podrá conectarlo al evento [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) generado por *OpenButton*.

En C++/WinRT, el método **OpenButton_Click** debe ser público en el [tipo de implementación](/windows/uwp/cpp-and-winrt-apis/author-apis) *si quieres registrarlo en el marcado XAML*. Si solo registras un controlador de eventos en código imperativo, no es necesario que el controlador de eventos sea público.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Como alternativa, puedes hacer que la página XAML de registro sea compatible con el tipo de implementación, y convertir el método **OpenButton_Click** en privado.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="port-language-syntax"></a>Sintaxis del lenguaje de migración

||C#|C++/WinRT|Consulta también|
|-|-|-|-|
|Modificadores de acceso|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[Migración del método **Button_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#button_click)|
|Acción asincrónica|`async Task ...`|`IAsyncAction ...`||
|Operación asincrónica|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|Método Fire-and-forget (implica una asincronía)|`async void ...`|`winrt::fire_and_forget ...`|[Migración del método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Espera cooperativa|`await ...`|`co_await ...`|[Migración del método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Acceso a una constante enumerada|`E.Value`|`E::Value`|[Migración del método **DisplayChangedFormats**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats)|
|Separador de espacios de nombres|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[Migración del método **UpdateStatus**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|Declaración de parámetros para un método|`MyType`|`MyType const&`|[Parameter-passing](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|Declaración de parámetros para un método asincrónico|`MyType`|`MyType`|[Parameter-passing](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|Llamada a un método estático|`T.Method()`|`T::Method()`||
|Cadenas|`string` o **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[Control de cadenas en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings)|
|Literal de cadena|`"a string literal"`|`L"a string literal"`|[Migración del constructor, **Current** y **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Literal de cadena sin formato/textual|`@"verbatim string literal"`|`LR"(raw string literal)"`|[Migración del método **DisplayToast**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast)|
|Acceso a un miembro de datos|`this.variable`|`this->variable`||
|Using-directive|`using A.B.C;`|`using namespace A::B::C;`|[Migración del constructor, **Current** y **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Tipo inferido (o deducido)|`var`|`auto`|[Migración del método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|

> [!NOTE]
> Si un archivo de encabezado no contiene una directiva de `using namespace` para un espacio de nombres determinado, deberás calificar completamente todos los nombres de tipo de ese espacio de nombre, o por lo menos lo suficiente como para que el compilador los pueda encontrar. Para obtener un ejemplo, consulta [Migración del método **DisplayToast**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast).

### <a name="porting-classes-and-members"></a>Migración de clases y miembros

Para cada tipo de C#, deberás decidir si quieres portarlo a un tipo de Windows Runtime o a una clase, estructura o enumeración de C++ normal. Para obtener más información y ejemplos detallados en los que se muestra cómo tomar esas decisiones, consulta el tema [Migración del ejemplo de Clipboard a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp).

Una propiedad de C#, normalmente, se convierte en una función de descriptor de acceso, una función mutadora y un miembro de datos de respaldo. Para obtener más información y un ejemplo, consulta el tema [Migración de la propiedad **IsClipboardContentChangedEnabled**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#isclipboardcontentchangedenabled).

En el caso de los campos no estáticos, haz que sean miembros de datos del [tipo de implementación](/windows/uwp/cpp-and-winrt-apis/author-apis).

Un campo estático de C# se convierte en una función mutadora o un descriptor de acceso estático de C++/WinRT. Para obtener más información y un ejemplo, consulta el tema [Migración del constructor, **Current** y **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name).

En el caso de las funciones de miembro, de nuevo deberás decidir si cada una de ellas forma parte o no de IDL, o si se trata de una función de miembro pública o privada del tipo de implementación. Para obtener más información y ejemplos sobre cómo decidirlo, consulta el tema [IDL para el tipo **MainPage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type).

### <a name="porting-xaml-markup-and-asset-files"></a>Migración del marcado XAML y los archivos de recursos

En el tema [Migración del ejemplo de Clipboard a C++/WinRT desde C#](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp), pudimos usar *el mismo* marcado XAML (incluidos los recursos) y los archivos de recursos en el proyecto de C# y de C++/WinRT. En algunos casos, se requerirán ediciones en el marcado para lograrlo. Consulta el tema [Copia de XAML y estilos necesarios para terminar de portar **MainPage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage).

## <a name="port-language-procedure"></a>Procedimiento de migración del lenguaje

||C#|C++/WinRT|Consulta también|
|-|-|-|-|
|Administración de la duración en un método asincrónico|N/A|`auto lifetime{ get_strong() };` o<br>`auto lifetime = get_strong();`|[Migración del método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Eliminación|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[Migración del método **CopyImage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copyimage)|
|Construcción del objeto|`new MyType(args)`|`MyType{ args }` o<br>`MyType(args)`|[Migración de la propiedad **Scenarios**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios)|
|Creación de una referencia no inicializada|`MyType myObject;`|`MyType myObject{ nullptr };` o<br>`MyType myObject = nullptr;`|[Migración del constructor, **Current** y **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Construcción de un objeto en una variable con argumentos|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` o <br>`auto myObject{ MyType(args) };` o <br>`auto myObject = MyType{ args };` o <br>`auto myObject = MyType(args);` o <br>`MyType myObject{ args };` o <br>`MyType myObject(args);`|[Migración del método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|Construcción de un objeto en una variable sin argumentos|`var myObject = new T();`|`MyType myObject;`|[Migración del método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Abreviatura de la inicialización del objeto|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|Operación del vector masiva|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[Migración del método **CopyButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Iteración en la colección|`foreach (var v in c)`|`for (auto&& v : c)`|[Migración del método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Detección de una excepción|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[Migración del método **PasteButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Detalles de la excepción|`ex.Message`|`ex.message()`|[Migración del método **PasteButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Obtención de un valor de propiedad|`myObject.MyProperty`|`myObject.MyProperty()`|[Migración del método **NotifyUser**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser)|
|Obtención de un valor de propiedad|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|Incremento de un valor de propiedad|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>Para las cadenas, cambie a un generador.||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|Cadena de idioma para la cadena Windows Runtime|N/A|`winrt::hstring{ s }`||
|Creación de cadenas|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[String-building](#string-building)|
|Interpolación de cadenas|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) o [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[Migración del método **OnNavigatedTo**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto)|
|Cadena vacía para la comparación|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[Migración del método **UpdateStatus**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|Operaciones de diccionario|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|Conversión de tipos (iniciar en caso de error)|`(MyType)v`|`v.as<MyType>()`|[Migración del método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|Conversión de tipos (null en caso de error)|`v as MyType`|`v.try_as<MyType>()`|[Migración del método **PasteButton_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Los elementos XAML con x:Name son propiedades.|`MyNamedElement`|`MyNamedElement()`|[Migración del constructor, **Current** y **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Cambio al subproceso de interfaz de usuario|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** o [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[Migración del método **NotifyUser**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser) y [Migración del método **HistoryAndRoaming**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#historyandroaming)|

En las secciones siguientes se explican con más detalle los elementos de la tabla.

### <a name="tostring"></a>ToString()

Los tipos de C# proporcionan el método [Object.ToString](/dotnet/api/system.object.tostring).

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/ WinRT no ofrece directamente esta función, pero puedes elegir alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT también admite [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) para un número limitado de tipos. Tendrás que agregar sobrecargas para los tipos adicionales a los que quieras aplicar stringify.

| Language | Stringify int | Stringify enum |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

En caso de aplicar stringify en una enumeración, debes proporcionar la implementación de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

El enlace de datos suele consumir implícitamente estas aplicaciones de stringify.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Estos enlaces ejecutarán **winrt::to_hstring** de la propiedad enlazada. En el caso del segundo ejemplo (**StatusEnum**), debes proporcionar tu propia sobrecarga de **winrt::to_hstring**; de lo contrario, recibirás un error del compilador.

Consulta también el tema [Migración del método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click).

### <a name="string-building"></a>Creación de cadenas

En el caso de la creación de cadenas, C# tiene un tipo [**StringBuilder**](/dotnet/api/system.text.stringbuilder) integrado.

| | C# | C++/WinRT |
|-|-|-|
| Creación de cadenas | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| Anexo de una cadena de Windows Runtime conservando los valores NULL | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| Adición de una nueva línea |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| Acceso al resultado | `s = builder.ToString();` | `ws = builder.str();` |

Consulta también los temas [Migración del método **BuildClipboardFormatsOutputString**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring) y [Migración del método **DisplayChangedFormats**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats).

## <a name="porting-tasks-that-are-specific-to-cwinrt"></a>Migración de tareas específicas de C++/WinRT

### <a name="define-your-runtime-classes-in-idl"></a>Definición de las clases en tiempo de ejecución en IDL

Consulta los temas [IDL para el tipo de **MainPage**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type) y [Consolidación de los archivos `.idl`](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#consolidate-your-idl-files).

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>Inclusión de los archivos de encabezado de espacio de nombres de Windows de C++/WinRT necesarios

En C++/WinRT, siempre que quieras usar un tipo desde un espacio de nombres de Windows, debes incluir el archivo de encabezado de espacio de nombres de Windows de C++/WinRT correspondiente. Para obtener un ejemplo, consulta el tema [Migración del método **NotifyUser**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser).

### <a name="boxing-and-unboxing"></a>Conversiones boxing y unboxing

C# aplica automáticamente boxing a los valores escalares para convertirlos en objetos. C++/WinRT requiere que llames a la función [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) de manera explícita. Ambos lenguajes requieren que apliques la conversión unboxing de manera explícita. Consulta [Conversión boxing y unboxing con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

En las tablas siguientes, usaremos estas definiciones.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Operación | C# | C++/WinRT|
|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX y C# producen excepciones si intentas aplicar unboxing para convertir un puntero nulo en a un tipo de valor. C++/WinRT considera que se trata de un error de programación y se bloquea. En C++/WinRT, usa la función [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) si quieres controlar el caso en el que el objeto no sea del tipo que pensabas que era.

| Escenario | C# | C++/WinRT|
|-|-|-|
| Conversión unboxing de un entero conocido |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Si o es null | `System.NullReferenceException` | Bloqueo |
| Si o no es un entero con conversión boxing | `System.InvalidCastException` | Bloqueo |
| Unboxing de entero, usar reserva si es null; bloquear en cualquier otro caso | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Unboxing de entero, si es posible; usar reversión en cualquier otro caso | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

Para obtener un ejemplo, consulta los temas [Migración del método **OnNavigatedTo**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto) y [Migración del método **Footer_Click**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click).

#### <a name="boxing-and-unboxing-a-string"></a>Conversiones boxing y unboxing en una cadena

En cierto modo, una cadena es un tipo de valor y, de otro modo, un tipo de referencia. C# y C++/WinRT tratan las cadenas de manera diferente.

El tipo de ABI [**HSTRING**](/windows/win32/winrt/hstring) es un puntero a una cadena de recuento de referencias. Pero no se obtiene de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), por lo que, técnicamente, no es un *objeto*. Además, un **HSTRING** nulo representa la cadena vacía. La conversión boxing aplicada a cosas que no se derivan de **IInspectable** se realiza encapsulándolas dentro de un [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_), y Windows Runtime proporciona una implementación estándar en forma de objeto [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (los tipos personalizados se notifican como [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

C# representa una cadena de Windows Runtime como tipo de referencia; mientras que C++/WinRT proyecta una cadena como tipo de valor. Es decir, una cadena nula a la que se ha aplicado la conversión boxing puede tener distintas representaciones en función de cómo llegue allí.

| Comportamiento | C# | C++/WinRT|
|-|-|-|
| Declaraciones | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| Categoría de tipo de cadena | Tipo de referencia | Tipo de valor |
| **HSTRING** null se proyecta como | `""` | `hstring{}` |
| ¿Son null y `""` idénticos? | No | Sí |
| Validez de null | `s = null;`<br>`s.Length` genera NullReferenceException | `s = hstring{};`<br>`s.size() == 0` (válido) |
| Si asignas una cadena null a un objeto | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Si asignas `""` a un objeto | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Conversiones boxing y unboxing básicas.

| Operación | C# | C++/WinRT|
|-|-|-|
| Aplicar boxing a una cadena | `o = s;`<br>Una cadena vacía se convierte en un objeto con un valor distinto de null. | `o = box_value(s);`<br>Una cadena vacía se convierte en un objeto con un valor distinto de null. |
| Unboxing a una cadena conocida | `s = (string)o;`<br>El objeto null se convierte en una cadena null.<br>InvalidCastException si no es una cadena. | `s = unbox_value<hstring>(o);`<br>Un objeto null se bloquea.<br>Se bloquea si no es una cadena. |
| Conversión unboxing de una posible cadena | `s = o as string;`<br>Un objeto null o que no es una cadena se convierte en una cadena null.<br><br>O<br><br>`s = o as string ?? fallback;`<br>Un valor null o que no es una cadena se convierte en un elemento Fallback.<br>Una cadena vacía se conserva. | `s = unbox_value_or<hstring>(o, fallback);`<br>Un valor null o que no es una cadena se convierte en un elemento Fallback.<br>Una cadena vacía se conserva. |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>Poner una clase a disposición de la extensión de marcado {Binding}

Si piensas usar la extensión de marcado {Binding} para enlazar datos al tipo de datos, consulta [Objeto de enlace que se declara usando {Binding}](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

### <a name="consuming-objects-from-xaml-markup"></a>Consumir objetos a partir del marcado XAML

En un proyecto de C#, puedes consumir miembros privados y elementos con nombre del marcado XAML. Pero en C++/WinRT, todas las entidades consumidas por la [**extensión de marcado {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) de XAML deben exponerse públicamente en IDL.

Además, el enlace a un valor booleano muestra `true` o `false`en C#, pero muestra **Windows.Foundation.IReference`1\<valor booleano\>** en C++/WinRT.

Para más información y ejemplos de código, consulta [Consumir objetos a partir del marcado](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

### <a name="making-a-data-source-available-to-xaml-markup"></a>Poner un origen de datos a disposición del marcado XAML

En C++/WinRT versión 2.0.190530.8 y posterior, [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) crea un vector observable que admite tanto **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** como **IObservableVector\<IInspectable\>** . Para obtener un ejemplo, consulta el tema [Migración de la propiedad **Scenarios**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios).

Puedes crear el **archivo Midl (.idl)** de este modo (consulta también [Factorizar clases en tiempo de ejecución en archivos (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

E implementarlo de este modo.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Para obtener más información, consulta los artículos [Controles de elementos de XAML; enlazar a una colección C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection) y [Colecciones con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/collections).

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Poner un origen de datos a disposición del marcado XAML (antes de C++/WinRT 2.0.190530.8)

El enlace de datos XAML requiere que un origen de los elementos implemente **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** , así como una de las siguientes combinaciones de interfaces.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** e **INotifyCollectionChanged**
- **IBindableVector** e **IBindableObservableVector**
- **IBindableVector** por sí mismo (no responderá a los cambios)
- **IVector\<IInspectable\>**
- **IBindableIterable** (se iterará y guardará los elementos en una colección privada)

No se puede detectar una interfaz genérica como **IVector\<T\>** en tiempo de ejecución. Cada **IVector\<T\>** tiene un identificador de interfaz diferente (IID), que es una función de **T**. Cualquier desarrollador puede expandir el conjunto de **T** de manera arbitraria, por lo que queda claro que el código de enlace XAML nunca puede conocer el conjunto completo al que va a consultar. Esa restricción no supone un problema para C#, ya que cada objeto CLR que implementa **IEnumerable\<T\>** implementa **IEnumerable** de manera automática. En el nivel de ABI, eso significa que cada objeto que implementa **IObservableVector\<T\>** implementa **IObservableVector\<IInspectable\>** de manera automática.

C++/WinRT no ofrece esa garantía. Si una clase de C++/WinRT en tiempo de ejecución implementa **IObservableVector\<T\>** , no podemos suponer que de, algún modo, también se proporciona una implementación de **IObservableVector\<IInspectable\>** .

Por lo tanto, así es como deberá verse el ejemplo anterior.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Y la implementación.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Si tienes que acceder a objetos en *m_bookSkus*, tendrás que volver a aplicarles QueryInterface en **Bookstore::BookSku**.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>Clases derivadas

Para derivar a partir de una clase en tiempo de ejecución, la clase base debe *admitir la composición*. C# no requiere que sigas ningún paso en particular para hacer que estas clases admitan composición, pero C++/WinRT sí. Usa la [palabra clave unsealed](/uwp/midl-3/intro#base-classes) para indicar que quieres que la clase se pueda usar como clase base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

En el archivo de encabezado de tu [tipo de implementación](/windows/uwp/cpp-and-winrt-apis/author-apis), tienes que incluir el archivo de encabezado de la clase base antes de incluir el encabezado generado automáticamente para la clase derivada. De lo contrario, obtendrás errores como "uso no válido de este tipo como expresión".

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>API importantes
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [Tutoriales de C#](/visualstudio/get-started/csharp)
* [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)
* [Enlace de datos en profundidad](/windows/uwp/data-binding/data-binding-in-depth)
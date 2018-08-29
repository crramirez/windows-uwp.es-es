---
author: msatranjr
title: Crear componentes de Windows Runtime en C++
description: En este tema se muestra cómo usar C++ / CX para crear un componente de Windows Runtime, que es un componente que se puede llamar desde una aplicación de Windows Universal creada con C#, Visual Basic, C++ o Javascript.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.author: misatran
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b5515d0ed5dc6e200c7c4fc9a7785c993d4cab59
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2911919"
---
# <a name="creating-windows-runtime-components-in-ccx"></a>Crear componentes de Windows Runtime en C++/CX
> [!NOTE]
> Este tema existe para ayudar a mantener tu aplicación de C++/CX. Pero te recomendamos que uses [C++ / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para nuevas aplicaciones. C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows. Para obtener información sobre cómo crear un componente de Windows Runtime con C++ / WinRT, consulta [crear eventos en C++ / WinRT](../cpp-and-winrt-apis/author-events.md).

En este tema se muestra cómo usar C++ / CX para crear un componente de Windows Runtime, que es un componente que se puede llamar desde una aplicación de Windows Universal creada con C#, Visual Basic, C++ o Javascript.

Hay varias razones para compilar un componente de Windows Runtime.
- Para obtener la ventaja de rendimiento de C++ en operaciones complejas o de computación intensivas.
- Reutilizar el código que ya se ha escrito y probado.

Al compilar una solución que contiene un proyecto de JavaScript o. NET y un proyecto de componente de Windows Runtime, los archivos de proyecto de JavaScript y la DLL compilada se combinan en un paquete que se puede depurar localmente en el simulador o remotamente en un dispositivo amarrado. También puedes distribuir solo el proyecto del componente como un SDK de extensión. Para obtener más información, consulta [Crear un kit de desarrollo de software](https://msdn.microsoft.com/library/hh768146.aspx).

En general, cuando escribas el código tu C++ / componente CX, usa la biblioteca de C++ regular y los tipos integrados, excepto en los límites de la interfaz binaria abstracta (ABI) donde pasas datos hacia y desde el código de otro paquete .winmd. Allí, usa los tipos de Windows Runtime y la sintaxis especial que C++ / CX admite para crear y manipular esos tipos. Además, en los tipos C++ / CX en el código, usa tipos como delegate y event para implementar los eventos que se pueden genera desde tu componente y controlar en JavaScript, Visual Basic, C++ o C#. Para obtener más información sobre C++ / sintaxis CX, consulta [referencia de lenguaje de Visual C++ (C++ / CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## <a name="casing-and-naming-rules"></a>Reglas de nomenclatura y del uso de mayúsculas y minúsculas

### <a name="javascript"></a>JavaScript
JavaScript distingue mayúsculas de minúsculas. Por lo tanto, debes seguir estas convenciones sobre el uso de mayúsculas y minúsculas:

-   Cuando hagas referencia a las clases y espacios de nombres de C++, usa las mayúsculas y minúsculas del mismo modo que en el lado de C++.
-   Al llamar a métodos, usa la convención Camel de mayúsculas y minúsculas incluso si el nombre del método está en mayúsculas en el lado de C++. Por ejemplo, "GetDate()" del método de C++ debe llamarse desde JavaScript como "getDate()".
-   Un nombre de clase activable y un nombre de espacio de nombres no pueden contener caracteres UNICODE.

### <a name="net"></a>.NET
Los lenguajes .NET siguen las reglas normales del uso de mayúsculas y minúsculas.

## <a name="instantiating-the-object"></a>Crear una instancia del objeto
Solo los tipos de Windows Runtime pueden pasarse a través del límite de la ABI. El compilador generará un error si el componente tiene un tipo como std::wstring como un parámetro o tipo devuelto en un método público. Las extensiones del componente Visual C++ (C++ / CX) tipos integrados incluyen los escalares habituales como int y double así como sus equivalentes typedef int32, float64 y así sucesivamente. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C++ / CX tipos integrados, tipos de la biblioteca y tipos de Windows Runtime
Una clase activable (también conocida como "clase de referencia") es una clase en torno a la cual se pueden crear instancias desde otro lenguaje, como JavaScript, C# o Visual Basic. Para ser consumible en otro idioma, un componente debe contener al menos una clase activable.

Un componente de Windows Runtime puede contener varias clases activables públicas, así como clases adicionales conocidas solo internamente por el componente. Aplicar el atributo [WebHostHidden](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.webhosthiddenattribute.aspx) a C++ / tipos CX que no están diseñados para ser visibles para JavaScript.

Todas las clases públicas deben residir en el mismo espacio de nombres de raíz con el mismo nombre que el archivo de metadatos del componente. Por ejemplo, para una clase que se denomina A.B.C.MyClass solo puede crearse una instancia si se define en un archivo de metadatos denominado A.winmd o A.B.winmd o A.B.C.winmd. El nombre de la DLL no es necesario que coincida con el nombre de archivo .winmd.

El código de cliente crea una instancia del componente con la palabra clave **new** (**New** en Visual Basic) del mismo modo que para cualquier otra clase.

Una clase activable debe declararse como **public ref class sealed**. La palabra clave **ref class** indica al compilador que cree la clase como tipo compatible de Windows Runtime, y la palabra clave sealed especifica que la clase no puede heredarse. Windows Runtime actualmente no es compatible con un modelo de herencia generalizada; un modelo de herencia limitado es compatible con la creación de controles de XAML personalizados. Para obtener más información, consulta [Clases y estructuras de referencia (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699870.aspx).

Para C++ / CX, todos los primitivos numéricos se definen en el espacio de nombres predeterminado. Contiene el espacio de nombres de [plataforma de](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx) C++ / sistema de tipos de clases CX que son específicas de Windows Runtime. Estos incluyen las clases [Platform::String](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) y [Platform::Object](https://msdn.microsoft.com/library/windows/apps/xaml/hh748265.aspx). Los tipos de colección concreta, como las clases [Platform::Collections::Map](https://msdn.microsoft.com/library/windows/apps/xaml/hh441508.aspx) y [Platform::Collections::Vector](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx), se definen en el espacio de nombres [Platform::Collections](https://msdn.microsoft.com/library/windows/apps/xaml/hh710418.aspx). Las interfaces públicas que implementan estos tipos se definen en [Windows::Foundation::Collections Namespace (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh441496.aspx). Estos son los tipos de interfaz consumidos por JavaScript, C# y Visual Basic. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

## <a name="method-that-returns-a-value-of-built-in-type"></a>Método que devuelve un valor de tipo integrado
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>Método que devuelve una estructura de valor personalizada
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

Para pasar las estructuras de valor definida por el usuario a través de la ABI, define un objeto de JavaScript que tiene los mismos miembros que la estructura de valor que se define en C++ / CX. Puedes pasar ese objeto como argumento a C++ / método CX para que el objeto se convierte implícitamente a C++ / tipo CX.

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

Otro enfoque es definir una clase que implemente IPropertySet (no se muestra).

En los lenguajes. NET, solo tienes que crear una variable del tipo que se define en tipos C++ / componentes CX.

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>Métodos sobrecargados
C++ / clase de referencia pública de CX puede contener métodos sobrecargados, pero JavaScript tiene una capacidad para diferenciar los métodos sobrecargados limitada. Por ejemplo, puede distinguir entre estas firmas:

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

Pero no puede distinguir entre estas:

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

En casos ambiguos, puedes asegurarte de que JavaScript llame siempre a una sobrecarga específica aplicando el atributo [Windows::Foundation::Metadata::DefaultOverload](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) a la firma del método en el archivo de encabezado.

Este código JavaScript siempre llama a la sobrecarga atribuida:

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
Los lenguajes .NET reconocen sobrecargas en C++ / clase de referencia CX al igual que en cualquier clase de .NET Framework.

## <a name="datetime"></a>DateTime
En Windows Runtime, un objeto [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/windows.foundation.datetime.aspx) es simplemente un entero firmado de 64 bits que representa el número de intervalos de 100 nanosegundos o bien antes o bien después del 1 de enero de 1601. No hay métodos en un objeto de Windows:Foundation::DateTime. En su lugar, cada lenguaje proyecta DateTime en la forma nativa para ese lenguaje: el objeto Date en JavaScript y los tipos System.DateTime y System.DateTimeOffset en .NET Framework.

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

Cuando se pasa un valor de DateTime de C++ / CX a JavaScript, JavaScript lo acepta como objeto Date y lo muestra de forma predeterminada como una cadena de fecha de formato largo.

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

Cuando un lenguaje .NET pasa System.DateTime a C++ / componente CX, el método lo acepta como un DateTime. Cuando el componente pasa Windows::Foundation::DateTime a un método de .NET Framework, el método Framework lo acepta como DateTimeOffset.

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>Colecciones y matrices
Las colecciones siempre se pasan a través del límite de la ABI como controladores para los tipos de Windows Runtime como Windows::Foundation::Collections::IVector^ y Windows::Foundation::Collections::IMap^. Por ejemplo, si devuelves un controlador a Platform::Collections::Map, este lo convierte implícitamente a Windows::Foundation::Collections::IMap^. Las interfaces de colección se definen en un espacio de nombres independiente de los tipos C++ / clases CX que proporcionan las implementaciones concretas. Los lenguajes de JavaScript y .NET consumen las interfaces. Para obtener más información, consulta [Colecciones (C++/CX)](https://msdn.microsoft.com//library/windows/apps/hh700103.aspx) y [Matriz y WriteOnlyArray (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh700131.aspx).

## <a name="passing-ivector"></a>Pasar el IVector
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

Los lenguajes .NET ven IVector&lt;T&gt; como IList&lt;T&gt;.

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>Pasar IMap
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

Los lenguajes .NET ven IMap e IDictionary&lt;K, V&gt;.

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>Propiedades
Una clase de referencia pública en C++ / extensiones de componentes de CX expone los miembros de datos públicos como propiedades, mediante el uso de la palabra clave de propiedad. El concepto es idéntico a las propiedades de .NET Framework. Una propiedad trivial es similar a un miembro de datos porque su funcionalidad es implícita. Una propiedad no trivial tiene descriptores de acceso get y set explícitos y una variable privada con nombre que es la "memoria auxiliar" para el valor. En este ejemplo, la variable de miembro privada \_propertyAValue es la copia de seguridad para PropertyA. Una propiedad puede generar un evento cuando cambia su valor, y una aplicación cliente puede registrarse para recibir ese evento.

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

Los lenguajes .NET acceden a propiedades en nativo C++ / objeto CX como si estuvieran en un objeto de .NET Framework.

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>Delegados y eventos
Un delegado es un tipo de Windows Runtime que representa un objeto de función. Puedes usar delegados en relación con eventos, devoluciones de llamada y llamadas a métodos asincrónicos para especificar una acción que debe realizarse más tarde. Del mismo modo que un objeto de función, el delegado proporciona seguridad de tipos mediante la habilitación del compilador para verificar el tipo devuelto y los tipos de parámetros de la función. La declaración de un delegado es similar a una firma de función, la implementación es similar a una definición de clase y la invocación es similar a una invocación de función.

## <a name="adding-an-event-listener"></a>Agregar una escucha de eventos
Puedes usar la palabra clave de evento para declarar a un miembro público de un tipo de delegado especificado. El código de cliente se suscribe al evento mediante los mecanismos estándar que se proporcionan en el lenguaje en particular.

```cpp
public:
    event SomeHandler^ someEvent;
```

En este ejemplo se usa el mismo código de C++ que para la sección de propiedades anterior.

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

En los lenguajes .NET, suscribirse a un evento en un componente de C++ es lo mismo que suscribirse a un evento en una clase de .NET Framework:

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>Agregar varias escuchas de eventos para un evento
JavaScript tiene un método de addEventListener que permite que varios controladores se suscriban a un solo evento.

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

En C#, cualquier número de controladores de eventos puede suscribirse al evento mediante el operador +=, como se muestra en el ejemplo anterior.

## <a name="enums"></a>Enumeraciones
Una numeración de Windows Runtime en C++ / CX se declara mediante la enumeración de clase pública; se parece a una numeración de ámbito en C++ estándar.

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

Valores de enumeración se pasan entre C++ / CX y JavaScript como enteros. Opcionalmente, puede declarar un objeto JavaScript que contiene los mismos valores con nombre que C++ / CX enumeración y el uso como sigue.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

Tanto C# como Visual Basic tienen son compatibles con el lenguaje para las enumeraciones. Estos lenguajes ven una clase de enumeración pública de C++ del mismo modo que verían una enumeración de .NET Framework.

## <a name="asynchronous-methods"></a>Métodos asincrónicos
Para consumir métodos asincrónicos expuestos por otros objetos de Windows Runtime, usa la [Clase de tarea (Runtime de simultaneidad)](https://msdn.microsoft.com/library/hh750113.aspx). Para obtener más información, consulta [Paralelismo de tareas (Runtime de simultaneidad)](https://msdn.microsoft.com/library/dd492427.aspx).

Para implementar los métodos asincrónicos en C++ / CX, usa la función [create\_async](https://msdn.microsoft.com/library/hh750102.aspx) que se define en ppltasks.h. Para obtener más información, consulta [crear operaciones asincrónicas en C++ / CX para aplicaciones para UWP](https://msdn.microsoft.com/library/vstudio/hh750082.aspx). Por ejemplo, consulta [Tutorial: crear un componente básico de Windows Runtime en C++ / CX y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). Los lenguajes .NET consumen C++ / métodos asincrónicos de CX igual que con cualquier método asincrónico que se define en .NET Framework.

## <a name="exceptions"></a>Excepciones
Puedes iniciar cualquier tipo de excepción definido por Windows Runtime. No puedes derivar tipos personalizados de ningún tipo de excepción de Windows Runtime. Sin embargo, puedes iniciar COMException y proporcionar un HRESULT personalizado al que se pueda acceder mediante el código que captura la excepción. No hay ninguna manera de especificar un mensaje personalizado en una COMException.

## <a name="debugging-tips"></a>Consejos de depuración
Cuando se depura una solución de JavaScript que tiene un archivo DLL del componente, puedes configurar el depurador para habilitar o bien la ejecución paso a paso a través del script, o bien del código nativo en el componente, pero no ambas opciones al mismo tiempo. Para cambiar la configuración, selecciona el nodo del proyecto de JavaScript en el Explorador de soluciones y, a continuación, elige Propiedades, Depuración, Tipo de depurador.

Asegúrate de seleccionar las funcionalidades adecuadas en el diseñador de paquetes. Por ejemplo, si estás intentando abrir un archivo de imagen en la biblioteca de imágenes del usuario mediante las API de Windows Runtime, asegúrate de seleccionar la casilla de la biblioteca de imágenes en el panel de capacidades del diseñador de manifiestos.

Si tu código JavaScript aparentemente no reconoce las propiedades o métodos públicos en el componente, asegúrate de que estás usando la convención Camel de mayúsculas y minúsculas en JavaScript. Por ejemplo, el LogCalc cuanto a c++ / CX método debe referenciarse como logCalc en JavaScript.

Si quitas C++ / CX Windows Runtime proyecto de componente de una solución, también debes quitar manualmente la referencia de proyecto del proyecto de JavaScript. De lo contrario, no se efectuará la depuración o las operaciones de compilación posteriores. Si es necesario, a continuación puedes agregar una referencia de ensamblado a la DLL.

## <a name="related-topics"></a>Temas relacionados
* [Tutorial: Crear un componente básico de Windows Runtime en C++ / CX y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)

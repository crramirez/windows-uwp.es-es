---
title: Componentes de Windows Runtime con C++/CX
description: En este tema se muestra cómo usar C++/CX para crear un componente de Windows Runtime&mdash;, que es un componente que se puede llamar desde una aplicación universal de Windows compilada mediante el lenguaje de Windows Runtime.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 288795b2dc189dae7b350a30446410b40044d08f
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192955"
---
# <a name="windows-runtime-components-with-ccx"></a>Componentes de Windows Runtime con C++/CX

> [!NOTE]
> Este tema le ayudará a mantener su aplicación de C++/CX. Pero se recomienda usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para las nuevas aplicaciones. C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Para obtener información sobre cómo crear un componente de Windows Runtime con C++/WinRT, consulte [Componentes de Windows Runtime con C++/WinRT](./create-a-windows-runtime-component-in-cppwinrt.md).

En este tema se muestra cómo usar C++/CX para crear un componente de Windows Runtime &mdash; un componente al que se puede llamar desde una aplicación universal de Windows compilada con cualquier lenguaje de Windows Runtime (C#, Visual Basic, C++ o JavaScript).

Hay varias razones para compilar un componente de Windows Runtime en C++.
- Para obtener la ventaja de rendimiento de C++ en operaciones complejas o de computación intensivas.
- Reutilizar el código que ya se ha escrito y probado.

Al compilar una solución que contiene un proyecto de JavaScript o. NET y un proyecto de componente de Windows Runtime, los archivos de proyecto de JavaScript y la DLL compilada se combinan en un paquete que se puede depurar localmente en el simulador o remotamente en un dispositivo amarrado. También puedes distribuir solo el proyecto del componente como un SDK de extensión. Para más información, vea [Crear un Kit de desarrollo de Software](/visualstudio/extensibility/creating-a-software-development-kit).

En general, al codificar el componente de C++/CX, use la biblioteca normal de C++ y los tipos integrados, excepto en el límite de la interfaz binaria abstracta (ABI) donde se pasan los datos hacia y desde el código en otro paquete. winmd. Allí, use tipos de Windows Runtime y la sintaxis especial que C++/CX admite para crear y manipular esos tipos. Además, en el código de C++/CX, use tipos como Delegate y Event para implementar eventos que se puedan generar desde su componente y que se controlen en JavaScript, Visual Basic, C++ o C#. Para obtener más información sobre la sintaxis de C++/CX, vea [Referencia del lenguaje Visual C++ (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx).

## <a name="casing-and-naming-rules"></a>Reglas de nomenclatura y del uso de mayúsculas y minúsculas

### <a name="javascript"></a>JavaScript
JavaScript distingue mayúsculas de minúsculas. Por lo tanto, debes seguir estas convenciones sobre el uso de mayúsculas y minúsculas:

-   Cuando hagas referencia a las clases y espacios de nombres de C++, usa las mayúsculas y minúsculas del mismo modo que en el lado de C++.
-   Al llamar a métodos, usa la convención Camel de mayúsculas y minúsculas incluso si el nombre del método está en mayúsculas en el lado de C++. Por ejemplo, "GetDate()" del método de C++ debe llamarse desde JavaScript como "getDate()".
-   Un nombre de clase activable y un nombre de espacio de nombres no pueden contener caracteres UNICODE.

### <a name="net"></a>.NET
Los lenguajes .NET siguen las reglas normales del uso de mayúsculas y minúsculas.

## <a name="instantiating-the-object"></a>Crear una instancia del objeto
Solo los tipos de Windows Runtime pueden pasarse a través del límite de la ABI. El compilador generará un error si el componente tiene un tipo como std::wstring como un parámetro o tipo devuelto en un método público. Los tipos integrados de las extensiones de componentes de Visual C++ (C++/CX) incluyen los valores escalares habituales, como int y Double, y también sus equivalentes de definición de tipo Int32, float64, etc. Para obtener más información, consulta [Sistema de tipos (C++/CX)](/cpp/cppcx/type-system-c-cx).

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

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>Tipos integrados de C++/CX, tipos de biblioteca y tipos de Windows Runtime
Una clase activable (también conocida como "clase de referencia") es una clase en torno a la cual se pueden crear instancias desde otro lenguaje, como JavaScript, C# o Visual Basic. Para ser consumible en otro idioma, un componente debe contener al menos una clase activable.

Un componente de Windows Runtime puede contener varias clases activables públicas, así como clases adicionales conocidas solo internamente por el componente. Aplique el atributo [WebHostHidden](/uwp/api/windows.foundation.metadata.webhosthiddenattribute) a los tipos de C++/CX que no están diseñados para ser visibles para JavaScript.

Todas las clases públicas deben residir en el mismo espacio de nombres de raíz con el mismo nombre que el archivo de metadatos del componente. Por ejemplo, se pueden crear instancias de una clase denominada A.B.C.MyClass solo si está definida en un archivo de metadatos denominado A.winmd, A.B.winmd o A.B.C.winmd. El nombre de la DLL no tiene que coincidir con el nombre del archivo .winmd.

El código de cliente crea una instancia del componente con la palabra clave **new** (**New** en Visual Basic) del mismo modo que para cualquier otra clase.

Una clase activable debe declararse como **public ref class sealed**. La palabra clave **ref class** indica al compilador que cree la clase como tipo compatible de Windows Runtime, y la palabra clave sealed especifica que la clase no puede heredarse. Windows Runtime actualmente no es compatible con un modelo de herencia generalizada; un modelo de herencia limitado es compatible con la creación de controles de XAML personalizados. Para obtener más información, consulta [Clases y estructuras de referencia (C++/CX)](/cpp/cppcx/ref-classes-and-structs-c-cx).

En C++/CX, todos los primitivos numéricos se definen en el espacio de nombres predeterminado. El espacio de nombres [Platform](/cpp/cppcx/platform-namespace-c-cx) contiene clases de C++/CX que son específicas del sistema de tipos Windows Runtime. Estos incluyen las clases [Platform::String](/cpp/cppcx/platform-string-class) y [Platform::Object](/cpp/cppcx/platform-object-class). Los tipos de colección concreta, como las clases [Platform::Collections::Map](/cpp/cppcx/platform-collections-map-class) y [Platform::Collections::Vector](/cpp/cppcx/platform-collections-vector-class), se definen en el espacio de nombres [Platform::Collections](/cpp/cppcx/platform-collections-namespace). Las interfaces públicas que implementan estos tipos se definen en [Windows::Foundation::Collections Namespace (C++/CX)](/cpp/cppcx/windows-foundation-collections-namespace-c-cx). Estos son los tipos de interfaz consumidos por JavaScript, C# y Visual Basic. Para obtener más información, consulta [Sistema de tipos (C++/CX)](/cpp/cppcx/type-system-c-cx).

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

Para pasar los Structs de valor definidos por el usuario a través de la ABI, defina un objeto de JavaScript que tenga los mismos miembros que el struct de valor que se define en C++/CX. Después, puede pasar ese objeto como un argumento a un método de C++/CX para que el objeto se convierta implícitamente al tipo de C++/CX.

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

En los lenguajes de .NET, solo tiene que crear una variable del tipo que se define en el componente de C++/CX.

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
Una clase Ref pública de C++/CX puede contener métodos sobrecargados, pero JavaScript tiene una capacidad limitada para diferenciar los métodos sobrecargados. Por ejemplo, puede distinguir entre estas firmas:

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

Pero no puede indicar la diferencia entre estas:

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

En casos ambiguos, puedes asegurarte de que JavaScript llame siempre a una sobrecarga específica aplicando el atributo [Windows::Foundation::Metadata::DefaultOverload](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) a la firma del método en el archivo de encabezado.

Este código JavaScript siempre llama a la sobrecarga atribuida:

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
Los lenguajes de .NET reconocen las sobrecargas en una clase Ref de C++/CX como en cualquier clase .NET.

## <a name="datetime"></a>DateTime
En Windows Runtime, un objeto [Windows::Foundation::DateTime](/uwp/api/windows.foundation.datetime) es simplemente un entero firmado de 64 bits que representa el número de intervalos de 100 nanosegundos o bien antes o bien después del 1 de enero de 1601. No hay métodos en un objeto de Windows:Foundation::DateTime. En su lugar, cada idioma proyecta la fecha y hora de forma nativa para ese lenguaje: el objeto Date en JavaScript y los tipos System. DateTime y System. DateTimeOffset en .NET.

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

Cuando se pasa un valor DateTime de C++/CX a JavaScript, JavaScript lo acepta como un objeto de fecha y lo muestra de forma predeterminada como una cadena de fecha de formato largo.

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

Cuando un lenguaje .NET pasa un System. DateTime a un componente de C++/CX, el método lo acepta como Windows:: Foundation::D ateTime. Cuando el componente pasa un elemento Windows:: Foundation::D ateTime a un método .NET, el método Framework lo acepta como DateTimeOffset.

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
Las colecciones siempre se pasan a través del límite de la ABI como controladores para los tipos de Windows Runtime como Windows::Foundation::Collections::IVector^ y Windows::Foundation::Collections::IMap^. Por ejemplo, si devuelves un controlador a Platform::Collections::Map, este lo convierte implícitamente a Windows::Foundation::Collections::IMap^. Las interfaces de colección se definen en un espacio de nombres que es independiente de las clases de C++/CX que proporcionan las implementaciones concretas. Los lenguajes de JavaScript y .NET consumen las interfaces. Para obtener más información, consulta [Colecciones (C++/CX)](/cpp/cppcx/collections-c-cx) y [Matriz y WriteOnlyArray (C++/CX)](/cpp/cppcx/array-and-writeonlyarray-c-cx).

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
Una clase Ref pública en las extensiones de componentes de C++/CX expone los miembros de datos públicos como propiedades, mediante la palabra clave Property. El concepto es idéntico a las propiedades de .NET. Una propiedad trivial es similar a un miembro de datos porque su funcionalidad es implícita. Una propiedad no trivial tiene descriptores de acceso get y set explícitos y una variable privada con nombre que es la "memoria auxiliar" para el valor. En este ejemplo, la variable de miembro privado \_ propertyAValue es la memoria auxiliar de la propiedada. Una propiedad puede generar un evento cuando cambia su valor, y una aplicación cliente puede registrarse para recibir ese evento.

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

Los lenguajes .NET tienen acceso a las propiedades de un objeto nativo de C++/CX tal como lo harían en un objeto .NET.

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

En los lenguajes .NET, la suscripción a un evento en un componente de C++ es la misma que la suscripción a un evento en una clase .NET:

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
Una enumeración de Windows Runtime en C++/CX se declara mediante la enumeración de clase pública; se parece a una enumeración con ámbito en C++ estándar.

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

Los valores de enumeración se pasan entre C++/CX y JavaScript como enteros. Opcionalmente, puede declarar un objeto de JavaScript que contenga los mismos valores con nombre que la enumeración de C++/CX y usarlo como se indica a continuación.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

Tanto C# como Visual Basic tienen son compatibles con el lenguaje para las enumeraciones. Estos lenguajes ven una clase de enumeración pública de C++ tal como verán una enumeración .NET.

## <a name="asynchronous-methods"></a>Métodos asincrónicos
Para consumir métodos asincrónicos expuestos por otros objetos de Windows Runtime, usa la [Clase de tarea (Runtime de simultaneidad)](/cpp/parallel/concrt/reference/task-class). Para obtener más información, consulta [Paralelismo de tareas (Runtime de simultaneidad)](/cpp/parallel/concrt/task-parallelism-concurrency-runtime).

Para implementar métodos asincrónicos en C++/CX, use la función [Create \_ Async](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) que se define en ppltasks. h. Para obtener más información, consulte [crear operaciones asincrónicas en C++/CX para aplicaciones para UWP](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps). Para obtener un ejemplo, vea [tutorial sobre cómo crear un componente de Windows Runtime de C++/CX y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). Los lenguajes .NET consumen métodos asincrónicos de C++/CX del mismo modo que cualquier método asincrónico definido en .NET.

## <a name="exceptions"></a>Excepciones
Puedes iniciar cualquier tipo de excepción definido por Windows Runtime. No puedes derivar tipos personalizados de ningún tipo de excepción de Windows Runtime. Sin embargo, puedes iniciar COMException y proporcionar un HRESULT personalizado al que se pueda acceder mediante el código que captura la excepción. No hay ninguna manera de especificar un mensaje personalizado en una COMException.

## <a name="debugging-tips"></a>Sugerencias de depuración
Cuando se depura una solución de JavaScript que tiene un archivo DLL del componente, puedes configurar el depurador para habilitar o bien la ejecución paso a paso a través del script, o bien del código nativo en el componente, pero no ambas opciones al mismo tiempo. Para cambiar la configuración, selecciona el nodo del proyecto de JavaScript en el Explorador de soluciones y, a continuación, elige Propiedades, Depuración, Tipo de depurador.

Asegúrate de seleccionar las funcionalidades adecuadas en el diseñador de paquetes. Por ejemplo, si estás intentando abrir un archivo de imagen en la biblioteca de imágenes del usuario mediante las API de Windows Runtime, asegúrate de seleccionar la casilla de la biblioteca de imágenes en el panel de capacidades del diseñador de manifiestos.

Si tu código JavaScript aparentemente no reconoce las propiedades o métodos públicos en el componente, asegúrate de que estás usando la convención Camel de mayúsculas y minúsculas en JavaScript. Por ejemplo, se debe hacer referencia al método LogCalc de C++/CX como logCalc en JavaScript.

Si quita de una solución un proyecto de componente de Windows Runtime de C++/CX, también debe quitar manualmente la referencia de proyecto del proyecto de JavaScript. De lo contrario, no se efectuará la depuración o las operaciones de compilación posteriores. Si es necesario, a continuación puedes agregar una referencia de ensamblado a la DLL.

## <a name="related-topics"></a>Temas relacionados
* [Tutorial para crear un componente de Windows Runtime en C++/CX y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)
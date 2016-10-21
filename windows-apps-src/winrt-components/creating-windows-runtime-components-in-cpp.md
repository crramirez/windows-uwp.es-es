---
author: msatranjr
title: Crear componentes de Windows Runtime en C++
description: "En este artículo se muestra cómo usar C++ para crear un componente de Windows Runtime, es decir, un archivo DLL al que se puede llamar desde una aplicación universal de Windows compilada con JavaScript, o C#, Visual Basic o C++."
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
translationtype: Human Translation
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 65114d476da1a7f9113987ebccc8bdbaca6381a7

---


# Crear componentes de Windows Runtime en C++


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este artículo se muestra cómo usar C++ para crear un componente de Windows Runtime, es decir, un archivo DLL al que se puede llamar desde una aplicación universal de Windows compilada con JavaScript, o C#, Visual Basic o C++.

Hay varias razones para compilar un componente de este tipo:

-   Para obtener la ventaja de rendimiento de C++ en operaciones complejas o de computación intensivas.

-   Reutilizar el código que ya se ha escrito y probado.

Al compilar una solución que contiene un proyecto de JavaScript o. NET y un proyecto de componente de Windows Runtime, los archivos de proyecto de JavaScript y la DLL compilada se combinan en un paquete que se puede depurar localmente en el simulador o remotamente en un dispositivo amarrado. También puedes distribuir solo el proyecto del componente como un SDK de extensión. Para obtener más información, consulta [Crear un kit de desarrollo de software](https://msdn.microsoft.com/library/hh768146.aspx).

En general, cuando codifiques tu componente C++, usa la biblioteca de C++ regular y los tipos integrados, excepto en los límites de la interfaz binaria abstracta (ABI), donde pasas los datos hacia y desde el código de otro paquete .winmd. Allí, usa los tipos de Windows Runtime y la sintaxis especial compatible con Visual C++ para crear y manipular esos tipos. Además, en tu código de Visual C++, usa tipos como delegate y event para implementar los eventos que se pueden desencadenar desde tu componente y controlar en JavaScript, Visual Basic o C#. Para obtener más información sobre la nueva sintaxis de Visual C++, consulta [ Referencia del lenguaje Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## Reglas de nomenclatura y del uso de mayúsculas y minúsculas


### JavaScript

JavaScript distingue mayúsculas de minúsculas. Por lo tanto, debes seguir estas convenciones sobre el uso de mayúsculas y minúsculas:

-   Cuando hagas referencia a las clases y espacios de nombres de C++, usa las mayúsculas y minúsculas del mismo modo que en el lado de C++.
-   Al llamar a métodos, usa la convención Camel de mayúsculas y minúsculas incluso si el nombre del método está en mayúsculas en el lado de C++. Por ejemplo, "GetDate()" del método de C++ debe llamarse desde JavaScript como "getDate()".
-   Un nombre de clase activable y un nombre de espacio de nombres no pueden contener caracteres UNICODE.

### .NET

Los lenguajes .NET siguen las reglas normales del uso de mayúsculas y minúsculas.

## Crear una instancia del objeto


Solo los tipos de Windows Runtime pueden pasarse a través del límite de la ABI. El compilador generará un error si el componente tiene un tipo como std::wstring como un parámetro o tipo devuelto en un método público. Los tipos integrados de las extensiones del componente Visual C++ (C++/CX) incluyen los escalares habituales como int y double, así como sus equivalentes typedef int32, float64, etc. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

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

## Tipos integrados de C++, tipos de biblioteca y tipos de Windows Runtime


Una clase activable (también conocida como "clase de referencia") es una clase en torno a la cual se pueden crear instancias desde otro lenguaje, como JavaScript, C# o Visual Basic. Para ser consumible en otro idioma, un componente debe contener al menos una clase activable.

Un componente de Windows Runtime puede contener varias clases activables públicas, así como clases adicionales conocidas solo internamente por el componente. Aplicar el atributo [WebHostHidden](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.webhosthiddenattribute.aspx) a los tipos de C++ que no están destinados a ser visibles para JavaScript.

Todas las clases públicas deben residir en el mismo espacio de nombres de raíz con el mismo nombre que el archivo de metadatos del componente. Por ejemplo, para una clase que se denomina A.B.C.MyClass solo puede crearse una instancia si se define en un archivo de metadatos denominado A.winmd o A.B.winmd o A.B.C.winmd. El nombre de la DLL no es necesario que coincida con el nombre de archivo .winmd.

El código de cliente crea una instancia del componente con la palabra clave **new** (**New** en Visual Basic) del mismo modo que para cualquier otra clase.

Una clase activable debe declararse como **public ref class sealed**. La palabra clave **ref class** indica al compilador que cree la clase como tipo compatible de Windows Runtime, y la palabra clave sealed especifica que la clase no puede heredarse. Windows Runtime actualmente no es compatible con un modelo de herencia generalizada; un modelo de herencia limitado es compatible con la creación de controles de XAML personalizados. Para obtener más información, consulta [Clases y estructuras de referencia (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699870.aspx).

En C++, se definen todos los primitivos numéricos en el espacio de nombres predeterminado. El espacio de nombres [Plataforma](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx) contiene las clases de C++ que son específicas para el sistema de tipos de Windows Runtime. Estos incluyen las clases [Platform::String](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) y [Platform::Object](https://msdn.microsoft.com/library/windows/apps/xaml/hh748265.aspx). Los tipos de colección concreta, como las clases [Platform::Collections::Map](https://msdn.microsoft.com/library/windows/apps/xaml/hh441508.aspx) y [Platform::Collections::Vector](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx), se definen en el espacio de nombres [Platform::Collections](https://msdn.microsoft.com/library/windows/apps/xaml/hh710418.aspx). Las interfaces públicas que implementan estos tipos se definen en [Windows::Foundation::Collections Namespace (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh441496.aspx). Estos son los tipos de interfaz consumidos por JavaScript, C# y Visual Basic. Para obtener más información, consulta [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

## Método que devuelve un valor de tipo integrado

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

## Método que devuelve una estructura de valor personalizada

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

Para pasar las estructuras de valor definidas por el usuario a través de la ABI, define un objeto JavaScript que tenga los mismos miembros que la estructura de valor definida en C++. A continuación, puedes pasar ese objeto como argumento a un método de C++ para que el objeto se convierta implícitamente al tipo de C++.

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

En los lenguajes .NET, solo tienes que crear una variable del tipo que se define en el componente de C++.

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

## Métodos sobrecargados


Una clase de referencia pública de C++ puede contener métodos sobrecargados, pero JavaScript tiene una capacidad limitada para diferenciar los métodos sobrecargados. Por ejemplo, puede distinguir entre estas firmas:

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

## .NET


Los lenguajes .NET reconocen sobrecargas en una clase de referencia de C++, del mismo modo que en cualquier clase de .NET Framework.

## DateTime

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

Cuando se pasa un valor de DateTime de C++ a JavaScript, JavaScript lo acepta como objeto Date y lo muestra de forma predeterminada como una cadena de fecha de formato largo.

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

Cuando un lenguaje .NET pasa System.DateTime a un componente de C++, el método lo acepta como Windows::Foundation::DateTime. Cuando el componente pasa Windows::Foundation::DateTime a un método de .NET Framework, el método Framework lo acepta como DateTimeOffset.

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

## Colecciones y matrices


Las colecciones siempre se pasan a través del límite de la ABI como controladores para los tipos de Windows Runtime como Windows::Foundation::Collections::IVector^ y Windows::Foundation::Collections::IMap^. Por ejemplo, si devuelves un controlador a Platform::Collections::Map, este lo convierte implícitamente a Windows::Foundation::Collections::IMap^. Las interfaces de colección se definen en un espacio de nombres que es independiente de las clases de C++ que proporcionan las implementaciones concretas. Los lenguajes de JavaScript y .NET consumen las interfaces. Para obtener más información, consulta [Colecciones (C++/CX)](https://msdn.microsoft.com//library/windows/apps/hh700103.aspx) y [Matriz y WriteOnlyArray (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh700131.aspx).

## Pasar el IVector


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

## Pasar IMap


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

## Propiedades


Una clase de referencia pública en extensiones de componentes de Visual C++ expone los miembros de datos públicos como propiedades, mediante la palabra clave de propiedad. El concepto es idéntico a las propiedades de .NET Framework. Una propiedad trivial es similar a un miembro de datos porque su funcionalidad es implícita. Una propiedad no trivial tiene descriptores de acceso get y set explícitos y una variable privada con nombre que es la "memoria auxiliar" para el valor. En este ejemplo, la variable de miembro privada \_propertyAValue es la copia de seguridad para PropertyA. Una propiedad puede generar un evento cuando cambia su valor, y una aplicación cliente puede registrarse para recibir ese evento.

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

Los lenguajes .NET acceden a propiedades en un objeto de C++ nativo del mismo modo que lo harían en un objeto de .NET Framework.

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

## Delegados y eventos


Un delegado es un tipo de Windows Runtime que representa un objeto de función. Puedes usar delegados en relación con eventos, devoluciones de llamada y llamadas a métodos asincrónicos para especificar una acción que debe realizarse más tarde. Del mismo modo que un objeto de función, el delegado proporciona seguridad de tipos mediante la habilitación del compilador para verificar el tipo devuelto y los tipos de parámetros de la función. La declaración de un delegado es similar a una firma de función, la implementación es similar a una definición de clase y la invocación es similar a una invocación de función.

## Agregar una escucha de eventos


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

## Agregar varias escuchas de eventos para un evento


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

## Enumeraciones


Una numeración de Windows Runtime en C++ se declara mediante la enumeración de clase pública; es similar a una numeración de ámbito en C++ estándar.

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

Los valores de enumeración se pasan entre C++ y JavaScript como enteros. Opcionalmente, puedes declarar un objeto de JavaScript que contenga los mismos valores con nombre que la enumeración de C++ y usarlo como sigue.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

Tanto C# como Visual Basic tienen son compatibles con el lenguaje para las enumeraciones. Estos lenguajes ven una clase de enumeración pública de C++ del mismo modo que verían una enumeración de .NET Framework.

## Métodos asincrónicos


Para consumir métodos asincrónicos expuestos por otros objetos de Windows Runtime, usa la [Clase de tarea (Runtime de simultaneidad)](https://msdn.microsoft.com/library/hh750113.aspx). Para obtener más información, consulta [Paralelismo de tareas (Runtime de simultaneidad)](https://msdn.microsoft.com/library/dd492427.aspx).

Para implementar los métodos asincrónicos en C++, usa la función [create\_async](https://msdn.microsoft.com/library/hh750102.aspx) que se define en ppltasks.h. Para obtener más información, consulta [Crear operaciones asincrónicas en C++ para aplicaciones de la Tienda Windows](https://msdn.microsoft.com/library/vstudio/hh750082.aspx). Para ver un ejemplo, consulta [Tutorial: crear un componente básico de Windows Runtime en C++ y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). Los lenguajes .NET consumen métodos asincrónicos de C++, del mismo modo que con cualquier método asincrónico definido en .NET Framework.

## Excepciones


Puedes iniciar cualquier tipo de excepción definido por Windows Runtime. No puedes derivar tipos personalizados de ningún tipo de excepción de Windows Runtime. Sin embargo, puedes iniciar COMException y proporcionar un HRESULT personalizado al que se pueda acceder mediante el código que captura la excepción. No hay ninguna manera de especificar un mensaje personalizado en una COMException.

## Consejos de depuración


Cuando se depura una solución de JavaScript que tiene un archivo DLL del componente, puedes configurar el depurador para habilitar o bien la ejecución paso a paso a través del script, o bien del código nativo en el componente, pero no ambas opciones al mismo tiempo. Para cambiar la configuración, selecciona el nodo del proyecto de JavaScript en el Explorador de soluciones y, a continuación, elige Propiedades, Depuración, Tipo de depurador.

Asegúrate de seleccionar las funcionalidades adecuadas en el diseñador de paquetes. Por ejemplo, si estás intentando abrir un archivo de imagen en la biblioteca de imágenes del usuario mediante las API de Windows Runtime, asegúrate de seleccionar la casilla de la biblioteca de imágenes en el panel de capacidades del diseñador de manifiestos.

Si tu código JavaScript aparentemente no reconoce las propiedades o métodos públicos en el componente, asegúrate de que estás usando la convención Camel de mayúsculas y minúsculas en JavaScript. Por ejemplo, el método LogCalc C++ debe referenciarse como logCalc en JavaScript.

Si quitas un proyecto de componente de Windows Runtime de C++ de una solución, también debes quitar manualmente la referencia del proyecto de JavaScript. De lo contrario, no se efectuará la depuración o las operaciones de compilación posteriores. Si es necesario, a continuación puedes agregar una referencia de ensamblado a la DLL.

## Temas relacionados

* [Tutorial: Crear en C++ un componente básico de Windows Runtime y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)



<!--HONumber=Aug16_HO3-->



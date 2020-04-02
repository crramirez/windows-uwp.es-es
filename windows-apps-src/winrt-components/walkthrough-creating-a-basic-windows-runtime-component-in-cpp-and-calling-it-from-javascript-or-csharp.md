---
title: Tutorial para crear un C++componente de Windows Runtime/CX y llamarlo desde JavaScript oC#
description: En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b4cd9bb5921209be852e183e1fa7a93ea18816a
ms.sourcegitcommit: 5618242614997045593821fdbe5ed8878fd8c01e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80578684"
---
# <a name="walkthrough-of-creating-a-ccx-windows-runtime-component-and-calling-it-from-javascript-or-c"></a>Tutorial para crear un C++componente de Windows Runtime/CX y llamarlo desde JavaScript oC#

> [!NOTE]
> Este tema existe para ayudar a mantener tu aplicación de C++/CX. Pero te recomendamos que uses [C++ / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para nuevas aplicaciones. C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Para obtener información sobre cómo crear un componente de C++Windows Runtime mediante/WinRT, vea crear [eventos en C++/WinRT](../cpp-and-winrt-apis/author-events.md).

En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic. Antes de iniciar este tutorial, asegúrate de familiarizarte con conceptos tales como la interfaz binaria abstracta (ABI), las clases ref y Extensiones de componentes de Visual C++ que facilitan el trabajo con las clases ref. Para obtener más información, consulte [componentes de C++Windows Runtime con/CX](creating-windows-runtime-components-in-cpp.md) y [Referencia delC++lenguaje visual C++ (/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

## <a name="creating-the-c-component-dll"></a>Crear la DLL del componente de C++
En este ejemplo, se crea en primer lugar el proyecto de componente, pero se podría crear el proyecto de JavaScript primero. El orden no importa.

Tenga en cuenta que la clase principal del componente contiene ejemplos de definiciones de propiedades y métodos, así como una declaración de evento. Estos se proporcionan solo para mostrar cómo se hizo. No son necesarios y, en este ejemplo, vamos a reemplazar todo el código generado con nuestro propio código.

### <a name="to-create-the-c-component-project"></a>**Para crear el C++ proyecto de componente**
1. En la barra de menús de Visual Studio, elige **Archivo, Nuevo, Proyecto**.

2. En el cuadro de diálogo **Nuevo proyecto**, en el panel izquierdo, expande **Visual C++** y, a continuación, selecciona el nodo para las aplicaciones universales de Windows.

3. En el panel central, seleccione **Windows Runtime componente** y, a continuación, asigne al proyecto el nombre WINRT\_CPP.

4. Elige el botón **Aceptar**.

## <a name="to-add-an-activatable-class-to-the-component"></a>**Para agregar una clase activable al componente**
Una clase activable es la que puede crear el código de cliente mediante una **nueva** expresión (**Nuevo** en Visual Basic, o **ref new** en C++). En tu componente, debe declararse como **public ref class sealed**. De hecho, los archivos Class1.h y .cpp ya tienen una clase ref. Puedes cambiar el nombre, pero en este ejemplo, usaremos el nombre predeterminado: Class1. Puede definir clases ref adicionales o clases regulares en el componente si es necesario. Para obtener más información sobre las clases de referencia, consulta [Sistema de tipo (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

Agregue estas \#incluir directivas a Class1. h:

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h es el archivo de encabezado para las clases concretas de C++ como la clase Platform::Collections::Vector y la clase Platform::Collections::Map , que implementan interfaces independientes del lenguaje definidas por Windows Runtime. Los encabezados de amp se usan para ejecutar cálculos en la GPU. No tienen ningún equivalente de Windows Runtime, y eso es satisfactorio puesto que son privados. En general, por motivos de rendimiento, debes usar el código ISO C++ y las bibliotecas estándar internamente en el componente; solo la interfaz de Windows Runtime debe expresarse en los tipos de Windows Runtime.

## <a name="to-add-a-delegate-at-namespace-scope"></a>Para agregar un delegado en el ámbito de espacio de nombres
Un delegado es una construcción que define los parámetros y el tipo de valor devuelto para los métodos. Un evento es una instancia de un tipo determinado de delegado, y cualquier método de controlador de eventos que se suscriba al evento debe tener la signatura especificada en el delegado. El siguiente código define un tipo de delegado que toma int y devuelve void. A continuación, el código declara un evento público de este tipo; esto permite que el código de cliente proporcione métodos que se invocan cuando se desencadena el evento.

Agrega la siguiente declaración de delegado en el ámbito de espacio de nombres en Class1.h, justo antes de la declaración de Class1.

```cpp
public delegate void PrimeFoundHandler(int result);
```

Si el código no se alinea correctamente al pegarlo en Visual Studio, simplemente presione Ctrl+K+D para corregir la sangría de todo el archivo.

## <a name="to-add-the-public-members"></a>Para agregar los miembros públicos
La clase expone tres métodos públicos y un evento público. El primer método es sincrónico porque siempre se ejecuta muy rápidamente. Dado que los otros dos métodos pueden tardar algún tiempo, son asincrónicos para que no bloqueen el subproceso de la interfaz de usuario. Estos métodos devuelven IAsyncOperationWithProgress e IAsyncActionWithProgress. El primero define un método async que devuelve un resultado, y el segundo define un método async que devuelve void. Estas interfaces también permiten al código de cliente recibir las notificaciones sobre el progreso de la operación.

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## <a name="to-add-the-private-members"></a>Para agregar los miembros privados
La clase contiene tres miembros privados: dos métodos del asistente para los cálculos numéricos y un objeto CoreDispatcher que se utiliza para calcular las referencias a las invocaciones de eventos desde los subprocesos de trabajo al subproceso de la interfaz de usuario.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

### <a name="to-add-the-header-and-namespace-directives"></a>Para agregar el encabezado y las directivas de espacio de nombres
1. En Class1.cpp, agrega estas directivas #include:

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

2. Ahora, agrega estas instrucciones using para extraer los espacios de nombres necesarios:

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>Para agregar la implementación para ComputeResult
En Class1.cpp, agrega la implementación de método siguiente. Este método se ejecuta sincrónicamente en el subproceso de llamada, pero es muy rápido porque utiliza C++ AMP para paralelizar el cálculo en la GPU. Para obtener más información, consulte Introducción a C++ AMP. Los resultados se anexan a un tipo concreto Platform::Collections::Vector<T>, que se convierte implícitamente a un Windows::Foundation::Collections::IVector<T> cuando se devuelve.

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>Para agregar la implementación para GetPrimesOrdered y el método del asistente
En Class1.cpp, agrega las implementaciones para GetPrimesOrdered y el método auxiliar is_prime. GetPrimesOrdered usa una clase concurrent_vector y un bucle de función parallel_for para dividir el trabajo y usa el máximo de recursos del equipo en el que se ejecuta el programa para producir resultados. Después de que se calculen, almacenen y ordenen los resultados, se agregan a Platform::Collections::Vector<T> y se devuelven como Windows::Foundation::Collections::IVector<T> al código de cliente.

Observe el código del indicador de progreso, que permite al cliente enlazar una barra de progreso u otro tipo de interfaz de usuario para mostrar al usuario cuánto tiempo queda para finalizar la operación. La notificación sobre el progreso tiene un costo. Debe desencadenarse un evento en el lado del componente y controlarse en el subproceso de la interfaz de usuario, y el valor de progreso se debe almacenar en cada iteración. Una manera de minimizar el costo consiste en limitar la frecuencia con la que se desencadena un evento de progreso. Si el costo sigue siendo prohibitivo, o si no puedes hacer un cálculo aproximado de la duración de la operación, considera la posibilidad de utilizar un anillo de progreso, que muestra que una operación está en curso pero no muestra el tiempo restante hasta la finalización.

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## <a name="to-add-the-implementation-for-getprimesunordered"></a>Para agregar la implementación para GetPrimesUnordered
El último paso para crear el componente de C++ es agregar la implementación para el GetPrimesUnordered en Class1.cpp. Este método devuelve cada resultado a medida que lo encuentra, sin esperar a que se encuentren todos los resultados. Cada resultado se devuelve en el controlador de eventos y se muestra en la interfaz de usuario en tiempo real. Una vez más, observa que se utiliza un indicador de progreso. Este método también usa el método auxiliar is_prime.

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## <a name="creating-a-javascript-client-app-visual-studio-2017"></a>Crear una aplicación cliente de JavaScript (Visual Studio 2017)

Si desea crear un C# cliente, puede omitir esta sección.

> [!NOTE]
> Los proyectos de Plataforma universal de Windows (UWP) no se admiten en Visual Studio 2019. Vea [JavaScript y TypeScript en Visual Studio 2019](/visualstudio/javascript/javascript-in-vs-2019?view=vs-2019#projects). Para seguir con esta sección, se recomienda usar Visual Studio 2017. Vea [JavaScript en Visual Studio 2017](/visualstudio/javascript/javascript-in-vs-2017).

### <a name="to-create-a-javascript-project"></a>Para crear un proyecto de JavaScript
1. En Explorador de soluciones (en Visual Studio 2017; vea la **Nota** anterior), abra el menú contextual del nodo de la solución y elija **Agregar, nuevo proyecto**.

2. Expande JavaScript (puede estar anidado en **Otros lenguajes**) y elige **Aplicación vacía (Windows Universal)** .

3. Elija el botón **Aceptar** para aceptar el nombre predeterminado&mdash;app1&mdash;.

4. Abre el menú contextual para el nodo de proyecto App1 y selecciona **Establecer como proyecto de inicio**.

5. Agregue una referencia de proyecto a WinRT_CPP:

6. Abre el menú contextual para el nodo Referencias y selecciona **Agregar referencia**.

7. En el panel izquierdo del cuadro de diálogo Administrador de referencias, selecciona **Proyectos** y, a continuación, selecciona **Solución**.

8. En el panel central, selecciona WinRT_CPP y, a continuación, elige el botón **Aceptar**.

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>Para agregar el código HTML que invoca los controladores de eventos de JavaScript
Pega este código HTML en el nodo <body> de la página default.html:

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## <a name="to-add-styles"></a>Para agregar estilos
En default.css, elimina el estilo de texto y, a continuación, agrega estos estilos:

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>Para agregar controladores de eventos de JavaScript que llaman a la DLL del componente
Agrega las siguientes funciones al final del archivo default.js: Se llama a estas funciones cuando se eligen los botones de la página principal. Observa cómo JavaScript genera clases de C++ y después llama a sus métodos y usa los valores devueltos para rellenar las etiquetas HTML.

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

Agrega código para agregar los agentes de escucha de eventos mediante el reemplazo de la llamada existente a WinJS.UI.processAll en app.onactivated en el archivo default.js por el siguiente código que implementa el registro de eventos en un bloque después. Para obtener una explicación detallada de esto, vea [creación de una aplicación "Hello, World" (JS)](/windows/uwp/get-started/create-a-hello-world-app-js-uwp).

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

Presione F5 para ejecutar la aplicación.

## <a name="creating-a-c-client-app"></a>Crear una aplicación cliente de C#

### <a name="to-create-a-c-project"></a>Para crear un proyecto de C#
1. En Explorador de soluciones, abre el menú contextual para el nodo Solución y, a continuación, elige **Agregar, Nuevo proyecto**.

2. Expande Visual C# (puede estar anidado en **Otros lenguajes**), selecciona **Windows** y después **Universal** en el panel izquierdo y, a continuación, selecciona **Aplicación vacía** en el panel central.

3. Dale el nombre de CS_Client a esta aplicación y, a continuación, elige el botón **Aceptar**.

4. Abre el menú contextual para el nodo de proyecto CS_Client y selecciona **Establecer como proyecto de inicio**.

5. Agregue una referencia de proyecto a WinRT_CPP:

   - Abre el menú contextual para el nodo **Referencias** y selecciona **Agregar referencia**.

   - En el panel izquierdo del cuadro de diálogo **Administrador de referencias,** selecciona **Proyectos** y, a continuación, selecciona **Solución**.

   - En el panel central, selecciona WinRT_CPP y, a continuación, elige el botón **Aceptar**.

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>Para agregar el código XAML que define la interfaz de usuario
Copia el siguiente código en el elemento de cuadrícula en MainPage.xaml.

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## <a name="to-add-the-event-handlers-for-the-buttons"></a>Para agregar controladores de eventos para los botones
En el Explorador de soluciones, abre MainPage.xaml.cs. (El archivo puede anidarse en MainPage.xaml). Agrega una directiva using para System.Text, y, a continuación, agrega el controlador de eventos para el cálculo del logaritmo en la clase MainPage.

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

Agrega el controlador de eventos para el resultado ordenado:

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

Agrega el controlador de eventos para el resultado sin ordenar y para el botón que borra los resultados, lo que te permitirá volver a ejecutar el código.

```csharp
private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (unordered): ");
    PrimesUnOrderedResult.Text = sb.ToString();

    // primeFoundEvent is a user-defined event in nativeObject
    // It passes the results back to this thread as they are produced
    // and the event handler that we define here immediately displays them.
    nativeObject.primeFoundEvent += (n) =>
    {
        sb.Append(n.ToString()).Append(" ");
        PrimesUnOrderedResult.Text = sb.ToString();
    };

    // Call the async method.
    var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

    // Provide a handler for the Progress event that the asyncResult
    // object fires at regular intervals. This handler updates the progress bar.
    asyncResult.Progress += (asyncInfo, progress) =>
        {
            PrimesUnOrderedProgress.Value = progress;
        };
}

private void Clear_Button_Click(object sender, RoutedEventArgs e)
{
    PrimesOrderedProgress.Value = 0;
    PrimesUnOrderedProgress.Value = 0;
    PrimesUnOrderedResult.Text = "";
    PrimesOrderedResult.Text = "";
    Result1.Text = "";
}
```

## <a name="running-the-app"></a>Ejecutar la aplicación
Selecciona o bien el proyecto de C# o o bien el proyecto de JavaScript como proyecto de inicio abriendo el menú contextual para el nodo del proyecto en el Explorador de soluciones y selecciona **Establecer como proyecto de inicio**. Presiona F5 para ejecutarlo con depuración o Ctrl+F5 para ejecutarlo sin depuración.

## <a name="inspecting-your-component-in-object-browser-optional"></a>Inspeccionar el componente en el Examinador de objetos (opcional)
En el Explorador de objetos, puedes inspeccionar todos los tipos de Windows Runtime que se definen en los archivos .winmd. Esto incluye los tipos en el espacio de nombres de plataforma y el espacio de nombres predeterminado. Sin embargo, dado que los tipos en el espacio de nombres Platform::Collections se definen en el archivo de encabezado collections.h, y no en un archivo winmd, no aparecen en el explorador de objetos.

### <a name="to-inspect-a-component"></a>**Para inspeccionar un componente**
1. En la barra de menús, elige **Vista, Explorador de objetos** (Ctrl+Alt+J).

2. En el panel izquierdo del Examinador de objetos, expanda el nodo WinRT\_CPP para mostrar los tipos y métodos definidos en el componente.

## <a name="debugging-tips"></a>Sugerencias de depuración
Para mejorar la experiencia al usar la depuración, descarga los símbolos de depuración de los servidores de símbolos públicos de Microsoft:

### <a name="to-download-debugging-symbols"></a>**Para descargar símbolos de depuración**
1. En la barra de menús, elige **Herramientas, Opciones**.

2. En el cuadro de diálogo de **Opciones**, expande **Depuración** y selecciona **Símbolos**.

3. Selecciona **Servidores de símbolos de Microsoft** y, a continuación, selecciona el botón **Aceptar**.

Es posible que la primera vez se tarde algo de tiempo en descargar los símbolos. Para acelerar el rendimiento la próxima vez que presiones F5, especifica un directorio local donde almacenar los símbolos en memoria caché.

Al depurar una solución de JavaScript que tenga un objeto DLL de componente, puedes establecer el depurador para habilitar el recorrido paso a paso a través del script o el recorrido paso a paso a través del código nativo del componente, pero no ambos al mismo tiempo. Para cambiar la configuración, abre el menú contextual para el nodo de proyecto de JavaScript en el Explorador de soluciones y elige **Propiedades, Depurar, Tipo de depurador**.

Asegúrese de seleccionar las capacidades adecuadas en el diseñador de paquetes. Puedes abrir el diseñador de paquetes abriendo el archivo Package.appxmanifest. Por ejemplo, si estás intentando acceder mediante programación a archivos en la carpeta Imágenes, asegúrate de seleccionar la casilla de verificación de la **Biblioteca de imágenes** en el panel **Funcionalidades** del diseñador de paquetes.

Si el código de JavaScript no reconoce los métodos o propiedades públicos del componente, asegúrate de que en JavaScript se esté usando el uso combinado de mayúsculas y minúsculas tipo Camel. Por ejemplo, se debe hacer referencia al método `ComputeResult` de C++ como `computeResult` en JavaScript.

Si quitas un proyecto de componente de Windows Runtime de C++ de una solución, también debes quitar manualmente la referencia del proyecto de JavaScript. De lo contrario, se impiden las operaciones de depuración o compilación subsiguientes. Si es necesario, puedes agregar una referencia de ensamblado al archivo DLL.

## <a name="related-topics"></a>Temas relacionados
* [Componentes de Windows Runtime con C++/CX](creating-windows-runtime-components-in-cpp.md)

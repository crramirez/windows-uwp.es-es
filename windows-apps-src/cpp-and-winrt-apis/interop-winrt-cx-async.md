---
description: Este es un tema avanzado relacionado con la migración gradual de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Muestra cómo las tareas y las corrutinas de la Biblioteca de patrones paralelos(PPL) pueden existir en paralelo en el mismo proyecto.
title: Asincronía e interoperabilidad entre C++/WinRT y C++/CX
ms.date: 08/06/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, proyección, portar, migrar, interoperabilidad, C++/CX, PPL, tarea, corrutina
ms.localizationpriority: medium
ms.openlocfilehash: daa263836e9024a0aaa55b239b1a0db9437f1cd5
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2020
ms.locfileid: "88181081"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>Asincronía e interoperabilidad entre C++/WinRT y C++/CX

> [!TIP]
> Aunque se recomienda leer este tema desde el principio, puede ir directamente a un resumen de las técnicas de interoperabilidad en la sección [Información general sobre la portabilidad asincrónica de C++/CX a C++/WinRT](#overview-of-porting-ccx-async-to-cwinrt).

Este es un tema avanzado relacionado con la portabilidad gradual de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) a [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx). En este tema se retoma donde se dejó en [Interoperabilidad entre C++/WinRT y C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx).

Si el tamaño o la complejidad del código base hacen necesario portar el proyecto gradualmente, necesitará un proceso de portabilidad en el que, en un momento, exista código de C++/CX y C++/WinRT en paralelo en el mismo proyecto. Si tiene código asincrónico, es posible que necesite que existan cadenas de tareas y corrutinas de la Biblioteca de patrones paralelos (PPL) en paralelo en el proyecto a medida que porta gradualmente el código fuente. Este tema se centra en las técnicas de interoperabilidad entre código asincrónico de C++/CX y código asincrónico de C++/WinRT. Puede utilizar estas técnicas de manera individual o conjunta.

Antes de leer este tema, se recomienda leer [Interoperabilidad entre C++/WinRT y C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx). En este tema se muestra cómo preparar el proyecto para la portabilidad gradual. Además, presenta dos funciones auxiliares que puede usarse para convertir un objeto de C++/CX y en un objeto de C++/WinRT (y viceversa). Este tema sobre la asincronía se basa en esa información y utiliza dichas funciones auxiliares.

> [!NOTE]
> La portabilidad gradual presenta algunas limitaciones. Si tiene un proyecto de componentes de Windows Runtime, no es posible la portabilidad gradual y necesitará portar el proyecto en un solo paso. Y para un proyecto XAML, en un momento dado, los tipos de página XAML deben estar, *o bien* todos en C++/WinRT, *o bien* todos en C++/CX. Para obtener más información, consulte [Migrar a C++/WinRT desde C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx).

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>Motivo por el que se dedica un tema entero a la interoperabilidad de código asincrónico

La portabilidad de C++/CX a C++/WinRT suele ser sencilla, con la única excepción de mover desde las tareas de la [Biblioteca de patrones paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) a corrutinas. Los modelos son diferentes. No hay una asignación natural de uno a uno de las tareas de PPL en corrutinas, y no hay ninguna manera sencilla de portar el código de forma mecánica que funcione en todos los casos.

La buena noticia es que la conversión de tareas a corrutinas genera simplificaciones significativas. Además, los equipos de desarrollo informan de forma rutinaria que, una vez que superan el obstáculo de portabilidad de su código asincrónico, el resto del trabajo de portabilidad es principalmente mecánico.

A menudo, un algoritmo se escribía originalmente para adaptarse a las API sincrónicas. Y luego se traducía en tareas y continuaciones explícitas&mdash;el resultado solía provocar una ofuscación involuntaria de la lógica subyacente. Por ejemplo, los bucles se convierten en recursividad; las ramas if-else se convierten en un árbol anidado (una cadena) de tareas; las variables compartidas se convierten en **shared_ptr**. Para deconstruir la estructura a menudo antinatural del código fuente de PPL, se recomienda que primero retroceda y comprenda la intención del código original (es decir, detectar la versión sincrónica original). Y, a continuación, inserte `co_await` en los lugares adecuados.

Por ese motivo, si tiene una versión de C# (en lugar de C++/CX) del código asincrónico a partir del cual empezar a portar, eso puede facilitar la tarea y ofrecer una portabilidad más ordenada. El código de C# usa `await`. Por lo tanto, el código de C# ya sigue una filosofía de comenzar con una versión sincrónica y, luego, insertar `await` en los lugares adecuados.

## <a name="some-background-in-asynchronous-programming"></a>Algo de contexto sobre la programación asincrónica

Para tener un marco firme de referencia para la terminología y los conceptos de programación asincrónica, vamos a enmarcar brevemente la programación asincrónica para Windows Runtime en general, y también cómo las proyecciones de los dos lenguajes de C++ se suman a esta cada una a su propio modo.

El proyecto tiene métodos que funcionan de forma asincrónica. En muchos casos, querrá esperar a que un trabajo se complete antes de hacer otra cosa. Para que pueda esperar en un método asincrónico, el método devuelve un objeto de operación asincrónica. Pero a veces no desea o no necesita esperar a que finalice el trabajo realizado de forma asincrónica. En este caso, el método no tiene que devolver un objeto de operación asincrónica. Un método asincrónico en el que no espera se conoce como método *fire-and-forget* (desencadenar y olvidar).

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Objetos asincrónicos de Windows Runtime (**IAsyncXxx**)

El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objeto de la operación asincrónica.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1) y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).

En este tema, cuando usamos la forma abreviada de **IAsyncXxx**, hacemos referencia a estos tipos colectivamente, o bien hablamos de uno de los cuatro tipos sin necesidad de especificar cuál.

### <a name="ccx-async"></a>C++/CX asincrónico

El código de C++/CX asincrónico usa tareas de la [Biblioteca de patrones paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl). Una tarea de PPL se representa mediante la clase [**concurrency::task**](/cpp/parallel/concrt/reference/task-class).

Normalmente, un método asincrónico de C++/CX encadena las tareas de PPL juntas mediante funciones lambda con [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) y [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then). Cada una de estas funciones lambda devuelve una tarea que, cuando se completa, genera un valor que luego se pasa a la expresión lambda de *continuación* de la tarea.

Como alternativa, en lugar de llamar a **create_task** para crear una tarea, un método asincrónico de C++/CX puede llamar a [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) para crear un **IAsyncXxx**\^.

Por lo tanto, el tipo devuelto de un método asincrónico de C++/CX puede ser una tarea de PPL o un **IAsyncXxx**\^.

En cualquier caso, el propio método usa la palabra clave `return` para devolver un objeto asincrónico que, cuando se completa, produce el valor que el autor de la llamada realmente desea (quizás un archivo, una matriz de bytes o un valor booleano).

> [!NOTE]
> Si un método asincrónico de C++/CX devuelve un **IAsyncXxx**\^, **TResult** se limita a ser un tipo de Windows Runtime. Un valor booleano, por ejemplo, es un tipo de Windows Runtime; sin embargo un tipo proyectado de C++/CX (por ejemplo, **Platform::Array<byte>** ^) no lo es.

### <a name="cwinrt-async"></a>C++/WinRT asincrónico

C++/WinRT integra corrutinas de C++ en el modelo de programación. Las corrutinas y la instrucción `co_await` ofrecen una manera natural de esperar un resultado de forma cooperativa.

Cada uno de estos tipos **IAsyncXxx** se proyecta en un tipo correspondiente en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. Nos referiremos a ellos como **winrt::IAsyncXxx**. El tipo devuelto de una corrutina de C++/WinRT es un **winrt::IAsyncXxx** o [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget). Y, en lugar de usar la palabra clave `return` para devolver un objeto asincrónico, una corrutina usa la palabra clave `co_return` para devolver el valor que el autor de la llamada realmente desea (quizás un archivo, una matriz de bytes o un valor booleano).

Si un método contiene al menos una instrucción `co_await` (o al menos un `co_return` o `co_yield`), el método es una corrutina por esa razón.

Para obtener más información, consulte [Operaciones simultáneas y asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Ejemplo de juego Direct3D (**Simple3DGameDX**)

Este tema contiene varios tutoriales que muestran cómo portar gradualmente el código asincrónico. Para que funcione como caso práctico, usaremos la versión de C++/CX del [ejemplo de juego Direct3D](/samples/microsoft/windows-universal-samples/simple3dgamedx/) (que se denomina **Simple3DGameDX**). Mostraremos algunos ejemplos de cómo puede tomar el código fuente de C++/CX original en ese proyecto y portar gradualmente su código asincrónico a C++/WinRT.

- Descargue el archivo ZIP del vínculo anterior y descomprímalo.
- Abra el proyecto de C++/CX (se encuentra en la carpeta denominada `cpp`) en Visual Studio.
- A continuación, deberá agregar compatibilidad con C++/WinRT al proyecto. Los pasos que debe seguir para ello se describen en [Adopción de un proyecto de C++/CX y adición de compatibilidad con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx#taking-a-ccx-project-and-adding-cwinrt-support). El paso sobre cómo agregar el archivo de encabezado `interop_helpers.h` a su proyecto es sumamente importante, ya que dependeremos de esas funciones auxiliares en este tema.
- Por último, agregue `#include <pplawait.h>` a `pch.h`. Esto proporciona compatibilidad de corrutinas para PPL (en la sección siguiente encontrará más información sobre esa compatibilidad).

Al compilar, obtendrá errores que indican que **byte** es ambiguo. Aquí tiene cómo resolver esto.

- Abra `BasicLoader.cpp` y comente `using namespace std;`.
- En el mismo archivo de código fuente, deberá calificar **shared_ptr** como **std::shared_ptr**. Puede hacerlo con una búsqueda y reemplazar en ese archivo.
- Califique **vector** como **std::vector** y **string** como **std::string**.

El proyecto se vuelve a compilar, tiene compatibilidad con C++/WinRT y contiene las funciones auxiliares de interoperabilidad **from_cx** y **to_cx**.

Ahora tiene el proyecto **Simple3DGameDX** listo para seguir con los tutoriales de codificación de este tema.

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>Información general sobre la portabilidad asincrónica de C++/CX a C++/WinRT

En pocas palabras, a medida que portemos, cambiaremos las cadenas de tareas de PPL a llamadas a `co_await`. Cambiaremos el valor devuelto de un método de una tarea de PPL a un objeto **IAsyncXxx** de C++/WinRT o a un **IAsyncXxx**\^. Y cambiaremos todo **IAsyncXxx**\^ a un **IAsyncXxx** de C++/WinRT.

Recordará que una corrutina es cualquier método que llame a `co_xxx`. Una corrutina de C++/WinRT usa `co_return` para devolver su valor. Gracias a la compatibilidad de las corrutinas con PPL (cortesía de `pplawait.h`), también puede usar `co_return` para devolver una tarea de PPL desde una corrutina. Además, también puede `co_await` ambas tareas y **IAsyncXxx**. Sin embargo, no puede usar `co_return` para devolver un **IAsyncXxx**\^. En la tabla siguiente se describe la compatibilidad con la interoperabilidad entre las distintas técnicas asincrónicas.

|Método|¿Puede aplicarle `co_await`?|¿Puede `co_return` desde él?|
|-|-|-|
|El método devuelve task\<void\>|Sí|Sí|
|El método devuelve task\<T\>|No|Sí|
|El método devuelve IAsyncXxx\^|Sí|No. Pero encapsula **create_async** en torno a una tarea que utiliza `co_return`.|
|El método devuelve winrt::IAsyncXxx|Sí|Sí|

Use esta tabla para ir directamente a una técnica de interoperabilidad que le interese.

|Técnica de interoperabilidad asincrónica|Sección de este tema|
|-|-|
|Usar `co_await` para esperar un método **task\<void\>** desde un método fire-and-forget.|[Esperar **task\<void\>** dentro de un método fire-and-forget](#await-taskvoid-within-a-fire-and-forget-method)|
|Usar `co_await` para esperar un método **task\<void\>** desde un método **task\<void\>** .|[Esperar **task\<void\>** en un método **task\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|Usar `co_await` para esperar un método **task\<void\>** desde un método **task\<T\>** .|[Esperar **task\<void\>** en un método **task\<T\>** ](#await-taskvoid-within-a-taskt-method)|
|Usar `co_await` para esperar un método **IAsyncXxx**^.|[Esperar un **IAsyncXxx**^ en un método **task**, dejando el resto del proyecto sin cambiar](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Usar `co_return` dentro de un método **task\<void\>** .|[Esperar **task\<void\>** en un método **task\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|Usar `co_return` dentro de un método **task\<T\>** .|[Esperar un **IAsyncXxx**^ en un método **task**, dejando el resto del proyecto sin cambiar](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Encapsular **create_async** en torno a una tarea que utiliza `co_return`.|[Encapsular **create_async** en torno a una tarea que utiliza `co_return`](#wrap-create_async-around-a-task-that-uses-co_return)|
|Portar **concurrency::wait**.|[Portar **concurrency::wait** a `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)|
|Devolver **winrt::IAsyncXxx** en lugar de **task\<void\>** .|[Portar un tipo devuelto de **task\<void\>** a **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|

Y este es un breve ejemplo de código que ilustra parte de la compatibilidad.

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that.
    return concurrency::create_async([=]() -> task<bool> {
        co_return true;
    });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

Incluso con estas excelentes opciones de interoperabilidad, la portabilidad gradual depende de la elección de cambios que podamos hacer de forma quirúrgica sin afectar al resto del sistema. Queremos evitar tirar de un hilo suelto arbitrario y que se enrede todo el proyecto. Para ello, tenemos que hacer las cosas en un orden determinado. Echemos un vistazo a algunos ejemplos de cómo realizar estos tipos de cambios de portabilidad relacionados con métodos asincrónicos.

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>Esperar un método **task\<void\>** , dejando el resto del proyecto sin cambiar

Un método que devuelve **task\<void\>** realiza el trabajo de forma asincrónica, pero no genera un valor en última instancia. Podemos `co_await` un método como este.

Por lo tanto, un buen lugar para empezar a portar el código asincrónico gradualmente es buscar los lugares en los que se llama a esos métodos. Estos lugares implicarán la creación o la devolución de una tarea; o una cadena de tareas donde no se pasa ningún valor de cada tarea a su continuación. En lugares como este, solo puede reemplazar el código asincrónico por instrucciones `co_await`, como veremos.

Busquemos algunos ejemplos. Abra el proyecto **Simple3DGameDX** (consulte [Ejemplo de juego Direct3D](#the-direct3d-game-sample-simple3dgamedx)).

> [!IMPORTANT]
> En los ejemplos siguientes, cuando vea las implementaciones de los métodos que se van a cambiar, tenga en cuenta que no es necesario cambiar los *autores de las llamadas* de los métodos que se están cambiando. Estos cambios son localizados y no se transmiten en cascada a lo largo del proyecto.

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>Esperar **task\<void\>** dentro de un método fire-and-forget

Comencemos con la espera de **task\<void\>** en métodos *fire-and-forget*, ya que este es el caso más sencillo. Se trata de métodos que funcionan de forma asincrónica, pero el autor de la llamada del método no espera a que se complete ese trabajo. Basta con llamar al método y olvidarlo, a pesar de que se completa de forma asincrónica.

Busque la raíz del gráfico de dependencias del proyecto para los métodos `void` que contienen **create_task** o cadenas de tareas donde solo se llama a los métodos **task\<void\>** .

En **Simple3DGameDX**, encontrará una coincidencia en la implementación del método **GameMain::Update**. Se encuentra en el archivo de código fuente `GameMain.cpp`.

#### <a name="gamemainupdate"></a>**GameMain::Update**

Este es un extracto de la versión de C++/CX del método, que muestra las dos partes que se completan de forma asincrónica.

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

Puede ver una llamada al método **Simple3DGame::LoadLevelAsync** (que devuelve una **task\<void\>** de PPL). Después de esto, hay una *continuación* que realiza algunos trabajos sincrónicos. **LoadLevelAsync** es asincrónico, pero no devuelve un valor. Por lo tanto, no se pasa ningún valor de la tarea a la continuación.

Podemos cambiar el código en estos dos lugares de la misma manera. El código se explica después del listado siguiente. Podríamos debatir sobre la manera segura de acceder al puntero *this* en una corrutina de miembro de clase. Pero aplazaremos eso hasta una sección posterior; por ahora, este código funciona.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

Como puede ver, dado que **LoadLevelAsync** devuelve una tarea, podemos aplicarle `co_await`. Y no necesitamos una continuación explícita&mdash;el código que sigue a `co_await` se ejecuta solo cuando se completa **LoadLevelAsync**.

Al introducir `co_await`, el método se convierte en una corrutina, por lo que no puede devolver `void`. Se trata de un método fire-and-forget, por lo que devolvemos [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget).

También tendrá que cambiar el tipo devuelto de **GameMain::Update** de `void` a **winrt::fire_and_forget** en su declaración en `GameMain.h`.

Puede hacer este cambio en su copia del proyecto, y el juego aún se compilará y se ejecutará igualmente. El código fuente sigue siendo fundamentalmente C++/CX, pero ahora usa los mismos patrones que C++/WinRT, por lo que nos hemos acercado un poco a poder portar el resto del código de forma mecánica.

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame** es otro método fire-and-forget; también llama a **LoadLevelAsync**. Así que podemos hacer el mismo cambio allí.

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

Las cosas se ponen un poco más interesante en **GameMain::OnDeviceRestored** debido a su anidamiento más profundo de código asincrónico, incluida una tarea no operativa. A continuación se muestra un esquema de las partes asincrónicas del método (donde el código sincrónico menos interesante se representa mediante puntos suspensivos).

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

En primer lugar, cambie el tipo devuelto de **GameMain::OnDeviceRestored** de `void` a **winrt::fire_and_forget** en `GameMain.h` y `.cpp`. También tendrá que abrir `DeviceResources.h` y hacer el mismo cambio en el tipo devuelto de **IDeviceNotify::OnDeviceRestored**.

Para portar el código asincrónico, quite todas las llamadas **create_task** y **then** y sus llaves, y simplifique el método en una serie plana de instrucciones.

Cambie todo `return` (que devuelve una tarea) a `co_await`. Quedará con un `return` que no devuelve nada, así que tan solo elimínelo. Cuando haya terminado, la tarea no operativa habrá desaparecido, y el esquema de las partes asincrónicas del método tendrá el siguiente aspecto. De nuevo, el código sincrónico menos interesante se representa mediante puntos suspensivos.

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

Como puede ver, es significativamente más sencillo y fácil de leer.

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

El constructor **GameMain::GameMain** realiza el trabajo de forma asincrónica, y ninguna parte del proyecto espera a que se complete ese trabajo. De nuevo, en este listado se describen las partes asincrónicas.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

Pero un constructor no puede devolver **winrt::fire_and_forget**, por lo que moveremos el código asincrónico a un nuevo método fire-and-forget **GameMain::ConstructInBackground**, aplanaremos el código en instrucciones `co_await` y llamaremos al nuevo método desde el constructor.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

Eso es todo para los métodos fire-and-forget&mdash;de hecho, todo el código asincrónico&mdash; en **GameMain** se convierte en corrutinas. Si lo cree conveniente, quizás pueda buscar métodos fire-and-forget en otras clases y realizar cambios similares.

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>La discusión aplazada sobre `co_await` y el puntero *this*

Cuando realizamos cambios en **GameMain::Update**, aplazamos el debate sobre el puntero *this*. Tengamos este debate aquí.

Esto se aplica a todos los métodos que hemos cambiado hasta ahora, y se aplica a *todas* las corrutinas, no solo a las fire-and-forget. Al introducir `co_await` en un método, se introduce un *punto de suspensión*. Y debido a eso, tenemos que tener cuidado con el puntero *this*, que por supuesto se usa *después* del punto de suspensión cada vez que se accede a un miembro de clase.

En resumen, la solución es llamar a [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function). Pero para obtener una descripción completa del problema y de la solución, consulte [Acceso de forma segura al puntero *this* en una corrutina de miembro de clase](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

Puede implementar **implements::get_strong** solo en una clase que deriva de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

#### <a name="derive-gamemain-from-winrtimplements"></a>Derivación de **GameMain** de **winrt::Implements**

El primer cambio que debemos hacer se encuentra en `GameMain.h`.

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

**GameMain** seguirá implementando **DX::IDeviceNotify**, pero lo cambiaremos para que derive de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

A continuación, en `App.cpp`, encontrará este método.

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

Pero ahora que **GameMain** deriva de **winrt::implements**, es necesario construirlo de manera diferente. En este caso, usaremos la plantilla de función [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). Para obtener más información, consulte [Crear instancias y devolver tipos de implementación e interfaces](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces).

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

Para cerrar el bucle en ese cambio, también tendremos que cambiar el tipo de *m_main*. En `App.h`, encontrará este código.

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

Cambie la declaración de *m_main* a lo siguiente.

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>Ahora podemos llamar a **implements::get_strong**

Para **GameMain::Update** y para cualquiera de los otros métodos a los que hemos agregado `co_await`, aquí se muestra cómo llamar a **get_strong** al principio de una corrutina para asegurarse de que una referencia segura sobreviva hasta que se complete la corrutina.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>Esperar **task\<void\>** en un método **task\<void\>**

El siguiente caso más sencillo es esperar **task\<void\>** dentro de un método que, a su vez, devuelve **task\<void\>** , porque podemos `co_await` una **task\<void\>** y podemos `co_return` de una.

Encontrará un ejemplo muy sencillo en la implementación del método **Simple3DGame::LoadLevelAsync**. Se encuentra en el archivo de código fuente `Simple3DGame.cpp`.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

Hay solo código sincrónico, seguido de la devolución de la tarea creada por **GameRenderer::LoadLevelResourcesAsync**.

En lugar de devolver esa tarea, se `co_await` y, a continuación, se `co_return` la `void`resultante.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

### <a name="await-taskvoid-within-a-taskt-method"></a>Esperar **task\<void\>** en un método **task\<T\>**

Aunque no se pueden encontrar ejemplos adecuados en **Simple3DGameDX**, podemos idear un ejemplo hipotético solo para mostrar el patrón.

En el ejemplo de código siguiente se muestra el `co_await` simple de una **task\<void\>** . A continuación, para satisfacer el tipo devuelto de **task\<T\>** , devolvemos de forma asincrónica un **StorageFile\^** al aplicar co_awaiting un método de Windows Runtime y `co_return` al archivo resultante.

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

El siguiente paso sería portar más del método a C++/WinRT como se muestra a continuación.

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

El miembro de datos *m_renderer* sigue estando en C++/CX en ese ejemplo.

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>Esperar un **IAsyncXxx**^ en un método **task**, dejando el resto del proyecto sin cambiar

Hemos visto cómo puede `co_await` **task\<void\>** . También puede `co_await` un método que devuelva **IAsyncXxx**, ya sea un método del proyecto o una API asincrónica de Windows (por ejemplo, [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync)).

Para ver un ejemplo de dónde podemos realizar este cambio de código, echemos un vistazo a **BasicReaderWriter::ReadDataAsync** (lo encontrará implementado en `BasicReaderWriter.cpp`). Esta es la versión original de C++/CX.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

No solo se puede `co_await` las API de Windows, sino que también `co_return` el valor que el método devuelve de forma asincrónica (en este caso, una matriz de bytes). Este primer paso muestra cómo realizar esos cambios; portaremos el código de C++/CX a C++/WinRT en la sección siguiente.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

De nuevo, no es necesario cambiar los *autores de las llamadas* de los métodos que estamos cambiando, ya que no hemos cambiado el tipo devuelto.

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>Portar **ReadDataAsync** (principalmente) a C++/WinRT, dejando el resto del proyecto sin cambios

Podemos ir un paso más allá y portar el método *casi por completo* a C++/WinRT sin necesidad de cambiar ninguna otra parte del proyecto.

La única dependencia que tiene este método en el resto del proyecto es el miembro de datos **BasicReaderWriter::m_location**, que es un **StorageFolder**^ de C++/CX. Para dejar ese miembro de datos sin modificar y dejar el tipo de parámetro y el tipo devuelto sin cambiar, solo necesitamos realizar un par de conversiones&mdash;una al principio del método y otra al final. Para ello, se puede usar las funciones auxiliares de interoperabilidad **from_cx** y **to_cx** .

Aquí se muestra cómo luce **BasicReaderWriter::ReadDataAsync** después de portar su implementación principalmente a C++/WinRT. Este es un buen ejemplo de *portabilidad gradual*. Y este método se encuentra en la fase en la que podemos dejar de pensar en él como *un método de C++/CX que usa algunas técnicas de C++/WinRT* y verlo como *un método de C++/WinRT que interopera con C++/CX*.

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> En **ReadDataAsync**, construimos y devolvimos una nueva matriz de C++/CX. Y, por supuesto, lo hicimos para satisfacer el tipo devuelto del método (de modo que no tuvimos que cambiar el resto del proyecto).
>
> Puede que se encuentre con otros ejemplos de su propio proyecto, donde, después de portarlo, llega al final del método y todo lo que tenga sea un objeto de C++/WinRT. Para aplicarle `co_return`, simplemente llame a **to_cx** para convertirlo. He aquí un ejemplo hipotético.

```cppcx
task<Windows::Storage::StorageFile^> RetrieveFileAsync()
{
    winrt::Windows::Storage::StorageFile storageFile = co_await ...
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>Encapsular **create_async** en torno a una tarea que utiliza `co_return`

No se puede `co_return` un **IAsyncXxx**\^ directamente, pero puede conseguir algo similar. Si tiene una tarea que usa `co_return`, puede encapsular [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) en torno a ella.

Este es un ejemplo hipotético, ya que no hay un ejemplo que podamos obtener de **Simple3DGameDX**.

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ Simple3DGame::ReturnBooleanAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await BooleanMemberFunctionAsync();
            co_return result;
        });
}
```

Como puede ver, puede obtener el valor devuelto por cualquier método que pueda `co_await`.

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>Portar **concurrency::wait** a `co_await winrt::resume_after`

Hay un par de lugares donde **Simple3DGameDX** usa [**concurrency::wait**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait) para pausar el subproceso durante un breve período de tiempo. Este es un ejemplo.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

La versión de C++/WinRT de **concurrency::wait** es el struct **winrt::resume_after**. Podemos `co_await` ese struct dentro de una tarea de PPL. Aquí tienes un ejemplo de código.

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

Observe los otros dos cambios que tuvimos que hacer. Hemos cambiado el tipo de **GameConstants::InitialLoadingDelay** a **std::chrono::duration**, y hemos hecho explícito el tipo devuelto de la función lambda, ya que el compilador ya no puede deducirlo.

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>Portar un tipo devuelto de **task\<void\>** a **winrt::IAsyncXxx**

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

En esta fase de nuestro trabajo con **Simple3DGameDX**, todos los lugares del proyecto que llaman a **Simple3DGame::LoadLevelAsync** usan `co_await` para llamarlo.

Esto significa que se puede cambiar el tipo devuelto de ese método de **task\<void\>** a **winrt::Windows::Foundation::IAsyncAction**.

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

Ahora debería ser bastante mecánico portar el resto del método, y sus dependencias, a C++/WinRT.

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

Esta es la versión original de C++/CX de **GameRenderer::LoadLevelResourcesAsync**.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

**Simple3DGame::LoadLevelAsync** es el único lugar del proyecto que llama a **GameRenderer::LoadLevelResourcesAsync** y ya usa `co_await` para llamarlo.

Por lo tanto, ya no es necesario que **GameRenderer::LoadLevelResourcesAsync** devuelva una tarea&mdash;puede devolver **winrt::Windows::Foundation::IAsyncAction** en su lugar. Y la propia implementación es lo suficientemente sencilla para portar a C++/WinRT por completo. Esto implica realizar el mismo cambio realizado en [Portar **concurrency::wait** a `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after). Y no hay dependencias significativas en el resto del proyecto por las que preocuparse.

Así es como se ve el método después de portarlo por completo a C++/WinRT.

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="basicreaderwriterreaddataasync"></a>**BasicReaderWriter::ReadDataAsync**

Para finalizar este tutorial, vamos a portar por completo **BasicReaderWriter::ReadDataAsync** a C++/WinRT. La última vez que lo examinamos (en [Portar **ReadDataAsync** (principalmente) a C++/WinRT, dejando el resto del proyecto sin cambios](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)), se portó principalmente a C++/WinRT. Pero aún devolvía una tarea de **Platform::Array\<byte\>** ^.

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

En lugar de devolver una tarea, vamos a cambiar eso para devolver de forma asincrónica un objeto [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer) de C++/WinRT, aunque eso signifique cambiar el código en los sitios de llamada.

Este es el aspecto que tendrá el método después de migrar su implementación, su parámetro y el miembro de datos *m_location* para utilizar la sintaxis y los objetos de C++/WinRT.

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

Como puede ver, **BasicReaderWriter::ReadDataAsync** mismo es mucho más sencillo, ya que hemos factorizado en su propio método la lógica sincrónica que recupera bytes del búfer.

Pero ahora tenemos que portar los sitios de llamadas desde este tipo de estructura en C++/CX.

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

A este patrón en C++/WinRT.

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>API importantes

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction),
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1) y
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Temas relacionados

* [Migrar a C++/WinRT desde C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [Interoperabilidad entre C++/WinRT y C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)
* [Operaciones simultáneas y asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [Referencias fuertes y débiles de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references)
* [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)

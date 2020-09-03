---
description: Este es un tema avanzado relacionado con la migración gradual de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Muestra cómo las tareas y las corrutinas de la Biblioteca de patrones paralelos(PPL) pueden existir en paralelo en el mismo proyecto.
title: Asincronía e interoperabilidad entre C++/WinRT y C++/CX
ms.date: 08/06/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, proyección, portar, migrar, interoperabilidad, C++/CX, PPL, tarea, corrutina
ms.localizationpriority: medium
ms.openlocfilehash: 1beff7fe5595a2601d56d65b52ca51eacedee47f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157399"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>Asincronía e interoperabilidad entre C++/WinRT y C++/CX

> [!TIP]
> Aunque se recomienda leer este tema desde el principio, puede ir directamente a un resumen de las técnicas de interoperabilidad en la sección [Información general sobre la portabilidad asincrónica de C++/CX a C++/WinRT](#overview-of-porting-ccx-async-to-cwinrt).

Este es un tema avanzado relacionado con la portabilidad gradual de [C++/WinRT](./intro-to-using-cpp-with-winrt.md) a [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx). En este tema se retoma donde se dejó en [Interoperabilidad entre C++/WinRT y C++/CX](./interop-winrt-cx.md).

Si el tamaño o la complejidad del código base hacen necesario portar el proyecto gradualmente, necesitará un proceso de portabilidad en el que, en un momento, exista código de C++/CX y C++/WinRT en paralelo en el mismo proyecto. Si tiene código asincrónico, es posible que necesite que existan cadenas de tareas y corrutinas de la Biblioteca de patrones paralelos (PPL) en paralelo en el proyecto a medida que porta gradualmente el código fuente. Este tema se centra en las técnicas de interoperabilidad entre código asincrónico de C++/CX y código asincrónico de C++/WinRT. Puede utilizar estas técnicas de manera individual o conjunta. Las técnicas permiten realizar cambios locales y controlados de forma gradual a lo largo de la ruta de acceso para portar todo el proyecto, sin tener que cada cambio en cascada en el proyecto de manera descontrolada.

Antes de leer este tema, se recomienda leer [Interoperabilidad entre C++/WinRT y C++/CX](./interop-winrt-cx.md). En este tema se muestra cómo preparar el proyecto para la portabilidad gradual. Además, presenta dos funciones auxiliares que puede usarse para convertir un objeto de C++/CX y en un objeto de C++/WinRT (y viceversa). Este tema sobre la asincronía se basa en esa información y utiliza dichas funciones auxiliares.

> [!NOTE]
> La portabilidad gradual de C++/CX a C++/WinRT presenta algunas limitaciones. Si tiene un proyecto de [componentes de Windows Runtime](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md), no es posible la portabilidad gradual y tendrá que portar el proyecto en un solo paso. Y para un proyecto XAML, en un momento dado, los tipos de página XAML deben estar, *o bien* todos en C++/WinRT, *o bien* todos en C++/CX. Para obtener más información, consulte el tema [Migrar a C++/WinRT desde C++/CX](./move-to-winrt-from-cx.md).

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>Motivo por el que se dedica un tema entero a la interoperabilidad de código asincrónico

La portabilidad de C++/CX a C++/WinRT suele ser sencilla, con la única excepción de mover desde las tareas de la [Biblioteca de patrones paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) a corrutinas. Los modelos son diferentes. No hay una asignación natural de uno a uno de las tareas de PPL en corrutinas, y no hay ninguna manera sencilla (que funcione en todos los casos) de portar el código de forma mecánica.

La buena noticia es que la conversión de tareas a corrutinas genera simplificaciones significativas. Y los equipos de desarrollo informan de manera rutinaria que, una vez que superan el obstáculo de portabilidad de su código asincrónico, el resto del trabajo de portabilidad es principalmente mecánico.

A menudo, un algoritmo se escribía originalmente para adaptarse a las API sincrónicas. Y luego se traducía en tareas y continuaciones explícitas&mdash;el resultado solía provocar una ofuscación involuntaria de la lógica subyacente. Por ejemplo, los bucles se convierten en recursividad; las ramas if-else se convierten en un árbol anidado (una cadena) de tareas; las variables compartidas se convierten en **shared_ptr**. Para deconstruir la estructura a menudo antinatural del código fuente de PPL, se recomienda que primero retroceda y comprenda la intención del código original (es decir, detectar la versión sincrónica original). Y, a continuación, inserte `co_await` (espera cooperativa) en los lugares adecuados.

Por ese motivo, si tiene una versión de C# (en lugar de C++/CX) del código asincrónico a partir del cual empezar a portar, eso puede facilitar la tarea y ofrecer una portabilidad más ordenada. El código de C# usa `await`. Por lo tanto, el código de C# ya sigue una filosofía de comenzar con una versión sincrónica y, luego, insertar `await` en los lugares adecuados.

Si *no* tiene una versión de C# del proyecto, puede usar las técnicas descritas en este tema. Y una vez que haya portado a C++/WinRT, la estructura del código asincrónico será más fácil de portar a C#, si lo desea.

## <a name="some-background-in-asynchronous-programming"></a>Algo de contexto sobre la programación asincrónica

Para tener un marco común de referencia para la terminología y los conceptos de programación asincrónica, vamos a enmarcar brevemente la programación asincrónica para Windows Runtime en general, y también cómo las proyecciones de los dos lenguajes de C++, cada una a su propio modo, se suman a esta.

El proyecto tiene métodos que funcionan de forma asincrónica, y existen dos tipos principales.

- Es común querer esperar a que un trabajo asincrónico se complete antes de hacer otra cosa. Un método que devuelve un objeto de operación asincrónica es aquel en el que se puede esperar.
- Pero a veces no desea o no necesita esperar a que finalice el trabajo realizado de forma asincrónica. En ese caso, es más eficaz que el método asincrónico *no* devuelva un objeto de operación asincrónica. Un método asincrónico como este &mdash;en el que no espera&mdash; se conoce como método *fire-and-forget* (desencadenar y olvidar).

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Objetos asincrónicos de Windows Runtime (**IAsyncXxx**)

El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objeto de la operación asincrónica.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1) y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).

En este tema, cuando usamos la forma abreviada de **IAsyncXxx**, hacemos referencia a estos tipos colectivamente, o bien hablamos de uno de los cuatro tipos sin necesidad de especificar cuál.

### <a name="ccx-async"></a>C++/CX asincrónico

El código de C++/CX asincrónico usa tareas de la [Biblioteca de patrones paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl). Una tarea de PPL se representa mediante la clase [**concurrency::task**](/cpp/parallel/concrt/reference/task-class).

Normalmente, un método asincrónico de C++/CX encadena las tareas de PPL juntas mediante funciones lambda con [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) y [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then). Cada función lambda devuelve una tarea que, cuando se completa, genera un valor que luego se pasa a la expresión lambda de *continuación* de la tarea.

Como alternativa, en lugar de llamar a **create_task** para crear una tarea, un método asincrónico de C++/CX puede llamar a [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) para crear un **IAsyncXxx**\^.

Por lo tanto, el tipo devuelto de un método asincrónico de C++/CX puede ser una tarea de PPL o un **IAsyncXxx**\^.

En cualquier caso, el propio método usa la palabra clave `return` para devolver un objeto asincrónico que, cuando se completa, produce el valor que el autor de la llamada realmente desea (quizás un archivo, una matriz de bytes o un valor booleano).

> [!NOTE]
> Si un método asincrónico de C++/CX devuelve un **IAsyncXxx**\^, **TResult** (de existir) se limita a ser un tipo de Windows Runtime. Un valor booleano, por ejemplo, es un tipo de Windows Runtime; sin embargo un tipo proyectado de C++/CX (por ejemplo, **Platform::Array<byte>** ^) no lo es.

### <a name="cwinrt-async"></a>C++/WinRT asincrónico

C++/WinRT integra corrutinas de C++ en el modelo de programación. Las corrutinas y la instrucción `co_await` ofrecen una manera natural de esperar un resultado de forma cooperativa.

Cada uno de estos tipos **IAsyncXxx** se proyecta en un tipo correspondiente en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. Vamos a referirnos a ellos como **winrt::IAsyncXxx** (en comparación con **IAsyncXxx**\^ de C++/CX).

El tipo devuelto de una corrutina de C++/WinRT es un **winrt::IAsyncXxx** o [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget). Y, en lugar de usar la palabra clave `return` para devolver un objeto asincrónico, una corrutina usa la palabra clave `co_return` para devolver de manera cooperativa el valor que el autor de la llamada realmente desea (quizás un archivo, una matriz de bytes o un valor booleano).

Si un método contiene al menos una instrucción `co_await` (o al menos un `co_return` o `co_yield`), el método es una corrutina por esa razón.

Para obtener más información y ejemplos de código, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](./concurrency.md).

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Ejemplo de juego Direct3D (**Simple3DGameDX**)

Este tema contiene varios tutoriales de varias técnicas de programación específicas que muestran cómo portar gradualmente el código asincrónico. Para que funcione como caso práctico, usaremos la versión de C++/CX del [ejemplo de juego Direct3D](/samples/microsoft/windows-universal-samples/simple3dgamedx/) (que se denomina **Simple3DGameDX**). Mostraremos algunos ejemplos de cómo puede tomar el código fuente de C++/CX original en ese proyecto y portar gradualmente su código asincrónico a C++/WinRT.

- Descargue el archivo ZIP del vínculo anterior y descomprímalo.
- Abra el proyecto de C++/CX (se encuentra en la carpeta denominada `cpp`) en Visual Studio.
- A continuación, deberá agregar compatibilidad con C++/WinRT al proyecto. Los pasos que debe seguir para ello se describen en [Adopción de un proyecto de C++/CX y adición de compatibilidad con C++/WinRT](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support). En esa sección, el paso sobre cómo agregar el archivo de encabezado `interop_helpers.h` a su proyecto es sumamente importante, ya que dependeremos de esas funciones auxiliares en este tema.
- Por último, agregue `#include <pplawait.h>` a `pch.h`. Esto proporciona compatibilidad de corrutinas para PPL (en la sección siguiente podrá encontrar más información sobre esa compatibilidad).

No compile aún, de lo contrario, obtendrá errores que indican que **byte** es ambiguo. Aquí tiene cómo resolver esto.

- Abra `BasicLoader.cpp` y comente `using namespace std;`.
- En el mismo archivo de código fuente, deberá calificar **shared_ptr** como **std::shared_ptr**. Puede hacerlo con una búsqueda y reemplazar en ese archivo.
- Luego, califique **vector** como **std::vector** y **string** como **std::string**.

El proyecto se vuelve a compilar, tiene compatibilidad con C++/WinRT y contiene las funciones auxiliares de interoperabilidad **from_cx** y **to_cx**.

Ahora tiene el proyecto **Simple3DGameDX** listo para seguir con los tutoriales de codificación de este tema.

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>Información general sobre la portabilidad asincrónica de C++/CX a C++/WinRT

En pocas palabras, a medida que portemos, cambiaremos las cadenas de tareas de PPL a llamadas a `co_await`. Cambiaremos el valor devuelto de un método de una tarea de PPL a un objeto **winrt::IAsyncXxx** de C++/WinRT. Y también cambiaremos todo **IAsyncXxx**\^ a un **winrt::IAsyncXxx** de C++/WinRT.

Recordará que una corrutina es cualquier método que llame a `co_xxx`. Una corrutina de C++/WinRT usa `co_return` para devolver su valor de manera cooperativa. Gracias a la compatibilidad de las corrutinas con PPL (cortesía de `pplawait.h`), también puede usar `co_return` para devolver una tarea de PPL desde una corrutina. Además, también puede `co_await` ambas tareas y **IAsyncXxx**. Sin embargo, no puede usar `co_return` para devolver un **IAsyncXxx**\^. En la tabla siguiente se describe la compatibilidad con la interoperabilidad entre las distintas técnicas asincrónicas con `pplawait.h` en la imagen.

|Método|¿Puede aplicarle `co_await`?|¿Puede `co_return` de él?|
|-|-|-|
|El método devuelve **task\<void\>**|Sí|Sí|
|El método devuelve **task\<T\>**|No|Sí|
|El método devuelve **IAsyncXxx**^|Sí|No. Pero encapsula **create_async** en torno a una tarea que utiliza `co_return`.|
|El método devuelve **winrt::IAsyncXxx**|Sí|Sí|

Use la siguiente tabla para ir directamente a la sección de este tema donde se describe una técnica de interoperabilidad de interés o simplemente continúe leyendo desde aquí.

|Técnica de interoperabilidad asincrónica|Sección de este tema|
|-|-|
|Usar `co_await` para esperar un método **task\<void\>** desde un método fire-and-forget o desde un constructor.|[Esperar **task\<void\>** dentro de un método fire-and-forget](#await-taskvoid-within-a-fire-and-forget-method)|
|Usar `co_await` para esperar un método **task\<void\>** desde un método **task\<void\>** .|[Esperar **task\<void\>** en un método **task\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|Usar `co_await` para esperar un método **task\<void\>** desde un método **task\<T\>** .|[Esperar **task\<void\>** en un método **task\<T\>** ](#await-taskvoid-within-a-taskt-method)|
|Usar `co_await` para esperar un método **IAsyncXxx**^.|[Esperar un **IAsyncXxx**^ en un método **task**, dejando el resto del proyecto sin cambiar](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Usar `co_return` dentro de un método **task\<void\>** .|[Esperar **task\<void\>** en un método **task\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|Usar `co_return` dentro de un método **task\<T\>** .|[Esperar un **IAsyncXxx**^ en un método **task**, dejando el resto del proyecto sin cambiar](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Encapsular **create_async** en torno a una tarea que utiliza `co_return`.|[Encapsular **create_async** en torno a una tarea que utiliza `co_return`](#wrap-create_async-around-a-task-that-uses-co_return)|
|Portar **concurrency::wait**.|[Portar **concurrency::wait** a `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)|
|Devolver **winrt::IAsyncXxx** en lugar de **task\<void\>** .|[Portar un tipo devuelto de **task\<void\>** a **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|
|Convertir **winrt::IAsyncXxx\<T\>** (T es primitivo) a **task\<T\>** .|[Convertir **winrt::IAsyncXxx\<T\>** (T es primitivo) en **task\<T\>** ](#convert-a-winrtiasyncxxxt-t-is-primitive-to-a-taskt)|
|Convertir **winrt::IAsyncXxx\<T\>** (T es un tipo de Windows Runtime) a **task\<T^\>** .|[Convertir **winrt::IAsyncXxx\<T\>** (T es un tipo de Windows Runtime) a **task\<T^\>** ](#convert-a-winrtiasyncxxxt-t-is-a-windows-runtime-type-to-a-taskt)|

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
    // co_return true; // Error! Can't do that. But you can do
    // the following.
    return concurrency::create_async([=]() -> concurrency::task<bool> {
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
    co_return co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

> [!IMPORTANT]
> Incluso con estas excelentes opciones de interoperabilidad, la portabilidad gradual depende de la elección de cambios que podamos hacer de forma quirúrgica sin afectar al resto del proyecto. Queremos evitar tirar de un hilo suelto arbitrario y, en definitiva, que se enrede la estructura de todo el proyecto. Para ello, tenemos que hacer las cosas en un orden determinado. A continuación, echemos un vistazo más de cerca a algunos ejemplos de cómo realizar estos tipos de cambios de portabilidad e interoperabilidad relacionados con métodos asincrónicos.

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>Esperar un método **task\<void\>** , dejando el resto del proyecto sin cambiar

Un método que devuelve **task\<void\>** realiza el trabajo de forma asincrónica, y devuelve un objeto de operación asincrónica, pero no genera un valor en última instancia. Podemos `co_await` un método como este.

Por lo tanto, un buen lugar para empezar a portar el código asincrónico gradualmente es buscar los lugares en los que se llama a dichos métodos. Estos lugares implicarán la creación o devolución de una tarea. También pueden implicar el tipo de cadena de tareas donde no se pasa ningún valor de cada tarea a su continuación. En lugares como este, solo puede reemplazar el código asincrónico por instrucciones `co_await`, como veremos.

> [!NOTE]
> A medida que avance por este tema, verá la ventaja de esta estrategia. Una vez que se llama exclusivamente a un método **task\<void\>** en particular a través de `co_await`, está en libertad de portar ese método a C++/WinRT y hacer que devuelva **winrt::IAsyncXxx**.

Busquemos algunos ejemplos. Abra el proyecto **Simple3DGameDX** (consulte [Ejemplo de juego Direct3D](#the-direct3d-game-sample-simple3dgamedx)).

> [!IMPORTANT]
> En los ejemplos siguientes, cuando vea las implementaciones de los métodos que se van a cambiar, tenga en cuenta que no es necesario cambiar los *autores de las llamadas* de los métodos que se están cambiando. Estos cambios son localizados y no se transmiten en cascada a lo largo del proyecto.

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>Esperar **task\<void\>** dentro de un método fire-and-forget

Comencemos con la espera de **task\<void\>** en métodos *fire-and-forget*, ya que este es el caso más sencillo. Se trata de métodos que funcionan de forma asincrónica, pero el autor de la llamada del método no espera a que se complete ese trabajo. Basta con llamar al método y olvidarlo, a pesar de que se completa de forma asincrónica.

Busque la raíz del gráfico de dependencias del proyecto para los métodos `void` que contienen **create_task** o cadenas de tareas donde solo se llama a los métodos **task\<void\>** .

En **Simple3DGameDX**, encontrará un código como ese en la implementación del método **GameMain::Update**. Se encuentra en el archivo de código fuente `GameMain.cpp`.

#### <a name="gamemainupdate"></a>**GameMain::Update**

Este es un extracto de la versión de C++/CX del método, que muestra las dos partes del método que se completan de forma asincrónica.

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

Podemos realizar el mismo tipo de cambio en el código en estos dos lugares. El código se explica después del listado siguiente. Podríamos debatir sobre la manera segura de acceder al puntero *this* en una corrutina de miembro de clase. Pero vamos a aplazar eso para una sección posterior ([La discusión aplazada sobre `co_await` y el puntero *this*](#the-deferred-discussion-about-co_await-and-the-this-pointer))&mdash;por ahora, este código funciona.

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

Al introducir `co_await`, el método se convierte en una corrutina, por lo que no podemos dejarlo que devuelva `void`. Se trata de un método fire-and-forget, por lo que lo cambiamos para devolver [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget).

También tendrá que editar `GameMain.h`. Cambie el tipo devuelto de **GameMain::Update** de `void` a **winrt::fire_and_forget** también en su declaración.

Puede hacer este cambio en su copia del proyecto, y el juego aún se compilará y se ejecutará igualmente. El código fuente sigue siendo fundamentalmente C++/CX, pero ahora usa los mismos patrones que C++/WinRT, por lo que nos hemos acercado un poco a poder portar el resto del código de forma mecánica.

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame** es otro método fire-and-forget; también llama a **LoadLevelAsync**. Por lo tanto, puede hacer el mismo cambio de código allí si desea practicar.

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

Cambie todo `return` (que devuelve una tarea) a `co_await`. Quedará con un `return` que no devuelve nada, así que tan solo elimínelo. Cuando haya terminado, la tarea no operativa habrá desaparecido, y el esquema de las partes asincrónicas del método tendrá el siguiente aspecto. De nuevo, el código sincrónico menos interesante se ha elidido.

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

Como puede ver, esta forma de estructura asincrónica es significativamente más sencilla y más fácil de leer.

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

Pero un constructor no puede devolver **winrt::fire_and_forget**, por lo que moveremos el código asincrónico a un nuevo método fire-and-forget **GameMain::ConstructInBackground**, aplanaremos el código en instrucciones `co_await` y llamaremos al nuevo método desde el constructor. Este es el resultado.

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

Ahora todos los métodos fire-and-forget&mdash;de hecho, todo el código asincrónico&mdash; en **GameMain** se han convertido en corrutinas. Si lo cree conveniente, quizás pueda buscar métodos fire-and-forget en otras clases y realizar cambios similares.

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>La discusión aplazada sobre `co_await` y el puntero *this*

Cuando realizamos cambios en **GameMain::Update**, aplazamos el debate sobre el puntero *this*. Tengamos este debate aquí.

Esto se aplica a todos los métodos que hemos cambiado hasta ahora, y se aplica a *todas* las corrutinas, no solo a las fire-and-forget. Al introducir `co_await` en un método, se introduce un *punto de suspensión*. Y debido a eso, tenemos que tener cuidado con el puntero *this*, que por supuesto se usa *después* del punto de suspensión cada vez que se accede a un miembro de clase.

En resumen, la solución es llamar a [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function). Pero para obtener una descripción completa del problema y de la solución, consulte [Acceso de forma segura al puntero *this* en una corrutina de miembro de clase](./weak-references.md#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

Puede llamar a **implements::get_strong** solo en una clase que deriva de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

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

Pero ahora que **GameMain** deriva de **winrt::implements**, es necesario construirlo de manera diferente. En este caso, usaremos la plantilla de función [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). Para obtener más información, consulte [Crear instancias y devolver tipos de implementación e interfaces](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces).

Reemplace esa línea de código con esta.

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

Cambie esa declaración de *m_main* a la siguiente.

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

El siguiente caso más sencillo es esperar **task\<void\>** dentro de un método que, a su vez, devuelve **task\<void\>** . Esto se debe a que se puede `co_await` **task\<void\>** y se puede `co_return` de una.

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

Ese no parece un cambio profundo. Pero ahora que llamamos a **GameRenderer::LoadLevelResourcesAsync** a través de `co_await`, estamos en libertad de portarla para que devuelva **winrt::IAsyncXxx** en lugar de una tarea. Lo haremos más adelante en la sección [Portar un tipo devuelto de **task\<void\>** a **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx).

### <a name="await-taskvoid-within-a-taskt-method"></a>Esperar **task\<void\>** en un método **task\<T\>**

Aunque no se pueden encontrar ejemplos adecuados en **Simple3DGameDX**, podemos idear un ejemplo hipotético solo para mostrar el patrón.

En la primera línea del ejemplo de código siguiente se muestra el `co_await` simple de una **task\<void\>** . A continuación, para satisfacer el tipo devuelto de **task\<T\>** , es necesario devolver de forma asincrónica un **StorageFile\^** . Para ello, aplicamos `co_await` a una API de Windows Runtime y `co_return` al archivo resultante.

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

Incluso podríamos portar más del método a C++/WinRT como se muestra a continuación.

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

Hemos visto cómo puede `co_await` **task\<void\>** . También puede `co_await` un método que devuelva **IAsyncXxx**, ya sea un método del proyecto o una API asincrónica de Windows (por ejemplo, [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync), que esperamos de manera cooperativa en la sección anterior).

Para ver un ejemplo de dónde podemos realizar este tipo de cambio de código, echemos un vistazo a **BasicReaderWriter::ReadDataAsync** (lo encontrará implementado en `BasicReaderWriter.cpp`).

Esta es la versión original de C++/CX.

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

La siguiente lista de código muestra que se puede aplicar `co_await` a las API de Windows que devuelven **IAsyncXxx**^. No solo eso, también se puede aplicar `co_return` al valor que **BasicReaderWriter::ReadDataAsync** devuelve de forma asincrónica (en este caso, una matriz de bytes). Este primer paso muestra cómo realizar esos cambios; portaremos el código de C++/CX a C++/WinRT en la sección siguiente.

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
> En **ReadDataAsync** anterior, construimos y devolvimos una nueva matriz de C++/CX. Y, por supuesto, lo hicimos para satisfacer el tipo devuelto del método (de modo que no tuvimos que cambiar el resto del proyecto).
>
> Puede que se encuentre con otros ejemplos de su propio proyecto, donde, después de portarlo, llega al final del método y todo lo que tenga sea un objeto de C++/WinRT. Para aplicarle `co_return`, simplemente llame a **to_cx** para convertirlo. Hay más información sobre eso y un ejemplo en la siguiente sección.

## <a name="convert-a-winrtiasyncxxxt-to-a-taskt"></a>Convertir **winrt::IAsyncXxx\<T\>** en **task\<T\>**

En esta sección se trata la situación en la que ha portado un método asincrónico a C++/WinRT (de modo que devuelva **winrt::IAsyncXxx\<T\>** ), pero sigue teniendo el código de C++/CX que llama a ese método como si todavía devolviera una tarea.

- Un caso es donde **T** es primitivo, lo que no requiere conversión.
- El otro caso es donde **T** es un tipo de Windows Runtime, en cuyo caso tendrá que convertirlo en **T**^.

### <a name="convert-a-winrtiasyncxxxt-t-is-primitive-to-a-taskt"></a>Convertir **winrt::IAsyncXxx\<T\>** (T es primitivo) en **task\<T\>**

El patrón de esta sección se aplica cuando se devuelve de forma asincrónica un valor primitivo (usaremos un valor booleano para ilustrarlo). Considere un ejemplo donde un método que ya ha portado a C++/WinRT tiene esta firma.

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<bool>
MyClass::GetBoolMemberFunctionAsync()
{
    bool value = ...
    co_return value;
}
```

Puede convertir una llamada a ese método en una tarea como esta.

```cppcx
task<bool> MyClass::RetrieveBoolTask()
{
    co_return co_await GetBoolMemberFunctionAsync();
}
```

O bien así.

```cppcx
task<bool> MyClass::RetrieveBoolTask()
{
    return concurrency::create_task(
        [this]() -> concurrency::task<bool> {
            auto result = co_await GetBoolMemberFunctionAsync();
            co_return result;
        });
}
```

Observe que tipo devuelto de **task** de la función lambda es explícito, ya que el compilador no puede deducirlo.

También se puede llamar al método desde una cadena de tareas arbitraria como esta. De nuevo, con un tipo devuelto de lambda explícito.

```cppcx
...
.then([this]() -> concurrency::task<bool> {
    co_return co_await GetBoolMemberFunctionAsync();
}).then([this](bool result) {
    ...
});
...
```

### <a name="convert-a-winrtiasyncxxxt-t-is-a-windows-runtime-type-to-a-taskt"></a>Convertir **winrt::IAsyncXxx\<T\>** (T es un tipo de Windows Runtime) a **task\<T^\>**

El patrón de esta sección se aplica cuando se devuelve de forma asincrónica un valor de Windows Runtime (usaremos un valor **StorageFile** para ilustrarlo). Considere un ejemplo donde un método que ya ha portado a C++/WinRT tiene esta firma.

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
MyClass::GetStorageFileMemberFunctionAsync()
{
    co_return co_await winrt::Windows::Storage::StorageFile::GetFileFromPathAsync
    (L"MyFile.txt");
}
```

En la siguiente lista se muestra cómo convertir una llamada a ese método en una tarea. Tenga en cuenta que es necesario llamar a la función auxiliar de interoperabilidad **to_cx** para convertir el objeto de C++/WinRT devuelto en un objeto de identificador de C++/CX (también conocido como *hat*).

```cppcx
task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    winrt::Windows::Storage::StorageFile storageFile =
        co_await GetStorageFileMemberFunctionAsync();
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

Esta es una versión más concisa de esto.

```cppcx
task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    co_return to_cx<Windows::Storage::StorageFile>(GetStorageFileMemberFunctionAsync());
}
```

Incluso puede optar por encapsular ese patrón en una plantilla de función reutilizable, y aplicar `return` tal como lo haría normalmente para devolver una tarea.

```cppcx
template<typename ResultTypeCX, typename Awaitable>
concurrency::task<ResultTypeCX^> to_task(Awaitable awaitable)
{
    co_return to_cx<ResultTypeCX>(co_await awaitable);
}

task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    return to_task<Windows::Storage::StorageFile>(GetStorageFileMemberFunctionAsync());
}
```

Si le gusta esta idea, es posible que desee agregar **to_task** a `interop_helpers.h`.

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>Encapsular **create_async** en torno a una tarea que utiliza `co_return`

No se puede `co_return` un **IAsyncXxx**\^ directamente, pero puede conseguir algo similar. Si tiene una tarea que devuelve un valor de forma cooperativa, puede encapsularla dentro de una llamada a [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async).

Este es un ejemplo hipotético, ya que no hay un ejemplo que podamos obtener de **Simple3DGameDX**.

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ MyClass::RetrieveBoolAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await GetBoolMemberFunctionAsync();
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

Esto significa que simplemente se puede cambiar el tipo devuelto de ese método de **task\<void\>** a **winrt::Windows::Foundation::IAsyncAction** (y dejar el resto invariable).

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

Ahora debería ser bastante mecánico portar el resto del método, y sus dependencias (como *m_level*, etc.), a C++/WinRT.

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

### <a name="the-goalmdashfully-port-a-method-to-cwinrt"></a>El objetivo&mdash;portar por completo un método a C++/WinRT

Vamos a concluir este tutorial con un ejemplo del objetivo final; para ello, vamos a portar por completo el método **BasicReaderWriter::ReadDataAsync** a C++/WinRT.

La última vez que examinamos este método (en la sección [Portar **ReadDataAsync** (principalmente) a C++/WinRT, dejando el resto del proyecto sin cambios](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)), se portó *principalmente* a C++/WinRT. Pero aún devolvía una tarea de **Platform::Array\<byte\>** ^.

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

En lugar de devolver una tarea, lo cambiaremos para que devuelva **IAsyncOperation**. Y en lugar de devolver una matriz de bytes a través de ese **IAsyncOperation**, se devolverá un objeto [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer) de C++/WinRT. Esto también requerirá un pequeño cambio en el código en los sitios de llamada, como veremos.

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

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Temas relacionados

* [Migrar a C++/WinRT desde C++/CX](./move-to-winrt-from-cx.md)
* [Interoperabilidad entre C++/WinRT y C++/CX](./interop-winrt-cx.md)
* [Operaciones simultáneas y asincrónicas con C++/WinRT](./concurrency.md)
* [Referencias fuertes y débiles de C++/WinRT](./weak-references.md)
* [Crear API con C++/WinRT](./author-apis.md)
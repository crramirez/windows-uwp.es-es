---
title: Crear y registrar una tarea en segundo plano fuera del proceso
description: Crea una clase de tarea en segundo plano fuera del proceso y regístrala para que se ejecute cuando tu aplicación no esté en primer plano.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 638d4e8b4d4bc074566c8b0a6c24d733a5769b32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164899"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Crear y registrar una tarea en segundo plano fuera del proceso

**API importantes**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Crea una tarea en segundo plano y regístrala para ejecutarla cuando tu aplicación no esté en primer plano. Este tema muestra cómo crear y registrar una tarea en segundo plano que se ejecuta en un proceso independiente al proceso de aplicación. Para efectuar un trabajo en segundo plano directamente en la aplicación en primer plano, consulta [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Si usas una tarea en segundo plano para reproducir contenido multimedia en segundo plano, consulta [Reproducir elementos multimedia en segundo plano](../audio-video-camera/background-audio.md) para obtener información sobre las mejoras en Windows 10, versión 1607, que lo hacen mucho más fácil.

## <a name="create-the-background-task-class"></a>Creación de la clase de tareas en segundo plano

Puedes ejecutar código en segundo plano escribiendo clases que implementen la interfaz [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask). Este código se ejecuta cuando se desencadena un evento específico mediante, por ejemplo, [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) o [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger).

En estos pasos te mostramos cómo escribir una nueva clase que implemente la interfaz [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).

1.  Crea un nuevo proyecto para tareas en segundo plano y agrégalo a tu solución. Para ello, haga clic con el botón derecho en el nodo de la solución en el **Explorador de soluciones** y seleccione **Agregar** \> **nuevo proyecto**. Después, seleccione el tipo de proyecto de **componente de Windows Runtime** , asigne un nombre al proyecto y haga clic en Aceptar.
2.  Haz referencia al proyecto de tareas en segundo plano desde tu proyecto de aplicación de la Plataforma universal de Windows (UWP). En el caso de una aplicación de C# o C++, en el proyecto de la aplicación, haga clic con el botón derecho en **referencias** y seleccione **Agregar nueva referencia**. En **Solución**, selecciona **Proyectos**, selecciona el nombre de tu proyecto de tarea en segundo plano y haz clic en **Aceptar**.
3.  En el proyecto de tareas en segundo plano, agregue una nueva clase que implemente la interfaz [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) . El método [**IBackgroundTask. Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) es un punto de entrada necesario al que se llamará cuando se desencadene el evento especificado. Este método es necesario en cada tarea en segundo plano.

> [!NOTE]
> La propia clase de tarea en segundo plano &mdash; y todas las demás clases del proyecto de tarea en segundo plano &mdash; deben ser clases **públicas** que estén **selladas** (o **finales**).

En el código de ejemplo siguiente se muestra un punto de partida muy básico para una clase de tarea en segundo plano.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  Si ejecutas algún código asincrónico en tu tarea en segundo plano, entonces esta necesita usar un aplazamiento. Si no usa un aplazamiento, el proceso de la tarea en segundo plano puede finalizar inesperadamente si el método **Run** vuelve antes de que se haya ejecutado cualquier trabajo asincrónico hasta que finalice.

Solicite el aplazamiento en el método de **ejecución** antes de llamar al método asincrónico. Guarde el aplazamiento en un miembro de datos de clase para que se pueda obtener acceso a él desde el método asincrónico. Declara el aplazamiento completo después de que se complete el código asincrónico.

En el código de ejemplo siguiente se obtiene el aplazamiento, se guarda y se libera cuando se completa el código asincrónico.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> En C#, se llama a los métodos asincrónicos de la tarea en segundo plano mediante las palabras clave **async/await**. En C++/CX, se puede lograr un resultado similar mediante el uso de una cadena de tareas.

Para más información acerca de los modelos asincrónicos, consulta el tema sobre la [Programación asincrónica](../threading-async/asynchronous-programming-universal-windows-platform-apps.md). Para obtener más ejemplos sobre cómo usar aplazamientos para evitar que una tarea en segundo plano termine antes de tiempo, consulta la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

Los siguientes pasos se completan en una de tus clases de aplicaciones (por ejemplo, MainPage.xaml.cs).

> [!NOTE]
> También puede crear una función dedicada para registrar tareas en segundo plano &mdash; . consulte [registrar una tarea en segundo plano](register-a-background-task.md). En ese caso, en lugar de usar los tres pasos siguientes, puede simplemente construir el desencadenador y proporcionarlo a la función de registro junto con el nombre de tarea, el punto de entrada de tarea y, opcionalmente, una condición.

## <a name="register-the-background-task-to-run"></a>Registrar la tarea en segundo plano por ejecutar

1.  Averigüe si la tarea en segundo plano ya está registrada recorriendo en iteración la propiedad [**BackgroundTaskRegistration. AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) . Este paso es importante; si tu aplicación no comprueba los registros de tareas en segundo plano existentes, fácilmente podría registrar una tarea varias veces. Esto causaría problemas de rendimiento y consumiría el tiempo de CPU disponible para la tarea antes de que su trabajo pueda completarse.

En el ejemplo siguiente se recorre en iteración la propiedad **AllTasks** y se establece una variable de marca en true si la tarea ya está registrada.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  Si aún no está registrada, usa [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para crear una instancia de la tarea en segundo plano. El punto de entrada de la tarea debe ser el nombre de tu clase de tarea en segundo plano con el espacio de nombres como prefijo.

El desencadenador de tarea en segundo plano controla cuándo se ejecutará la tarea en segundo plano. Para obtener una lista de posibles desencadenadores del sistema, consulta [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

Por ejemplo, este código crea una nueva tarea en segundo plano y la establece para que se ejecute cuando se produce el desencadenador **TimeZoneChanged** :

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  Puedes agregar una condición para controlar si la tarea se ejecutará después de que se produzca el evento del desencadenador (opcional). Por ejemplo, si no quieres que la tarea se ejecute hasta que el usuario esté presente, usa la condición **UserPresent**. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

El siguiente código de muestra asigna una condición que requiere que el usuario esté presente:

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  Registra la tarea en segundo plano llamando al método Register en el objeto [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Almacena el resultado de [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para usarlo en el siguiente paso.

El siguiente código registra la tarea en segundo plano y almacena el resultado:

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) antes de registrar cualquier tipo de desencadenador en segundo plano.

Para garantizar que tu aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, usa el desencadenador **ServicingComplete** (consulta [SystemTriggerType](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) para realizar los cambios de configuración posteriores a la actualización como la migración de la base de datos de la aplicación y el registro de tareas en segundo plano. Es recomendable anular el registro de tareas en segundo plano asociadas con la versión anterior de la aplicación (consulta [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)) y registrar las tareas en segundo plano para la nueva versión de la aplicación (consulta [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)) en este momento.

Para obtener más información, vea [instrucciones para tareas en segundo plano](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Controlar la finalización de tareas en segundo plano mediante controladores de eventos

Debes registrar un método con [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler), de manera que tu aplicación pueda obtener resultados de la tarea en segundo plano. Cuando la aplicación se inicia o se reanuda, se llama al método marcado si la tarea en segundo plano se ha completado desde la última vez que la aplicación estuvo en primer plano. (Se llamará de forma inmediata al método OnCompleted si la tarea en segundo plano se completa mientras tu aplicación se encuentra actualmente en primer plano).

1.  Escribe un método OnCompleted para administrar la finalización de tareas en segundo plano. Por ejemplo, el resultado de la tarea en segundo plano podría provocar una actualización de la interfaz de usuario. La superficie del método que se muestra aquí es necesaria para el método del controlador de eventos OnCompleted, incluso aunque este ejemplo no usa el parámetro *args*.

La siguiente muestra de código reconoce la finalización de la tarea en segundo plano y llama a un método de ejemplo de actualización de la interfaz de usuario que toma una cadena de mensaje.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> Las actualizaciones de la interfaz de usuario se deberían realizar de forma asincrónica, para evitar retener el subproceso de interfaz de usuario. Para ver un ejemplo, consulta el método UpdateUI en la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

2.  Vuelve al punto en el que registraste la tarea en segundo plano. Después de la línea de código, agrega un nuevo objeto [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler). Proporciona tu método OnCompleted como parámetro para el constructor **BackgroundTaskCompletedEventHandler**.

La siguiente muestra de código agrega un constructor [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) a [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration):

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Declare en el manifiesto de la aplicación que la aplicación usa tareas en segundo plano

Antes de que tu aplicación pueda ejecutar tareas en segundo plano, debes declarar cada una de ellas en el manifiesto de la aplicación. Si la aplicación intenta registrar una tarea en segundo plano con un desencadenador que no aparece en el manifiesto, se producirá un error en el registro de la tarea en segundo plano con el error "tiempo de ejecución de clase no registrado".

1.  Abre el diseñador de manifiestos del paquete. Para ello, abre el archivo Package.appxmanifest.
2.  Abre la pestaña **Declaraciones**.
3.  Desde la lista desplegable **Declaraciones disponibles**, selecciona **Tareas en segundo plano** y haz clic en **Agregar**.
4.  Activa la casilla **Evento del sistema**.
5.  En el cuadro de texto **punto de entrada:** , escriba el espacio de nombres y el nombre de la clase en segundo plano que se va a realizar en este ejemplo es Tasks. ExampleBackgroundTask.
6.  Cierra el diseñador de manifiesto.

Se añadirá el siguiente elemento Extensions al archivo Package.appxmanifest para registrar la tarea en segundo plano:

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Ahora deberías conocer los conceptos básicos de cómo escribir una clase de tareas en segundo plano, cómo registrar la tarea en segundo plano desde dentro de tu aplicación y cómo hacer que tu aplicación reconozca cuándo se ha completado la tarea en segundo plano. También deberías comprender cómo actualizar el manifiesto de la aplicación para que la aplicación pueda registrar correctamente la tarea en segundo plano.

> [!NOTE]
> Descarga la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) para ver ejemplos de código similares en el contexto de una aplicación para UWP completa y robusta que usa tareas en segundo plano.

Consulta los siguientes temas relacionados para obtener referencia de las API, una guía conceptual sobre tareas en segundo plano e instrucciones más detalladas para escribir aplicaciones que usan tareas en segundo plano.

## <a name="related-topics"></a>Temas relacionados

**Temas con instrucciones detalladas sobre las tareas en segundo plano**

* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Cree y registre una tarea en segundo plano en proceso](create-and-register-an-inproc-background-task.md).
* [Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso](convert-out-of-process-background-task.md)  

**Guía de tareas en segundo plano**

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones UWP (durante la depuración)](/previous-versions/hh974425(v=vs.110))

**Referencia de API de tareas en segundo plano**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)
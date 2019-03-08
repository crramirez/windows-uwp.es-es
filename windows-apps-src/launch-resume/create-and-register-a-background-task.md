---
title: Crear y registrar una tarea en segundo plano fuera del proceso
description: Crea una clase de tarea en segundo plano fuera del proceso y regístrala para que se ejecute cuando tu aplicación no esté en primer plano.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, uwp, tareas en segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9df6eef44d45db37e17610d6a5333f3a387b5cf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592170"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Crear y registrar una tarea en segundo plano fuera del proceso

**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Crea una tarea en segundo plano y regístrala para ejecutarla cuando tu aplicación no esté en primer plano. Este tema muestra cómo crear y registrar una tarea en segundo plano que se ejecuta en un proceso independiente al proceso de aplicación. Para efectuar un trabajo en segundo plano directamente en la aplicación en primer plano, consulta [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Si usas una tarea en segundo plano para reproducir contenido multimedia en segundo plano, consulta [Reproducir elementos multimedia en segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obtener información sobre las mejoras en Windows 10, versión 1607, que lo hacen mucho más fácil.

## <a name="create-the-background-task-class"></a>Creación de la clase de tareas en segundo plano

Puedes ejecutar código en segundo plano escribiendo clases que implementen la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Este código se ejecuta cuando se desencadena un evento específico mediante el uso, por ejemplo, [ **SystemTrigger** ](https://msdn.microsoft.com/library/windows/apps/br224839) o [ **MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517).

En estos pasos te mostramos cómo escribir una nueva clase que implemente la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).

1.  Crea un nuevo proyecto para tareas en segundo plano y agrégalo a tu solución. Para ello, haga doble clic en el nodo de solución en el **el Explorador de soluciones** y seleccione **agregar** \> **nuevo proyecto**. A continuación, seleccione el **componente de Windows en tiempo de ejecución** tipo de proyecto, denomine el proyecto y haga clic en Aceptar.
2.  Haz referencia al proyecto de tareas en segundo plano desde tu proyecto de aplicación de la Plataforma universal de Windows (UWP). Para un C# o aplicación de C++, en el proyecto de aplicación, haga doble clic en **referencias** y seleccione **agregar nueva referencia**. En **Solución**, selecciona **Proyectos**, selecciona el nombre de tu proyecto de tarea en segundo plano y haz clic en **Aceptar**.
3.  En el proyecto de tareas en segundo plano, agregue una nueva clase que implementa el [ **IBackgroundTask** ](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) interfaz. El [ **IBackgroundTask.Run** ](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) método es un punto de entrada obligatoria que se llamará cuando se desencadene el evento especificado, este método es necesario en todas las tareas en segundo plano.

> [!NOTE]
> La propia clase de tarea en segundo plano&mdash;y todas las demás clases en el proyecto de la tarea en segundo plano&mdash;deba ser **pública** clases que son **sealed** (o **final**).

El código de ejemplo siguiente muestra un punto de partida muy básico para una clase de tarea en segundo plano.

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

4.  Si ejecutas algún código asincrónico en tu tarea en segundo plano, entonces esta necesita usar un aplazamiento. Si no usas un aplazamiento, entonces el proceso de tarea en segundo plano puede finalizar inesperadamente si el **ejecutar** vuelve antes de cualquier trabajo asincrónico se ha ejecutado hasta su finalización.

Solicitar el aplazamiento en el **ejecutar** método antes de llamar al método asincrónico. Guarde el aplazamiento a un miembro de datos de clase para que se puede acceder desde el método asincrónico. Declara el aplazamiento completo después de que se complete el código asincrónico.

Ejemplo de código siguiente obtiene el aplazamiento, lo guarda y lo libera cuando se completa el código asincrónico.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral()
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
> En C#, se llama a los métodos asincrónicos de la tarea en segundo plano mediante las palabras clave **async/await**. En C / c++ / CX, se puede lograr un resultado similar mediante el uso de una cadena de tareas.

Para más información acerca de los modelos asincrónicos, consulta el tema sobre la [Programación asincrónica](https://msdn.microsoft.com/library/windows/apps/mt187335). Para obtener más ejemplos sobre cómo usar aplazamientos para evitar que una tarea en segundo plano termine antes de tiempo, consulta la [muestra de tarea en segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666).

Los siguientes pasos se completan en una de tus clases de aplicaciones (por ejemplo, MainPage.xaml.cs).

> [!NOTE]
> También puede crear una función dedicada para registrar tareas en segundo plano&mdash;vea [registrar una tarea en segundo plano](register-a-background-task.md). En ese caso, en lugar de los tres pasos siguientes, puede simplemente construir el desencadenador y proporcionarlos a la función de registro junto con el nombre de tarea, punto de entrada de tarea y (opcionalmente) una condición.

## <a name="register-the-background-task-to-run"></a>Registrar la tarea en segundo plano por ejecutar

1.  Averigüe si la tarea en segundo plano ya está registrada mediante la iteración a través de la [ **BackgroundTaskRegistration.AllTasks** ](https://msdn.microsoft.com/library/windows/apps/br224787) propiedad. Este paso es importante; si tu aplicación no comprueba los registros de tareas en segundo plano existentes, fácilmente podría registrar una tarea varias veces. Esto causaría problemas de rendimiento y consumiría el tiempo de CPU disponible para la tarea antes de que su trabajo pueda completarse.

El ejemplo siguiente se procesa una iteración en la **AllTasks** propiedad y establece una variable de marca en true si la tarea ya está registrada.

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

2.  Si aún no está registrada, usa [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para crear una instancia de la tarea en segundo plano. El punto de entrada de la tarea debe ser el nombre de tu clase de tarea en segundo plano con el espacio de nombres como prefijo.

El desencadenador de tarea en segundo plano controla cuándo se ejecutará la tarea en segundo plano. Para obtener una lista de posibles desencadenadores del sistema, consulta [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Por ejemplo, este código crea una nueva tarea en segundo plano y establece que se ejecute cuando el **TimeZoneChanged** desencadenador se produce:

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

3.  Puedes agregar una condición para controlar si la tarea se ejecutará después de que se produzca el evento del desencadenador (opcional). Por ejemplo, si no quieres que la tarea se ejecute hasta que el usuario esté presente, usa la condición **UserPresent**. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

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

4.  Registra la tarea en segundo plano llamando al método Register en el objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Almacena el resultado de [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) para usarlo en el siguiente paso.

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
> Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.

Para garantizar que tu aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, usa el desencadenador **ServicingComplete** (consulta [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) para realizar los cambios de configuración posteriores a la actualización como la migración de la base de datos de la aplicación y el registro de tareas en segundo plano. Es recomendable anular el registro de tareas en segundo plano asociadas con la versión anterior de la aplicación (consulta [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)) y registrar las tareas en segundo plano para la nueva versión de la aplicación (consulta [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)) en este momento.

Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Controlar la finalización de tareas en segundo plano mediante controladores de eventos

Debes registrar un método con [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781), de manera que tu aplicación pueda obtener resultados de la tarea en segundo plano. Cuando la aplicación se inicia o reanuda, se llamará el método marcado si se ha completado la tarea en segundo plano desde la última vez que la aplicación estaba en primer plano. (Se llamará de forma inmediata al método OnCompleted si la tarea en segundo plano se completa mientras tu aplicación se encuentra actualmente en primer plano).

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
> Las actualizaciones de la interfaz de usuario se deberían realizar de forma asincrónica, para evitar retener el subproceso de interfaz de usuario. Para ver un ejemplo, consulta el método UpdateUI en la [muestra de tarea en segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666).

2.  Vuelve al punto en el que registraste la tarea en segundo plano. Después de la línea de código, agrega un nuevo objeto [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Proporciona tu método OnCompleted como parámetro para el constructor **BackgroundTaskCompletedEventHandler**.

La siguiente muestra de código agrega un constructor [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) a [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786):

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Declarar en el manifiesto de aplicación que la aplicación usa tareas en segundo plano

Antes de que tu aplicación pueda ejecutar tareas en segundo plano, debes declarar cada una de ellas en el manifiesto de la aplicación. Si la aplicación intenta registrar una tarea en segundo plano con un desencadenador que no aparece en el manifiesto, se producirá un error en el registro de la tarea en segundo plano con un error "en tiempo de ejecución class no registrado".

1.  Abre el diseñador de manifiestos del paquete. Para ello, abre el archivo Package.appxmanifest.
2.  Abre la pestaña **Declaraciones**.
3.  Desde la lista desplegable **Declaraciones disponibles**, selecciona **Tareas en segundo plano** y haz clic en **Agregar**.
4.  Activa la casilla **Evento del sistema**.
5.  En el **punto de entrada:** cuadro de texto, escriba el espacio de nombres y el nombre de la clase en segundo plano que para este ejemplo es Tasks.ExampleBackgroundTask.
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
> Descarga la [muestra de tarea en segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666) para ver ejemplos de código similares en el contexto de una aplicación para UWP completa y robusta que usa tareas en segundo plano.

Consulta los siguientes temas relacionados para obtener referencia de las API, una guía conceptual sobre tareas en segundo plano e instrucciones más detalladas para escribir aplicaciones que usan tareas en segundo plano.

## <a name="related-topics"></a>Temas relacionados

**Información detallada de la tarea temas informativos**

* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Uso de un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Controlar una tarea cancelada en segundo plano](handle-a-cancelled-background-task.md)
* [Supervisar el progreso de la tarea en segundo plano y de finalización](monitor-background-task-progress-and-completion.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md).
* [Convertir una tarea en segundo plano fuera de proceso en una tarea en segundo plano en curso](convert-out-of-process-background-task.md)  

**Guía de tareas en segundo plano**

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar suspender, reanudar y en segundo plano de los eventos en aplicaciones para UWP (al depurar)](https://go.microsoft.com/fwlink/p/?linkid=254345)

**Referencia de API de tareas en segundo plano**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

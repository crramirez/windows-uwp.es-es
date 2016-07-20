---
author: TylerMSFT
title: Crear y registrar una tarea en segundo plano
description: "Crea una tarea en segundo plano y regístrala para ejecutarla cuando tu aplicación no esté en primer plano."
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
translationtype: Human Translation
ms.sourcegitcommit: 579547b7bd2ee76390b8cac66855be4a9dce008e
ms.openlocfilehash: e8da193f96709bdd87bd6a008eb5885cc5c819fd

---

# Crear y registrar una tarea en segundo plano


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Crea una tarea en segundo plano y regístrala para ejecutarla cuando tu aplicación no esté en primer plano.

## Crear la clase de tareas en segundo plano


Puedes ejecutar código en segundo plano escribiendo clases que implementen la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Este código se ejecutará cuando se desencadene un evento específico usando, por ejemplo, [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) o [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517).

En estos pasos te mostramos cómo escribir una nueva clase que implemente la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Antes de comenzar, crea un nuevo proyecto en tu solución para tareas en segundo plano. Agrega una clase vacía para tu tarea en segundo plano e importa el espacio de nombres [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847).

1.  Crea un nuevo proyecto para tareas en segundo plano y agrégalo a tu solución. Para ello, haz clic con el botón secundario en el nodo de la solución en el **Explorador de soluciones** y selecciona Agregar-&gt;Nuevo proyecto. Después, selecciona el tipo de proyecto **Componente de Windows Runtime (Windows Universal)**, asigna un nombre al proyecto y haz clic en Aceptar.
2.  Haz referencia al proyecto de tareas en segundo plano desde tu proyecto de aplicación de la Plataforma universal de Windows (UWP).

    Para una aplicación C++, haz clic con el botón secundario en el proyecto de tu aplicación y selecciona **Propiedades**. Luego, ve a **Propiedades comunes** y haz clic en **Agregar nueva referencia**, activa la casilla que se encuentra junto a tu proyecto de tareas en segundo plano y haz clic en **Aceptar** en los dos cuadros de diálogo.

    Para una aplicación C#, en tu proyecto de aplicación, haz clic con el botón secundario en **Referencias** y selecciona **Agregar nueva referencia**. En **Solución**, selecciona **Proyectos**, selecciona el nombre de tu proyecto de tarea en segundo plano y haz clic en **Aceptar**.

3.  Crea una nueva clase que implemente la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). El método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) es un punto de entrada necesario al que se llamará cuando se desencadene el evento especificado; este método es necesario en todas las tareas en segundo plano.

    > [!NOTE]
    > La propia clase de tareas en segundo plano (y todas las demás clases en el proyecto de tareas en segundo plano) deben ser clases **public** de tipo **sealed**.

    El siguiente código de ejemplo muestra un punto de inicio muy básico para una clase de tarea en segundo plano:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     //
    >     // ExampleBackgroundTask.cs
    >     //
    >
    >     using Windows.ApplicationModel.Background;
    >
    >     namespace Tasks
    >     {
    >         public sealed class ExampleBackgroundTask : IBackgroundTask
    >         {
    >             public void Run(IBackgroundTaskInstance taskInstance)
    >             {
    >                 
    >             }        
    >         }
    >     }
    > ```
    > ```cpp
    >     //
    >     // ExampleBackgroundTask.h
    >     //
    >
    >     #pragma once
    >
    >     using namespace Windows::ApplicationModel::Background;
    >
    >     namespace RuntimeComponent1
    >     {
    >         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    >         {
    >
    >         public:
    >             ExampleBackgroundTask();
    >
    >             virtual void Run(IBackgroundTaskInstance^ taskInstance);
    >             void OnCompleted(
    >                     BackgroundTaskRegistration^ task,
    >                     BackgroundTaskCompletedEventArgs^ args
    >                     );
    >         };
    >     }
    >
    >     //
    >     // ExampleBackgroundTask.cpp
    >     //
    >
    >     #include "ExampleBackgroundTask.h"
    >
    >     using namespace Tasks;
    >
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >
    >     }
    >  ```

4.  Si ejecutas algún código asincrónico en tu tarea en segundo plano, entonces esta necesita usar un aplazamiento. Si no, el proceso de la tarea en segundo plano puede terminar de forma inesperada si el método Run finaliza antes de que lo haga la llamada a tu método asincrónico.

    Solicita el aplazamiento en el método Run antes de llamar al método asincrónico. Guarda el aplazamiento en una variable global de forma que se pueda obtener acceso a él desde el método asincrónico. Declara el aplazamiento completo después de que se complete el código asincrónico.

    El siguiente código de ejemplo obtiene el aplazamiento, lo guarda y lo libera cuando se completa el código asincrónico:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskDeferral _deferral = taskInstance.GetDeferral(); // Note: define at class scope
    >     public async void Run(IBackgroundTaskInstance taskInstance)
    >     {
    >         //
    >         // TODO: Insert code to start one or more asynchronous methods using the
    >         //       await keyword, for example:
    >         //
    >         // await ExampleMethodAsync();
    >         //
    >         
    >         _deferral.Complete();
    >     }
    > ```
    > ```cpp
    >     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: define at class scope
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >         //
    >         // TODO: Modify the following line of code to call a real async function.
    >         //       Note that the task<void> return type applies only to async
    >         //       actions. If you need to call an async operation instead, replace
    >         //       task<void> with the correct return type.
    >         //
    >         task<void> myTask(ExampleFunctionAsync());
    >         
    >         myTask.then([=] () {
    >             deferral->Complete();
    >         });
    >     }
    > ```

> [!NOTE]
> En C#, se llama a los métodos asincrónicos de la tarea en segundo plano mediante las palabras clave **async/await**. En C++, se puede lograr un resultado similar con una cadena de tareas.

Para más información acerca de los modelos asincrónicos, consulta el tema sobre la [Programación asincrónica](https://msdn.microsoft.com/library/windows/apps/mt187335). Para obtener más ejemplos sobre cómo usar aplazamientos para evitar que una tarea en segundo plano termine antes de tiempo, consulta la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Los siguientes pasos se completan en una de tus clases de aplicaciones (por ejemplo, MainPage.xaml.cs).

> [!NOTE]
> También puedes crear una función dedicada al registro de tareas en segundo plano. Consulta [Registrar una tarea en segundo plano](register-a-background-task.md). En ese caso, en lugar de usar los siguientes 3 pasos, simplemente puedes construir el desencadenador y proporcionar la función de registro junto con el nombre de la tarea, el punto de entrada de la tarea y (de forma opcional) una condición.


## Registrar la tarea en segundo plano por ejecutar

1.  Averigua si la tarea en segundo plano ya está registrada mediante la iteración a través de la propiedad [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787). Este paso es importante; si tu aplicación no comprueba los registros de tareas en segundo plano existentes, fácilmente podría registrar una tarea varias veces. Esto causaría problemas de rendimiento y consumiría el tiempo de CPU disponible para la tarea antes de que su trabajo pueda completarse.

    El siguiente ejemplo itera en la propiedad AllTasks y establece una variable de marca en true, si la tarea ya está registrada:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var taskRegistered = false;
    >     var exampleTaskName = "ExampleBackgroundTask";
    >
    >     foreach (var task in BackgroundTaskRegistration.AllTasks)
    >     {
    >         if (task.Value.Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >     }
    > ```
    > ```cpp
    >     boolean taskRegistered = false;
    >     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
    >
    >     auto iter = BackgroundTaskRegistration::AllTasks->First();
    >     auto hascur = iter->HasCurrent;
    >
    >     while (hascur)
    >     {
    >         auto cur = iter->Current->Value;
    >
    >         if(cur->Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >
    >         hascur = iter->MoveNext();
    >     }
    > ```

2.  Si aún no está registrada, usa [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para crear una instancia de la tarea en segundo plano. El punto de entrada de la tarea debe ser el nombre de tu clase de tarea en segundo plano con el espacio de nombres como prefijo.

    El desencadenador de tarea en segundo plano controla cuándo se ejecutará la tarea en segundo plano. Para obtener una lista de posibles desencadenadores del sistema, consulta [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

    Por ejemplo, este código crea una nueva tarea en segundo plano y establece que se ejecute cuando el desencadenador **TimeZoneChanged** se activa:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var builder = new BackgroundTaskBuilder();
    >
    >     builder.Name = exampleTaskName;
    >     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
    > ```
    > ```cpp
    >     auto builder = ref new BackgroundTaskBuilder();
    >
    >     builder->Name = exampleTaskName;
    >     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
    > ```

3.  Puedes agregar una condición para controlar si la tarea se ejecutará después de que se produzca el evento del desencadenador (opcional). Por ejemplo, si no quieres que la tarea se ejecute hasta que el usuario esté presente, usa la condición **UserPresent**. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    El siguiente código de muestra asigna una condición que requiere que el usuario esté presente:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```
    > ```cpp
    >     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
    > ```

4.  Registra la tarea en segundo plano llamando al método Register en el objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Almacena el resultado de [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) para usarlo en el siguiente paso.

    El siguiente código registra la tarea en segundo plano y almacena el resultado:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskRegistration task = builder.Register();
    >     ```
    > ```cpp
    >     BackgroundTaskRegistration^ task = builder->Register();
    > ```

> [!NOTE]
> Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.

Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

## Controlar la finalización de tareas en segundo plano mediante controladores de eventos


Debes registrar un método con [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781), de manera que tu aplicación pueda obtener resultados de la tarea en segundo plano. Cuando se inicie o reanude la aplicación, se llamará al método mark si la tarea en segundo plano se ha completado desde la última vez que la aplicación estuvo en primer plano. (Se llamará de forma inmediata al método OnCompleted si la tarea en segundo plano se completa mientras tu aplicación se encuentra actualmente en primer plano).

1.  Escribe un método OnCompleted para administrar la finalización de tareas en segundo plano. Por ejemplo, el resultado de la tarea en segundo plano podría provocar una actualización de la interfaz de usuario. La superficie del método que se muestra aquí es necesaria para el método del controlador de eventos OnCompleted, incluso aunque este ejemplo no usa el parámetro *args*.

    La siguiente muestra de código reconoce la finalización de la tarea en segundo plano y llama a un método de ejemplo de actualización de la interfaz de usuario que toma una cadena de mensaje.

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    >         var key = task.TaskId.ToString();
    >         var message = settings.Values[key].ToString();
    >         UpdateUI(message);
    >     }
    > ```
    > ```cpp
    >     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {
    >         auto settings = ApplicationData::Current->LocalSettings->Values;
    >         auto key = task->TaskId.ToString();
    >         auto message = dynamic_cast<String^>(settings->Lookup(key));
    >         UpdateUI(message);
    >     }
    > ```

    > [!NOTE]
    > Las actualizaciones de la interfaz de usuario se deberían realizar de forma asincrónica, para evitar retener el subproceso de interfaz de usuario. Para ver un ejemplo, consulta el método UpdateUI en la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).



2.  Vuelve al punto en el que registraste la tarea en segundo plano. Después de la línea de código, agrega un nuevo objeto [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Proporciona tu método OnCompleted como parámetro para el constructor **BackgroundTaskCompletedEventHandler**.

    La siguiente muestra de código agrega un constructor [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) a [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786):

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    > ```
    > ```cpp
    >     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    > ```

## Declarar que tu aplicación usa tareas en segundo plano en el manifiesto de la aplicación

Antes de que tu aplicación pueda ejecutar tareas en segundo plano, debes declarar cada una de ellas en el manifiesto de la aplicación. Si tu aplicación trata de registrar una tarea en segundo plano con un desencadenador que no esté en la lista del manifiesto, no se podrá realizar el registro.

1.  Abre el diseñador de manifiestos del paquete. Para ello, abre el archivo Package.appxmanifest.
2.  Abre la pestaña **Declaraciones**.
3.  Desde la lista desplegable **Declaraciones disponibles**, selecciona **Tareas en segundo plano** y haz clic en **Agregar**.
4.  Activa la casilla **Evento del sistema**.
5.  En el cuadro de texto **Punto de entrada:**, escribe el espacio de nombres y el nombre de la clase de segundo plano que, para este ejemplo, es RuntimeComponent1.ExampleBackgroundTask.
6.  Cierra el diseñador de manifiesto.

    Se añadirá el siguiente elemento Extensions al archivo Package.appxmanifest para registrar la tarea en segundo plano:

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## Resumen y pasos siguientes


Ahora deberías conocer los conceptos básicos de cómo escribir una clase de tareas en segundo plano, cómo registrar la tarea en segundo plano desde dentro de tu aplicación y cómo hacer que tu aplicación reconozca cuándo se ha completado la tarea en segundo plano. También deberías comprender cómo actualizar el manifiesto de la aplicación para que la aplicación pueda registrar correctamente la tarea en segundo plano.

> [!NOTE]
> Descarga la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) para ver ejemplos de código similares en el contexto de una aplicación para UWP completa y robusta que usa tareas en segundo plano.

Consulta los siguientes temas relacionados para obtener referencia de las API, una guía conceptual sobre tareas en segundo plano e instrucciones más detalladas para escribir aplicaciones que usan tareas en segundo plano.

> [!NOTE]
> Este artículo está orientado a desarrolladores de Windows 10 que vayan a programar aplicaciones de la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Temas relacionados

**Temas con instrucciones detalladas sobre las tareas en segundo plano**

* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)

**Guía de tareas en segundo plano**

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones de la Tienda Windows (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Referencia de API de tareas en segundo plano**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)



<!--HONumber=Jul16_HO1-->



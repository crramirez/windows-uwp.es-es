---
author: TylerMSFT
title: Establecer condiciones para ejecutar una tarea en segundo plano
description: "Aprende a establecer condiciones que controlen cuándo se ejecutará tu tarea en segundo plano."
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 0f95bdcb197f472b743f81c0d941196d5e53f60a

---

# Establecer condiciones para ejecutar una tarea en segundo plano


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Aprende a establecer condiciones que controlan cuándo se ejecutará tu tarea en segundo plano.

En ocasiones, las tareas en segundo plano requieren que se cumplan ciertas condiciones, además del evento que desencadena la tarea, para que la tarea en segundo plano se desarrolle correctamente. Puedes especificar una o más de las condiciones especificadas por [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) al registrar tu tarea en segundo plano. La condición se comprobará después de que se active el desencadenador; la tarea en segundo plano se pondrá en cola, pero no se ejecutará hasta que se satisfagan todas las condiciones.

Establecer condiciones sobre las tareas en segundo plano ahorra vida útil de batería y tiempo de ejecución de CPU evitando que las tareas se ejecuten innecesariamente. Por ejemplo, si una tarea en segundo plano se ejecuta en un temporizador y requiere conectividad a Internet, agrega la condición **InternetAvailable** al [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) antes de registrar la tarea. Esto ayudará a impedir que la tarea use recursos del sistema y consuma batería de forma innecesaria dejando que se ejecute cuando haya transcurrido el temporizador e Internet esté disponible.


            **Nota** Es posible combinar varias condiciones llamando a AddCondition varias veces en el mismo [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Procura no agregar condiciones conflictivas, como **UserPresent** y **UserNotPresent**.

 

## Crear un objeto SystemCondition


En este tema se supone que ya tienes una tarea en segundo plano asociada con tu aplicación y que tu aplicación ya incluye código que crea un objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) denominado **taskBuilder**.

Antes de agregar la condición, crea un objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) que representa la condición que debe estar vigente para que una tarea en segundo plano se ejecute. En el constructor, especifica la condición que debe cumplirse proporcionando un valor de enumeración [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

El siguiente código crea un objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) que especifica la disponibilidad de Internet como requisito condicional:

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## [!div class="tabbedCodeSnippets"]


Agregar el objeto SystemCondition a la tarea en segundo plano

Para agregar una condición, llama al método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) en el objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) y pásalo al objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834).

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## El siguiente código registra la condición de tarea en segundo plano InternetAvailable con TaskBuilder:


[!div class="tabbedCodeSnippets"]

Registrar tu tarea en segundo plano

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> Ahora ya puedes registrar tu tarea en segundo plano con el método [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) y la tarea no comenzará hasta que no se satisfaga la condición especificada.

El siguiente código registra la tarea y almacena el objeto BackgroundTaskRegistration resultante: [!div class="tabbedCodeSnippets"]

> 
            **Nota** Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano. Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

## 
            **Nota** Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro.

Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se enfrente correctamente a los escenarios en que se produce un error en el registro de tareas en segundo plano. Si la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

> Colocar varias condiciones en tu tarea en segundo plano
 

Para agregar varias condiciones, tu aplicación realiza varias llamadas al método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) .

> [!div class="tabbedCodeSnippets"]
```cs
> //
> // Set up the background task.
> //
>
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
>
> var recurringTaskBuilder = new BackgroundTaskBuilder();
>
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
>
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```
```cpp
> //
> // Set up the background task.
> //
>
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
>
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
>
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
>
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## Estas llamadas deben recibirse antes de que el registro de la tarea entre en vigor.


> 
            **Nota** Procura no agregar condiciones conflictivas a una tarea en segundo plano. En el siguiente fragmento se muestran múltiples condiciones en el contexto de creación y registro de una tarea en segundo plano:

> [!div class="tabbedCodeSnippets"] Observaciones

 

## 
            **Nota** Elige las condiciones adecuadas para tu tarea en segundo plano de forma que solo se ejecute cuando sea necesaria y no se ejecute cuando no vaya a funcionar.


****

* [Consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) para obtener descripciones de las diferentes condiciones de las tareas en segundo plano.](create-and-register-a-background-task.md)
* [
            **Nota** Este artículo está orientado a desarrolladores de Windows 10 que escriben aplicaciones para la Plataforma universal de Windows (UWP).](declare-background-tasks-in-the-application-manifest.md)
* [Si estás desarrollando para Windows8.x o Windows Phone8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).](handle-a-cancelled-background-task.md)
* [Temas relacionados](monitor-background-task-progress-and-completion.md)
* [Crear y registrar una tarea en segundo plano](register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](respond-to-system-events-with-background-tasks.md)
* [Controlar una tarea en segundo plano cancelada](update-a-live-tile-from-a-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](use-a-maintenance-trigger.md)
* [Registrar una tarea en segundo plano](run-a-background-task-on-a-timer-.md)
* [Responder a eventos del sistema con tareas en segundo plano](guidelines-for-background-tasks.md)

****

* [Actualizar un icono dinámico desde una tarea en segundo plano](debug-a-background-task.md)
* [Usar un desencadenador de mantenimiento](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->



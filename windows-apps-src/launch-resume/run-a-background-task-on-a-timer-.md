---
author: TylerMSFT
title: Ejecutar una tarea en segundo plano en un temporizador
description: "Aprende a programar una tarea en segundo plano única o a ejecutar una tarea en segundo plano periódica."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fc1e3efa742ff8ab24f78856872fe322703f152

---

# Ejecutar una tarea en segundo plano en un temporizador


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Aprende a programar una tarea en segundo plano única o ejecutar una tarea en segundo plano periódica.

-   En este ejemplo se da por hecho que tienes una tarea en segundo plano que necesita ejecutarse periódicamente o en un momento específico para admitir tu aplicación. Una tarea en segundo plano solo se ejecutará mediante un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) si has llamado a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).
-   En este tema se da por hecho que ya has creado una clase de tarea en segundo plano, incluido el método Run que se usa como punto de entrada de la tarea en segundo plano. Para comenzar rápidamente a crear una tarea en segundo plano, consulta [Creación y registro de una tarea en segundo plano](create-and-register-a-background-task.md). Para obtener información más detallada sobre las condiciones y los desencadenadores, consulta [Dar soporte a una aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

## Crear un desencadenador de hora


-   Crea un nuevo [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). El segundo parámetro, *OneShot*, especifica si la tarea en segundo plano se ejecutará una vez o seguirá ejecutándose periódicamente. Si *OneShot* se establece en true, el primer parámetro (*FreshnessTime*) especifica el número de minutos que deben esperarse antes de programar la tarea en segundo plano. Si *OneShot* se establece en false, *FreshnessTime* especifica la frecuencia con la que se ejecutará la tarea en segundo plano.

    El temporizador integrado de las aplicaciones para la Plataforma universal de Windows (UWP) destinadas a la familia de dispositivos de escritorio o móviles ejecuta tareas en segundo plano en intervalos de 15 minutos.

    -   Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor true, la tarea se ejecutará una vez entre 0 y 15 minutos después de que se registre.

    -   Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor false, la tarea se ejecutará cada 15 minutos comenzando entre el minuto 0 y el 15 desde el momento en que se registre.

    **Nota** Si *FreshnessTime* se establece en menos de 15 minutos, se iniciará una excepción cuando se intente registrar la tarea en segundo plano.

     

    Por ejemplo, este desencadenador hará que una tarea en segundo plano se ejecute una vez cada hora:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## [!div class="tabbedCodeSnippets"]


-   (Opcional) Agregar una condición Si es necesario, crea una condición a la tarea en segundo plano para controlar cuándo debe ejecutarse la tarea.

    Una condición evita que tu tarea en segundo plano se ejecute hasta que no se satisfaga la condición; para obtener más información, consulta [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md). En este ejemplo, la condición se establece en **UserPresent** de forma que, al desencadenarse, la tarea solo se ejecuta cuando el usuario está activo.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).


-   [!div class="tabbedCodeSnippets"]

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## Llamar a RequestAccessAsync()


-   Antes de intentar registrar la tarea en segundo plano [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), llama a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494). [!div class="tabbedCodeSnippets"]

    Registrar la tarea en segundo plano

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```

    > Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para más información sobre el registro de tareas en segundo plano, consulta [Registrar una tarea en segundo plano](register-a-background-task.md). El siguiente código registra la tarea en segundo plano:


## [!div class="tabbedCodeSnippets"]

> **Nota** Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido.

> Asegúrate de que la aplicación se enfrente correctamente a los escenarios en que se produce un error en el registro de tareas en segundo plano. Si la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee. Observaciones


## **Nota** A partir de Windows 10, ya no es necesario que el usuario agregue tu aplicación a la pantalla de bloqueo para poder usar las tareas en segundo plano.


* [Para obtener una guía sobre los tipos de desencadenadores de tarea en segundo plano, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).](create-and-register-a-background-task.md)
* [**Nota** Este artículo está orientado a desarrolladores de Windows 10 que escriben aplicaciones para la Plataforma universal de Windows (UWP).](declare-background-tasks-in-the-application-manifest.md)
* [Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).](handle-a-cancelled-background-task.md)
* [Temas relacionados](monitor-background-task-progress-and-completion.md)
* [Crear y registrar una tarea en segundo plano](register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](respond-to-system-events-with-background-tasks.md)
* [Controlar una tarea en segundo plano cancelada](set-conditions-for-running-a-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Registrar una tarea en segundo plano](use-a-maintenance-trigger.md)
* [Responder a eventos del sistema con tareas en segundo plano](guidelines-for-background-tasks.md)

* [Establecer condiciones para ejecutar una tarea en segundo plano](debug-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->



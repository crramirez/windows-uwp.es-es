---
author: mcleblanc
title: Usar un desencadenador de mantenimiento
description: Aprende a usar la clase MaintenanceTrigger para ejecutar código ligero en segundo plano mientras el dispositivo está enchufado.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
---

# Usar un desencadenador de mantenimiento


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Aprende a usar la clase [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para ejecutar código ligero en segundo plano mientras el dispositivo está enchufado.

## Crear un objeto de desencadenador de mantenimiento


Este ejemplo da por hecho que tienes un código ligero que puedes ejecutar en segundo plano para mejorar tu aplicación mientras el dispositivo está enchufado. Este tema se centra en la clase [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), que es similar a [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). Para obtener más información sobre cómo escribir una clase de tarea en segundo plano, consulta [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md).

Crea un nuevo objeto [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). El segundo parámetro, *OneShot*, especifica si la tarea de mantenimiento se ejecutará una vez o seguirá ejecutándose periódicamente. Si *OneShot* se establece en true, el primer parámetro (*FreshnessTime*) especifica el número de minutos que deben esperarse antes de programar la tarea en segundo plano. Si *OneShot* se establece en false, *FreshnessTime* especifica la frecuencia con la que se ejecutará la tarea en segundo plano.

> **Nota** Si *FreshnessTime* se establece en menos de 15 minutos, se iniciará una excepción cuando se intente registrar la tarea en segundo plano.

 

Este código de ejemplo crea un desencadenador que se ejecuta una vez cada hora:

> [!div class="tabbedCodeSnippets"]
> ```cs
> uint waitIntervalMinutes = 60;
> 
> MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
> ```
> ```cpp
> unsigned int waitIntervalMinutes = 60;
> 
> MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
> ```

## (Opcional) Agregar una condición

-   Si es necesario, crea una condición a la tarea en segundo plano para controlar cuándo debe ejecutarse la tarea. Una condición evita que tu tarea en segundo plano se ejecute hasta que no se cumpla la condición. Para obtener más información, consulta [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

    En este ejemplo, la condición se establece en **InternetAvailable** de forma que el mantenimiento se ejecuta cuando Internet está disponible (o cuando vuelva a estarlo). Para obtener una lista de posibles condiciones de tareas en segundo plano, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    El siguiente código agrega una condición al generador de tareas de mantenimiento:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
    > ```
    > ```cpp
    > SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
    > ```

## Registrar la tarea en segundo plano


-   Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para más información sobre el registro de tareas en segundo plano, consulta [Registrar una tarea en segundo plano](register-a-background-task.md).

    El siguiente código registra la tarea de mantenimiento:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Maintenance background task example";
    > 
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Maintenance background task example";
    > 
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```
    
    > **Nota** Para todas las familias de dispositivos, excepto la de equipos de escritorio, si el dispositivo dispone de poca memoria, las tareas en segundo plano pueden finalizarse. Si no se expone una excepción de falta de memoria o la aplicación no la controla, la tarea en segundo plano finalizará sin que se muestre ninguna advertencia y sin que se genere el evento OnCanceled. Esto contribuye a garantizar la experiencia del usuario de la aplicación en primer plano. La tarea en segundo plano debe estar diseñada para controlar este escenario.

    > **Nota** Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.

    Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

    > **Nota** Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se enfrente correctamente a los escenarios en que se produce un error en el registro de tareas en segundo plano. Si la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.


> **Nota** Este artículo está orientado a desarrolladores de Windows 10 que escriben aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Temas relacionados


****

* [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)

****

* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones de la Tienda Windows (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=May16_HO2-->



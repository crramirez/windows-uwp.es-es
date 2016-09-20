---
author: TylerMSFT
title: Responder a eventos del sistema con tareas en segundo plano
description: Aprende a crear una tarea en segundo plano que responda a eventos SystemTrigger.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: f6845dce428f5e22ec68744293b1668da52002bf

---

# Responder a eventos del sistema con tareas en segundo plano


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

Aprende a crear una tarea en segundo plano que responda a eventos [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

En este tema se supone que tienes una clase de tareas en segundo plano escrita para tu aplicación y que esta tarea necesita ejecutarse en respuesta a un evento desencadenado por el sistema, como que Internet vuelva a estar disponible o que el usuario inicie sesión. Este tema se centra en la clase [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). Para obtener más información sobre cómo escribir una clase de tarea en segundo plano, consulta [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md).

## Crear un objeto SystemTrigger


-   En el código de tu aplicación, crea un nuevo objeto [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). El primer parámetro, *triggerType*, especifica el tipo de desencadenador de eventos del sistema que activará esta tarea en segundo plano. Para ver una lista de los tipos de eventos, consulta [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839).

    El segundo parámetro, *OneShot*, especifica si la tarea en segundo plano se ejecutará una vez la próxima ocasión que ocurra el evento del sistema y se desencadenen las tareas en segundo plano; o, cada vez que ocurra el evento del sistema, hasta que se elimine el registro de la tarea.

    El siguiente código especifica que la tarea en segundo plano se ejecuta siempre que Internet vuelva a estar disponible:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## [!div class="tabbedCodeSnippets"]


-   Registrar la tarea en segundo plano Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano.

    Para más información sobre el registro de tareas en segundo plano, consulta [Registrar una tarea en segundo plano](register-a-background-task.md).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > El siguiente código registra la tarea en segundo plano:

    [!div class="tabbedCodeSnippets"] 
            **Nota** Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.

    > Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md). 
            **Nota** Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro.

     

## Se devuelve un error si cualquiera de los parámetros de registro no es válido.


Asegúrate de que la aplicación se enfrente correctamente a los escenarios en que se produce un error en el registro de tareas en segundo plano. Si la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

Observaciones Para ver el registro de una tarea en segundo plano en acción, descarga la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Las tareas en segundo plano pueden ejecutarse en respuesta a los eventos [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) y [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), pero aún debes [declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md). También debes llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de tarea en segundo plano.

> Las aplicaciones pueden registrar tareas en segundo plano que respondan a los eventos [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) y [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831), lo que les permite proporcionar una comunicación en tiempo real con el usuario aunque la aplicación no esté en el primer plano. Para obtener más información, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

 
## 
            **Nota** Este artículo está orientado a desarrolladores de Windows 10 que escriben aplicaciones para la Plataforma universal de Windows (UWP).


****

* [Si estás desarrollando para Windows8.x o Windows Phone8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).](create-and-register-a-background-task.md)
* [Temas relacionados](declare-background-tasks-in-the-application-manifest.md)
* [Crear y registrar una tarea en segundo plano](handle-a-cancelled-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](monitor-background-task-progress-and-completion.md)
* [Controlar una tarea en segundo plano cancelada](register-a-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](set-conditions-for-running-a-background-task.md)
* [Registrar una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](use-a-maintenance-trigger.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](run-a-background-task-on-a-timer-.md)
* [Usar un desencadenador de mantenimiento](guidelines-for-background-tasks.md)

****

* [Ejecutar una tarea en segundo plano en un temporizador](debug-a-background-task.md)
* [Directrices para tareas en segundo plano](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->



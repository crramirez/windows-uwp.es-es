---
author: TylerMSFT
title: Ejecutar una tarea en segundo plano en un temporizador
description: "Aprende a programar una tarea en segundo plano única o a ejecutar una tarea en segundo plano periódica."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
translationtype: Human Translation
ms.sourcegitcommit: ea862ef33f58b33b70318ddfc1d09d9aca9b3517
ms.openlocfilehash: 488bbbf1dbe99d653dded0af78a8fd22c7429cde

---

# <a name="run-a-background-task-on-a-timer"></a>Ejecutar una tarea en segundo plano en un temporizador

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Aprende a programar una tarea en segundo plano única o ejecutar una tarea en segundo plano periódica.

-   En este ejemplo se da por hecho que tienes una tarea en segundo plano que necesita ejecutarse periódicamente o en un momento específico para admitir tu aplicación. Una tarea en segundo plano solo se ejecutará mediante un [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) si has llamado a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).
-   En este tema, se supone que ya has creado una clase de tarea en segundo plano. Para comenzar rápidamente a crear una tarea en segundo plano, consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) o [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md). Para obtener información más detallada acerca de las condiciones y los desencadenadores, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

## <a name="create-a-time-trigger"></a>Crear un desencadenador de hora

-   Crea un nuevo [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). El segundo parámetro, *OneShot*, especifica si la tarea en segundo plano se ejecutará solo una vez o seguirá ejecutándose periódicamente. Si *OneShot* se establece en True, el primer parámetro (*FreshnessTime*) especifica el número de minutos que deben esperarse antes de programar la tarea en segundo plano. Si el elemento *OneShot* se establece en False, el elemento *FreshnessTime* especifica la frecuencia con la que se ejecutará la tarea en segundo plano.

    El temporizador integrado de las aplicaciones para la Plataforma universal de Windows (UWP) destinadas a la familia de dispositivos de escritorio o móviles ejecuta tareas en segundo plano en intervalos de 15 minutos.

    -   Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor True, la tarea se programará para ejecutarse una vez entre 15 y 30 minutos después de que se registre. Si se establece a 25 minutos y el elemento *OneShot* tiene el valor True, la tarea se programará para ejecutarse una vez entre 25 y 40 minutos después de que se registre.

    -   Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor False, la tarea se programará para ejecutarse cada 15 minutos comenzando entre el minuto 15 y el 30 desde el momento en que se registre. Si se ha establecido en n minutos y el elemento *OneShot* tiene el valor False, la tarea se programará para ejecutarse cada n minutos comenzando entre el minuto n y n + 15 después de registrarse.

    **Nota** Si *FreshnessTime* se establece en menos de 15 minutos, se iniciará una excepción cuando se intente registrar la tarea en segundo plano.
 

    Por ejemplo, este desencadenador hará que una tarea en segundo plano se ejecute una vez cada hora:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## <a name="optional-add-a-condition"></a>(Opcional) Agregar una condición

-   Si es necesario, crea una condición a la tarea en segundo plano para controlar cuándo debe ejecutarse la tarea. Una condición evita que tu tarea en segundo plano se ejecute hasta que no se satisfaga la condición; para obtener más información, consulta [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

    En este ejemplo, la condición se establece en **UserPresent** de forma que, al desencadenarse, la tarea solo se ejecuta cuando el usuario está activo. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  <a name="call-requestaccessasync"></a>Llamar a RequestAccessAsync()

-   Antes de intentar registrar la tarea en segundo plano [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), llama a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

-   Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano, consulta [Registrar una tarea en segundo plano](register-a-background-task.md).

> [!Important]
> Para las tareas en segundo plano que se ejecutan en el mismo proceso que la aplicación, no establezcas `entryPoint` Para tareas en segundo plano que se ejecutan en un proceso independiente de la aplicación, establece `entryPoint` para que sea el espacio de nombres, '.', y el nombre de la clase que contiene la implementación de la tarea en segundo plano.

    The following code registers a background task that runs out-of-process:

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

    > **Note**  Background task registration parameters are validated at the time of registration. An error is returned if any of the registration parameters are invalid. Ensure that your app gracefully handles scenarios where background task registration fails - if instead your app depends on having a valid registration object after attempting to register a task, it may crash.


## <a name="remarks"></a>Observaciones

> **Nota** A partir de Windows 10, ya no es necesario que el usuario agregue tu aplicación a la pantalla de bloqueo para poder usar las tareas en segundo plano. Para obtener una guía sobre los tipos de desencadenadores de tarea en segundo plano, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

> **Nota** Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso.](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones de la Tienda Windows (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Dec16_HO2-->



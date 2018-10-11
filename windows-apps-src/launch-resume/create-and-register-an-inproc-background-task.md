---
author: TylerMSFT
title: Crear y registrar una tarea en segundo plano dentro del proceso
description: Crea y registra una tarea dentro del proceso que se ejecuta en el mismo proceso que la aplicación en primer plano.
ms.author: twhitney
ms.date: 11/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarea en segundo plano
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 5879977662dc2bd609d09e5fe53fc2a2f0b9180f
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "4535700"
---
# <a name="create-and-register-an-in-process-background-task"></a>Crear y registrar una tarea en segundo plano dentro de proceso

**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Este tema muestra cómo crear y registrar una tarea en segundo plano que se ejecuta en el mismo proceso que la aplicación.

Las tareas en segundo plano dentro del proceso son más fáciles de implementar que las tareas en segundo plano fuera del proceso. No obstante, son menos resistentes. Si se bloquea el código que se ejecuta en una tarea en segundo plano dentro del proceso, la aplicación también se bloqueará. Ten en cuenta también que [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) y **IoTStartupTask** no pueden usarse con el modelo dentro del proceso. Activar una tarea en segundo plano de VoIP dentro de la aplicación tampoco es posible. Estas tareas y los desencadenadores aún son compatibles con el modelo de tarea en segundo plano fuera del proceso.

Ten en cuenta que la actividad en segundo plano puede finalizarse incluso cuando se ejecuta dentro del proceso en primer plano de la aplicación si se ejecuta más allá de los límites de tiempo de ejecución. Para algunos fines sigue siendo útil la resistencia de separar el trabajo en una tarea en segundo plano que se ejecuta en un proceso independiente. Mantener el trabajo en segundo plano como una tarea independiente de la aplicación en primer plano puede ser la mejor opción para el trabajo que no requiere la comunicación con la aplicación en primer plano.

## <a name="fundamentals"></a>Conceptos básicos

El modelo dentro del proceso mejora el ciclo de vida de la aplicación con mejores notificaciones para cuando la aplicación está en primer plano o en segundo plano. Dos nuevos eventos están disponibles en el objeto de aplicación de estas transiciones: [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) y [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Estos eventos encajan en el ciclo de vida de la aplicación en función del estado de visibilidad de la aplicación. Obtén más información sobre estos eventos y cómo afectan al ciclo de vida de la aplicación en [Ciclo de vida de la aplicación](app-lifecycle.md).

En un nivel alto, controlarás el evento **EnteredBackground** para ejecutar el código que se ejecutará mientras la aplicación se ejecuta en segundo plano y controlarás **LeavingBackground** para saber cuándo tu aplicación se ha desplazado a un primer plano.

## <a name="register-your-background-task-trigger"></a>Registrar el desencadenador de tareas en segundo plano

La actividad en segundo plano dentro del proceso se registra de forma similar a la actividad en segundo plano fuera del proceso. Todos los desencadenadores en segundo plano empiezan con el registro con el uso de [BackgroundTaskBuilder](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.aspx?f=255&MSPPError=-2147217396). El ensamblador facilita el registro de una tarea en segundo plano mediante la configuración de todos los valores necesarios en un solo lugar:

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.
> Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

Para actividades en segundo plano dentro del proceso no se configura `TaskEntryPoint.` Dejarlo en blanco habilita el punto de entrada predeterminado, un nuevo método protegido en el objeto de la aplicación denominado [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Una vez que se registra un desencadenador, se activará en función del tipo de desencadenador establecido en el método [SetTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.settrigger.aspx). En el ejemplo anterior se utiliza un [TimeTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.timetrigger.aspx), que se activará quince minutos desde el momento en que se registró.

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>Adición de una condición para controlar cuándo se ejecutará la tarea (opcional)

Puedes agregar una condición para controlar cuándo se ejecutará la tarea después de que se produzca el evento del desencadenador. Por ejemplo, si no quieres que la tarea se ejecute hasta que el usuario esté presente, usa la condición **UserPresent**. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

El siguiente código de muestra asigna una condición que requiere que el usuario esté presente:

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>Colocación del código de la actividad en segundo plano en OnBackgroundActivated()

Coloca el código de la actividad en segundo plano en [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) para responder al desencadenador en segundo plano cuando se desencadena. **OnBackgroundActivated** puede tratarse solo como [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396). El método tiene un parámetro de [BackgroundActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.backgroundactivatedeventargs.aspx) , que todo lo que ofrece el método **Run** contiene. Por ejemplo, en App.xaml.cs:

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

Para obtener un ejemplo de **OnBackgroundActivated** más enriquecido, vea [convertir un servicio de aplicación para ejecutarse en el mismo proceso que su aplicación host](convert-app-service-in-process.md).

## <a name="handle-background-task-progress-and-completion"></a>Administración del progreso y la finalización de tareas en segundo plano

La finalización y el progreso de la tarea puede supervisarse del mismo modo que para las tareas en segundo plano en múltiples procesos (consulta [Supervisión del progreso y la finalización de las tareas en segundo plano](monitor-background-task-progress-and-completion.md)), pero probablemente verás que puedes rastrearlas más fácilmente mediante el uso de variables para realizar un seguimiento del estado de progreso o finalización en tu aplicación. Esta es una de las ventajas de ejecutar el código de actividad en segundo plano en el mismo proceso que la aplicación.

## <a name="handle-background-task-cancellation"></a>Administrar la cancelación de tareas en segundo plano

Las tareas en segundo plano dentro del proceso se cancelarán de la misma manera que las tareas en segundo plano fuera del proceso (consulta [Administrar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)). Ten en cuenta que el controlador de eventos **BackgroundActivated** debe salir antes de que se produzca la cancelación o finalice todo el proceso. Si tu aplicación en primer plano se cierra inesperadamente al cancelar la tarea en segundo plano, comprueba que el controlador salió antes de que se produjera la cancelación.

## <a name="the-manifest"></a>El manifiesto

A diferencia de las tareas en segundo plano fuera del proceso, no es necesario agregar información de la tarea en segundo plano al manifiesto del paquete para ejecutar tareas en segundo plano dentro del proceso.

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Ahora debes comprender los conceptos básicos de cómo escribir una tarea en segundo plano dentro del proceso.

Consulta los siguientes temas relacionados para obtener referencia de las API, una guía conceptual sobre tareas en segundo plano e instrucciones más detalladas para escribir aplicaciones que usan tareas en segundo plano.

## <a name="related-topics"></a>Temas relacionados

**Temas con instrucciones detalladas sobre las tareas en segundo plano**

* [Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso](convert-out-of-process-background-task.md)
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Reproducir elementos multimedia en segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registro de una tarea en segundo plano](register-a-background-task.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)

**Guía de tareas en segundo plano**

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Referencia de API de tareas en segundo plano**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

---
author: TylerMSFT
title: Directrices para tareas en segundo plano
description: Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tareas en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 7adfecbe216dce25d0f80eb3ef1f528196299db4
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "2816335"
---
# <a name="guidelines-for-background-tasks"></a>Directrices para tareas en segundo plano


Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano.

## <a name="background-task-guidance"></a>Guía de tareas en segundo plano

Ten en cuenta las indicaciones siguientes a la hora de desarrollar la tarea en segundo plano y antes de publicar una aplicación.

Si usas una tarea en segundo plano para reproducir contenido multimedia en segundo plano, consulta [Reproducir elementos multimedia en segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obtener información sobre las mejoras en Windows 10, versión 1607, que lo hacen mucho más fácil.

**Tareas en segundo plano dentro de proceso frente a aquellas fuera de proceso:** Windows 10, versión 1607, incluye [tareas en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) que te permiten ejecutar código en segundo plano en el mismo proceso que la aplicación en primer plano. Ten en cuenta estos factores a la hora de decidir si quieres tareas en segundo plano dentro o fuera de proceso:

|Consideración | Impacto |
|--------------|--------|
|Resistencia   | Si el proceso en segundo plano se ejecuta en otro proceso y se produce un bloqueo en el proceso en segundo plano, este no provocará un error en la aplicación en primer plano. Además, la actividad en segundo plano puede finalizar, incluso dentro de la aplicación, si se ejecuta más allá de los límites de tiempo de ejecución. Separar el trabajo en segundo plano en una tarea independiente de la aplicación en primer plano puede ser una mejor elección cuando no es necesario que los procesos de primer y segundo plano se comuniquen entre sí (porque una de las principales ventajas de las tareas en segundo plano dentro de proceso es que acaban con la necesidad de comunicación entre procesos). |
|Simplicidad    | Las tareas en segundo plano dentro de proceso no requieren la comunicación entre procesos y son menos complejas de escribir.  |
|Desencadenadores disponibles | Las tareas en segundo plano dentro de proceso no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**. |
|VoIP | Las tareas en segundo plano dentro de proceso no admiten la activación de una tarea en segundo plano VoIP dentro de la aplicación. |  

**Límites en el número de instancias de desencadenador:** hay límites respecto al número de instancias de algunos desencadenadores que puede registrar una aplicación. Solo puedes registrar [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) y [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396) una vez por instancia de la aplicación. Si una aplicación supera este límite, el registro iniciará una excepción.

**Cuota de CPU:** las tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que obtienen según el tipo de desencadenador. La mayoría de los desencadenadores están limitados a 30 segundos de uso de reloj, aunque algunos tienen la capacidad de ejecutarse hasta 10 minutos para completar tareas intensivas. Las tareas en segundo plano deben ser ligeras para ahorrar batería y proporcionar una mejor experiencia de usuario para las aplicaciones en primer plano. Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

**Administración de tareas en segundo plano:** la aplicación debería obtener una lista de las tareas en segundo plano registradas, registrarse para controladores de progreso y finalización, y controlar dichos eventos de forma adecuada. Tus clases de tareas en segundo plano deben informar del progreso, la cancelación y la finalización. Para obtener más información, consulta [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md) y [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md).

**Usa [BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499):** si la clase de la tarea en segundo plano ejecuta código asincrónico, asegúrate de usar aplazamientos. De lo contrario, la tarea en segundo plano puede finalizar de forma prematura cuando lo haga el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) (o el método [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) en el caso de las tareas en segundo plano dentro de proceso). Para obtener más información, consulta [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md).

Como alternativa, solicita un solo aplazamiento y usa **async/await** para completar las llamadas a métodos asincrónicos. Cierra el aplazamiento después de las llamadas al método **await**.

**Actualización del manifiesto de la aplicación:** en el caso de las tareas que se ejecutan fuera de proceso, declara todas las tareas en segundo plano en el manifiesto de la aplicación, junto con el tipo de desencadenadores con que se usan. En caso contrario, tu aplicación no podrá registrar la tarea en segundo plano en tiempo de ejecución.

Si tienes varias tareas en segundo plano, piensa si deberían ejecutarse en el mismo proceso de host o dividirse en diferentes procesos de host. Colócalas en procesos de host independientes si te preocupa que un error de una tarea en segundo plano pudiera eliminar otras tareas en segundo plano.  Usa la entrada **Grupo de recursos** en el diseñador de manifiestos para agrupar tareas en segundo plano en procesos de host diferentes. 

Para establecer el **Grupo de recursos**, abre el diseñador Package.appxmanifest, elige **Declaraciones** y agrega una declaración de **Servicio de aplicaciones**:

![Configuración del grupo de recursos](images/resourcegroup.png)

Consulta la [referencia del esquema de aplicación](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) para obtener más información acerca de la configuración del grupo de recursos.

Las tareas en segundo plano que se ejecutan en el mismo proceso que la aplicación en primer plano no necesitan declararse a sí mismas en el manifiesto de la aplicación. Para obtener más información sobre cómo declarar tareas en segundo plano que se ejecutan fuera de proceso en el manifiesto, consulta [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md).

**Preparación para actualizaciones de la aplicación:** si la aplicación se va a actualizar, crea y registra una tarea en segundo plano **ServicingComplete** (consulta [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) para anular el registro de tareas en segundo plano para las versiones anteriores de la aplicación y registrar las tareas en segundo plano para la nueva versión. También es un momento adecuado para realizar actualizaciones de aplicaciones que puedan ser necesarias fuera del contexto de ejecución en primer plano.

**Solicitar la ejecución de tareas en segundo plano:**

> **Importante:**  a partir de Windows 10, ya no es necesario que las aplicaciones estén en la pantalla de bloqueo como un requisito previo para ejecutar tareas en segundo plano.

Todas las aplicaciones para la Plataforma universal de Windows (UWP) pueden ejecutar tipos de tareas admitidos sin necesidad de que se anclen en la pantalla de bloqueo. Sin embargo, las aplicaciones deben llamar al método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar ningún tipo de tarea en segundo plano. Este método devolverá el objeto [**BackgroundAccessStatus.DeniedByUser**](https://msdn.microsoft.com/library/windows/apps/hh700439) si el usuario denegó explícitamente los permisos de tareas de segundo plano de la aplicación en la configuración del dispositivo. Para obtener más información sobre la elección del usuario sobre la actividad en segundo plano y el ahorro de batería, consulta [Optimizar la actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity). 
## <a name="background-task-checklist"></a>Lista de comprobación de tareas en segundo plano

*Se aplica tanto a las tareas en segundo plano tanto dentro como fuera de proceso.*

-   Asocia tu tarea en segundo plano con el desencadenador correcto.
-   Agrega condiciones para asegurarte de que tu tarea en segundo plano se ejecuta correctamente.
-   Controla el progreso, la finalización y la cancelación de las tareas en segundo plano.
-   Vuelve a registrar las tareas en segundo plano durante el inicio de la aplicación. Esto garantiza su registro la primera vez que se inicie la aplicación. Además, proporciona una manera de detectar si el usuario deshabilitó las funcionalidades de ejecución en segundo plano de la aplicación (en caso de error en el registro del evento).
-   Comprueba si hay errores de registro de tareas en segundo plano. Si procede, intenta volver a registrar la tarea en segundo plano con otros valores de parámetros.
-   Para todas las familias de dispositivos excepto la de equipos de escritorio, si el dispositivo dispone de poca memoria, las tareas en segundo plano podrían finalizarse. Si no se expone una excepción de falta de memoria o la aplicación no la controla, la tarea en segundo plano finalizará sin que se muestre ninguna advertencia y sin que se genere el evento OnCanceled. Esto contribuye a garantizar la experiencia del usuario de la aplicación en primer plano. La tarea en segundo plano debe estar diseñada para controlar este escenario.

*Se aplica solo a las tareas en segundo plano fuera de proceso.*

-   Crea la tarea en segundo plano en un componente de Windows Runtime.
-   No muestres opciones de la interfaz de usuario que no sean notificaciones del sistema, iconos o actualizaciones de distintivo procedentes de la tarea en segundo plano.
-   En el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), solicita aplazamientos para todas las llamadas a métodos asincrónicos y ciérralos cuando el método haya terminado. O bien, usa un aplazamiento con **async/await**.
-   Usa almacenamiento persistente para compartir datos entre la tarea en segundo plano y la aplicación.
-   Declara todas las tareas en segundo plano en el manifiesto de la aplicación, junto con el tipo de desencadenador con el que se usan. Asegúrate de que el punto de entrada y los tipos de desencadenadores son correctos.
-   No especifiques un elemento Executable en el manifiesto a menos que estés usando un desencadenador que debería ejecutarse en el mismo contexto que la aplicación (como, por ejemplo, el elemento [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).

*Se aplica solo a las tareas en segundo plano dentro de proceso.*

- Al cancelar una tarea, asegúrate de que el controlador de eventos `BackgroundActivated` existe antes de que se produzca la cancelación o la finalización de todo el proceso.
-   Escribe tareas en segundo plano de corta duración. Las tareas en segundo plano se limitan a 30 segundos de uso.
-   No confíes en la interacción con el usuario en las tareas en segundo plano.

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso.](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Reproducir elementos multimedia en segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 

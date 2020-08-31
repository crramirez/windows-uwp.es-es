---
title: Directrices para tareas en segundo plano
description: Vea instrucciones detalladas sobre cómo desarrollar y ejecutar tareas en segundo plano en proceso y fuera de proceso en la aplicación.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: bdcf398b448a3b0571b07063b9d4e70800259248
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094592"
---
# <a name="guidelines-for-background-tasks"></a>Directrices para tareas en segundo plano


Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano.

## <a name="background-task-guidance"></a>Guía de tareas en segundo plano

Ten en cuenta las indicaciones siguientes a la hora de desarrollar la tarea en segundo plano y antes de publicar una aplicación.

Si usas una tarea en segundo plano para reproducir contenido multimedia en segundo plano, consulta [Reproducir elementos multimedia en segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obtener información sobre las mejoras en Windows 10, versión 1607, que lo hacen mucho más fácil.

**Tareas en segundo plano dentro de proceso frente a aquellas fuera de proceso:** Windows 10, versión 1607, incluye [tareas en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) que te permiten ejecutar código en segundo plano en el mismo proceso que la aplicación en primer plano. Ten en cuenta estos factores a la hora de decidir si quieres tareas en segundo plano dentro o fuera de proceso:

|Consideración | Impacto |
|--------------|--------|
|Resistencia   | Si el proceso en segundo plano se ejecuta en otro proceso y se produce un bloqueo en el proceso en segundo plano, este no provocará un error en la aplicación en primer plano. Además, la actividad en segundo plano puede finalizar, incluso dentro de la aplicación, si se ejecuta más allá de los límites de tiempo de ejecución. Separar el trabajo en segundo plano en una tarea independiente de la aplicación en primer plano puede ser una mejor elección cuando no es necesario que los procesos de primer y segundo plano se comuniquen entre sí (porque una de las principales ventajas de las tareas en segundo plano dentro de proceso es que acaban con la necesidad de comunicación entre procesos). |
|Simplicidad    | Las tareas en segundo plano dentro de proceso no requieren la comunicación entre procesos y son menos complejas de escribir.  |
|Desencadenadores disponibles | Las tareas en segundo plano dentro de proceso no admiten los siguientes desencadenadores: [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) e **IoTStartupTask**. |
|VoIP | Las tareas en segundo plano dentro de proceso no admiten la activación de una tarea en segundo plano VoIP dentro de la aplicación. |  

**Límites en el número de instancias de desencadenador:** Hay límites en el número de instancias de algunos desencadenadores que puede registrar una aplicación. Una aplicación solo puede registrar   [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) y [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) una vez por cada instancia de la aplicación. Si una aplicación supera este límite, el registro producirá una excepción.

**Cuotas de CPU:** Las tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que se basa en el tipo de desencadenador. La mayoría de los desencadenadores están limitados a 30 segundos de uso de reloj, aunque algunos tienen la capacidad de ejecutarse hasta 10 minutos para completar tareas intensivas. Las tareas en segundo plano deben ser ligeras para ahorrar batería y proporcionar una mejor experiencia de usuario para las aplicaciones en primer plano. Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

**Administrar tareas en segundo plano:** La aplicación debe obtener una lista de tareas en segundo plano registradas, registrarse para los controladores de progreso y finalización, y controlar dichos eventos de forma adecuada. Tus clases de tareas en segundo plano deben informar del progreso, la cancelación y la finalización. Para obtener más información, consulte [administrar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)y [supervisar el progreso y la finalización de las tareas en segundo plano](monitor-background-task-progress-and-completion.md).

**Usar [BackgroundTaskDeferral](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral):** Si la clase de tarea en segundo plano ejecuta código asincrónico, asegúrese de usar aplazamientos. De lo contrario, es posible que la tarea en segundo plano finalice prematuramente cuando el método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) devuelva (o el método [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated) en el caso de tareas en segundo plano en proceso). Para obtener más información, consulta [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md).

Como alternativa, solicita un solo aplazamiento y usa **async/await** para completar las llamadas a métodos asincrónicos. Cierra el aplazamiento después de las llamadas al método **await**.

**Actualización del manifiesto de la aplicación:** en el caso de las tareas que se ejecutan fuera de proceso, declara todas las tareas en segundo plano en el manifiesto de la aplicación, junto con el tipo de desencadenadores con que se usan. En caso contrario, tu aplicación no podrá registrar la tarea en segundo plano en tiempo de ejecución.

Si tiene varias tareas en segundo plano, considere si deben ejecutarse en el mismo proceso de host o separarse en procesos de host diferentes. Colóquelos en procesos de host independientes si le preocupa que un error en una tarea en segundo plano desactive otras tareas en segundo plano.  Use la entrada del **grupo de recursos** en el diseñador de manifiestos para agrupar las tareas en segundo plano en procesos de host diferentes. 

Para establecer el **grupo de recursos**, abra el diseñador package. appxmanifest, elija **declaraciones**y agregue una declaración de **App Service** :

![Configuración del grupo de recursos](images/resourcegroup.png)

Vea la [Referencia del esquema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) de la aplicación para obtener más información sobre la configuración del grupo de recursos.

Las tareas en segundo plano que se ejecutan en el mismo proceso que la aplicación en primer plano no necesitan declararse a sí mismas en el manifiesto de la aplicación. Para obtener más información sobre cómo declarar tareas en segundo plano que se ejecutan fuera de proceso en el manifiesto, consulta [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md).

**Preparación para actualizaciones de la aplicación:** si la aplicación se va a actualizar, crea y registra una tarea en segundo plano **ServicingComplete** (consulta [SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) para anular el registro de tareas en segundo plano para las versiones anteriores de la aplicación y registrar las tareas en segundo plano para la nueva versión. También es un momento adecuado para realizar actualizaciones de aplicaciones que puedan ser necesarias fuera del contexto de ejecución en primer plano.

**Solicitar la ejecución de tareas en segundo plano:**

> **Importante**    A partir de Windows 10, ya no es necesario que las aplicaciones estén en la pantalla de bloqueo como requisito previo para ejecutar tareas en segundo plano.

Todas las aplicaciones para la Plataforma universal de Windows (UWP) pueden ejecutar tipos de tareas admitidos sin necesidad de que se anclen en la pantalla de bloqueo. Sin embargo, las aplicaciones deben llamar a [**GetAccessState**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) y comprobar que no se deniegue la ejecución de la aplicación en segundo plano. Asegúrese de que [**GetAccessStatus**] no devuelve una de las enumeraciones de [**BackgroundAccessStatus**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) denegadas. Por ejemplo, este método devolverá ( https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus) si el usuario ha denegado explícitamente los permisos de tarea en segundo plano para la aplicación en la configuración del dispositivo.

Si se deniega la ejecución de la aplicación en segundo plano, la aplicación debe llamar a [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) y asegurarse de que la respuesta no se deniega antes de registrar las tareas en segundo plano.

Para obtener más información sobre la elección del usuario alrededor de la actividad en segundo plano y el ahorro de batería, consulte [optimizar la actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity). 
## <a name="background-task-checklist"></a>Lista de comprobación de tareas en segundo plano

*Se aplica tanto a las tareas en segundo plano tanto dentro como fuera de proceso.*

-   Asocia tu tarea en segundo plano con el desencadenador correcto.
-   Agrega condiciones para asegurarte de que tu tarea en segundo plano se ejecuta correctamente.
-   Controla el progreso, la finalización y la cancelación de las tareas en segundo plano.
-   Vuelve a registrar las tareas en segundo plano durante el inicio de la aplicación. Esto garantiza su registro la primera vez que se inicie la aplicación. Además, proporciona una manera de detectar si el usuario deshabilitó las funcionalidades de ejecución en segundo plano de la aplicación (en caso de error en el registro del evento).
-   Comprueba si hay errores de registro de tareas en segundo plano. Si procede, intenta volver a registrar la tarea en segundo plano con otros valores de parámetros.
-   Para todas las familias de dispositivos excepto la de equipos de escritorio, si el dispositivo dispone de poca memoria, las tareas en segundo plano podrían finalizarse. Si no se expone una excepción de falta de memoria o la aplicación no la controla, la tarea en segundo plano finalizará sin que se muestre ninguna advertencia y sin que se genere el evento OnCanceled. Esto contribuye a garantizar la experiencia del usuario de la aplicación en primer plano. La tarea en segundo plano debe estar diseñada para controlar este escenario.

*Se aplica solo a las tareas en segundo plano fuera de proceso.*

-   Cree su tarea en segundo plano en un componente de Windows Runtime.
-   No muestres opciones de la interfaz de usuario que no sean notificaciones del sistema, iconos o actualizaciones de distintivo procedentes de la tarea en segundo plano.
-   En el método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run), solicita aplazamientos para todas las llamadas a métodos asincrónicos y ciérralos cuando el método haya terminado. O bien, use un aplazamiento con **Async/Await**.
-   Usa almacenamiento persistente para compartir datos entre la tarea en segundo plano y la aplicación.
-   Declara todas las tareas en segundo plano en el manifiesto de la aplicación, junto con el tipo de desencadenador con el que se usan. Asegúrate de que el punto de entrada y los tipos de desencadenadores son correctos.
-   No especifique un elemento ejecutable en el manifiesto a menos que esté usando un desencadenador que se debe ejecutar en el mismo contexto que la aplicación (por ejemplo, la [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)).

*Se aplica solo a las tareas en segundo plano dentro de proceso.*

- Al cancelar una tarea, asegúrate de que el controlador de eventos `BackgroundActivated` existe antes de que se produzca la cancelación o la finalización de todo el proceso.
-   Escribe tareas en segundo plano de corta duración. Las tareas en segundo plano se limitan a 30 segundos de uso.
-   No confíes en la interacción con el usuario en las tareas en segundo plano.

## <a name="related-topics"></a>Temas relacionados

* [Cree y registre una tarea en segundo plano en proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Reproducir elementos multimedia en segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones UWP (durante la depuración)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

 

 

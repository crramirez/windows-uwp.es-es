---
title: Hacer que tu aplicación sea compatible con las tareas en segundo plano
description: Los temas de esta sección muestran cómo hacer que un código ligero se ejecute en segundo plano en respuesta a desencadenadores.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: ac3a20afc75cc7a6cf3c9f874fa26e1b6387dd89
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171849"
---
# <a name="support-your-app-with-background-tasks"></a>Hacer que tu aplicación sea compatible con las tareas en segundo plano


Los temas de esta sección muestran cómo hacer que un código ligero se ejecute en segundo plano en respuesta a desencadenadores. Puedes usar tareas en segundo plano para proporcionar funcionalidad cuando la aplicación esté suspendida o no se esté ejecutando. También puedes usar tareas en segundo plano para aplicaciones de comunicación en tiempo real como VOIP, correo y mensajería instantánea.

## <a name="playing-media-in-the-background"></a>Reproducir elementos multimedia en segundo plano

A partir de la versión 1607 de Windows 10, la reproducción de audio en segundo plano es mucho más fácil. Consulta [Reproducir elementos multimedia en segundo plano](../audio-video-camera/background-audio.md).

## <a name="in-process-and-out-of-process-background-tasks"></a>Tareas en segundo plano dentro y fuera de proceso

Existen dos enfoques para implementar tareas en segundo plano:

* En proceso: la aplicación y su proceso en segundo plano se ejecutan en el mismo proceso.
* Fuera de proceso: la aplicación y el proceso en segundo plano se ejecutan en procesos independientes.

Con la versión 1607 de Windows 10, se introdujo la admisión del segundo plano en proceso para simplificar la escritura de las tareas en segundo plano. Pero todavía se pueden escribir tareas fuera del proceso en segundo plano. Consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md) para ver recomendaciones sobre cuándo escribir una tarea en segundo plano en proceso o fuera de proceso.

Las tareas en segundo plano fuera de proceso son más resistentes porque el proceso en segundo plano no puede reducir el proceso de la aplicación si algo va mal. Sin embargo, la resistencia tiene el precio de mayor complejidad para administrar la comunicación entre procesos entre la aplicación y la tarea en segundo plano.

Las tareas en segundo plano fuera de proceso se implementan como clases ligeras que implementan la interfaz [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) que el sistema operativo ejecuta en un proceso independiente (backgroundtaskhost.exe). Registre una tarea en segundo plano mediante la clase [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) . El nombre de la clase se especifica como punto de entrada al registrar la tarea en segundo plano.

Con la versión 1607 de Windows 10, puedes habilitar la actividad en segundo plano sin tener que crear una tarea en segundo plano. En su lugar, puede ejecutar el código de fondo directamente dentro del proceso de la aplicación de primer plano.

Para comenzar rápidamente con las tareas en segundo plano dentro de proceso, consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md).

Para comenzar rápidamente con las tareas en segundo plano fuera de proceso, consulta [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md).

> [!TIP]
> A partir de Windows 10, ya no necesitarás colocar una aplicación en la pantalla de bloqueo como requisito previo para registrarle una tarea en segundo plano.

## <a name="background-tasks-for-system-events"></a>Tareas en segundo plano para eventos del sistema

Puedes hacer que tu aplicación responda a eventos generados por el sistema registrando una tarea en segundo plano con la clase [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger). Una aplicación puede usar cualquiera de los siguientes desencadenadores de eventos del sistema (definidos en [**SystemTriggerType**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType))

| Nombre del desencadenador                     | Descripción                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | Internet pasa a estar disponible.                                                                                |
| **NetworkStateChange**           | Se produce un cambio en la red, como un cambio en el costo o la conectividad.                                              |
| **OnlineIdConnectedStateChange** | Cambia el identificador en línea asociado a la cuenta.                                                                 |
| **SmsReceived**                  | Se ha recibido un nuevo mensaje SMS por un dispositivo instalado de banda ancha móvil.                                         |
| **TimeZoneChange**               | La zona horaria cambia en el dispositivo (por ejemplo, cuando el sistema ajusta el reloj al horario de verano). |

Para obtener más información, consulte [responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md).

## <a name="conditions-for-background-tasks"></a>Condiciones para tareas en segundo plano

Puedes controlar cuándo se ejecuta la tarea en segundo plano, incluso después de que se desencadene, agregando una condición. Una vez desencadenada, la tarea en segundo plano no se ejecutará hasta que se cumplan todas las condiciones. Se pueden usar las siguientes condiciones (representadas por la enumeración [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)).

| Nombre de condición           | Descripción                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | Internet debe estar disponible.   |
| **InternetNotAvailable** | Internet no debe estar disponible. |
| **SessionConnected**     | La sesión debe estar conectada.    |
| **SessionDisconnected**  | La sesión no debe estar conectada. |
| **UserNotPresent**       | El usuario debe estar ausente.            |
| **UserPresent**          | El usuario debe estar presente.         |

Agregue la condición **InternetAvailable** a su tarea en segundo plano [BackgroundTaskBuilder.AddCondition](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para retrasar la activación de la tarea en segundo plano hasta que la pila de red se ejecute. Esta condición ahorra energía porque la tarea en segundo plano no se ejecutará hasta que la red esté disponible. Esta condición no proporciona una activación en tiempo real.

Si la tarea en segundo plano requiere conectividad de red, establezca [IsNetworkRequested](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para asegurarse de que la red permanece activa mientras se ejecuta la tarea en segundo plano. Esto indica a la infraestructura de tareas en segundo plano que debe mantener la red mientras se esté ejecutando la tarea, incluso si el dispositivo ha entrado en modo de espera conectado. Si la tarea en segundo plano no establece **IsNetworkRequested**, la tarea en segundo plano no podrá tener acceso a la red en modo de espera conectado (por ejemplo, cuando la pantalla de un teléfono esté desactivada).  
Para obtener más información sobre las condiciones de la tarea en segundo plano, vea [establecer las condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

## <a name="application-manifest-requirements"></a>Requisitos del manifiesto de aplicación

Antes de que la aplicación pueda registrar correctamente una tarea en segundo plano que se ejecuta fuera de proceso, debe estar declarada en el manifiesto de la aplicación. Las tareas en segundo plano que se ejecutan en el mismo proceso que su aplicación de host no necesitan declararse en el manifiesto de la aplicación. Para obtener más información, consulta [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Tareas en segundo plano

Los siguientes desencadenadores en tiempo real pueden usarse para ejecutar el código personalizado ligero en segundo plano:

| Desencadenador en tiempo real  | Descripción |
|--------------------|-------------|
| **Canal de control** | Las tareas en segundo plano pueden mantener una conexión activa y recibir mensajes en el canal de control mediante la clase [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Si tu aplicación está escuchando un socket, puedes usar el agente de socket en lugar de la clase **ControlChannelTrigger**. Para más información sobre el uso del agente de socket, consulta [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger). La clase **ControlChannelTrigger** no es compatible con Windows Phone. |
| **Temporizador** | Las tareas en segundo plano se pueden ejecutar cada 15 minutos y se pueden configurar para ejecutarse a una determinada hora mediante el control [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger). Para obtener más información, consulta [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md). |
| **Notificación de inserción** | Las tareas en segundo plano responden a la clase [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) para recibir notificaciones de inserción sin procesar. |

**Nota**  

Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) antes de registrar cualquier tipo de desencadenador en segundo plano.

Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, llama a [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) y luego a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) cuando se inicia la aplicación tras su actualización. Para obtener más información, vea [instrucciones para tareas en segundo plano](guidelines-for-background-tasks.md).

**Límites en el número de instancias de desencadenador:** Hay límites en el número de instancias de algunos desencadenadores que puede registrar una aplicación. Una aplicación solo puede registrar   [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) y [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) una vez por cada instancia de la aplicación. Si una aplicación supera este límite, el registro producirá una excepción.

## <a name="system-event-triggers"></a>Desencadenadores de eventos del sistema

La enumeración [**SystemTriggerType**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) representa los siguientes desencadenadores de eventos del sistema:

| Nombre del desencadenador            | Descripción                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | La tarea en segundo plano se desencadena cuando el usuario pasa a estar presente.   |
| **UserAway**            | La tarea en segundo plano se desencadena cuando el usuario pasa a estar ausente.    |
| **ControlChannelReset** | La tarea en segundo plano se desencadena cuando se restablece un canal de control. |
| **SessionConnected**    | La tarea en segundo plano se desencadena cuando se conecta la sesión,   |

   
Los siguientes desencadenadores de eventos del sistema indican cuándo el usuario ha movido una aplicación o desactivado la pantalla de bloqueo.

| Nombre del desencadenador                     | Descripción                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | Un icono dinámico de la aplicación se agrega a la pantalla de bloqueo.     |
| **LockScreenApplicationRemoved** | Un icono dinámico de la aplicación se quita de la pantalla de bloqueo. |

 
## <a name="background-task-resource-constraints"></a>Restricciones de recursos de las tareas en segundo plano

Las tareas en segundo plano son ligeras. Mantener la ejecución en segundo plano en unos mínimos garantiza la mejor experiencia del usuario con las aplicaciones en primer plano y una mayor vida de la batería. Esto se logra aplicando restricciones de recursos a las tareas en segundo plano.

Las tareas en segundo plano se limitan a 30 segundos de uso.

### <a name="memory-constraints"></a>Restricciones de memoria

Debido a las restricciones de recursos para los dispositivos con poca memoria, las tareas en segundo plano pueden tener un límite de memoria que determina la cantidad máxima de memoria que puede usar la tarea en segundo plano. Si la tarea en segundo plano intenta realizar una operación que superaría este límite, no se podrá realizar la operación y puede que esta genere una excepción de falta de memoria que puede controlar la tarea. Si la tarea no controla la excepción de falta de memoria o la naturaleza de la operación intentada es tal que no se genera una excepción de falta de memoria, la tarea se finalizará inmediatamente.  

Puedes usar las API [**MemoryManager**](/uwp/api/Windows.System.MemoryManager) para consultar el uso de memoria actual y limitarlo para descubrir tu límite (si existe) y para supervisar el uso de memoria en curso de la tarea en segundo plano.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Límite por dispositivo para aplicaciones con tareas en segundo plano para dispositivos de baja memoria

En los dispositivos con restricciones de memoria, existe un límite en el número de aplicaciones que se pueden instalar en un dispositivo y en el uso de tareas en segundo plano en cualquier momento. Si se supera este número, no se podrá realizar la llamada a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync), que es necesaria para registrar todas las tareas en segundo plano.

### <a name="battery-saver"></a>Ahorro de batería

A menos que permitas que tu aplicación siga ejecutando tareas en segundo plano y recibiendo notificaciones de inserción cuando el Ahorro de batería está activado, cuando se habilite la característica Ahorro de batería, esta evitará la ejecución de las tareas en segundo plano cuando el dispositivo no esté conectado a alimentación externa y la batería tenga un nivel de carga restante inferior al especificado. Esto no impide que puedas registrar tareas en segundo plano.

Sin embargo, en el caso de las aplicaciones empresariales y las aplicaciones que no se publicarán en el Microsoft Store, consulte [ejecutar en segundo plano indefinidamente](run-in-the-background-indefinetly.md) para obtener información sobre cómo usar una función para ejecutar una tarea en segundo plano o una sesión de ejecución extendida en segundo plano indefinidamente.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>Los recursos de tareas en segundo plano garantizan la comunicación en tiempo real.

Para evitar que las cuotas de recursos interfieran con la funcionalidad de comunicación en tiempo real, las tareas en segundo plano que usan el [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) y el [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) reciben cuotas de recursos de CPU garantizadas para todas las tareas en ejecución. Las cuotas de recursos son las mencionadas anteriormente y permanecen constantes para estas tareas en segundo plano.

Tu aplicación no tiene que hacer nada de forma distinta para obtener las cuotas de recursos garantizadas para las tareas en segundo plano [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) y [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger). El sistema siempre trata estas tareas como tareas en segundo plano críticas.

## <a name="maintenance-trigger"></a>Desencadenador de mantenimiento

Las tareas de mantenimiento solo se ejecutan cuando el dispositivo está conectado a la corriente alterna. Para obtener más información, vea [usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Tareas en segundo plano para sensores y dispositivos

La aplicación puede acceder a sensores y dispositivos periféricos desde una tarea en segundo plano mediante la clase [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). Puedes usar este desencadenador para operaciones de larga duración como, por ejemplo, la sincronización o supervisión de datos. A diferencia de las tareas para eventos del sistema, una tarea **DeviceUseTrigger** solo se puede desencadenar mientras tu aplicación se está ejecutando en primer plano y no se puede establecer en ella ninguna condición.

> [!IMPORTANT]
> Las clases **DeviceUseTrigger** y **DeviceServicingTrigger** no pueden usarse con tareas en segundo plano dentro de proceso.

Algunas operaciones críticas del dispositivo, como las actualizaciones del firmware que se ejecutan durante mucho tiempo, no se pueden realizar con [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). Esas operaciones solo se pueden realizar en el equipo y solo las puede realizar una aplicación privilegiada que use [**DeviceServicingTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger). Una *aplicación privilegiada* es una aplicación que ha recibido la autorización del fabricante del dispositivo para realizar esas operaciones. Los metadatos del dispositivo se usan para especificar qué aplicación, si es el caso, se ha designado como aplicación privilegiada para un dispositivo. Para obtener más información, consulte [sincronización y actualización de dispositivos para aplicaciones de dispositivos Microsoft Store](/windows-hardware/drivers/devapps/device-sync-and-update-for-uwp-device-apps)

## <a name="managing-background-tasks"></a>Administrar tareas en segundo plano

Las tareas en segundo plano pueden notificar progreso, finalización o cancelación a tu aplicación usando eventos y almacenamiento local. La aplicación también puede capturar excepciones generadas por una tarea en segundo plano, y administrar el registro de tareas en segundo plano durante las actualizaciones de la aplicación. Para más información, consulte:

[Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)  
[Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)

Compruebe el registro de la tarea en segundo plano durante el inicio de la aplicación. Asegúrese de que las tareas en segundo plano no agrupadas de la aplicación están presentes en BackgroundTaskBuilder. AllTasks. Vuelva a registrar los que no están presentes. Anula el registro de las tareas que ya no se necesitan. Esto garantiza que todos los registros de tareas en segundo plano están actualizados cada vez que se inicia la aplicación.

## <a name="related-topics"></a>Temas relacionados

**Guía conceptual para multitarea en Windows 10**

* [Launching, resuming, and multitasking](index.md)

**Guía de tareas en segundo plano relacionadas**

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Acceder a sensores y dispositivos desde una tarea en segundo plano](access-sensors-and-devices-from-a-background-task.md)
* [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md)
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso](convert-out-of-process-background-task.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Agrupar el registro de tareas en segundo plano](group-background-tasks.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones UWP (durante la depuración)](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Reproducir elementos multimedia en segundo plano](../audio-video-camera/background-audio.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP](run-a-background-task-during-updatetask.md)
* [Ejecutar en segundo plano de manera indefinida](run-in-the-background-indefinetly.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Desencadenar una tarea en segundo plano desde la aplicación](trigger-background-task-from-app.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
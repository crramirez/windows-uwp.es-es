---
author: TylerMSFT
title: Hacer que tu aplicación sea compatible con las tareas en segundo plano
description: Los temas de esta sección muestran cómo hacer que un código ligero se ejecute en segundo plano en respuesta a desencadenadores.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.author: twhitney
ms.date: 08/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a22c08dffc3deb22978fd45051ba86ada0f64be
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "927770"
---
# <a name="support-your-app-with-background-tasks"></a>Hacer que tu aplicación sea compatible con las tareas en segundo plano


Los temas de esta sección muestran cómo hacer que un código ligero se ejecute en segundo plano en respuesta a desencadenadores. Puedes usar tareas en segundo plano para proporcionar funcionalidad cuando la aplicación esté suspendida o no se esté ejecutando. También puedes usar tareas en segundo plano para aplicaciones de comunicación en tiempo real como VOIP, correo y mensajería instantánea.

## <a name="playing-media-in-the-background"></a>Reproducir elementos multimedia en segundo plano

A partir de la versión 1607 de Windows 10, la reproducción de audio en segundo plano es mucho más fácil. Consulta [Reproducir elementos multimedia en segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio).

## <a name="in-process-and-out-of-process-background-tasks"></a>Tareas en segundo plano dentro y fuera de proceso

Existen dos enfoques para implementar tareas en segundo plano:

* En proceso: la aplicación y su proceso en segundo plano se ejecutan en el mismo proceso
* Fuera de proceso: la aplicación y el proceso en segundo plano se ejecutan en procesos independientes.

Con la versión 1607 de Windows 10, se introdujo la admisión del segundo plano en proceso para simplificar la escritura de las tareas en segundo plano. Pero todavía se pueden escribir tareas fuera del proceso en segundo plano. Consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md) para ver recomendaciones sobre cuándo escribir una tarea en segundo plano en proceso o fuera de proceso.

Tareas en segundo plano de fuera de proceso son más resistentes debido a que el proceso en segundo plano no puede acabar con el proceso de aplicación si algo vaya mal. Pero la resistencia tiene el precio de mayor complejidad para administrar la comunicación entre procesos entre la aplicación y la tarea en segundo plano.

Tareas en segundo plano de fuera de proceso se implementan como ligeras clases que implementan la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) que el sistema operativo se ejecuta en un proceso independiente (backgroundtaskhost.exe). Registrar una tarea en segundo plano mediante el uso de la clase [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) . El nombre de la clase se especifica como punto de entrada al registrar la tarea en segundo plano.

Con la versión 1607 de Windows 10, puedes habilitar la actividad en segundo plano sin tener que crear una tarea en segundo plano. En su lugar, puede ejecutar el código de fondo directamente dentro de proceso de la aplicación de primer plano.

Para comenzar rápidamente con las tareas en segundo plano dentro de proceso, consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md).

Para comenzar rápidamente con las tareas en segundo plano fuera de proceso, consulta [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md).

> [!TIP]
> A partir de Windows 10, ya no necesitarás colocar una aplicación en la pantalla de bloqueo como requisito previo para registrarle una tarea en segundo plano.

## <a name="background-tasks-for-system-events"></a>Tareas en segundo plano para eventos del sistema

Puedes hacer que tu aplicación responda a eventos generados por el sistema registrando una tarea en segundo plano con la clase [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). Una aplicación puede usar cualquiera de los siguientes desencadenadores de eventos del sistema (definidos en [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839))

| Nombre del desencadenador                     | Descripción                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | Internet pasa a estar disponible.                                                                                |
| **NetworkStateChange**           | Se produce un cambio en la red, como un cambio en el costo o la conectividad.                                              |
| **OnlineIdConnectedStateChange** | Cambia el identificador en línea asociado a la cuenta.                                                                 |
| **SmsReceived**                  | Se ha recibido un nuevo mensaje SMS por un dispositivo instalado de banda ancha móvil.                                         |
| **TimeZoneChange**               | La zona horaria cambia en el dispositivo (por ejemplo, cuando el sistema ajusta el reloj al horario de verano). |

Para obtener más información, consulta [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md).

## <a name="conditions-for-background-tasks"></a>Condiciones para tareas en segundo plano

Puedes controlar cuándo se ejecuta la tarea en segundo plano, incluso después de que se desencadene, agregando una condición. Una vez desencadenada, la tarea en segundo plano no se ejecutará hasta que se cumplan todas las condiciones. Se pueden usar las siguientes condiciones (representadas por la enumeración [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)).

| Nombre de la condición           | Descripción                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | Internet debe estar disponible.   |
| **InternetNotAvailable** | Internet no debe estar disponible. |
| **SessionConnected**     | La sesión debe estar conectada.    |
| **SessionDisconnected**  | La sesión no debe estar conectada. |
| **UserNotPresent**       | El usuario debe estar ausente.            |
| **UserPresent**          | El usuario debe estar presente.         |

Agrega la condición **InternetAvailable** a tu tarea en segundo plano [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para retrasar la activación de la tarea en segundo plano hasta que la pila de red se ejecute. Esta condición ahorra energía debido a que no se ejecutará la tarea en segundo plano hasta que la red está disponible. Esta condición no proporciona una activación en tiempo real.

Si la tarea en segundo plano requiere conectividad de red, establezca [IsNetworkRequested](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para asegurarse de que la red permanece copia de seguridad mientras se ejecuta la tarea en segundo plano. Esto indica a la infraestructura de tareas en segundo plano que debe mantener conectada la red mientras se esté ejecutando la tarea, incluso si el dispositivo ha entrado en modo de espera conectado. Si la tarea en segundo plano no establece **IsNetworkRequested**, a continuación, la tarea en segundo plano no podrá tener acceso a la red cuando conectado en modo de espera (por ejemplo, cuando se desactiva la pantalla de un teléfono.)
 
Para obtener más información acerca de las condiciones de tarea de fondo, vea [establecer las condiciones para la ejecución de una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

## <a name="application-manifest-requirements"></a>Requisitos del manifiesto de la aplicación

Antes de que la aplicación pueda registrar correctamente una tarea en segundo plano que se ejecuta fuera de proceso, debe estar declarada en el manifiesto de la aplicación. Las tareas en segundo plano que se ejecutan en el mismo proceso que su aplicación de host no necesitan declararse en el manifiesto de la aplicación. Para obtener más información, consulta [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Tareas en segundo plano

Los siguientes desencadenadores en tiempo real pueden usarse para ejecutar el código personalizado ligero en segundo plano:

| Desencadenador en tiempo real  | Descripción |
|--------------------|-------------|
| **Canal de control** | Las tareas en segundo plano pueden mantener una conexión activa y recibir mensajes en el canal de control mediante la clase [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Si tu aplicación está escuchando un socket, puedes usar el agente de socket en lugar de la clase **ControlChannelTrigger**. Para más información sobre el uso del agente de socket, consulta [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009). La clase **ControlChannelTrigger** no es compatible con Windows Phone. |
| **Temporizador** | Las tareas en segundo plano se pueden ejecutar cada 15 minutos y se pueden configurar para ejecutarse a una determinada hora mediante el control [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Para obtener más información, consulta [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md). |
| **Notificación de inserción** | Las tareas en segundo plano responden a la clase [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) para recibir notificaciones de inserción sin procesar. |

**Nota**  

Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.

Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, llama a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

**Límites en el número de instancias de desencadenador:** hay límites respecto al número de instancias de algunos desencadenadores que puede registrar una aplicación. Solo puedes registrar [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) y [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396) una vez por instancia de la aplicación. Si una aplicación supera este límite, el registro iniciará una excepción.

## <a name="system-event-triggers"></a>Desencadenadores de eventos del sistema

La enumeración [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) representa los siguientes desencadenadores de eventos del sistema:

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

Puedes usar las API [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) para consultar el uso de memoria actual y limitarlo para descubrir tu límite (si existe) y para supervisar el uso de memoria en curso de la tarea en segundo plano.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Límite por dispositivo para aplicaciones con tareas en segundo plano para dispositivos de baja memoria

En los dispositivos con restricciones de memoria, existe un límite en el número de aplicaciones que se pueden instalar en un dispositivo y en el uso de tareas en segundo plano en cualquier momento. Si se supera este número, no se podrá realizar la llamada a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485), que es necesaria para registrar todas las tareas en segundo plano.

### <a name="battery-saver"></a>Ahorro de batería

A menos que permitas que tu aplicación siga ejecutando tareas en segundo plano y recibiendo notificaciones de inserción cuando el Ahorro de batería está activado, cuando se habilite la característica Ahorro de batería, esta evitará la ejecución de las tareas en segundo plano cuando el dispositivo no esté conectado a alimentación externa y la batería tenga un nivel de carga restante inferior al especificado. Esto no impide que puedas registrar tareas en segundo plano.

Sin embargo, para aplicaciones de empresa y aplicaciones que no se publicará en el Microsoft Store, vea [ejecutar en segundo plano indefinidamente](run-in-the-background-indefinetly.md) para aprender a usar una capacidades para ejecutar una tarea en segundo plano o sesión de ejecución extendida en segundo plano indefinidamente.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>Los recursos de tareas en segundo plano garantizan la comunicación en tiempo real.

Para evitar que las cuotas de recursos interfieran con la funcionalidad de comunicación en tiempo real, las tareas en segundo plano que usan el [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) y el [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) reciben cuotas de recursos de CPU garantizadas para todas las tareas en ejecución. Las cuotas de recursos son las mencionadas anteriormente y permanecen constantes para estas tareas en segundo plano.

Tu aplicación no tiene que hacer nada de forma distinta para obtener las cuotas de recursos garantizadas para las tareas en segundo plano [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) y [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543). El sistema siempre trata estas tareas como tareas en segundo plano críticas.

## <a name="maintenance-trigger"></a>Desencadenador de mantenimiento

Las tareas de mantenimiento solo se ejecutan cuando el dispositivo está conectado a la corriente alterna. Para obtener más información, consulta [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Tareas en segundo plano para sensores y dispositivos

La aplicación puede acceder a sensores y dispositivos periféricos desde una tarea en segundo plano mediante la clase [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Puedes usar este desencadenador para operaciones de larga duración como, por ejemplo, la sincronización o supervisión de datos. A diferencia de las tareas para eventos del sistema, una tarea **DeviceUseTrigger** solo se puede desencadenar mientras tu aplicación se está ejecutando en primer plano y no se puede establecer en ella ninguna condición.

> [!IMPORTANT]
> Las clases **DeviceUseTrigger** y **DeviceServicingTrigger** no pueden usarse con tareas en segundo plano dentro de proceso.

Algunas operaciones críticas del dispositivo, como las actualizaciones del firmware que se ejecutan durante mucho tiempo, no se pueden realizar con [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Esas operaciones solo se pueden realizar en el equipo y solo las puede realizar una aplicación privilegiada que use [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). Una *aplicación privilegiada* es una aplicación que ha recibido la autorización del fabricante del dispositivo para realizar esas operaciones. Los metadatos del dispositivo se usan para especificar qué aplicación, si es el caso, se ha designado como aplicación privilegiada para un dispositivo. Para obtener más información, consulte [sincronización de dispositivo y la actualización de aplicaciones de dispositivo de almacenamiento de Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=306619)

## <a name="managing-background-tasks"></a>Administrar tareas en segundo plano

Las tareas en segundo plano pueden notificar progreso, finalización o cancelación a tu aplicación usando eventos y almacenamiento local. La aplicación también puede capturar excepciones generadas por una tarea en segundo plano, y administrar el registro de tareas en segundo plano durante las actualizaciones de la aplicación. Si quieres obtener más información, consulta:

[Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)  
[Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)

Compruebe el registro de la tarea de fondo durante el inicio de la aplicación. Asegúrese de que las tareas de fondo desagruparse de la aplicación estén presentes en BackgroundTaskBuilder.AllTasks. Vuelva a registrar los que no están presentes. Anular el registro de las tareas que ya no son necesarias. Esto garantiza que todos los registros de tareas de fondo están actualizados cada vez que se inicia la aplicación.

## <a name="related-topics"></a>Temas relacionados

**Guía conceptual para multitarea en Windows 10**

* [Iniciar, reanudar y multitarea](index.md)

**Guía de tareas en segundo plano relacionadas**

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Acceder a sensores y dispositivos desde una tarea en segundo plano](access-sensors-and-devices-from-a-background-task.md)
* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md)
* [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md)
* [Convertir una tarea en segundo plano fuera del proceso en una tarea en segundo plano dentro del proceso](convert-out-of-process-background-task.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Agrupar registro de tarea en segundo plano](group-background-tasks.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Reproducir elementos multimedia en segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Ejecutar una tarea en segundo plano cuando se actualice la aplicación para UWP](run-a-background-task-during-updatetask.md)
* [Ejecutar en segundo plano de manera indefinida](run-in-the-background-indefinetly.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Desencadenar una tarea en segundo plano desde la aplicación](trigger-background-task-from-app.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)

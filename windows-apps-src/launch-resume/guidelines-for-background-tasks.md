---
author: TylerMSFT
title: Directrices para tareas en segundo plano
description: "Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano."
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fb6884a968afb87e8de303bbd17feba4993c597

---

# Directrices para tareas en segundo plano


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Asegúrate de que tu aplicación cumple los requisitos para ejecutar tareas en segundo plano.

## Guía de tareas en segundo plano


Ten en cuenta las indicaciones siguientes a la hora de desarrollar la tarea en segundo plano y antes de publicar una aplicación.

**Cuota de CPU:  **las tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que obtienen según el tipo de desencadenador. La mayoría de los desencadenadores están limitados a 30 segundos de uso de reloj, aunque algunos tienen la capacidad de ejecutarse hasta 10 minutos para completar tareas intensivas. Las tareas en segundo plano deben ser ligeras para ahorrar batería y proporcionar una mejor experiencia de usuario para las aplicaciones en primer plano. Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

**Administración de tareas en segundo plano:  **la aplicación debería obtener una lista de las tareas en segundo plano registradas, registrarse para controladores de progreso y finalización, y controlar dichos eventos de forma adecuada. Tus clases de tareas en segundo plano deben informar del progreso, la cancelación y la finalización. Para obtener más información, consulta [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md) y [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md).

**Usa [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499): **Si la clase de la tarea en segundo plano ejecuta código asincrónico, asegúrate de usar aplazamientos. De lo contrario, es posible que la tarea en segundo termine de forma anticipada cuando se complete el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx). Para más información, consulta [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md).

Como alternativa, solicita un solo aplazamiento y usa **async/await** para completar las llamadas a métodos asincrónicos. Cierra el aplazamiento después de las llamadas al método **await**.

**Actualización del manifiesto de la aplicación:  **declara todas las tareas en segundo plano en el manifiesto de la aplicación, junto con el tipo de desencadenadores con que se usan. En caso contrario, tu aplicación no podrá registrar la tarea en segundo plano en tiempo de ejecución. Para obtener más información, consulta [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md).

**Preparación para actualizaciones de la aplicación:  **Si la aplicación se va a actualizar, crea y registra una tarea en segundo plano **ServicingComplete** (consulta [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)) para llevar a cabo las actualizaciones de la aplicación que puedan ser necesarias fuera del contexto de la ejecución en primer plano.

**Solicitar la ejecución de tareas en segundo plano:  **

> **Importante:**  A partir de Windows 10, ya no es necesario que las aplicaciones estén en la pantalla de bloqueo para ejecutar tareas en segundo plano.

Todas las aplicaciones para la Plataforma universal de Windows (UWP) pueden ejecutar tipos de tareas admitidos sin necesidad de que se anclen en la pantalla de bloqueo. Sin embargo, las aplicaciones deben llamar al método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar ningún tipo de tarea en segundo plano. Este método devolverá el objeto [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439) si el usuario denegó explícitamente los permisos de tareas en segundo plano de la aplicación en la configuración del dispositivo.
## Lista de comprobación de tareas en segundo plano


La siguiente lista de comprobación se aplica a todas las tareas en segundo plano.

-   Crea la tarea en segundo plano en un componente de Windows Runtime.
-   Asocia tu tarea en segundo plano con el desencadenador correcto.
-   Agrega condiciones para asegurarte de que tu tarea en segundo plano se ejecuta correctamente.
-   Controla el progreso, la finalización y la cancelación de las tareas en segundo plano.
-   No muestres opciones de la interfaz de usuario que no sean notificaciones del sistema, iconos o actualizaciones de distintivo procedentes de la tarea en segundo plano.
-   En el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), solicita aplazamientos para todas las llamadas a métodos asincrónicos y ciérralos cuando el método haya terminado. O bien, usa un aplazamiento con **async/await**.
-   Usa almacenamiento persistente para compartir datos entre la tarea en segundo plano y la aplicación.
-   Declara todas las tareas en segundo plano en el manifiesto de la aplicación, junto con el tipo de desencadenador con el que se usan. Asegúrate de que el punto de entrada y los tipos de desencadenadores son correctos.
-   Escribe tareas en segundo plano de corta duración. Las tareas en segundo plano se limitan a 30 segundos de uso.
-   No confíes en la interacción con el usuario en las tareas en segundo plano.
-   Vuelve a registrar las tareas en segundo plano durante el inicio de la aplicación. Esto garantiza su registro la primera vez que se inicie la aplicación. Además, proporciona una manera de detectar si el usuario deshabilitó las funcionalidades de ejecución en segundo plano de la aplicación (en caso de error en el registro del evento).
-   No especifiques un elemento Executable en el manifiesto a menos que estés usando un desencadenador que debería ejecutarse en el mismo contexto que la aplicación (como por ejemplo el elemento [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).
-   Comprueba si hay errores de registro de tareas en segundo plano. Si procede, intenta volver a registrar la tarea en segundo plano con otros valores de parámetros.
-   Para todas las familias de dispositivos excepto la de equipos de escritorio, si el dispositivo dispone de poca memoria, las tareas en segundo plano podrían finalizarse. Si no se expone una excepción de falta de memoria o la aplicación no la controla, la tarea en segundo plano finalizará sin que se muestre ninguna advertencia y sin que se genere el evento OnCanceled. Esto contribuye a garantizar la experiencia del usuario de la aplicación en primer plano. La tarea en segundo plano debe estar diseñada para controlar este escenario.

## Windows: lista de comprobación de tareas en segundo plano para aplicaciones compatibles con la pantalla de bloqueo


Sigue esta directriz cuando desarrolles tareas en segundo plano para aplicaciones aptas para estar en la pantalla de bloqueo. Sigue la directriz en [Directrices y lista de comprobación de iconos de pantalla de bloqueo](https://msdn.microsoft.com/library/windows/apps/hh465403).

-   Asegúrate de que tu aplicación necesita estar en la pantalla de bloqueo antes de desarrollarla como una aplicación compatible con la pantalla de bloqueo. Para obtener más información, consulta [Introducción a la pantalla de bloqueo](https://msdn.microsoft.com/library/windows/apps/hh779720).

-   Asegúrate de que la aplicación siga funcionando cuando no esté en la pantalla de bloqueo.

-   Incluye una tarea en segundo plano registrada con [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) o [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), y declárala en el manifiesto de la aplicación. Asegúrate de que el punto de entrada y los tipos de desencadenadores son correctos. Esto es necesario para la certificación y permite al usuario colocar la aplicación en la pantalla de bloqueo.

**Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Temas relacionados

* [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones de la Tienda Windows (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->



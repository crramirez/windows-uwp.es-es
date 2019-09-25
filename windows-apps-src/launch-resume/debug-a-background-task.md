---
title: Depurar una tarea en segundo plano
description: Aprende a depurar una tarea en segundo plano, incluida la activación y el seguimiento de depuración de la tarea en segundo plano en el registro de eventos de Windows.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: ad133a9b1eb22695e6ce5d8b3edba9ad3a138b68
ms.sourcegitcommit: f1261aa6f7eeb62bf770a08b58ec4357bdc20c7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224764"
---
# <a name="debug-a-background-task"></a>Depurar una tarea en segundo plano


**API importantes**
-   [Windows.ApplicationModel.Background](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

Aprende a depurar una tarea en segundo plano, incluida la activación y el seguimiento de depuración de la tarea en segundo plano en el registro de eventos de Windows.

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>Depuración de tareas en segundo plano fuera y dentro del proceso
Este tema aborda principalmente las tareas en segundo plano que se ejecutan en un proceso independiente de la aplicación host. Si estás depurando una tarea en segundo plano dentro del proceso, no tendrás un proyecto de tarea en segundo plano independiente y podrás establecer un punto de interrupción en **OnBackgroundActivated()** (donde se ejecuta el código en segundo plano dentro del proceso) y consultar el paso 2 en [Desencadenar manualmente tareas en segundo plano para depurar el código de la tarea en segundo plano](#trigger-background-tasks-manually-to-debug-background-task-code) a continuación para obtener instrucciones sobre cómo activar el código en segundo plano para ejecutarlo.

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>Comprobar que el proyecto de tarea en segundo plano está correctamente configurado

En este tema se supone que tienes una aplicación existente con una tarea en segundo plano que depurar. Lo siguiente es específico para las tareas en segundo plano fuera del proceso y no se aplican a las tareas en segundo plano dentro del proceso.

-   En C# y C++, asegúrate de que el proyecto principal hace referencia al proyecto de tarea en segundo plano. Si esta referencia no se realiza, la tarea en segundo plano no se incluirá en el paquete de la aplicación.
-   En C# y C++, asegúrate de que el **Tipo de resultado** del proyecto de tarea en segundo plano sea "Componente de Windows Runtime".
-   La clase de segundo plano debe declararse en el atributo del punto de entrada en el manifiesto de paquete.

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>Desencadenar manualmente tareas en segundo plano para depurar el código de la tarea en segundo plano

Las tareas en segundo plano se pueden desencadenar manualmente mediante Microsoft Visual Studio. Después, podrás analizar el código y depurarlo.

1.  En C#, coloca un punto de interrupción en el método Run de la clase en segundo plano (para las tareas en segundo plano dentro del proceso, coloca el punto de interrupción en App.OnBackgroundActivated()) o escribe el resultado de depuración mediante [**System.Diagnostics**](https://docs.microsoft.com/dotnet/api/system.diagnostics?view=netframework-4.7.2).

    En C++, coloca un punto de interrupción en la función Run de la clase en segundo plano (para las tareas en segundo plano dentro del proceso, coloca el punto de interrupción en App.OnBackgroundActivated()) o escribe el resultado de depuración mediante [**OutputDebugString**](https://docs.microsoft.com/windows/desktop/api/debugapi/nf-debugapi-outputdebugstringw).

2.  Ejecuta la aplicación en el depurador y después desencadena la tarea en segundo plano mediante la barra de herramientas **Eventos de ciclo de vida**. Este menú desplegable muestra los nombres de las tareas en segundo plano que pueden activarse con Visual Studio.

> [!NOTE]
> Las opciones de la barra de herramientas eventos del ciclo de vida no se muestran de forma predeterminada en Visual Studio. Para mostrar estas opciones, haga clic con el botón derecho en la barra de herramientas actual de Visual Studio y asegúrese de que la opción **Ubicación de depuración** está habilitada.

    For this to work, the background task must already be registered and it must still be waiting for the trigger. For example, if a background task was registered with a one-shot TimeTrigger and that trigger has already fired, launching the task through Visual Studio will have no effect.

> [!Note]
> Las tareas en segundo plano que usan los siguientes desencadenadores no se pueden activar de esta manera: [**Desencadenador de aplicación**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger), [**desencadenador de MediaProcessing**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger), [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)y tareas en segundo plano con un [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) con el tipo de desencadenador [**SmsReceived**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) .  
> **ApplicationTrigger** y **MediaProcessingTrigger** se pueden señalar manualmente en el código con `trigger.RequestAsync()`.

![depurar tareas en segundo plano](images/debugging-activation.png)

3.  Cuando la tarea en segundo plano se activa, el depurador se adjuntará a ella y mostrará el resultado de depuración en VS.

## <a name="debug-background-task-activation"></a>Depurar la activación de tareas en segundo plano

> [!NOTE]
> Esta sección es específica para las tareas en segundo plano fuera del proceso y no se aplica a las tareas en segundo plano dentro del proceso.

La activación de la tarea en segundo plano depende de tres cosas:

-   Nombre y espacio de nombres de la clase de tarea en segundo plano
-   Atributo de punto de entrada especificado en el manifiesto del paquete
-   Punto de entrada especificado por la aplicación al registrar la tarea en segundo plano

1.  Usa Visual Studio para anotar el punto de entrada de la tarea en segundo plano:

    -   En C# y C++, anota el nombre y el espacio de nombres de la clase de tarea en segundo plano especificada en el proyecto de tarea en segundo plano.

2.  Usa el diseñador de manifiestos para comprobar que la tarea en segundo plano se ha declarado correctamente en el manifiesto de la aplicación:

    -   En C# y C++, el atributo de punto de entrada debe coincidir con el espacio de nombres de la tarea en segundo plano seguido por el nombre de clase. Por ejemplo: RuntimeComponent1.MyBackgroundTask.
    -   También deben especificarse todos los tipos de desencadenadores usados con la tarea.
    -   El ejecutable NO DEBE especificarse a menos que estés usando [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) o [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger).

3.  Solo Windows. Para ver el punto de entrada que usa Windows para activar la tarea en segundo plano, habilita el seguimiento de depuración y usa el registro de eventos de Windows.

    Si sigues este procedimiento y el registro de eventos muestra un punto de entrada o un desencadenador incorrecto para la tarea en segundo plano, la aplicación no está registrando la tarea en segundo plano correctamente. Para obtener ayuda con esta tarea, consulta [Registrar una tarea en segundo plano](register-a-background-task.md).

    1.  Abre el visor de eventos; para ello, ve a la pantalla Inicio y busca eventvwr.exe.
    2.   - Vaya a **registros de aplicaciones y servicios** **Microsoft**  - &gt; **Windows** BackgroundTaskInfrastructureenelvisordeeventos.&gt;  - &gt;
    3.  En el panel acciones, seleccione **Ver**  - &gt; **Mostrar registros analíticos y de depuración** para habilitar el registro de diagnóstico.
    4.  Selecciona el **Registro de diagnóstico** y haz clic en **Habilitar registro**.
    5.  Ahora, intenta usar tu aplicación para registrar y activar de nuevo la tarea en segundo plano.
    6.  Consulta los registros de diagnóstico para obtener información detallada del error. Esto incluirá el punto de entrada registrado para la tarea en segundo plano.

![visor de eventos para información de depuración de tareas en segundo plano](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>Tareas en segundo plano e implementación del paquete de Visual Studio

Si una aplicación que usa tareas en segundo plano se implementa con Visual Studio y la versión (mayor y/o menor) especificada en el diseñador de manifiestos después se actualiza, al reimplementar posteriormente la aplicación con Visual Studio es posible que las tareas en segundo plano de la aplicación se atasquen. Esto puede remediarse de la siguiente manera:

-   Usa Windows PowerShell para implementar la aplicación actualizada (en lugar de Visual Studio) mediante la ejecución del script generado junto con el paquete.
-   Si ya implementaste la aplicación con Visual Studio y las tareas en segundo plano de la aplicación ahora están atascadas, reinicia o cierra sesión y vuelve a abrirla para que las tareas en segundo plano vuelvan a funcionar.
-   Puedes seleccionar la opción de depuración "Reinstalar siempre mi paquete" para evitar esto en proyectos con C#.
-   Espera hasta que la aplicación está lista para la implementación final para incrementar la versión del paquete (no lo cambies durante la depuración).

## <a name="remarks"></a>Comentarios

-   Asegúrate de que tu aplicación compruebe los registros de tareas en segundo plano existentes antes de registrar la tarea nuevamente. Varios registros de la misma tarea pueden causar resultados inesperados al ejecutar la tarea en segundo plano más de una vez cada vez que esta se desencadena.
-   Si la tarea en segundo plano requiere acceso a la pantalla de bloqueo, asegúrate de poner la aplicación en la pantalla de bloqueo antes de intentar depurar la tarea en segundo plano. Para obtener información acerca de cómo especificar opciones para bloquear aplicaciones compatibles con la pantalla de bloqueo, consulta [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md).
-   Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se ocupe correctamente de los escenarios en los que se produce un error en el registro de tareas en segundo plano. Si, en cambio, la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

Para obtener más información sobre el uso de VS para depurar una tarea en segundo plano [, consulte Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones de UWP](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015).

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones para UWP](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015)
* [Análisis de la calidad del código de las aplicaciones para UWP con análisis de código de Visual Studio](https://docs.microsoft.com/visualstudio/test/analyze-the-code-quality-of-store-apps-using-visual-studio-static-code-analysis?view=vs-2015)

 

 

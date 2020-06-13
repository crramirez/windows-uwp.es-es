---
title: Ciclo de vida de una aplicación para UWP de Windows 10
description: En este tema se describe el ciclo de vida de una aplicación para la Plataforma universal de Windows (UWP) desde el momento en que se activa hasta que se cierra.
keywords: ciclo de vida de una aplicación, suspendida, reanudar, iniciar, activar
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0098e3d0ab31c8a3756ec7d4bf05844ace95d555
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756571"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Ciclo de vida de una aplicación para la Plataforma universal de Windows (UWP) de Windows 10


En este tema se describe el ciclo de vida de una aplicación para la Plataforma universal de Windows (UWP) desde el momento en que se inicia hasta que se cierra.

## <a name="a-little-history"></a>Un poco de historia

Antes de Windows 8, las aplicaciones tenían un ciclo de vida simple. Las aplicaciones Win32 y .NET están en ejecución o no. Cuando un usuario las minimiza o sale de ellas, continúan ejecutándose. Esto funcionó bien hasta que los dispositivos portátiles y la administración de la energía empezaron a cobrar cada vez más importancia.

Windows 8 presentó un nuevo modelo de aplicación con aplicaciones de UWP. En un nivel alto, se agregó un nuevo estado, el estado suspendido. Una aplicación para UWP se suspende poco después de que el usuario la minimice o cambie a otra aplicación. Esto significa que los subprocesos de la aplicación se detienen y la aplicación se deja en la memoria, a menos que el sistema operativo necesite recuperar algunos recursos. Cuando el usuario vuelve a la aplicación, esta se puede restaurar rápidamente a un estado en ejecución.

Existen varias maneras para las aplicaciones que deben seguir ejecutándose cuando están en segundo plano, como las [tareas en segundo plano](support-your-app-with-background-tasks.md), la [ejecución ampliada](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution) y la ejecución patrocinada por la actividad (por ejemplo, la funcionalidad **BackgroundMediaEnabled**, que permite a una aplicación proseguir la [reproducción de elementos multimedia en segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)). Además, las operaciones de transferencia en segundo plano pueden continuar incluso si la aplicación está suspendida o incluso si ha finalizado. Para obtener más información, consulta [Cómo descargar un archivo](https://docs.microsoft.com/previous-versions/windows/apps/jj152726(v=win.10)).

De manera predeterminada, las aplicaciones que no están en primer plano se suspenden, lo que resulta en un ahorro de energía y en que haya más recursos disponibles para la aplicación actualmente en primer plano.

El estado de suspendido agrega nuevos requisitos para los desarrolladores, porque el sistema operativo puede optar por finalizar una aplicación suspendida para liberar recursos. La aplicación finalizada seguirá apareciendo en la barra de tareas. Cuando el usuario haga clic en ella, la aplicación deberá restaurar el estado en el que estaba antes de finalizar, porque el usuario no sabrá que el sistema la ha cerrado. Pensará que ha estado en espera en segundo plano mientras estaba haciendo otras cosas y esperará que esté en el mismo estado que tenía cuando la ha dejado. En este tema veremos cómo lograrlo.

Windows 10, versión 1607, presenta dos estados más del modelo de aplicaciones: **Ejecución en primer plano** y **Ejecución en segundo plano**. También veremos estos nuevos estados en las secciones siguientes.

## <a name="app-execution-state"></a>Estado de ejecución de la aplicación

Esta ilustración representa los posibles estados del modelo de aplicaciones a partir de Windows 10, versión 1607. Vamos a examinar el ciclo de vida típico de una aplicación para UWP.

![Diagrama de estados en el que se muestran las transiciones entre los estados de ejecución de la aplicación](images/updated-lifecycle.png)

Las aplicaciones entran en el estado de ejecución en segundo plano cuando se inician o se activan. Si la aplicación necesita moverse al primer plano debido a un inicio de la aplicación en primer plano, la aplicación obtiene el evento [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) .

Aunque "iniciado" y "activado" pueden parecer términos similares, hacen referencia a las distintas formas en que el sistema operativo puede iniciar la aplicación. Primero analicemos el inicio de una aplicación.

## <a name="app-launch"></a>Inicio de la aplicación

Cuando se inicia una aplicación, se llama al método [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched). Se pasa un parámetro [**LaunchActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) que proporciona, entre otras cosas, los argumentos pasados a la aplicación, el identificador del icono que ha iniciado la aplicación y el estado anterior en el que se encontraba la aplicación.

Para obtener el estado anterior de la aplicación, usa [LaunchActivatedEventArgs.PreviousExecutionState](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate), que devuelve [ApplicationExecutionState](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.applicationexecutionstate). Sus valores y la acción apropiada que realizar debido a ese estado son los siguientes:

| ApplicationExecutionState | Explicación | Acción que realizar |
|-------|-------------|----------------|
| **NotRunning** | Una aplicación puede estar en este estado debido a que no se haya iniciado desde la última vez que el usuario ha reiniciado el sistema o ha iniciado sesión. También puede encontrarse en este estado si se estaba ejecutando pero se ha bloqueado o porque el usuario la haya cerrado antes.| Inicializa la aplicación como si se ejecutara por primera vez en la sesión del usuario actual. |
|**Suspendido** | El usuario ha minimizado o ha abandonado la aplicación y no ha vuelto a ella en pocos segundos. | Cuando la aplicación se ha suspendido, su estado se ha conservado en la memoria. Solo tienes que volver a adquirir los identificadores de archivos u otros recursos que has liberado al suspender la aplicación. |
| **Finalizado** | La aplicación se había suspendida anteriormente, pero luego se ha cerrado en algún momento porque el sistema necesitaba recuperar memoria. | Restaura el estado en el que estaba la aplicación cuando el usuario la ha abandonado.|
|**ClosedByUser** | El usuario ha cerrado la aplicación con el gesto de cerrar en el modo tableta o con Alt+F4. Cuando el usuario cierra la aplicación, primero se suspende y después finaliza. | Dado que la aplicación básicamente ha pasado por los mismos pasos que conducen al estado Terminated, debes gestionar este estado del mismo modo que lo harías con el estado Terminated.|
|**Ejecución** | La aplicación ya estaba abierta cuando el usuario ha intentado volver a iniciarla. | Nada. Ten en cuenta que no se inicia otra instancia de la aplicación. Simplemente se activa la instancia que ya está en ejecución. |

**Nota**            La   *sesión del usuario actual* se basa en el inicio de sesión de Windows. Siempre y cuando el usuario actual no haya cerrado la sesión explícitamente, haya apagado el equipo ni haya reiniciado Windows, la sesión del usuario actual persiste durante eventos tales como la autenticación de pantalla de bloqueo y el cambio de usuario, entre otros. 

Una circunstancia importante a tener en cuenta es que si el dispositivo tiene suficientes recursos, el sistema operativo realizará un inicio previo de las aplicaciones usadas frecuentemente que han optado por ese comportamiento para optimizar la capacidad de respuesta. Las aplicaciones que cuentan con inicio previo se inician en segundo plano y luego se suspenden rápidamente para que, cuando el usuario cambia a ellas, se puedan reanudar, lo que es más rápido que iniciar la aplicación.

Debido al inicio previo, el sistema puede iniciar el método **OnLaunched()** de la aplicación, en lugar de que lo deba hacer el usuario. Debido al inicio previo de la aplicación en segundo plano, puede que debas realizar una acción distinta en **OnLaunched()**. Por ejemplo, si la aplicación empieza a reproducir música cuando se inicia, no sabrán de dónde proviene, porque la aplicación se habrá iniciado previamente en segundo plano. Una vez que la aplicación está iniciada previamente en segundo plano, le sigue una llamada a **Application.Suspending**. Después, cuando el usuario inicia la aplicación, se invoca el evento resuming, así como el método **OnLaunched()**. Consulta [Controlar el inicio previo de las aplicaciones](handle-app-prelaunch.md) para obtener más información sobre cómo controlar el escenario de inicio previo. Solo aplicaciones que optan por ello participan en el inicio previo.

Cuando se inicia una aplicación, Windows muestra una pantalla de presentación. Para configurar la pantalla de presentación, consulte [Agregar una pantalla de presentación](https://docs.microsoft.com/previous-versions/windows/apps/hh465331(v=win.10)).

Mientras se muestra la pantalla de presentación, la aplicación debe registrar controladores de eventos y configurar cualquier interfaz de usuario personalizada que necesite para la página inicial. Comprueba que las tareas que se estén ejecutando en el constructor de la aplicación y **OnLaunched()** se completen en pocos segundos o el sistema podría pensar que la aplicación no responde y finalizarla. Si la aplicación necesita pedir datos de la red o recuperar grandes cantidades de datos del disco, estas actividades se deberán llevar a cabo fuera del inicio. Una aplicación puede usar su propia interfaz de usuario de carga personalizada o una pantalla de presentación ampliada mientras espera a que finalicen las operaciones cuya ejecución requiere mucho tiempo. Consulta [Mostrar una pantalla de presentación durante más tiempo](create-a-customized-splash-screen.md) y [Splash screen sample](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/SplashScreen) (Muestra de pantalla de presentación) para obtener más información.

Cuando la aplicación completa el inicio, entra en el estado **Running**, la pantalla de presentación desaparece y se desactivan todos los recursos y objetos de dicha pantalla de presentación.

## <a name="app-activation"></a>Activación de una aplicación

Una aplicación puede activarla el sistema, en lugar de que la inicie el usuario. Una aplicación puede activarse mediante un contrato, como el contrato para contenido compartido. O se puede activar para controlar un protocolo URI personalizado o un archivo con una extensión que la aplicación esté registrada para controlar. Para obtener una lista con las distintas formas en las que se puede activar una aplicación, consulta [**ActivationKind**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).

La clase [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) define los métodos que puedes invalidar para controlar los distintos modos en los que se puede activar la aplicación.
[**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) puede controlar todos los tipos de activación posibles. Sin embargo, es más habitual usar métodos específicos para controlar los tipos de activación más comunes y usar **OnActivated** como método de reserva para los tipos de activación menos comunes. Estos son los métodos adicionales para las activaciones específicas:

[**OnCachedFileUpdaterActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated)  
[**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)  
[**OnFileOpenPickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated)  [**OnFileSavePickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated)  
[**OnSearchActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsearchactivated)  
[**OnShareTargetActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated)

Los datos de evento de estos métodos incluyen la misma propiedad [**PreviousExecutionState**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) que se ha tratado anteriormente, que indica el estado en el que se encontraba la aplicación antes de activarla. Interpreta el estado y lo que debes hacer del mismo modo que se describe anteriormente en la sección [Inicio de la aplicación](#app-launch).

**Nota:**   Si inicia sesión con la cuenta de administrador del equipo, no podrá activar las aplicaciones UWP.

## <a name="running-in-the-background"></a>Ejecución en segundo plano ##

A partir de Windows 10, versión 1607, las aplicaciones pueden ejecutar tareas en segundo plano en el mismo proceso que la propia aplicación. Puedes leer más sobre este tema en [Actividad en segundo plano con el modelo de proceso único](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99). En este artículo no analizaremos el procesamiento en segundo plano dentro del proceso, pero este influye en el ciclo de vida de la aplicación porque se han agregado dos nuevos eventos relacionados con cuando la aplicación está en segundo plano. Se trata de: [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) y [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground).

Estos eventos también reflejan si el usuario puede ver la interfaz de usuario de la aplicación.

La ejecución en segundo plano es el estado predeterminado en el que se inicia, se activa o se reanuda una aplicación. En este estado, la interfaz de usuario de la aplicación aún no es visible.

## <a name="running-in-the-foreground"></a>Ejecución en primer plano ##

La ejecución en primer plano significa que la interfaz de usuario de la aplicación es visible.

El evento **LeavingBackground** se desencadena justo antes de que la interfaz de usuario de la aplicación sea visible y antes de entrar en el estado de ejecución en primer plano. También se desencadena cuando el usuario vuelve a la aplicación.

Anteriormente, la mejor ubicación para cargar activos de la interfaz de usuario eran los controladores de eventos **Activated** o **Resuming**. Ahora **LeavingBackground** es el mejor lugar para comprobar que la interfaz de usuario está lista.

Es importante comprobar que los activos visuales están listos en este momento, porque es la última oportunidad de realizar algún trabajo antes de que la aplicación sea visible para el usuario. Todo el trabajo de la interfaz de usuario en este controlador de eventos debería completarse rápidamente, puesto que afecta al tiempo de inicio y reanudación que experimenta el usuario. **LeavingBackground** es el momento de asegurarse de que el primer fotograma de la interfaz de usuario está listo. Después, las llamadas de larga ejecución de almacenamiento o de red deberían controlarse asincrónicamente, de forma que el controlador de eventos pueda volver.

Cuando el usuario sale de la aplicación, esta vuelve a entrar en el estado de ejecución en segundo plano.

## <a name="reentering-the-background-state"></a>Volver a entrar en el estado en segundo plano

El evento **EnteredBackground** indica que la aplicación ya no está visible en primer plano. En un equipo de escritorio, **EnteredBackground** se desencadena cuando la aplicación está minimizada; en un teléfono, se desencadena al cambiar a la pantalla de inicio o a otra aplicación.

### <a name="reduce-your-apps-memory-usage"></a>Reducir el uso de memoria de la aplicación

Dado que la aplicación ya no es visible para el usuario, este es el mejor lugar para detener el trabajo de representación y las animaciones de la interfaz de usuario. Para volver a iniciar ese trabajo, puedes usar **LeavingBackground**.

Si vas a realizar algún trabajo en segundo plano, este es el lugar para prepararlo. Es mejor comprobar [MemoryManager.AppMemoryUsageLevel](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelevel) y, si es necesario, reducir la cantidad de memoria usada por la aplicación cuando se está ejecutando en segundo plano, de forma que dicha aplicación no corra el riesgo de que el sistema la finalice para liberar recursos.

Consulta [Reducir el uso de memoria cuando la aplicación pasa al estado en segundo plano](reduce-memory-usage.md) para obtener más detalles.

### <a name="save-your-state"></a>Guardar el estado

El controlador de eventos de suspensión es el mejor lugar para guardar el estado de la aplicación. Sin embargo, si está trabajando en segundo plano (por ejemplo, reproducción de audio, mediante una sesión de ejecución extendida o una tarea en segundo plano en proceso), también es recomendable guardar los datos de forma asincrónica desde el controlador de eventos de **EnteredBackground** . Esto se debe a que es posible que la aplicación finalice mientras tiene una prioridad inferior en segundo plano. Y dado que en ese caso la aplicación no habrá pasado por el estado de suspensión, los datos se perderán.

Al guardar los datos en el controlador de eventos de **EnteredBackground** , antes de que se inicie la actividad en segundo plano, garantiza una buena experiencia del usuario cuando el usuario vuelve a poner la aplicación en primer plano. Puedes usar las API de datos de aplicación para guardar los datos y la configuración. Para obtener más información, vea [almacenar y recuperar la configuración y otros datos](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)de la aplicación.

Después de guardar los datos, si superas el límite de uso de memoria, puedes liberar los datos de la memoria, ya que se pueden volver a cargar más adelante. Eso liberará memoria, que podrán usar los activos necesarios para la actividad en segundo plano.

Ten en cuenta si la aplicación tiene actividad en segundo plano en curso que pueda mover del estado de ejecución en segundo plano al estado de ejecución en primer plano sin alcanzar el estado suspendido.

### <a name="asynchronous-work-and-deferrals"></a>Trabajo asincrónico y aplazamientos

Si realizas una llamada asincrónica en el controlador, el control vuelve inmediatamente de esa llamada asincrónica. Eso significa que, a continuación, la ejecución puede volver del controlador de eventos y la aplicación se moverá al siguiente estado aunque aún no haya completado la llamada asincrónica. Usa el método [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) en el objeto [**EnteredBackgroundEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel?redirectedfrom=MSDN) que se pasa al controlador de eventos para retrasar la suspensión hasta después de llamar al método [**Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete) en el objeto [**Windows.Foundation.Deferral**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral) devuelto.

Un aplazamiento no aumenta la cantidad que tienes que ejecutar el código antes de que finalice la aplicación. Solo retrasa su terminación hasta que se llama al método *Complete* del aplazamiento o hasta que pasa la fecha límite, *lo que ocurra primero*.

Si necesitas más tiempo para guardar el estado, investiga formas de guardar dicho estado por fases antes de que la aplicación entre en el estado en segundo plano para que haya menos que guardar en el controlador de eventos **EnteredBackground**. O puedes solicitar una [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx) para obtener más tiempo. Sin embargo, no hay ninguna garantía de que la solicitud se conceda, por lo que es mejor encontrar maneras de minimizar la cantidad de tiempo que se debe guardar el estado.

## <a name="app-suspend"></a>Suspensión de aplicaciones

Cuando el usuario minimiza una aplicación, Windows espera unos segundos para ver si dicho usuario vuelve a ella. Si no lo hace en ese periodo de tiempo y no hay activa ninguna ejecución ampliada, tarea en segundo plano ni ejecución patrocinada por la actividad, Windows suspende la aplicación. También se suspende una aplicación cuando aparece la pantalla de bloqueo, siempre que no haya ninguna sesión de ejecución ampliada, etc. activa en esa aplicación.

Cuando se suspende una aplicación, esta invoca el evento [**Application.Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending). Las plantillas de proyecto para UWP de Visual Studio proporcionan un controlador para este evento denominado **OnSuspending** en **App.xaml.cs**. Antes de Windows 10, versión 1607, el código para guardar el estado se ponía aquí. Ahora recomendamos guardar el estado cuando se entra en el estado en segundo plano, como se ha descrito anteriormente.

Conviene que liberes los recursos exclusivos y los identificadores de archivos para que otras aplicaciones puedan tener acceso a ellos cuando la aplicación esté suspendida. Algunos ejemplos de recursos exclusivos son las cámaras, los dispositivos de E/S, los dispositivos externos y los recursos de red. Liberar de forma explícita recursos exclusivos e identificadores de archivos sirve para que otras aplicaciones puedan acceder a ellos cuando la aplicación esté suspendida. Cuando la aplicación se reanude, deberá volver a adquirir sus recursos exclusivos y sus identificadores de archivos.

### <a name="be-aware-of-the-deadline"></a>Ten en cuenta el plazo límite

Para garantizar que un dispositivo sea rápido y responda adecuadamente, hay un límite de tiempo para ejecutar el código en el controlador de eventos de suspensión. Este es diferente para cada dispositivo, y puedes averiguar cuál es mediante una propiedad del objeto [**SuspendingOperation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingOperation) llamada deadline.

Al igual que sucede con el controlador de eventos **EnteredBackground**, si realizas una llamada asincrónica desde el controlador, el control vuelve inmediatamente desde esa llamada asincrónica. Eso significa que, a continuación, la ejecución puede volver del controlador de eventos, y la aplicación se moverá al estado suspendido aunque aún no se haya completado la llamada asincrónica. Usa el método [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) en el objeto [**SuspendingOperation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingOperation) (disponible a través de los argumentos de evento) para retrasar la entrada en el estado suspendido hasta después de haber llamado al método [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingdeferral.complete) en el objeto [**SuspendingDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingDeferral) devuelto.

Si necesitas más tiempo, puedes solicitar una [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx). Sin embargo, no existe ninguna garantía de que la solicitud se conceda, por lo que es mejor encontrar maneras de minimizar la cantidad de tiempo que se necesita en el controlador de eventos **Suspended**.

### <a name="app-terminate"></a>Finalizar la aplicación

El sistema intenta mantener la aplicación y sus datos en la memoria mientras está suspendida. No obstante, si el sistema no tiene los recursos necesarios para mantener la aplicación en la memoria, la finalizará. Las aplicaciones no reciben ninguna notificación de que van a finalizar, por lo que la única oportunidad de guardar los datos de la aplicación es en el controlador de eventos **OnSuspension** o de forma asincrónica desde el controlador **EnteredBackground**.

Cuando la aplicación determina que se ha activado tras finalizar, debe cargar los datos de aplicación que se han guardado, de forma que se encuentre en el mismo estado en el que estaba antes de finalizar. Cuando el usuario vuelve a una aplicación suspendida que ha finalizado, la aplicación debe restaurar los datos de aplicación en el método [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched). El sistema no notifica a una aplicación cuándo va a finalizar, por lo que la aplicación deberá guardar sus datos de aplicación y liberar los recursos exclusivos y los identificadores de archivos antes de su suspensión y restaurarlos cuando vuelva a activarse después de la finalización.

**Una nota sobre la depuración con Visual Studio:** Visual Studio impide que Windows suspenda una aplicación que está asociada al depurador. Esto permite que el usuario vea la interfaz de usuario de depuración de Visual Studio mientras se ejecuta la aplicación. Mientras depuras una aplicación, puedes enviarle un evento de suspensión mediante Visual Studio. Asegúrate de que se muestra la barra de herramientas **Ubicación de depuración** y luego haz clic en el botón **Suspender**.

## <a name="app-resume"></a>Reanudación de la aplicación

Una aplicación suspendida se reanuda cuando el usuario vuelve a ella o cuando es la aplicación activa cuando el dispositivo sale de un estado de bajo consumo.

Cuando una aplicación se reanuda desde el estado **Suspendido**, entra en el estado **Ejecución en segundo plano** y el sistema restaura la aplicación donde se quedó, para que al usuario le parezca como hubiese estado ejecutándose todo el tiempo. No se pierde ningún dato de la aplicación que esté almacenado en la memoria. Por lo tanto, las aplicaciones no necesitan restaurar el estado cuando se reanudan, aunque deben adquirir cualquier archivo o identificador de dispositivo que hayan liberado al suspenderse y restaurar cualquier estado que se haya liberado explícitamente en el momento de la suspensión.

La aplicación puede suspenderse durante horas o días. Por lo tanto, si la aplicación tiene contenido o conexiones de red que puedan haber quedado obsoletos, se deberán actualizar al reanudarse. Si una aplicación ha registrado un controlador de eventos para el evento [**Application.Resuming**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming), se llamará a este controlador de eventos cuando la aplicación se reanude desde el estado **Suspended**. Puedes actualizar el contenido y los datos de la aplicación en este controlador de eventos.

Si se activa una aplicación suspendida para que participe en un contrato entre aplicaciones o una extensión, recibirá primero el evento **Resuming** y después el evento **Activated**.

Si la aplicación suspendida se finalizó, no hay ningún evento **Resuming**, y en su lugar se llama a **OnLaunched()** con un **ApplicationExecutionState** de **Terminated**. Debido a que el estado se ha guardado cuando se ha suspendido la aplicación, puedes restaurar dicho estado durante **OnLaunched()** para que al usuario le parezca que aplicación está igual que cuando la ha abandonado.

Mientras una aplicación está suspendida, no recibe ninguno de los eventos de red que se haya registrado para recibir. Dichos eventos no se colocan en la cola; simplemente, se pierden. Por ello, la aplicación debe comprobar el estado de red cuando se reanude.

**Nota:**    Dado que el evento [**RESUMING**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) no se genera desde el subproceso de la interfaz de usuario, se debe usar un distribuidor si el código del controlador de reanudación se comunica con la interfaz de usuario. Consulta [Update the UI thread from a background thread](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md) (Actualizar el subproceso de la interfaz de usuario desde un subproceso en segundo plano) para obtener un ejemplo de cómo hacerlo.

Para obtener instrucciones generales, consulta [Directrices para suspender y reanudar una aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/index).

## <a name="app-close"></a>Cierre de la aplicación

Por lo general, no es necesario que los usuarios cierren las aplicaciones, sino que pueden dejar que Windows se encargue de ello. No obstante, los usuarios pueden decidir cerrar una aplicación mediante el gesto de cerrar, presionando Alt y F4 o mediante el conmutador de tareas en Windows Phone.

No hay ningún evento que indique que el usuario ha cerrado dicha aplicación. Cuando el usuario cierra una aplicación, primero se suspende para que tengas la oportunidad de guardar su estado. En Windows 8.1 y en versiones posteriores, una vez que el usuario cierra la aplicación, esta se quita de la pantalla y de la lista de cambio, pero no finaliza explícitamente.

**Comportamiento cerrado por usuario:**    Si la aplicación necesita hacer algo diferente cuando está cerrada por el usuario que cuando está cerrada por Windows, puede usar el controlador de eventos de activación para determinar si la aplicación ha sido finalizada por el usuario o por Windows. Consulta las descripciones de los estados **ClosedByUser** y **Terminated** en la referencia relativa a la enumeración [**ApplicationExecutionState**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState).

Te recomendamos que las aplicaciones no se cierren automáticamente mediante programación a menos que sea absolutamente necesario. Por ejemplo, si una aplicación detecta una fuga de memoria, se puede cerrar para preservar la seguridad de los datos personales del usuario.

## <a name="app-crash"></a>Bloqueo de la aplicación

La experiencia de bloqueo del sistema está diseñada para que los usuarios puedan volver a lo que estaban haciendo lo antes posible. No se debe proporcionar ningún cuadro de diálogo de advertencia o notificación, ya que retrasaría más al usuario.

Si la aplicación se bloquea, deja de responder o genera una excepción, se enviará un informe del problema a Microsoft según la [configuración de comentarios y diagnósticos](https://support.microsoft.com/help/4468236/diagnostics-feedback-and-privacy-in-windows-10-microsoft-privacy) del usuario. Microsoft te proporciona un subconjunto de datos de error en el informe del problema, para que puedas usarlo para mejorar la aplicación. Puedes consultar estos datos en la página Calidad de la aplicación en el panel.

Cuando el usuario activa una aplicación tras un bloqueo, su controlador de eventos de activación recibe un valor [**ApplicationExecutionState**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) de **NotRunning** y debe mostrar simplemente su interfaz de usuario y datos iniciales. Después de un bloqueo, no use rutinariamente los datos de la aplicación que habría usado para **reanudar** con **Suspended** porque los datos podrían estar dañados. Consulte las [instrucciones para la suspensión y reanudación de aplicaciones](https://docs.microsoft.com/windows/uwp/launch-resume/index).

## <a name="app-removal"></a>Eliminación de aplicaciones

Cuando un usuario elimina la aplicación, esta se quita, junto con todos los datos locales. La eliminación de una aplicación no afecta a los datos del usuario almacenados en ubicaciones comunes como, por ejemplo, las bibliotecas de imágenes o documentos.

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>Ciclo de vida de aplicación y plantillas de proyecto de Visual Studio

En las plantillas de proyecto de Visual Studio se proporciona el código básico relevante para el ciclo de vida de la aplicación. La aplicación básica controla la activación del inicio, ofrece un lugar para restaurar los datos de aplicación y muestra la interfaz de usuario principal, incluso antes de que agregues tu propio código. Para obtener más información, vea [plantillas de proyecto de C#, VB y C++ para aplicaciones](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10)).

## <a name="key-application-lifecycle-apis"></a>API clave del ciclo de vida de la aplicación

-   Espacio de nombres [**Windows.ApplicationModel**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel)
-   Espacio de nombres [**Windows. ApplicationModel. Activation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation)
-   Espacio de nombres [**Windows.ApplicationModel.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core)
-   Clase [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) (XAML)
-   Clase [**Windows.UI.Xaml.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) (XAML)

## <a name="related-topics"></a>Temas relacionados

* [**ApplicationExecutionState**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)
* [Directrices para suspender y reanudar una aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/index)
* [Administrar el inicio previo de aplicaciones](handle-app-prelaunch.md)
* [Controlar la activación de aplicaciones](activate-an-app.md)
* [Controlar la suspensión de aplicaciones](suspend-an-app.md)
* [Controlar la reanudación de aplicaciones](resume-an-app.md)
* [Actividad en segundo plano con el modelo de proceso único](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [Reproducir contenido multimedia en segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)

 

 

---
description: Aprender a usar la ejecución extendida para mantener la aplicación en ejecución mientras está minimizada
title: Aplazar la suspensión de aplicaciones con ejecución ampliada
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ejecución extendida, minimizada, ExtendedExecutionSession, tarea en segundo plano, ciclo de vida de la aplicación, pantalla de bloqueo
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 41a084088ab75293cd4f8f70d5d8c248000645b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167749"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>Aplazar la suspensión de aplicaciones con ejecución ampliada

En este artículo se muestra cómo usar la ejecución extendida para posponer cuando la aplicación se suspende para que pueda ejecutarse mientras está minimizada o en la pantalla de bloqueo.

Cuando el usuario minimiza la aplicación o cambia a otra, la primera entra en estado suspendido.  Su memoria se mantiene, pero no ejecuta el código. Esto sucede en todas las ediciones del sistema operativo con una interfaz de usuario visual. Para obtener más detalles sobre cuándo se suspende la aplicación, consulta [Ciclo de vida de la aplicación](app-lifecycle.md). 

Hay casos en los que es posible que una aplicación necesite seguir ejecutándose, en lugar de suspenderse, cuando el usuario navega fuera de la aplicación o mientras está minimizada. Por ejemplo, una aplicación de recuento de pasos debe seguir los pasos de ejecución y seguimiento incluso cuando el usuario navega fuera para usar otras aplicaciones. 

Si es necesario que una aplicación se siga ejecutando, la puede seguir ejecutando el sistema operativo o este puede solicitar que se siga ejecutando. Por ejemplo, al reproducir audio en segundo plano, el sistema operativo puede mantener una aplicación ejecutándose más tiempo si sigues estos pasos de [Reproducción de contenido multimedia en segundo plano](../audio-video-camera/background-audio.md). De lo contrario, debes solicitar más tiempo manualmente. La cantidad de tiempo que puede obtenerse al realizar la ejecución en segundo plano puede ser de varios minutos, pero debe estar preparado para controlar la sesión que se va a revocar en cualquier momento. Estas restricciones de tiempo del ciclo de vida de la aplicación se deshabilitan mientras la aplicación se ejecuta bajo un depurador. Por esta razón, es importante probar la ejecución extendida y otras herramientas para posponer la suspensión de la aplicación mientras no se ejecuta en un depurador o mediante los eventos de ciclo de vida disponibles en Visual Studio. 
 
Crea una clase [ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) para solicitar más tiempo para completar una operación en segundo plano. El tipo de clase **ExtendedExecutionSession** que se crea está determinado por la enumeración [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) que se proporciona al crearla. Existen tres valores de enumeración **ExtendedExecutionReason**: **Unspecified, LocationTracking** y **SavingData**. Solo se puede solicitar un **ExtendedExecutionSession** en cualquier momento; Si se intenta crear otra sesión mientras una solicitud de sesión aprobada está activa, se producirá una excepción 0x8007139F en el constructor de **ExtendedExecutionSession** que indica que el grupo o recurso no está en el estado correcto para realizar la operación solicitada. No use [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) y [ExtendedExecutionForegroundReason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason); requieren capacidades restringidas y no están disponibles para su uso en aplicaciones de la tienda.

## <a name="run-while-minimized"></a>Ejecutar mientras está minimizada

Hay dos casos en los que se puede usar la ejecución extendida:
- En cualquier momento durante la ejecución normal en primer plano, mientras la aplicación se encuentra en el estado en ejecución.
- Una vez que la aplicación ha recibido un evento de suspensión (el sistema operativo está a punto de trasladar la aplicación al estado suspendido) en el controlador de eventos de suspensión de la aplicación.

El código para estos dos casos es el mismo, pero la aplicación se comporta de un modo ligeramente diferente en cada uno. En el primer caso, la aplicación permanece en el estado en ejecución, incluso si se produce un evento que normalmente desencadenaría la suspensión (por ejemplo, el usuario se desplaza fuera de la aplicación). La aplicación nunca recibirá un evento de suspensión mientras la extensión de ejecución esté en vigor. Cuando se desecha la extensión, la aplicación vuelve a ser válida para la suspensión.

En el segundo caso, si la aplicación está pasando al estado suspendido, permanecerá en estado de suspensión durante el período de la extensión. Una vez que la extensión expira, la aplicación entra en el estado Suspended sin recibir más notificaciones.

Use **ExtendedExecutionReason. unespecifiqued** al crear una **ExtendedExecutionSession** para solicitar más tiempo antes de que la aplicación pase a un segundo plano para escenarios como el procesamiento multimedia, la compilación de proyectos o el mantenimiento de una conexión de red activa. En dispositivos de escritorio que ejecutan Windows 10 para ediciones de escritorio (Home, Pro, Enterprise y Education), este es el enfoque que se debe usar si es necesario evitar que una aplicación se suspenda mientras está minimizada.

Solicita la extensión cuando se inicie una operación de larga duración para aplazar la transición al estado **Suspendiendo** que, de otro modo, se produciría al colocar la aplicación en segundo plano. En dispositivos de escritorio, las sesiones de ejecución extendida creadas con **ExtendedExecutionReason.Unspecified** tienen un límite de tiempo de reconocimiento de batería. Si el dispositivo está conectado a una toma eléctrica, no hay ningún límite en la duración del período de tiempo de ejecución extendida. Si el dispositivo funciona con batería, el período de tiempo de ejecución extendida puede ejecutarse hasta diez minutos en segundo plano.

Un usuario de tableta o portátil puede obtener el mismo comportamiento de ejecución prolongada, a costa de la duración de la batería, cuando la opción **permitir que la aplicación ejecute tareas en segundo plano** esté seleccionada en uso de la **batería por** la configuración de la aplicación. (Para encontrar esta opción en un equipo portátil, vaya a **configuración**  >  . **Del sistema**  >  **Batería**  >  de **Uso de la batería por aplicación** (el vínculo en el porcentaje de energía restante de la batería) > Seleccione una aplicación > desactivar **administrado por Windows** > seleccione **permitir que la aplicación ejecute tareas en segundo plano**.  

En todas las ediciones del sistema operativo, este tipo de sesión de ejecución extendida se detiene cuando el dispositivo entra en modo de espera conectado. En dispositivos móviles que ejecutan Windows 10 Mobile, este tipo de sesión de ejecución extendida se ejecutará siempre que la pantalla esté encendida. Cuando la pantalla se apaga, el dispositivo intenta entrar en el modo de espera conectado de bajo consumo inmediatamente. En dispositivos de escritorio, la sesión seguirá ejecutándose si aparece la pantalla de bloqueo. El dispositivo no entrará en modo de espera conectado durante un período de tiempo después de apagar la pantalla. En la edición del sistema operativo de Xbox, el dispositivo entra en modo de espera conectado después de una hora, a menos que el usuario cambia el valor predeterminado.

## <a name="track-the-users-location"></a>Realizar un seguimiento de la ubicación del usuario

Especifica **ExtendedExecutionReason.LocationTracking** cuando crees una clase **ExtendedExecutionSession** si tu aplicación necesita registrar con regularidad la ubicación desde la clase [Geolocator](/uwp/api/windows.devices.geolocation.geolocator). Las aplicaciones de navegación y seguimiento de la forma física que necesitan controlar regularmente la ubicación del usuario deben usar este motivo.

Una sesión de ejecución extendida de seguimiento de ubicación se puede ejecutar siempre que sea necesario, incluso mientras la pantalla está bloqueada en un dispositivo móvil. Sin embargo, solo se puede ejecutar una sesión de este tipo por dispositivo. Una sesión de ejecución extendida de seguimiento de ubicación solo se puede solicitar en primer plano y la aplicación debe estar en estado **En ejecución**. Esto garantiza que el usuario es consciente de que la aplicación ha iniciado una sesión de seguimiento de ubicación extendida. Es posible usar el ubicador geográfico mientras la aplicación está en segundo plano mediante el uso de una tarea en segundo plano, o un servicio de aplicaciones, sin solicitar un seguimiento de la ubicación de sesión de ejecución ampliada.

## <a name="save-critical-data-locally"></a>Guardar datos críticos localmente

Especifica **ExtendedExecutionReason.SavingData** cuando crees una clase **ExtendedExecutionSession** para guardar los datos de usuario, si el hecho de no guardar los datos antes de que finalice la aplicación va a causar la pérdida de datos y una experiencia de usuario negativa.

No uses este tipo de sesión para extender el tiempo que tarda una aplicación en cargar o descargar datos. Si necesitas cargar datos, solicita una [transferencia en segundo plano](../networking/background-transfers.md) o registra el objeto **MaintenanceTrigger** para controlar la transferencia cuando exista corriente alterna disponible. Una sesión de ejecución extendida **ExtendedExecutionReason.SavingData** se puede solicitar cuando la aplicación está en primer plano y con el estado **En ejecución**, o bien en segundo plano y con el estado **Suspendiendo**.

El estado **Suspendiendo** es la última oportunidad durante el ciclo de vida de la aplicación que una aplicación puede funcionar antes de cerrarse. **ExtendedExecutionReason. SavingData** es el único tipo de **ExtendedExecutionSession** que se puede solicitar en el estado de **suspensión** . Solicitar una sesión de ejecución extendida **ExtendedExecutionReason.SavingData** mientras la aplicación está en estado **Suspendiendo** causa un problema potencial que debes tener en cuenta. Si se solicita una sesión de ejecución extendida durante el estado **Suspendiendo** y el usuario solicita que se vuelva a iniciar la aplicación, puede parecer que tarda mucho tiempo en iniciarse. La causa es que el período de tiempo de la sesión de ejecución extendida debe finalizar para que la antigua instancia de la aplicación se pueda cerrar y se pueda iniciar una nueva. Para garantizar que no se pierda el estado de usuario se sacrifica el tiempo de rendimiento de inicio.

## <a name="request-disposal-and-revocation"></a>Solicitud, eliminación y revocación

Existen tres interacciones fundamentales con una sesión de ejecución extendida: la solicitud, la eliminación y la revocación.  La realización de la solicitud se muestra en el siguiente fragmento de código.

### <a name="request"></a>Solicitud

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```
[Consulta el ejemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

La llamada a **RequestExtensionAsync** comprueba con el sistema operativo si el usuario ha aprobado la actividad en segundo plano de la aplicación y si el sistema tiene los recursos disponibles para permitir la ejecución en segundo plano. Solo se aprobará una sesión para una aplicación en cualquier momento, lo que provocará llamadas adicionales a **RequestExtensionAsync** para dar lugar a la denegación de la sesión.

Puedes comprobar la clase [BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) con antelación para determinar la enumeración [BackgroundAccessStatus](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus?f=255&MSPPError=-2147217396), que es la configuración de usuario que indica si la aplicación puede ejecutarse en segundo plano. Para obtener más información sobre estas configuraciones de usuario, consulta [Battery awareness and background activity](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97) (Reconocimiento de batería y actividad en segundo plano).

La enumeración **ExtendedExecutionReason** indica la operación que tu aplicación está realizando en segundo plano. La cadena **Description** es una cadena en lenguaje natural que explica por qué la aplicación tiene que realizar la operación. Esta cadena no se presenta al usuario, pero puede estar disponible en una versión futura de Windows. El controlador de eventos **Revoked** es necesario para que una sesión de ejecución extendida pueda detener correctamente si el usuario (o el sistema) decide que la aplicación ya no se puede ejecutar en segundo plano.

### <a name="revoked"></a>Revocado

Si una aplicación tiene una sesión de ejecución extendida activa y el sistema requiere que se detenga la actividad en segundo plano porque una aplicación de primer plano requiere los recursos, se revoca la sesión. El período de tiempo de una sesión de ejecución extendida nunca termina sin activar primero el controlador de eventos **Revoked**.

Si el evento **Revoked** se activa para una sesión de ejecución extendida **ExtendedExecutionReason.SavingData**, la aplicación tiene un segundo para completar la operación que estaba ejecutando y finalizar el estado **Suspendiendo**.

La revocación puede producirse por muchas razones: se alcanzó el límite de tiempo de una ejecución, se alcanzó una cuota de energía en segundo plano o es necesario reclamar memoria para que el usuario pueda abrir una nueva aplicación en primer plano.

Este es un ejemplo de un controlador de eventos Revoked:

```cs
private async void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        switch (args.Reason)
        {
            case ExtendedExecutionRevokedReason.Resumed:
                rootPage.NotifyUser("Extended execution revoked due to returning to foreground.", NotifyType.StatusMessage);
                break;

            case ExtendedExecutionRevokedReason.SystemPolicy:
                rootPage.NotifyUser("Extended execution revoked due to system policy.", NotifyType.StatusMessage);
                break;
        }

        EndExtendedExecution();
    });
}
```
[Consulta el ejemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Dispose

El paso final es la eliminación de la sesión de ejecución extendida. Debes eliminar la sesión y cualquier otro activo que consuma mucha memoria, ya que, de lo contrario, la energía que usa la aplicación mientras espera que la sesión se cierre se contará en la cuota de energía de la aplicación. Para conservar la mayor cuota de energía posible para la aplicación, es importante eliminar la sesión al terminar el trabajo de la sesión para que la aplicación pueda cambiar al estado **Suspendido** más rápidamente.

Si eliminas la sesión tú mismo en lugar de esperar el evento de revocación, se reduce el uso de la cuota de energía de la aplicación. Esto significa que la aplicación podrá ejecutarse en segundo plano más tiempo en futuras sesiones, dado que tendrás más cuota de energía disponible para hacerlo. Debes mantener una referencia al objeto **ExtendedExecutionSession** hasta el final de la operación para poder llamar a su método **Dispose**.

A continuación se muestra un fragmento de código que cancela una sesión de ejecución extendida:

```cs
void ClearExtendedExecution(ExtendedExecutionSession session)
{
    if (session != null)
    {
        session.Revoked -= SessionRevoked;
        session.Dispose();
        session = null;
    }
}
```
[Consulta el ejemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

Una aplicación solo puede tener un objeto **ExtendedExecutionSession** activo a la vez. Muchas aplicaciones usan tareas asincrónicas para completar operaciones complejas que requieren acceso a recursos, tales como los de almacenamiento, red o servicios basados en red. Si una operación requiere varias tareas asincrónicas para completarse, el estado de cada una de estas tareas debe tener en cuenta antes de eliminar el objeto **ExtendedExecutionSession** y de permitir que la aplicación se suspenda. Esto requiere el recuento de referencia del número de tareas que aún se están ejecutando y que no se elimine la sesión hasta que dicho valor sea cero.

A continuación se muestra código de ejemplo para administrar varias tareas durante un período de sesión de ejecución extendida. Para obtener más información sobre cómo usar esto en la aplicación, vea el ejemplo de código vinculado a continuación:

```cs
static class ExtendedExecutionHelper
{
    private static ExtendedExecutionSession session = null;
    private static int taskCount = 0;

    public static bool IsRunning
    {
        get
        {
            if (session != null)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked, String description)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = reason;
        newSession.Description = description;
        newSession.Revoked += SessionRevoked;

        // Add a revoked handler provided by the app in order to clean up an operation that had to be halted prematurely
        if(revoked != null)
        {
            newSession.Revoked += revoked;
        }

        ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

        switch (result)
        {
            case ExtendedExecutionResult.Allowed:
                session = newSession;
                break;
            default:
            case ExtendedExecutionResult.Denied:
                newSession.Dispose();
                break;
        }
        return result;
    }

    public static void ClearSession()
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }

        taskCount = 0;
    }

    public static Deferral GetExecutionDeferral()
    {
        if (session == null)
        {
            throw new InvalidOperationException("No extended execution session is active");
        }

        taskCount++;
        return new Deferral(OnTaskCompleted);
    }

    private static void OnTaskCompleted()
    {
        if (taskCount > 0)
        {
            taskCount--;
        }
        
        //If there are no more running tasks than end the extended lifetime by clearing the session
        if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        //The session has been prematurely revoked due to system constraints, ensure the session is disposed
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
        
        taskCount = 0;
    }
}
```
[Consulta el ejemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>Asegúrate de que la aplicación usa bien los recursos

El ajuste de uso de memoria y energía de la aplicación es esencial para garantizar que el sistema operativo permitirá que la aplicación siga ejecutándose cuando deje de ser la aplicación en primer plano. Usa las [API de administración de memoria](/uwp/api/windows.system.memorymanager) para ver cuánta memoria está usando la aplicación. Cuando más memoria use la aplicación, más difícil será para el sistema operativo mantener la aplicación en ejecución cuando otra aplicación esté en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.

Usa [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario.

## <a name="see-also"></a>Vea también

[Muestra de ejecución extendida](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[Ciclo de vida de la aplicación](./app-lifecycle.md)  
[Ciclo de vida de la aplicación: mantener las aplicaciones activas con tareas en segundo plano y ejecución](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) 
 extendida [Administración de memoria en segundo plano](./reduce-memory-usage.md)  
[Transferencias en segundo plano](../networking/background-transfers.md)  
[Detección de la batería y actividad en segundo plano](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[Clase MemoryManager](/uwp/api/windows.system.memorymanager)  
[Reproducir contenido multimedia en segundo plano](../audio-video-camera/background-audio.md)
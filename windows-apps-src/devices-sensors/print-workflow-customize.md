---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Personalizar el flujo de trabajo de impresión
description: Cree experiencias de flujo de trabajo de impresión personalizadas para satisfacer las necesidades de su organización.
ms.date: 07/03/2020
ms.topic: article
keywords: Windows 10, UWP, impresión
ms.localizationpriority: medium
ms.openlocfilehash: 779965ca46efe7fb63adac46ef2568c2ecff66f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172199"
---
# <a name="customize-the-print-workflow"></a>Personalizar el flujo de trabajo de impresión

## <a name="overview"></a>Información general

Los desarrolladores pueden personalizar la experiencia de impresión del flujo de trabajo mediante el uso de una aplicación de flujo de trabajo de impresión. Las aplicaciones de flujo de trabajo de impresión son aplicaciones para UWP que amplían la funcionalidad de [Microsoft Store aplicaciones de dispositivos (WSDAs)](/windows-hardware/drivers/devapps/), por lo que le resultará útil tener cierta familiaridad con WSDAs antes de continuar.

Al igual que en el caso de WSDAs, cuando el usuario de una aplicación de origen elige imprimir algo y navega por el cuadro de diálogo Imprimir, el sistema comprueba si una aplicación de flujo de trabajo está asociada a esa impresora. Si es así, se inicia la aplicación de flujo de trabajo de impresión (principalmente como una tarea en segundo plano; más información a continuación). Una aplicación de flujo de trabajo puede modificar la solicitud de impresión (el documento XML que configura los valores del dispositivo de impresora para la tarea de impresión actual) y el contenido XPS real que se va a imprimir. Opcionalmente, puede exponer esta funcionalidad al usuario mediante el inicio de una interfaz de usuario a lo largo del proceso. Después de realizar su trabajo, pasa el contenido de impresión y el vale de impresión al controlador.

Dado que implica componentes de segundo plano y de primer plano, y dado que está funcionalmente acoplada a otras aplicaciones, una aplicación de flujo de trabajo de impresión puede ser más complicada de implementar que otras categorías de aplicaciones para UWP. Se recomienda que inspeccione el [ejemplo de aplicación de flujo de trabajo](https://github.com/Microsoft/print-oem-samples) durante la lectura de esta guía para comprender mejor cómo se pueden implementar las distintas características. Algunas características, como las distintas comprobaciones de errores y la administración de la interfaz de usuario, no están presentes en esta guía por motivos de simplicidad.

## <a name="getting-started"></a>Introducción

La aplicación de flujo de trabajo debe indicar su punto de entrada al sistema de impresión para que se pueda iniciar en el momento adecuado. Esto se hace insertando la siguiente declaración en el `Application/Extensions` elemento del archivo *Package. appxmanifest* del proyecto de UWP.

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT]
> Hay muchos escenarios en los que la personalización de impresión no requiere la intervención del usuario. Por esta razón, las aplicaciones de flujo de trabajo de impresión se ejecutan como tareas en segundo plano de forma predeterminada.

Si una aplicación de flujo de trabajo está asociada a la aplicación de origen que inició el trabajo de impresión (consulte la sección más adelante para obtener instrucciones sobre esto), el sistema de impresión examina los archivos de manifiesto para ver si hay un punto de entrada de tarea en segundo plano.

## <a name="do-background-work-on-the-print-ticket"></a>Realizar trabajo en segundo plano en la solicitud de impresión

Lo primero que hace el sistema de impresión con la aplicación de flujo de trabajo es activar su tarea en segundo plano (en este caso, la `WfBackgroundTask` clase en el `WFBackgroundTasks` espacio de nombres). En el método de la tarea en segundo plano `Run` , debe convertir los detalles del desencadenador de la tarea como una instancia de **[PrintWorkflowTriggerDetails](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** . Esto proporcionará la funcionalidad especial para una tarea en segundo plano de flujo de trabajo de impresión. Expone la propiedad **[PrintWorkflowSession](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** , que es una instancia de **[PrintWorkFlowBackgroundSession](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)**. Clases de sesión de flujo de trabajo de impresión: las variedades de fondo y de primer plano: controlará los pasos secuenciales de la aplicación de flujo de trabajo de impresión.

A continuación, registre los métodos de controlador para los dos eventos que generará esta clase de sesión. Definirá estos métodos más adelante.

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

Cuando `Start` se llama al método, el administrador de sesión generará el evento **[SetupRequested](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** en primer lugar. Este evento expone información general sobre la tarea de impresión, así como la solicitud de impresión. En esta fase, la solicitud de impresión se puede editar en segundo plano.

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

Lo importante es que el control del **SetupRequested** de la aplicación determine si se va a iniciar un componente de primer plano. Esto podría depender de una configuración que se guardó anteriormente en el almacenamiento local o de un evento que se produjo durante la edición de la solicitud de impresión, o puede ser una configuración estática de la aplicación en particular.

```csharp
// ...

if (UIrequested) {
    printTaskSetupArgs.SetRequiresUI();

    // Any data that is to be passed to the foreground task must be stored the app's local storage.
    // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
    // it can be identified as pertaining to this workflow app session.
}

// Complete the deferral taken out at the start of OnSetupRequested
setupRequestedDeferral.Complete();
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Realizar trabajo en primer plano en el trabajo de impresión (opcional)

Si se llamó al método **[SetRequiresUI](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** , el sistema de impresión examinará el archivo de manifiesto para el punto de entrada a la aplicación de primer plano. El `Application/Extensions` elemento del archivo *Package. appxmanifest* debe tener las líneas siguientes. Reemplace el valor de `EntryPoint` por el nombre de la aplicación en primer plano.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" />
```

Después, el sistema de impresión llama al método **onactivated** para el punto de entrada de la aplicación especificado. En el método **onactivated** de su archivo _app.Xaml.CS_ , la aplicación de flujo de trabajo debe comprobar el tipo de activación para comprobar que es una activación del flujo de trabajo. Si es así, la aplicación de flujo de trabajo puede convertir los argumentos de activación a un objeto **[PrintWorkflowUIActivatedEventArgs](/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** , que expone un objeto **[PrintWorkflowForegroundSession](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** como una propiedad. Este objeto, como su homólogo de fondo en la sección anterior, contiene los eventos que genera el sistema de impresión y puede asignar controladores a estos. En este caso, la funcionalidad de control de eventos se implementará en una clase independiente denominada `WorkflowPage` .

En primer lugar, en el archivo _app.Xaml.CS_ :

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

Una vez que la interfaz de usuario tiene controladores de eventos adjuntos y el método **onactivated** ha salido, el sistema de impresión activará el evento **[SetupRequested](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** para que la interfaz de usuario se controle. Este evento proporciona los mismos datos que el evento de configuración de tareas en segundo plano proporcionado, incluidos el documento de impresión y la información del trabajo de impresión, pero sin la capacidad de solicitar el inicio de la interfaz de usuario adicional. En el archivo _WorkflowPage.Xaml.CS_ :

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);

    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

Después, el sistema de impresión generará el evento **[XpsDataAvailable](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** para la interfaz de usuario. En el controlador de este evento, la aplicación de flujo de trabajo puede tener acceso a todos los datos disponibles para el evento de instalación y, además, puede leer los datos XPS directamente, ya sea como un flujo de bytes sin formato o como un modelo de objetos. El acceso a los datos XPS permite que la interfaz de usuario proporcione servicios de vista previa de impresión y proporcione información adicional al usuario sobre las operaciones que la aplicación de flujo de trabajo ejecutará en los datos.

Como parte de este controlador de eventos, la aplicación de flujo de trabajo debe adquirir un objeto de aplazamiento si seguirá interactuando con el usuario. Sin un aplazamiento, el sistema de impresión considerará que la tarea de la interfaz de usuario se completa cuando el controlador de eventos **XpsDataAvailable** se cierra o cuando llama a un método asincrónico. Cuando la aplicación ha recopilado toda la información necesaria de la interacción del usuario con la interfaz de usuario, debe completar el aplazamiento para que el sistema de impresión pueda avanzar.

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent();

    IInputStream inputStream = xpsStream.GetInputSpoolStream();

    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        // Read the XPS data from input stream
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength];
        while (inputReader.UnconsumedBufferLength > 0)
        {
            inputReader.ReadBytes(xpsData);
            // Do something with the XPS data, e.g. preview
            // ...
        }
    }

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

Además, la instancia de **[PrintWorkflowSubmittedOperation](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** expuesta por los argumentos del evento ofrece la opción de cancelar el trabajo de impresión o indicar que el trabajo se ha realizado correctamente, pero que no se necesitará ningún trabajo de impresión de salida. Esto se hace llamando al método **[Complete](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** con un valor de **[PrintWorkflowSubmittedStatus](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** .

> [!NOTE]
> Si la aplicación de flujo de trabajo cancela el trabajo de impresión, se recomienda encarecidamente que proporcione una notificación del sistema que indique por qué se canceló el trabajo.

## <a name="do-final-background-work-on-the-print-content"></a>Realizar trabajo final en segundo plano en el contenido de impresión

Una vez que la interfaz de usuario ha completado el aplazamiento en el evento **PrintTaskXpsDataAvailable** (o si se ha omitido el paso de la interfaz de usuario), el sistema de impresión activará el evento **[enviado](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** para la tarea en segundo plano. En el controlador de este evento, la aplicación de flujo de trabajo puede obtener acceso a todos los datos proporcionados por el evento **XpsDataAvailable** . Sin embargo, a diferencia de los eventos anteriores, los **enviados** también proporcionan acceso de *escritura* al contenido final del trabajo de impresión a través de una instancia de **[PrintWorkflowTarget](/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** .

El objeto que se usa para poner en cola los datos para la impresión final depende de si se tiene acceso a los datos de origen como un flujo de bytes sin formato o como modelo de objetos XPS. Cuando la aplicación de flujo de trabajo accede a los datos de origen a través de una secuencia de bytes, se proporciona una secuencia de bytes de salida para escribir los datos de trabajo finales en. Cuando la aplicación de flujo de trabajo accede a los datos de origen a través del modelo de objetos, se proporciona un escritor de documentos para escribir objetos en el trabajo de salida. En cualquier caso, la aplicación de flujo de trabajo debe leer todos los datos de origen, modificar los datos necesarios y escribir los datos modificados en el destino de salida.

Cuando la tarea en segundo plano termina de escribir los datos, debe llamar a **Complete** en el objeto **PrintWorkflowSubmittedOperation** correspondiente. Una vez que la aplicación de flujo de trabajo completa este paso y se cierra el controlador de eventos **enviado** , la sesión de flujo de trabajo se cierra y el usuario puede supervisar el estado del trabajo de impresión final a través de los cuadros de diálogo de impresión estándar.

## <a name="final-steps"></a>Pasos finales

### <a name="register-the-print-workflow-app-to-the-printer"></a>Registrar la aplicación de flujo de trabajo de impresión en la impresora

La aplicación de flujo de trabajo está asociada a una impresora con el mismo tipo de envío de archivos de metadatos que para WSDAs. De hecho, una sola aplicación de UWP puede actuar como una aplicación de flujo de trabajo y un WSDA que proporciona la funcionalidad de configuración de la tarea de impresión. Siga los [pasos de WSDA correspondientes para crear la Asociación de metadatos](/windows-hardware/drivers/devapps/step-2--create-device-metadata).

La diferencia es que mientras que los WSDAs se activan automáticamente para el usuario (la aplicación siempre se iniciará cuando el usuario imprima en el dispositivo asociado), las aplicaciones de flujo de trabajo no lo son. Tienen una directiva independiente que debe establecerse.

### <a name="set-the-workflow-apps-policy"></a>Establecimiento de la Directiva de la aplicación de flujo de trabajo

La Directiva de aplicación de flujo de trabajo se establece mediante comandos de PowerShell en el dispositivo que va a ejecutar la aplicación de flujo de trabajo. Los comandos SET-Printer, Add-Printer (puerto existente) y Add-Printer (nuevo puerto WSD) se modificarán para permitir que se establezcan directivas de flujo de trabajo.

* `Disabled`: Las aplicaciones de flujo de trabajo no se activarán.
* `Uninitialized`: Las aplicaciones de flujo de trabajo se activarán si el flujo de trabajo DCA está instalado en el sistema. Si la aplicación no está instalada, la impresión continuará.
* `Enabled`: El contrato de flujo de trabajo se activará si el flujo de trabajo DCA está instalado en el sistema. Si la aplicación no está instalada, se producirá un error en la impresión.

El comando siguiente hace que la aplicación de flujo de trabajo sea necesaria en la impresora especificada.

```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy Enabled
```

Un usuario local puede ejecutar esta directiva en una impresora local o, en el caso de la implementación empresarial, el administrador de la impresora puede ejecutar esta directiva en el servidor de impresión. La Directiva se sincronizará a continuación en todas las conexiones de cliente. El administrador de la impresora puede usar esta Directiva cada vez que se agrega una nueva impresora.

## <a name="see-also"></a>Vea también

[Ejemplo de aplicación de flujo de trabajo](https://github.com/Microsoft/print-oem-samples)

[Windows. Graphics. Printing. Workflow (espacio de nombres)](/uwp/api/windows.graphics.printing.workflow)
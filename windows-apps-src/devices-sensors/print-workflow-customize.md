---
author: PatrickFarley
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Personalizar el flujo de trabajo de impresión
description: Crea experiencias personalizadas del flujo de trabajo de impresión para satisfacer las necesidades de tu organización.
ms.author: pafarley
ms.date: 08/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, impresión
ms.localizationpriority: medium
ms.openlocfilehash: 9e53c15b01a08c8c617529fe074929ce89a68ce9
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4952710"
---
# <a name="customize-the-print-workflow"></a>Personalizar el flujo de trabajo de impresión

## <a name="overview"></a>Información general
Los desarrolladores pueden personalizar la experiencia del flujo de trabajo de impresión mediante el uso de una aplicación de flujo de trabajo de impresión. Las aplicaciones de flujo de trabajo de impresión son aplicaciones para UWP que expanden las funciones de las [aplicaciones para dispositivos de Microsoft Store (WSDA)](https://docs.microsoft.com/windows-hardware/drivers/devapps/), de modo que es útil tener ciertos conocimientos sobre las WSDAs antes de continuar. 

Al igual que sucede con las WSDA, cuando el usuario de una aplicación de origen opta por imprimir algo y se desplaza por el cuadro de diálogo de impresión, el sistema comprueba si hay una aplicación de flujo de trabajo asociada con la impresora. Si es así, se inicia dicha aplicación de flujo de trabajo de impresión (principalmente como una tarea en segundo plano; encontrarás más información sobre este tema más adelante). Una aplicación de flujo de trabajo es capaz de alterar tanto el vale de impresión (el documento XML que configura las opciones de la impresora para la tarea de impresión actual) como el contenido XPS real que va a imprimirse. Como opción, puede exponer esta funcionalidad al usuario iniciando una interfaz de usuario en mitad del proceso. Tras realizar su trabajo, pasa al controlador el contenido de impresión y el vale de impresión.

Como implica componentes en primer y en segundo plano, y además su funcionalidad está asociada a otras aplicaciones, una aplicación de flujo de trabajo de impresión puede ser más complicada de implementar que otras categorías de aplicaciones para UWP. Te recomendamos que inspecciones la [aplicación de flujo de trabajo de ejemplo](https://github.com/Microsoft/print-oem-samples) al leer esta guía para comprender mejor cómo se pueden implementar las diferentes características. Algunas características, como diversas comprobaciones de errores y la administración de la interfaz de usuario, no se muestran en esta guía para mantener su sencillez.

## <a name="getting-started"></a>Introducción

La aplicación de flujo de trabajo debe indicar al sistema de impresión su punto de entrada para poder iniciarla en el momento adecuado. Para ello, inserta la siguiente declaración en el elemento `Application/Extensions` del archivo *package.appxmanifest* del proyecto para la UWP . 

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT] 
> Existen muchos escenarios en los que la personalización de la impresión no requiere ninguna entrada del usuario. Por este motivo, las aplicaciones de flujo de trabajo de impresión se ejecutan como tareas en segundo plano de forma predeterminada.

Si una aplicación de flujo de trabajo está asociada con la aplicación de origen que inició el trabajo de impresión (consulta la sección posterior para obtener instrucciones sobre esto), el sistema de impresión examina sus archivos de manifiesto en busca de un punto de entrada de tarea en segundo plano.

## <a name="do-background-work-on-the-print-ticket"></a>Realización de trabajo en segundo plano en el vale de impresión

Lo primero que hace el sistema de impresión con la aplicación de flujo de trabajo es activar su tarea en segundo plano (en este caso, la clase `WfBackgroundTask` en el espacio de nombres `WFBackgroundTasks`). En el método `Run` de la tarea en segundo plano, debes transmitir los detalles del desencadenador de la tarea como una instancia de **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)**. Esto proporcionará la funcionalidad especial para una tarea en segundo plano del flujo de trabajo de impresión. Expone la propiedad **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)**, es una instancia de **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)**. Las clases de la sesión de flujo de trabajo de impresión (las variedades en primer plano y en segundo plano) controlarán los pasos de la secuencia de la aplicación de flujo de trabajo de impresión. 

A continuación registra los métodos de controlador para los dos eventos que generará esta clase de sesión. Estos métodos se definen más adelante.

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

Cuando se llama al método `Start`, el administrador de sesiones generará primero el evento **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)**. Este evento expone información general sobre la tarea de impresión, así como el vale de impresión. En esta fase, el vale de impresión se puede editar en segundo plano. 

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

Es importante tener en cuenta que es en el control de **SetupRequested** donde la aplicación determinará si se debe iniciar un componente en primer plano. Esto podría depender de una configuración que se haya guardado anteriormente en el almacenamiento local o de un evento que se haya producido durante la edición del vale de impresión, o podría ser una configuración estática de esa aplicación en concreto.

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
}
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Realización de trabajo en primer plano en el trabajo de impresión (opcional)

Si SetRequiresUI se ha llamado al método **[](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)**, el sistema de impresión examinará el archivo de manifiesto en busca del punto de entrada para la aplicación en primer plano. El elemento `Application/Extensions` de tu archivo *package.appxmanifest* debe tener las siguientes líneas. Sustituya el valor de `EntryPoint` por el nombre de la aplicación en primer plano.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" /> 
```

A continuación, el sistema de impresión llama al método **OnActivated** para el punto de entrada de la aplicación concreto. En el método **OnActivated** de su archivo _App.xaml.cs_, la aplicación de flujo de trabajo debe comprobar el tipo de activación para comprobar que se trata de una activación de flujo de trabajo. Si es así, la aplicación de flujo de trabajo puede transmitir los argumentos de activación a un objeto **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)**, que expone un objeto **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** como una propiedad. Este objeto, como su equivalente en segundo plano de la sección anterior, contiene eventos generados por el sistema de impresión, a los que puedes asignarles controladores. En este caso, la funcionalidad de control de eventos se implementará en una clase distinta llamada `WorkflowPage`.

En primer lugar, en el archivo _App.xaml.cs_:

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

Una vez que la interfaz de usuario ha adjuntado controladores de eventos y el método **OnActivated** ha salido, el sistema de impresión activará el evento **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** para que la interfaz de usuario lo controle. Este evento proporciona los mismos datos que ha proporcionado el evento de configuración de la tarea en segundo plano, lo que incluye la información del trabajo de impresión y el documento del vale de impresión, pero sin la capacidad de solicitar el inicio de una interfaz de usuario adicional. En el archivo _WorkflowPage.xaml.cs_:

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

A continuación, el sistema de impresión generará el evento **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** para la interfaz de usuario. En el controlador de este evento, la aplicación de flujo de trabajo puede tener acceso a todos los datos disponibles para el evento de configuración y además puede leer los datos XPS directamente, como una secuencia de bytes sin procesar o como un modelo de objetos. El acceso a los datos XPS permite a la interfaz de usuario proporcionar servicios de vista previa de impresión y suministrar información adicional al usuario acerca de las operaciones que la aplicación de flujo de trabajo ejecutará en los datos. 

Como parte de este controlador de eventos, la aplicación de flujo de trabajo debe adquirir un objeto de aplazamiento si va a seguir interactuando con el usuario. Sin un aplazamiento, el sistema de impresión considerará que la tarea de la interfaz de usuario se ha completado cuando el controlador de eventos **XpsDataAvailable** salga o cuando llame a un método asincrónico. Cuando la aplicación ha recopilado toda la información necesaria de la interacción del usuario con la interfaz de usuario, debe completar el aplazamiento para que el sistema de impresión pueda avanzar.

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

Además, la instancia de **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** expuesta por los argumentos del evento proporciona la opción de cancelar el trabajo de impresión o de indicar que el trabajo es correcto pero que no se necesitará que ningún trabajo de impresión de salida. Esto se lleva a cabo con una llamada al método **[completado](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** con un valor **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)**.

> [!NOTE]
> Si la aplicación de flujo de trabajo cancela el trabajo de impresión, se recomienda encarecidamente que proporcione una notificación del sistema que indique el motivo de la cancelación de la tarea. 


## <a name="do-final-background-work-on-the-print-content"></a>Realización del trabajo en segundo plano final en el contenido de impresión

Cuando la interfaz de usuario haya finalizado el aplazamiento en el evento **PrintTaskXpsDataAvailable** (o si se ha omitido el paso de la interfaz de usuario), el sistema de impresión activará el evento **[Submitted](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** para la tarea en segundo plano. En el controlador de este evento, la aplicación de flujo de trabajo puede tener acceso a los mismos datos proporcionados por el evento **XpsDataAvailable**. Sin embargo, a diferencia de los eventos anteriores, el evento **Submitted** también proporciona acceso de *escritura* al contenido final del trabajo de impresión a través de una instancia de **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)**. 

El objeto usado para poner en cola los datos para la impresión final depende de si se accede a los datos de origen como una secuencia de bytes sin procesar o como el modelo de objetos XPS. Cuando la aplicación de flujo de trabajo tiene acceso a los datos de origen mediante una secuencia de bytes, se proporciona una secuencia de bytes de salida en la que escribir los datos del trabajo final. Cuando la aplicación de flujo de trabajo tiene acceso a los datos de origen mediante el modelo de objetos, se proporciona un escritor de documentos para escribir objetos en el trabajo de salida. En cualquier caso, la aplicación de flujo de trabajo debe leer todos los datos de origen, modificar los datos necesarios y escribir los datos modificados en el destino de la salida.

Cuando la tarea en segundo plano termina de escribir los datos, debe llamar a **Complete** en el objeto **PrintWorkflowSubmittedOperation** correspondiente. Cuando la aplicación de flujo de trabajo haya completado este paso y el controlador de eventos **Submitted** haya salido, se cerrará la sesión del el flujo de trabajo y el usuario podrá supervisar el estado del trabajo de impresión final a través de los cuadros de diálogo de impresión estándar.

## <a name="final-steps"></a>Últimos pasos

### <a name="register-the-print-workflow-app-to-the-printer"></a>Registro de la aplicación de flujo de trabajo de impresión en la impresora

Tu aplicación de flujo de trabajo se asocia con una impresora mediante el mismo tipo de envío de archivos de metadatos que en el caso de las WSDA. De hecho, una sola aplicación para UWP puede actuar como aplicación de flujo de trabajo y también como WSDA que proporcione la funcionalidad de configuración de la tarea de impresión. Sigue los pasos adecuados de [WSDA steps for creating the metadata association](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata) (Pasos de WSDA para la creación de la asociación de metadatos).

La diferencia es que mientras que las WSDA se activan automáticamente para el usuario (la aplicación se iniciará siempre que ese usuario imprima en el dispositivo asociado), las aplicaciones de flujo de trabajo no lo hacen. Tienen una directiva independiente que se debe establecer.

### <a name="set-the-workflow-apps-policy"></a>Establecimiento de la directiva de la aplicación de flujo de trabajo
La directiva de la aplicación de flujo de trabajo se establece con comandos de Powershell en el dispositivo que ejecutará dicha aplicación. Se modificarán los comandos Set-Printer, Add-Printer (puerto existente) y Add-Printer (puerto nuevo de WSD) para permitir que se establezcan directivas de flujo de trabajo. 
* `Disabled`: Las aplicaciones de flujo de trabajo no se activarán.
* `Uninitialized`: Las aplicaciones de flujo de trabajo se activarán si el DCA del flujo de trabajo está instalado en el sistema. La impresión seguirá adelante aunque la aplicación no esté instalada. 
* `Enabled`: El contrato de flujo de trabajo se activará si el DCA del flujo de trabajo está instalado en el sistema. Si la aplicación no está instalada, la impresión no se realizará. 

El siguiente comando hace que la aplicación de flujo de trabajo sea necesaria en la impresora especificada.
```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy On
```

Un usuario local puede ejecutar esta directiva en una impresora local o, en el caso de su implementación empresarial, el administrador de la impresora puede ejecutar esta directiva en el servidor de impresión. Luego la directiva se sincronizará con todas las conexiones de cliente. El administrador de la impresora puede usar esta directiva siempre que se agregue una nueva impresora.

## <a name="see-also"></a>Véase también

[Aplicación de flujo de trabajo de ejemplo](https://github.com/Microsoft/print-oem-samples)

[Espacio de nombres Windows.Graphics.Printing.Workflow](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)



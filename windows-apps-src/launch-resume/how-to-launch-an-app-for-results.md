---
title: Iniciar una aplicación para obtener resultados
description: Aprende a iniciar una aplicación desde otra aplicación y a intercambiar datos entre las dos. Esto se denomina iniciar una aplicación para obtener resultados.
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fa018920f069c0b4f1d963c6cdfd3213df08fb45
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158770"
---
# <a name="launch-an-app-for-results"></a>Iniciar una aplicación para obtener resultados




**API importantes**

-   [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)
-   [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)

Aprende a iniciar una aplicación desde otra aplicación y a intercambiar datos entre las dos. Esto se denomina *iniciar una aplicación para obtener resultados*. En el siguiente ejemplo se muestra cómo usar [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) para iniciar una aplicación para obtener resultados.

Las nuevas API de comunicación entre aplicaciones de Windows 10 hacen que las aplicaciones de Windows (y las aplicaciones web de Windows) puedan iniciar una aplicación e intercambiar archivos y datos. Esto te permite crear soluciones combinadas a partir de varias aplicaciones. Con estas nuevas API, las tareas complejas que hubieran requerido que el usuario usara varias aplicaciones ahora se pueden controlar sin problemas. Por ejemplo, la aplicación podría iniciar una aplicación de redes sociales para elegir un contacto o iniciar una aplicación de confirmación de compra para completar un proceso de pago.

La aplicación que inicias para obtener resultados se denomina la aplicación iniciada. La aplicación que inicia la aplicación se denomina aplicación que llama. Para este ejemplo escribirás tanto la aplicación que llama como la aplicación iniciada.

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>Paso 1: registrar el protocolo que se controlará en la aplicación que se iniciará para obtener resultados


En el archivo package. appxmanifest de la aplicación iniciada, agregue una extensión de protocolo a la sección de la ** &lt; aplicación &gt; ** . En el siguiente ejemplo se usa un protocolo ficticio denominado **test-app2app**.

El atributo **ReturnResults** de la extensión de protocolo acepta uno de estos valores:

-   **optional**: es posible iniciar la aplicación para obtener resultados mediante el método [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) o para no obtenerlos, con [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync). Cuando se usa **optional**, la aplicación iniciada debe determinar si se inició para obtener resultados. Puede hacerlo si comprueba el argumento del evento [**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated). Si la propiedad [**IActivatedEventArgs.Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) del argumento devuelve [**ActivationKind.ProtocolForResults**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)o si el tipo del argumento del evento es [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs), la aplicación se inició mediante **LaunchUriForResultsAsync**.
-   **always**: la aplicación solo se puede iniciar para obtener resultados, es decir, solo puede responder a [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync).
-   **none**: la aplicación no se puede iniciar para obtener resultados; solo puede responder a [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync).

En este ejemplo de extensión de protocolo, la aplicación solo puede iniciarse para obtener resultados. Esto simplifica la lógica dentro del método **OnActivated**, que se explica a continuación, ya que solo hay que controlar el caso "iniciado para obtener resultados" y no el resto de maneras mediante las que se pudo activar la aplicación.

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>Paso 2: Reemplazar Application.OnActivated en la aplicación que iniciarás para obtener resultados


Si este método ya no existe en la aplicación iniciada, créalo dentro de la clase `App` definida en App.xaml.cs.

En una aplicación que te permite elegir a tus amigos en una red social, esta función puede ser donde abres la página del selector de contactos. En el siguiente ejemplo, se muestra una página llamada **LaunchedForResultsPage** cuando se activa la aplicación para obtener resultados. Asegúrate de que se incluya la instrucción **using** en la parte superior del archivo.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

Como la extensión de protocolo del archivo Package.appxmanifest especifica **ReturnResults** como **always**, el código que se acaba de mostrar puede convertir `args` directamente en [**ProtocolForResultsActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolForResultsActivatedEventArgs) con la seguridad que solamente se enviará **ProtocolForResultsActivatedEventArgs** a **OnActivated** para esta aplicación. Si la aplicación se puede activar de otras maneras distintas de para la obtención de resultados, puedes comprobar si la propiedad [**IActivatedEventArgs.Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) devuelve [**ActivationKind.ProtocolForResults**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) para indicar si la aplicación se inició para obtener resultados.

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>Paso 3: agregar un campo ProtocolForResultsOperation a la aplicación que se inicia para obtener resultados


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

El campo [**ProtocolForResultsOperation**](/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation) se usará para indicar el momento en que la aplicación iniciada esté lista para devolver el resultado a la aplicación que llama. En este ejemplo, el campo se agrega a la clase **LaunchedForResultsPage** porque se completará la operación de inicio para obtener resultados desde esa página y se necesitará tener acceso a él.

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>Paso 4: reemplazar OnNavigatedTo() en la aplicación que se inicia para obtener resultados


Reemplaza el método [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) en la página que se mostrará cuando se inicie la aplicación para obtener los resultados. Si este método aún no existe, créalo dentro de la clase de la página definida en &lt;nombrepágina&gt;.xaml.cs. Asegúrate de que se incluye la siguiente instrucción **using** en la parte superior del archivo:

```cs
using Windows.ApplicationModel.Activation
```

El objeto [**NavigationEventArgs**](/uwp/api/Windows.UI.Xaml.Navigation.NavigationEventArgs) del método [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) contiene los datos pasados desde la aplicación que llama. Los datos no puede superar los 100 KB y se almacenan en un objeto [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet).

En el código de ejemplo siguiente, la aplicación iniciada espera que los datos enviados desde la aplicación que llama estén en un elemento [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet), en una clave denominada **TestData**, ya que así es como se ha codificado que realice el envío la aplicación que llama del ejemplo.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>Paso 5: escribir código para devolver datos a la aplicación que llama


En la aplicación iniciada, usa [**ProtocolForResultsOperation**](/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation) para devolver los datos a la aplicación que llama. En el código de ejemplo siguiente, se crea un objeto [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet) que contiene el valor que se devolverá a la aplicación que llama. Después, se usa el campo **ProtocolForResultsOperation** para enviar el valor a la aplicación que llama.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>Paso 6: escribir código para iniciar la aplicación para obtener resultados y obtener los datos devueltos


Inicia la aplicación desde dentro de un método asincrónico en la aplicación que llama tal y como se muestra en el ejemplo de código siguiente. Observa las instrucciones **using**, que son necesarias compilar el código:

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

En este ejemplo, un elemento [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet) que contiene la clave **TestData** se pasa a la aplicación iniciada. La aplicación iniciada crea un elemento **ValueSet** con una clave denominada **ReturnedData** que contiene el resultado que se devuelve al llamador.

Debes compilar e implementar la aplicación que se iniciará para obtener resultados antes de ejecutar la aplicación que llama. De lo contrario, [**LaunchUriResult.Status**](/uwp/api/Windows.System.LaunchUriStatus) notificará **LaunchUriStatus.AppUnavailable**.

Necesitarás el nombre de familia de la aplicación iniciada cuando establezcas la propiedad [**TargetApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.targetapplicationpackagefamilyname). Una forma de obtener el nombre de familia es realizar la siguiente llamada desde la aplicación iniciada:

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>Observaciones


En el ejemplo de este procedimiento se ofrece una introducción de tipo "hola a todos" a fin de iniciar una aplicación para obtener resultados. Los conceptos clave que se deben tener en cuenta son que la nueva API [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) permite iniciar una aplicación de manera asincrónica y comunicarse a través de la clase [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet). La transmisión de datos a través de un elemento **ValueSet** está limitada a 100 KB. Si necesitas transmitir mayores volúmenes de datos, puedes compartir archivos mediante la clase [**SharedStorageAccessManager**](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) para crear tokens de archivo que se puedan pasar entre aplicaciones. Por ejemplo, si existe un elemento **ValueSet** denominado `inputData`, podrías almacenar el token en un archivo que quieras compartir con la aplicación iniciada:

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

Después, pásalo a la aplicación iniciada mediante **LaunchUriForResultsAsync**.

## <a name="related-topics"></a>Temas relacionados


* [**LaunchUri**](/uwp/api/windows.system.launcher.launchuriasync)
* [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)
* [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)

 

 
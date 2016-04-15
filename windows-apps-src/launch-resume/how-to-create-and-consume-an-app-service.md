---
title: Crear y consumir un servicio de aplicación
description: Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo consumir esos servicios.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# Crear y consumir un servicio de aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo consumir esos servicios.

## Crear un proyecto nuevo de proveedor de servicios de aplicaciones


En este procedimiento lo crearemos todo en una solución para hacerlo más sencillo.

-   En Microsoft Visual Studio 2015, crea un nuevo proyecto de aplicación para UWP y asígnale el nombre AppServiceProvider. (En el cuadro de diálogo **Nuevo proyecto** , selecciona **Plantillas &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Aplicación vacía (Windows Universal)**). Esta será la aplicación que proporciona el servicio de aplicaciones.

## Agregar una extensión de servicio de aplicaciones a package.appxmanifest


En el archivo Package.appxmanifest del proyecto AppServiceProvider, agrega la siguiente extensión de AppService al elemento **&lt;Application&gt;**. Este ejemplo anuncia el servicio `com.Microsoft.Inventory` y es lo que identifica a esta aplicación como un proveedor de servicios de aplicaciones. El servicio real se implementará como una tarea en segundo plano. La aplicación de servicio de aplicaciones expone el servicio a otras aplicaciones. Se recomienda usar un estilo inverso de nombre de dominio para el nombre del servicio.

``` syntax
... 
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

El atributo **Category** identifica esta aplicación como un proveedor de servicios de aplicaciones.

El atributo **EntryPoint** identifica la clase que implementa el servicio, que se implementará a continuación.

## Crear el servicio de aplicaciones


1.  El servicio de aplicaciones se implementa como una tarea en segundo plano. Esto permite que una aplicación en primer plano invoque el servicio de aplicaciones en otra aplicación para realizar tareas en segundo plano. Agrega un nuevo proyecto de componente de Windows Runtime a la solución (**Archivo &gt; Agregar &gt; Nuevo proyecto**) denominado MyAppService. (En el cuadro de diálogo **Agregar nuevo proyecto** elige **Instalado &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Componente de Windows Runtime (Windows universal)**
2.  En el proyecto AppServiceProvider, agrega una referencia al proyecto MyAppService.
3.  En el proyecto MyappService, agrega las siguientes instrucciones **using** a la parte superior de Class1.cs:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Reemplaza el código auxiliar de **Class1** por una nueva clase de tarea en segundo plano denominada **Inventory**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    Esta clase es el lugar donde el servicio de aplicaciones hará su trabajo.

    Se llama a **Run()** cuando se crea la tarea en segundo plano. Como las tareas en segundo plano finalizan cuando se completa la función **Run**, el código toma un aplazamiento para que la tarea en segundo plano se mantenga hasta que atienda las solicitudes.

    Se llama a **OnTaskCanceled()** cuando se cancela la tarea. La tarea se cancela cuando la aplicación cliente desecha el elemento [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704), cuando se suspende la aplicación cliente, cuando el sistema operativo se apaga o se suspende o cuando el sistema operativo se queda sin recursos para ejecutar la tarea.

## Escribir el código para el servicio de aplicaciones


El código para el servicio de aplicaciones se encuentra en **OnRequestedReceived()**. Reemplaza el código auxiliar **OnRequestedReceived()** en el archivo Class1.cs de MyAppService por el código de este ejemplo. Este código obtiene un índice para un elemento de inventario y, junto con una cadena de comandos, lo pasa al servicio para recuperar el nombre y el precio del elemento de inventario especificado. Se ha quitado el código de control de errores por motivos de brevedad.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don&#39;t want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &amp;&amp;
         inventoryIndex.Value >= 0 &amp;&amp;
         inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
}
```

Ten en cuenta que **OnRequestedReceived()** es de tipo **async** porque en este ejemplo se realiza una llamada de método que admite await a [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722).

Se toma un aplazamiento para que el servicio pueda usar métodos **async** en el controlador OnRequestReceived. Esto asegura que la llamada a OnRequestReceived no se completará hasta que termine de procesar el mensaje. [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) se usa para enviar una respuesta junto con la finalización. **SendResponseAsync** no indica la finalización de la llamada. Es la finalización del aplazamiento lo que indica a [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) que se ha completado el procesamiento de OnRequestReceived.

Los servicios de aplicaciones usan un elemento [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) para intercambiar información. El tamaño de los datos que se pueden pasar solo está limitado por los recursos del sistema. No hay claves predefinidas para su uso en el elemento **ValueSet**. Debes determinar qué valores clave usarás para definir el protocolo del servicio de aplicaciones. Debes escribir el llamador teniendo ese protocolo presente. En este ejemplo, hemos elegido una clave denominada "Command" que tiene un valor que indica si queremos que el servicio de aplicaciones proporcione el nombre del elemento de inventario o su precio. El índice del nombre de inventario se almacena en la clave "ID". El valor devuelto se almacena en la clave "Result".

Se devuelve al llamador una enumeración [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) para indicar si la llamada al servicio de aplicaciones se ha realizado correctamente o no. Un ejemplo de cómo podría producirse un error en la llamada al servicio de aplicaciones es si el sistema operativo anula el extremo de servicio, se superan los recursos, etc. Puedes devolver información adicional sobre errores a través de [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). En este ejemplo, usamos una clave denominada "Status" para devolver información más detallada del error al llamador.

La llamada a [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) devuelve el elemento [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) al llamador.

## Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete


Es necesario implementar la aplicación del proveedor del servicio de aplicaciones antes de llamarla desde un cliente. También se necesitará el nombre de familia de paquete de la aplicación de servicio de aplicaciones para poder llamarlo.

-   Una forma de obtener el nombre de familia de paquete de la aplicación de servicio de aplicaciones consiste en llamar a [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) desde el proyecto **AppServiceProvider** (por ejemplo, desde `public App()` en App.xaml.cs) y tomar nota del resultado. Para ejecutar AppServiceProvider en Microsoft Visual Studio, establécelo como proyecto de inicio en la ventana Explorador de soluciones y ejecuta el proyecto.
-   Otra forma de obtener el nombre de familia de paquete consiste en implementar la solución (**Compilar &gt; Implementar solución**) y anotar el nombre completo del paquete de la ventana de salida (**Vista &gt; Salida**). Debes quitar la información de plataforma de la cadena en la ventana de salida para derivar el nombre del paquete. Por ejemplo, si el nombre completo del paquete indicado en la ventana de salida era "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra", se quitaría "1.0.0.0\_x86\_\_", lo que dejaría "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra" como el nombre de familia de paquete.

## Escribir un cliente para llamar al servicio de aplicaciones


1.  Agrega un nuevo proyecto vacío de aplicación universal de Windows a la solución (**Archivo &gt; Agregar &gt; Nuevo proyecto**) denominado ClientApp. (En el cuadro de diálogo **Agregar nuevo proyecto** elige **Instalado &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Aplicación vacía (Windows universal)**).
2.  En el proyecto ClientApp, agrega las siguientes instrucciones **using** a la parte superior de MainPage.xaml.cs:
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  Agregar un cuadro de texto y un botón a MainPage.xaml.
4.  Agrega un botón controlador de clic para el botón y agrega la palabra clave **async** a la firma del controlador del botón.
5.  Reemplaza el código auxiliar del controlador de clic del botón por el siguiente código. Asegúrate de incluir la declaración de campo `inventoryService`.

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider&#39;s Package.appxmanifest file in the &lt;Extension&gt; section. 
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
                return;
            }
        }

        // Call the service.
        int idx = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("Command", "Item");
        message.Add("ID", idx);
        AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
        string result = "";

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data  that the service sent  to us.
            if (response.Message["Status"] as string == "OK")
            {
                result = response.Message["Result"] as string;
            }
        }

        message.Clear();
        message.Add("Command", "Price");
        message.Add("ID", idx);
        response = await this.inventoryService.SendMessageAsync(message);

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data that the service sent to us.
            if (response.Message["Status"] as string == "OK")
            {
                result += " : Price = " + response.Message["Result"] as string;
            }
        }

        button.Content = result;
    }
    ```

    Replace the package family name in the line `this.inventoryService.PackageFamilyName = "replace with the package family name";` with the package family name of the **AppServiceProvider** project that you obtained in \[Step 5: Deploy the service app and get the package family name\].

    The code first establishes a connection with the app service. The connection will remain open until you dispose **this.inventoryService**. The app service name must match the **AppService Name** attribute that you added to the AppServiceProvider project's Package.appxmanifest file. In this example, it is `<uap:AppService Name="com.microsoft.inventory"/>`.

    A [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) named **message** is created to specify the command that we want to send to the app service. The example app service expects a command to indicate which of two actions to take. We get the index from the textbox in the ClientApp, and then call the service with the "Item" command to get the description of the item. Then, we make the call with the "Price" command to get the item's price. The button text is set to the result.

    Because [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) only indicates whether the operating system was able to connect the call to the app service, we check the "Status" key in the [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) we receive from the app service to ensure that it was able to fulfill the request.

6.  In Visual Studio, set the ClientApp project to be the startup project in the Solution Explorer window and run the solution. Enter the number 1 into the text box and click the button. You should get "Chair : Price = 88.99" back from the service.

    ![sample app displaying chair price=88.99](images/appserviceclientapp.png)

If the app service call fails, check the following in the ClientApp:

1.  Verify that the package family name assigned to the inventory service connection matches the package family name of the AppServiceProvider app. See: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  In **button\_Click()**, verify that the app service name that is assigned to the inventory service connection matches the app service name in the AppServiceProvider's Package.appxmanifest file. See: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Ensure that the AppServiceProvider app has been deployed (In the Solution Explorer, right-click the solution and choose **Deploy**).

## Debug the app service


1.  Ensure that the entire solution is deployed before debugging because the app service provider app must be deployed before the service can be called. (In Visual Studio, **Build &gt; Deploy Solution**).
2.  In the Solution Explorer, right-click the AppServiceProvider project and choose **Properties**. From the **Debug** tab, change the **Start action** to **Do not launch, but debug my code when it starts**.
3.  In the MyAppService project, in the Class1.cs file, set a breakpoint in OnRequestReceived().
4.  Set the AppServiceProvider project to be the startup project and press F5.
5.  Start ClientApp from the Start menu (not from Visual Studio).
6.  Enter the number 1 into the text box and press the button. The debugger will stop in the app service call on the breakpoint in your app service.

## Debug the client


1.  Follow the instructions in the preceding step to debug the app service.
2.  Launch ClientApp from the Start menu.
3.  Attach the debugger to the ClientApp.exe process (not the ApplicationFrameHost.exe process). (In Visual Studio, choose **Debug &gt; Attach to Process...**.)
4.  In the ClientApp project, set a breakpoint in **button\_Click()**.
5.  The breakpoints in both the client and the app service will now be hit when you enter the number 1 into the text box of the ClientApp and click the button.

## Remarks


This example provides a simple introduction to creating an app service and calling it from another app. The key things to note are the creation of a background task to host the app service, the addition of the windows.appservice extension to the app service provider app's Package.appxmanifest file, obtaining the package family name of the app service provider app so that we can connect to it from the client app, and using [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) to call the service.

## Full code for MyAppService


```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don&#39;t want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &amp;&amp;
                 inventoryIndex.Value >= 0 &amp;&amp;
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## Temas relacionados


* [Dar soporte a una aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->



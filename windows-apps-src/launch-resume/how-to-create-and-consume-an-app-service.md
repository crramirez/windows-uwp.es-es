---
author: TylerMSFT
title: "Crear y usar un servicio de aplicación"
description: "Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo consumir esos servicios."
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keyword: app to app communication, interprocess communication, IPC, Background messaging, background communication, app to app
translationtype: Human Translation
ms.sourcegitcommit: 8b3ad18a3a0561d344b0d88a529cd929dafd9e4b
ms.openlocfilehash: c925015e9f74edcb1859ca10279beefc31286b1e

---

# Crear y consumir un servicio de aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo consumir esos servicios.

A partir de Windows 10, versión 1607, puedes crear servicios de aplicación que se ejecutan en el mismo proceso que la aplicación host. Este artículo se centra en la creación de servicios de aplicación que se ejecutan en un proceso en segundo plano independiente. Consulta [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para obtener más información acerca de los servicios de aplicación que se ejecutan en el mismo proceso que el proveedor.

## Crear un proyecto nuevo de proveedor de servicios de aplicaciones

En este procedimiento, lo crearemos todo en una solución para hacerlo más sencillo.

-   En Microsoft Visual Studio2015, crea un nuevo proyecto de aplicación para UWP y asígnale el nombre AppServiceProvider. (En el cuadro de diálogo **Nuevo proyecto**, selecciona **Plantillas &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Aplicación vacía (Windows Universal)**). Esta será la aplicación que proporciona el servicio de aplicaciones.

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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
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
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
         inventoryIndex.Value >= 0 &&
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
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
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

            // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section.
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

    Reemplaza el nombre de familia de paquete en la línea `this.inventoryService.PackageFamilyName = "replace with the package family name";` por el nombre de familia de paquete del proyecto **AppServiceProvider** que obtuviste en \[Paso 5: Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete\].

    En primer lugar, el código establece una conexión con el servicio de aplicaciones. La conexión permanecerá abierta hasta que deseches **this.inventoryService**. El nombre del servicio de aplicaciones debe coincidir con el atributo **AppService Name** que hayas agregado al archivo Package.appxmanifest del proyecto AppServiceProvider. En este ejemplo, es `<uap:AppService Name="com.microsoft.inventory"/>`.

    Se crea un [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) denominado **message** para especificar el comando que se va a enviar al servicio de aplicaciones. El servicio de aplicaciones de ejemplo espera un comando para indicar qué dos acciones realizar. Obtenemos el índice desde el cuadro de texto en ClientApp y luego llamamos al servicio con el comando "Item" para obtener la descripción del artículo. A continuación, creamos la llamada con el comando "Price" para obtener el precio del artículo. El texto del botón se define en el resultado.

    Dado que [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) solo indica si el sistema operativo pudo conectar la llamada con el servicio de aplicaciones, comprobamos la clave "Status" en el [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) que recibimos del servicio de aplicaciones para garantizar que pudo cumplir con la solicitud.

6.  En Visual Studio, establece el proyecto ClientApp como proyecto de inicio en la ventana Explorador de soluciones y ejecuta la solución. Escribe el número 1 en el cuadro de texto y haz clic en el botón. El servicio debería devolver "Chair : Price = 88.99".

    ![aplicación de muestra que muestra el precio de la silla=88.99](images/appserviceclientapp.png)

Si se produce un error en la llamada al servicio de aplicaciones, comprueba lo siguiente en el ClientApp:

1.  Comprueba que el nombre de familia de paquete asignado a la conexión de servicio de inventario coincide con el nombre de familia de paquete de la aplicación AppServiceProvider. Consulta: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  En **button\_Click()**, comprueba que el nombre del servicio de aplicaciones que se asigna a la conexión de servicio de inventario coincide con el nombre del servicio de aplicaciones en el archivo Package.appxmanifest de AppServiceProvider. Consulta: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Asegúrate de que se haya implementado la aplicación AppServiceProvider (en el Explorador de soluciones, haz clic con el botón secundario en la solución y elige **Implementar**).

## Depurar el servicio de aplicaciones


1.  Asegúrate de que se implemente la solución completa antes de la depuración porque hay que implementar la aplicación del proveedor de servicio de aplicaciones para poder llamar al servicio. (En Visual Studio, **Compilar &gt; Implementar solución**).
2.  En el Explorador de soluciones, haz clic con el botón secundario en el proyecto AppServiceProvider y elige **Propiedades**. Desde la pestaña **Depurar** cambia **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**.
3.  En el proyecto MyAppService, en el archivo Class1.cs, establece un punto de interrupción en OnRequestReceived().
4.  Establece el proyecto AppServiceProvider para que sea el proyecto de inicio y a continuación presiona F5.
5.  Inicia ClientApp desde el menú Inicio (y no desde Visual Studio).
6.  Escribe el número 1 en el cuadro de texto y presiona el botón. El depurador se detendrá en la llamada al servicio de aplicaciones en el punto de interrupción del servicio de aplicaciones.

## Depurar el cliente

1.  Sigue las instrucciones del paso anterior para depurar el servicio de aplicaciones.
2.  Inicia ClientApp desde el menú Inicio.
3.  Asocia al depurador al proceso ClientApp.exe (no al proceso ApplicationFrameHost.exe). (En Visual Studio, elige **Depurar &gt; Asociar al proceso…**)
4.  En el proyecto ClientApp, establece un punto de interrupción en **button\_Click()**.
5.  Ahora se alcanzarán los puntos de interrupción tanto en el cliente como en el servicio de aplicaciones cuando escribas el número 1 en el cuadro de texto del ClientApp y hagas clic en el botón.

## Observaciones

Este ejemplo proporciona una introducción sencilla para crear un servicio de aplicaciones y llamarlo desde otra aplicación. Los puntos clave que hay que tener en cuenta son la creación de una tarea en segundo plano para hospedar el servicio de aplicaciones, la adición de la extensión windows.appservice al archivo Package.appxmanifest de la aplicación del proveedor de servicio de aplicaciones, la obtención del nombre de familia de paquete de la aplicación del proveedor de servicio de aplicaciones para poder conectarse a este desde la aplicación cliente y el uso de [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) para llamar al servicio.

## Código completo para MyAppService

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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
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
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
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

* [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md)
* [Hacer que tu aplicación sea compatible con las tareas en segundo plano](support-your-app-with-background-tasks.md)



<!--HONumber=Nov16_HO4-->



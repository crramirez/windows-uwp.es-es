---
title: Crear y usar un servicio de aplicaciones
description: Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo usar esos servicios.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: una aplicación a la comunicación, comunicación entre procesos, IPC, mensajería, servicio de aplicación de una aplicación a, de comunicación, en segundo plano de fondo
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d122a51c53fc7eb32ab79f6decc570238af22973
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67821045"
---
# <a name="create-and-consume-an-app-service"></a>Crear y usar un servicio de aplicaciones

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son similares a servicios web, en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.  Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos.

A partir de Windows 10, versión 1607, puedes crear servicios de aplicación que se ejecutan en el mismo proceso que la aplicación host. Este artículo se centra en la creación y el consumo de un servicio de aplicaciones que se ejecuta en un proceso en segundo plano independiente. Consulta [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para obtener más información acerca de los servicios de aplicación que se ejecutan en el mismo proceso que el proveedor.

Para un ejemplo de código de servicio de aplicación, consulta [Muestras de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Crear un proyecto nuevo de proveedor de servicios de aplicaciones

En este procedimiento, lo crearemos todo en una solución para hacerlo más sencillo.

1. En Visual Studio 2015 o versiones posteriores, cree un nuevo proyecto de aplicación UWP y asígnele el nombre **AppServiceProvider**.
    1. Seleccione **archivo > Nuevo > proyecto...** 
    2. En el **crear un nuevo proyecto** cuadro de diálogo, seleccione **aplicación vacía (Windows Universal) C#** . Esta será la aplicación que dejará disponible el servicio de la aplicación para otras aplicaciones para UWP.
    3. Haga clic en **siguiente**y, a continuación, denomine al proyecto **AppServiceProvider**, elija una ubicación para él y, a continuación, haga clic en **crear**.

2. Cuando se le pide que seleccione un **destino** y **versión mínima** para el proyecto, seleccione al menos **10.0.14393**. Si desea usar el nuevo **SupportsMultipleInstances** atributo, debe usar Visual Studio 2017 o Visual Studio de 2019 y destino **10.0.15063** (**deWindows10CreatorsUpdate**) o superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Agregar una extensión de servicio de aplicación a Package.appxmanifest

En el **AppServiceProvider** proyecto, abra el **Package.appxmanifest** archivo en un editor de texto: 

1. Haga doble clic en el **el Explorador de soluciones**. 
2. Seleccione **abrir con**. 
3. Seleccione **Editor XML (texto)** . 

Agregue el siguiente `AppService` extensión dentro de la `<Application>` elemento. Este ejemplo anuncia el servicio `com.microsoft.inventory` y es lo que identifica a esta aplicación como un proveedor de servicios de aplicaciones. El servicio real se implementará como una tarea en segundo plano. El proyecto de servicio de aplicaciones expone el servicio a otras aplicaciones. Se recomienda usar un estilo inverso de nombre de dominio para el nombre del servicio.

Ten en cuenta que el prefijo del espacio de nombres `xmlns:uap4` y el atributo `uap4:SupportsMultipleInstances` solo son válidos si seleccionas la versión de Windows SDK 10.0.15063 o superior. Puedes eliminarlos con seguridad si estás seleccionando versiones anteriores de SDK.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

El `Category` atributo identifica esta aplicación como un proveedor de servicios de aplicación.

El `EntryPoint` atributo identifica la clase del espacio de nombres calificado que implementa el servicio, que se implementará a continuación.

El `SupportsMultipleInstances` atributo indica que cada vez que se llama al servicio de aplicación que debe ejecutar en un nuevo proceso. Esto no es necesario, pero está disponible para usted si necesita esa funcionalidad y están destinadas a la 10.0.15063 SDK (**Windows 10 Creators Update**) o superior. También debe ser precedido por el espacio de nombres `uap4`.

## <a name="create-the-app-service"></a>Crear el servicio de aplicaciones

1.  Un servicio de aplicaciones puede implementarse como una tarea en segundo plano. Esto permite que una aplicación en primer plano invoque un servicio de aplicaciones en otra aplicación. Para crear un servicio de aplicación como una tarea en segundo plano, agregue un nuevo proyecto de componente de Windows en tiempo de ejecución a la solución (**archivo &gt; agregar &gt; nuevo proyecto**) denominado **MyAppService**. En el **Agregar nuevo proyecto** diálogo cuadro, elija **instalado > Visual C# > componente de tiempo de ejecución de Windows (Windows Universal)** .
2.  En el **AppServiceProvider** del proyecto, agregue una referencia de proyecto a proyecto a la nueva **MyAppService** proyecto (en el **el Explorador de soluciones**, haga doble clic en el  **AppServiceProvider** proyecto > **agregar** > **referencia** > **proyectos**  >   **Solución**, seleccione **MyAppService** > **Aceptar**). Este paso es fundamental porque, si no agregas la referencia, el servicio de aplicaciones no se conectará en el momento de ejecución.
3.  En el **MyAppService** del proyecto, agregue el siguiente **mediante** instrucciones al principio del **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Cambiar el nombre de **Class1.cs** a **Inventory.cs**y reemplace el código auxiliar para **Class1** con una nueva clase de tarea en segundo plano denominado **inventario**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
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

    **Ejecute** se llama cuando se crea la tarea en segundo plano. Como las tareas en segundo plano finalizan cuando se completa la función **Run**, el código sufre un aplazamiento, para que la tarea en segundo plano se mantenga hasta que atienda las solicitudes de servicio. Un servicio de aplicación que se implementa como una tarea en segundo plano permanecerá activo durante unos 30 segundos después de recibir una llamada a menos que se llama de nuevo dentro de ese período de tiempo o se toma un aplazamiento. Si el servicio de aplicación se implementa en el mismo proceso que el llamador, la duración del servicio en la aplicación está asociada a la duración del llamador.

    La duración del servicio de aplicaciones depende el llamador:

    * Si el llamador está en primer plano, la duración del servicio de aplicación es el mismo que el llamador.
    * Si el llamador está en segundo plano, el servicio de la aplicación obtiene 30 segundos en ejecutarse. La finalización de un aplazamiento conlleva un tiempo adicional de 5 segundos.

    **OnTaskCanceled** se llama cuando se cancela la tarea. Se cancela la tarea cuando la aplicación cliente se deshace el [AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), se suspende la aplicación cliente, el sistema operativo se apaga o entra en suspensión o el sistema operativo se ejecuta fuera de los recursos para ejecutar la tarea.

## <a name="write-the-code-for-the-app-service"></a>Escribir el código para el servicio de aplicaciones

**OnRequestReceived** es donde se sitúa el código para el servicio de aplicación. Reemplace el código auxiliar **OnRequestReceived** en **MyAppService**del **Inventory.cs** con el código de este ejemplo. Este código obtiene un índice para un elemento de inventario y, junto con una cadena de comandos, lo pasa al servicio para recuperar el nombre y el precio del elemento de inventario especificado. Para tus propios proyectos, agrega un código de manejo de errores.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
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

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Tenga en cuenta que **OnRequestReceived** es **async** porque tomamos la llamada a un método que admite await [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) en este ejemplo.

Se toma un aplazamiento para que pueda usar el servicio **async** métodos en el **OnRequestReceived** controlador. Esto asegura que la llamada a **OnRequestReceived** no finalizará hasta que termine de procesar el mensaje.  [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) envía el resultado al llamador. **SendResponseAsync** no indica la finalización de la llamada. Es la finalización de aplazamiento que indique a [SendMessageAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) que **OnRequestReceived** se ha completado. La llamada a **SendResponseAsync** se encapsula en un bloque try/finally, porque debe completar la incluso si aplazamiento **SendResponseAsync** produce una excepción.

Uso de servicios de aplicación [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) objetos para intercambiar información. El tamaño de los datos que se pueden pasar solo está limitado por los recursos del sistema. No hay claves predefinidas para su uso en el elemento **ValueSet**. Debes determinar qué valores clave usarás para definir el protocolo del servicio de aplicaciones. Debes escribir el llamador teniendo ese protocolo presente. En este ejemplo, hemos elegido una clave denominada `Command` que tiene un valor que indica si queremos que el servicio de aplicaciones proporcione el nombre del elemento de inventario o su precio. El índice del nombre de inventario se almacena en la clave `ID`. El valor devuelto se almacena en la clave `Result`.

Un [AppServiceClosedStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) enum se devuelve al autor de llamada para indicar si la llamada al servicio de la aplicación se ha realizado correctamente o no. Un ejemplo de cómo podría producirse un error en la llamada al servicio de aplicaciones es cuando el sistema operativo anula el extremo de servicio porque se han excedido sus recursos. Puede devolver información de error adicional a través de la [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet). En este ejemplo, usamos una clave denominada `Status` para devolver información más detallada del error al llamador.

La llamada a [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) devuelve el [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) al llamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete

El proveedor de servicios de aplicación debe implementarse antes de invocarlo desde un cliente. Puede implementar mediante la selección **compilar > Implementar solución** en Visual Studio.

También necesitará el nombre de familia de paquete del proveedor de servicios de aplicación para llamarlo. Puede obtener abriendo el **AppServiceProvider** del proyecto **Package.appxmanifest** archivo en la vista del diseñador (haga doble clic en el **el Explorador de soluciones**). Seleccione el **empaquetado** pestaña, copie el valor junto a **nombre de familia de paquete**y péguelo en algún lugar como el Bloc de notas por ahora.

## <a name="write-a-client-to-call-the-app-service"></a>Escribir un cliente para llamar al servicio de aplicaciones

1.  Agrega un nuevo proyecto vacío de aplicación universal de Windows a la solución con **Archivo &gt; Agregar &gt; Nuevo proyecto**. En el **Agregar nuevo proyecto** diálogo cuadro, elija **instalado > Visual C# > aplicación vacía (Windows Universal)** y asígnele el nombre **ClientApp**.

2.  En el **ClientApp** del proyecto, agregue el siguiente **mediante** instrucción al principio del **MainPage.xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Agregar un cuadro de texto denominado **textBox** y un botón para **MainPage.xaml**.

4.  Agregue un botón haga clic en el controlador para el botón llamado **button_Click**y agregue la palabra clave **async** para la firma del controlador de botón.

5. Reemplaza el código auxiliar del controlador de clic del botón por el siguiente código. Asegúrate de incluir la declaración de campo `inventoryService`.
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
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
           // Get the data  that the service sent to us.
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

       textBox.Text = result;
   }
   ```
    
    Reemplaza el nombre de familia de paquete en la línea `this.inventoryService.PackageFamilyName = "Replace with the package family name";` por el nombre de familia de paquete del proyecto **AppServiceProvider** que obtuviste anteriormente en [Implementar la aplicación de servicio y obtener el nombre de familia de paquete](#deploy-the-service-app-and-get-the-package-family-name).

    > [!NOTE]
    > Asegúrese de pegar en el literal de cadena, en lugar de ponerlo en una variable. No funcionará si usa una variable.

    En primer lugar, el código establece una conexión con el servicio de aplicaciones. La conexión permanecerá abierta hasta que deseches `this.inventoryService`. Debe coincidir con el nombre del servicio de aplicación el `AppService` del elemento `Name` atributo que se ha agregado a la **AppServiceProvider** del proyecto **Package.appxmanifest** archivo. En este ejemplo, es `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Un [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) denominado `message` se crea para especificar el comando que se va a enviar al servicio de aplicación. El servicio de aplicaciones de ejemplo espera un comando para indicar qué dos acciones realizar. Se obtiene el índice en el cuadro de texto en la aplicación cliente y, a continuación, llamar al servicio con el `Item` comando para obtener la descripción del elemento. A continuación, hacemos la llamada con el comando `Price` para obtener el precio del artículo. El texto del botón se define en el resultado.

    Dado que [AppServiceResponseStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) sólo indica si el sistema operativo fue capaz de conectarse a la llamada al servicio de aplicación, comprobamos el `Status` clave en el [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) que recibimos de la aplicación servicio para asegurarse de que era capaz de satisfacer la solicitud.

6. Establecer el **ClientApp** proyecto para que sea el proyecto de inicio (haga clic en él en el **el Explorador de soluciones** > **establecer como proyecto de inicio**) y ejecutar la solución. Escribe el número 1 en el cuadro de texto y haz clic en el botón. Debería obtener "silla: Precio = 88.99" del servicio.

    ![aplicación de muestra que muestra el precio de la silla=88.99](images/appserviceclientapp.png)

Si se produce un error en la llamada al servicio de aplicación, compruebe lo siguiente el **ClientApp** proyecto:

1.  Compruebe que el nombre de familia de paquete asignado a la conexión de servicio de inventario coincide con el nombre de familia de paquete de la **AppServiceProvider** app. Vea la línea de **botón\_haga clic en** con `this.inventoryService.PackageFamilyName = "...";`.
2.  En **botón\_haga clic en**, compruebe que el nombre del servicio de aplicación que se asigna a la conexión de servicio de inventario coincide con el nombre del servicio de aplicación en el **AppServiceProvider**del  **Package.appxmanifest** archivo. Consulta: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Asegúrese de que el **AppServiceProvider** se ha implementado la aplicación. (En el **el Explorador de soluciones**, haga clic en la solución y elija **implementar solución**).

## <a name="debug-the-app-service"></a>Depurar el servicio de aplicaciones

1.  Asegúrate de que se implemente la solución antes de la depuración, porque hay que implementar la aplicación del proveedor de servicio de aplicaciones para poder llamar al servicio. (En Visual Studio, **Compilar &gt; Implementar solución**).
2.  En el **el Explorador de soluciones**, haga clic en el **AppServiceProvider** proyecto y elija **propiedades**. Desde la pestaña **Depurar** cambia **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. (Ten en cuenta que si estuvieras usando C++ para implementar el proveedor de servicios de aplicaciones, desde la pestaña **Depuración** deberías cambiar **Iniciar aplicación** a **No**).
3.  En el **MyAppService** del proyecto, en el **Inventory.cs** de archivos, establezca un punto de interrupción **OnRequestReceived**.
4.  Establecer el **AppServiceProvider** proyecto para que sea el proyecto de inicio y presione **F5**.
5.  Iniciar **ClientApp** desde el menú Inicio (y no desde Visual Studio).
6.  Escribe el número 1 en el cuadro de texto y presiona el botón. El depurador se detendrá en la llamada al servicio de aplicaciones en el punto de interrupción del servicio de aplicaciones.

## <a name="debug-the-client"></a>Depurar el cliente

1.  Sigue las instrucciones del paso anterior para depurar el cliente que llama al servicio de aplicaciones.
2.  Iniciar **ClientApp** desde el menú Inicio.
3.  Asociar el depurador a la **ClientApp.exe** proceso (no el **ApplicationFrameHost.exe** proceso). (En Visual Studio, elige **Depurar &gt; Asociar al proceso…** )
4.  En el **ClientApp** del proyecto, establezca un punto de interrupción **botón\_haga clic en**.
5.  Ahora se alcanzarán los puntos de interrupción en el cliente y el servicio de aplicación al escribir el número 1 en el cuadro de texto de **ClientApp** y haga clic en el botón.

## <a name="general-app-service-troubleshooting"></a>Solución de errores generales de servicio de aplicaciones

Si se produce un **AppUnavailable** estado después de intentar conectarse a un servicio de aplicación, compruebe lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente ya que, de lo contrario, el cliente no tendrá nada a lo que conectarse. Puedes implementar desde Visual Studio usando **Compilar** > **solución de implementación**.
- En el **el Explorador de soluciones**, asegúrese de que el proyecto de proveedor de servicio de aplicación tiene una referencia de proyecto a proyecto al proyecto que implementa el servicio de aplicación.
- Compruebe que la `<Extensions>` entrada y sus elementos secundarios, se han agregado a la **Package.appxmanifest** que pertenecen al proyecto de proveedor de servicios de aplicación según lo especificado anteriormente en el archivo [agregar una extensión de servicio de aplicación Package.appxmanifest](#appxmanifest).
- Asegúrese de que el [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) cadena en el cliente que llama el proveedor de servicios de aplicación coincide con el `<uap3:AppService Name="..." />` especificado en el proyecto proveedor de aplicación y servicio **Package.appxmanifest**  archivo.
- Asegúrese de que el [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) coincide con el nombre de familia de paquete del componente del proveedor de servicio de aplicación según lo especificado anteriormente en [agregar una extensión de servicio de aplicación a Package.appxmanifest](#appxmanifest)
- Para los servicios de aplicación fuera de proceso como uno en este ejemplo, validar que el `EntryPoint` especificado en el `<uap:Extension ...>` elemento de su proyecto proveedor de aplicación y servicio **Package.appxmanifest** archivo coincide con el espacio de nombres y nombre de clase de la clase pública que implemente [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) en su proyecto de aplicación de servicio.

### <a name="troubleshoot-debugging"></a>Solucionar problemas de depuración

Si el depurador no se detiene en los puntos de interrupción de los proyectos del proveedor de servicios de aplicaciones o de servicio de aplicaciones, comprueba lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente. Puedes implementarlos desde Visual Studio usando **Compilar** > **Solución de implementación**.
- Asegúrese de que el proyecto que va a depurar está establecido como proyecto de inicio y que se establecen las propiedades de depuración para ese proyecto para no ejecutar el proyecto cuando **F5** está presionado. Haz clic con el botón derecho en el proyecto y, a continuación, haz clic en **Propiedades** y después en **Depurar** (o **Depuración** en C++). En C#, cambia la **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. En C++, establece **Iniciar aplicación** en **No**.

## <a name="remarks"></a>Comentarios

Este ejemplo proporciona una introducción para crear un servicio de aplicaciones que se ejecuta como tarea en segundo plano y llamarlo desde otra aplicación. Tenga en cuenta los aspectos claves son:

* Crear una tarea en segundo plano para hospedar el servicio de aplicación.
* Agregar el `windows.appService` extensión para el proveedor de servicios de aplicación **Package.appxmanifest** archivo.
* Obtenga el nombre de familia de paquete del proveedor de servicios de aplicación para que podemos conectarnos a él desde la aplicación cliente.
* Agregue una referencia de proyecto a proyecto desde el proyecto de proveedor de servicio de aplicación para el proyecto de aplicación de servicio.
* Use [Windows.ApplicationModel.AppService.AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) para llamar al servicio.

## <a name="full-code-for-myappservice"></a>Código completo para MyAppService

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
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
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

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
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

## <a name="related-topics"></a>Temas relacionados

* [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md)
* [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md)
* [Ejemplo de código de servicio de aplicación (C#, C++ y VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)

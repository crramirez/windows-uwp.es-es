---
title: Crear y usar un servicio de aplicación
description: Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo usar esos servicios.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: comunicación de la aplicación a otra, comunicación entre procesos, IPC, mensajería, servicio de aplicaciones en segundo plano comunicación, aplicaciones,
ms.date: 1/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9029c8ee3a930e66ebdbd0c4d0681d87486a8393
ms.sourcegitcommit: 6e2027f8ebc1d891d27ea6b2e4676d592871bcc7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "9011265"
---
# <a name="create-and-consume-an-app-service"></a>Crear y usar un servicio de aplicaciones

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son similares a servicios web, en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.  Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos.

A partir de Windows 10, versión 1607, puedes crear servicios de aplicación que se ejecutan en el mismo proceso que la aplicación host. Este artículo se centra en la creación y el consumo de un servicio de aplicaciones que se ejecuta en un proceso en segundo plano independiente. Consulta [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para obtener más información acerca de los servicios de aplicación que se ejecutan en el mismo proceso que el proveedor.

Para un ejemplo de código de servicio de aplicación, consulta [Muestras de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Crear un proyecto nuevo de proveedor de App Service

En este procedimiento, lo crearemos todo en una solución para hacerlo más sencillo.

1. En Visual Studio 2015 o posterior, crea un nuevo proyecto de aplicación para UWP y asígnale el nombre **AppServiceProvider**.
    1. Selecciona el **archivo > > proyecto nuevo …** 
    2. En el cuadro de diálogo **Nuevo proyecto** , selecciona **instalado > Visual C# > aplicación vacía (Universal Windows)**. Esta será la aplicación que dejará disponible el servicio de la aplicación para otras aplicaciones para UWP.
    3. Nombra el proyecto **AppServiceProvider**, elige una ubicación para ella y haz clic en **Aceptar**.

2. Cuando se pida seleccionar un **destino** y la **versión mínima** del proyecto, selecciona al menos **10.0.14393**. Si quieres usar el nuevo atributo **SupportsMultipleInstances** , debes estar usando Visual Studio 2017 y seleccionar **10.0.15063** (**Windows 10 Creators Update**) o superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Agregar una extensión de servicio de aplicaciones a Package.appxmanifest

En el proyecto **AppServiceProvider** , abre el archivo **Package.appxmanifest** en un editor de texto: 

1. Haz clic en él en el **Explorador de soluciones**. 
2. Seleccione **Abrir con**. 
3. Selecciona el **Editor XML (texto)**. 

Agrega el siguiente `AppService` extensión dentro de la `<Application>` elemento. Este ejemplo anuncia el servicio `com.microsoft.inventory` y es lo que identifica a esta aplicación como proveedor de servicios de aplicaciones. El servicio real se implementará como una tarea en segundo plano. El proyecto de servicio de aplicaciones expone el servicio a otras aplicaciones. Se recomienda usar un estilo inverso de nombre de dominio para el nombre del servicio.

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

El `Category` atributo identifica esta aplicación como un proveedor de servicio de aplicaciones.

El `EntryPoint` atributo identifica la clase de espacio de nombres completado que implementa el servicio, que se implementará a continuación.

El `SupportsMultipleInstances` atributo indica que cada vez que se llama al servicio de aplicaciones se debe ejecutar en un nuevo proceso. Esto no es necesario, pero está disponible si necesitas esa funcionalidad y seleccionas el 10.0.15063 SDK (**Windows 10 Creators Update**) o superior. También debe ser precedido por el espacio de nombres `uap4`.

## <a name="create-the-app-service"></a>Crear el servicio de aplicaciones

1.  Un servicio de aplicaciones puede implementarse como una tarea en segundo plano. Esto permite que una aplicación en primer plano invoque un servicio de aplicaciones en otra aplicación. Para crear un servicio de aplicaciones como una tarea en segundo plano, agrega un nuevo proyecto de componente de Windows Runtime a la solución (**archivo &gt; agregar &gt; nuevo proyecto**) denominado **MyAppService**. En el cuadro de diálogo **Agregar nuevo proyecto** , elige **instalado > Visual C# > componente de Windows Runtime (Windows Universal)**.
2.  En el proyecto **AppServiceProvider** , agrega una referencia de proyecto a proyecto al nuevo proyecto **MyAppService** (en el **Explorador de soluciones**, haz clic en el > de proyecto **AppServiceProvider** **Agregar**  >  ** Hacer referencia a** > **proyectos** > **soluciones**, selecciona **MyAppService** > **Aceptar**). Este paso es fundamental porque, si no agregas la referencia, el servicio de aplicaciones no se conectará en el momento de ejecución.
3.  En el proyecto **MyAppService** , agrega las siguientes instrucciones **using** a la parte superior de **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Cambia el nombre **Class1.cs** **Inventory.cs**y reemplaza el código auxiliar para **Class1** con una nueva clase de tarea en segundo plano denominada **inventario**:

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

    **Ejecutar** , se llama cuando se crea la tarea en segundo plano. Como las tareas en segundo plano finalizan cuando se completa la función **Run**, el código sufre un aplazamiento, para que la tarea en segundo plano se mantenga hasta que atienda las solicitudes de servicio. Un servicio de aplicaciones que se implementa como tarea en segundo plano se mantendrá activo durante aproximadamente 30 segundos después de recibir una llamada, a menos que se le llame de nuevo dentro de dicha ventana del tiempo o se produzca un aplazamiento. Si el servicio de aplicaciones se implementa en el mismo proceso que el llamador, la duración del servicio de aplicaciones queda vinculada a la duración del llamador.

    La duración del servicio de aplicaciones depende el llamador:

    * Si el llamador está en primer plano, la duración del servicio de aplicaciones es el mismo que el llamador.
    * Si el llamador está en segundo plano, el servicio de aplicaciones tarda 30 segundos en ejecutarse. La finalización de un aplazamiento conlleva un tiempo adicional de 5 segundos.

    **OnTaskCanceled** se llama cuando se cancela la tarea. Cuando la aplicación cliente desecha el [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704), se suspende la aplicación de cliente, el sistema operativo se apaga o se suspende o el sistema operativo se queda sin recursos para ejecutar la tarea, se cancela la tarea.

## <a name="write-the-code-for-the-app-service"></a>Escribir el código para el servicio de aplicaciones

**OnRequestReceived** es dónde va el código para el servicio de aplicaciones. Reemplaza el código auxiliar **OnRequestReceived** en de **MyAppService** **Inventory.cs** con el código de este ejemplo. Este código obtiene un índice para un elemento de inventario y, junto con una cadena de comandos, lo pasa al servicio para recuperar el nombre y el precio del elemento de inventario especificado. Para tus propios proyectos, agrega un código de manejo de errores.

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

Ten en cuenta que **OnRequestReceived** es **asincrónico** , ya que se realiza un método llamado a [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) en este ejemplo.

Se toma un aplazamiento para que el servicio pueda usar métodos **asincrónicos** en el controlador **OnRequestReceived** . Esto asegura que la llamada a **OnRequestReceived** no finalizará hasta que termine de procesar el mensaje.  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) envía el resultado a la persona que llama. **SendResponseAsync** no indica la finalización de la llamada. Es la finalización del aplazamiento que indica a [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712) que **OnRequestReceived** se ha completado. La llamada a **SendResponseAsync** se ajusta en un bloque try/finally porque debes completar el aplazamiento incluso si **SendResponseAsync** inicia una excepción.

Servicios de aplicaciones usan objetos [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) para intercambiar información. El tamaño de los datos que se pueden pasar solo está limitado por los recursos del sistema. No hay claves predefinidas para su uso en el elemento **ValueSet**. Debes determinar qué valores clave usarás para definir el protocolo del servicio de aplicaciones. Debes escribirse el llamador teniendo ese protocolo presente. En este ejemplo, hemos elegido una clave denominada `Command` que tiene un valor que indica si queremos que el servicio de aplicaciones proporcione el nombre del elemento de inventario o su precio. El índice del nombre de inventario se almacena en la clave `ID`. El valor devuelto se almacena en la clave `Result`.

Se devuelve al llamador una enumeración [AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) para indicar si la llamada al servicio de aplicaciones se ha realizado correctamente o no. Un ejemplo de cómo podría producirse un error en la llamada al servicio de aplicaciones es cuando el sistema operativo anula el extremo de servicio porque se han excedido sus recursos. Puedes devolver información adicional sobre errores a través de [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131). En este ejemplo, usamos una clave denominada `Status` para devolver información más detallada del error al llamador.

La llamada a [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) devuelve el elemento [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) al llamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete

El proveedor de servicios de la aplicación debe implementarse antes de llamarla desde un cliente. También tendrás que el nombre de familia de paquete de servicio de aplicaciones para poder llamarlo.

Una forma de obtener el nombre de familia de paquete de la aplicación de servicio de aplicación es llamar a [Windows.ApplicationModel.Package.Current.Id.FamilyName](https://msdn.microsoft.com/library/windows/apps/br224670) desde dentro del proyecto **AppServiceProvider** (por ejemplo, desde el constructor de la **aplicación** en ** App.Xaml.cs**) y ten en cuenta el resultado. Para ejecutar **AppServiceProvider** en Visual Studio, establecer como proyecto de inicio en la ventana **Explorador de soluciones** y ejecuta el proyecto.

Otra forma de obtener el nombre de familia de paquete consiste en implementar la solución (**compilación &gt; implementar solución**) y anotar el nombre completo del paquete en la ventana de **salida** (**vista &gt; salida**). Debes quitar la información de la plataforma de la cadena en la ventana de **salida** para derivar el nombre del paquete. Por ejemplo, si el nombre completo del paquete indicado en la ventana de **salida** son:

`Microsoft.SDKSamples.AppServicesProvider.CPP_1.0.0.0_x86__8wekyb3d8bbwe`

A continuación, deberías extraer `1.0.0.0\_x86\_\_`, dejando los siguientes como el nombre de familia de paquete:

`Microsoft.SDKSamples.AppServicesProvider.CPP_8wekyb3d8bbwe`

## <a name="write-a-client-to-call-the-app-service"></a>Escribir a un cliente para llamar al servicio de aplicaciones

1.  Agrega un nuevo proyecto vacío de aplicación universal de Windows a la solución con **Archivo &gt; Agregar &gt; Nuevo proyecto**. En el cuadro de diálogo **Agregar nuevo proyecto** , elige **instalado > Visual C# > aplicación vacía (Universal Windows)** y asígnale **ClientApp**.

2.  En el proyecto **ClientApp** , agrega la siguiente instrucción **using** a la parte superior de **MainPage.xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Agregar un cuadro de texto denominado **textBox** y un botón a **MainPage.xaml**.

4.  Agrega un botón controlador de clic para el botón llamado **button_Click**y agrega la palabra clave **async** a firma del controlador de botón.

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
    > Asegúrate de pegar en el literal de cadena, en lugar de colocarlo en una variable. No lo hará si usas una variable.

    En primer lugar, el código establece una conexión con el servicio de aplicaciones. La conexión permanecerá abierta hasta que deseches `this.inventoryService`. El nombre del servicio de aplicación debe coincidir con el `AppService` del elemento `Name` atributo que hayas agregado al archivo **Package.appxmanifest** del proyecto **AppServiceProvider** . En este ejemplo, es `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Un [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) denominado `message` se crea para especificar el comando que se va a enviar al servicio de aplicaciones. El servicio de aplicaciones de ejemplo espera un comando para indicar cuál de dos acciones realizar. Obtenemos el índice desde el cuadro de texto de la aplicación cliente y, a continuación, llamamos al servicio con el `Item` comando para obtener la descripción del artículo. A continuación, hacemos la llamada con el comando `Price` para obtener el precio del artículo. El texto del botón se adecúa al resultado.

    Dado que [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724) solo indica si el sistema operativo pudo conectar la llamada con el servicio de aplicaciones, comprobamos la clave `Status` en el [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) que recibimos del servicio de aplicaciones para garantizar que pudo cumplir con la solicitud.

6. Establece el proyecto **ClientApp** como proyecto de inicio (con el botón derecho en el **Explorador de soluciones** > **establecer como proyecto de inicio**) y ejecutar la solución. Escribe el número 1 en el cuadro de texto y haz clic en el botón. El servicio debería devolver "Chair : Price = 88.99".

    ![aplicación de muestra que muestra el precio de la silla=88.99](images/appserviceclientapp.png)

Si se produce un error en la llamada al servicio de aplicaciones, comprueba lo siguiente en el proyecto **ClientApp** :

1.  Comprueba que el nombre de familia de paquete asignado a la conexión de servicio de inventario coincide con el nombre de familia de paquete de la aplicación **AppServiceProvider** . Consulta la línea en **button\_Click** con `this.inventoryService.PackageFamilyName = "...";`.
2.  En **button\_Click**, comprueba que el nombre de servicio de aplicaciones que se asigna a la conexión de servicio de inventario coincide con el nombre de archivo **Package.appxmanifest** de **AppServiceProvider**de servicio de aplicación. Consulta: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Asegúrate de que se ha implementado la aplicación **AppServiceProvider** . (En el **Explorador de soluciones**, haz clic en la solución y elige **Implementar solución**).

## <a name="debug-the-app-service"></a>Depurar el servicio de aplicaciones

1.  Asegúrate de que se implemente la solución antes de la depuración, porque hay que implementar la aplicación del proveedor de servicio de aplicaciones para poder llamar al servicio. (En Visual Studio, **Compilar &gt; Implementar solución**).
2.  En el **Explorador de soluciones**, haz clic en el proyecto **AppServiceProvider** y elige **las propiedades**. Desde la pestaña **Depurar** cambia **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. (Ten en cuenta que si estuvieras usando C++ para implementar el proveedor de servicios de aplicaciones, desde la pestaña **Depuración** deberías cambiar **Iniciar aplicación** a **No**).
3.  En el proyecto **MyAppService** , en el archivo **Inventory.cs** , establece un punto de interrupción en **OnRequestReceived**.
4.  Establece el proyecto **AppServiceProvider** para el proyecto de inicio y presiona **F5**.
5.  Inicia **ClientApp** desde el menú Inicio (y no desde Visual Studio).
6.  Escribe el número 1 en el cuadro de texto y presiona el botón. El depurador se detendrá en la llamada al servicio de aplicaciones en el punto de interrupción del servicio de aplicaciones.

## <a name="debug-the-client"></a>Depurar el cliente

1.  Sigue las instrucciones del paso anterior para depurar el cliente que llama al servicio de aplicaciones.
2.  Inicia **ClientApp** desde el menú Inicio.
3.  Asociar al depurador al proceso **ClientApp.exe** (no al proceso **ApplicationFrameHost.exe** ). (En Visual Studio, elige **Depurar &gt; Asociar al proceso…**)
4.  En el proyecto **ClientApp** , establece un punto de interrupción en **button\_Click**.
5.  Ahora se alcanzarán los puntos de interrupción en el cliente y el servicio de aplicaciones cuando se escribe el número 1 en el cuadro de texto del **ClientApp** y haz clic en el botón.

## <a name="general-app-service-troubleshooting"></a>Solución de errores generales de servicio de aplicaciones

Si encuentras un estado **AppUnavailable** después de intentar conectarse a un servicio de aplicaciones, comprueba lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente ya que, de lo contrario, el cliente no tendrá nada a lo que conectarse. Puedes implementar desde Visual Studio usando **Compilar** > **solución de implementación**.
- En el **Explorador de soluciones**, asegúrate de que el proyecto de proveedor de servicio de aplicación tiene una referencia de proyecto a proyecto al proyecto que implementa el servicio de aplicaciones.
- Comprueba que la `<Extensions>` entrada y sus elementos secundarios, se agregaron al archivo **Package.appxmanifest** que pertenece al proyecto de proveedor de servicio de aplicaciones especificado anteriormente en [Agregar una extensión de servicio de aplicaciones a Package.appxmanifest](#appxmanifest).
- Asegúrate de que coincida con la cadena de [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) del cliente que llama el proveedor de servicio de aplicaciones del `<uap3:AppService Name="..." />` especificado en el archivo **Package.appxmanifest** de la aplicación servicio del proyecto del proveedor.
- Asegúrate de que el [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) coincide con el nombre de familia de paquete del componente del proveedor de servicio de aplicaciones especificado anteriormente en [Agregar una extensión de servicio de aplicaciones a Package.appxmanifest](#appxmanifest)
- Para los servicios de la aplicación fuera de proceso, como en este ejemplo, validar que el `EntryPoint` especificado en el `<uap:Extension ...>` elemento del archivo **Package.appxmanifest** de tu aplicación servicio del proyecto del proveedor que coincida con el espacio de nombres y nombre de clase del público de clase que implementa [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) en el proyecto de servicio de aplicación.

### <a name="troubleshoot-debugging"></a>Solucionar problemas de depuración

Si el depurador no se detiene en los puntos de interrupción de los proyectos del proveedor de servicios de aplicaciones o de servicio de aplicaciones, comprueba lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente. Puedes implementarlos desde Visual Studio usando **Compilar** > **Solución de implementación**.
- Asegúrate de que el proyecto que desee depurar está establecido como proyecto de inicio y que se establecen las propiedades de depuración para ese proyecto no se ejecute el proyecto cuando se presiona **F5** . Haz clic con el botón derecho en el proyecto y, a continuación, haz clic en **Propiedades** y después en **Depurar** (o **Depuración** en C++). En C#, cambia la **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. En C++, establece **Iniciar aplicación** en **No**.

## <a name="remarks"></a>Comentarios

Este ejemplo proporciona una introducción para crear un servicio de aplicaciones que se ejecuta como tarea en segundo plano y llamarlo desde otra aplicación. Ten en cuenta las cosas que debes claves son:

* Crear una tarea en segundo plano para hospedar el servicio de aplicaciones.
* Agregar el `windows.appService` extensión al archivo **Package.appxmanifest** del proveedor de servicios de aplicación.
* Obtener el nombre de familia de paquete del proveedor de servicios de aplicación para que nos podemos conectar a él desde la aplicación cliente.
* Agregar una referencia de proyecto a proyecto desde el proyecto de proveedor de servicio de aplicaciones para el proyecto de servicio de aplicaciones.
* Usar [Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704) para llamar al servicio.

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
* [Compatibilidad de la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md)
* [Ejemplo de código de servicio de aplicación (C#, C++ y VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)

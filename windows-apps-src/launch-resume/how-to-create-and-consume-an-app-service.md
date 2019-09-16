---
title: Crear y usar un servicio de aplicaciones
description: Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo usar esos servicios.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: comunicación entre aplicaciones, comunicación entre procesos, IPC, mensajería en segundo plano, comunicación en segundo plano, aplicación en aplicación, App Service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d6edc49bc97a336b8d722c496c1980a5f9b0efb
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393556"
---
# <a name="create-and-consume-an-app-service"></a>Crear y usar un servicio de aplicaciones

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son similares a servicios web, en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.  Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos.

A partir de Windows 10, versión 1607, puedes crear servicios de aplicación que se ejecutan en el mismo proceso que la aplicación host. Este artículo se centra en la creación y el consumo de un servicio de aplicaciones que se ejecuta en un proceso en segundo plano independiente. Consulta [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para obtener más información acerca de los servicios de aplicación que se ejecutan en el mismo proceso que el proveedor.

Para un ejemplo de código de servicio de aplicación, consulta [Muestras de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Crear un proyecto nuevo de proveedor de servicios de aplicaciones

En este procedimiento, lo crearemos todo en una solución para hacerlo más sencillo.

1. En Visual Studio 2015 o posterior, cree un nuevo proyecto de aplicación para UWP y asígnele el nombre **AppServiceProvider**.
    1. Seleccione el **archivo > nuevo proyecto de >...** 
    2. En el cuadro de diálogo **crear un nuevo proyecto** , seleccione **aplicación vacía (Windows universal C#)** . Esta será la aplicación que dejará disponible el servicio de la aplicación para otras aplicaciones para UWP.
    3. Haga clic en **siguiente**y, a continuación, asigne al proyecto el nombre **AppServiceProvider**, elija una ubicación para él y, a continuación, haga clic en **crear**.

2. Cuando se le pida que seleccione un **destino** y una **versión mínima** para el proyecto, seleccione al menos **10.0.14393**. Si desea usar el nuevo atributo **SupportsMultipleInstances** , debe usar visual Studio 2017 o visual Studio 2019 y el destino **10.0.15063** (**Windows 10 Creators Update**) o superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Agregar una extensión de App Service a package. appxmanifest

En el proyecto **AppServiceProvider** , abra el archivo **Package. appxmanifest** en un editor de texto: 

1. Haga clic con el botón secundario en el **Explorador de soluciones**. 
2. Seleccione **abrir con**. 
3. Seleccione **Editor XML (texto)** . 

Agregue la siguiente `AppService` extensión dentro del `<Application>` elemento. Este ejemplo anuncia el servicio `com.microsoft.inventory` y es lo que identifica a esta aplicación como un proveedor de servicios de aplicaciones. El servicio real se implementará como una tarea en segundo plano. El proyecto de servicio de aplicaciones expone el servicio a otras aplicaciones. Se recomienda usar un estilo inverso de nombre de dominio para el nombre del servicio.

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

El `Category` atributo identifica esta aplicación como un proveedor de App Service.

El `EntryPoint` atributo identifica la clase de espacio de nombres calificado que implementa el servicio, que se implementará a continuación.

El `SupportsMultipleInstances` atributo indica que, cada vez que se llama a App Service, debe ejecutarse en un nuevo proceso. Esto no es necesario, pero está disponible si necesita esa funcionalidad y tiene como destino el SDK de 10.0.15063 (**Windows 10 Creators Update**) o superior. También debe ser precedido por el espacio de nombres `uap4`.

## <a name="create-the-app-service"></a>Crear el servicio de aplicaciones

1.  Un servicio de aplicaciones puede implementarse como una tarea en segundo plano. Esto permite que una aplicación en primer plano invoque un servicio de aplicaciones en otra aplicación. Para crear una aplicación de App Service como una tarea en segundo plano, agregue un nuevo Windows Runtime proyecto de componente a la solución (**archivo &gt; agregar &gt; nuevo proyecto**) llamado **MyAppService**. En el cuadro de diálogo **Agregar nuevo proyecto** , elija **instalado > C# componente de Visual > Windows Runtime (Windows universal)** .
2.  En el proyecto **appserviceprovider** , agregue una referencia de proyecto a proyecto al nuevo proyecto **MyAppService** (en el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **AppServiceProvider** > **Agregar**  >  > **Solución** **proyectos**dereferencia > , seleccione **MyAppService** **Aceptar**). >  Este paso es fundamental porque, si no agregas la referencia, el servicio de aplicaciones no se conectará en el momento de ejecución.
3.  En el proyecto **MyAppService** , agregue las siguientes instrucciones **using** a la parte superior de **Class1.CS**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Cambie el nombre de **Class1.CS** a **Inventory.CS**y reemplace el código auxiliar de **Class1** por una nueva clase de tarea en segundo plano denominada **Inventory**:

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

    Se llama a **Run** cuando se crea la tarea en segundo plano. Como las tareas en segundo plano finalizan cuando se completa la función **Run**, el código sufre un aplazamiento, para que la tarea en segundo plano se mantenga hasta que atienda las solicitudes de servicio. Un servicio de aplicaciones que se implementa como una tarea en segundo plano permanecerá activo durante unos 30 segundos después de recibir una llamada a menos que se vuelva a llamar en ese período de tiempo o se extraiga un aplazamiento. Si app Service se implementa en el mismo proceso que el autor de la llamada, la duración de App Service está ligada a la duración del autor de la llamada.

    La duración del servicio de aplicaciones depende el llamador:

    * Si el autor de la llamada está en primer plano, la duración de App Service es la misma que la del autor de la llamada.
    * Si el autor de la llamada está en segundo plano, el servicio de aplicación obtiene 30 segundos para ejecutarse. La finalización de un aplazamiento conlleva un tiempo adicional de 5 segundos.

    Se llama a **OnTaskCanceled** cuando se cancela la tarea. La tarea se cancela cuando la aplicación cliente desecha el [AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), se suspende la aplicación cliente, el sistema operativo se apaga o se suspende, o el sistema operativo se queda sin recursos para ejecutar la tarea.

## <a name="write-the-code-for-the-app-service"></a>Escribir el código para el servicio de aplicaciones

**OnRequestReceived** es el lugar donde se dirige el código para App Service. Reemplace el código auxiliar **OnRequestReceived** en **Inventory.CS** de **MyAppService**por el código de este ejemplo. Este código obtiene un índice para un elemento de inventario y, junto con una cadena de comandos, lo pasa al servicio para recuperar el nombre y el precio del elemento de inventario especificado. Para tus propios proyectos, agrega un código de manejo de errores.

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

Tenga en cuenta que **OnRequestReceived** es **Async** porque se realiza una llamada de método que se pueda esperar a [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) en este ejemplo.

Se toma un aplazamiento para que el servicio pueda usar métodos **asincrónicos** en el controlador **OnRequestReceived** . Esto asegura que la llamada a **OnRequestReceived** no finalizará hasta que termine de procesar el mensaje.  [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) envía el resultado al llamador. **SendResponseAsync** no indica la finalización de la llamada. Se trata de la finalización del aplazamiento que señala a [SendMessageAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) que **OnRequestReceived** se ha completado. La llamada a **SendResponseAsync** se ajusta en un bloque try/finally porque debe completar el aplazamiento incluso si **SendResponseAsync** produce una excepción.

Los servicios de aplicaciones usan objetos [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) para intercambiar información. El tamaño de los datos que se pueden pasar solo está limitado por los recursos del sistema. No hay claves predefinidas para su uso en el elemento **ValueSet**. Debes determinar qué valores clave usarás para definir el protocolo del servicio de aplicaciones. Debes escribir el llamador teniendo ese protocolo presente. En este ejemplo, hemos elegido una clave denominada `Command` que tiene un valor que indica si queremos que el servicio de aplicaciones proporcione el nombre del elemento de inventario o su precio. El índice del nombre de inventario se almacena en la clave `ID`. El valor devuelto se almacena en la clave `Result`.

Se devuelve una enumeración [AppServiceClosedStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) al autor de la llamada para indicar si la llamada a App Service se realizó correctamente o no. Un ejemplo de cómo podría producirse un error en la llamada al servicio de aplicaciones es cuando el sistema operativo anula el extremo de servicio porque se han excedido sus recursos. Puede devolver información de error adicional a través de la [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet). En este ejemplo, usamos una clave denominada `Status` para devolver información más detallada del error al llamador.

La llamada a [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) devuelve el [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) al llamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete

El proveedor de App Service debe implementarse para poder llamarlo desde un cliente. Puede implementarlo seleccionando **Compilar > implementar solución** en Visual Studio.

También necesitará el nombre de familia del paquete del proveedor de App Service para llamarlo. Para obtenerlo, abra el archivo **Package. appxmanifest** del proyecto **AppServiceProvider** en la vista del diseñador (haga doble clic en él en el **Explorador de soluciones**). Seleccione la pestaña **empaquetado** , copie el valor junto a **nombre de familia de paquete**y péguelo en un lugar como el Bloc de notas por ahora.

## <a name="write-a-client-to-call-the-app-service"></a>Escribir un cliente para llamar al servicio de aplicaciones

1.  Agrega un nuevo proyecto vacío de aplicación universal de Windows a la solución con **Archivo &gt; Agregar &gt; Nuevo proyecto**. En el cuadro de diálogo **Agregar nuevo proyecto** , elija **instalado > C# visual > aplicación vacía (Windows universal)** y asígnele el nombre **ClientApp**.

2.  En el proyecto **ClientApp** , agregue la siguiente instrucción **using** a la parte superior de **mainpage.Xaml.CS**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Agregue un cuadro de texto denominado **textBox** y un botón a **mainpage. Xaml**.

4.  Agregue un controlador de clic de botón para el botón llamado **button_Click**y agregue la palabra clave **Async** a la firma del controlador de botón.

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
    > Asegúrese de pegar en el literal de cadena, en lugar de colocarlo en una variable. No funcionará si usa una variable.

    En primer lugar, el código establece una conexión con el servicio de aplicaciones. La conexión permanecerá abierta hasta que deseches `this.inventoryService`. El nombre de App Service debe coincidir con `Name` el `AppService` atributo del elemento que agregó al archivo **Package. appxmanifest** del proyecto **AppServiceProvider** . En este ejemplo, es `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Se crea un [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) denominado `message` para especificar el comando que se desea enviar al servicio de aplicaciones. El servicio de aplicaciones de ejemplo espera un comando para indicar qué dos acciones realizar. Obtenemos el índice en el cuadro de texto de la aplicación cliente y, a continuación, llamaremos `Item` al servicio con el comando para obtener la descripción del elemento. A continuación, hacemos la llamada con el comando `Price` para obtener el precio del artículo. El texto del botón se define en el resultado.

    Dado que [AppServiceResponseStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) solo indica si el sistema operativo pudo conectar la llamada a App Service, comprobamos la `Status` clave de la [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) que recibimos de App Service para asegurarse de que pudo completar el Solicite.

6. Establezca el proyecto **ClientApp** en el proyecto de inicio (haga clic con el botón secundario en el **Explorador de soluciones** > **establecer como proyecto de inicio**) y ejecute la solución. Escribe el número 1 en el cuadro de texto y haz clic en el botón. Debe obtener "silla: Price = 88,99 "atrás desde el servicio.

    ![aplicación de muestra que muestra el precio de la silla=88.99](images/appserviceclientapp.png)

Si se produce un error en la llamada de App Service, compruebe lo siguiente en el proyecto **ClientApp** :

1.  Compruebe que el nombre de familia del paquete asignado a la conexión del servicio de inventario coincide con el nombre de familia del paquete de la aplicación **AppServiceProvider** . Vea el botón `this.inventoryService.PackageFamilyName = "...";` **\_** de línea en haga clic con.
2.  En **haga\_clic**en el botón, compruebe que el nombre de App Service que se asigna a la conexión del servicio de inventario coincide con el nombre del servicio de la aplicación en el archivo **Package. appxmanifest** de **AppServiceProvider**. Consulta: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Asegúrese de que se ha implementado la aplicación **AppServiceProvider** . (En el **Explorador de soluciones**, haga clic con el botón secundario en la solución y elija **implementar solución**).

## <a name="debug-the-app-service"></a>Depurar el servicio de aplicaciones

1.  Asegúrate de que se implemente la solución antes de la depuración, porque hay que implementar la aplicación del proveedor de servicio de aplicaciones para poder llamar al servicio. (En Visual Studio, **Compilar &gt; Implementar solución**).
2.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **AppServiceProvider** y elija **propiedades**. Desde la pestaña **Depurar** cambia **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. (Ten en cuenta que si estuvieras usando C++ para implementar el proveedor de servicios de aplicaciones, desde la pestaña **Depuración** deberías cambiar **Iniciar aplicación** a **No**).
3.  En el proyecto **MyAppService** , en el archivo **Inventory.CS** , establezca un punto de interrupción en **OnRequestReceived**.
4.  Establezca el proyecto **AppServiceProvider** en el proyecto de inicio y presione **F5**.
5.  Inicie **ClientApp** desde el menú Inicio (no desde Visual Studio).
6.  Escribe el número 1 en el cuadro de texto y presiona el botón. El depurador se detendrá en la llamada al servicio de aplicaciones en el punto de interrupción del servicio de aplicaciones.

## <a name="debug-the-client"></a>Depurar el cliente

1.  Sigue las instrucciones del paso anterior para depurar el cliente que llama al servicio de aplicaciones.
2.  Inicie **ClientApp** desde el menú Inicio.
3.  Adjunte el depurador al proceso **ClientApp. exe** (no el proceso **ApplicationFrameHost. exe** ). (En Visual Studio, elige **Depurar &gt; Asociar al proceso…** )
4.  En el proyecto **ClientApp** , establezca un punto de interrupción en el **botón\_clic**.
5.  Ahora se alcanzarán los puntos de interrupción tanto en el cliente como en el servicio de aplicaciones cuando escriba el número 1 en el cuadro de texto de **ClientApp** y haga clic en el botón.

## <a name="general-app-service-troubleshooting"></a>Solución de errores generales de servicio de aplicaciones

Si encuentra un estado de **AppUnavailable** después de intentar conectarse a un servicio de aplicaciones, compruebe lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente ya que, de lo contrario, el cliente no tendrá nada a lo que conectarse. Puedes implementar desde Visual Studio usando **Compilar** > **solución de implementación**.
- En el **Explorador de soluciones**, asegúrese de que el proyecto de proveedor de App Service tiene una referencia de proyecto a proyecto al proyecto que implementa App Service.
- Compruebe que la `<Extensions>` entrada y sus elementos secundarios se han agregado al archivo **Package. appxmanifest** que pertenece al proyecto de proveedor de App Service, tal y como se especificó anteriormente en [Agregar una extensión de App Service a package. appxmanifest](#appxmanifest).
- Asegúrese de que la cadena [AppServiceConnection. AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) del cliente que llama al proveedor de App Service coincide `<uap3:AppService Name="..." />` con la especificada en el archivo **Package. appxmanifest** del proyecto del proveedor de App Service.
- Asegúrese de que [AppServiceConnection. PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) coincide con el nombre de familia del paquete del componente de proveedor de App Service tal y como se especificó anteriormente en [Agregar una extensión de App Service a package. appxmanifest.](#appxmanifest)
- En el caso de los servicios de aplicaciones fuera de proceso, como el de este ejemplo, compruebe `EntryPoint` que el especificado `<uap:Extension ...>` en el elemento del archivo **Package. appxmanifest** del proyecto de App Service Provider coincide con el espacio de nombres y el nombre de clase del público. clase que implementa [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) en el proyecto de App Service.

### <a name="troubleshoot-debugging"></a>Solucionar problemas de depuración

Si el depurador no se detiene en los puntos de interrupción de los proyectos del proveedor de servicios de aplicaciones o de servicio de aplicaciones, comprueba lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente. Puedes implementarlos desde Visual Studio usando **Compilar** > **Solución de implementación**.
- Asegúrese de que el proyecto que desea depurar se establece como el proyecto de inicio y que las propiedades de depuración de ese proyecto están configuradas para no ejecutar el proyecto cuando se presiona **F5** . Haz clic con el botón derecho en el proyecto y, a continuación, haz clic en **Propiedades** y después en **Depurar** (o **Depuración** en C++). En C#, cambia la **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. En C++, establece **Iniciar aplicación** en **No**.

## <a name="remarks"></a>Comentarios

Este ejemplo proporciona una introducción para crear un servicio de aplicaciones que se ejecuta como tarea en segundo plano y llamarlo desde otra aplicación. Los aspectos clave que hay que tener en cuenta son:

* Cree una tarea en segundo plano para hospedar el servicio de aplicaciones.
* Agregue la `windows.appService` extensión al archivo **Package. appxmanifest** del proveedor de App Service.
* Obtenga el nombre de familia del paquete del proveedor de App Service para que podamos conectarse a él desde la aplicación cliente.
* Agregue una referencia de proyecto a proyecto del proyecto de proveedor de App Service al proyecto de App Service.
* Use [Windows. ApplicationModel. AppService. AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) para llamar al servicio.

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
* [Ejemplo de código de APPC#Service C++(, y VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)

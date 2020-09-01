---
title: Crear y usar un servicio de aplicaciones
description: Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo usar esos servicios.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: comunicación entre aplicaciones, comunicación entre procesos, IPC, mensajería en segundo plano, comunicación en segundo plano, aplicación en aplicación, App Service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd67c8a94ae7fc46956f5a6f7c3358e0e28965d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158829"
---
# <a name="create-and-consume-an-app-service"></a>Crear y usar un servicio de aplicaciones

Los servicios de aplicaciones son aplicaciones para UWP que proporcionan servicios a otras aplicaciones de UWP. Son análogos a los servicios Web, en un dispositivo. Una aplicación de App Service se ejecuta como una tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de análisis de código de barras que otras aplicaciones podrían usar. O quizás un conjunto de aplicaciones de empresa tiene un servicio de aplicaciones de revisión ortográfica común que está disponible para las demás aplicaciones del conjunto.  App Services le permite crear servicios sin interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y a partir de Windows 10, versión 1607, en dispositivos remotos.

A partir de Windows 10, versión 1607, puedes crear servicios de aplicación que se ejecutan en el mismo proceso que la aplicación host. Este artículo se centra en la creación y el consumo de una aplicación de App Service que se ejecuta en un proceso en segundo plano independiente. Consulte [conversión de un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para obtener más detalles sobre cómo ejecutar un servicio de aplicaciones en el mismo proceso que el proveedor.

Para obtener un ejemplo de código de App Service, consulte [ejemplos de aplicaciones de plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Crear un proyecto nuevo de proveedor de servicios de aplicaciones

En este procedimiento, lo crearemos todo en una solución para hacerlo más sencillo.

1. En Visual Studio 2015 o posterior, cree un nuevo proyecto de aplicación para UWP y asígnele el nombre **AppServiceProvider**.
    1. Seleccione el **archivo > nuevo proyecto de >...** 
    2. En el cuadro de diálogo **crear un nuevo proyecto** , seleccione **aplicación vacía (Windows universal) C#**. Se trata de la aplicación que hace que App Service esté disponible para otras aplicaciones de UWP.
    3. Haga clic en **siguiente**y, a continuación, asigne al proyecto el nombre **AppServiceProvider**, elija una ubicación para él y, a continuación, haga clic en **crear**.

2. Cuando se le pida que seleccione un **destino** y una **versión mínima** para el proyecto, seleccione al menos **10.0.14393**. Si desea usar el nuevo atributo **SupportsMultipleInstances** , debe usar visual Studio 2017 o visual Studio 2019 y el destino **10.0.15063** (**Windows 10 Creators Update**) o superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Agregar una extensión de App Service a package. appxmanifest

En el proyecto **AppServiceProvider** , abra el archivo **Package. appxmanifest** en un editor de texto: 

1. Haga clic con el botón secundario en el **Explorador de soluciones**. 
2. Seleccione **abrir con**. 
3. Seleccione **Editor XML (texto)**. 

Agregue la siguiente `AppService` extensión dentro del `<Application>` elemento. Este ejemplo anuncia el servicio `com.microsoft.inventory` y es lo que identifica a esta aplicación como un proveedor de servicios de aplicaciones. El servicio real se implementará como una tarea en segundo plano. El proyecto de App Service expone el servicio a otras aplicaciones. Se recomienda usar un estilo inverso de nombre de dominio para el nombre del servicio.

Tenga en cuenta que el `xmlns:uap4` Prefijo de espacio de nombres y el `uap4:SupportsMultipleInstances` atributo solo son válidos si tiene como destino Windows SDK versión 10.0.15063 o posterior. Puede quitarlos de forma segura si tiene como destino versiones anteriores del SDK.

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

El `SupportsMultipleInstances` atributo indica que, cada vez que se llama a App Service, debe ejecutarse en un nuevo proceso. Esto no es necesario, pero está disponible si necesita esa funcionalidad y tiene como destino el SDK de 10.0.15063 (**Windows 10 Creators Update**) o superior. También debe ir precedido por el espacio de `uap4` nombres.

## <a name="create-the-app-service"></a>Crear el servicio de aplicaciones

1.  Una aplicación de App Service se puede implementar como una tarea en segundo plano. Esto permite que una aplicación de primer plano invoque un servicio de aplicaciones en otra aplicación. Para crear una aplicación de App Service como una tarea en segundo plano, agregue un nuevo Windows Runtime proyecto de componente a la solución (**archivo &gt; agregar &gt; nuevo proyecto**) llamado **MyAppService**. En el cuadro de diálogo **Agregar nuevo proyecto** , elija **instalado > componente de Visual C# > Windows Runtime (Windows universal)**.
2.  En el proyecto **appserviceprovider** , agregue una referencia de proyecto a proyecto al nuevo proyecto **MyAppService** (en el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **AppServiceProvider** > solución **Agregar**  >  **Reference**  >  **proyectos**  >  **Solution**de referencia, seleccione **MyAppService**  >  **Aceptar**). Este paso es fundamental porque si no agrega la referencia, App Service no se conectará en tiempo de ejecución.
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

    Se llama a **Run** cuando se crea la tarea en segundo plano. Dado que las tareas en segundo plano finalizan una vez completada la **ejecución** , el código toma un aplazamiento para que la tarea en segundo plano esté a la espera de atender las solicitudes. Un servicio de aplicaciones que se implementa como una tarea en segundo plano permanecerá activo durante unos 30 segundos después de recibir una llamada a menos que se vuelva a llamar en ese período de tiempo o se extraiga un aplazamiento. Si app Service se implementa en el mismo proceso que el autor de la llamada, la duración de App Service está ligada a la duración del autor de la llamada.

    La duración de la aplicación de servicio depende del autor de la llamada:

    * Si el autor de la llamada está en primer plano, la duración de App Service es la misma que la del autor de la llamada.
    * Si el autor de la llamada está en segundo plano, el servicio de aplicación obtiene 30 segundos para ejecutarse. La toma de un aplazamiento proporciona una vez más una vez 5 segundos.

    Se llama a **OnTaskCanceled** cuando se cancela la tarea. La tarea se cancela cuando la aplicación cliente desecha el [AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), se suspende la aplicación cliente, el sistema operativo se apaga o se suspende, o el sistema operativo se queda sin recursos para ejecutar la tarea.

## <a name="write-the-code-for-the-app-service"></a>Escribir el código para el servicio de aplicaciones

**OnRequestReceived** es el lugar donde se dirige el código para App Service. Reemplace el código auxiliar **OnRequestReceived** en **Inventory.CS** de **MyAppService**por el código de este ejemplo. Este código obtiene un índice para un elemento de inventario y, junto con una cadena de comandos, lo pasa al servicio para recuperar el nombre y el precio del elemento de inventario especificado. Para sus propios proyectos, agregue código de control de errores.

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

Tenga en cuenta que **OnRequestReceived** es **Async** porque se realiza una llamada de método que se pueda esperar a [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) en este ejemplo.

Se toma un aplazamiento para que el servicio pueda usar métodos **asincrónicos** en el controlador **OnRequestReceived** . Garantiza que la llamada a **OnRequestReceived** no se completa hasta que se termina de procesar el mensaje.  [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) envía el resultado al llamador. **SendResponseAsync** no indica la finalización de la llamada. Se trata de la finalización del aplazamiento que señala a [SendMessageAsync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) que **OnRequestReceived** se ha completado. La llamada a **SendResponseAsync** se ajusta en un bloque try/finally porque debe completar el aplazamiento incluso si **SendResponseAsync** produce una excepción.

Los servicios de aplicaciones usan objetos [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) para intercambiar información. El tamaño de los datos que se pueden pasar solo está limitado por los recursos del sistema. No hay claves predefinidas para su uso en el elemento **ValueSet**. Debes determinar qué valores clave usarás para definir el protocolo del servicio de aplicaciones. Debes escribir el llamador teniendo ese protocolo presente. En este ejemplo, se ha elegido una clave denominada `Command` que tiene un valor que indica si se desea que App Service proporcione el nombre del artículo de inventario o su precio. El índice del nombre del inventario se almacena en la `ID` clave. El valor devuelto se almacena en la `Result` clave.

Se devuelve al llamador una enumeración [AppServiceClosedStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) para indicar si la llamada al servicio de aplicaciones se ha realizado correctamente o no. Un ejemplo de cómo se puede producir un error en la llamada a App Service es si el sistema operativo anula el punto de conexión de servicio porque se han superado sus recursos. Puedes devolver información adicional sobre errores a través de [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet). En este ejemplo, usamos una clave denominada `Status` para devolver información más detallada sobre el error al autor de la llamada.

La llamada a [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) devuelve el elemento [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) al llamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete

El proveedor de App Service debe implementarse para poder llamarlo desde un cliente. Puede implementarlo seleccionando **Compilar > implementar solución** en Visual Studio.

También necesitará el nombre de familia del paquete del proveedor de App Service para llamarlo. Para obtenerlo, abra el archivo **Package. appxmanifest** del proyecto **AppServiceProvider** en la vista del diseñador (haga doble clic en él en el **Explorador de soluciones**). Seleccione la pestaña **empaquetado** , copie el valor junto a **nombre de familia de paquete**y péguelo en un lugar como el Bloc de notas por ahora.

## <a name="write-a-client-to-call-the-app-service"></a>Escribir un cliente para llamar al servicio de aplicaciones

1.  Agregue un nuevo proyecto de aplicación universal de Windows en blanco a la solución con el **archivo &gt; agregar &gt; nuevo proyecto**. En el cuadro de diálogo **Agregar nuevo proyecto** , elija **instalado > Visual C# > aplicación vacía (Windows universal)** y asígnele el nombre **ClientApp**.

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
    
    Reemplace el nombre de la familia del paquete en la línea `this.inventoryService.PackageFamilyName = "Replace with the package family name";` con el nombre de familia del paquete del proyecto **AppServiceProvider** que obtuvo anteriormente en [implementación de la aplicación de servicio y obtenga el nombre de familia del paquete](#deploy-the-service-app-and-get-the-package-family-name).

    > [!NOTE]
    > Asegúrese de pegar en el literal de cadena, en lugar de colocarlo en una variable. No funcionará si usa una variable.

    En primer lugar, el código establece una conexión con el servicio de aplicaciones. La conexión permanecerá abierta hasta que se elimine `this.inventoryService` . El nombre de App Service debe coincidir con el `AppService` atributo del elemento `Name` que agregó al archivo **Package. Appxmanifest** del proyecto **AppServiceProvider** . En este ejemplo, es `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Se crea un [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) denominado `message` para especificar el comando que se desea enviar a App Service. El servicio de aplicaciones de ejemplo espera un comando para indicar qué dos acciones realizar. Obtenemos el índice en el cuadro de texto de la aplicación cliente y, a continuación, llamaremos al servicio con el `Item` comando para obtener la descripción del elemento. A continuación, realizamos la llamada con el `Price` comando para obtener el precio del artículo. El texto del botón se define en el resultado.

    Dado que [AppServiceResponseStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) solo indica si el sistema operativo pudo conectar la llamada a App Service, comprobamos la `Status` clave de la instancia de [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) que recibimos de App Service para asegurarse de que pudo completar la solicitud.

6. Establezca el proyecto **ClientApp** en el proyecto de inicio (haga clic con el botón secundario en el **Explorador de soluciones**  >  **establecer como proyecto de inicio**) y ejecute la solución. Escribe el número 1 en el cuadro de texto y haz clic en el botón. El servicio debería devolver "Chair : Price = 88.99".

    ![aplicación de muestra que muestra el precio de la silla=88.99](images/appserviceclientapp.png)

Si se produce un error en la llamada de App Service, compruebe lo siguiente en el proyecto **ClientApp** :

1.  Compruebe que el nombre de familia del paquete asignado a la conexión del servicio de inventario coincide con el nombre de familia del paquete de la aplicación **AppServiceProvider** . Vea el botón de línea en ** \_ haga clic** con `this.inventoryService.PackageFamilyName = "...";` .
2.  En ** \_ haga clic**en el botón, compruebe que el nombre de App Service que se asigna a la conexión del servicio de inventario coincide con el nombre del servicio de la aplicación en el archivo **Package. appxmanifest** de **AppServiceProvider**. Consulta: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Asegúrese de que se ha implementado la aplicación **AppServiceProvider** . (En el **Explorador de soluciones**, haga clic con el botón secundario en la solución y elija **implementar solución**).

## <a name="debug-the-app-service"></a>Depurar el servicio de aplicaciones

1.  Asegúrese de que la solución se implementa antes de la depuración porque se debe implementar la aplicación del proveedor de App Service antes de que se pueda llamar al servicio. (En Visual Studio, **Compilar &gt; Implementar solución**).
2.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **AppServiceProvider** y elija **propiedades**. Desde la pestaña **Depurar** cambia **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. (Tenga en cuenta que, si usa C++ para implementar su proveedor de servicios de aplicaciones, en la pestaña **depuración** se cambiaría la **aplicación de inicio** a **no**).
3.  En el proyecto **MyAppService** , en el archivo **Inventory.CS** , establezca un punto de interrupción en **OnRequestReceived**.
4.  Establezca el proyecto **AppServiceProvider** en el proyecto de inicio y presione **F5**.
5.  Inicie **ClientApp** desde el menú Inicio (no desde Visual Studio).
6.  Escribe el número 1 en el cuadro de texto y presiona el botón. El depurador se detendrá en la llamada al servicio de aplicaciones en el punto de interrupción del servicio de aplicaciones.

## <a name="debug-the-client"></a>Depurar el cliente

1.  Siga las instrucciones del paso anterior para depurar el cliente que llama a App Service.
2.  Inicie **ClientApp** desde el menú Inicio.
3.  Adjunte el depurador al proceso de **ClientApp.exe** (no al proceso de **ApplicationFrameHost.exe** ). (En Visual Studio, elige **Depurar &gt; Asociar al proceso…**)
4.  En el proyecto **ClientApp** , establezca un punto de interrupción en el **botón \_ clic**.
5.  Ahora se alcanzarán los puntos de interrupción tanto en el cliente como en el servicio de aplicaciones cuando escriba el número 1 en el cuadro de texto de **ClientApp** y haga clic en el botón.

## <a name="general-app-service-troubleshooting"></a>Solución de problemas generales de App Service

Si encuentra un estado de **AppUnavailable** después de intentar conectarse a un servicio de aplicaciones, compruebe lo siguiente:

- Asegúrese de que se han implementado el proyecto de proveedor de App Service y el proyecto de App Service. Ambos deben implementarse antes de ejecutar el cliente porque, de lo contrario, el cliente no tendrá ningún elemento al que conectarse. Puede implementar desde Visual Studio mediante la solución de implementación de **compilación**  >  **Deploy Solution**.
- En el **Explorador de soluciones**, asegúrese de que el proyecto de proveedor de App Service tiene una referencia de proyecto a proyecto al proyecto que implementa App Service.
- Compruebe que la `<Extensions>` entrada y sus elementos secundarios se han agregado al archivo **Package. appxmanifest** que pertenece al proyecto de proveedor de App Service, tal y como se especificó anteriormente en [Agregar una extensión de App Service a package. appxmanifest](#appxmanifest).
- Asegúrese de que la cadena [AppServiceConnection. AppServiceName](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) del cliente que llama al proveedor de App Service coincide con la `<uap3:AppService Name="..." />` especificada en el archivo **Package. appxmanifest** del proyecto del proveedor de App Service.
- Asegúrese de que [AppServiceConnection. PackageFamilyName](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) coincide con el nombre de familia del paquete del componente de proveedor de App Service tal y como se especificó anteriormente en [Agregar una extensión de App Service a package. appxmanifest.](#appxmanifest)
- En el caso de los servicios de aplicaciones fuera de proceso, como el de este ejemplo, compruebe que el `EntryPoint` especificado en el `<uap:Extension ...>` elemento del archivo **Package. appxmanifest** del proyecto de App Service Provider coincide con el espacio de nombres y el nombre de clase de la clase pública que implementa [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask) en el proyecto de App Service.

### <a name="troubleshoot-debugging"></a>Solución de problemas de depuración

Si el depurador no se detiene en los puntos de interrupción en el proveedor de App Service o en los proyectos de App Service, compruebe lo siguiente:

- Asegúrese de que se han implementado el proyecto de proveedor de App Service y el proyecto de App Service. Ambos deben implementarse antes de ejecutar el cliente. Puede implementarlos desde Visual Studio mediante la implementación de la solución de **compilación**  >  **Deploy Solution**.
- Asegúrese de que el proyecto que desea depurar se establece como el proyecto de inicio y que las propiedades de depuración de ese proyecto están configuradas para no ejecutar el proyecto cuando se presiona **F5** . Haga clic con el botón secundario en el proyecto, haga clic en **propiedades**y, a continuación, en **depurar** (o **depurar** en C++). En C#, cambie la **acción de inicio** a **no iniciar, pero depure mi código cuando se inicie**. En C++, establezca **Iniciar aplicación** en **no**.

## <a name="remarks"></a>Observaciones

Este ejemplo proporciona una introducción a la creación de una aplicación de App Service que se ejecuta como una tarea en segundo plano y la llama desde otra aplicación. Los aspectos clave que hay que tener en cuenta son:

* Cree una tarea en segundo plano para hospedar el servicio de aplicaciones.
* Agregue la `windows.appService` extensión al archivo **Package. appxmanifest** del proveedor de App Service.
* Obtenga el nombre de familia del paquete del proveedor de App Service para que podamos conectarse a él desde la aplicación cliente.
* Agregue una referencia de proyecto a proyecto del proyecto de proveedor de App Service al proyecto de App Service.
* Use [Windows. ApplicationModel. AppService. AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) para llamar al servicio.

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
* [Hacer que tu aplicación sea compatible con las tareas en segundo plano](support-your-app-with-background-tasks.md)
* [Ejemplo de código de App Service (C#, C++ y VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
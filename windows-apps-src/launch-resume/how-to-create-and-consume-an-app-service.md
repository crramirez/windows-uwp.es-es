---
author: TylerMSFT
title: Crear y usar un servicio de aplicación
description: Obtén información sobre cómo escribir una aplicación para la Plataforma universal de Windows (UWP) que pueda proporcionar servicios a otras aplicaciones para UWP y cómo usar esos servicios.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: comunicación de la aplicación a otra, comunicación entre procesos, IPC, mensajería, servicio de aplicaciones en segundo plano comunicación, aplicaciones,
ms.author: twhitney
ms.date: 09/18/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1407187f9883f44bb9fdc56fd3ae80820b5920f8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6979683"
---
# <a name="create-and-consume-an-app-service"></a>Crear y usar un servicio de aplicaciones

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son similares a servicios web, en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.  Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos.

A partir de Windows 10, versión 1607, puedes crear servicios de aplicación que se ejecutan en el mismo proceso que la aplicación host. Este artículo se centra en la creación y el consumo de un servicio de aplicaciones que se ejecuta en un proceso en segundo plano independiente. Consulta [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para obtener más información acerca de los servicios de aplicación que se ejecutan en el mismo proceso que el proveedor.

Para un ejemplo de código de servicio de aplicación, consulta [Muestras de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Crear un proyecto nuevo de proveedor de App Service

En este procedimiento, lo crearemos todo en una solución para hacerlo más sencillo.

-   En Microsoft Visual Studio, crea un nuevo proyecto de aplicación para UWP y llámalo **AppServiceProvider**. (En el cuadro de diálogo **Nuevo proyecto**, selecciona **Plantillas &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Aplicación vacía (Windows Universal)**). Esta será la aplicación que dejará disponible el servicio de la aplicación para otras aplicaciones para UWP.
-   Cuando se pida seleccionar una **versión de destino** para el proyecto, selecciona al menos **10.0.14393**. Si deseas usar el nuevo atributo `SupportsMultipleInstances`, debes usar Visual Studio 2017 y seleccionar **10.0.15063** (**Windows 10 Creators Update**) o superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Agregar una extensión de servicio de aplicaciones a package.appxmanifest

En el archivo Package.appxmanifest del proyecto AppServiceProvider, agrega la siguiente extensión de AppService dentro del elemento `&lt;Application&gt;`. Este ejemplo anuncia el servicio `com.Microsoft.Inventory` y es lo que identifica a esta aplicación como proveedor de servicios de aplicaciones. El servicio real se implementará como una tarea en segundo plano. El proyecto de servicio de aplicaciones expone el servicio a otras aplicaciones. Se recomienda usar un estilo inverso de nombre de dominio para el nombre del servicio.

Ten en cuenta que el prefijo del espacio de nombres `xmlns:uap4` y el atributo `uap4:SupportsMultipleInstances` solo son válidos si seleccionas la versión de Windows SDK 10.0.15063 o superior. Puedes eliminarlos con seguridad si estás seleccionando versiones anteriores de SDK.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServicesProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServicesProvider.App">
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

El atributo **Category** identifica esta aplicación como proveedor de servicios de aplicaciones.

El atributo **EntryPoint** identifica la clase cualificada de espacio de nombres que implementa el servicio, y que se implementará a continuación.

El atributo **SupportsMultipleInstances** indica que cada vez que se llama al servicio de aplicaciones se debe ejecutar en un nuevo proceso. Esto no es necesario, pero está disponible si necesitas esa funcionalidad y seleccionas el `10.0.15063` SDK (**Windows 10 Creators Update**) o superior. También debe ser precedido por el espacio de nombres `uap4`.

## <a name="create-the-app-service"></a>Crear el servicio de aplicaciones

1.  Un servicio de aplicaciones puede implementarse como una tarea en segundo plano. Esto permite que una aplicación en primer plano invoque un servicio de aplicaciones en otra aplicación. Para crear un servicio de aplicaciones como tarea en segundo plano, agrega un nuevo proyecto de componente de Windows Runtime a la solución (**Archivo &gt; Agregar &gt; Nuevo proyecto**) denominado MyAppService. (En el cuadro de diálogo **Agregar nuevo proyecto** elige **Instalado &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Componente de Windows Runtime (Windows universal)**
2.  En el proyecto **AppServiceProvider**, agrega una referencia de proyecto a proyecto en el nuevo proyecto **MyAppService** (en el Explorador de soluciones, haz clic con el botón derecho en el proyecto **AppServiceProvider** > **Agregar** > **Referencia** > **Proyectos** > **Solución** y selecciona **MyAppService** > **Aceptar**). Este paso es fundamental porque, si no agregas la referencia, el servicio de aplicaciones no se conectará en el momento de ejecución.
3.  En el proyecto MyappService, agrega las siguientes instrucciones **usando** declaraciones a la parte superior de Class1.cs:
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
            // This function is called when the app service receives a request
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

    Se llama a **Run()** cuando se crea la tarea en segundo plano. Como las tareas en segundo plano finalizan cuando se completa la función **Run**, el código sufre un aplazamiento, para que la tarea en segundo plano se mantenga hasta que atienda las solicitudes de servicio. Un servicio de aplicaciones que se implementa como tarea en segundo plano se mantendrá activo durante aproximadamente 30 segundos después de recibir una llamada, a menos que se le llame de nuevo dentro de dicha ventana del tiempo o se produzca un aplazamiento. Si el servicio de aplicaciones se implementa en el mismo proceso que el llamador, la duración del servicio de aplicaciones queda vinculada a la duración del llamador.

    La duración del servicio de aplicaciones depende el llamador:

    1. Si el llamador está en primer plano, la duración del servicio de aplicaciones es la misma que la del llamador.
    2. Si el llamador está en segundo plano, el servicio de aplicaciones tarda 30 segundos en ejecutarse. La finalización de un aplazamiento conlleva un tiempo adicional de 5 segundos.

    Se llama a **OnTaskCanceled()** cuando se cancela la tarea. La tarea se cancela cuando la aplicación cliente desecha el elemento [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704), cuando se suspende la aplicación cliente, cuando el sistema operativo se apaga o se suspende o cuando el sistema operativo se queda sin recursos para ejecutar la tarea.

## <a name="write-the-code-for-the-app-service"></a>Escribir el código para el servicio de aplicaciones

El código para el servicio de aplicaciones se encuentra en **OnRequestReceived()**. Reemplaza el código auxiliar **OnRequestReceived()** en el archivo Class1.cs de MyAppService por el código de este ejemplo. Este código obtiene un índice para un elemento de inventario y, junto con una cadena de comandos, lo pasa al servicio para recuperar el nombre y el precio del elemento de inventario especificado. Para tus propios proyectos, agrega un código de manejo de errores.

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

    try
    {
        await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    }
    catch (Exception e)
    {
        // your exception handling code here
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Ten en cuenta que **OnRequestReceived()** es de tipo **async** porque en este ejemplo se realiza una llamada de método que admite espera a [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722).

Se toma un aplazamiento para que el servicio pueda usar métodos **async** en el controlador OnRequestReceived. Esto asegura que la llamada a **OnRequestReceived** no finalizará hasta que termine de procesar el mensaje.  [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) envía el resultado a la persona que llama. **SendResponseAsync** no indica la finalización de la llamada. Es la finalización del aplazamiento que indica a [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) que **OnRequestReceived** se ha completado. La llamada a **SendResponseAsync()** se ajusta en un bloque try/finally porque debes completar el aplazamiento incluso si **SendResponseAsync()** inicia una excepción.

Los servicios de aplicaciones usan un elemento [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) para intercambiar información. El tamaño de los datos que se pueden pasar solo está limitado por los recursos del sistema. No hay claves predefinidas para su uso en el elemento **ValueSet**. Debes determinar qué valores clave usarás para definir el protocolo del servicio de aplicaciones. Debes escribirse el llamador teniendo ese protocolo presente. En este ejemplo, hemos elegido una clave denominada `Command` que tiene un valor que indica si queremos que el servicio de aplicaciones proporcione el nombre del elemento de inventario o su precio. El índice del nombre de inventario se almacena en la clave `ID`. El valor devuelto se almacena en la clave `Result`.

Se devuelve al llamador una enumeración [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) para indicar si la llamada al servicio de aplicaciones se ha realizado correctamente o no. Un ejemplo de cómo podría producirse un error en la llamada al servicio de aplicaciones es cuando el sistema operativo anula el extremo de servicio porque se han excedido sus recursos. Puedes devolver información adicional sobre errores a través de [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). En este ejemplo, usamos una clave denominada `Status` para devolver información más detallada del error al llamador.

La llamada a [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) devuelve el elemento [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) al llamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implementar el servicio de aplicaciones y obtener el nombre de familia de paquete

Es necesario implementar la aplicación del proveedor del servicio de aplicaciones antes de llamarla desde un cliente. También se necesitará el nombre de familia de paquete de la aplicación de servicio de aplicaciones para poder llamarlo.

Una forma de obtener el nombre de familia de paquete de la aplicación de servicio de aplicaciones consiste en llamar a [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) desde el proyecto **AppServiceProvider** (por ejemplo, desde `public App()` en App.xaml.cs) y tomar nota del resultado. Para ejecutar AppServiceProvider en Microsoft Visual Studio, establécelo como proyecto de inicio en la ventana Explorador de soluciones y ejecuta el proyecto.

Otra forma de obtener el nombre de familia de paquete consiste en implementar la solución (**Compilar &gt; Implementar solución**) y anotar el nombre completo del paquete de la ventana de salida (**Vista &gt; Salida**). Debes quitar la información de plataforma de la cadena en la ventana de salida para derivar el nombre del paquete. Por ejemplo, si el nombre completo del paquete indicado en la ventana de salida era `Microsoft.SDKSamples.AppServicesProvider.CPP_1.0.0.0_x86__8wekyb3d8bbwe`, deberías extraer `1.0.0.0\_x86\_\_" leaving "Microsoft.SDKSamples.AppServicesProvider.CPP_8wekyb3d8bbwe` como nombre de familia de paquete.

## <a name="write-a-client-to-call-the-app-service"></a>Escribir a un cliente para llamar al servicio de aplicaciones

1.  Agrega un nuevo proyecto vacío de aplicación universal de Windows a la solución con **Archivo &gt; Agregar &gt; Nuevo proyecto**. En el cuadro de diálogo **Agregar nuevo proyecto** elige **Instalado &gt; Otros idiomas &gt; Visual C# &gt; Windows &gt; Windows universal &gt; Aplicación vacía (Windows universal)** y dale el nombre **ClientApp**.
2.  En el proyecto ClientApp, agrega lo siguiente **usando** declaraciones a la parte superior de MainPage.xaml.cs:
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

       textBox.Text = result;
   }
   ```
Reemplaza el nombre de familia de paquete en la línea `this.inventoryService.PackageFamilyName = "replace with the package family name";` por el nombre de familia de paquete del proyecto **AppServiceProvider** que obtuviste anteriormente en [Implementar la aplicación de servicio y obtener el nombre de familia de paquete](#deploy-the-service-app-and-get-the-package-family-name).

En primer lugar, el código establece una conexión con el servicio de aplicaciones. La conexión permanecerá abierta hasta que deseches `this.inventoryService`. El nombre del servicio de aplicaciones debe coincidir con el atributo **AppService Name** que hayas agregado al archivo Package.appxmanifest del proyecto AppServiceProvider. En este ejemplo, es `<uap:AppService Name="com.microsoft.inventory"/>`.

Se crea un [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) denominado **message** para especificar el comando que se va a enviar al servicio de aplicaciones. El servicio de aplicaciones de ejemplo espera un comando para indicar cuál de dos acciones realizar. Obtenemos el índice desde el cuadro de texto de la aplicación cliente y luego llamamos al servicio con el comando `Item` para obtener la descripción del artículo. A continuación, hacemos la llamada con el comando `Price` para obtener el precio del artículo. El texto del botón se adecúa al resultado.

Dado que [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) solo indica si el sistema operativo pudo conectar la llamada con el servicio de aplicaciones, comprobamos la clave `Status` en el [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) que recibimos del servicio de aplicaciones para garantizar que pudo cumplir con la solicitud.

6.  En Visual Studio, establece el proyecto ClientApp como proyecto de inicio en la ventana Explorador de soluciones y ejecuta la solución. Escribe el número 1 en el cuadro de texto y haz clic en el botón. El servicio debería devolver "Chair : Price = 88.99".

    ![aplicación de muestra que muestra el precio de la silla=88.99](images/appserviceclientapp.png)

Si se produce un error en la llamada al servicio de aplicaciones, comprueba lo siguiente en el ClientApp:

1.  Comprueba que el nombre de familia de paquete asignado a la conexión de servicio de inventario coincide con el nombre de familia de paquete de la aplicación AppServiceProvider. Consulta: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  En **button\_Click()**, comprueba que el nombre del servicio de aplicaciones que se asigna a la conexión de servicio de inventario coincide con el nombre del servicio de aplicaciones en el archivo Package.appxmanifest de AppServiceProvider. Consulta: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Asegúrate de que se haya implementado la aplicación AppServiceProvider (en el Explorador de soluciones, haz clic con el botón derecho en la solución y elige **Implementar**).

## <a name="debug-the-app-service"></a>Depurar el servicio de aplicaciones

1.  Asegúrate de que se implemente la solución antes de la depuración, porque hay que implementar la aplicación del proveedor de servicio de aplicaciones para poder llamar al servicio. (En Visual Studio, **Compilar &gt; Implementar solución**).
2.  En el Explorador de soluciones, haz clic con el botón derecho en el proyecto **AppServiceProvider** y elige **Propiedades**. Desde la pestaña **Depurar** cambia **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. (Ten en cuenta que si estuvieras usando C++ para implementar el proveedor de servicios de aplicaciones, desde la pestaña **Depuración** deberías cambiar **Iniciar aplicación** a **No**).
3.  En el proyecto MyAppService, en el archivo Class1.cs, establece un punto de interrupción en `OnRequestReceived()`.
4.  Establece el proyecto AppServiceProvider para que sea el proyecto de inicio y a continuación presiona F5.
5.  Inicia ClientApp desde el menú Inicio (y no desde Visual Studio).
6.  Escribe el número 1 en el cuadro de texto y presiona el botón. El depurador se detendrá en la llamada al servicio de aplicaciones en el punto de interrupción del servicio de aplicaciones.

## <a name="debug-the-client"></a>Depurar el cliente

1.  Sigue las instrucciones del paso anterior para depurar el cliente que llama al servicio de aplicaciones.
2.  Inicia ClientApp desde el menú Inicio.
3.  Asocia al depurador al proceso ClientApp.exe (no al proceso ApplicationFrameHost.exe). (En Visual Studio, elige **Depurar &gt; Asociar al proceso…**)
4.  En el proyecto ClientApp, establece un punto de interrupción en **button\_Click()**.
5.  Ahora se alcanzarán los puntos de interrupción tanto en el cliente como en el servicio de aplicaciones cuando escribas el número 1 en el cuadro de texto del ClientApp y hagas clic en el botón.

## <a name="general-app-service-troubleshooting"></a>Solución de errores generales de servicio de aplicaciones ##

Si encuentras un estado **AppUnavailable** después de intentar conectarte a un servicio de aplicaciones, comprueba lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente ya que, de lo contrario, el cliente no tendrá nada a lo que conectarse. Puedes implementar desde Visual Studio usando **Compilar** > **solución de implementación**.
- En el explorador de soluciones, asegúrate de que el proyecto de proveedor de servicio de aplicaciones tiene una referencia proyecto a proyecto al proyecto que implementa el servicio de aplicaciones.
- Comprueba que la entrada `<Extensions>` y sus elementos secundarios se hayan agregado al archivo Package.appxmanifest que pertenece al proyecto de proveedor de servicio de aplicaciones especificado anteriormente en [Agregar una extensión de servicio de aplicaciones a package.appxmanifest](#appxmanifest).
- Asegúrate de que la cadena `AppServiceConnection.AppServiceName` del cliente que llama el proveedor de servicios de aplicaciones coincide con el `<uap3:AppService Name="..." />` especificado en el archivo Package.appxmanifest del proyecto del proveedor de servicio de aplicaciones.
- Asegúrate de que la `AppServiceConnection.PackageFamilyName` coincide con el nombre de familia de paquete del componente del proveedor de servicio de aplicaciones como se especifica más arriba en [Agregar una extensión de servicio de aplicaciones a package.appxmanifest](#appxmanifest)
- Para los servicios de aplicaciones de fuera de proceso, como el de este ejemplo, comprueba que la `EntryPoint` especificada en el elemento `<uap:Extension ...>` del archivo Package.appxmanifest del proyecto del proveedor de servicios de aplicaciones se corresponda con el espacio de nombres y el nombre de la clase pública que implementa `IBackgroundTask` en el proyecto de servicio de aplicaciones.

### <a name="troubleshoot-debugging"></a>Solucionar problemas de depuración

Si el depurador no se detiene en los puntos de interrupción de los proyectos del proveedor de servicios de aplicaciones o de servicio de aplicaciones, comprueba lo siguiente:

- Asegúrate de que el proyecto de proveedor de servicio de aplicaciones y el proyecto de servicio de aplicaciones estén implementados. Ambos deben implementarse antes de ejecutar el cliente. Puedes implementarlos desde Visual Studio usando **Compilar** > **Solución de implementación**.
- Asegúrate de que el proyecto que desee depurar está establecido como proyecto de inicio y que se establecen las propiedades de depuración para el proyecto no se ejecute el proyecto cuando se presiona F5. Haz clic con el botón derecho en el proyecto y, a continuación, haz clic en **Propiedades** y después en **Depurar** (o **Depuración** en C++). En C#, cambia la **Acción de inicio** a **No iniciar, pero depurar mi código al empezar**. En C++, establece **Iniciar aplicación** en **No**.

## <a name="remarks"></a>Comentarios

Este ejemplo proporciona una introducción para crear un servicio de aplicaciones que se ejecuta como tarea en segundo plano y llamarlo desde otra aplicación. Los puntos clave que hay que tener en cuenta son la creación de una tarea en segundo plano para hospedar el servicio de aplicaciones, la adición de la extensión windows.appservice al archivo Package.appxmanifest de la aplicación del proveedor de servicio de aplicaciones, la obtención del nombre de familia de paquete de la aplicación del proveedor de servicio de aplicaciones para poder conectarse a este desde la aplicación cliente, agregar una referencia proyecto a proyecto desde el proyecto del proveedor del servicio de aplicaciones hasta el proyecto de servicio de aplicaciones, y el uso de [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) para llamar al servicio.

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

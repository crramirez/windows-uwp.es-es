---
title: Crear y hospedar una extensión de aplicación
description: Escriba y hospede extensiones de aplicaciones que le permitan ampliar su aplicación a través de paquetes que los usuarios pueden instalar desde el Microsoft Store.
keywords: extensión de aplicación, App Service, Fondo
ms.date: 01/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 122c7c4d206c014d7d76cdab0b1b8fc66c0c9371
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158849"
---
# <a name="create-and-host-an-app-extension"></a>Crear y hospedar una extensión de aplicación

En este artículo se muestra cómo crear una extensión de aplicación de Windows 10 y hospedarla en una aplicación. Las extensiones de aplicación se admiten en aplicaciones para UWP y [aplicaciones de escritorio empaquetadas](/windows/apps/desktop/modernize/#msix-packages).

Para mostrar cómo crear una extensión de aplicación, en este artículo se usan fragmentos de código XML de manifiesto de paquete y fragmentos de código del ejemplo de código de la [extensión Math](https://github.com/MicrosoftDocs/windows-topic-specific-samples/tree/MathExtensionSample). Este ejemplo es una aplicación de UWP, pero las características mostradas en el ejemplo también se aplican a las aplicaciones de escritorio empaquetadas. Siga estas instrucciones para empezar a trabajar con el ejemplo:

- Descargue y descomprima el [ejemplo de código de la extensión Math](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip).
- En Visual Studio 2019, abra MathExtensionSample. sln. Establezca el tipo de compilación en x86 (**compilar**  >  **Configuration Manager**y, a continuación, cambie **plataforma** a **x86** para ambos proyectos).
- Implemente la solución: **compilar**la  >  **solución**.

## <a name="introduction-to-app-extensions"></a>Introducción a las extensiones de aplicaciones

En Windows 10, las extensiones de aplicación proporcionan una funcionalidad similar a la de los complementos, complementos y complementos en otras plataformas. Las extensiones de aplicación se introdujeron en la edición de aniversario de Windows 10 (versión 1607, compilación 10.0.14393).

Las extensiones de aplicaciones son aplicaciones UWP o aplicaciones de escritorio empaquetadas que tienen una declaración de extensión que les permite compartir eventos de contenido e implementación con una aplicación host. Una aplicación de extensión puede proporcionar varias extensiones.

Dado que las extensiones de aplicación son solo aplicaciones UWP o aplicaciones de escritorio empaquetadas, también pueden ser aplicaciones totalmente funcionales, extensiones de host y proporcionar extensiones a otras aplicaciones, sin necesidad de crear paquetes de aplicaciones independientes.

Cuando se crea un host de extensión de aplicación, se crea una oportunidad para desarrollar un ecosistema en torno a la aplicación en la que otros desarrolladores pueden mejorar la aplicación de la forma que pueda no haber esperado o tener los recursos para. Considere la posibilidad de Microsoft Office extensiones, extensiones de Visual Studio, extensiones del explorador, etc. Estos crean experiencias más enriquecidas para las aplicaciones que van más allá de la funcionalidad con la que se distribuyen. Las extensiones pueden agregar valor y longevidad a la aplicación.

En un nivel alto, para configurar una relación de extensión de aplicación, es necesario:

1. Declare una aplicación para que sea un host de extensión.
2. Declare una aplicación para que sea una extensión.
3. Decida si desea implementar la extensión como un servicio de aplicaciones, una tarea en segundo plano u otra manera.
4. Defina cómo se comunicarán los hosts y sus extensiones.
5. Use la API [Windows. ApplicationModel. extensiones](/uwp/api/Windows.ApplicationModel.AppExtensions) en la aplicación host para tener acceso a las extensiones.

Veamos cómo se realiza esta operación examinando el ejemplo de [código de extensión matemática](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) que implementa una calculadora hipotética a la que puede agregar nuevas funciones mediante extensiones. En Microsoft Visual Studio 2019, cargue **MathExtensionSample. sln** desde el ejemplo de código.

![Ejemplo de código de extensión matemática](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Declarar que una aplicación sea un host de extensión

Una aplicación se identifica como un host de extensión de aplicación declarando el `<AppExtensionHost>` elemento en el archivo package. appxmanifest. Vea el archivo **Package. appxmanifest** en el proyecto **MathExtensionHost** para ver cómo se realiza esta operación.

_Package. appxmanifest en el proyecto MathExtensionHost_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Observe la `xmlns:uap3="http://..."` y la presencia de `uap3` en `IgnorableNamespaces` . Estos son necesarios porque usamos el espacio de nombres uap3.

`<uap3:Extension Category="windows.appExtensionHost">` identifica esta aplicación como un host de extensión.

El elemento **Name** en `<uap3:AppExtensionHost>` es el nombre del _contrato_ de la extensión. Cuando una extensión especifica el mismo nombre de contrato de extensión, el host podrá encontrarla. Por Convención, se recomienda crear el nombre del contrato de extensión con el nombre de la aplicación o del editor para evitar posibles colisiones con otros nombres de contrato de extensión.

Puede definir varios hosts y varias extensiones en la misma aplicación. En este ejemplo, se declara un host. La extensión se define en otra aplicación.

## <a name="declare-an-app-to-be-an-extension"></a>Declarar una aplicación para que sea una extensión

Una aplicación se identifica como una extensión de la aplicación declarando el `<uap3:AppExtension>` elemento en el archivo **Package. appxmanifest** . Abra el archivo **Package. appxmanifest** en el proyecto **MathExtension** para ver cómo se realiza esta operación.

_Package. appxmanifest en el proyecto MathExtension:_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

De nuevo, observe la `xmlns:uap3="http://..."` línea y la presencia de `uap3` en `IgnorableNamespaces` . Estos son necesarios porque usamos el espacio de `uap3` nombres.

`<uap3:Extension Category="windows.appExtension">` identifica esta aplicación como una extensión.

El significado de los `<uap3:AppExtension>` atributos es el siguiente:

|Atributo|Descripción|Obligatorio|
|---------|-----------|:------:|
|**Nombre**|Este es el nombre del contrato de extensión. Cuando coincida con el **nombre** declarado en un host, ese host podrá encontrar esta extensión.| :heavy_check_mark: |
|**Id**| Identifica de forma única esta extensión. Dado que puede haber varias extensiones que usen el mismo nombre de contrato de extensión (Imagine una aplicación de Paint que admita varias extensiones), puede usar el identificador para distinguirlos. Los hosts de extensión de aplicación pueden usar el identificador para deducir algo sobre el tipo de extensión. Por ejemplo, podría tener una extensión diseñada para el escritorio y otra para dispositivos móviles, con el identificador como el diferenciador. También puede usar el elemento **Properties** , que se describe a continuación, para eso.| :heavy_check_mark: |
|**DisplayName**| Se puede usar desde la aplicación host para identificar la extensión para el usuario. Es consultable desde y puede usar el [nuevo sistema de administración de recursos](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) ( `ms-resource:TokenName` ) para la localización. El contenido localizado se carga desde el paquete de extensión de aplicación, no desde la aplicación host. | |
|**Descripción** | Se puede usar desde la aplicación host para describir la extensión al usuario. Es consultable desde y puede usar el [nuevo sistema de administración de recursos](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) ( `ms-resource:TokenName` ) para la localización. El contenido localizado se carga desde el paquete de extensión de aplicación, no desde la aplicación host. | |
|**PublicFolder**|El nombre de una carpeta, relativa a la raíz del paquete, donde puede compartir contenido con el host de la extensión. Por Convención, el nombre es "público", pero puede usar cualquier nombre que coincida con una carpeta de la extensión.| :heavy_check_mark: |

`<uap3:Properties>` es un elemento opcional que contiene metadatos personalizados que los hosts pueden leer en tiempo de ejecución. En el ejemplo de código, la extensión se implementa como App Service, por lo que el host necesita una manera de obtener el nombre de ese servicio de la aplicación para que pueda llamarlo. El nombre de App Service se define en el <Service> elemento, que se definió (podríamos haber llamado a todo lo que queríamos). El host del ejemplo de código busca esta propiedad en tiempo de ejecución para conocer el nombre de App Service.

## <a name="decide-how-you-will-implement-the-extension"></a>Decida cómo va a implementar la extensión.

La [sesión Build 2016 acerca de las extensiones de aplicación](https://channel9.msdn.com/Events/Build/2016/B808) muestra cómo usar la carpeta pública compartida entre el host y las extensiones. En ese ejemplo, la extensión se implementa mediante un archivo JavaScript que se almacena en la carpeta pública, que invoca el host. Este enfoque tiene la ventaja de ser ligero, no requiere compilación y puede admitir la creación de la página de aterrizaje predeterminada que proporciona instrucciones para la extensión y un vínculo a la página de Microsoft Store de la aplicación host. Consulte el ejemplo de código de la extensión de la [aplicación Build 2016](https://github.com/Microsoft/App-Extensibility-Sample) para obtener más información. En concreto, vea el proyecto **InvertImageExtension** y `InvokeLoad()` en ExtensionManager.CS en el proyecto **ExtensibilitySample** .

En este ejemplo, usaremos una aplicación de App Service para implementar la extensión. App Services tiene las siguientes ventajas:

- Si la extensión se bloquea, no se desactivará la aplicación host porque la aplicación host se ejecuta en su propio proceso.
- Puede usar el lenguaje que prefiera para implementar el servicio. No es necesario que coincida con el idioma que se usa para implementar la aplicación host.
- App Service tiene acceso a su propio contenedor de aplicaciones, que puede tener diferentes capacidades de las que tiene el host.
- Existe aislamiento entre los datos del servicio y la aplicación host.

### <a name="host-app-service-code"></a>Código de host app Service

Este es el código de host que invoca el servicio de aplicaciones de la extensión:

_ExtensionManager.cs en el proyecto MathExtensionHost_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

Este es el código típico para invocar un servicio de aplicaciones. Para más información sobre cómo implementar y llamar a un servicio de aplicaciones, consulte [creación y uso de una aplicación de App Service](how-to-create-and-consume-an-app-service.md).

Un aspecto que se debe tener en cuenta es cómo se determina el nombre del servicio de aplicaciones que se va a llamar. Dado que el host no tiene información sobre la implementación de la extensión, la extensión debe proporcionar el nombre de su servicio de aplicación. En el ejemplo de código, la extensión declara el nombre de App Service en su archivo en el `<uap3:Properties>` elemento:

_Package. appxmanifest en el proyecto MathExtension_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

Puede definir su propio XML en el `<uap3:Properties>` elemento. En este caso, se define el nombre del servicio de aplicaciones para que el host pueda hacerlo al invocar la extensión.

Cuando el host carga una extensión, el código como este extrae el nombre del servicio de las propiedades definidas en package. appxmanifest de la extensión:

_`Update()` en ExtensionManager.cs, en el proyecto MathExtensionHost_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

Con el nombre del servicio de aplicaciones almacenado en `_serviceName` , el host puede usarlo para invocar a App Service.

La llamada a App Service también requiere el nombre de familia del paquete que contiene App Service. Afortunadamente, la API de extensión de la aplicación proporciona esta información, que se obtiene en la línea: `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Definir cómo se comunicarán el host y la extensión

Los servicios de aplicaciones usan un elemento [ValueSet](/uwp/api/windows.foundation.collections.valueset) para intercambiar información. Como autor del host, necesitará un protocolo para comunicarse con extensiones que sea flexible. En el ejemplo de código, eso significa cuentas para las extensiones que pueden tomar 1, 2 o más argumentos en el futuro.

En este ejemplo, el protocolo para los argumentos es un objeto **ValueSet** que contiene los pares clave-valor denominados ' arg ' + el número de argumento, por ejemplo, `Arg1` y `Arg2` . El host pasa todos los argumentos en el **ValueSet**y la extensión hace uso de los que necesita. Si la extensión puede calcular un resultado, el host espera que la **ValueSet** devuelta por la extensión tenga una clave denominada `Result` que contenga el valor del cálculo. Si esa clave no está presente, el host supone que la extensión no pudo completar el cálculo.

### <a name="extension-app-service-code"></a>Código de App Service de extensión

En el ejemplo de código, el servicio de aplicaciones de la extensión no se implementa como una tarea en segundo plano. En su lugar, usa el modelo de App Service de un solo proc en el que App Service se ejecuta en el mismo proceso que la aplicación de extensión que lo hospeda. Todavía es un proceso diferente de la aplicación host, lo que proporciona las ventajas de la separación de procesos, a la vez que obtiene algunas ventajas de rendimiento evitando la comunicación entre procesos entre el proceso de extensión y el proceso en segundo plano que implementa App Service. Consulte [conversión de un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para ver la diferencia entre un servicio de aplicaciones que se ejecuta como una tarea en segundo plano frente al mismo proceso.

El sistema lo hace `OnBackgroundActivate()` cuando se activa App Service. Ese código configura los controladores de eventos para controlar la llamada real de App Service cuando se incluye ( `OnAppServiceRequestReceived()` ), así como para tratar con eventos de mantenimiento como la obtención de un objeto de aplazamiento que controla un evento Cancel o Closed.

_App.xaml.cs en el proyecto MathExtension._
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

El código que realiza el trabajo de la extensión está en `OnAppServiceRequestReceived()` . Se llama a esta función cuando se invoca App Service para realizar un cálculo. Extrae los valores que necesita de la **ValueSet**. Si puede realizar el cálculo, coloca el resultado, en una clave denominada **result**, en el objeto **ValueSet** que se devuelve al host. Recuerde que, según el protocolo definido para el modo en que este host y sus extensiones se comunicarán, la presencia de una clave de **resultado** indicará que la operación se ha realizado correctamente; de lo contrario, error.

_App.xaml.cs en el proyecto MathExtension._
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>Administración de extensiones

Ahora que hemos visto cómo implementar la relación entre un host y sus extensiones, veremos cómo encuentra un host las extensiones instaladas en el sistema y reacciona a la adición y eliminación de paquetes que contienen extensiones.

El Microsoft Store proporciona extensiones como paquetes. [AppExtensionCatalog](/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) busca los paquetes instalados que contienen extensiones que coinciden con el nombre de contrato de la extensión del host y proporciona eventos que se activan cuando se instala o se quita un paquete de extensión de aplicación pertinente para el host.

En el ejemplo de código, la `ExtensionManager` clase (definida en **ExtensionManager.CS** en el proyecto **MathExtensionHost** ) incluye la lógica para cargar extensiones y responder a las instalaciones y desinstalaciones del paquete de extensión.

El `ExtensionManager` constructor utiliza `AppExtensionCatalog` para buscar las extensiones de la aplicación en el sistema que tienen el mismo nombre de contrato de extensión que el host:

_ExtensionManager.cs en el proyecto MathExtensionHost._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

Cuando se instala un paquete de extensión, `ExtensionManager` recopila información sobre las extensiones del paquete que tienen el mismo nombre de contrato de extensión que el host. Una instalación puede representar una actualización en cuyo caso se actualiza la información de la extensión afectada. Cuando se desinstala un paquete de extensión, el `ExtensionManager` quita la información sobre las extensiones afectadas para que el usuario sepa qué extensiones ya no están disponibles.

La `Extension` clase (definida en **ExtensionManager.CS** en el proyecto **MathExtensionHost** ) se creó para el ejemplo de código para tener acceso al identificador, la descripción, el logotipo y la información específica de la aplicación, por ejemplo, si el usuario ha habilitado la extensión.

Para indicar que la extensión se carga (consulte `Load()` en **ExtensionManager.CS**) significa que el estado del paquete es correcto y que hemos obtenido su identificador, logotipo, Descripción y carpeta pública (que no usamos en este ejemplo, solo para mostrar cómo se obtiene). No se está cargando el paquete de extensión.

El concepto de descarga se usa para realizar un seguimiento de las extensiones que ya no se deben presentar al usuario.

`ExtensionManager`Proporciona instancias de colección `Extension` para que las extensiones, sus nombres, descripciones y logotipos puedan ser datos enlazados a la interfaz de usuario. La página **ExtensionsTab** se enlaza a esta colección y proporciona la interfaz de usuario para habilitar o deshabilitar las extensiones, así como para quitarlas.

![IU de ejemplo de la pestaña extensiones](images/mathextensionhost-extensiontab.png)

 Cuando se quita una extensión, el sistema solicita al usuario que confirme que desea desinstalar el paquete que contiene la extensión (y posiblemente contiene otras extensiones). Si el usuario acepta, se desinstala el paquete y `ExtensionManager` quita las extensiones del paquete desinstalado de la lista de extensiones disponibles para la aplicación host.

 ![Interfaz de usuario de desinstalación](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Depurar extensiones y hosts de aplicaciones

A menudo, el host de extensión y la extensión no forman parte de la misma solución. En ese caso, para depurar el host y la extensión:

1. Cargue el proyecto host en una instancia de Visual Studio.
2. Cargue la extensión en otra instancia de Visual Studio.
3. Inicie la aplicación host en el depurador.
4. Inicie la aplicación de extensión en el depurador. (Si desea implementar la extensión, en lugar de depurarla, para probar el evento de instalación del paquete del host, haga **compilar &gt; implementar solución**en su lugar).

Ahora podrá alcanzar puntos de interrupción en el host y la extensión.
Si inicia la depuración de la aplicación de extensión, verá una ventana en blanco para la aplicación. Si no desea ver la ventana en blanco, puede cambiar la configuración de depuración del proyecto de extensión para que no inicie la aplicación, sino que la depure en su lugar cuando se inicie (haga clic con el botón derecho en el proyecto de extensión, **propiedades**de  >  **depuración** > seleccione no **iniciar, pero depurar mi código al iniciarse**) todavía necesitará iniciar la depuración (**F5**) el proyecto de extensión, pero esperará hasta que el host Active la extensión y se alcanzarán los puntos de interrupción de la extensión.

**Depurar el ejemplo de código**

En el ejemplo de código, el host y la extensión se encuentran en la misma solución. Realice lo siguiente para depurar:

1. Asegúrese de que **MathExtensionHost** es el proyecto de inicio (haga clic con el botón derecho en el proyecto **MathExtensionHost** , haga clic en **establecer como proyecto de inicio**).
2. Coloque un punto de interrupción en `Invoke` ExtensionManager.CS, en el proyecto **MathExtensionHost** .
3. **F5** para ejecutar el proyecto **MathExtensionHost** .
4. Coloque un punto de interrupción en `OnAppServiceRequestReceived` app.Xaml.CS en el proyecto **MathExtension** .
5. Comience a depurar el proyecto **MathExtension** (haga clic con el botón derecho en el proyecto **MathExtension** , **depure > iniciar nueva instancia**) que lo implementará y desencadenará el evento de instalación del paquete en el host.
6. En la aplicación **MathExtensionHost** , navegue hasta la página de **cálculo** y haga clic en **x ^ y** para activar la extensión. El `Invoke()` punto de interrupción se alcanza primero y puede ver la llamada a App Service de extensiones que se está realizando. A continuación `OnAppServiceRequestReceived()` , se alcanza el método de la extensión y puede ver que App Service calcula el resultado y lo devuelve.

**Solución de problemas de extensiones implementadas como App Service**

Si el host de extensión tiene problemas para conectarse a App Service para la extensión, asegúrese de que el `<uap:AppService Name="...">` atributo coincide con lo que se coloca en el `<Service>` elemento. Si no coinciden, el nombre del servicio que proporciona la extensión el host no coincidirá con el nombre del servicio de aplicaciones que ha implementado y el host no podrá activar la extensión.

_Package. appxmanifest en el proyecto MathExtension:_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Una lista de comprobación de escenarios básicos para probar

Al compilar un host de extensión y estar listo para probar la compatibilidad con las extensiones, estos son algunos escenarios básicos para probar:

- Ejecutar el host y, a continuación, implementar una aplicación de extensión  
    - ¿El host recoge nuevas extensiones que se encuentran durante su ejecución?  
- Implemente la aplicación de extensión y, a continuación, implemente y ejecute el host.
    - ¿El host recoge las extensiones ya existentes?  
- Ejecute el host y, a continuación, quite la aplicación de extensión.
    - ¿Detecta correctamente el host la eliminación?
- Ejecute el host y, a continuación, actualice la aplicación de extensión a una versión más reciente.
    - ¿El host recoge el cambio y descarga correctamente las versiones anteriores de la extensión?  

**Escenarios avanzados para probar:**

- Ejecutar el host, migrar la aplicación de extensión a un medio extraíble, quitar el medio
    - ¿Detecta el host el cambio en el estado del paquete y deshabilita la extensión?
- Ejecute el host y, a continuación, dañe la aplicación de extensión (conviértalo en no válido, firmado de manera diferente, etc.).
    - ¿Detecta el host la extensión alterada y la controla correctamente?
- Ejecute el host y, a continuación, implemente una aplicación de extensión que tenga contenido o propiedades no válidos
    - ¿Detecta el host contenido no válido y lo controla correctamente?

## <a name="design-considerations"></a>Consideraciones de diseño

- Proporcione la interfaz de usuario que muestra al usuario qué extensiones están disponibles y permite habilitarlas o deshabilitarlas. También puede considerar la posibilidad de agregar glifos para extensiones que dejan de estar disponibles porque un paquete se queda sin conexión, etc.
- Indique al usuario dónde puede obtener las extensiones. Quizás su página de extensión puede proporcionar una Microsoft Store consulta de búsqueda que muestra una lista de extensiones que se pueden usar con la aplicación.
- Considere la posibilidad de notificar al usuario la adición y eliminación de extensiones. Puede crear una notificación para cuando se instala una nueva extensión e invitar al usuario a habilitarla. Las extensiones deben estar deshabilitadas de forma predeterminada para que los usuarios tengan el control.

## <a name="how-app-extensions-differ-from-optional-packages"></a>Diferencias entre las extensiones de aplicación y los paquetes opcionales

El diferenciador clave entre [paquetes opcionales](/windows/msix/package/optional-packages) y extensiones de aplicación son ecosistema abierto frente a ecosistema cerrado y paquete dependiente frente a paquete independiente.

Las extensiones de aplicaciones participan en un ecosistema abierto. Si la aplicación puede hospedar extensiones de aplicaciones, cualquier usuario puede escribir una extensión para el host siempre que cumplan el método de pasar o recibir información de la extensión. Esto difiere de los paquetes opcionales que participan en un ecosistema cerrado donde el publicador decide quién tiene permiso para crear un paquete opcional que puede usarse con la aplicación.

Las extensiones de aplicaciones son paquetes independientes y pueden ser aplicaciones independientes. No pueden tener una dependencia de implementación en otra aplicación.Los paquetes opcionales requieren el paquete principal y no se pueden ejecutar sin él.

Un paquete de expansión para un juego sería un buen candidato para un paquete opcional porque está estrechamente enlazado con el juego, no se puede ejecutar independientemente del juego y es posible que no quiera que los paquetes de expansión los cree simplemente ningún desarrollador del ecosistema.

Si ese mismo juego disponía de complementos de interfaz de usuario personalizables o de los mismos, una extensión de aplicación podría ser una buena opción porque la aplicación que proporciona la extensión podría ejecutarse por sí misma y cualquier tercero podría crearla.

## <a name="remarks"></a>Observaciones

En este tema se proporciona una introducción a las extensiones de aplicaciones. Los aspectos clave que se deben tener en cuenta son la creación del host y su marca como tal en su paquete. archivo appxmanifest, creando la extensión y marcándolos como tal en el archivo package. appxmanifest, determinando cómo se implementa la extensión (por ejemplo, una aplicación de App Service, una tarea en segundo plano u otros medios), que define cómo el host se comunicará con las extensiones y el uso de la [API de extensiones](/uwp/api/windows.applicationmodel.appextensions) para acceder a las extensiones y administrarlas.

## <a name="related-topics"></a>Temas relacionados

* [Introducción a las extensiones de aplicaciones](/windows/msix/)
* [Compilación 2016 de sesión sobre extensiones de aplicaciones](https://channel9.msdn.com/Events/Build/2016/B808)
* [Ejemplo de código de extensión de aplicación de compilación 2016](https://github.com/Microsoft/App-Extensibility-Sample)
* [Hacer que tu aplicación sea compatible con las tareas en segundo plano](support-your-app-with-background-tasks.md)
* [Cómo crear y usar una aplicación de App Service](how-to-create-and-consume-an-app-service.md).
* [Espacio de nombres extensiones](/uwp/api/windows.applicationmodel.appextensions)
* [Ampliar la aplicación con servicios, extensiones y paquetes](./extend-your-app-with-services-extensions-packages.md)
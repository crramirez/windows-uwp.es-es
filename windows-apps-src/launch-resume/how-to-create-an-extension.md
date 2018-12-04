---
title: Crear y usar una extensión de aplicación
description: Crea y hospeda las extensiones de aplicaciones de la Plataforma universal de Windows (UWP) que te permiten ampliar tu aplicación mediante paquetes que los usuarios pueden instalar desde la Microsoft Store.
keywords: extensión de aplicación, servicio de la aplicación, en segundo plano
ms.date: 10/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 99ba3ee5f62ed9455e95d9e760abdba6009e5027
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8473795"
---
# <a name="create-and-host-an-app-extension"></a>Crear y hospedar una extensión de aplicación

En este artículo se muestra cómo crear una extensión de aplicación para UWP y hospedarla en una aplicación para UWP.

Este artículo se incluye con un ejemplo de código:
- Descarga y descomprime [ejemplo de código de extensión de matemáticas](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip).
- En Visual Studio 2017, abre MathExtensionSample.sln. Establece el tipo de compilación en x86 (**Compilación** > **Configuration Manager** y, a continuación, cambia **Plataforma** a **x86** para ambos proyectos).
- Implementa la solución: **Compilación** > **Implementar solución**.

## <a name="introduction-to-app-extensions"></a>Introducción a las extensiones para aplicaciones

En la Plataforma universal de Windows (UWP), las extensiones para aplicaciones proporcionan una funcionalidad similar a la de los complementos en otras plataformas. Las extensiones de Microsoft Edge son extensiones de la aplicación para UWP, por ejemplo. Las extensiones de aplicaciones para UWP se introdujeron en la Windows 10 Anniversary Edition (versión 1607, compilación 10.0.14393).

Las extensiones de aplicaciones para UWP son aplicaciones para UWP que tienen una declaración de extensión que les permite compartir eventos de contenido e implementación con una aplicación host. Una aplicación de extensión puede proporcionar varias extensiones.

Dado que las extensiones de aplicaciones son solo aplicaciones para UWP, también pueden ser aplicaciones totalmente funcionales, extensiones de host y proporcionar extensiones a otras aplicaciones, todo ello sin tener que crear paquetes de aplicación independientes.

Al crear un host de extensión de aplicación, se crea una oportunidad de desarrollar un ecosistema alrededor de la aplicación en la que otros desarrolladores pueden mejorar tu aplicación de formas que es posible que no esperaras o para las que no tuvieras los recursos. Ten en cuenta las extensiones de Microsoft Office, las extensiones de Visual Studio, las extensiones del explorador, etc. Estas animaciones crean experiencias más enriquecidas para las aplicaciones que van más allá de la funcionalidad con las que se incluyen. Las extensiones pueden agregar valor y longevidad a tu aplicación.

**Introducción**

En un nivel alto, para configurar una relación de extensión de aplicación, tenemos que:

1. Declarar una aplicación para que sea un host de extensión.
2. Declarar una aplicación para que sea una extensión.
3. Decidir si vas a implementar la extensión como servicio de aplicaciones, tarea en segundo plano o alguna otra forma.
4. Definir cómo se comunicarán los hosts y sus extensiones.
5. Usar la API [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions) en la aplicación host para tener acceso a las extensiones.

Veamos cómo se hace esto examinando el [ejemplo de código de extensión de matemáticas](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) que implementa una calculadora hipotética a la que puedes agregar nuevas características mediante extensiones. En Microsoft Visual Studio 2017, carga **MathExtensionSample.sln** del ejemplo de código.

![Ejemplo de código de extensión de matemáticas](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Declarar una aplicación para que sea un host de extensión

Una app se identifica como host de extensión de app declarando el elemento `<AppExtensionHost>` en su archivo Package.appxmanifest. Consulta el archivo **Package.appxmanifest** del proyecto **MathExtensionHost** para ver cómo se hace.

_Package.appxmanifest del proyecto MathExtensionHost_
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

Observe `xmlns:uap3="http://..."` y la presencia de `uap3` en `IgnorableNamespaces`. Son necesarios porque estamos usando el espacio de nombres uap3.

`<uap3:Extension Category="windows.appExtensionHost">` Identifica esta app como host de extensión.

El elemento **Name** de `<uap3:AppExtensionHost>` es el nombre de _contrato de extensión_. Cuando una extensión especifica el mismo nombre de contrato de extensión, el host podrá encontrarlo. Por convención, se recomienda crear el nombre del contrato de extensión con el nombre del editor o la aplicación para evitar posibles conflictos con otros nombres de contrato de extensión.

Puedes definir varios hosts y extensiones en la misma app. En este ejemplo, declaramos un host. La extensión se defe en otra aplicación.

## <a name="declare-an-app-to-be-an-extension"></a>Declarar una aplicación para que sea una extensión

Una app se identifica como extensión de app declarando el elemento `<uap3:AppExtension>` en su archivo **Package.appxmanifest**. Abra el archivo **Package.appxmanifest** del proyecto **MathExtension** para ver cómo se hace.

_Package.appxmanifest del proyecto MathExtension:_
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

De nuevo, observe la línea `xmlns:uap3="http://..."` y la presencia de `uap3` en `IgnorableNamespaces`. Son necesarios porque estamos usando el espacio de nombres `uap3`.

`<uap3:Extension Category="windows.appExtension">` Identifica esta app como extensión.

El significado de los atributos `<uap3:AppExtension>` son los que se muestran a continuación:

|Atributo|Descripción|Requerido|
|---------|-----------|:------:|
|**Nombre**|Este es el nombre de contrato de extensión. Cuando coincida con el **Name** declarado en un host, ese host podrá encontrar esta extensión.| :heavy_check_mark: |
|**Id.**| Identifica esta extensión de manera única. Dado que puede haber varias extensiones que usen el mismo nombre de contrato de extensión (imagina una app de pintura que admite varias extensiones), puedes usar el identificador para diferenciarlas. Los hosts de extensión de aplicación pueden usar el identificador para deducir algo sobre el tipo de extensión. Por ejemplo, podrías tener una extensión diseñada para escritorio y otra para móvil, siendo el identificador el diferenciador. También podrías usar el elemento **Properties** , que se explica a continuación, para ello.| :heavy_check_mark: |
|**DisplayName**| Se puede usar desde la app host para identificar la extensión al usuario. Se puede consultar desde, y puede usar, el [nuevo sistema de administración de recursos](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) para la localización. El contenido localizado se carga desde el paquete de extensión de app, no desde la app host. | |
|**Description** | Se puede usar desde la app host para describir la extensión al usuario. Se puede consultar desde, y puede usar, el [nuevo sistema de administración de recursos](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) para la localización. El contenido localizado se carga desde el paquete de extensión de app, no desde la app host. | |
|**PublicFolder**|El nombre de una carpeta, en relación con la raíz del paquete, donde puedes compartir contenido con el host de extensión. Por convención, el nombre es "Public", pero puedes usar cualquier nombre que coincida con una carpeta de la extensión.| :heavy_check_mark: |

`<uap3:Properties>` es un elemento opcional que contiene metadatos personalizados que los hosts pueden leer en tiempo de ejecución. En el ejemplo de código, la extensión se implementa como servicio de app para que el host necesite una manera de obtener el nombre de dicho servicio de app para que pueda llamarle. El nombre del servicio de la app se define en el elemento <Service>, que hemos definido (podríamos haberlo llamado de cualquier manera que quisiéramos). El host del ejemplo de código busca esta propiedad en tiempo de ejecución para obtener el nombre del servicio de la app.

## <a name="decide-how-you-will-implement-the-extension"></a>Decida cómo implementará la extensión.

La [sesión de la compilación 2016 sobre las extensiones para aplicaciones](https://channel9.msdn.com/Events/Build/2016/B808) demuestra cómo usar la carpeta pública compartida entre el host y las extensiones. En el ejemplo, la extensión se implementa mediante un archivo Javascript que se almacena en la carpeta pública, invocada por el host. Ese enfoque tiene la ventaja de ser ligero, no requiere compilación y puede admitir la creación de la página de aterrizaje predeterminada que proporciona instrucciones para la extensión y un vínculo a la página de Microsoft Store de la app host. Consulta el [ejemplo de código de extensión de app de la compilación 2016](https://github.com/Microsoft/App-Extensibility-Sample) para obtener detalles. De manera específica, consulta el proyecto **InvertImageExtension** y `InvokeLoad()` en ExtensionManager.cs del proyecto **ExtensibilitySample**.

En este ejemplo, usaremos un servicio de app para implementar la extensión. Los servicios de aplicaciones tienen las siguientes ventajas:

- Si la extensión se bloquea, no bloqueará la app host dado que la app host se ejecuta en su propio proceso.
- Puedes usar el idioma de tu elección para implementar el servicio. No tiene que coincidir con el idioma usado para implementar la app host.
- El servicio de aplicaciones tiene acceso a su propio contenedor de aplicaciones, cuyas funcionalidades pueden ser diferentes a las del host.
- No hay aislamiento entre los datos de servicio y la app host.

### <a name="host-app-service-code"></a>Código de servicio de aplicación host

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

Este es el código típico para invocar a un servicio de aplicaciones. Para obtener más información sobre cómo implementar y llamar a un servicio de aplicaciones, consulta [Cómo crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md).

Un aspecto que debemos tener en cuenta es cómo se determina el nombre del servicio de aplicaciones al que se llamará. Dado que el host no tiene información sobre la implementación de la extensión, la extensión debe proporcionar el nombre de su servicio de aplicaciones. En el ejemplo de código, la extensión declara el nombre del servicio de aplicaciones en su archivo del elemento `<uap3:Properties>`:

_Package.appxmanifest del proyecto MathExtension_
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

Puedes definir tu propio XML en el elemento `<uap3:Properties>`. En este caso, definimos el nombre del servicio de aplicaciones para que el host pueda utilizarlo cuando invoque la extensión.

Cuando el host carga una extensión, código como este extrae el nombre del servicio de las propiedades definidas en Package.appxmanifest de la extensión:

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

Con el nombre del servicio de aplicaciones almacenado en `_serviceName`, el host puede usarlo para invocar el servicio de aplicaciones.

Llamar a un servicio de aplicaciones también requiere el nombre de familia del paquete que contiene el servicio de aplicaciones. Afortunadamente, la API de extensión de aplicación proporciona esta información que se obtiene en la línea: `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Definir cómo se comunicará el host y la extensión

Los servicios de aplicaciones usan un elemento [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) para intercambiar información. Como el autor del host, debes presentar un protocolo para la comunicación con extensiones que sea flexible. En el ejemplo de código, eso significa dar cuenta de extensiones que podrían tomar 1, 2 o más argumentos en el futuro.

Para este ejemplo, el protocolo de los argumentos es un **ValueSet** que contiene los pares clave-valor denominado "Arg" + el número de argumento, por ejemplo, `Arg1` y `Arg2`. El host pasa todos los argumentos de **ValueSet** y la extensión usa los que necesita. Si la extensión puede calcular un resultado, el host espera el valor de **ValueSet** devuelto de la extensión para tener una clave denominada `Result` que contiene el valor del cálculo. Si esa clave no está presente, el host da por sentando que la extensión no ha podido completar el cálculo.

### <a name="extension-app-service-code"></a>Código de servicio de aplicaciones de extensión

En el ejemplo de código, el servicio de aplicaciones de la extensión no se implementa como tarea en segundo plano. En su lugar, usa el modelo de servicio de aplicaciones de proceso único en el que se ejecuta el servicio de aplicaciones en el mismo proceso que la aplicación de la extensión que lo hospeda. Este sigue siendo un proceso diferente de la aplicación host y proporciona las ventajas de separación de proceso, al tiempo que obtiene algunas ventajas de rendimiento evitando la comunicación entre procesos entre el proceso de extensión y el proceso en segundo plano que implementa el servicio de aplicaciones. Consulta [Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host](convert-app-service-in-process.md) para ver la diferencia entre un servicio de aplicaciones que se ejecuta como una tarea en segundo plano frente a la que se ejecuta en el mismo proceso.

El sistema permite `OnBackgroundActivate()` cuando se activa el servicio de aplicaciones. Ese código configura controladores de eventos para controlar la llamada al servicio de aplicaciones real cuando aparece (`OnAppServiceRequestReceived()`), así como para tratar los eventos de mantenimiento como la obtención de un objeto de aplazamiento que controla un evento de cancelación o cerrado.

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

El código que realiza el trabajo de la extensión se encuentra en `OnAppServiceRequestReceived()`. A esta función se llama cuando se invoca el servicio de aplicaciones para realizar un cálculo. Extrae los valores que necesita del **ValueSet **. Si puede realizar el cálculo, coloca el resultado debajo de una clave llamada **Result** del **ValueSet** que se devuelve al host. Recuerda que, según el protocolo definido para la manera en que se comunicarán este host y sus extensiones, la presencia de una clave **Result** indicará que la operación se ha realizado correctamente; en caso contrario, será error.

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

## <a name="manage-extensions"></a>Administrar extensiones

Ahora que hemos visto cómo implementar la relación entre un host y sus extensiones, veamos cómo un host encuentra las extensiones instaladas en el sistema y reacciona a la adición y eliminación de paquetes que contienen extensiones.

La Microsoft Store ofrece extensiones como paquetes. El [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) busca paquetes instalados que contienen extensiones que coinciden con el nombre del contrato de extensión del host y proporciona eventos que se desencadenan cuando se instala o se quita un paquete de extensión de aplicación relevante para el host.

En el ejemplo de código, la clase `ExtensionManager` (definida en **ExtensionManager.cs** en el proyecto **MathExtensionHost**) encapsula la lógica para cargar extensiones y responder a instalaciones y desinstalaciones de paquetes de extensión.

El constructor `ExtensionManager` usa `AppExtensionCatalog` para encontrar las extensiones para aplicaciones en el sistema que tienen el mismo nombre de contrato de extensión que el host:

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

Cuando se instala un paquete de extensión, `ExtensionManager` recopila información acerca de las extensiones del paquete que tienen el mismo nombre de contrato de extensión que el host. Una instalación puede representar una actualización en cuyo caso se actualiza la información de la extensión afectada. Al desinstalar un paquete de extensión, `ExtensionManager` quita la información sobre las extensiones afectadas para que el usuario sepa qué extensiones ya no están disponibles.

La clase `Extension` (definida en **ExtensionManager.cs** en el proyecto **MathExtensionHost**) se ha creado para el ejemplo de código para acceder al id. de una extensión, la descripción, el logotipo y la información específica de la app como si el usuario ha habilitado la extensión.

Decir que la extensión está cargada (consulta `Load()` en **ExtensionManager.cs**) significa que el estado del paquete está bien y que hemos obtenido su id., logotipo, descripción y carpeta pública (que no usamos en este ejemplo, solo es para mostrarle cómo obtenerlo). El propio paquete de la extensión no se está cargando.

El concepto de descarga se usa para realizar un seguimiento de qué extensiones no se deben presentar ya al usuario.

El valor de `ExtensionManager` proporciona instancias de `Extension` de una colección para que las extensiones, sus nombres, descripciones y logotipos puedan ser de datos enlazados a la interfaz de usuario. La página **ExtensionsTab** se enlaza a esta colección y proporciona interfaz de usuario para habilitar o deshabilitar extensiones, así como para quitarlas.

![Interfaz de usuario de ejemplo de la pestaña Extensiones](images/mathextensionhost-extensiontab.png)

 Cuando se quita una extensión, el sistema pide al usuario que compruebe que desea desinstalar el paquete que contiene la extensión (y que posiblemente contiene otras extensiones). Si el usuario lo permite, el paquete se desinstala y `ExtensionManager` quita las extensiones del paquete desinstalado de la lista de extensiones disponibles para la aplicación host.

 ![Desinstalar interfaz de usuario](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Depuración de hosts y extensiones de aplicaciones

A menudo, el host de extensión y la extensión no forman parte de la misma solución. En ese caso, para depurar el host y la extensión:

1. Carga el proyecto de host en una instancia de Visual Studio.
2. Carga la extensión en otra instancia de Visual Studio.
3. Inicia la aplicación host en el depurador.
4. Inicia la aplicación de extensión en el depurador. (Si quieres implementar la extensión, en lugar de depurarla, para probar el evento de instalación del paquete del host, realiza **Compilación &gt; Implementar solución**, en su lugar).

Ahora podrás alcanzar puntos de interrupción en el host y la extensión.
Si empiezas a depurar la propia aplicación de la extensión, verás una ventana en blanco de la aplicación. Si no quieres ver la ventana en blanco, puedes cambiar la configuración de depuración para que el proyecto de extensión no inicie la aplicación sino que en su lugar la depure cuando se inicie (haz clic en el proyecto de extensión, en **Propiedades** > **Depurar** > selecciona **No iniciar, pero depurar mi código al empezar**). Todavía tendrás que empezar a depurar (**F5**) el proyecto de extensión, pero esperará hasta que el host active la extensión y, a continuación, se alcanzarán los puntos de interrupción de la extensión.

**Depurar el ejemplo de código**

En el ejemplo de código, el host y la extensión se encuentran en la misma solución. Haz lo siguiente para depurar:

1. Asegúrate de que **MathExtensionHost** es el proyecto de inicio (haz clic con el botón derecho en el proyecto **MathExtensionHost** y haz clic en **Establecer como proyecto de inicio**).
2. Coloque un punto de interrupción en `Invoke` en ExtensionManager.cs, en el proyecto **MathExtensionHost**.
3. **F5** para ejecutar el proyecto **MathExtensionHost**.
4. Coloque un punto de interrupción en `OnAppServiceRequestReceived` en App.xaml.cs, en el proyecto **MathExtension**.
5. Inicia la depuración del proyecto **MathExtension** (haz clic con el botón secundario en el proyecto **MathExtension**, **Depurar > Iniciar nueva instancia**) que lo implementará y desencadenar el evento de instalación del paquete en el host.
6. En la app **MathExtensionHost**, navega a la página **Cálculo** y haz clic en **x^y** para activar la extensión. El punto de interrupción `Invoke()` se alcanza primero y puedes ver la llamada del servicio de aplicaciones que se está realizando. A continuación, se alcanza el método `OnAppServiceRequestReceived()` en la extensión, y puedes ver al servicio de aplicaciones calcular el resultado y devolverlo.

**Solución de problemas de extensiones implementadas como servicio de aplicaciones**

Si tu host de extensión tiene problemas para conectarse al servicio de aplicaciones de la extensión, asegúrate de que el atributo `<uap:AppService Name="...">` coincide con lo que colocas en el elemento `<Service>`. Si no coinciden, el nombre del servicio que tu extensión proporciona al host no coincidirá con el nombre del servicio de aplicaciones que ha implementado y el host no podrá activar la extensión.

_Package.appxmanifest del proyecto MathExtension:_
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

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Lista de comprobación de escenarios básicos que se probarán

Cuando compilas un host de extensión y estás preparado para probar cómo de bien admite extensiones, estos son algunos escenarios básicos que se deben probar:

- Ejecutar el host y, a continuación, implementar una aplicación de extensión  
    - ¿El host recoge nuevas extensiones que se incluyen mientras se ejecutan?  
- Implementa la aplicación de extensión y, a continuación, implementa y ejecuta el host.
    - ¿El host recoge extensiones anteriormente existentes?  
- Ejecuta el host y, a continuación, quita la aplicación de extensión.
    - ¿El host detecta la eliminación correctamente?
- Ejecuta el host y, a continuación, actualiza la aplicación de extensión a una versión más reciente.
    - ¿El host recibe el cambio y descarga las versiones anteriores de la extensión correctamente?  

**Escenarios avanzados que se deben probar:**

- Ejecutar el host, mover la aplicación de extensión a medios extraíbles, quitar los medios
    - ¿El host detecta el cambio de estado del paquete y deshabilita la extensión?
- Ejecuta el host y, a continuación, daña la aplicación de la extensión (conviértela en no válida, con firma diferente, etc.)
    - ¿El host detecta la extensión alterada y la controla correctamente?
- Ejecuta el host y, a continuación, implementa una aplicación de extensión que tiene propiedades o contenido no válidos.
    - ¿El host detecta contenido no válido y lo controla correctamente?

## <a name="design-considerations"></a>Consideraciones de diseño

- Proporciona la interfaz de usuario que muestra al usuario las extensiones que están disponibles y les permite habilitarlas o deshabilitarlas. También puedes pensar en agregar glifos para extensiones que se vuelven no disponibles porque un paquete se desconecta, etc.
- Dirija al usuario a donde pueda obtener extensiones. Quizás tu página de la extensión puede proporcionar una consulta de búsqueda de Microsoft Store que muestra una lista de extensiones que se puede usar con tu aplicación.
- Piensa en cómo notificar al usuario acerca de la adición y eliminación de extensiones. Podrías crear una notificación para cuando se instala una nueva extensión e invitar al usuario a habilitarla. Las extensiones deben estar deshabilitadas de manera predeterminada para que los usuarios tengan el control.

## <a name="how-app-extensions-differ-from-optional-packages"></a>En qué se diferencian las extensiones de aplicaciones de los paquetes opcionales

El diferenciador clave entre los [paquetes opcionales](https://docs.microsoft.com/windows/uwp/packaging/optional-packages) y las extensiones de aplicación son un ecosistema abierto frente a un ecosistema cerrado y un paquete dependiente frente a un paquete independiente.

Las extensiones de aplicaciones participan en un ecosistema abierto. Si tu aplicación puede hospedar para aplicaciones, cualquiera puede escribir una extensión para tu host siempre que cumpla con tu método de pasar o recibir información de la extensión. Esto es diferente de los paquetes opcionales que participan en un ecosistema cerrado cuando el editor decide a quién se le permite crear un paquete opcional que se puede usar con la aplicación.

Las extensiones de aplicaciones son paquetes independientes y pueden ser aplicaciones independientes. No pueden tener una dependencia de implementación en otra aplicación.Los paquetes opcionales requieren el paquete principal y no se pueden ejecutar sin él.

Un paquete de expansión para un juego sería un buen candidato para un paquete opcional porque está estrechamente enlazado al juego, pero no se puede ejecutar independientemente del juego y quizá no quieras que los paquetes de expansión se creen por cualquier desarrollador del ecosistema.

Si ese mismo juego tenía temas o complementos de interfaz de usuario personalizables, una extensión de la aplicación podría ser una buena elección porque la app que proporciona la extensión podría ejecutarse por sí sola y cualquiera tercera parte podría crearla.

## <a name="remarks"></a>Observaciones

En este tema se proporciona una introducción a extensiones de aplicaciones. Los conceptos clave que se deben tener en cuenta son la creación del host y su marcado como tal en su archivo Package.appxmanifest, creando la extensión y marcándola como tal en su archivo Package.appxmanifest, determinando cómo implementar la extensión (por ejemplo, servicio de aplicaciones, tarea en segundo plano u otros medios), definiendo cómo se comunicará el host con las extensiones y usado la [API de AppExtensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) para tener acceso a extensiones y administrarlas.

## <a name="related-topics"></a>Temas relacionados

* [Introducción a las extensiones para aplicaciones](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [Sesión de la compilación 2016 sobre las extensiones para aplicaciones](https://channel9.msdn.com/Events/Build/2016/B808)
* [Ejemplo de código de extensión de aplicación de la compilación 2016](https://github.com/Microsoft/App-Extensibility-Sample)
* [Compatibilidad de la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md)
* [Cómo crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md).
* [Espacio de nombres AppExtensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [Ampliar tu aplicación con servicios, extensiones y paquetes](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)

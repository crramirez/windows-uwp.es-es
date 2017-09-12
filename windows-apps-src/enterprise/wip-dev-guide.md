---
author: normesta
Description: "Esta guía te ayuda a optimizar tu aplicación para controlar los datos de empresa administrados por la directiva de Windows Information Protection (WIP) así como los datos personales."
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Guía para desarrolladores sobre Windows Information Protection (WIP)"
ms.author: normesta
ms.date: 06/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, trabajo en curso, Windows Information Protection, datos empresariales, protección de datos empresariales, edp, aplicaciones habilitadas"
ms.assetid: 913ac957-ea49-43b0-91b3-e0f6ca01ef2c
ms.openlocfilehash: 23604e4ca549bbb11885e681500f4f41531c2b6f
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2017
---
# <a name="windows-information-protection-wip-developer-guide"></a>Guía para desarrolladores sobre Windows Information Protection (WIP)

Una aplicación *habilitada* diferencia los datos corporativos y personales y sabe cuáles tiene que proteger en función de las directivas de Windows Information Protection (WIP) definidas por el administrador.

En esta guía te mostraremos cómo crear una. Una vez finalizada, los administradores de las directivas confiarán en tu aplicación para que pueda consumir datos de su organización. Y los empleados estarán encantados de que sus datos personales se mantengan intactos en sus dispositivos, incluso si dejan de formar parte de la administración de dispositivos móviles (MDM) de la organización o si ellos mismos la abandonan.

__Nota__ Esta guía te ayuda a optimizar una aplicación para UWP. Si quieres optimizar una aplicación de escritorio de Windows en C++, consulta [Guía para desarrolladores sobre Windows Information Protection (WIP)](http://go.microsoft.com/fwlink/?LinkId=822192).

Puedes leer más sobre WIP y las aplicaciones habilitadas aquí: [Windows Information Protection (WIP)](wip-hub.md).

Encontrarás una muestra completa [aquí](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection).

Si estás listo para realizar cada una de las tareas, empecemos.

## <a name="first-gather-what-you-need"></a>En primer lugar, recopila aquello que necesites.

Necesitarás lo siguiente:

* Una máquina Virtual (VM) de prueba que ejecute Windows 10, versión 1607 o posterior. Vas a depurar la aplicación usando esta VM de prueba.

* Un equipo de desarrollo que ejecute Windows 10, versión 1607 o superior. Este podría ser la VM de prueba si tienes Visual Studio instalado.

## <a name="setup-your-development-environment"></a>Configuración de tu entorno de desarrollo

Para ello deberás hacer lo siguiente:

* [Instalar WIP Setup Developer Assistant en la VM de prueba](#install-assistant)

* [Crear una directiva de protección mediante WIP Setup Developer Assistant](#create-protection-policy)

* [Configurar un proyecto de Visual Studio](#setup-vs-project)

* [Configurar la depuración remota](#setup-remote-debugging)

* [Agregar espacios de nombres a los archivos de código](#add-namespaces)

<span id="install-assistant" />
### <a name="install-the-wip-setup-developer-assistant-onto-your-test-vm"></a>Instalar WIP Setup Developer Assistant en la VM de prueba

 Usa esta herramienta para configurar una directiva de Windows Information Protection en la VM de prueba.

 Descarga la herramienta aquí: [WIP Setup Developer Assistant](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf).
<span id="create-protection-policy" />
### <a name="create-a-protection-policy"></a>Crear una directiva de protección

Para definir la directiva, agrega información a cada sección del asistente del desarrollador para la configuración de WIP. Elige el icono de ayuda junto a cualquier configuración para obtener más información sobre cómo usarla.

Para obtener instrucciones más generales sobre cómo usar esta herramienta, consulta la sección de notas de la versión en la página de descarga de la aplicación.
<span id="setup-vs-project" />
### <a name="setup-a-visual-studio-project"></a>Configuración de un proyecto de Visual Studio

1. Abre el proyecto en tu equipo de desarrollo.

2. Agrega una referencia a las extensiones móviles y de escritorio para la Plataforma universal de Windows (UWP).

    ![Agregar extensiones de UWP](images/extensions.png)

3. Agregar esta funcionalidad al archivo de manifiesto del paquete:

    ```xml
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*Lectura opcional*: El prefijo "rescap" significa *Funcionalidad restringida*. Consulta [Funcionalidades especiales y restringidas](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).

4. Agrega este espacio de nombres al archivo de manifiesto del paquete:

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. Agrega el prefijo de espacio de nombres al elemento ``<ignorableNamespaces>`` de tu archivo de manifiesto del paquete.

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    De esta forma, si la aplicación se ejecuta en una versión del sistema operativo Windows que no sea compatible con las funcionalidades restringidas, Windows ignorará la funcionalidad ``enterpriseDataPolicy``.
<span id="setup-remote-debugging" />
### <a name="setup-remote-debugging"></a>Configurar la depuración remota

Instala Herramientas remotas para Visual Studio en la VM de prueba solo si vas a desarrollar tu aplicación en un equipo que no sea la VM. A continuación, inicia el depurador remoto en el equipo de desarrollo y comprueba si tu aplicación se ejecuta en la VM de prueba.

Consulta [Instrucciones del equipo remoto](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#remote-pc-instructions).
<span id="add-namespaces" />
### <a name="add-these-namespaces-to-your-code-files"></a>Agregar estos espacios de nombres a los archivos de código

Agrégalos usando estas instrucciones en la parte superior de los archivos de código (los fragmentos de código utilizados en esta guía):

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml;
using Windows.ApplicationModel.Activation;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Data.Xml.Dom;
using Windows.Foundation.Metadata;
using Windows.Web.Http.Headers;
```

## <a name="determine-whether-to-use-wip-apis-in-your-app"></a>Determinar si usar API WIP en tu aplicación

Asegurarte de que el sistema operativo que ejecuta tu aplicación admite WIP y que WIP está habilitado en el dispositivo.

```csharp
bool use_WIP_APIs = false;

if ((ApiInformation.IsApiContractPresent
    ("Windows.Security.EnterpriseData.EnterpriseDataContract", 3)
    && ProtectionPolicyManager.IsProtectionEnabled))
{
    use_WIP_APIs = true;
}
else
{
    use_WIP_APIs = false;
}
```
No llamar a las API WIP si el sistema operativo no admite WIP o WIP no está habilitado en el dispositivo.

## <a name="read-enterprise-data"></a>Leer datos de empresa

Para leer los archivos protegidos, puntos de conexión de red, datos del portapapeles y datos que puede aceptar de un contrato de uso compartido, tu aplicación tendrá que pedir acceso.

Windows Information Protection da a tu aplicación permiso si esta se encuentra en la lista de aplicaciones permitidas de la directiva de protección.

**En esta sección:**

* [Leer datos desde un archivo](#read-file)
* [Leer los datos desde el punto de conexión de red](#read-network)
* [Leer datos desde el Portapapeles](#read-clipboard)
* [Leer datos desde un contrato para contenido compartido](#read-contract)

<span id="read-file" />
### <a name="read-data-from-a-file"></a>Leer datos desde un archivo

**Paso 1: Obtener el identificador de archivo**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**Paso 2: Determinar si la aplicación puede abrir el archivo**

Llama a [FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx) para determinar si la aplicación puede abrir el archivo.

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if ((protectionInfo.Status != FileProtectionStatus.Protected &&
    protectionInfo.Status != FileProtectionStatus.Unprotected))
{
    return false;
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```

Un valor [FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx) de **Protected** significa que el archivo está protegido y que la aplicación puede abrirlo porque se encuentra en la lista de aplicaciones permitidas de la directiva.

Un valor [FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx) de **desprotegido** significa que el archivo no está protegido y que la aplicación puede abrirlo incluso si no figura en la lista de aplicaciones permitidas de la directiva.

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx)<br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)

**Paso 3: Leer el archivo en una secuencia o búfer**

*Leer el archivo en una secuencia*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*Leer el archivo en un búfer*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```
<span id="read-network" />
### <a name="read-data-from-a-network-endpoint"></a>Leer los datos desde el punto de conexión de red

Crea un contexto de subproceso protegido para leer desde un punto de conexión de empresa.

**Paso 1: Obtener la identidad del punto de conexión de red**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

Si el punto de conexión no está administrado por la directiva, obtendrás una cadena vacía.

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)


**Paso 2: Crear un contexto de subproceso protegido**

Si el punto de conexión está administrado por una directiva, crea un contexto de subproceso protegido. Esto etiqueta todas las conexiones de red que se realizan en el mismo subproceso a dicha identidad.

También proporciona acceso a los recursos de red de empresa que administra esa directiva.

```csharp
if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
    }
}
else
{
    return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
}
```
Este ejemplo incluye llamadas de socket en un bloque ``using``. Si no haces esto, asegúrate de cerrar el contexto de subproceso tras haber recuperado los recursos. Consulta [ThreadNetworkContext.Close](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.threadnetworkcontext.close.aspx).

No crees ningún archivo personal en ese subproceso protegido porque los archivos se cifrarán automáticamente.

El método [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx) devuelve un objeto [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.threadnetworkcontext.aspx) independientemente de si el punto de conexión está administrado por una directiva. Si la aplicación controla recursos personales y de empresa, llama a [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx) para todas las identidades.  Una vez obtenido el recurso, eliminar el ThreadNetworkContext para borrar cualquier etiqueta de identidad del subproceso actual.

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)

**Paso 3: Leer el recurso en un búfer**

```csharp
private static async Task<IBuffer> GetDataFromNetworkHelperMethod(Uri resourceURI)
{
    HttpClient client;

    client = new HttpClient();

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**(Opcional) Usa un token de encabezado en lugar de crear un contexto de subproceso protegido**

```csharp
public static async Task<IBuffer> GetDataFromNetworkbyUsingHeader(Uri resourceURI)
{
    HttpClient client;

    Windows.Networking.HostName hostName =
        new Windows.Networking.HostName(resourceURI.Host);

    string identity = await ProtectionPolicyManager.
        GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

    if (!string.IsNullOrEmpty(identity))
    {
        client = new HttpClient();

        HttpRequestHeaderCollection headerCollection = client.DefaultRequestHeaders;

        headerCollection.Add("X-MS-Windows-HttpClient-EnterpriseId", identity);

        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }
    else
    {
        client = new HttpClient();
        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }

}

private static async Task<IBuffer> GetDataFromNetworkbyUsingHeaderHelperMethod(HttpClient client, Uri resourceURI)
{

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**Controlar las redirecciones de página**

En ocasiones, un servidor web redirigirá el tráfico hacia una versión más actual de un recurso.

Para controlar esto, realiza solicitudes hasta que el estado de la respuesta de la solicitud tiene el valor **Aceptar**.

A continuación, usa el URI de la respuesta para obtener la identidad del punto de conexión. Esta es una forma de hacerlo:

```csharp
private static async Task<IBuffer> GetDataFromNetworkRedirectHelperMethod(Uri resourceURI)
{
    HttpClient client = null;

    HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
    filter.AllowAutoRedirect = false;

    client = new HttpClient(filter);

    HttpResponseMessage response = null;

        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

    if (response.StatusCode == HttpStatusCode.MultipleChoices ||
        response.StatusCode == HttpStatusCode.MovedPermanently ||
        response.StatusCode == HttpStatusCode.Found ||
        response.StatusCode == HttpStatusCode.SeeOther ||
        response.StatusCode == HttpStatusCode.NotModified ||
        response.StatusCode == HttpStatusCode.UseProxy ||
        response.StatusCode == HttpStatusCode.TemporaryRedirect ||
        response.StatusCode == HttpStatusCode.PermanentRedirect)
    {
        message = new HttpRequestMessage(HttpMethod.Get, message.RequestUri);
        response = await client.SendRequestAsync(message);

        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
    else
    {
        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
}

```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

<span id="read-clipboard" />
### <a name="read-data-from-the-clipboard"></a>Leer datos desde el Portapapeles

**Obtener permiso para usar los datos del Portapapeles**

Para obtener datos del Portapapeles, tendrás que pedir permiso a Windows. Usa [**DataPackageView.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx) para ello.

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **API** <br>
[DataPackageView.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx)

**Ocultar o deshabilitar las características que usan datos del Portapapeles**

Determinar si la vista actual tiene permiso para obtener los datos que están en el Portapapeles.

Si no es así, puedes deshabilitar u ocultar los controles que permiten a los usuarios pegar información desde el Portapapeles u obtener una vista previa de su contenido.

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **API** <br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

**Impedir a los usuarios que les aparezca un cuadro de diálogo de consentimiento**

Un documento nuevo no es *personal* o *de empresa*. Es simplemente nuevo. Si un usuario pega los datos de empresa en él, Windows aplicará la directiva y se le mostrará al usuario un cuadro de diálogo de consentimiento. Este código impide que eso ocurra. En esta tarea no se trata de proteger los datos. Es más sobre cómo hacer que los usuarios reciban el cuadro de diálogo de consentimiento en los casos donde la aplicación crea un elemento nuevo.

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **API** <br>
[DataPackageView.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx)<br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

<span id="read-share" />
### <a name="read-data-from-a-share-contract"></a>Leer datos desde un contrato para contenido compartido

Cuando los empleados elijan tu aplicación para compartir su información, tu aplicación abrirá un nuevo elemento con ese contenido.

Como ya hemos mencionado, un elemento nuevo no es ni *personal* ni *de empresa*. Es simplemente nuevo. Si tu código agrega contenido de empresa a dicho elemento, Windows aplicará la directiva y se le mostrará al usuario un cuadro de diálogo de consentimiento. Este código impide que eso ocurra.

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **API** <br>
[ProtectionPolicyManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn705789.aspx)<br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

## <a name="protect-enterprise-data"></a>Protección de datos de empresa

Protege los datos de empresa que se usan fuera de la aplicación. Los datos se usan fuera de la aplicación cuando se muestran en una página, se guardan en un archivo o punto de conexión de red o a través de un contrato para contenido compartido.

**En esta sección:**

* [Proteger los datos que aparecen en las páginas](#protect-pages)
* [Proteger datos en un archivo como un proceso en segundo plano](#protect-background)
* [Proteger parte de un archivo](#protect-part-file)
* [Leer la parte protegida de un archivo](#read-protected)
* [Proteger datos en una carpeta](#protect-folder)
* [Proteger los datos en un punto de conexión de red](#protect-network)
* [Proteger los datos que tu aplicación comparte a través de un contrato para contenido compartido](#protect-share)
* [Proteger los archivos que copies en otra ubicación](#protect-other-location)
* [Proteger los datos de empresa cuando la pantalla del dispositivo esté bloqueada](#protect-locked)

<span id="protect-pages" />
### <a name="protect-data-that-appears-in-pages"></a>Proteger los datos que aparecen en las páginas

Al mostrar datos en una página, tendrás que informar a Windows de qué tipo de datos se tratan (personal o de empresa). Para ello, *etiqueta* la vista actual de la aplicación o todo su proceso.

Al etiquetar la vista o el proceso, Windows aplica la directiva. Esto evita que se produzcan fugas de datos como resultado de las acciones que la aplicación no controla. Por ejemplo, en un equipo, un usuario podría usar CTRL+V para copiar la información de empresa desde una vista y, a continuación, pegar esa información en otra aplicación. Windows evita que esto pase. Windows también ayuda a aplicar los contratos para contenido compartido.

**Etiquetar la vista actual de la aplicación**

Realiza esta acción si tu aplicación tiene varias vistas y algunas usarán datos de empresa y otras datos personales.

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty();
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

**Etiquetar el proceso**

Realiza esta acción si todas las vistas de tu aplicación usan solo un tipo de datos (personales o de empresa).

Esto evitará tener que administrar las vistas etiquetadas de forma independiente.

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(String.Empty());
```

> **API** <br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

<span id="protect-file" />
### <a name="protect-data-to-a-file"></a>Proteger datos en un archivo

Crea un archivo protegido y escribe en él.

**Paso 1: Determinar si tu aplicación puede crear un archivo de empresa**

Tu aplicación puede crear un archivo de empresa si la cadena de identidad está administrada por la directiva y tu aplicación está en la lista de aplicaciones permitidas por esta.

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)


**Paso 2: Crear un archivo y protegerlo a la identidad**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **API** <br>
[FileProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.protectasync.aspx)

**Paso 3: Escribir esa secuencia o búfer en el archivo**

*Escribir una secuencia*

```csharp
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*Escribir un búfer*

```csharp
     if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **API** <br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>

<span id="protect-background" />
### <a name="protect-data-to-a-file-as-a-background-process"></a>Proteger datos en un archivo como un proceso en segundo plano

Este código puede ejecutarse mientras la pantalla del dispositivo está bloqueada. Si el administrador ha configurado una directiva de "protección de datos con la pantalla bloqueada (DPL)", Windows eliminará las claves de cifrado necesarias para acceder a los recursos protegido desde la memoria del dispositivo. Esto impide la pérdida de datos en caso de pérdida del dispositivo. La misma función también quita las claves asociadas con los archivos protegidos cuando se cierran sus identificadores.

Al crear un archivo, tendrás que usar un enfoque que mantiene el identificador de archivo abierto.  

**Paso 1: Determinar si puedes crear un archivo de empresa**

Puedes crear un archivo de empresa si la cadena de identidad que estás usando está administrada por la directiva y tu aplicación está en la lista de aplicaciones permitidas por esta.

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)

**Paso 2: Crear un archivo y protegerlo a la identidad**

El método [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync.aspx) crea un archivo protegido y mantiene el identificador de archivo abierto mientras escribes en él.

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **API** <br>
[FileProtectionManager.CreateProtectedAndOpenAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync.aspx)

**Paso 3: Escribir esa secuencia o búfer en el archivo**

En este ejemplo se escribe una secuencia en un archivo.

```csharp
if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **API** <br>
[ProtectedFileCreateResult.ProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>
[ProtectedFileCreateResult.Stream](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedfilecreateresult.stream.aspx)<br>

<span id="protect-part-file" />
### <a name="protect-part-of-a-file"></a>Proteger parte de un archivo

En la mayoría de los casos, es más limpio almacenar los datos personales y de empresa por separado, pero puedes almacenarlos en el mismo archivo si lo deseas. Por ejemplo, Microsoft Outlook puede almacenar correos electrónicos de la empresa junto a correos electrónicos personales en un archivo de almacenamiento único.

Cifrar los datos de empresa pero no el archivo completo. De este modo, los usuarios pueden seguir usando ese archivo incluso si se cancela la inscripción a MDM o se revocan sus derechos de acceso a los datos de empresa. Además, la aplicación deberá llevar un seguimiento de los datos que cifra para que sepa qué datos tiene que proteger cuando el archivo se lee de nuevo en la memoria.

**Paso 1: Agregar los datos de empresa a una secuencia cifrada o búfer**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **API** <br>
[DataProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.protectasync.aspx)<br>
[BufferProtectUnprotectResult.buffer](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.buffer.aspx)


**Paso 2: Agregar datos personales a una secuencia o búfer sin cifrar**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**Paso 3: Escribir secuencias o búferes en un archivo**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**Paso 4: Realizar un seguimiento de la ubicación de los datos de empresa en un archivo**

Es responsabilidad de la aplicación llevar un seguimiento de los datos del archivo que pertenecen a la empresa.

Puedes almacenar dicha información en una propiedad asociada al archivo, en una base de datos o en el texto del encabezado del archivo.

Este ejemplo, guarda esta información en un archivo XML independiente.

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```
<span id="read-protected" />
### <a name="read-the-protected-part-of-a-file"></a>Leer la parte protegida de un archivo

Aquí te mostramos cómo podrías leer los datos de empresa de ese archivo.

**Paso 1: Obtener la posición de los datos de empresa en el archivo**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**Paso 2: Abre el archivo de datos y asegúrate de que no está protegido**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx)<br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>

**Paso 3: Leer los datos de empresa desde el archivo**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**Paso 4: Descifrar el búfer que contiene los datos de empresa**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **API** <br>
[DataProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectioninfo.aspx)<br>
[DataProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync.aspx)<br>

<span id="protect-folder" />
### <a name="protect-data-to-a-folder"></a>Proteger datos en una carpeta

Puedes crear una carpeta y protegerla. De esta forma cualquier elemento que añadas a la carpeta estará automáticamente protegido.

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

Asegúrate de que la carpeta esté vacía antes de protegerla. No puedes proteger una carpeta que ya contiene elementos.

> **API** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)<br>
[FileProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.protectasync.aspx)<br>
[FileProtectionInfo.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.identity.aspx)<br>
[FileProtectionInfo.Status](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.status.aspx)

<span id="protect-network" />
### <a name="protect-data-to-a-network-end-point"></a>Proteger los datos en un punto de conexión de red

Crea un contexto de subproceso protegido para enviar datos a un punto de conexión de empresa.  

**Paso 1: Obtener la identidad del punto de conexión de red**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)

**Paso 2: Crear un contexto de subproceso protegido y enviar datos al punto de conexión de red**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)

<span id="protect-share" />
### <a name="protect-data-that-your-app-shares-through-a-share-contract"></a>Proteger los datos que tu aplicación comparte a través de un contrato para contenido compartido

Si quieres que los usuarios compartan contenido desde tu aplicación, tendrás que implementar un contrato para contenido compartido y controlar el evento [**DataTransferManager.DataRequested**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested).

En el controlador de eventos, establece el contexto de identidad de la empresa en el paquete de datos.

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

<span id="protect-other-location" />
### <a name="protect-files-that-you-copy-to-another-location"></a>Proteger los archivos que copies en otra ubicación

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **API** <br>
[FileProtectionManager.CopyProtectionAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync.aspx)<br>

<span id="protect-locked" />
### <a name="protect-enterprise-data-when-the-screen-of-the-device-is-locked"></a>Proteger los datos de empresa cuando la pantalla del dispositivo esté bloqueada

Elimina todos los datos confidenciales de la memoria si el dispositivo está bloqueado. Una vez que el usuario desbloquee el dispositivo, la aplicación podrá volver a agregar esos datos de forma segura.

Controla el evento [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending.aspx) para que tu aplicación sepa cuando la pantalla está bloqueada. Este evento se genera solo si el administrador configura una directiva de protección de datos con la pantalla bloqueada. Windows elimina temporalmente las claves de protección de datos que se han aprovisionado en el dispositivo. Windows elimina estas claves para garantizar que no hay ningún acceso no autorizado a los datos cifrados mientras el dispositivo esté bloqueado y posiblemente no esté con su propietario.  

Controla el evento [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx) para que tu aplicación sepa cuando la pantalla está desbloqueada. Este evento se genera independientemente de si el administrador configura una directiva de protección de datos con la pantalla bloqueada.

#### <a name="remove-sensitive-data-in-memory-when-the-screen-is-locked"></a>Eliminar datos confidenciales de la memoria cuando la pantalla esté bloqueada

Protege la información confidencial y cierra cualquier flujo de archivo que tu aplicación haya abierto en los archivos protegidos para garantizar que el sistema no almacene en caché ningún dato confidencial de la memoria.

Este ejemplo guarda el contenido de un bloque de texto en un búfer cifrado y elimina el contenido de dicho bloque de texto.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessSuspending](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)</br>
[DataProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.protectasync.aspx)<br>
[BufferProtectUnprotectResult.buffer](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.buffer.aspx)<br>
[ProtectedAccessSuspendingEventArgs.GetDeferral](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral.aspx)<br>
[Deferral.Complete](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx)<br>

#### <a name="add-back-sensitive-data-when-the-device-is-unlocked"></a>Agregar de nuevo datos confidenciales cuando el dispositivo está desbloqueado

[**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx) se genera una vez que las claves vuelven a estar disponibles en el momento del desbloqueo del dispositivo.

[**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedaccessresumedeventargs.identities.aspx) es una colección vacía si el administrador no ha configurado una directiva de protección de datos con la pantalla bloqueada.

En este ejemplo se hace lo contrario que en el ejemplo anterior. Descifra el búfer, agrega de nuevo información de ese búfer al cuadro de texto y, a continuación, elimina el búfer.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessResumed](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)</br>
[DataProtectionManager.UnprotectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.unprotectasync.aspx)<br>
[BufferProtectUnprotectResult.Status](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.aspx)<br>

## <a name="handle-enterprise-data-when-protected-content-is-revoked"></a>Administrar datos de empresa cuando se revoca el contenido protegido

Si quieres que la aplicación reciba notificaciones cuando se haya anulado la inscripción de MDM del dispositivo o cuando el administrador de directivas revoque de manera explícita el acceso a los datos de empresa, controla el evento [**ProtectionPolicyManager_ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked.aspx).

Este ejemplo determina si se han revocado los datos en el buzón de la empresa para una aplicación de correo electrónico.

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **API** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked.aspx)<br>

## <a name="related-topics"></a>Temas relacionados

[Muestra de Windows Information Protection (WIP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

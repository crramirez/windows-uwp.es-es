---
author: DelfCo
Description: "En este tema se muestra cómo crear un contexto de subproceso protegido antes de crear conexiones de red en un escenario de Enterprise Data Protection (EDP)."
MS-HAID: dev\_networking.tagging\_network\_connections\_with\_edp\_identity
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Etiquetado de conexiones de red con identidad EDP
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2b960bbb5cf58991778e5c20bb915a202ecf6e04

---

# Etiquetado de conexiones de red con identidad EDP

__Nota__ La directiva de Protección de datos de empresa (EDP) no se puede aplicar en la versión 1511 de Windows 10 (compilación 10586) o en una versión anterior.

En este tema se muestra cómo crear un contexto de subproceso protegido antes de crear conexiones de red en un escenario de Enterprise Data Protection (EDP). Para obtener una perspectiva de desarrollador completa sobre cómo se relaciona EDP con los archivos, las secuencias, el portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada, consulta [Enterprise Data Protection (EDP) (Protección de datos de empresa [EDP])](../enterprise/edp-hub.md).

**Nota** La [muestra de Protección de datos de empresa (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) abarca numerosos escenarios de EDP.



## Requisitos previos


-   **Prepararse para EDP**

    Consulta [Configurar el equipo para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Confirmar la compilación de una aplicación habilitada para empresas**

    Una aplicación que garantiza de forma independiente que los datos de la empresa se mantienen bajo el control administrativo de la empresa se conoce como una aplicación habilitada para empresas. Una aplicación habilitada es más eficaz, inteligente, flexible y confiable que una que no lo está. La aplicación indica al sistema que está habilitada al declarar la funcionalidad **enterpriseDataPolicy** restringida. Aun así, además de configurar una funcionalidad, hay que realizar otras operaciones para habilitar la aplicación. Para obtener más información, consulta [Aplicaciones habilitadas para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Para aprender a escribir aplicaciones asincrónicas en C\# o Visual Basic, consulta [Llamar a API asincrónicas en C\# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Asynchronous programming in C++ (Programación asincrónica en C++)](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Acceso a recursos de empresa en la red


En este escenario, una aplicación de correo mejorada sincroniza un conjunto de buzones que son una mezcla de buzones de empresa y personales. La aplicación pasa la identidad del usuario a una llamada a [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) para crear un contexto de subproceso protegido. Esto etiqueta todas las conexiones de red que se realizan posteriormente en el mismo subproceso a dicha identidad y permite el acceso controlado a recursos de red de empresa por parte de la directiva de la empresa.

En este caso, "la empresa" se refiere a la empresa a la que pertenece la identidad del usuario. [
              **CreateCurrentThreadNetworkContext**
            ](https://msdn.microsoft.com/library/windows/apps/dn706025) devuelve un objeto [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029) independientemente de la aplicación de directivas. Por lo general, si la aplicación espera controlar los recursos combinados, puede llamar a **CreateCurrentThreadNetworkContext** para todas las identidades. Después de recuperar recursos de red, la aplicación llama a **Desechar** en el **ThreadNetworkContext** para borrar cualquier etiqueta de identidad del subproceso actual. El patrón que uses para desechar el objeto de contexto dependerá del lenguaje de programación.

Si la identidad es desconocida, la aplicación puede consultar la identidad administrada de directiva de empresa de la dirección de red del recurso con la API [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027).

**Nota** Como puedes ver en el ejemplo de código, el patrón de uso correcto para [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) es reducir al mínimo el ámbito en el que está activado. Debes establecer el contexto de la red de empresa, crear y usar conexiones de red, y luego cerrar revertir el contexto, usar las conexiones y cerrarlas. El siguiente ejemplo de código ilustra los detalles. Es importante no crear archivos en un subproceso mientras se establece el contexto de la red de empresa en ese subproceso. Si lo haces, el archivo se cifrará de forma automática, independientemente de si tu intención es para que el archivo sea personal. Esta es una de las razones por las que se recomienda revertir el contexto lo antes posible.



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```

**Nota** Este artículo está orientado a desarrolladores de Windows 10 que escriben aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Temas relacionados


[enterprise data protection (EDP) sample (Muestra de Protección de datos de empresa [EDP])](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espacio de nombres Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->



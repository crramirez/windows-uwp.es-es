---
author: DelfCo
description: "Recupera o crea el contenido web más reciente y popular usando fuentes sindicadas generadas a partir de los estándares de RSS y Atom mediante características del espacio de nombres Windows.Web.Syndication."
title: Fuentes RSS y Atom
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: dff718fe6ea3516730079c7533b6a868b8f67280
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="rssatom-feeds"></a>Fuentes RSS y Atom

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)
-   [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)
-   [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)

Recupera o crea el contenido web más reciente o popular usando fuentes sindicadas generadas a partir de los estándares RSS y Atom mediante características del espacio de nombres [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632).

## <a name="what-is-a-feed"></a>¿Qué es una fuente?

Una fuente web es un documento que contiene cualquier cantidad de entradas individuales compuestas de texto, vínculos e imágenes. Las actualizaciones que se hacen a una fuente se realizan en forma de entradas nuevas para promover el contenido más actual en toda la Web. Los consumidores de contenido pueden usar una aplicación de lector de fuentes para agregar y supervisar fuentes de distintos autores de contenido individuales y obtener acceso, así, al contenido más actual de forma rápida y cómoda.

## <a name="which-feed-format-standards-are-supported"></a>¿Qué estándares de formato de fuentes se admiten?

La Plataforma universal de Windows (UWP) admite la recuperación de fuentes para los estándares de formato RSS de 0.91 a RSS 2.0 y los estándares de Atom de 0.3 a 1.0. Las clases del espacio de nombres [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) pueden definir fuentes y elementos de fuente capaces de representar elementos tanto de RSS como de Atom.

Además, los formatos de Atom 1.0 y RSS 2.0 permiten que los documentos de fuentes contengan elementos o atributos no definidos en las especificaciones oficiales. Con el tiempo, estos atributos y elementos personalizados han pasado a ser una forma de definir información específica de dominio consumida por otros formatos de datos de servicios web como GData y OData. Para admitir esta característica agregada, la clase [**SyndicationNode**](https://msdn.microsoft.com/library/windows/apps/br243585) representa elementos XML genéricos. Si se usa **SyndicationNode** con las clases del espacio de nombres [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819), las aplicaciones podrán obtener acceso a atributos, extensiones y otro contenido que incluyan.

Ten en cuenta que para la publicación de contenido sindicado, la implementación de UWP del protocolo de publicación Atom ([**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)) solo admite operaciones de contenido de fuentes según los estándares de Atom y Atom Publication.

## <a name="using-syndicated-content-with-network-isolation"></a>Uso de contenido sindicado con aislamiento de red

La característica de aislamiento de red de UWP permite al desarrollador controlar y limitar el acceso a la red de una aplicación para UWP. No todas las aplicaciones necesitarán tener acceso a la red. No obstante, para las aplicaciones que sí deben obtener acceso, UWP proporciona distintos niveles de acceso a la red que pueden habilitarse seleccionando las funcionalidades que correspondan.

El aislamiento de red permite al desarrollador definir el ámbito de acceso a la red requerido para cada aplicación. Si una aplicación no tiene definido el ámbito de acceso apropiado, no podrá acceder al tipo especificado de red ni al tipo específico de solicitud de red (las solicitudes salientes iniciadas por el cliente o ambas, las solicitudes entrantes no solicitadas y las solicitudes salientes iniciadas por el cliente). La capacidad de establecer y exigir el aislamiento de red garantiza que si una aplicación se compromete, solo puede obtener acceso a las redes a las que se le haya concedido acceso de forma explícita. Esto reduce significativamente el ámbito del impacto en otras aplicaciones y en Windows.

El aislamiento de red afecta a todos los elementos de clase en los espacios de nombres [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) y [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609) que intentan obtener acceso a la red. Windows aplica de manera activa el aislamiento de red. Si no se habilita la funcionalidad de red que corresponde, una llamada a un elemento de clase de los espacios de nombres **Windows.Web.Syndication** o **Windows.Web.AtomPub** que implique el acceso a la red puede presentar errores debido al aislamiento de red.

Las funcionalidades de red para una aplicación se configuran en su manifiesto al crear la aplicación. Las funcionalidades de red suelen agregarse con Microsoft Visual Studio 2015 cuando se desarrolla la aplicación. Las funcionalidades de red también pueden establecerse de forma manual en el archivo de manifiesto de la aplicación mediante el uso de un editor de texto.

Para obtener más información sobre las funcionalidades de red y el aislamiento de red, consulta la sección "Funcionalidades" del tema [Conceptos básicos de redes](networking-basics.md).

## <a name="how-to-access-a-web-feed"></a>Procedimiento para obtener acceso a una fuente web

En esta sección se muestra cómo recuperar y mostrar una fuente web mediante clases del espacio de nombres [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) en la aplicación para UWP escrita en C# o Javascript.

**Requisitos previos**

Para asegurarte de que la aplicación para UWP está lista para la red, debes establecer las funcionalidades de red necesarias en el archivo **Package.appxmanifest** del proyecto. Si la aplicación necesita conectarse a servicios remotos de Internet como cliente, se necesitará la funcionalidad **internetClient**. Para obtener más información, consulta la sección "Funcionalidades" del tema [Conceptos básicos de redes](networking-basics.md).

**Recuperación del contenido sindicado de una fuente web**

Ahora revisaremos el código que demuestra cómo recuperar una fuente y luego mostraremos cada elemento en particular de la fuente. Para poder configurar y enviar la solicitud, definiremos algunas variables que usaremos durante la operación e inicializaremos una instancia de [**SyndicationClient**](https://msdn.microsoft.com/library/windows/apps/br243456), que define los métodos y las propiedades para recuperar y mostrar la fuente.

El constructor [**URI**](https://msdn.microsoft.com/library/windows/apps/br226017) inicia una excepción si el valor *uriString* que se pasó al constructor no es un URI válido. Así que validamos *uriString* mediante un bloque try/catch.

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;
// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;
// Use your own uriString for the feed you are connecting to.
string uriString = "";
try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
var client = new Windows.Web.Syndication.SyndicationClient();
// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

Después, para configurar la solicitud definimos las credenciales del servidor (la propiedad [**ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461)), las credenciales de proxy (la propiedad [**ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459)) y los encabezados HTTP (el método [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br243462)) necesarios. Con los parámetros de solicitud básicos configurados, creamos un objeto [**URI**](https://msdn.microsoft.com/library/windows/apps/br226017) válido mediante una cadena de URI de fuente proporcionada por la aplicación. Después, se pasa el objeto **URI** a la función [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) para solicitar la fuente.

Suponiendo que se devolvió el contenido deseado de la fuente, el código de ejemplo itera en cada elemento llamando a **displayCurrentItem** (definido a continuación) para mostrar elementos y su contenido en una lista a través de la interfaz de usuario.

Debes escribir código para controlar las excepciones cuando llamas a la mayoría de los métodos de red asincrónicos. Tu controlador de excepciones puede recuperar información más detallada sobre la causa de la excepción para comprender mejor el error y tomar las decisiones adecuadas.

El método [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) inicia una excepción si no se puede establecer una conexión con el servidor HTTP o si el objeto [**URI**](https://msdn.microsoft.com/library/windows/apps/br226017) no señala a una fuente AtomPub o RSS válida. En el código de muestra en Javascript se usa una función **onError** para captar las excepciones e imprimir información más detallada sobre la excepción si se produce un error.

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    feed = await client.RetrieveFeedAsync(uri);
    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;
    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");
    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}
// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;
        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");
        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;
        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }
        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

En el paso anterior, [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) devolvió el contenido de la fuente solicitada y el código de ejemplo tuvo que procesar iteraciones en los elementos disponibles de la fuente. Cada uno de estos elementos se representa con un objeto [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) que contiene todo el contenido y las propiedades de los elementos contemplados en el estándar de redifusión web correspondiente (RSS o Atom). En el siguiente ejemplo, vemos la función **displayCurrentItem** trabajando en cada elemento y mostrando su contenido mediante distintos elementos denominados de la interfaz de usuario.

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.
```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];
    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;
    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;
    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }
    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;
    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

Como se sugirió anteriormente, el tipo de contenido representado por un objeto [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) diferirá según el estándar de fuente (RSS o Atom) empleado para publicar la fuente. Por ejemplo, una fuente Atom puede proporcionar una lista de [**Contributors**](https://msdn.microsoft.com/library/windows/apps/br243540), mientras que la fuente RSS no puede hacerlo. Sin embargo, se puede obtener acceso a los elementos de extensión de la fuente que no sean compatibles con ninguno de los estándares (por ejemplo, elementos de extensión Dublin Core), por medio de la propiedad [**SyndicationItem.ElementExtensions**](https://msdn.microsoft.com/library/windows/apps/br243543). Después se pueden mostrar como en el siguiente código de ejemplo.

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";
    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;
        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }
    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }
    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource
    });
}
```

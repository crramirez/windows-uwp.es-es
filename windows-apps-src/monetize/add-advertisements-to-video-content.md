---
author: mcleanbyron
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: "Aprende a usar la clase AdScheduler para agregar anuncios a contenido de vídeo."
title: "Agregar anuncios a contenido de vídeo en HTML 5 y JavaScript"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, anuncios, publicidad, vídeo, programador, JavaScript"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b42b57f385857301bb74037dbb5c0c7200653316
ms.lasthandoff: 02/07/2017

---

# <a name="add-advertisements-to-video-content-in-html-5-and-javascript"></a>Agregar anuncios a contenido de vídeo en HTML 5 y JavaScript


Este tutorial muestra cómo usar la clase [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) para agregar anuncios a contenido de vídeo en una aplicación para la plataforma Universal de Windows (UWP) que se haya escrito mediante JavaScript con HTML.

>**Nota**&nbsp;&nbsp;Actualmente, esta característica solo se admite para aplicaciones para UWP que se hayan escrito mediante JavaScript con HTML.

[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) funciona tanto con contenido multimedia progresivo como de streaming y utiliza los formatos de carga Video Ad Serving Template (VAST) 2.0/3.0 y VMAP estándar de IAB. Al usar estándares, [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) es independiente del servicio de anuncios con el que interactúa.

La publicidad para el contenido de vídeo es diferente en función de si el programa es inferior a 10 minutos (formato corto) o superior a 10 minutos (formato largo). Aunque la segunda opción es más complicada de configurar en el servicio, realmente no existe ninguna diferencia en cómo se escribe el código del lado cliente. Si la clase [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) recibe una carga de VAST con un solo anuncio en lugar de un manifiesto, esto se trata como si el manifiesto llamara a un solo anuncio ya preparado (un salto en el minuto 00:00).

## <a name="prerequisites"></a>Requisitos previos

* Instala el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) con Visual Studio 2015.

* El proyecto debe usar el control [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) para proporcionar el contenido de vídeo en el que se programarán los anuncios. Este control está disponible en la colección de bibliotecas [TVHelpers](https://github.com/Microsoft/TVHelpers) disponible a través de Microsoft en GitHub.

  En el siguiente ejemplo se muestra cómo declarar un control [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) en formato HTML. Por lo general, este formato se incluye en la sección `<body>` en el archivo index.html (u otro archivo HTML, según corresponda al proyecto).

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  En el siguiente ejemplo se muestra cómo establecer un control [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) en código JavaScript.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Cómo usar la clase AdScheduler en el código

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega a tu proyecto una referencia a la biblioteca **Microsoft Advertising SDK for JavaScript**.

  a. Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**

  b. En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for JavaScript** (versión 10.0).

  c. En el **Administrador de referencias**, haz clic en Aceptar.

4.  Agrega el archivo AdScheduler.js al proyecto:

  a.  En Visual Studio, haz clic en **Proyecto** y luego en **Administrar paquetes de NuGet**.

  b.  En el cuadro de búsqueda, escribe **Microsoft.StoreServices.VideoAdScheduler** e instala el paquete Microsoft.StoreServices.VideoAdScheduler. El archivo AdScheduler.js se agrega al subdirectorio .. /js en el proyecto.

5.  Abre el archivo index.html (u otro archivo HTML adecuado para el proyecto). En la sección `<head>`, después de las referencias de JavaScript del proyecto de default.css y main.js, agrega la referencia a ad.js y adscheduler.js.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  <script src="/js/adscheduler.js"></script>
  ```

  <span/>
  > **Nota**&nbsp;&nbsp;Esta línea debe colocarse en la sección `<head>` después de incluir main.js. De lo contrario, se producirá un error al compilar el proyecto.

6.  En el archivo main.js del proyecto, agrega código que cree un nuevo objeto [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx). Pasa el objeto **MediaPlayer** que hospeda el contenido de vídeo. El código debe colocarse de forma que se ejecute después de [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/hh440975.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Usa los métodos [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) o [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) para solicitar una programación de anuncios del servidor e insertarla en la línea de tiempo de **MediaPlayer** y, a continuación, reproducir el contenido multimedia de vídeo.

  * Si eres un asociado de Microsoft que ha recibido permiso para solicitar una programación de anuncios del servidor de anuncios de Microsoft, usa [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) y especifica el identificador de aplicación y el identificador de unidad de anuncios que te haya proporcionado tu representante de Microsoft. Este método toma la forma de una **promesa**, que es una construcción asincrónica en la que se pasan dos punteros de función para controlar los casos de éxito y de error, respectivamente. Para obtener más información, consulta [Socios asíncronos en la UWP mediante JavaScript](https://msdn.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript).

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

  * Para solicitar una programación de anuncios de un servidor de anuncios que no sea de Microsoft, usa [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) y pasa la dirección URL del servidor. Este método también adopta la forma de una **promesa**.

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    <span/>
    >**Nota**&nbsp;&nbsp;Debes llamar a **play** incluso si la función no se ejecuta, ya que el objeto [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) indicará al objeto **MediaPlayer** que omita los anuncios y pase directamente al contenido. Puede que tengas un requisito de negocio distinto, como insertar un anuncio integrado si no es posible obtener un anuncio de forma remota.

8.  Durante la reproducción, puedes controlar eventos adicionales que permiten a tu aplicación realizar el seguimiento del progreso o los errores que pueden producirse tras el proceso de coincidencia de anuncios inicial. El siguiente código muestra algunos de estos eventos, incluidos [onPodStart](https://msdn.microsoft.com/library/windows/apps/mt732206.aspx), [onPodEnd](https://msdn.microsoft.com/library/windows/apps/mt732205.aspx), [onPodCountdown](https://msdn.microsoft.com/library/windows/apps/mt732204.aspx), [onAdProgress](https://msdn.microsoft.com/library/windows/apps/mt732201.aspx), [onAllComplete](https://msdn.microsoft.com/library/windows/apps/mt732202.aspx) y [onErrorOccurred](https://msdn.microsoft.com/library/windows/apps/mt732203.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]


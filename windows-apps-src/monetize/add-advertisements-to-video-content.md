---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Aprende a usar la clase AdScheduler para mostrar anuncios en contenido de vídeo.
title: Mostrar anuncios en contenido de vídeo
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, ads, anuncios, advertising, publicidad, video, vídeo, scheduler, programador, javascript, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 29e2c46636445adac496d0f2149e956c5703c20d
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691720"
---
# <a name="show-ads-in-video-content"></a>Mostrar anuncios en contenido de vídeo

Este tutorial muestra cómo usar la clase **AdScheduler** para mostrar anuncios en contenido de vídeo en una aplicación para la Plataforma universal de Windows (UWP) que se haya escrito mediante JavaScript con HTML.

> [!NOTE]
> Actualmente, esta característica solo se admite para aplicaciones para UWP que se hayan escrito mediante JavaScript con HTML.

**AdScheduler** funciona tanto con contenido multimedia progresivo como de streaming y utiliza los formatos de carga Video Ad Serving Template (VAST) 2.0/3.0 y VMAP estándar de IAB. Al usar estándares, **AdScheduler** es independiente del servicio de anuncios con el que interactúa.

La publicidad para el contenido de vídeo es diferente en función de si el programa es inferior a 10minutos (formato corto) o superior a 10minutos (formato largo). Aunque la segunda opción es más complicada de configurar en el servicio, realmente no existe ninguna diferencia en cómo se escribe el código del lado cliente. Si la clase **AdScheduler** recibe una carga de VAST con un solo anuncio en lugar de un manifiesto, esto se trata como si el manifiesto llamara a un solo anuncio ya preparado (un salto en el minuto 00:00).

## <a name="prerequisites"></a>Requisitos previos

* Instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior.

* El proyecto debe usar el control [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) para proporcionar el contenido de vídeo en el que se programarán los anuncios. Este control está disponible en la colección de bibliotecas [TVHelpers](https://github.com/Microsoft/TVHelpers) disponible a través de Microsoft en GitHub.

  En el siguiente ejemplo se muestra cómo declarar un control [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) en formato HTML. Por lo general, este formato se incluye en la sección `<body>` en el archivo index.html (u otro archivo HTML, según corresponda al proyecto).

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  En el siguiente ejemplo se muestra cómo establecer un control [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) en código JavaScript.

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Cómo usar la clase AdScheduler en el código

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega a tu proyecto una referencia a la biblioteca **Microsoft Advertising SDK for JavaScript**.

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y luego selecciona **Agregar referencia...**
    2. En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para JavaScript** (versión 10.0).
    3. En el **Administrador de referencias**, haz clic en Aceptar.

4.  Agrega el archivo AdScheduler.js al proyecto:

    1. En VisualStudio, haz clic en **Proyecto** y en **Administrar paquetes de NuGet**.
    2. En el cuadro de búsqueda, escribe **Microsoft.StoreServices.VideoAdScheduler** e instala el paquete Microsoft.StoreServices.VideoAdScheduler. El archivo AdScheduler.js se agrega al subdirectorio .. /js en el proyecto.

5.  Abre el archivo index.html (u otro archivo HTML adecuado para el proyecto). En la sección `<head>`, después de las referencias de JavaScript del proyecto de default.css y main.js, agrega la referencia a ad.js y adscheduler.js.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > Esta línea debe colocarse en la sección `<head>` después de incluir main.js. De lo contrario, se producirá un error al compilar el proyecto.

6.  En el archivo main.js del proyecto, agrega código que cree un nuevo objeto **AdScheduler**. Pasa el objeto **MediaPlayer** que hospeda el contenido de vídeo. El código debe colocarse de forma que se ejecute después de [WinJS.UI.processAll](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh440975).

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Usa los métodos **requestSchedule** o **requestScheduleByUrl** del objeto **AdScheduler** para solicitar una programación de anuncios del servidor e insertarla en la línea de tiempo de **MediaPlayer** y, a continuación, reproducir el contenido multimedia de vídeo.

    * Si eres un asociado de Microsoft que ha recibido permiso para solicitar una programación de anuncios del servidor de anuncios de Microsoft, usa **requestSchedule** y especifica el identificador de aplicación y el identificador de unidad de anuncios que te haya proporcionado tu representante de Microsoft.

        Este método toma la forma de una [Promesa](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript), que es una construcción asincrónica, donde se pasan dos punteros de función: un puntero para la función **onComplete** para llamarla cuando la promesa se completa correctamente y un puntero para la función **onError** para llamarla si se produce un error. En la función **onComplete**, inicia la reproducción de tu contenido de vídeo. El anuncio comenzará a reproducirse a la hora programada. En tu función **onError**, gestiona el error y, a continuación, inicia la reproducción del vídeo. El contenido del vídeo se reproducirá sin anuncio. El argumento de la función **onError** es un objeto que contiene los siguientes miembros.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Para solicitar una programación de anuncios de un servidor de anuncios que no sea de Microsoft, usa **requestScheduleByUrl** y pasa la dirección URI del servidor. Este método también adopta la forma de una **Promesa** que acepta punteros para las funciones **onComplete** y **onError**. La carga de anuncios que se devuelve del servidor debe cumplir con los formatos de carga Video Ad Serving Template (VAST) o Video Multiple Ad Playlist (VMAP).

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > Debería esperar a que se devuelvan **requestSchedule** o **requestScheduleByUrl** antes de comenzar a reproducir el contenido del vídeo principal en **MediaPlayer**. Empezar a reproducir medios antes de que se devuelva **requestSchedule** (en el caso de un anuncio ya preparado) dará como resultado la interrupción del contenido del vídeo principal. Debes llamar a **play** incluso si la función no se ejecuta, ya que el objeto **AdScheduler** indicará al objeto **MediaPlayer** que omita los anuncios y pase directamente al contenido. Puede que tengas un requisito de negocio distinto, como insertar un anuncio integrado si no es posible obtener un anuncio de forma remota.

8.  Durante la reproducción, puedes controlar eventos adicionales que permiten a tu aplicación realizar el seguimiento del progreso o los errores que pueden producirse tras el proceso de coincidencia de anuncios inicial. El siguiente código muestra algunos de estos eventos, incluidos **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** y **onErrorOccurred**.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>Miembros de AdScheduler

Esta sección proporciona algunos detalles sobre los miembros del objeto **AdScheduler**. Para obtener más información sobre estos miembros, consulta los comentarios y las definiciones del archivo AdScheduler.js del proyecto.

### <a name="requestschedule"></a>requestSchedule

Este método solicita un programa de anuncios desde el servidor de anuncios de Microsoft y lo inserta en la línea de tiempo de **MediaPlayer** pasada al constructor **AdScheduler**.

El tercer parámetro opcional (*adTags*) es una colección de JSON de pares nombre-valor que puede usarse para las aplicaciones que tienen un destino avanzado. Por ejemplo, una aplicación que reproduce una variedad de vídeos autorrelacionados puede complementar el identificador de unidad de anuncios del coche se muestra. Este parámetro está pensado para que lo usen solo los partners que reciben la aprobación de Microsoft para usar etiquetas de anuncios.

Deben tenerse en cuenta los siguientes elementos al hacer referencia a *adTags*:

* Este parámetro es una opción apenas utilizada. El editor debe trabajar estrechamente con Microsoft antes de usar adTags.
* Los nombres y los valores deben predeterminarse en el servicio de anuncios. Las etiquetas de anuncios no son términos de búsqueda abiertos o palabras clave.
* El número máximo de etiquetas admitido es 10.
* Los nombres de las etiquetas están restringidos a 16 caracteres.
* Los valores de las etiquetas tienen un máximo de 128 caracteres.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

Este método solicita un programa de anuncios desde el servidor de anuncios que no sea de Microsoft especificado en el URI y lo inserta en la línea de tiempo de **MediaPlayer** pasada al constructor **AdScheduler**. La carga de anuncios que devuelve el servidor de anuncios debe cumplir con los formatos de carga Video Ad Serving Template (VAST) o Video Multiple Ad Playlist (VMAP).

### <a name="mediatimeout"></a>mediaTimeout

Esta propiedad obtiene o establece el número de milisegundos que los medios deben reproducirse. Un valor de 0 informa al sistema ningún tiempo de espera. El valor predeterminado es 30000 ms (30 segundos).

### <a name="playskippedmedia"></a>playSkippedMedia

Esta propiedad obtiene o establece un valor **booleano** que indica si se reproducirán medios programados si el usuario salta a un punto posterior al tiempo de inicio programado.

El cliente de anuncios y el reproductor de medios aplicará reglas en cuanto a qué sucede con los anuncios durante el avance o retroceso rápidos del contenido del vídeo principal. En la mayoría de los casos, los desarrolladores de aplicaciones no permiten que los anuncios se salten totalmente sino que desean proporcionar una experiencia razonable para el usuario. Las dos opciones siguientes se insertan en las necesidades de la mayoría de los desarrolladores:

1. Permitir que los usuarios finales saltar pods de anuncios a voluntad.
2. Permitir a los usuarios saltar pods de anuncios pero reproducir el pod más reciente cuando se reanude la reproducción.

La propiedad **playSkippedMedia** tiene las siguientes condiciones:

* Los anuncios no se pueden saltar o avanzar rápidamente una vez que comienzan.
* Todos los anuncios de un pod de anuncio se reproducirán una vez que se inicie el pod.
* Una vez reproducido, no se reproducirá de nuevo un anuncio durante el contenido principal (película, episodio, etc.); los marcadores de anuncios se marcarán como reproducidos o eliminados.
* No se pueden saltar los anuncios ya preparados.

Cuando se reanuda el contenido que contiene publicidad, establece **playSkippedMedia** en **false** para saltar anuncios ya preparados y evitar que se reproduzcan los cortes de anuncios más recientes. A continuación, después de que se inicie el contenido, establezca **playSkippedMedia** en **true** para garantizar que los usuarios no puedan avanzar rápidamente a través de los anuncios posteriores.

> [!NOTE]
> Un pod es un grupo de anuncios que se reproducen en una secuencia, como un grupo de anuncios que se reproduce durante una interrupción comercial. Para obtener más información, consulta la especificación de IAB Digital Video Ad Serving Template (VAST).

### <a name="requesttimeout"></a>requestTimeout

Esta propiedad obtiene o establece el número de milisegundos para esperar una respuesta de la solicitud de anuncios antes del tiempo de espera. Un valor de 0 informa al sistema ningún tiempo de espera. El valor predeterminado es 30000 ms (30 segundos).

### <a name="schedule"></a>programar

Esta propiedad obtiene los datos de programación que se recuperaron del servidor de anuncios. Este objeto incluye la jerarquía completa de los datos que corresponde a la estructura de la carga de Video Ad Serving Template (VAST) o Video Multiple Ad Playlist (VMAP)

### <a name="onadprogress"></a>onAdProgress  

Este evento se genera cuando la reproducción de anuncios alcanza puntos de control de cuartil. El segundo parámetro del controlador de eventos (*eventInfo*) es un objeto JSON con los siguientes miembros:

* **progress**: el estado de reproducción de anuncios (uno de los valores de enumeración de **MediaProgress** definidos en AdScheduler.js).
* **clip**: el clip de vídeo que se está reproduciendo. Este objeto no está pensado para utilizarlo en el código.
* **adPackage**: un objeto que representa la parte de la carga de anuncios que corresponde al anuncio que se está reproduciendo. Este objeto no está pensado para utilizarlo en el código.

### <a name="onallcomplete"></a>onAllComplete  

Este evento se genera cuando el contenido principal llega al final y los anuncios posteriores al álbum programados también han terminado.

### <a name="onerroroccurred"></a>onErrorOccurred  

Este evento se genera cuando **AdScheduler** encuentra un error. Para obtener más información acerca de los valores de los códigos de error, consulta [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onPodCountdown

Este evento se genera cuando un anuncio se está reproduciendo e indica cuánto tiempo permanece en el pod actual. El segundo parámetro del controlador de eventos (*eventData*) es un objeto JSON con los siguientes miembros:

* **remainingAdTime**: el número de segundos restantes para el anuncio actual.
* **remainingPodTime**: el número de segundos restantes para el pod actual.

> [!NOTE]
> Un pod es un grupo de anuncios que se reproducen en una secuencia, como un grupo de anuncios que se reproduce durante una interrupción comercial. Para obtener más información, consulta la especificación de IAB Digital Video Ad Serving Template (VAST).

### <a name="onpodend"></a>onPodEnd  

Este evento se genera cuando finaliza un pod de anuncios. El segundo parámetro del controlador de eventos (*eventData*) es un objeto JSON con los siguientes miembros:

* **startTime**: el tiempo de inicio del pod, en segundos.
* **pod**: un objeto que representa el pod. Este objeto no está pensado para utilizarlo en el código.

### <a name="onpodstart"></a>onPodStart

Este evento se genera cuando se inicia un pod de anuncios. El segundo parámetro del controlador de eventos (*eventData*) es un objeto JSON con los siguientes miembros:

* **startTime**: el tiempo de inicio del pod, en segundos.
* **pod**: un objeto que representa el pod. Este objeto no está pensado para utilizarlo en el código.

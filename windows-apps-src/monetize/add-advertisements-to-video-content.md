---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Obtenga información sobre cómo usar la clase AdScheduler para mostrar anuncios en el contenido de vídeo en una aplicación Plataforma universal de Windows (UWP) que se escribió con JavaScript con HTML.
title: Mostrar anuncios en contenido de vídeo
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, vídeo, Scheduler, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 6baf26b083cce08557a9b09f2ba95d5ad889f4a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175109"
---
# <a name="show-ads-in-video-content"></a>Mostrar anuncios en contenido de vídeo

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tutorial se muestra cómo usar la clase **AdScheduler** para mostrar anuncios en el contenido de vídeo en una aplicación plataforma universal de Windows (UWP) que se escribió con JavaScript con HTML.

> [!NOTE]
> Actualmente, esta característica solo se admite para las aplicaciones para UWP escritas con JavaScript con HTML.

**AdScheduler** funciona tanto con contenido multimedia progresivo como de streaming y utiliza los formatos de carga Video Ad Serving Template (VAST) 2.0/3.0 y VMAP estándar de IAB. Al usar estándares, **AdScheduler** es independiente del servicio de anuncios con el que interactúa.

La publicidad para el contenido de vídeo es diferente en función de si el programa es inferior a 10 minutos (formato corto) o superior a 10 minutos (formato largo). Aunque la segunda opción es más complicada de configurar en el servicio, realmente no existe ninguna diferencia en cómo se escribe el código del lado cliente. Si la clase **AdScheduler** recibe una carga de VAST con un solo anuncio en lugar de un manifiesto, esto se trata como si el manifiesto llamara a un solo anuncio ya preparado (un salto en el minuto 00:00).

## <a name="prerequisites"></a>Requisitos previos

* Instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) con Visual Studio 2015 o una versión posterior.

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

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2. En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for JavaScript** (versión 10.0).
    3. En el **Administrador de referencias**, haz clic en Aceptar.

4.  Agrega el archivo AdScheduler.js al proyecto:

    1. En Visual Studio, haz clic en **Proyecto** y luego en **Administrar paquetes de NuGet**.
    2. En el cuadro de búsqueda, escribe **Microsoft.StoreServices.VideoAdScheduler** e instala el paquete Microsoft.StoreServices.VideoAdScheduler. El archivo AdScheduler.js se agrega al subdirectorio .. /js en el proyecto.

5.  Abre el archivo index.html (u otro archivo html adecuado para el proyecto). En la sección `<head>`, después de las referencias de JavaScript del proyecto de default.css y main.js, agrega la referencia a ad.js y adscheduler.js.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > Esta línea se debe colocar en la `<head>` sección después de la inclusión de main.js; de lo contrario, se producirá un error al compilar el proyecto.

6.  En el archivo main.js del proyecto, agrega código que cree un nuevo objeto **AdScheduler**. Pasa el objeto **MediaPlayer** que hospeda el contenido de vídeo. El código debe colocarse de forma que se ejecute después de [WinJS.UI.processAll](/previous-versions/windows/apps/hh440975).

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Use los métodos **requestSchedule** o **RequestScheduleByUrl** del objeto **AdScheduler** para solicitar una programación de ad del servidor e insértela en la escala de tiempo de **MediaPlayer** y, a continuación, reproduzca el medio de vídeo.

    * Si eres un asociado de Microsoft que ha recibido permiso para solicitar una programación de anuncios del servidor de anuncios de Microsoft, usa **requestSchedule** y especifica el identificador de aplicación y el identificador de unidad de anuncios que te haya proporcionado tu representante de Microsoft.

        Este método toma la forma de una [promesa](../threading-async/asynchronous-programming-universal-windows-platform-apps.md#asynchronous-patterns-in-uwp-using-javascript), que es una construcción asincrónica donde se pasan dos punteros de función: un puntero a la función **alcompletar** para llamar a cuando la promesa se completa correctamente y un puntero para que la función **OnError** llame a si se produce un error. En la función **Alfinalizar** , inicie la reproducción del contenido de vídeo. El anuncio comenzará a reproducirse a la hora programada. En la función **OnError** , controle el error y, a continuación, inicie la reproducción del vídeo. El contenido de vídeo se reproducirá sin un anuncio. El argumento de la función **OnError** es un objeto que contiene los miembros siguientes.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Para solicitar una programación de ad de un servidor de ad que no sea de Microsoft, use **requestScheduleByUrl**y pase el URI del servidor. Este método también toma la forma de una **promesa** que acepta punteros para las funciones **Alfinalizar** y **OnError** . La carga de ad que se devuelve desde el servidor debe ajustarse a los formatos de carga de la plantilla de servicio de anuncios de vídeo (VAST) o vídeo de varias listas de reproducción (VMAP).

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > Debe esperar a que se devuelvan **requestSchedule** o **requestScheduleByUrl** antes de empezar a reproducir el contenido de vídeo principal en el **MediaPlayer**. A partir de la reproducción de medios antes de que **requestSchedule** devuelva (en el caso de un anuncio de relanzamiento), la reversión previa interrumpirá el contenido del vídeo principal. Debe llamar a **Replay** incluso si se produce un error en la función, porque el **AdScheduler** indicará a la **MediaPlayer** que omita los anuncios y se desplace directamente al contenido. Puede que tengas un requisito de negocio distinto, como insertar un anuncio integrado si no es posible obtener un anuncio de forma remota.

8.  Durante la reproducción, puedes controlar eventos adicionales que permiten a tu aplicación realizar el seguimiento del progreso o los errores que pueden producirse tras el proceso de coincidencia de anuncios inicial. El siguiente código muestra algunos de estos eventos, incluidos **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** y **onErrorOccurred**.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>Miembros de AdScheduler

En esta sección se proporcionan algunos detalles sobre los miembros del objeto **AdScheduler** . Para obtener más información sobre estos miembros, vea los comentarios y las definiciones en el archivo AdScheduler.js del proyecto.

### <a name="requestschedule"></a>requestSchedule

Este método solicita una programación de ad del servidor de Microsoft ad y la inserta en la escala de tiempo del objeto **MediaPlayer** que se pasó al constructor **AdScheduler** .

El tercer parámetro opcional (*adTags*) es una colección JSON de pares nombre-valor que se puede usar para las aplicaciones que tienen destinatarios avanzados. Por ejemplo, una aplicación que reproduzca una variedad de vídeos relacionados automáticamente podría complementar el identificador de la unidad de anuncio con la marca y el modelo del coche que se muestra. Este parámetro está pensado para que lo usen únicamente los asociados que reciben la aprobación de Microsoft para usar etiquetas de ad.

Los siguientes elementos deben indicarse al hacer referencia a *adTags*:

* Este parámetro es una opción muy rara vez utilizada. El publicador debe trabajar en estrecha colaboración con Microsoft antes de usar adTags.
* Tanto los nombres como los valores deben estar predeterminados en el servicio de ad. Las etiquetas de ad no están abiertas, términos o palabras clave de búsqueda.
* El número máximo admitido de etiquetas es 10.
* Los nombres de etiqueta están limitados a 16 caracteres.
* Los valores de etiqueta tienen un máximo de 128 caracteres.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

Este método solicita una programación de ad del servidor que no es de Microsoft ad especificado en el URI y lo inserta en la escala de tiempo del objeto **MediaPlayer** que se pasó al constructor **AdScheduler** . La carga de ad que devuelve el servidor de ad debe ajustarse a los formatos de carga de vídeo de la plantilla de servicio de anuncios de vídeo (VAST) o vídeo de varias listas de reproducción (VMAP).

### <a name="mediatimeout"></a>mediaTimeout

Esta propiedad obtiene o establece el número de milisegundos que se debe reproducir el medio. Un valor de 0 informa al sistema de que nunca se agota el tiempo de espera. El valor predeterminado es 30000 MS (30 segundos).

### <a name="playskippedmedia"></a>playSkippedMedia

Esta propiedad obtiene o establece un valor **booleano** que indica si los medios programados se reproducirán si el usuario se salta a un punto más allá de la hora de inicio programada.

El cliente de AD y el reproductor de media aplicarán reglas en términos de lo que ocurre con los anuncios durante el reenvío rápido y el rebobinado del contenido del vídeo principal. En la mayoría de los casos, los desarrolladores de aplicaciones no permiten omitir los anuncios por completo, pero quieren proporcionar una experiencia razonable para el usuario. Las dos opciones siguientes se encuentran dentro de las necesidades de la mayoría de los desarrolladores:

1. Permitir a los usuarios finales omitir los pods de anuncios en lo que se requiera.
2. Permitir a los usuarios omitir los pods de anuncios pero reproducir el POD más reciente cuando se reanude la reproducción.

La propiedad **playSkippedMedia** tiene las siguientes condiciones:

* Los anuncios no se pueden omitir ni reenviar rápidamente una vez que se inicia el anuncio.
* Todos los anuncios de un pod de anuncio se reproducirán una vez que se haya iniciado el POD.
* Una vez jugado, un anuncio no se reproducirá de nuevo durante el contenido principal (película, episodio, etc.); los marcadores de anuncio se marcarán como reproducidos o eliminados.
* Los anuncios anteriores al lanzamiento no se pueden omitir.

Al reanudar el contenido que contiene publicidad, establezca **playSkippedMedia** en **false** para omitir las reversiones previas y evitar que se reproduzca la interrupción más reciente del anuncio. Después, después de que se inicie el contenido, establezca **playSkippedMedia** en **true** para asegurarse de que los usuarios no puedan avanzar rápidamente a través de los anuncios posteriores.

> [!NOTE]
> Un POD es un grupo de anuncios que se reproducen en una secuencia, como un grupo de anuncios que se reproducen durante una pausa comercial. Para obtener más información, consulte la especificación de la plantilla de anuncio de vídeo digital de IAB (VAST).

### <a name="requesttimeout"></a>requestTimeout

Esta propiedad obtiene o establece el número de milisegundos que se debe esperar una respuesta de solicitud de ad antes de que se agote el tiempo de espera. Un valor de 0 informa al sistema de que nunca se agota el tiempo de espera. El valor predeterminado es 30000 MS (30 segundos).

### <a name="schedule"></a>schedule

Esta propiedad obtiene los datos de programación recuperados del servidor de ad. Este objeto incluye la jerarquía completa de datos que corresponde a la estructura de la carga de vídeo de la plantilla de servicio de anuncios de vídeo (VAST) o de vídeo de varias listas de reproducción de anuncios (VMAP).

### <a name="onadprogress"></a>onAdProgress  

Este evento se desencadena cuando la reproducción de anuncios alcanza los puntos de control del cuartil. El segundo parámetro del controlador de eventos (*eventInfo*) es un objeto JSON con los siguientes miembros:

* **progreso**: el estado de reproducción de ad (uno de los valores de enumeración **MediaProgress** definidos en AdScheduler.js).
* **clip**: clip de vídeo que se está reproduciendo. Este objeto no está pensado para usarse en el código.
* **adPackage**: un objeto que representa la parte de la carga de ad que corresponde al anuncio que se está reproduciendo. Este objeto no está pensado para usarse en el código.

### <a name="onallcomplete"></a>onAllComplete  

Este evento se desencadena cuando el contenido principal alcanza el final y también finalizan los anuncios post-Roll programados.

### <a name="onerroroccurred"></a>onErrorOccurred  

Este evento se desencadena cuando **AdScheduler** detecta un error. Para obtener más información sobre los valores del código de error, vea [ErrorCode](/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onPodCountdown

Este evento se desencadena cuando se está reproduciendo un anuncio e indica cuánto tiempo queda en el POD actual. El segundo parámetro del controlador de eventos (*eventData*) es un objeto JSON con los siguientes miembros:

* **remainingAdTime**: el número de segundos que quedan para el anuncio actual.
* **remainingPodTime**: el número de segundos que quedan para el POD actual.

> [!NOTE]
> Un POD es un grupo de anuncios que se reproducen en una secuencia, como un grupo de anuncios que se reproducen durante una pausa comercial. Para obtener más información, consulte la especificación de la plantilla de anuncio de vídeo digital de IAB (VAST).

### <a name="onpodend"></a>onPodEnd  

Este evento se desencadena cuando finaliza un pod de ad. El segundo parámetro del controlador de eventos (*eventData*) es un objeto JSON con los siguientes miembros:

* **startTime**: la hora de inicio del Pod, en segundos.
* **Pod**: un objeto que representa el POD. Este objeto no está pensado para usarse en el código.

### <a name="onpodstart"></a>onPodStart

Este evento se desencadena cuando se inicia un pod de ad. El segundo parámetro del controlador de eventos (*eventData*) es un objeto JSON con los siguientes miembros:

* **startTime**: la hora de inicio del Pod, en segundos.
* **Pod**: un objeto que representa el POD. Este objeto no está pensado para usarse en el código.
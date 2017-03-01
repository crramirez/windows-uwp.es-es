---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Aprende a iniciar un anuncio intersticial con JavaScript/HTML.
title: "Código de ejemplo de anuncios intersticiales en JavaScript"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, anuncios, publicidad, intersticial, javascript, código de muestra, ads, advertising, interstitial, sample code"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 91a54bdd2e41b3e7df0ee0aad32448ab9ed66ac0
ms.lasthandoff: 02/07/2017

---

# <a name="interstitial-ad-sample-code-in-javascript"></a>Código de ejemplo de anuncios intersticiales en JavaScript

En este tema se proporciona el código de ejemplo completo para una aplicación básica para la Plataforma universal de Windows (UWP) en JavaScript y HTML que muestra un anuncio intersticial. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos HTML y JavaScript en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copia este código en un proyecto **WinJS App (Windows universal)** en JavaScript de Visual Studio 2015.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Los archivos main.js y index.html generados por Visual Studio se han modificado y se muestran a continuación. El archivo script.js que se muestra a continuación contiene la mayor parte del código de la muestra y debes agregar este archivo a la carpeta **js** del proyecto.

>**Nota para Windows 8.x y Windows Phone 8.1**&nbsp;&nbsp;Si el proyecto está dirigido a Windows 8.1 o Windows Phone 8.1, el archivo HTML predeterminado del proyecto se denomina default.html en lugar de index.html y el archivo JavaScript predeterminado del proyecto se denomina default.js en lugar de main.js.

Reemplaza los valores de las variables ```applicationId``` y ```adUnitId``` por valores dinámicos del Centro de desarrollo de Windows antes de enviar la aplicación a la Tienda. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

<span/>
>**Nota para Windows 8.x y Windows Phone 8.1**&nbsp;&nbsp;Si el proyecto está destinado a Windows 8.1 o Windows Phone 8.1, reemplaza la línea ```<script src="//Microsoft.Advertising.JavaScript/ad.js"></script>``` del ejemplo por ```<script src="/MSAdvertisingJS/ads/ad.js"></script>```.

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 


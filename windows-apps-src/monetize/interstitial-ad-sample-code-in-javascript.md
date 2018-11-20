---
author: Xansky
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Aprende a iniciar un anuncio intersticial con JavaScript/HTML.
title: Código de ejemplo de anuncios intersticiales en JavaScript
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, intersticial, javascript, código de muestra, ads, advertising, interstitial, sample code
ms.localizationpriority: medium
ms.openlocfilehash: 1921725e5b598a2e5e79ed90c607414e4efe574e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7421693"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Código de ejemplo de anuncios intersticiales en JavaScript

En este tema se proporciona el código de ejemplo completo para una aplicación básica para la Plataforma universal de Windows (UWP) en JavaScript y HTML que muestra un anuncio intersticial. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos HTML y JavaScript en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copia este código en un proyecto **WinJS App (Windows universal)** de JavaScript en Visual Studio.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Los archivos main.js y index.html generados por Visual Studio se han modificado y se muestran a continuación. El archivo script.js que se muestra a continuación contiene la mayor parte del código de la muestra y debes agregar este archivo a la carpeta **js** del proyecto.

Reemplaza los valores de la ```applicationId``` y ```adUnitId``` variables por valores dinámicos del centro de partners antes de enviar la aplicación a la tienda. Para más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para modificar este ejemplo y mostrar un anuncio de banner intersticial en lugar de un anuncio de vídeo intersticial, pasa el valor **InterstitialAdType.display** al primer parámetro del método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) en lugar de **InterstitialAdType.video**. Para más información, consulta [Anuncios intersticiales](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Artículos relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)

 

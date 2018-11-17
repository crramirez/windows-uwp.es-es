---
author: Xansky
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Aprende a iniciar un anuncio intersticial con C#.
title: Código de ejemplo de anuncios intersticiales en C#
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, intersticial, c#, código de muestra, ads, advertising, interstitial, sample code
ms.localizationpriority: medium
ms.openlocfilehash: 07f0927d741bbadc31aaf26f5188af245ec44790
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7155257"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Código de ejemplo de anuncios intersticiales en C\# #  

En este tema se proporciona un código de muestra completo para una aplicación básica para la Plataforma universal de Windows (UWP) en C# y XAML que muestra un anuncio intersticial. Para instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos MainPage.xaml y MainPage.xaml.cs en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copia el código en un proyecto de **aplicación vacía (Windows universal)** de Visual C# en Visual Studio.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Reemplaza los valores de la ```myAppId``` y ```myAdUnitId``` campos por valores dinámicos del centro de partners antes de enviar la aplicación a la tienda. Para más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para modificar este ejemplo y mostrar un anuncio de banner intersticial en lugar de un anuncio de vídeo intersticial, pasa el valor **AdType.Display** al primer parámetro del método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) en lugar de **AdType.Video**. Para más información, consulta [Anuncios intersticiales](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Artículos relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
 

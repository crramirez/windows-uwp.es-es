---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "Obtén información sobre cómo iniciar un anuncio intersticial con C#."
title: "Código de ejemplo de anuncios intersticiales en C#"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, anuncios, publicidad, intersticial, c#, código de muestra, ads, advertising, interstitial, sample code"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4c57cf4909028d5aa81c75d9e1b6f1bf28d41ad7
ms.lasthandoff: 02/07/2017

---

# <a name="interstitial-ad-sample-code-in-c"></a>Código de ejemplo de anuncios intersticiales en C\# #  

En este tema se proporciona el código de ejemplo completo para una aplicación básica para la Plataforma universal de Windows (UWP) en C# y XAML que muestra un anuncio intersticial. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos MainPage.xaml y MainPage.xaml.cs en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copia el código en un proyecto de **aplicación vacía (Windows universal)** en Visual C# de Visual Studio 2015.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Reemplaza los valores de los campos ```myAppId``` y ```myAdUnitId``` por valores dinámicos del Centro de desarrollo de Windows antes de enviar tu aplicación a la Tienda. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)
 


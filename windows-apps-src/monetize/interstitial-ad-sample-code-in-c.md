---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Aprende a iniciar un anuncio intersticial con C#.
title: "Código de ejemplo de anuncios intersticiales en C#"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, anuncios, publicidad, intersticial, c#, código de muestra, ads, advertising, interstitial, sample code"
ms.openlocfilehash: 2c0020ff7d750a97380c351358ec6aafb996125f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="interstitial-ad-sample-code-in-c"></a>Código de ejemplo de anuncios intersticiales en C\# #  

En este tema se proporciona un código de muestra completo para una aplicación básica para la Plataforma universal de Windows (UWP) en C# y XAML que muestra un anuncio intersticial. Para instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos MainPage.xaml y MainPage.xaml.cs en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copia el código en un proyecto de **aplicación vacía (Windows universal)** en Visual C# de Visual Studio 2015.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Reemplaza los valores de los campos ```myAppId``` y ```myAdUnitId``` por valores dinámicos del Centro de desarrollo de Windows antes de enviar tu aplicación a la Tienda. Para más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

>**Nota**&nbsp;&nbsp;Para modificar este ejemplo y mostrar un anuncio de banner intersticial en lugar de un anuncio de vídeo intersticial, pasa el valor **AdType.Display** al primer parámetro del método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) en lugar de **AdType.Video**. Para más información, consulta [Anuncios intersticiales](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
 

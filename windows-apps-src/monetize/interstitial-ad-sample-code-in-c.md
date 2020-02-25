---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Aprende a iniciar un anuncio intersticial con C#.
title: Código de ejemplo de anuncios intersticiales en C#
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, intersticial, c#, código de muestra, ads, advertising, interstitial, sample code
ms.localizationpriority: medium
ms.openlocfilehash: 9d908151e30510977669f2bc101575754a946560
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507049"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Código de ejemplo de ad intersticial en C\# #  

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tema se proporciona un código de muestra completo para una aplicación básica para la Plataforma universal de Windows (UWP) en C# y XAML que muestra un anuncio intersticial. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos MainPage.xaml y MainPage.xaml.cs en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copia el código en un proyecto de **aplicación vacía (Windows universal)** de Visual C# en Visual Studio.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Reemplace los valores de los campos ```myAppId``` y ```myAdUnitId``` por los valores activos del centro de Partners antes de enviar la aplicación a la tienda. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para modificar este ejemplo y mostrar un anuncio de banner intersticial en lugar de un anuncio de vídeo intersticial, pasa el valor **AdType.Display** al primer parámetro del método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) en lugar de **AdType.Video**. Para obtener más información, consulta [Anuncios intersticiales](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Temas relacionados

* [Muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 

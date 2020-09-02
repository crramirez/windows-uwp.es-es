---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Obtenga información sobre cómo mostrar e iniciar un anuncio intersticial en una aplicación Plataforma universal de Windows (UWP) con C# y XAML.
title: Código de ejemplo de anuncios intersticiales en C#
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Advertising, intersticial, c#, código de ejemplo
ms.localizationpriority: medium
ms.openlocfilehash: 3d80e53fd8d122e01588ffcbd16f487e1526c08d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363688"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Código de ejemplo de ad intersticial en C\# #  

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tema se proporciona el código de ejemplo completo para una aplicación básica de C# y Plataforma universal de Windows XAML (UWP) que muestra un anuncio de vídeo intersticial. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos MainPage.xaml y MainPage.xaml.cs en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copie el código en un proyecto de **aplicación vacía de Visual C# (Windows universal)** en Visual Studio.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Reemplace los valores de los ```myAppId``` ```myAdUnitId``` campos y por los valores activos del centro de Partners antes de enviar la aplicación a la tienda. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para modificar este ejemplo para mostrar un anuncio de banner intersticial en lugar de un anuncio de vídeo intersticial, pase el valor **AdType. display** al primer parámetro del método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) en lugar de **AdType. video**. Para obtener más información, consulta [Anuncios intersticiales](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
:::code language="xml" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml" range="1-13":::

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="CompleteSample":::

 
## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 

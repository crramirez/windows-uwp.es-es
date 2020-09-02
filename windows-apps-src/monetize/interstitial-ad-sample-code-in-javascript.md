---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Obtenga información sobre cómo iniciar un anuncio intersticial con una aplicación Plataforma universal de Windows (UWP) escrita en JavaScript y HTML.
title: Código de ejemplo de anuncios intersticiales en JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Advertising, intersticial, JavaScript, código de ejemplo
ms.localizationpriority: medium
ms.openlocfilehash: 07a61bfde1fc6a77f87617ee553408c71f79cbc3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363647"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Código de ejemplo de anuncios intersticiales en JavaScript

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tema se proporciona el código de ejemplo completo para una aplicación básica para la Plataforma universal de Windows (UWP) en JavaScript y HTML que muestra un anuncio intersticial. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Ejemplo de código

En esta sección se muestra el contenido de los archivos HTML y JavaScript en una aplicación básica que muestra un anuncio intersticial. Para usar estos ejemplos, copie este código en un proyecto de **aplicación WinJS de JavaScript (Windows universal)** en Visual Studio.

Esta aplicación de muestra usa dos botones para solicitar y, después, iniciar un anuncio intersticial. Los archivos main.js y index.html generados por Visual Studio se han modificado y se muestran a continuación. El archivo script.js que se muestra a continuación contiene la mayor parte del código de la muestra y debes agregar este archivo a la carpeta **js** del proyecto.

Reemplace los valores de las ```applicationId``` ```adUnitId``` variables y por los valores activos del centro de Partners antes de enviar la aplicación a la tienda. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Para modificar este ejemplo para mostrar un anuncio de banner intersticial en lugar de un anuncio de vídeo intersticial, pase el valor **InterstitialAdType. display** al primer parámetro del método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) en lugar de **InterstitialAdType. video**. Para obtener más información, consulta [Anuncios intersticiales](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
:::code language="html" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/index.html" range="1-21":::

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="script":::

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/main.js" id="main":::

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 

---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "Aprende a crear mediante programación un objeto **AdControl** con JavaScript."
title: Crear un objeto AdControl en Javascript
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, JavaScript
ms.openlocfilehash: b669925c3b630ddbfe82086231c46c951072244b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-an-adcontrol-in-javascript"></a>Crear un objeto AdControl en Javascript




En los ejemplos de este artículo se muestra cómo crear mediante programación un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) con JavaScript. En este artículo se supone que ya has agregado las referencias necesarias a tu proyecto para usar **AdControl**. Para obtener más información, incluido un tutorial detallado para crear e inicializar un objeto **AdControl** en formato HTML en lugar de JavaScript, consulta [AdControl en HTML 5 y Javascript](adcontrol-in-html-5-and-javascript.md).

## <a name="html-div-for-an-adcontrol"></a>Div HTML para un objeto AdControl

Un objeto **AdControl** debe incluir **div** en la página HTML que mostrará el anuncio. El siguiente código proporciona un ejemplo de **div**.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## <a name="javascript-for-creating-an-adcontrol"></a>JavaScript para crear un objeto AdControl

En el siguiente ejemplo se da por hecho que estás usando un valor **div** existente en el código HTML con el identificador **MyAd**.

Crea una instancia del objeto **AdControl** en la función **app.onactivated**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

En este ejemplo se supone que ya has declarado los métodos del controlador de eventos llamados **myAdError**, **myAdRefreshed** y **myAdEngagedChanged**.

>**Nota**&nbsp;&nbsp;Los valores de *applicationId* y *adUnitId* que se muestran en este ejemplo son [valores de modo de prueba](test-mode-values.md). Debes [reemplazar estos valores por valores dinámicos](set-up-ad-units-in-your-app.md) en el Centro de desarrollo de Windows antes de enviar tu aplicación.

Si usas este código y no ves anuncios, puedes intentar insertar un atributo **position:relative** en **div** que contenga **AdControl**. Esto invalidará la configuración predeterminada de **IFrame**. Los anuncios se mostrarán correctamente, a menos que no se muestren debido al valor de este atributo. Ten en cuenta que es posible que las nuevas unidades de anuncios no estén disponibles hasta 30 minutos.

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)

 

 

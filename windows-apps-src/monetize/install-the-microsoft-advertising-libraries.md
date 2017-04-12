---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Aprende a instalar las bibliotecas de Microsoft Advertising.
title: Instalar las bibliotecas de Microsoft Advertising
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, instalar, SDK, bibliotecas, ads, advertising, install, libraries
ms.openlocfilehash: 3304efd659a32176a44c33d9df4e8062b3bc7700
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="install-the-microsoft-advertising-libraries"></a>Instalar las bibliotecas de Microsoft Advertising




En el caso de las aplicaciones para la Plataforma universal de Windows (UWP) para Windows 10, las bibliotecas de Microsoft Advertising se incluyen en el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Este SDK es una extensión de Visual Studio 2015 y versiones posteriores. Para obtener más información sobre la instalación de este SDK, consulta [este artículo](microsoft-store-services-sdk.md).

> **Nota:**&nbsp;&nbsp;Si instalaste Windows 10 SDK (14393) o una versión posterior, debes instalar también la biblioteca WinJS si quieres agregar anuncios a una aplicación para UWP JavaScript/HTML. Esta biblioteca estaba incluida en versiones anteriores de Windows 10 SDK, pero a partir de Windows 10 SDK (14339), debe instalarse por separado. Para instalar WinJS, consulta [Obtener WinJS](http://try.buildwinjs.com/download/GetWinJS/).

Para aplicaciones XAML y JavaScript/HTML para Windows 8.1 y Windows Phone 8.x, las bibliotecas de Microsoft Advertising se incluyen en el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk). Este SDK es una extensión de Visual Studio 2015 y Visual Studio 2013.

Para las aplicaciones de Windows Phone Silverlight 8.x, las bibliotecas de Microsoft Advertising están disponibles en un paquete NuGet que puedes descargar e instalar en tu proyecto. Para obtener más información, consulta [AdControl en Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## <a name="library-names-for-advertising"></a>Nombres de las bibliotecas de publicidad


Hay varias bibliotecas de publicidad diferentes en el Microsoft Store Services SDK y el SDK de Microsoft Advertising para Windows y Windows Phone 8.x:

* El Microsoft Store Services SDK incluye las bibliotecas de Microsoft Advertising (que proporcionan las clases [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para las aplicaciones XAML y JavaScript/HTML).

* El SDK de Microsoft Advertising para Windows y Windows Phone 8.x incluye dos conjuntos de bibliotecas de publicidad: las bibliotecas de Microsoft Advertising (que proporcionan las clases [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para aplicaciones XAML y JavaScript/HTML) y las bibliotecas para la mediación de anuncios (que proporcionan la clase **AdMediatorControl**).

En esta documentación se describe cómo usar las clases **AdControl** e **InterstitialAd** en las bibliotecas de Microsoft Advertising para mostrar anuncios de banner o intersticiales. Para obtener información sobre cómo usar la mediación de anuncios para las aplicaciones de Windows 8.1 y Windows Phone 8.x, consulta [Usar la mediación de anuncios para maximizar los ingresos](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Nota**&nbsp;&nbsp;La mediación de anuncios que usa la clase **AdMediatorControl** actualmente no se admite para aplicaciones para UWP en Windows10. La mediación del lado del servidor estará disponible próximamente para aplicaciones para UWP con las mismas API de anuncios de banner (**AdControl**) y de anuncios intersticiales (**InterstitialAd**).

Antes de poder usar cualquiera de los controles de publicidad en el código de la aplicación, debes hacer referencia a la biblioteca adecuada en el proyecto. En las tablas siguientes se indican los nombres de cada una de las bibliotecas, tal como aparecen en el cuadro de diálogo **Administrador de referencias** en Visual Studio.


<table>
    <thead>
        <tr><th>Nombre del control</th><th>Tipo de proyecto</th><th>Nombre de la biblioteca en el Administrador de referencias</th><th>Número de versión</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** e **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>SDK de Microsoft Advertising para XAML</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>SDK de Ad Mediator para Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>SDK de Ad Mediator para Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** e **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>SDK de Microsoft Advertising para JavaScript</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>SDK de Microsoft Advertising para Windows 8.1 (JS) nativo</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>SDK de Microsoft Advertising para Windows Phone 8.1 (JS) nativo</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl** (XAML únicamente)</td>
            <td>UWP</td>
            <td>SDK de Microsoft Advertising Universal</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows8.1</td>
            <td>SDK de Ad Mediator para Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>SDK de Ad Mediator para Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 

---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Aprende a instalar las bibliotecas de Microsoft Advertising.
title: Instalar las bibliotecas de Microsoft Advertising

---

# Instalar las bibliotecas de Microsoft Advertising


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Las bibliotecas de Microsoft Advertising para aplicaciones de Windows se incluyen en el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Este SDK es una extensión de Visual Studio 2015 y Visual Studio 2013.

Para obtener instrucciones de instalación, consulta [Rentabilizar tu aplicación y atraer a los clientes con el SDK de Microsoft Store Engagement and Monetization](https://msdn.microsoft.com/windows/uwp/monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk).

> **Nota** Si has instalado la compilación 14295 de Windows 10 Anniversary SDK Preview o posterior con Visual Studio de 2015, también debes instalar la biblioteca WinJS si quieres agregar anuncios a una aplicación de JavaScript/HTML. Esta biblioteca antes estaba incluida en versiones anteriores de Windows SDK para Windows 10, pero esta biblioteca debe instalarse por separado a partir de la compilación 14295 de Windows 10 Anniversary SDK Preview. Para instalar WinJS, consulta [Obtener WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## Nombres de las bibliotecas de publicidad y mediación de anuncios


El SDK de Microsoft Store Engagement and Monetization incluye dos conjuntos de bibliotecas de publicidad: las bibliotecas para Microsoft Advertising (que proporcionan las clases [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para aplicaciones de XAML y JavaScript/HTML) y las bibliotecas para las bibliotecas de mediación de anuncios (que proporcionan la clase **AdMediatorControl**).

En esta documentación se describe cómo usar las clases **AdControl** e **InterstitialAd** en las bibliotecas de Microsoft Advertising para mostrar banners o anuncios intersticiales en vídeo de Microsoft. Para obtener información sobre el uso de la mediación de anuncios, consulta [Usar la mediación de anuncios para maximizar los ingresos](https://msdn.microsoft.com/windows/uwp/monetize/use-ad-mediation-to-maximize-revenue).


Antes de poder usar cualquiera de los controles de publicidad en el código de la aplicación, debes hacer referencia a la biblioteca adecuada en el proyecto. Las siguientes tablas enumeran los nombres de cada una de las bibliotecas en el SDK de Microsoft Store Engagement and Monetization a medida que aparecen en el cuadro de diálogo **Administrador de referencias** de Visual Studio.


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
            <td>Windows 8.1</td>
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
            <td>Windows 8.1</td>
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
            <td>Windows 8.1</td>
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

 

 

 


<!--HONumber=May16_HO2-->



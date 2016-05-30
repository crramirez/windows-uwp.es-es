---
author: mcleanbyron
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: Antes de usar mediación de anuncios, tienes que configurar cuentas con cada red de anuncios que quieras usar en tus aplicaciones.
title: Seleccionar y administrar redes de anuncios
---

# Seleccionar y administrar redes de anuncios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Antes de usar mediación de anuncios, tienes que configurar cuentas con cada red de anuncios que quieras usar en tus aplicaciones. Te recomendamos hacerlo antes de agregar el control de Ad Mediator a tu proyecto de Microsoft Visual Studio. También recomendamos configurar las cuentas con todas las redes de anuncios que sea posible, para que puedas disponer de la máxima flexibilidad para optimizar el rendimiento de mediación de anuncios.

## Tipos de redes de anuncios y proyectos compatibles

Actualmente, la mediación de anuncios es compatible con los tipos de redes de anuncios y proyectos siguientes.

|  Red de anuncios    | Aplicación de la Plataforma universal de Windows (UWP) con C# o Visual Basic con XAML | Aplicación para Windows 8.1 con C# o Visual Basic con XAML | Aplicación para Windows Phone 8.1 con C# o Visual Basic con XAML | Aplicación para Windows Phone 8.x Silverlight |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **Admitido**                                      | **Admitido**                                            | **Admitido**                     | **Admitido** |
| [AdDuplex](#adduplex)                                                   | **Admitido**                                      | **Admitido**                                            | **Admitido**                     | **Admitido** |
| [Smaato](#smaato)                                                       | No admitido                                      | **Admitido**                                            | **Admitido**                     | **Admitido** |
| [AdMob (Google)](#admob)                                                | No admitido                                      | No admitido                                            | No admitido                     | **Admitido** |
| [Inneractive](#inneractive)                                             | No admitido                                      | No admitido                                            | No admitido                     | **Admitido** |
| [Vserv VMAX](#vserv)                                                    | No admitido                                      | No admitido                                            | **Admitido**                     | No admitido | 

La mediación de anuncios no admite actualmente algunos tipos de proyecto, como C++ con XAML y JavaScript con HTML. Para estos tipos de proyecto, puedes mostrar anuncios de Microsoft sin necesidad de usar la mediación de anuncios. Para obtener más información, consulta [AdControl en XAML y .NET](https://msdn.microsoft.com/library/mt313186.aspx) y [AdControl en HTML 5 y JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Parámetros y detalles específicos de cada red


A continuación se describen los detalles específicos que necesitarás para cada red de anuncios, incluidos los procedimientos para configurar la cuenta e incorporar a una aplicación. También ofrecemos una lista de los parámetros necesarios que debes proporcionar cuando [envíes tu aplicación y configures la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md) (o en Servicios conectados para [probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)). Para obtener más información sobre el uso de una red de anuncios específica, visita su sitio web.

Además de los parámetros necesarios, cada red de anuncios también tiene parámetros opcionales adicionales que se puede establecer a través del código de la aplicación. Para ver ejemplos, consulta [Parámetros de red de anuncios opcionales](#optionalparameters) más adelante en este tema. Para obtener la lista completa de los parámetros opcionales, consulta la documentación que se incluye con cada red de anuncios. La mediación de anuncios omitirá o sobrescribirá algunos de estos parámetros, que se enumerarán en las secciones siguientes. Por ejemplo, los parámetros relacionados con la ubicación actual normalmente se sobrescriben con información sobre la ubicación del cliente que viene determinada por la funcionalidad de ubicación de la propia aplicación.

Ten en cuenta que al [agregar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md) y especificar las redes de anuncios que se usarán, Visual Studio intenta capturar los ensamblados necesarios mediante programación para algunas redes de anuncios. Si hay ensamblados que no se pueden recuperar automáticamente, puedes instalarlos mediante los vínculos que se muestran a continuación para cada red de anuncios.

### Microsoft

| Sitio web                        | Para configurar los parámetros de red de anuncios, usa la página [rentabilizar con anuncios](https://msdn.microsoft.com/library/windows/apps/mt170658) en el [panel del Centro de desarrollo de Windows](https://dev.windows.com/overview).   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ubicación del SDK                   | [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk).                                                                                                                                                                                                                         |
| Incorporación de una aplicación              | Agregar el control de mediación de anuncios a tu aplicación y enviar la aplicación en el panel del Centro de desarrollo de Windows.                                                                                                                                                                                                            |
| Parámetros necesarios            | ApplicationId y AdUnitId: estos parámetros se rellenan automáticamente al enviar el paquete de la aplicación, en función del contenido de la aplicación. Sin embargo, también puedes editar estos parámetros al [enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md). <br> <br> Alto y ancho (solo necesario para Windows Phone 8 Silverlight y Windows Phone 8.1 Silverlight).                                                                                                                                                                                                           |
| Parámetros que se sobrescriben u omiten | Latitude (se sobrescribe)  <br><br> Longitude (se sobrescribe) <br><br> AutoRefreshIntervalInSeconds (se omite) <br><br> IsAutoRefreshEnabled (se omite) <br><br> IsAutoCollapsedEnabled (se omite) <br><br> IsEngaged (se omite) <br><br> IsSuspended (se omite) |

 

### AdDuplex

| Sitio web                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| Ubicación del SDK                   | En primer lugar, intenta dejar que el control de Ad Mediator recupere los ensamblados a través de Servicios conectados como se describe en [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md). Si necesitas descargarlos manualmente, ve a la sección [Introducción](http://go.microsoft.com/fwlink/p/?LinkId=518029) del sitio web de AdDuplex. |
| Incorporación de una aplicación              | En el portal de AdDuplex, crea una nueva aplicación.                                                                                                                                                                                                                                                                                                        |
| Parámetros necesarios            | AppKey <br><br> AdUnitId <br><br> Size (solo es necesario para Windows 8.1 XAML)  |
| Parámetros que se sobrescriben u omiten | Latitude (se sobrescribe) <br><br> Longitude (se sobrescribe) <br><br> AutoRefreshIntervalInSeconds (se omite) <br><br> IsAutoRefreshEnabled (se omite) <br><br> IsAutoCollapsedEnabled (se omite) <br><br> IsEngaged (se omite) <br><br> IsSuspended (se omite) |
 

### Smaato

| Sitio web                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ubicación del SDK                   | En primer lugar, intenta dejar que el control de Ad Mediator recupere los ensamblados a través de Servicios conectados como se describe en [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md). Si necesitas descargarlos manualmente, ve a la pestaña Descargas del portal de Smaato. |
| Incorporación de una aplicación              | En el portal Smaato, ve a My Spaces (mis espacios) y genera un nuevo Adspace (espacio publicitario).                                                                                                                                                                                                                |
| Parámetros necesarios            | Pub <br> <br> Adspace <br> <br> Height y Width (solo son necesarios para Windows 8.1 XAML)  |
| Parámetros que se sobrescriben u omiten | Gps (se sobrescribe)                                                                                                                                                                                                                                                                |

 

### AdMob (Google)

| Sitio web                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Ubicación del SDK                   | Obtén el SDK de Windows Phone 8 del [sitio web de Google Mobile Ads SDK](http://go.microsoft.com/fwlink/p/?LinkId=518032). |
| Incorporación de una aplicación              | En el portal de AdMob, selecciona Obtener ingresos de la nueva aplicación.                                                                          |
| Parámetros necesarios            | AdUnitID <br> <br> Format                                                                                              |
| Parámetros que se sobrescriben u omiten | Location (se sobrescribe)  <br><br> ForceTesting (se omite) <br><br> Refresh Rate (se omite)                                |
 

### Inneractive

| Sitio web             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ubicación del SDK        | En primer lugar, intenta dejar que el control de Ad Mediator recupere los ensamblados a través de Servicios conectados como se describe en [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md). Si necesitas descargarlos manualmente, inicia sesión en tu cuenta y ve a la pestaña SDK en el panel para descargar el SDK de Windows Phone 8. |
| Incorporación de una aplicación   | En el portal de Inneractive, crea una nueva aplicación.                                                                                                                                                                                                                                                                                           |
| Parámetros necesarios | AppID <br> <br> AdType (IaAdType_Banner o IaAdType_Text)                                                                               |
 

### Vserv VMAX

| Sitio web             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ubicación del SDK        | En primer lugar, intenta dejar que el control de Ad Mediator recupere los ensamblados a través de Servicios conectados como se describe en [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md). Si necesitas descargarlos manualmente, ve a la sección [SDK](http://go.microsoft.com/fwlink/?LinkId=627078) del sitio web de VMAX. |
| Incorporación de una aplicación   | Obtén el id. de Adspot para la aplicación desde el sitio web de VMAX (el id. de Adspot se denomina ZoneId en las API de VMAX).                                                                                                                                                                                                                     |
| Parámetros necesarios | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## Parámetros de red de anuncios opcionales


Además de los parámetros necesarios, cada red de anuncios también tiene parámetros opcionales adicionales que se puede establecer a través del código de la aplicación. Para obtener la lista completa de los parámetros opcionales, consulta la documentación que se incluye con cada red de anuncios. Para establecer estos parámetros opcionales en el código, usa la propiedad **AdSdkOptionalParameters** de tu objeto **AdMediatorControl**.

El siguiente ejemplo muestra cómo establecer la propiedad [CountryOrRegion](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.countryorregion.aspx) de Microsoft Advertising en el código de país o región de dos letras del usuario.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

En el siguiente ejemplo se muestra cómo establecer los parámetros **Width** y **Height** para Smaato.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## Temas relacionados

* [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md)
* [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)
* [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de mediación de anuncios](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->



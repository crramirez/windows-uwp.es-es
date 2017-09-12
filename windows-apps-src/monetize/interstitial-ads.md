---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Aprende a incluir anuncios intersticiales en una aplicación de Windows10, Windows8.1 o Windows Phone8.1 con las bibliotecas de Microsoft Advertising."
title: Anuncios intersticiales
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, control de anuncios, intersticial
ms.openlocfilehash: 16cb78d9419d54a1bd56fb855ca1fad6fedf665a
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2017
---
# <a name="interstitial-ads"></a>Anuncios intersticiales

En este tutorial se muestra como incluir anuncios intersticiales en aplicaciones y juegos para la Plataforma universal de Windows (UWP) para Windows 10, así como en aplicaciones para Windows 8.1 o Windows Phone 8.1. Para obtener proyectos de muestra completos que muestran cómo agregar anuncios intersticiales a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>¿Qué son los anuncios intersticiales?

A diferencia de los anuncios de banner estándar, que están confinados a una porción de la interfaz de usuario en una aplicación o juego, los anuncios intersticiales se muestran en toda la pantalla. Con frecuencia se usan dos formas básicas en los juegos.

* Con anuncios *Paywall*, el usuario debe ver un anuncio en algún intervalo periódico. Por ejemplo, entre niveles del juego:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Con los anuncios *Rewards Based*, el usuario busca explícitamente alguna ventaja, como una sugerencia o tiempo extra para completar el nivel, e inicializa el anuncio a través de la interfaz de usuario de la aplicación.

Ofrecemos dos tipos de anuncios intersticiales para usarlos en aplicaciones y juegos:

* **Interstitial video ads**: están disponibles en aplicaciones para la Plataforma universal de Windows (UWP) para Windows 10 y en aplicaciones para Windows 8.1 o Windows Phone 8.1.

* **Interstitial banner ads**: solo están disponibles en aplicaciones para UWP para Windows 10.

> [!NOTE]
> La API para anuncios intersticiales no controla la interfaz de usuario excepto en el momento de la reproducción del vídeo. Consulta las [prácticas recomendadas para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obtener instrucciones sobre qué hacer y qué evitar cuando pienses en cómo integrar anuncios intersticiales en la aplicación.

## <a name="build-an-app-with-interstitial-ads"></a>Crear una aplicación con anuncios intersticiales

Para mostrar anuncios intersticiales en tu aplicación, sigue las instrucciones para el tipo de proyecto:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX Interop)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>Requisitos previos

* Para aplicaciones para UWP: instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior.

* En el caso de las aplicaciones de Windows8.1 o Windows Phone 8.1, instala [el SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) con Visual Studio 2015 o Visual Studio 2013.

<span id="interstitialadsxaml10"/>
### <a name="xamlnet"></a>XAML/.NET

En esta sección se proporcionan ejemplos en C#, pero Visual Basic y C++ también son compatibles con proyectos XAML/.NET. Para obtener un ejemplo de código C# completo, consulta [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md).

1. Abre el proyecto en Visual Studio.

2. En **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

  * Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

  * Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

  * Para un proyecto de Windows Phone 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows Phone 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puedes ignorar las bibliotecas de Ad Mediator.

3.  En el archivo de código adecuado de la aplicación (por ejemplo, en MainPage.xaml.cs o un archivo de código para otra página), agrega la siguiente referencia de espacio de nombres.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  En una ubicación adecuada de tu aplicación (por ejemplo, en ```MainPage``` o en otra página), declara un objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) y varios campos de cadenas que representen el id. de aplicación y el id. de unidad del anuncio intersticial. En el siguiente ejemplo de código se asignan los campos `myAppId` y `myAdUnitId` a los valores de prueba para anuncios de vídeo intersticiales proporcionados en [Valores del modo de prueba](test-mode-values.md).

  > [!NOTE]
  > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Tienda, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para los eventos del objeto.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Si quieres mostrar un anuncio de *vídeo intersticial*, aproximadamente entre 30 y 60 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType.Video** para el tipo de anuncio.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

  Si quieres mostrar un anuncio de *banner intersticial* (para aplicaciones para UWP solo), aproximadamente entre 5 y 8 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType.Display** para el tipo de anuncio.

  ```csharp
  myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
  ```

6.  En el punto del código donde quieras mostrar el anuncio de vídeo intersticial o de banner intersticial, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Define los controladores de eventos para el objeto **InterstitialAd**.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadshtml10"/>
### <a name="htmljavascript"></a>HTML/JavaScript

En las siguientes instrucciones se supone que has creado un proyecto de Windows universal para JavaScript en Visual Studio y te diriges a una CPU específica. Para obtener un ejemplo de código completo, consulta [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Abre el proyecto en Visual Studio.

2.  En **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

  * Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para JavaScript** (versión 10.0).

  * Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Microsoft Advertising para Windows 8.1 (JS) nativo**.

  * Para un proyecto de Windows 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Microsoft Advertising para Windows Phone 8.1 (JS) nativo**.

3.  En la sección **&lt;head&gt;** del archivo HTML del proyecto, después de las referencias JavaScript del proyecto de default.css y default.js, agrega la referencia a ad.js. En un proyecto de UWP, agrega la referencia siguiente.

  ``` HTML
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  En un proyecto de Windows 8.1 o Windows Phone 8.1, agrega la referencia siguiente.

  ``` HTML
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  En un archivo .js del proyecto, declara un objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) y varios campos que contengan el id. de aplicación y el id. de unidad para tu anuncio intersticial. En el siguiente ejemplo de código se asignan los campos `applicationId` y `adUnitId` a los valores de prueba para anuncios de vídeo intersticiales proporcionados en [Valores del modo de prueba](test-mode-values.md).

  > [!NOTE]
  > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Tienda, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para el objeto.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Si quieres mostrar un anuncio de *vídeo intersticial*, aproximadamente entre 30 y 60 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **InterstitialAdType.video** para el tipo de anuncio.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

  Si quieres mostrar un anuncio de *banner intersticial* (para aplicaciones para UWP solo), aproximadamente entre 5 y 8 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **InterstitialAdType.display** para el tipo de anuncio.

  ```js
  if (interstitialAd) {
      interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
  }
  ```

6.  En el punto del código donde quieras mostrar el anuncio, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Define los controladores de eventos para el objeto **InterstitialAd**.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadsdirectx10"/>
### <a name="c-directx-interop"></a>C++ (DirectX Interop)

En este ejemplo se supone que has creado un proyecto de **Aplicación XAML y DirectX (Windows universal)** en C++ de Visual Studio y te diriges a una arquitectura de CPU concreta.
 
1. Abre el proyecto en Visual Studio.

1.  En **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

  * Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

  * Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

  * Para un proyecto de Windows Phone 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows Phone 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puedes ignorar las bibliotecas de Ad Mediator.

2.  En un archivo de encabezado apropiado para tu aplicación (por ejemplo, DirectXPage.xaml.h), declara un objeto [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) y los métodos de controlador de eventos relacionados.  

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  En el mismo archivo de encabezado, declara varios campos de cadena que representen el id. de aplicación y el id. de unidad para tu anuncio intersticial. En el siguiente ejemplo de código se asignan los campos `myAppId` y `myAdUnitId` a los valores de prueba para anuncios de vídeo intersticiales proporcionados en [Valores del modo de prueba](test-mode-values.md).

  > [!NOTE]
  > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Tienda, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  En el archivo .cpp donde quieras agregar código para mostrar un anuncio intersticial, agrega la siguiente referencia de espacio de nombres. En los siguientes ejemplos se supone que agregas el código al archivo DirectXPage.xaml.cpp de tu aplicación.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para los eventos del objeto. En el siguiente ejemplo, ```InterstitialAdSamplesCpp``` es el espacio de nombres para el proyecto; cambia este nombre si es necesario en tu código.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Si quieres mostrar un anuncio de *vídeo intersticial*, aproximadamente entre 30 y 60 segundos antes de que necesites el anuncio intersticial, usa el método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType::Video** para el tipo de anuncio.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

  Si quieres mostrar un anuncio de *banner intersticial* (para aplicaciones para UWP solo), aproximadamente entre 5 y 8 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType::Display** para el tipo de anuncio.

  ```cpp
  m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
  ```

7.  En el punto del código donde quieras mostrar el anuncio, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Define los controladores de eventos para el objeto **InterstitialAd**.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publicar la aplicación con anuncios dinámicos mediante el Centro de desarrollo de Windows

1.  En el panel del Centro de desarrollo, ve a la página [Monetizar con anuncios](../publish/monetize-with-ads.md) correspondiente a la aplicación y [crea una unidad de anuncios](../monetize/set-up-ad-units-in-your-app.md). Para el tipo de unidad de anuncios, elige **Vídeo intersticial** o **Banner intersticial**, según el tipo de anuncio intersticial que muestres. Anota el id. de la unidad de anuncios y el id. de la aplicación.

2. Si la aplicación es una aplicación para UWP para Windows10, tienes la opción de habilitar la mediación de anuncios para **InterstitialAd** mediante la configuración de las opciones en la sección [Mediación de anuncios](../publish/monetize-with-ads.md#mediation) de la página [Monetizar con anuncios](../publish/monetize-with-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft.

3.  En el código, reemplaza los valores de unidades de anuncios de prueba con los valores dinámicos generados en el Centro de desarrollo.

4.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda mediante el panel del Centro de desarrollo de Windows.

5.  Revisa los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios intersticiales en tu aplicación

Puedes usar varios controles **InterstitialAd** en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/monetize-with-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>Directivas y procedimientos recomendados para anuncios intersticiales


Para obtener más información sobre cómo usar anuncios intersticiales de forma eficaz y las directivas que se deben seguir, consulta [Directivas y procedimientos recomendados para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>Quitar los errores de referencia: dirigirse a una plataforma de CPU específica (XAML y HTML)


Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, es posible que veas una advertencia en el proyecto después de agregar una referencia a las bibliotecas de Microsoft Advertising. Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Para obtener más información, consulta [Problemas conocidos](known-issues-for-the-advertising-libraries.md).

## <a name="related-topics"></a>Temas relacionados


* [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md)
* [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Muestras de publicidad en GitHub](http://aka.ms/githubads)
* [Configurar unidades de anuncios para la aplicación](../monetize/set-up-ad-units-in-your-app.md)

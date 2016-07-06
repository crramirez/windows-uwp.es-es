---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Aprende a incluir anuncios intersticiales en una aplicación de Windows 10, Windows 8.1 o Windows Phone 8.1 con las bibliotecas de Microsoft Advertising en el SDK Microsoft Store Engagement and Monetization."
title: Anuncios intersticiales
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0f159409bb584aacaf66550efe8d147cd8fddd50

---

# Anuncios intersticiales


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tutorial te muestra cómo incluir anuncios intersticiales en una aplicación de Windows 10, Windows 8.1 o Windows Phone 8.1 con las bibliotecas de Microsoft Advertising en el SDK Microsoft Store Engagement and Monetization.

Para obtener proyectos de muestra completos que muestran cómo agregar anuncios intersticiales a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## ¿Qué son los anuncios intersticiales?

A diferencia de los banners, los anuncios intersticiales **se muestran en toda la pantalla de la aplicación. Con frecuencia se usan dos formas básicas en los juegos.

* Con anuncios *Paywall*, el usuario debe ver un anuncio en algún intervalo periódico. Por ejemplo, entre niveles del juego:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Con los anuncios *Rewards Based*, el usuario busca alguna ventaja de manera explícita, como una sugerencia o tiempo extra para completar el nivel, e inicializa el anuncio de vídeo a través de la interfaz de usuario de la aplicación.

    Es importante tener en cuenta que este SDK no controla nada de la interfaz de usuario, excepto durante la reproducción del vídeo. Consulta las [prácticas recomendadas para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obtener instrucciones sobre qué hacer y qué evitar cuando pienses en cómo integrar anuncios intersticiales en la aplicación.

## Crear una aplicación con anuncios intersticiales


### Requisitos previos

1.  Instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) con Visual Studio 2015 o Visual Studio 2013.

2.  En Visual Studio, abre el proyecto o crea uno nuevo.

### Programación de código

* [Pasos para una aplicación de XAML/.NET](#interstitialadsxaml10)

* [Pasos para HTML/JavaScript](#interstitialadshtml10)

* [Pasos para C++ (interoperabilidad de DirectX)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### Anuncios intersticiales (XAML/.NET)

> **Nota** Esta sección proporciona ejemplos de C#, pero Visual Basic y C++ también son compatibles.
 
1. Abre el proyecto en Visual Studio.
2. En **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

    -   Para un proyecto de Windows Phone 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows Phone 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

3.  En el código de la aplicación, incluye la siguiente referencia al espacio de nombres.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  Declara las propiedades `MyAppId` y `MyAdUnitId`.

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **Nota** Reemplazarás los valores de prueba con valores dinámicos antes de enviar tu aplicación para su envío.

5.  Crea una instancia de un [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), conecta todos los controladores de eventos y solicita un anuncio.

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  En el punto del código donde debe aparecer el anuncio, asegúrate de que el anuncio esté listo y muéstralo.

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  Define e incluye el código de los eventos.

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  Asigna la propiedad `MyAppId` para el valor de prueba proporcionado en [Test mode values](test-mode-values.md) (Valores de modo de prueba). Este valor se usa solo para realizar pruebas; lo reemplazarás con un valor dinámico antes de publicar la aplicación.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Asigna la propiedad `MyAdUnitId` para el valor de prueba proporcionado en [Test mode values](test-mode-values.md) (Valores de modo de prueba). Este valor se usa solo para realizar pruebas; lo reemplazarás con un valor dinámico antes de publicar la aplicación.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadshtml10"/>
### Anuncios intersticiales (HTML/JavaScript)

En este ejemplo se supone que has creado un proyecto de aplicación universal para JavaScript en Visual Studio de 2015 y te diriges a una CPU específica.

1. Abre el proyecto en Visual Studio.
2.  En **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para JavaScript** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Microsoft Advertising para Windows 8.1 (JS) nativo**.

    -   Para un proyecto de Windows 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Microsoft Advertising para Windows Phone 8.1 (JS) nativo**.

3.  En el código HTML, incluye la siguiente referencia de script.

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Declara las propiedades `myAppId` y `myAdUnitId`.

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  Crea una instancia de un **InterstitialAd**, conecta todos los controladores de eventos y solicita un anuncio.

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  En el punto del código donde debe aparecer el anuncio, asegúrate de que el anuncio esté listo y muéstralo.

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  Define e incluye el código de los eventos.

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  Asigna la propiedad `MyAppId` para el valor de prueba proporcionado en [Test mode values](test-mode-values.md) (Valores de modo de prueba). Este valor se usa solo para realizar pruebas; lo reemplazarás con un valor dinámico antes de publicar la aplicación.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  Asigna la propiedad `MyAdUnitId` para el valor de prueba proporcionado en [Test mode values](test-mode-values.md) (Valores de modo de prueba). Este valor se usa solo para realizar pruebas; lo reemplazarás con un valor dinámico antes de publicar la aplicación.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadsdirectx10"/>
### Anuncios intersticiales (C++ e interoperabilidad de DirectX con XAML)

En este ejemplo se supone que has creado un proyecto de aplicación universal para XAML en Visual Studio de 2015 y te diriges a una arquitectura de CPU concreta.

> **Importante** Este código está escrito en C++ según lo necesario para DirectX.

 
1. Abre el proyecto en Visual Studio.
1.  En **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

    -   Para un proyecto de Windows Phone 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows Phone 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

2.  En el archivo de encabezados apropiado para tu aplicación, declara el objeto de anuncio intersticial y las propiedades o métodos relacionados.

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  Declara las propiedades `AppId` y `AdUnitId`.

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  En el archivo .cpp, agrega una referencia al espacio de nombres.

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  Crea una instancia de un **InterstitialAd**, conecta todos los controladores de eventos y solicita un anuncio.

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  En el punto del código donde debe aparecer el anuncio, asegúrate de que el anuncio esté listo y muéstralo.

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  Define e incluye el código de los eventos.

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  Asigna la propiedad `AppId` para el valor de prueba proporcionado en [Test mode values](test-mode-values.md) (Valores de modo de prueba). Este valor se usa solo para realizar pruebas; lo reemplazarás con un valor dinámico antes de publicar la aplicación.

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Asigna la propiedad `AdUnitId` para el valor de prueba proporcionado en [Test mode values](test-mode-values.md) (Valores de modo de prueba). Este valor se usa solo para realizar pruebas; lo reemplazarás con un valor dinámico antes de publicar la aplicación.

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

### Publicar la aplicación con anuncios dinámicos mediante el Centro de desarrollo de Windows

1.  En el panel del Centro de desarrollo, ve a la página **Monetización**&gt;**Monetizar con anuncios** de la aplicación y [crea una unidad de Microsoft Advertising independiente](../publish/monetize-with-ads.md). Especifica **Vídeo intersticial** para el tipo de unidad de anuncio. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2.  En el código, reemplaza los valores de unidades de anuncios de prueba con los valores dinámicos generados en el Centro de desarrollo.

3.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda mediante el panel del Centro de desarrollo de Windows.

4.  Revisa los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

<span id="interstitialbestpractices10"/>
## Procedimientos recomendados para anuncios intersticiales


Para obtener más información sobre cómo usar anuncios intersticiales de forma eficaz, consulta el tema [UI and user experience guidelines](ui-and-user-experience-guidelines.md) (Directrices para la experiencia de UI y de usuario).

<span id="targetplatform10"/>
## Quitar los errores de referencia: dirigirse a una plataforma de CPU específica (XAML y HTML)


Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, es posible que veas una advertencia en el proyecto después de agregar una referencia a las bibliotecas de Microsoft Advertising. Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Para obtener más información, consulta [Problemas conocidos](known-issues-for-the-advertising-libraries.md).

## Temas relacionados


* [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md)
* [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->



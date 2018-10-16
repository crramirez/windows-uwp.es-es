---
author: Xansky
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Aprende a incluir anuncios intersticiales en una aplicación para UWP para Windows 10 con el SDK de Microsoft Advertising.
title: Anuncios intersticiales
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, control de anuncios, intersticial
ms.localizationpriority: medium
ms.openlocfilehash: 547a582064262d18467df4868df17a08e73b279c
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4617320"
---
# <a name="interstitial-ads"></a>Anuncios intersticiales

En este tutorial se muestra cómo incluir anuncios intersticiales en juegos y aplicaciones de la Plataforma universal de Windows (UWP) para Windows 10. Para obtener proyectos de muestra completos que muestran cómo agregar anuncios intersticiales a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>¿Qué son los anuncios intersticiales?

A diferencia de los anuncios de banner estándar, que están confinados a una porción de la interfaz de usuario en una aplicación o juego, los anuncios intersticiales se muestran en toda la pantalla. Con frecuencia se usan dos formas básicas en los juegos.

* Con anuncios *Paywall*, el usuario debe ver un anuncio en algún intervalo periódico. Por ejemplo, entre niveles del juego:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Con los anuncios *Rewards Based*, el usuario busca explícitamente alguna ventaja, como una sugerencia o tiempo extra para completar el nivel, e inicializa el anuncio a través de la interfaz de usuario de la aplicación.

Ofrecemos dos tipos de anuncios intersticiales para usar en tus aplicaciones y juegos: **anuncios intersticiales en vídeo** y **anuncios de banner intersticiales**.

> [!NOTE]
> La API para anuncios intersticiales no controla la interfaz de usuario excepto en el momento de la reproducción del vídeo. Consulta las [prácticas recomendadas para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obtener instrucciones sobre qué hacer y qué evitar cuando pienses en cómo integrar anuncios intersticiales en la aplicación.

## <a name="prerequisites"></a>Requisitos previos

* Instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior de VisualStudio. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Integrar un anuncio intersticial en la aplicación

Para mostrar anuncios intersticiales en tu aplicación, sigue las instrucciones para el tipo de proyecto:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

En esta sección se proporcionan ejemplos en C#, pero Visual Basic y C++ también son compatibles con proyectos XAML/.NET. Para obtener un ejemplo de código C# completo, consulta [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md).

1. Abre el proyecto en Visual Studio.
    > [!NOTE]
    > Si estás usando un proyecto existente, abre el archivo Package.appxmanifest en el proyecto y asegúrate de que la funcionalidad **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios dinámicos.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega una referencia al SDK de Microsoft Advertising en el proyecto:

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y luego selecciona **Agregar referencia...**
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

3.  En el archivo de código adecuado de la aplicación (por ejemplo, en MainPage.xaml.cs o un archivo de código para otra página), agrega la siguiente referencia de espacio de nombres.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  En una ubicación adecuada de tu aplicación (por ejemplo, en ```MainPage``` o en otra página), declara un objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y varios campos de cadenas que representen el id. de aplicación y el id. de unidad del anuncio intersticial. En el siguiente ejemplo de código, se asignan los campos `myAppId` y `myAdUnitId` a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para anuncios intersticiales.

    > [!NOTE]
    > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Store, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para los eventos del objeto.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Si quieres mostrar un anuncio de *vídeo intersticial*, aproximadamente entre 30 y 60 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType.Video** para el tipo de anuncio.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    Si quieres mostrar un anuncio de *banner intersticial*, aproximadamente entre 5 y 8 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType.Display** para el tipo de anuncio.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  En el punto del código donde quieras mostrar el anuncio de vídeo intersticial o de banner intersticial, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Define los controladores de eventos para el objeto **InterstitialAd**.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

En las siguientes instrucciones se supone que has creado un proyecto de Windows universal para JavaScript en Visual Studio y te diriges a una CPU específica. Para obtener un ejemplo de código completo, consulta [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Abre el proyecto en Visual Studio.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega una referencia al SDK de Microsoft Advertising en el proyecto:

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y luego selecciona **Agregar referencia...**
    2.  En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para JavaScript** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

3.  En la sección **&lt;head&gt;** del archivo HTML del proyecto, después de las referencias JavaScript del proyecto de default.css y default.js, agrega la referencia a ad.js.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  En un archivo .js del proyecto, declara un objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y varios campos que contengan el id. de aplicación y el id. de unidad para tu anuncio intersticial. En el siguiente ejemplo de código, se asignan los campos `applicationId` y `adUnitId` a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para anuncios intersticiales.

    > [!NOTE]
    > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Store, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para el objeto.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Si quieres mostrar un anuncio de *vídeo intersticial*, aproximadamente entre 30 y 60 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **InterstitialAdType.video** para el tipo de anuncio.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    Si quieres mostrar un anuncio de *banner intersticial*, aproximadamente entre 5 y 8 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **InterstitialAdType.display** para el tipo de anuncio.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  En el punto del código donde quieras mostrar el anuncio, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Define los controladores de eventos para el objeto **InterstitialAd**.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (DirectX Interop)

En este ejemplo se supone que has creado un proyecto de **Aplicación XAML y DirectX (Windows universal)** en C++ de Visual Studio y te diriges a una arquitectura de CPU concreta.
 
1. Abre el proyecto en Visual Studio.

3. Agrega una referencia al SDK de Microsoft Advertising en el proyecto:

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y luego selecciona **Agregar referencia...**
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

2.  En un archivo de encabezado apropiado para tu aplicación (por ejemplo, DirectXPage.xaml.h), declara un objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y los métodos de controlador de eventos relacionados.  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  En el mismo archivo de encabezado, declara varios campos de cadena que representen el id. de aplicación y el id. de unidad para tu anuncio intersticial. En el siguiente ejemplo de código, se asignan los campos `myAppId` y `myAdUnitId` a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para anuncios intersticiales.

    > [!NOTE]
    > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Store, debes [reemplazar estos valores de prueba con valores dinámicos](#release) desde el Centro de desarrollo de Windows.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  En el archivo .cpp donde quieras agregar código para mostrar un anuncio intersticial, agrega la siguiente referencia de espacio de nombres. En los siguientes ejemplos se supone que agregas el código al archivo DirectXPage.xaml.cpp de tu aplicación.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para los eventos del objeto. En el siguiente ejemplo, ```InterstitialAdSamplesCpp``` es el espacio de nombres para el proyecto; cambia este nombre si es necesario en tu código.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Si quieres mostrar un anuncio de *vídeo intersticial*, aproximadamente entre 30 y 60 segundos antes de que necesites el anuncio intersticial, usa el método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType::Video** para el tipo de anuncio.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    Si quieres mostrar un anuncio de *banner intersticial*, aproximadamente entre 5 y 8 segundos antes de que necesites el anuncio, usa el método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para la captura previa del anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrate de especificar **AdType::Display** para el tipo de anuncio.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  En el punto del código donde quieras mostrar el anuncio, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Define los controladores de eventos para el objeto **InterstitialAd**.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publicar tu aplicación con anuncios dinámicos

1. Asegúrate de que el uso de anuncios intersticiales en tu aplicación sigue nuestras [directrices para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

2.  En el panel del Centro de desarrollo, ve a la página [Anuncios desde la aplicación](../publish/in-app-ads.md) y [crea una unidad de anuncios](set-up-ad-units-in-your-app.md#live-ad-units). Para el tipo de unidad de anuncios, elige **Vídeo intersticial** o **Banner intersticial**, según el tipo de anuncio intersticial que muestres. Anota el id. de la unidad de anuncios y el id. de la aplicación.
    > [!NOTE]
    > Los valores de id. de la aplicación para unidades de anuncios de prueba y unidades de anuncios dinámicos de UWP tienen diferentes formatos. Los valores de id. de la aplicación de prueba son GUID. Al crear una unidad de anuncio dinámica para UWP en el panel de información, el valor de id. de la aplicación de la unidad de anuncios siempre coincide con el id. de Microsoft Store de la aplicación (un valor de id. de Microsoft Store de muestra se parece a 9NBLGGH4R315).

3. Tienes la opción de habilitar la mediación de anuncios para el **InterstitialAd** mediante la configuración de las opciones de la sección [Configuración de la mediación](../publish/in-app-ads.md#mediation) de la página [Anuncios desde la aplicación](../publish/in-app-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft.

4.  En el código, reemplaza los valores de unidades de anuncios de prueba con los valores dinámicos generados en el Centro de desarrollo.

5.  [Envía la aplicación](../publish/app-submissions.md) a la Store mediante el panel del Centro de desarrollo de Windows.

6.  Revisa los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios intersticiales en tu aplicación

Puedes usar varios controles **InterstitialAd** en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md)
* [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Muestras de publicidad en GitHub](http://aka.ms/githubads)
* [Configurar unidades de anuncios para la aplicación](set-up-ad-units-in-your-app.md)

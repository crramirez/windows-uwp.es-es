---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Obtenga información sobre cómo incluir anuncios intersticiales en una aplicación para UWP para Windows 10 mediante el SDK de Microsoft Advertising.
title: Anuncios intersticiales
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, control de anuncios, intersticial
ms.localizationpriority: medium
ms.openlocfilehash: eedb3f12bfc9da51afb2d5205122cbd42d7b31bf
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363098"
---
# <a name="interstitial-ads"></a>Anuncios intersticiales

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tutorial se muestra cómo incluir anuncios intersticiales en aplicaciones de Plataforma universal de Windows (UWP) y juegos para Windows 10. Para obtener proyectos de muestra completos que muestran cómo agregar anuncios intersticiales a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>¿Qué son los anuncios intersticiales?

A diferencia de los anuncios de banner estándar, que se limitan a una parte de la interfaz de usuario de una aplicación o un juego, los anuncios intersticiales se muestran en toda la pantalla. Con frecuencia se usan dos formas básicas en los juegos.

* Con anuncios *Paywall*, el usuario debe ver un anuncio en algún intervalo periódico. Por ejemplo, entre niveles del juego:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Con los anuncios *basados en premios* , el usuario está buscando explícitamente alguna ventaja, como una sugerencia o un tiempo adicional para completar el nivel, e inicializa el anuncio a través de la interfaz de usuario de la aplicación.

Ofrecemos dos tipos de anuncios intersticiales para usarlos en sus aplicaciones y juegos: anuncios de **vídeo intersticiales** y **anuncios de banners intersticiales**.

> [!NOTE]
> La API para anuncios intersticiales no controla ninguna interfaz de usuario, excepto en el momento de la reproducción del vídeo. Consulta las [prácticas recomendadas para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10) para obtener instrucciones sobre qué hacer y qué evitar cuando pienses en cómo integrar anuncios intersticiales en la aplicación.

## <a name="prerequisites"></a>Requisitos previos

* Instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) con visual Studio 2015 o una versión posterior de Visual Studio. Para obtener instrucciones de instalación, consulte [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Integrar un anuncio intersticial en la aplicación

Para mostrar anuncios intersticiales en tu aplicación, sigue las instrucciones para el tipo de proyecto:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

En esta sección se proporcionan ejemplos en C#, pero Visual Basic y C++ también son compatibles con proyectos XAML/.NET. Para obtener un ejemplo de código C# completo, consulta [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md).

1. Abra el proyecto en Visual Studio.
    > [!NOTE]
    > Si utiliza un proyecto existente, abra el archivo package. appxmanifest en el proyecto y asegúrese de que la opción **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios en directo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agregue una referencia al SDK de Microsoft Advertising en el proyecto:

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

3.  En el archivo de código adecuado de la aplicación (por ejemplo, en MainPage.xaml.cs o un archivo de código para otra página), agrega la siguiente referencia de espacio de nombres.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet1":::

4.  En una ubicación adecuada de tu aplicación (por ejemplo, en ```MainPage``` o en otra página), declara un objeto [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y varios campos de cadenas que representen el id. de aplicación y el id. de unidad del anuncio intersticial. En el ejemplo de código siguiente se asignan los `myAppId` `myAdUnitId` campos y a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) de los anuncios intersticiales.

    > [!NOTE]
    > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control, y cada unidad de ad consta de un identificador de *unidad de ad* y un identificador de *aplicación*. En estos pasos, asignará el identificador de unidad de ad de prueba y los valores de ID. de aplicación al control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debe [reemplazar estos valores de prueba con valores dinámicos](#release) del centro de Partners.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet2":::

5.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para los eventos del objeto.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet3":::

6.  Si desea mostrar un anuncio de *vídeo intersticial* : aproximadamente 30-60 segundos antes de necesitar el anuncio, use el método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para obtener previamente el anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrese de especificar **AdType. video** para el tipo de anuncio.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet4":::

    Si desea mostrar un anuncio de *banner intersticial* : aproximadamente 5-8 segundos antes de que se necesite el anuncio, use el método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para obtener previamente el anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrese de especificar **AdType. display** para el tipo de anuncio.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  En el punto del código en el que desea mostrar el anuncio de banner intersticial o de vídeo intersticial, confirme que el método **InterstitialAd** está listo para mostrarse y, a continuación, se muestra mediante el método [Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) .

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet5":::

7.  Define los controladores de eventos para el objeto **InterstitialAd**.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet6":::

8.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

En las siguientes instrucciones se supone que ha creado un proyecto universal de Windows para JavaScript en Visual Studio y que tiene como destino una CPU específica. Para obtener un ejemplo de código completo, consulta [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Abra el proyecto en Visual Studio.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agregue una referencia al SDK de Microsoft Advertising en el proyecto:

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2.  En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for JavaScript** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

3.  En la sección de ** &lt; encabezado &gt; ** del archivo HTML del proyecto, después de las referencias JavaScript del proyecto de default. CSS y default.js, agregue la referencia a ad.js.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  En un archivo .js del proyecto, declara un objeto [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y varios campos que contengan el id. de aplicación y el id. de unidad para tu anuncio intersticial. En el ejemplo de código siguiente se asignan los `applicationId` `adUnitId` campos y a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) de los anuncios intersticiales.

    > [!NOTE]
    > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control, y cada unidad de ad consta de un identificador de *unidad de ad* y un identificador de *aplicación*. En estos pasos, asignará el identificador de unidad de ad de prueba y los valores de ID. de aplicación al control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debe [reemplazar estos valores de prueba con valores dinámicos](#release) del centro de Partners.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet1":::

5.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para el objeto.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet2":::

5. Si desea mostrar un anuncio de *vídeo intersticial* : aproximadamente 30-60 segundos antes de necesitar el anuncio, use el método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para obtener previamente el anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrese de especificar **InterstitialAdType. video** para el tipo de anuncio.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet3":::

    Si desea mostrar un anuncio de *banner intersticial* : aproximadamente 5-8 segundos antes de que se necesite el anuncio, use el método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para obtener previamente el anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrese de especificar **InterstitialAdType. display** para el tipo de anuncio.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  En el punto del código donde quieras mostrar el anuncio, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet4":::

7.  Define los controladores de eventos para el objeto **InterstitialAd**.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet5":::

9.  Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (DirectX Interop)

En este ejemplo se supone que ha creado un proyecto de **aplicación XAML y DirectX de C++ (Windows universal)** en Visual Studio y que tiene como destino una arquitectura de CPU específica.
 
1. Abra el proyecto en Visual Studio.

3. Agregue una referencia al SDK de Microsoft Advertising en el proyecto:

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

2.  En un archivo de encabezado apropiado para tu aplicación (por ejemplo, DirectXPage.xaml.h), declara un objeto [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y los métodos de controlador de eventos relacionados.  

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet1":::

3.  En el mismo archivo de encabezado, declara varios campos de cadena que representen el id. de aplicación y el id. de unidad para tu anuncio intersticial. En el ejemplo de código siguiente se asignan los `myAppId` `myAdUnitId` campos y a los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) de los anuncios intersticiales.

    > [!NOTE]
    > Cada **InterstitialAd** tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control, y cada unidad de ad consta de un identificador de *unidad de ad* y un identificador de *aplicación*. En estos pasos, asignará el identificador de unidad de ad de prueba y los valores de ID. de aplicación al control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debe [reemplazar estos valores de prueba con valores dinámicos](#release) del centro de Partners.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet2":::

4.  En el archivo .cpp donde quieras agregar código para mostrar un anuncio intersticial, agrega la siguiente referencia de espacio de nombres. En los siguientes ejemplos se supone que agregas el código al archivo DirectXPage.xaml.cpp de tu aplicación.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet3":::

6.  En el código que se ejecuta en el inicio (por ejemplo, en el constructor de la página), crea una instancia del objeto **InterstitialAd** y conecta controladores de eventos para los eventos del objeto. En el siguiente ejemplo, ```InterstitialAdSamplesCpp``` es el espacio de nombres para el proyecto; cambia este nombre si es necesario en tu código.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet4":::

7. Si desea mostrar un anuncio de *vídeo intersticial* : aproximadamente 30-60 segundos antes de necesitar el anuncio intersticial, use el método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para obtener previamente el anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrese de especificar **AdType:: video** para el tipo de anuncio.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet5":::

    Si desea mostrar un anuncio de *banner intersticial* : aproximadamente 5-8 segundos antes de que se necesite el anuncio, use el método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) para obtener previamente el anuncio. Esto deja tiempo suficiente para solicitar y preparar el anuncio antes de que deba mostrarse. Asegúrese de especificar **AdType::D** Mostrar para el tipo de anuncio.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  En el punto del código donde quieras mostrar el anuncio, confirma que **InterstitialAd** está listo para mostrarse y, después, muéstralo con el método [Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet6":::

8.  Define los controladores de eventos para el objeto **InterstitialAd**.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet7":::

9. Compila y prueba la aplicación para confirmar que muestra los anuncios de prueba.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publique su aplicación con anuncios en vivo

1. Asegúrate de que el uso de anuncios intersticiales en tu aplicación siga nuestras [directrices para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

2.  En el centro de Partners, vaya a la página [anuncios en la aplicación](../publish/in-app-ads.md) y [cree una unidad de anuncio](set-up-ad-units-in-your-app.md#live-ad-units). Para el tipo de unidad de ad, elija **vídeo intersticial** o **banner intersticial**, en función del tipo de anuncio intersticial que se muestre. Anota el identificador de unidad de anuncio y el identificador de la aplicación.
    > [!NOTE]
    > Los valores de ID. de aplicación para las unidades de anuncios de prueba y las unidades de anuncio de UWP en directo tienen distintos formatos. Los valores de ID. de aplicación de prueba son GUID. Cuando se crea una unidad de ad de UWP activa en el centro de Partners, el valor de identificador de la aplicación de la unidad de anuncio coincide siempre con el identificador de almacén de la aplicación (un valor de identificador de almacén de ejemplo es similar a 9NBLGGH4R315).

3. Opcionalmente, puede habilitar la mediación de anuncios para **InterstitialAd** mediante la configuración de los valores de la sección [configuración de mediación](../publish/in-app-ads.md#mediation) en la página [ADS en la aplicación](../publish/in-app-ads.md) . La mediación de anuncios permite maximizar las funcionalidades de promoción de ingresos y de las aplicaciones de anuncios mediante la visualización de anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago como Taboola y Smaato y los anuncios de las campañas de promoción de aplicaciones de Microsoft.

4.  En el código, reemplace los valores de unidad de ad de prueba por los valores activos generados en el centro de Partners.

5.  [Envíe la aplicación](../publish/app-submissions.md) a la tienda mediante el centro de Partners.

6.  Revise los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de Partners.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios intersticiales en la aplicación

Puede usar varios controles **InterstitialAd** en una sola aplicación. En este escenario, se recomienda asignar una unidad de anuncio diferente a cada control. El uso de diferentes unidades de anuncio para cada control le permite [configurar por separado los valores de mediación](../publish/in-app-ads.md#mediation) y obtener [datos de informes](../publish/advertising-performance-report.md) discretos para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que sirven a su aplicación.

> [!IMPORTANT]
> Puede usar cada unidad de anuncio en una sola aplicación. Si usa una unidad de anuncio en más de una aplicación, los anuncios no se servirán para esa unidad de ad.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios intersticiales](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Código de ejemplo de anuncios intersticiales en C#](interstitial-ad-sample-code-in-c.md)
* [Código de ejemplo de anuncios intersticiales en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurar unidades de anuncio para la aplicación](set-up-ad-units-in-your-app.md)

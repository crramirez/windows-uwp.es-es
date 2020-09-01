---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Obtenga información sobre cómo usar la clase AdControl para mostrar anuncios de banner en una aplicación JavaScript/HTML para Windows 10 (UWP).
title: AdControl en HTML 5 y JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Advertising, AdControl, ad control, JavaScript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: d770e8a9a15835d7fab52e7383acca3a3df6c6cd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155679"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 y JavaScript

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tutorial se muestra cómo usar la clase [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar los anuncios de banner en una aplicación JavaScript/HTML de plataforma universal de Windows (UWP) para Windows 10.

Para obtener un ejemplo de proyecto completo que muestre cómo agregar anuncios en banner a una aplicación JavaScript o HTML, consulta los [ejemplos de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Requisitos previos

* Instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) con visual Studio 2015 o una versión posterior de Visual Studio. Para obtener instrucciones de instalación, consulte [este artículo](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Si ha instalado la versión 10.0.14393 del SDK de Windows 10 (actualización de aniversario) o una versión posterior de la Windows SDK, también debe instalar la biblioteca [WinJS](https://github.com/winjs/winjs) . Esta biblioteca solía incluirse en versiones anteriores de la Windows SDK para Windows 10, pero a partir de la versión 10.0.14393 del SDK de Windows 10 (actualización de aniversario) esta biblioteca debe instalarse por separado. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Integración de un anuncio de banner en la aplicación

1. En Visual Studio, abre el proyecto o crea uno nuevo.

    > [!NOTE]
    > Si utiliza un proyecto existente, abra el archivo package. appxmanifest en el proyecto y asegúrese de que la opción **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios en directo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agregue una referencia al SDK de Microsoft Advertising en el proyecto:

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2.  En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for JavaScript** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

6.  Abre el archivo index.html (u otro archivo html adecuado para el proyecto).

7.  En la sección ** &lt; principal &gt; ** , después de las referencias JavaScript del proyecto de default. CSS y main.js, agregue la referencia a ad.js.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Esta línea se debe colocar en la sección ** &lt; principal &gt; ** después de la inclusión de main.js; de lo contrario, se producirá un error al compilar el proyecto.

8.  Modifique la sección de ** &lt; cuerpo &gt; ** del archivo de default.html (u otro archivo HTML según corresponda para el proyecto) para incluir el elemento **div** para **AdControl**. Asigne las propiedades **ApplicationID** y **AdUnitId** de **AdControl** a los [valores de unidad de ad de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Ajuste también el **alto** y el **ancho** para que el control sea uno de los [tamaños de anuncio admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control, y cada unidad de ad está formada por un *identificador de unidad de ad* y un identificador de *aplicación*. En estos pasos, asignará el identificador de unidad de ad de prueba y los valores de ID. de aplicación al control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debe [reemplazar estos valores de prueba con valores dinámicos](#release) del centro de Partners.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compila y ejecuta la aplicación para verla con un anuncio.

En el ejemplo siguiente se muestra el index.htmcompleto l para una aplicación sencilla.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Crear un control AdControl mediante programación en JavaScript

En los pasos anteriores se muestra cómo declarar un **AdControl** en el marcado HTML. Como alternativa, puede crear mediante programación un **control** de la con JavaScript. En este ejemplo se da por supuesto que usa un **div** existente en el código HTML con el identificador **myAd**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

En este ejemplo se supone que ya has declarado los métodos del controlador de eventos llamados **myAdError**, **myAdRefreshed** y **myAdEngagedChanged**.

Si usas este código y no ves anuncios, puedes intentar insertar un atributo **position:relative** en **div** que contenga **AdControl**. Esto invalidará la configuración predeterminada de **IFrame**. Los anuncios se mostrarán correctamente, a menos que no se muestren debido al valor de este atributo. Ten en cuenta que es posible que las nuevas unidades de anuncios no estén disponibles hasta 30 minutos.

> [!NOTE]
> Los valores *ApplicationID* y *adUnitId* mostrados en este ejemplo son valores del [modo de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Debe [reemplazar estos valores por los valores activos](set-up-ad-units-in-your-app.md#live-ad-units) del centro de Partners antes de enviar la aplicación para su envío.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publique su aplicación con anuncios en vivo

1. Asegúrate de que el uso de anuncios de banner en tu aplicación siga nuestras [directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

1.  En el centro de Partners, vaya a la página [anuncios en la aplicación](../publish/in-app-ads.md) y [cree una unidad de anuncio](set-up-ad-units-in-your-app.md#live-ad-units). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.
    > [!NOTE]
    > Los valores de ID. de aplicación para las unidades de anuncios de prueba y las unidades de anuncio de UWP en directo tienen distintos formatos. Los valores de ID. de aplicación de prueba son GUID. Cuando se crea una unidad de ad de UWP activa en el centro de Partners, el valor de identificador de la aplicación de la unidad de anuncio coincide siempre con el identificador de almacén de la aplicación (un valor de identificador de almacén de ejemplo es similar a 9NBLGGH4R315).

2. Opcionalmente, puede habilitar la mediación de anuncios para **AdControl** si configura los valores en la sección [configuración de mediación](../publish/in-app-ads.md#mediation) en la página [ADS en la aplicación](../publish/in-app-ads.md) . La mediación de anuncios permite maximizar las funcionalidades de promoción de ingresos y de las aplicaciones de anuncios mediante la visualización de anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago como Taboola y Smaato y los anuncios de las campañas de promoción de aplicaciones de Microsoft.

3.  En el código, reemplace los valores de unidad de ad de prueba (**ApplicationID** y **adUnitId**) por los valores activos generados en el centro de Partners.

4.  [Envíe la aplicación](../publish/app-submissions.md) a la tienda mediante el centro de Partners.

5.  Revise los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de Partners.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de AD en la aplicación

Puede usar varios objetos de **AdControl** en una sola aplicación (por ejemplo, cada página de la aplicación podría hospedar un objeto de **AdControl** diferente). En este escenario, se recomienda asignar una unidad de anuncio diferente a cada control. El uso de diferentes unidades de anuncio para cada control le permite [configurar por separado los valores de mediación](../publish/in-app-ads.md#mediation) y obtener [datos de informes](../publish/advertising-performance-report.md) discretos para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que sirven a su aplicación.

> [!IMPORTANT]
> Puede usar cada unidad de anuncio en una sola aplicación. Si usa una unidad de anuncio en más de una aplicación, los anuncios no se servirán para esa unidad de ad.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurar unidades de anuncio para la aplicación](set-up-ad-units-in-your-app.md)
* [Tutorial de control de errores en JavaScript](error-handling-in-javascript-walkthrough.md)
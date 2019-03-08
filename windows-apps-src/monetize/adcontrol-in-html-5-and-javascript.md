---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Aprende a usar la clase AdControl para mostrar anuncios de banner en una aplicación JavaScript/HTML para Windows 10 (UWP).
title: AdControl en HTML 5 y JavaScript
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, control de anuncios, javaScript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: 556493ffc901021310036cfb417c3c3b815c529e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611010"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 y JavaScript

En este tutorial se muestra cómo usar la clase [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar anuncios de banner en una aplicación para Plataforma universal de Windows (UWP) JavaScript/HTML para Windows 10.

Para obtener un ejemplo de proyecto completo que muestre cómo agregar anuncios en banner a una aplicación JavaScript o HTML, consulta los [ejemplos de publicidad en GitHub](https://aka.ms/githubads).

## <a name="prerequisites"></a>Requisitos previos

* Instala el [SDK de Microsoft Advertising](https://aka.ms/ads-sdk-uwp) con Visual Studio 2015 o una versión posterior de Visual Studio. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Si ha instalado la versión de SDK de Windows 10 10.0.14393 (actualización de aniversario) o una versión posterior del SDK de Windows, debe instalar también la [WinJS](https://github.com/winjs/winjs) biblioteca. Antes, esta biblioteca estaba incluida en versiones anteriores de Windows SDK para Windows 10, pero desde la versión 10.0.14393 del SDK de Windows 10 (Actualización de aniversario), debe instalarse por separado. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Integrar un anuncio de banner en la aplicación

1. En Visual Studio, abre el proyecto o crea uno nuevo.

    > [!NOTE]
    > Si estás usando un proyecto existente, abre el archivo Package.appxmanifest en el proyecto y asegúrate de que la funcionalidad **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios dinámicos.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega una referencia al SDK de Microsoft Advertising en el proyecto:

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**
    2.  En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for JavaScript** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

6.  Abre el archivo index.html (u otro archivo html adecuado para el proyecto).

7.  En la sección **&lt;head&gt;**, después de las referencias de JavaScript del proyecto de default.css y main.js, agrega la referencia a ad.js.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Esta línea debe colocarse en la sección **&lt;head&gt;** después de incluir main.js. De lo contrario, se producirá un error al compilar el proyecto.

8.  Modifica la sección **&lt;body&gt;** en el archivo default.html (u otro archivo html adecuado para el proyecto) para que incluya el elemento **div** para **AdControl**. Asigna las propiedades **applicationId** y **adUnitId** de **AdControl** a los [valores de unidad de anuncios de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Ajusta también el **alto** y el **ancho** para que el control sea uno de los [tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en el Store, primero debe [reemplazar estos valores con los valores en vivo de prueba](#release) desde el centro de partners.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compila y ejecuta la aplicación para verla con un anuncio.

El siguiente ejemplo muestra el archivo index.html completo para una aplicación sencilla.

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

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Crear un objeto AdControl mediante programación en Javascript

En los pasos anteriores se muestra cómo declarar un **AdControl** en el marcado HTML. También puedes crear mediante programación un objeto **AdControl** con JavaScript. En este ejemplo se da por hecho que estás usando un valor **div** existente en el código HTML con el identificador **myAd**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

En este ejemplo se supone que ya has declarado los métodos del controlador de eventos llamados **myAdError**, **myAdRefreshed** y **myAdEngagedChanged**.

Si usas este código y no ves anuncios, puedes intentar insertar un atributo **position:relative** en **div** que contenga **AdControl**. Esto invalidará la configuración predeterminada de **IFrame**. Los anuncios se mostrarán correctamente, a menos que no se muestren debido al valor de este atributo. Ten en cuenta que es posible que las nuevas unidades de anuncios no estén disponibles hasta 30 minutos.

> [!NOTE]
> Los valores de *applicationId* y *adUnitId* que se muestran en este ejemplo son [valores de modo de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Debe [reemplazar estos valores con los valores en vivo](set-up-ad-units-in-your-app.md#live-ad-units) desde el centro de partners antes de enviar la aplicación para enviarla.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publicar tu aplicación con anuncios dinámicos

1. Asegúrate de que el uso de anuncios de banner en tu aplicación sigue nuestras [directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

1.  En el centro de partners, vaya a la [anuncios en la aplicación](../publish/in-app-ads.md) página y [crear una unidad de anuncio](set-up-ad-units-in-your-app.md#live-ad-units). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.
    > [!NOTE]
    > Los valores de id. de la aplicación para unidades de anuncios de prueba y unidades de anuncios dinámicas de UWP tienen diferentes formatos. Los valores de id. de la aplicación de prueba son GUID. Cuando se crea una unidad de anuncio UWP en vivo en el centro de partners, el valor de identificador de aplicación para la unidad de ad siempre coincide con el identificador de Store para su aplicación (un valor de Id. de Store de ejemplo similar a 9NBLGGH4R315).

2. También tienes la opción de habilitar la mediación de anuncios para el **AdControl** mediante la configuración de las opciones de la sección [Configuración de la mediación](../publish/in-app-ads.md#mediation) de la página [Anuncios desde la aplicación](../publish/in-app-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft.

3.  En el código, reemplace los valores de unidad de prueba de ad (**applicationId** y **adUnitId**) con los valores en vivo que generó en el centro de partners.

4.  [Enviar la aplicación](../publish/app-submissions.md) para el Store mediante el centro de partners.

5.  Revise su [publicidad informes de rendimiento](../publish/advertising-performance-report.md) en el centro de partners.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios en tu aplicación

Puedes usar varios objetos **AdControl** en una sola aplicación (por ejemplo, cada página de la aplicación puede hospedar un objeto **AdControl** diferente). En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios de pancarta](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Muestras de publicidad en GitHub](https://aka.ms/githubads)
* [Configurar las unidades de anuncios para la aplicación](set-up-ad-units-in-your-app.md)
* [Control de errores en el tutorial de JavaScript](error-handling-in-javascript-walkthrough.md)

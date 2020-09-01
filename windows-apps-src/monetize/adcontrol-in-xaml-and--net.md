---
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Obtenga información sobre cómo usar la clase AdControl para mostrar anuncios de banner en una aplicación XAML para Windows 10 (UWP).
title: AdControl en XAML y .NET
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Advertising, AdControl, ad control, XAML, .net, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 8d1d1e75754bc9f3bed0be862a38ede43d55ad52
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155669"
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl en XAML y .NET

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tutorial se muestra cómo usar la clase [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar los anuncios de banner en una aplicación XAML plataforma universal de Windows (UWP) para Windows 10 que se implementa mediante C#.

> [!NOTE]
> El SDK de Microsoft Advertising también admite aplicaciones XAML que se implementan mediante C++. Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Requisitos previos

* Instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) con visual Studio 2015 o una versión posterior de Visual Studio. Para obtener instrucciones de instalación, consulte [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Integración de un anuncio de banner en la aplicación

1. En Visual Studio, abre el proyecto o crea uno nuevo.

    > [!NOTE]
    > Si utiliza un proyecto existente, abra el archivo package. appxmanifest en el proyecto y asegúrese de que la opción **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios en directo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agregue una referencia al SDK de Microsoft Advertising en el proyecto:

    1. En la ventana de **Explorador de soluciones** , haga clic con el botón derecho en **referencias**y seleccione **Agregar referencia..** .
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for XAML** (versión 10.0).
    3.  En el **Administrador de referencias**, haz clic en Aceptar.

4.  Modifica el código XAML de la página en la que se insertará publicidad para que incluya el espacio de nombres **Microsoft.Advertising.WinRT.UI**. Por ejemplo, en la aplicación de ejemplo predeterminada que genera Visual Studio (denominada, en esta aplicación, MyAdFundedWindows10AppXAML), la página XAML es **MainPage.XAML**.

    La sección **Page** del archivo MainPage.xaml que genera Visual Studio tiene el código siguiente.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    Agrega la referencia de espacio de nombres **Microsoft.Advertising.WinRT.UI** para que la sección **Page** del archivo MainPage.xaml tenga el código siguiente.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. En la etiqueta **Grid**, agrega el código para **AdControl**. Asigne las propiedades  [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) y [ApplicationID](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) a los [valores de unidad de ad de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Ajuste también el **alto** y el **ancho** del control para que sea uno de los [tamaños de anuncio admitidos para los anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control, y cada unidad de ad está formada por un *identificador de unidad de ad* y un identificador de *aplicación*. En estos pasos, asignará el identificador de unidad de ad de prueba y los valores de ID. de aplicación al control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debe [reemplazar estos valores de prueba con valores dinámicos](#release) del centro de Partners.

    La etiqueta **Grid** completa tiene el aspecto de este código.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    El código completo del archivo MainPage.xaml debería tener este aspecto.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Compila y ejecuta la aplicación para verla con un anuncio.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publique su aplicación con anuncios en vivo

1. Asegúrate de que el uso de anuncios de banner en tu aplicación siga nuestras [directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

2.  En el centro de Partners, vaya a la página [anuncios en la aplicación](../publish/in-app-ads.md) y [cree una unidad de anuncio](set-up-ad-units-in-your-app.md#live-ad-units). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.
    > [!NOTE]
    > Los valores de ID. de aplicación para las unidades de anuncios de prueba y las unidades de anuncio de UWP en directo tienen distintos formatos. Los valores de ID. de aplicación de prueba son GUID. Cuando se crea una unidad de ad de UWP activa en el centro de Partners, el valor de identificador de la aplicación de la unidad de anuncio coincide siempre con el identificador de almacén de la aplicación (un valor de identificador de almacén de ejemplo es similar a 9NBLGGH4R315).

3. Opcionalmente, puede habilitar la mediación de anuncios para **AdControl** si configura los valores en la sección [configuración de mediación](../publish/in-app-ads.md#mediation) en la página [ADS en la aplicación](../publish/in-app-ads.md) . La mediación de anuncios permite maximizar las funcionalidades de promoción de ingresos y de las aplicaciones de anuncios mediante la visualización de anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago como Taboola y Smaato y los anuncios de las campañas de promoción de aplicaciones de Microsoft.

4.  En el código, reemplace los valores de unidad de ad de prueba (**ApplicationID** y **AdUnitId**) por los valores activos generados en el centro de Partners.

5.  [Envíe la aplicación](../publish/app-submissions.md) a la tienda mediante el centro de Partners.

6.  Revise los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de Partners.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de AD en la aplicación

Puede usar varios objetos de **AdControl** en una sola aplicación (por ejemplo, cada página de la aplicación podría hospedar un objeto de **AdControl** diferente). En este escenario, se recomienda asignar una unidad de anuncio diferente a cada control. El uso de diferentes unidades de anuncio para cada control le permite [configurar por separado los valores de mediación](../publish/in-app-ads.md#mediation) y obtener [datos de informes](../publish/advertising-performance-report.md) discretos para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que sirven a su aplicación.

> [!IMPORTANT]
> Puede usar cada unidad de anuncio en una sola aplicación. Si usa una unidad de anuncio en más de una aplicación, los anuncios no se servirán para esa unidad de ad.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Tutorial de control de errores en XAML/C#](error-handling-in-xamlc-walkthrough.md).
* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurar unidades de anuncio para la aplicación](set-up-ad-units-in-your-app.md)
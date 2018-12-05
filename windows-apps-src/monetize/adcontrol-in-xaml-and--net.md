---
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Aprende a usar la clase AdControl para mostrar anuncios de banner en una aplicación XAML para Windows10 (UWP).
title: AdControl en XAML y .NET
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, control de anuncios, XAML, .NET, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 161718bb88d7588db345f3de26de2a955ca6620e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8735268"
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl en XAML y .NET


En este tutorial se muestra cómo usar la clase [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar anuncios de banner en una aplicación para Plataforma universal de Windows (UWP) XAML para Windows10 que se implementa mediante C#.

> [!NOTE]
> El SDK de Microsoft Advertising también admite aplicaciones XAML que se implementan mediante C++. Para ver un proyecto de ejemplo completo, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Requisitos previos

* Instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior de VisualStudio. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Integrar un anuncio de banner en la aplicación

1. En Visual Studio, abre el proyecto o crea uno nuevo.

    > [!NOTE]
    > Si estás usando un proyecto existente, abre el archivo Package.appxmanifest en el proyecto y asegúrate de que la funcionalidad **Internet (cliente)** está seleccionada. La aplicación necesita esta funcionalidad para recibir anuncios de prueba y anuncios dinámicos.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Agrega una referencia al SDK de Microsoft Advertising en el proyecto:

    1. Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y luego selecciona **Agregar referencia...**
    2.  En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).
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

5. En la etiqueta **Grid**, agrega el código para **AdControl**. Asigna las propiedades [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) y [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) a los [valores de unidad de anuncios de prueba](set-up-ad-units-in-your-app.md#test-ad-units). Ajusta también el **Alto** y el **Ancho** del control para que se corresponda con uno de los [tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la tienda, debes [reemplazar estos valores de prueba por valores dinámicos](#release) del centro de partners.

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

## <a name="release-your-app-with-live-ads"></a>Publicar tu aplicación con anuncios dinámicos

1. Asegúrate de que el uso de anuncios de banner en tu aplicación sigue nuestras [directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

2.  En el centro de partners, ve a la página de [anuncios en la aplicación](../publish/in-app-ads.md) y [crear una unidad de anuncios](set-up-ad-units-in-your-app.md#live-ad-units). Especifica el tipo de unidad de anuncio **Banner**. Anota el id. de la unidad de anuncios y el id. de la aplicación.
    > [!NOTE]
    > Los valores de id. de la aplicación para unidades de anuncios de prueba y unidades de anuncios dinámicos de UWP tienen diferentes formatos. Los valores de id. de la aplicación de prueba son GUID. Cuando se crea una unidad de anuncios dinámica de UWP en el centro de partners, el valor de Id. de aplicación de la unidad de anuncios siempre coincide con el identificador de la tienda de la aplicación (un valor de Id. de Store parece a 9NBLGGH4R315).

3. También tienes la opción de habilitar la mediación de anuncios para el **AdControl** mediante la configuración de las opciones de la sección [Configuración de la mediación](../publish/in-app-ads.md#mediation) de la página [Anuncios desde la aplicación](../publish/in-app-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft.

4.  En el código, reemplaza los valores de unidad de anuncio de prueba (**ApplicationId** y **AdUnitId**) con los valores dinámicos generados en el centro de partners.

5.  [Enviar la aplicación](../publish/app-submissions.md) a la tienda mediante el centro de partners.

6.  Revisa los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de partners.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios en tu aplicación

Puedes usar varios objetos **AdControl** en una sola aplicación (por ejemplo, cada página de la aplicación puede hospedar un objeto **AdControl** diferente). En este escenario, te recomendamos que asignes una unidad de anuncio diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para anuncios de banner](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Tutorial de control de errores en XAML/C#](error-handling-in-xamlc-walkthrough.md).
* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
* [Configurar unidades de anuncios para la aplicación](set-up-ad-units-in-your-app.md)

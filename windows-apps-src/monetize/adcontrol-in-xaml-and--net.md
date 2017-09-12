---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "Aprende a usar la clase AdControl para mostrar anuncios en banner en una aplicación XAML para Windows10 (UWP), Windows8.1 o Windows Phone8.1."
title: AdControl en XAML y .NET
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, control de anuncios, XAML, .NET, tutorial
ms.openlocfilehash: be273ca4c17edb4affa5e0abb4b3317b03893280
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl en XAML y .NET


En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) para mostrar anuncios en banner en una aplicación XAML para Windows10 (UWP), Windows8.1 o Windows Phone8.1.

Para obtener un ejemplo de proyecto completo que enseña cómo agregar anuncios en banner a una aplicación XAML con C# y C++, consulta los [ejemplos de publicidad en GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Requisitos previos

* Para aplicaciones para UWP: instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior.
* En el caso de las aplicaciones de Windows8.1 o Windows Phone 8.1, instala [el SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) con Visual Studio 2015 o Visual Studio 2013.

## <a name="code-development"></a>Programación de código

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

1.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**

2.  En el **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

    -   Para un proyecto de Windows Phone 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows Phone 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y Ad Mediator al proyecto, pero puedes ignorar las bibliotecas de Ad Mediator.

    ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

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

5. En la etiqueta **Grid**, agrega el código para **AdControl**. Asigna las propiedades [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) y [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) de **Page** a los valores de prueba proporcionados en [Valores del modo de prueba](test-mode-values.md). Ajusta también el alto y el ancho del control para que se corresponda con uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Tienda, debes [reemplazar estos valores de prueba por valores dinámicos](#release) desde el Centro de desarrollo de Windows.

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
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publicar la aplicación con anuncios dinámicos mediante el Centro de desarrollo de Windows

1.  En el panel del Centro de desarrollo, ve a la página [Monetizar con anuncios](../publish/monetize-with-ads.md) correspondiente a la aplicación y [crea una unidad de anuncios](../monetize/set-up-ad-units-in-your-app.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2. Si la aplicación es una aplicación para UWP para Windows10, tienes la opción de habilitar la mediación de anuncios para **AdControl** mediante la configuración de las opciones en la sección [Mediación de anuncios](../publish/monetize-with-ads.md#mediation) de la página [Monetizar con anuncios](../publish/monetize-with-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft.

3.  En el código, reemplaza los valores de unidad de anuncio de prueba (**ApplicationId** y **AdUnitId**) por los valores dinámicos generados en el Centro de desarrollo.

4.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

5.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios en tu aplicación

Puedes usar varios objetos **AdControl** en una sola aplicación (por ejemplo, cada página de la aplicación puede hospedar un objeto **AdControl** diferente). En este escenario, te recomendamos que asignes una unidad de anuncio diferente para cada control. Con unidades de anuncio diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/monetize-with-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="notes"></a>Notas

* C#: consulta el [Ejemplo de propiedades XAML](xaml-properties-example.md) para ver un ejemplo de cómo asignar controladores de eventos a eventos **AdControl**. A continuación, consulta el tema [Eventos de AdControl en C#](adcontrol-events-in-c.md) para obtener un código de ejemplo que muestre los controladores de eventos escritos en C#.

* C++: la versión actual de las bibliotecas de Microsoft Advertising admite C++. La clase **AdControl** está implementada en C++ nativo y no carga .NET CLR. Para ver ejemplos de código que muestran cómo usar **AdControl** en C++, consulta los [ejemplos de publicidad en GitHub](http://aka.ms/githubads).

* Visual Basic: consulta el [Ejemplo de propiedades XAML](xaml-properties-example.md) para ver un ejemplo de cómo asignar controladores de eventos a eventos **AdControl**.

* Control de errores: para obtener información sobre cómo controlar errores, consulta [control de errores de AdControl](adcontrol-error-handling.md).

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
* [Configurar unidades de anuncios para la aplicación](../monetize/set-up-ad-units-in-your-app.md)

---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "Aprende a usar la clase AdControl para mostrar anuncios en banner en una aplicación XAML para Windows10 (UWP), Windows8.1 o Windows Phone8.1."
title: AdControl en XAML y .NET
translationtype: Human Translation
ms.sourcegitcommit: 35f07c73a72e5242d59c6b45e6d5b4ac62f40741
ms.openlocfilehash: 0652bd1c3e52c9026b26e14b2475a4b34997ac91

---

# AdControl en XAML y .NET




En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) para mostrar anuncios en banner en una aplicación XAML para Windows10 (UWP), Windows8.1 o Windows Phone8.1. En este tutorial no se usa **AdMediatorControl** ni la mediación de anuncios.

Para obtener un ejemplo de proyecto completo que enseña cómo agregar anuncios en banner a una aplicación XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## Requisitos previos

* En el caso de las aplicaciones para UWP, instala el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) con Visual Studio 2015.
* En el caso de las aplicaciones de Windows8.1 o Windows Phone 8.1, instala [el SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) con Visual Studio 2015 o Visual Studio 2013.

## Programación de código

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

1.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**

2.  En el **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para XAML** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y de Ad Mediator al proyecto, pero puede ignorar las bibliotecas de Ad Mediator.

    -   Para un proyecto de Windows Phone 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Ad Mediator para Windows Phone 8.1 XAML**. Esta opción agregará las bibliotecas de Microsoft Advertising y Ad Mediator al proyecto, pero puedes ignorar las bibliotecas de Ad Mediator.

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **Nota** Esta imagen corresponde a la creación de un proyecto para la UWP para Windows10 en Visual Studio 2015. Si estás creando una aplicación de Windows 8.1 o Windows Phone 8.1 o si usas Visual Studio 2013, la pantalla tendrá un aspecto diferente.

3.  En el **Administrador de referencias**, haz clic en Aceptar.
4.  Modifica el código XAML de la página en la que se insertará publicidad para que incluya el espacio de nombres **Microsoft.Advertising.WinRT.UI**. Por ejemplo, en la aplicación de ejemplo predeterminada que genera Visual Studio (denominada, en esta aplicación, MyAdFundedWindows10AppXAML), la página XAML es **MainPage.XAML**.

    La sección **Page** del archivo MainPage.xaml que genera Visual Studio tiene el código siguiente.

    ``` syntax
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

    ``` syntax
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

5.  En la etiqueta **Grid**, agrega el código para **AdControl**.

    1.  Asigna las propiedades [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) y [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) en **Page** para probar los valores proporcionados en los [Valores del modo de prueba](test-mode-values.md).

        > **Nota** Sustituirás los valores de prueba por valores dinámicos antes de enviar la aplicación para su envío.

    2.  Ajusta el alto y ancho del control para que se corresponda con uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    La etiqueta **Grid** completa tiene el aspecto de este código.

    ``` syntax
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
    </Grid>
    ```

    El código completo del archivo MainPage.xaml debería tener este aspecto.

    ``` syntax
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
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
        </Grid>
    </Page>
    ```

6.  Compila y ejecuta la aplicación para verla con un anuncio.

## Publicar la aplicación con anuncios dinámicos mediante el Centro de desarrollo de Windows


1.  En el panel del Centro de desarrollo, ve a la página **Monetización** &gt; **Monetizar con anuncios** correspondiente a la aplicación y [crea una unidad de Microsoft Advertising independiente](../publish/monetize-with-ads.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2.  En el código, reemplaza los valores de unidad de anuncio de prueba (**ApplicationId** y **AdUnitId**) con los valores dinámicos generados en el Centro de desarrollo.

3.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

4.  Revisa los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

## Notas

* C#: consulta el [Ejemplo de propiedades XAML](xaml-properties-example.md) para ver un ejemplo de cómo asignar controladores de eventos a eventos **AdControl**. A continuación, consulta el tema [Eventos de AdControl en C#](adcontrol-events-in-c.md) para obtener un código de ejemplo que muestre los controladores de eventos escritos en C#.

* C++: la versión actual de las bibliotecas de Microsoft Advertising admite C++. La clase **AdControl** está implementada en C++ nativo y no carga .NET CLR. Para ver ejemplos de código que muestran cómo usar **AdControl** en C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

* Visual Basic: consulta el [Ejemplo de propiedades XAML](xaml-properties-example.md) para ver un ejemplo de cómo asignar controladores de eventos a eventos **AdControl**.

* Control de errores: para obtener información sobre cómo controlar errores, consulta [control de errores de AdControl](adcontrol-error-handling.md).

## Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)

 



<!--HONumber=Nov16_HO1-->



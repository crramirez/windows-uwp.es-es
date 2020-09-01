---
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
title: Tutorial de control de errores en XAML y C#
description: Siga este tutorial para obtener información sobre cómo detectar y controlar los errores de un control en una aplicación XAML/C#.
ms.date: 02/18/2020
ms.topic: article
keywords: 'Windows 10, UWP, anuncios, publicidad, control de errores, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 7ad3d7dfcbf191960fba03808270a55d3128d4f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158589"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>Tutorial de control de errores en XAML y C#

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tutorial se muestra cómo detectar errores relacionados con ad en la aplicación. En este tutorial se usa un [control AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar un banner, pero los conceptos generales de también se aplican a los anuncios intersticiales y a los anuncios nativos.

En estos ejemplos se da por hecho que tienes una aplicación XAML o C# que contiene un objeto **AdControl**. Para obtener instrucciones paso a paso que muestran cómo agregar un objeto **AdControl** a la aplicación, consulta [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md). 

1.  En el archivo MainPage.xaml, busca la definición de **AdControl**. El código tiene esta apariencia.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   Después de la propiedad **Width**, pero antes de la etiqueta de cierre, asigna un nombre de un controlador de evento de error a el evento[ErrorOccurred](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred). En este tutorial, el nombre del controlador de evento de error es **OnAdError**.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  Para generar un error en tiempo de ejecución, crea un segundo objeto **AdControl** con un id. de la aplicación diferente. Dado que todos los objetos **AdControl** de una aplicación deben usar el mismo id. de la aplicación, al crear un objeto **AdControl** adicional con un id. de la aplicación diferente se producirá un error.

    Define un segundo objeto **AdControl** en MainPage.xaml justo después del primer **AdControl**y establece la propiedad [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) en cero ("0").
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  En el archivo MainPage.xaml.cs, agrega el siguiente controlador de eventos **OnAdError** a la clase **MainPage**. Este controlador de eventos escribe información en la ventana **Resultados** de Visual Studio.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Compile y ejecute el proyecto. Una vez que se ejecute la aplicación, verás un mensaje similar al siguiente en la ventana **Resultados** de Visual Studio.
    ```json
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
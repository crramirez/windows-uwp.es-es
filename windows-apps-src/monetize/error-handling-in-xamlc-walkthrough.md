---
author: Xansky
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: Aprende a detectar errores de AdControl en la aplicación.
title: Tutorial de control de errores en XAML y C#
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, anuncios, publicidad, control de errores, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: be101f5ec189d822bc9704b435f4a098b61f57ac
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6842612"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>Tutorial de control de errores en XAML y C#

En este tutorial se muestra cómo detectar errores relacionados con los anuncios en la aplicación. Este tutorial usa un objeto [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar un anuncio de banner, pero los conceptos generales en él también se aplican a anuncios intersticiales y anuncios nativos.

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

2.   Después de la propiedad **Width**, pero antes de la etiqueta de cierre, asigna un nombre de un controlador de evento de error a el evento[ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred). En este tutorial, el nombre del controlador de evento de error es **OnAdError**.
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

    Define un segundo objeto **AdControl** en MainPage.xaml justo después del primer **AdControl**y establece la propiedad [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) en cero ("0").
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

4.  Compilar y ejecutar el proyecto. Una vez que se ejecute la aplicación, verás un mensaje similar al siguiente en la ventana **Resultados** de Visual Studio.
    ```
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)

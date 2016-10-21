---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: "Aprende a detectar errores de AdControl en la aplicación."
title: Tutorial de control de errores en XAML y C#
translationtype: Human Translation
ms.sourcegitcommit: 90c866fcdb4df0f32a4ace0cb4f6b761d6e9170e
ms.openlocfilehash: bca54776fb4793fbc9e0b9af070a0cc676168d86


---

# Tutorial de control de errores en XAML y C#




En este tema se muestra cómo detectar errores de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en la aplicación.

En estos ejemplos se da por hecho que tienes una aplicación XAML o C# que contiene un objeto **AdControl**. Para obtener instrucciones paso a paso que muestran cómo agregar un objeto **AdControl** a la aplicación, consulta [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md). Para un proyecto de muestra completo que muestra cómo agregar anuncios en banners a una aplicación XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

1.  En el archivo MainPage.xaml, busca la definición de **AdControl**. El código tiene esta apariencia.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300" />
    ```

2.   Después de la propiedad **Width**, pero antes de la etiqueta de cierre, asigna un nombre de un controlador de evento de error a el evento[ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx). En este tutorial, el nombre del controlador de evento de error es **OnAdError**.

    ``` syntax
    ErrorOccurred="OnAdError"
    ```

    El código XAML completo que define el objeto **AdControl** tiene este aspecto.

    ``` syntax
    <UI:AdControl
       ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
       AdUnitId="10865270"
       HorizontalAlignment="Left"
       Height="250"
       Margin="10,10,0,0"
       VerticalAlignment="Top"
       Width="300"
       ErrorOccurred="OnAdError"/>
    ```

2.  Para generar un error en tiempo de ejecución, crea un segundo objeto **AdControl** con un id. de la aplicación diferente. Dado que todos los objetos **AdControl** de una aplicación deben usar el mismo id. de la aplicación, al crear un objeto **AdControl** adicional con un id. de la aplicación diferente se producirá un error.

    Define un segundo objeto **AdControl** en MainPage.xaml justo después del primer **AdControl**y establece la propiedad [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) en cero ("0").

    ``` syntax
    <UI:AdControl
      ApplicationId="0"
      AdUnitId="10865270"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,265,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError" />
    ```

3.  En el archivo MainPage.xaml.cs, agrega el siguiente controlador de eventos **OnAdError** a la clase **MainPage**. Este controlador de eventos escribe información en la ventana **Resultados** de Visual Studio.

    ``` syntax
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Compilar y ejecutar el proyecto.

Una vez que se ejecute la aplicación, verás un mensaje similar al siguiente en la ventana **Resultados** de Visual Studio.

``` syntax
AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
```

## Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 



<!--HONumber=Sep16_HO3-->



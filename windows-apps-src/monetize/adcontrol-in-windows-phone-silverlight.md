---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Aprende a usar la clase AdControl para mostrar anuncios de banner en una aplicación Silverlight para Windows Phone 8.1 o Windows Phone 8.0."
title: AdControl en Windows Phone Silverlight
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, Silverlight, Windows Phone
ms.openlocfilehash: 743b9faccaa120f1904b592fc09a965dc7878e03
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>AdControl en Windows Phone Silverlight

En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) para mostrar anuncios de banner en una aplicación Silverlight para Windows Phone8.1 o Windows Phone8.0.

> **Nota para Windows Phone Silverlight8.0**&nbsp;&nbsp;Los anuncios en banner aún se admiten para las aplicaciones de Silverlight de Windows Phone8.0 que usan **AdControl** de una versión anterior del Universal Ad Client SDK o Microsoft Advertising SDK y que ya están disponibles en la Tienda. Sin embargo, los anuncios en banner ya no se admiten en los nuevos proyectos de Silverlight para Windows Phone8.0. Además, algunos escenarios de depuración y pruebas están limitados en los proyectos de Silverlight de Windows Phone8.x. Para obtener más información, consulta [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md#silverlight_support).

## <a name="add-the-advertising-assemblies-to-your-project"></a>Agregar los ensamblados publicitarios a tu proyecto

Para comenzar, descarga e instala el paquete de NuGet que contiene los ensamblados de Microsoft Advertising para Windows Phone Silverlight en tu proyecto.

1.  Abre el proyecto en Visual Studio.

2.  Haz clic en **Herramientas**, señala **Administrador de paquetes de NuGet**y haz clic en **Consola del Administrador de paquetes**.

3.  En la ventana de la **Consola del Administrador de paquetes**, escriba uno de estos comandos.

  * Si el proyecto está destinado a Windows Phone8.0, escribe este comando.

      > [!div class="tabbedCodeSnippets"]
      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * Si el proyecto está destinado a Windows Phone8.1, escribe este comando.

      > [!div class="tabbedCodeSnippets"]
      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    Tras escribir el comando, todos los ensamblados de Microsoft Advertising para Silverlight se descargan en el proyecto local a través de paquetes NuGet y se agregan automáticamente al proyecto referencias a estos ensamblados.

## <a name="code-your-app"></a>Programar la aplicación


1.  Agrega las siguientes funciones desde el nodo **Capabilities** del archivo WMAppManifest.xml.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  Para este ejemplo, el nodo **Capabilities** tiene el aspecto siguiente:

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  <Capabilities>
      <Capability Name="ID_CAP_NETWORKING"/>
      <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
      <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
      <Capability Name="ID_CAP_SENSORS"/>
      <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
      <Capability Name="ID_CAP_IDENTITY_USER"/>
      <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
      <Capability Name="ID_CAP_PHONEDIALER"/>
  </Capabilities>
  ```

2.  (Opcional) Guarda el proyecto. Haz clic en el icono **Guardar todo** o, en el menú **Archivo**, haz clic en **Guardar todo**.

3.  Agrega la funcionalidad **Internet (cliente y servidor)** al archivo Package.appxmanifest del proyecto. En el **Explorador de soluciones**, haz doble clic en **Package.appxmanifest**.

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    En el **Editor**, haz clic en la pestaña **Funcionalidad**. Activa la casilla **Internet (cliente y servidor)**.

4.  (Opcional) Guarda el proyecto. Haz clic en el icono **Guardar todo** o, en el menú **Archivo**, haz clic en **Guardar todo**.

5.  Modifica el marcado de Silverlight en el archivo MainPage.xaml para incluir el espacio de nombres **Microsoft.Advertising.Mobile.UI**.

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  El encabezado de la página tendrá el código siguiente:

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  En la **Cuadrícula**, agrega el siguiente código para **AdControl**. Asigna las propiedades **ApplicationId** y **AdUnitId** a los valores de prueba proporcionados en los [Valores del modo de prueba](test-mode-values.md) y ajusta las propiedades **Height** y **Width** para que se correspondan con uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

  > **Nota**&nbsp;&nbsp;Deberás reemplazar los valores de **ApplicationId** y **AdUnitId** de prueba por valores dinámicos antes de enviar la aplicación.

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <Grid x:Name="ContentPanel" Grid.Row="1">
      <UI:AdControl
          ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
          AdUnitId="10865270"
          HorizontalAlignment="Left"
          Height="80"
          VerticalAlignment="Top"
          Width="480"/>
  </Grid>
  ```

7.  Compila y ejecuta el proyecto. Confirma que se muestre un anuncio en la aplicación con un aspecto similar al siguiente:

  ![wp81silverlight\ emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## <a name="release-your-app-with-live-ads-using-dev-center"></a>Publica la aplicación con anuncios dinámicos desde el Centro de desarrollo de Windows

1.  En el panel del Centro de desarrollo, ve a la página **Monetización** &gt; **Monetizar con anuncios** correspondiente a la aplicación y [crea una unidad de Microsoft Advertising independiente](../publish/monetize-with-ads.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2.  En el código, reemplaza los valores de unidades de anuncio de prueba (**applicationId** y **adUnitId**) con los valores dinámicos generados en el Centro de desarrollo.

3.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

4.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.


 

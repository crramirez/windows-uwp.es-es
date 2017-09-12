---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Aprende a usar la clase AdControl para mostrar anuncios de banner en una aplicación Silverlight para Windows Phone 8.1 o Windows Phone 8.0."
title: AdControl en Windows Phone Silverlight
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, Silverlight, Windows Phone
ms.openlocfilehash: f1582639757abfb6de156bf88ce8af71ba3eaacd
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/21/2017
---
# <a name="adcontrol-in-windows-phone-silverlight"></a>AdControl en Windows Phone Silverlight

En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) para mostrar anuncios de banner en una aplicación Silverlight para Windows Phone8.1 o Windows Phone8.0.

<span id="silverlight_support"/>
## <a name="advertising-support-for-windows-phone-8x-silverlight-projects"></a>Compatibilidad de publicidad para proyectos de Windows Phone 8.x Silverlight

Algunos escenarios de desarrollado ya no se admiten en proyectos de Windows Phone 8.x Silverlight. Para obtener más información, consulta la tabla siguiente.

|  Versión de plataforma  |  Proyectos existentes    |   Proyectos nuevos  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  Si tienes un proyecto existente de Windows Phone 8.0 Silverlight que ya usa clase **AdControl** o **AdMediatorControl** de una versión anterior de Microsoft Universal Ad Client o Microsoft Advertising SDK y esta aplicación ya está publicada en la Tienda Windows, puedes modificar y volver a compilar el proyecto y puedes depurar o probar los cambios en un dispositivo. No se admite la depuración o la prueba del proyecto en el emulador.  |  No se admite.  |
| Windows Phone 8.1 Silverlight    |  Si tienes un proyecto existente de Windows Phone 8.1 Silverlight que usa una clase **AdControl** o **AdMediatorControl** de una versión anterior del SDK, puedes modificar y volver a compilar el proyecto. Sin embargo, para depurar o probar la aplicación, debes ejecutarla en el emulador y usar [valores del modo de prueba](test-mode-values.md) para el identificador de aplicación y el identificador de unidad de anuncio. No se admite la depuración o la prueba de la aplicación en un dispositivo.  |   Puedes agregar una clase **AdControl** o **AdMediatorControl** a un nuevo proyecto nuevo de Windows Phone 8.1 Silverlight. Sin embargo, para depurar o probar la aplicación, debes ejecutarla en el emulador y usar [valores del modo de prueba](test-mode-values.md) para el identificador de aplicación y el identificador de unidad de anuncio. No se admite la depuración o la prueba de la aplicación en un dispositivo. |

## <a name="add-the-advertising-assemblies-to-your-project"></a>Agregar los ensamblados publicitarios a tu proyecto

Para comenzar, descarga e instala el paquete de NuGet que contiene los ensamblados de Microsoft Advertising para Windows Phone Silverlight en tu proyecto.

1.  Abre el proyecto en Visual Studio.

2.  Haz clic en **Herramientas**, señala **Administrador de paquetes de NuGet**y haz clic en **Consola del Administrador de paquetes**.

3.  En la ventana de la **Consola del Administrador de paquetes**, escriba uno de estos comandos.

  * Si el proyecto está destinado a Windows Phone8.0, escribe este comando.

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL80 -Version 6.2.40501.1
      ```

  * Si el proyecto está destinado a Windows Phone8.1, escribe este comando.

      ```syntax
      Install-Package Microsoft.Advertising.WindowsPhone.SL81 -Version 8.1.50112
      ```

    Tras escribir el comando, todos los ensamblados de Microsoft Advertising para Silverlight se descargan en el proyecto local a través de paquetes NuGet y se agregan automáticamente al proyecto referencias a estos ensamblados.

## <a name="code-your-app"></a>Programar la aplicación


1.  Agrega las siguientes funciones desde el nodo **Capabilities** del archivo WMAppManifest.xml.

  ``` syntax
  <Capability Name="ID_CAP_IDENTITY_USER"/>
  <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
  <Capability Name="ID_CAP_PHONEDIALER"/>
  ```

  Para este ejemplo, el nodo **Capabilities** tiene el aspecto siguiente:

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

  ``` xml
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  ```

  El encabezado de la página tendrá el código siguiente:

  ``` xml
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
  xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
  x:Class="PhoneApp1.MainPage"
  ```

6.  En la **Cuadrícula**, agrega el siguiente código para **AdControl**. Asigna las propiedades **ApplicationId** y **AdUnitId** a los valores de prueba proporcionados en los [Valores del modo de prueba](test-mode-values.md) y ajusta las propiedades **Height** y **Width** para que se correspondan con uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

  > **Nota**&nbsp;&nbsp;Deberás reemplazar los valores de **ApplicationId** y **AdUnitId** de prueba por valores dinámicos antes de enviar la aplicación.

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

1.  En el panel del Centro de desarrollo, ve a la página **Monetización** &gt; **Monetizar con anuncios** correspondiente a la aplicación y [crea una unidad de anuncios independiente](../publish/monetize-with-ads.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2.  En el código, reemplaza los valores de unidades de anuncio de prueba (**applicationId** y **adUnitId**) con los valores dinámicos generados en el Centro de desarrollo.

3.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

4.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.


 

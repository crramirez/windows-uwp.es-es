---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Aprende a usar la clase AdControl para mostrar anuncios de banner en una aplicación Silverlight para Windows Phone 8.1 o Windows Phone 8.0."
title: AdControl en Windows Phone Silverlight
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a12badfb11cfd43c0833522d996da7df73b3d55


---

# AdControl en Windows Phone Silverlight


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) para mostrar anuncios de banner en una aplicación Silverlight para Windows Phone 8.1 o Windows Phone 8.0.

## Requisitos previos.

*  Instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) con Visual Studio 2015 o Visual Studio 2013.


## Agregar las referencias de ensamblado de publicidad

Los ensamblados de Microsoft Advertising para proyectos de Windows Phone Silverlight no se instalan localmente con el SDK de Microsoft Store Engagement and Monetization. Para poder empezar a actualizar el código, debes usar **Servicios conectados** con compatibilidad con la mediación de anuncios en el SDK de Microsoft Store Engagement and Monetization para descargar los ensamblados y hacer referencia a ellos en el proyecto.

1.  En Visual Studio, haz clic en **Proyecto** y en **Agregar servicio conectado**.

2.  En el cuadro de diálogo **Agregar servicio conectado**, haz clic en **Ad Mediator** y, a continuación, en **Configurar**.

3.  Haz clic en **Seleccionar redes de anuncios** y selecciona solo **Microsoft Advertising**.

    En este momento, se descargan todos los ensamblados de Microsoft Advertising para Silverlight en el proyecto local a través de paquetes NuGet y se agregan al proyecto referencias a estos ensamblados automáticamente. También se agrega al proyecto una referencia al ensamblado de mediación de anuncios. En un paso posterior, se quitará la referencia de ensamblado de mediación de anuncios, ya que no es necesaria para este escenario.

4.  En el cuadro de diálogo **Seleccionar redes de anuncios**, haz clic en **Aceptar**. Haz clic en **Aceptar** de nuevo en la siguiente página de confirmación **Obteniendo estado** y, por último, en **Agregar** para cerrar el cuadro de diálogo **Ad Mediator**.

5.  En el **Explorador de soluciones**, expande el nodo **Referencias**. Haz clic con el botón secundario en **Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising** y haz clic en **Quitar**. Esta referencia de ensamblado no es necesaria para este escenario.

## Programar la aplicación


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

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    El encabezado de la página tendrá el código siguiente:

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  En la **Cuadrícula**, agrega el siguiente código para **AdControl**. Asigna las propiedades **ApplicationId** y **AdUnitId** a los valores de prueba proporcionados en los [Valores del modo de prueba](test-mode-values.md) y ajusta las propiedades **Height** y **Width** para que se correspondan con uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > **Nota**  
    Deberás reemplazar los valores de **ApplicationId** y **AdUnitId** de prueba con valores dinámicos antes de enviar la aplicación.

    ``` syntax
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

## Publica la aplicación con anuncios dinámicos desde el Centro de desarrollo de Windows


1.  En el panel del Centro de desarrollo, ve a la página **Monetización**&gt;**Monetizar con anuncios** de la aplicación y [crea una unidad de Microsoft Advertising independiente](../publish/monetize-with-ads.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2.  En el código, reemplaza los valores de unidades de anuncio de prueba (**applicationId** y **adUnitId**) con los valores dinámicos generados en el Centro de desarrollo.

3.  
            [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

4.  Revisa los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.


 



<!--HONumber=Jun16_HO4-->



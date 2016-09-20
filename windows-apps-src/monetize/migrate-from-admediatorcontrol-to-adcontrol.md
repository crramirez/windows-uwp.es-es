---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: "Aprende cómo se realiza la migración de AdMediatorControl a AdControl en las aplicaciones para UWP."
title: Migrar de AdMediatorControl a AdControl para las aplicaciones para UWP
translationtype: Human Translation
ms.sourcegitcommit: 07baa54990ec31dc0cb9c289f9f0222754da9d7c
ms.openlocfilehash: 3abef943530cc756de117edccc5ab16e5f178604

---

# Migrar de AdMediatorControl a AdControl para las aplicaciones para UWP

Las versiones de SDK de publicidad anteriores de Microsoft permitían a las aplicaciones para la Plataforma universal de Windows (UWP) mostrar anuncios de banner mediante la clase **AdMediatorControl**, con la que los desarrolladores podían optimizar sus ingresos por anuncios con la visualización de anuncios de banner desde nuestras redes socias (AOL y AppNexus), así como AdDuplex. El [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) ya no admite la clase **AdMediatorControl**. Si tienes una aplicación existente que usa la clase **AdMediatorControl** de un SDK anterior y quieres migrarla a una aplicación para UWP que usa el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk), sigue las instrucciones de este artículo para actualizar el código de modo que use la clase **AdControl** en lugar de la clase **AdMediatorControl**. De manera opcional, puedes configurar la aplicación para la mediación de anuncios con AdDuplex, mediante un enfoque ponderado o clasificado.

>**Nota**&nbsp;&nbsp;Los ejemplos de código de este artículo se proporcionan solo con fines ilustrativos. Es posible que debas realizar ajustes en los ejemplos de código para que funcione en tu aplicación.

## Requisitos previos

* Una aplicación para UWP que esté usando actualmente la clase AdMediatorControl y esté publicada en la Tienda Windows.
* Un equipo de desarrollo con Visual Studio 2015 y el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) instalado.
* Si quieres mediar anuncios con AdDuplex, también debes tener el [AdDuplex Windows 10 SDK](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077) instalado en el equipo de desarrollo.

  >**Nota**&nbsp;&nbsp;Como alternativa a ejecutar el instalador del SDK de AdDuplex desde el vínculo anterior, puedes instalar las bibliotecas de AdDuplex para tu proyecto de aplicación para UWP en Visual Studio 2015. Con el proyecto de aplicación para UWP abierto en Visual Studio 2015, haz clic en **Proyecto** > **Administrar paquetes NuGet**, busca el paquete NuGet denominado **AdDuplexWin10** e instálalo.

## Paso 1: Recuperar tus identificadores de aplicación y los identificadores de unidades de anuncios

Al migrar el código para usar la clase **AdControl**, debes conocer tus identificadores de aplicación y los identificadores de unidades de anuncios. La mejor manera de obtener los identificadores más recientes es recuperarlos del archivo de configuración de mediación.

1. Inicia sesión en el panel del Centro de desarrollo de Windows y haz clic en la aplicación que está usando actualmente la clase **AdMediatorControl**.
2. Haz clic en **Monetización** y, después, en **Monetizar con anuncios**.
3. En la sección **Mediación de anuncios de Windows**, haz clic en el vínculo **Descargar configuración de mediación** y abre el archivo AdMediator.config en un editor de texto como el Bloc de notas.
4. En el archivo, localiza el elemento ```<AdAdapterInfo>``` con el elemento secundario ```<Name>MicrosoftAdvertising</Name>```. Esta sección contiene la configuración para los anuncios de pago de Microsoft.
5. En este elemento ```<AdAdapterInfo>```, busca los elementos ```<Property>``` que contengan elementos ```<Key>``` con los valores **WApplicationId** y **WAdUnitId**. En el ejemplo siguiente, los valores de los elementos ```<Value>``` son ejemplos.

  ```xml
  <Metadata>
      <Property>
          <Key>WApplicationId</Key>
          <Value>364d4938-c0f5-4c3d-8aae-090206211dc9</Value>
      </Property>
      <Property>
          <Key>WAdUnitId</Key>
          <Value>301568</Value>
      </Property>
  </Metadata>
  ```
6. Copia ambos valores en estos elementos ```<Value>``` para su uso posterior. Estos valores contienen el identificador de aplicación y el identificador de unidad de anuncios de la unidad de anuncios no móviles para los anuncios de pago de Microsoft.
5. En el mismo elemento ```<AdAdapterInfo>```, busca los elementos ```<Property>``` que contengan elementos ```<Key>``` con los valores **MApplicationId** y **MAdUnitId**. En el ejemplo siguiente, los valores de los elementos ```<Value>``` son ejemplos.

  ```xml
  <Metadata>
      <Property>
          <Key>MApplicationId</Key>
          <Value>e2cf8388-7018-4a11-8ab0de90f2a7a401</Value>
      </Property>
      <Property>
          <Key>MAdUnitId</Key>
          <Value>301056</Value>
      </Property>
  </Metadata>
  ```
6. Copia ambos valores en los elementos ```<Value>``` para su uso posterior. Estos valores contienen el identificador de aplicación y el identificador de unidad de anuncios de la unidad de anuncios móviles para los anuncios de pago de Microsoft.
7. Si usas [anuncios internos](../publish/about-house-ads.md), busca el elemento ```<AdAdapterInfo>``` con el elemento secundario ```<Name>MicrosoftAdvertisingHouse</Name>```. En este elemento, busca elementos ```<Key>``` con los valores **MAdUnitId** y **WAdUnitId**, y guarda los valores de los elementos ```<Value>``` correspondientes para su uso posterior. Son los identificadores de unidades de anuncios móviles y no móviles para anuncios internos de Microsoft, respectivamente.
8. Si usas AdDuplex, busca el elemento ```<AdAdapterInfo>``` con el elemento secundario ```<Name>AdDuplex</Name>```. En este elemento, busca los elementos ```<Key>``` con los valores **AppKey** y **AdUnitId**, y guarda los valores de los elementos ```<Value>``` correspondientes para su uso posterior. Son tu clave de aplicación de AdDuplex y tu identificador de unidad de anuncios, respectivamente.

## Paso 2: Actualizar el código de la aplicación

Ahora que ya tienes tus identificadores de aplicación y los identificadores de unidades de anuncios, estás listo para actualizar el código de la aplicación para usar **AdControl** en lugar de **AdMediatorControl**.

### Solo anuncios de pago de Microsoft

Si solo usas anuncios de pago de Microsoft en la configuración de mediación de anuncios, sigue estos pasos.

  >**Nota**&nbsp;&nbsp;En estos pasos, se supone que la página de la aplicación en la que quieres mostrar anuncios contiene una cuadrícula vacía denominada **myAdGrid**, por ejemplo: ```<Grid x:Name="myAdGrid"/>```. En estos pasos, crearás y configurarás los controles de anuncios por completo en el código, en lugar de en XAML.

1. En Visual Studio, abre tu proyecto de aplicación para UWP.
2.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y selecciona **Agregar referencia**.
En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for XAML** (versión 10.0).
3. En el **Administrador de referencias**, haz clic en Aceptar.
4. Quita la declaración **AdMediatorControl** de tu código XAML y quita cualquier código que use este objeto **AdMediatorControl**, incluidos los controladores de eventos relacionados.
5. Abre el archivo de código para la **página** de la aplicación en la que quieres mostrar anuncios.
6. Agrega la siguiente instrucción en la parte superior del archivo de código, si todavía no existe.

  ```csharp
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Agrega las siguientes declaraciones de constantes a tu clase **Page**.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID = "<tbd>";
  ```
7. Para cada una de estas declaraciones de constantes, reemplaza los valores ```<tbd>```:
  * **AD_WIDTH** y **AD_HEIGHT**: asígnalos a uno de los [tamaños de anuncio admitidos para anuncios de banner]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **WAPPLICATIONID** y **WADUNITID**: asígnalos a los valores **WApplicationId** y **WAdUnitId** de los anuncios de pago de Microsoft que recuperaste anteriormente del archivo de configuración de mediación (estos valores están destinados a la unidad de anuncios no móviles para los anuncios de pago).
  * **MAPPLICATIONID** y **MADUNITID**: asígnalos a los valores **MApplicationId** y **MAdUnitId** de los anuncios de pago de Microsoft que recuperaste anteriormente del archivo de configuración de mediación (estos valores están destinados a la unidad de anuncios móviles para los anuncios de pago).

8. Agrega las siguientes declaraciones de variables a tu clase **Page**.

  ```csharp
  // Declare an AdControl.
  private AdControl myAdControl = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myAppId = WAPPLICATIONID;
  private string myAdUnitId = WADUNITID;
  ```
5. Agrega el siguiente código a tu constructor de clase **Page**, después de la llamada al método **InitializeComponent()**.

  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myAppId = MAPPLICATIONID;
      myAdUnitId = MADUNITID;
  }

  // Initialize the AdControl.
  myAdControl = new AdControl();
  myAdControl.ApplicationId = myAppId;
  myAdControl.AdUnitId = myAdUnitId;
  myAdControl.Width = AD_WIDTH;
  myAdControl.Height = AD_HEIGHT;
  myAdControl.IsAutoRefreshEnabled = true;

  myAdGrid.Children.Add(myAdControl);
  ```  

### Anuncios de pago e internos de Microsoft y AdDuplex

Si usas anuncios internos de Microsoft o AdDuplex, así como anuncios de pago de Microsoft, y quieres continuar mediando anuncios con AdDuplex, sigue los pasos que se describen en esta sección. Los ejemplos de código admiten los anuncios internos de Microsoft y AdDuplex. Si usas AdDuplex, pero no anuncios internos de Microsoft, o viceversa, quita el código que no se aplique a tu escenario.

  >**Nota**&nbsp;&nbsp;En estos pasos, se supone que la página de la aplicación en la que quieres mostrar anuncios contiene una cuadrícula vacía denominada **myAdGrid**, por ejemplo: ```<Grid x:Name="myAdGrid"/>```. En estos pasos, crearás y configurarás los controles de anuncios por completo en el código, en lugar de en XAML.

1. En Visual Studio, abre tu proyecto de aplicación para UWP.
2.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y selecciona **Agregar referencia**.
En **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **Microsoft Advertising SDK for XAML** (versión 10.0).
3. En el **Administrador de referencias**, haz clic en Aceptar.
4. Quita la declaración **AdMediatorControl** de tu código XAML y quita cualquier código que use este objeto **AdMediatorControl**, incluidos los controladores de eventos relacionados.
5. Abre el archivo de código para la **página** de la aplicación en la que quieres mostrar anuncios.
6. Agrega las siguientes instrucciones en la parte superior del archivo de código, si todavía no existen.

  ```csharp
  using Windows.System.UserProfile;
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Agrega las siguientes declaraciones de constantes a tu clase **Page**.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const int HOUSE_AD_WEIGHT = <tbd>;
  private const int AD_REFRESH_SECONDS = 35;
  private const int MAX_ERRORS_PER_REFRESH = 3;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID_PAID = "<tbd>";
  private const string WADUNITID_HOUSE = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID_PAID = "<tbd>";
  private const string MADUNITID_HOUSE = "<tbd>";
  private const string ADDUPLEX_APPKEY = "<tbd>";
  private const string ADDUPLEX_ADUNIT = "<tbd>";
  ```
4. Para cada una de las declaraciones de constantes asignada a ```<tbd>```, reemplaza el valor ```<tbd>``` por el valor real de tu escenario.
  * **AD_WIDTH** y **AD_HEIGHT**: asígnalos a uno de los [tamaños de anuncio admitidos para anuncios de banner]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **HOUSE_AD_WEIGHT**: asígnalo a un entero de 0 a 100 que especifique el valor de peso que quieres aplicar a los anuncios internos de Microsoft en comparación con los anuncios de pago de Microsoft (donde 0 corresponde a no mostrar nunca anuncios internos y 100, a mostrarlos siempre).
  * **WAPPLICATIONID** y **WADUNITID_PAID**: asígnalos a los valores **WApplicationId** y **WAdUnitId** de los anuncios de pago de Microsoft que recuperaste anteriormente del archivo de configuración de mediación (estos valores están destinados a la unidad de anuncios no móviles para los anuncios de pago).
  * **WADUNITID_HOUSE**: asígnalo a **WAdUnitId** para los anuncios internos que recuperaste anteriormente del archivo de configuración de mediación (este valor es para la unidad de anuncios no móviles para anuncios internos).
  * **MAPPLICATIONID** y **MADUNITID_PAID**: asígnalos a los valores **MApplicationId** y **MAdUnitId** de los anuncios de pago de Microsoft que recuperaste anteriormente del archivo de configuración de mediación (estos valores están destinados a la unidad de anuncios móviles para los anuncios de pago).
  * **MADUNITID_HOUSE**: asígnalo a **MAdUnitId** para los anuncios internos que recuperaste anteriormente del archivo de configuración de mediación (este valor es para la unidad de anuncios móviles para anuncios internos).
  * **ADDUPLEX_APPKEY** y **ADDUPLEX_ADUNIT**: asígnalos a los valores de clave de aplicación de AdDuplex e identificador de unidad de anuncios que recuperaste anteriormente del archivo de configuración de mediación.
5. Agrega las siguientes declaraciones de variables a tu clase **Page**.
  ```csharp
  // Dispatch timer to fire at each ad refresh interval.
  private DispatcherTimer myAdRefreshTimer = new DispatcherTimer();

  // Global variables used for mediation decisions.
  private Random randomGenerator = new Random();
  private int errorCountCurrentRefresh = 0;  // Prevents infinite redirects.
  private int adDuplexWeight = 0;            // Will be set by GetAdDuplexWeight().

  // Microsoft and AdDuplex controls for banner ads.
  private AdControl myMicrosoftBanner = null;
  private AdDuplex.AdControl myAdDuplexBanner = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myMicrosoftAppId = WAPPLICATIONID;
  private string myMicrosoftPaidUnitId = WADUNITID_PAID;
  private string myMicrosoftHouseUnitId = WADUNITID_HOUSE;
  ```
5. Agrega el siguiente código a tu constructor de clase **Page** después de la llamada al método **InitializeComponent()**.
  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;
  adDuplexWeight = GetAdDuplexWeight();
  RefreshBanner();

  // Start the timer to refresh the banner at the desired interval.
  myAdRefreshTimer.Interval = new TimeSpan(0, 0, AD_REFRESH_SECONDS);
  myAdRefreshTimer.Tick += myAdRefreshTimer_Tick;
  myAdRefreshTimer.Start();

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myMicrosoftAppId = MAPPLICATIONID;
      myMicrosoftPaidUnitId = MADUNITID_PAID;
      myMicrosoftHouseUnitId = MADUNITID_HOUSE;
  }
  ```
6. Por último, agrega los siguientes métodos a tu clase **Page**. Estos métodos crean una instancia de los objetos **AdControl** de Microsoft y **AdControl** de AdDuplex, y usan un generador de números aleatorios junto con los valores de peso proporcionados para actualizar los anuncios de banner en estos controles a intervalos del temporizador regulares.
  ```csharp
  private int GetAdDuplexWeight()
  {
      // TODO: Change this logic to fit your needs.
      // This example uses Microsoft ads first in Canada and Mexico, then
      // AdDuplex as fallback. In France, AdDuplex is first. In other regions,
      // this example uses a weighted average approach, with 50% to AdDuplex.

      int returnValue = 0;
      switch (GlobalizationPreferences.HomeGeographicRegion)
      {
          case "CA":
          case "MX":
              returnValue = 0;
              break;
          case "FR":
              returnValue = 100;
              break;
          default:
              returnValue = 50;
              break;
      }
      return returnValue;
  }

  private void ActivateMicrosoftBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Use random number generator and house ads weight to determine whether
      // to use paid ads or house ads. Paid is the default. You could alternatively
      // write a method similar to GetAdDuplexWeight and override by region.
      string myAdUnit = myMicrosoftPaidUnitId;
      int houseWeight = HOUSE_AD_WEIGHT;
      int randomInt = randomGenerator.Next(0, 100);
      if (randomInt < houseWeight)
      {
          myAdUnit = myMicrosoftHouseUnitId;
      }

      // Hide the AdDuplex control if it is showing.
      if (null != myAdDuplexBanner)
      {
          myAdDuplexBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the Microsoft control.
      if (null == myMicrosoftBanner)
      {
          myMicrosoftBanner = new AdControl();
          myMicrosoftBanner.ApplicationId = myMicrosoftAppId;
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Width = AD_WIDTH;
          myMicrosoftBanner.Height = AD_HEIGHT;
          myMicrosoftBanner.IsAutoRefreshEnabled = false;

          myMicrosoftBanner.AdRefreshed += myMicrosoftBanner_AdRefreshed;
          myMicrosoftBanner.ErrorOccurred += myMicrosoftBanner_ErrorOccurred;

          myAdGrid.Children.Add(myMicrosoftBanner);
      }
      else
      {
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Visibility = Visibility.Visible;
          myMicrosoftBanner.Refresh();
      }
  }

  private void ActivateAdDuplexBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Hide the Microsoft control if it is showing.
      if (null != myMicrosoftBanner)
      {
          myMicrosoftBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the AdDuplex control.
      if (null == myAdDuplexBanner)
      {
          myAdDuplexBanner = new AdDuplex.AdControl();
          myAdDuplexBanner.AppKey = ADDUPLEX_APPKEY;
          myAdDuplexBanner.AdUnitId = ADDUPLEX_ADUNIT;
          myAdDuplexBanner.Width = AD_WIDTH;
          myAdDuplexBanner.Height = AD_HEIGHT;
          myAdDuplexBanner.RefreshInterval = AD_REFRESH_SECONDS;

          myAdDuplexBanner.AdLoaded += myAdDuplexBanner_AdLoaded;
          myAdDuplexBanner.AdCovered += myAdDuplexBanner_AdCovered;
          myAdDuplexBanner.AdLoadingError += myAdDuplexBanner_AdLoadingError;
          myAdDuplexBanner.NoAd += myAdDuplexBanner_NoAd;

          myAdGrid.Children.Add(myAdDuplexBanner);
      }
      myAdDuplexBanner.Visibility = Visibility.Visible;
  }

  private void myAdRefreshTimer_Tick(object sender, object e)
  {
      RefreshBanner();
  }

  private void RefreshBanner()
  {
      // Reset the error counter for this refresh interval and
      // make sure the ad grid is visible.
      errorCountCurrentRefresh = 0;
      myAdGrid.Visibility = Visibility.Visible;

      // Display ad from AdDuplex.
      if (100 == adDuplexWeight)
      {
          ActivateAdDuplexBanner();
      }
      // Display Microsoft ad.
      else if (0 == adDuplexWeight)
      {
          ActivateMicrosoftBanner();
      }
      // Use weighted approach.
      else
      {
          int randomInt = randomGenerator.Next(0, 100);
          if (randomInt < adDuplexWeight) ActivateAdDuplexBanner();
          else ActivateMicrosoftBanner();
      }
  }

  private void myMicrosoftBanner_AdRefreshed(object sender, RoutedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myMicrosoftBanner_ErrorOccurred(object sender, AdErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateAdDuplexBanner();
  }

  private void myAdDuplexBanner_AdLoaded(object sender, AdDuplex.Banners.Models.BannerAdLoadedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myAdDuplexBanner_NoAd(object sender, AdDuplex.Common.Models.NoAdEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdLoadingError(object sender, AdDuplex.Common.Models.AdLoadingErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdCovered(object sender,   AdDuplex.Banners.Core.AdCoveredEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }
  ```



<!--HONumber=Sep16_HO1-->



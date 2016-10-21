---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Aprende a iniciar un anuncio intersticial con C#.
title: "Código de ejemplo de anuncios intersticiales en C#"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 5969bfacd34bcfab5f1bebd2cfbade4fd16c5a39


---

# Código de ejemplo de anuncios intersticiales en C\# #  




En este tema se muestra cómo iniciar un anuncio intersticial con C#. Para obtener instrucciones paso a paso que ilustran cómo configurar el proyecto para usar este código, consulta [Anuncios intersticiales](interstitial-ads.md). Para obtener un proyecto de ejemplo completo que demuestra cómo agregar anuncios intersticiales en vídeo a una aplicación XAML con C#, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).


## Ejemplo de código

En este ejemplo de código se muestra un archivo de código MainPage.xaml.cs que implementa un anuncio intersticial. Este código supone que el archivo MainPage.xaml tiene un botón que funciona, con un evento **Click** que desencadena un método denominado **button_Click** y que se controla mediante este. Este código inicia el anuncio intersticial cuando se desencadena el evento **Click** en el botón.

Reemplaza el texto en las variables **AppID** y **AdUnitId** con valores dinámicos antes de enviar la aplicación a la Tienda. Para obtener más información, consulta [Set up ad units in your app](set-up-ad-units-in-your-app.md) (Configurar unidades de anuncios en tu aplicación).

``` syntax
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Microsoft.Advertising.WinRT.UI;


namespace BasicCSharpInterstitialUWP
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        InterstitialAd MyVideoAd = new InterstitialAd();

        public MainPage()
        {
            this.InitializeComponent();
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Define ApplicationId and AdUnitId.
            // Test values are shown here. Replace the test values with live values before submitting the app to the Store.
            var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
            var MyAdUnitId = "11389925";

            MyVideoAd.AdReady += MyVideoAd_AdReady;
            MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
            MyVideoAd.Completed += MyVideoAd_Completed;
            MyVideoAd.Cancelled += MyVideoAd_Cancelled;

            // Pre-fetch an ad 30-60 seconds before you need it.
            MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
        }

        void MyVideoAd_AdReady(object sender, object e)
        {
            if ((InterstitialAdState.Ready) == (MyVideoAd.State))
            {
                MyVideoAd.Show();
            }
        }

        void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
        {
            // Add your code here.
        }

        void MyVideoAd_Completed(object sender, object e)
        {
            // Add your code here.
        }

        void MyVideoAd_Cancelled(object sender, object e)
        {
            // Add your code here.  
        }
    }
}
```

 
## Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)
 



<!--HONumber=Aug16_HO3-->



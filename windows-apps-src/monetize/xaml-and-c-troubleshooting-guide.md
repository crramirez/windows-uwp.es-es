---
author: Xansky
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: Obtén información sobre las soluciones a problemas comunes de desarrollo con las bibliotecas de Microsoft Advertising en aplicaciones XAML.
title: Guía de solución de problemas de XAML y C#
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, anuncios, publicidad, AdControl, solución de problemas, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 12789767694e4ab3fa13efec4a31c8db4acd5420
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6040414"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>Guía de solución de problemas de XAML y C#

En este tema encontrarás soluciones a problemas comunes de desarrollo con las bibliotecas de Microsoft Advertising en aplicaciones de XAML.

* [XAML](#xaml)
  * [AdControl no aparece](#xaml-notappearing)
  * [La caja negra parpadea y desaparece](#xaml-blackboxblinksdisappears)
  * [Los anuncios no se actualizan](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [AdControl no aparece](#csharp-adcontrolnotappearing)
  * [La caja negra parpadea y desaparece](#csharp-blackboxblinksdisappears)
  * [Los anuncios no se actualizan](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl no aparece

1.  Asegúrate de que la funcionalidad **Internet (Client)** esté seleccionada en Package.appxmanifest.

2.  Comprueba el id. de aplicación y el id. de unidad de anuncios. Estos identificadores deben coincidir con el Id. de aplicación y el Id. de unidad de anuncio que obtuviste en el centro de partners. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Comprueba las propiedades **Height** y **Width**. Estas deben establecerse en uno de los [tamaños de anuncio admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Comprueba la posición de los elementos. [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) debe estar dentro del área visible.

5.  Comprueba la propiedad **Visibility**. La propiedad **Visibility** opcional no se debe establecer en contraída ni oculta. Esta propiedad puede establecerse en línea (como se muestra a continuación) o en una hoja de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Comprueba el elemento primario de **AdControl**. Si el elemento **AdControl** reside en un elemento primario, el elemento primario debe estar activo y visible.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  Asegúrate de que **AdControl** no esté oculto en la ventanilla. **AdControl** debe ser visible para que los anuncios se muestren correctamente.

8.  Los valores activos para **ApplicationId** y **AdUnitId** no deben probarse en el emulador. Para garantizar que **AdControl** funcione según lo previsto, usa los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) tanto para **ApplicationId** como para **AdUnitId**.

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>La caja negra parpadea y desaparece

1.  Vuelve a comprobar todos los pasos de la sección [AdControl no aparece](#xaml-notappearing) anterior.

2.  Controla el evento **ErrorOccurred** y usa el mensaje que se pasa al controlador de eventos para determinar si se produjo un error y de qué tipo de error se trata. Consulta [Tutorial de control de errores en XAML y C#](error-handling-in-xamlc-walkthrough.md) para obtener más información.

    Este ejemplo muestra un controlador de eventos **ErrorOccurred**. El primer fragmento es el marcado de interfaz de usuario de XAML.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Este ejemplo muestra el código C# correspondiente.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    El error más común que provoca una caja negra es "No ad available". Este error significa que no hay ningún anuncio disponible para devolver desde la solicitud.

3.  [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) se comporta con normalidad.

    De manera predeterminada, **AdControl** se contraerá cuando no pueda mostrar un anuncio. Si otros elementos son elementos secundarios del mismo elemento principal, pueden moverse para rellenar el espacio de **AdControl** contraído y expandirse cuando se realice la siguiente solicitud.

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Los anuncios no se actualizan

1.  Comprueba la propiedad [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled). De manera predeterminada, esta propiedad opcional está establecida en **True**. Cuando se establece en **False**, debe usarse el método [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) para recuperar otro anuncio.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Comprueba las llamadas al método **Refresh**. Al usar la actualización automática, **Refresh** no puede usarse para recuperar otro anuncio. Al usar la actualización manual, debe llamarse a **Refresh** solo después de un mínimo de 30 a 60 segundos, en función de la conexión de datos actual del dispositivo.

    Los fragmentos de código siguientes muestran un ejemplo de uso del método **Refresh**. El primer fragmento es el marcado de interfaz de usuario de XAML.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Este fragmento de código muestra un ejemplo del código C# tras el marcado de interfaz de usuario.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl no aparece

1.  Asegúrate de que la funcionalidad **Internet (Client)** esté seleccionada en Package.appxmanifest.

2.  Asegúrate de crear la instancia de **AdControl**. Si no se crean instancias de **AdControl**, no estará disponible.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  Comprueba el id. de aplicación y el id. de unidad de anuncios. Estos identificadores deben coincidir con el Id. de aplicación y el Id. de unidad de anuncio que obtuviste en el centro de partners. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Comprueba los parámetros **Height** y **Width**. Estos deben establecerse en uno de los [tamaños de anuncio admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Asegúrate de que **AdControl** esté agregado a un elemento primario. Para que se muestre, **AdControl** debe agregarse como un elemento secundario a un control primario (por ejemplo, **StackPanel** o **Grid**).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  Comprueba el parámetro **Margin**. **AdControl** debe estar dentro del área visible.

7.  Comprueba la propiedad **Visibility**. La propiedad **Visibility** opcional debe estar establecida en **Visible**.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Comprueba el elemento primario de **AdControl**. El elemento primario debe estar activo y visible.

9. Los valores activos para **ApplicationId** y **AdUnitId** no deben probarse en el emulador. Para garantizar que **AdControl** funcione según lo previsto, usa los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) tanto para **ApplicationId** como para **AdUnitId**.

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>La caja negra parpadea y desaparece

1.  Vuelve a comprobar todos los pasos de la sección [AdControl no aparece](#csharp-adcontrolnotappearing) anterior.

2.  Controla el evento **ErrorOccurred** y usa el mensaje que se pasa al controlador de eventos para determinar si se produjo un error y de qué tipo de error se trata. Consulta [Tutorial de control de errores en XAML y C#](error-handling-in-xamlc-walkthrough.md) para obtener más información.

    Los siguientes ejemplos muestran el código básico necesario para implementar una llamada de error. Este código XAML define un **TextBlock** que se usa para mostrar el mensaje de error.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Este código de C# recupera el mensaje de error y lo muestra en el **TextBlock**.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    El error más común que provoca una caja negra es "No ad available". Este error significa que no hay ningún anuncio disponible para devolver desde la solicitud.

3.  **AdControl** funciona con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Los anuncios no se actualizan

1.  Comprueba si la propiedad [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) de tu **AdControl** está establecida en false. Esta propiedad opcional está establecida en **true** de manera predeterminada. Cuando se establece en **false**, debe usarse el método **Refresh** para recuperar otro anuncio.

2.  Comprueba las llamadas al método [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx). Al usar la actualización automática (**IsAutoRefreshEnabled** es **true**), no puede usarse **Refresh** para recuperar otro anuncio. Al usar la actualización manual (**IsAutoRefreshEnabled** es **false**), debe llamarse a **Refresh** solo después de un mínimo de 30 a 60 segundos, en función de la conexión de datos actual del dispositivo.

    En el ejemplo siguiente se muestra cómo llamar al método **Refresh**.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

 

 

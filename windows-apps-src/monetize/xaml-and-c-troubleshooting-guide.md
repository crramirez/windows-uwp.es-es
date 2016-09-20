---
author: mcleanbyron
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: "Obtén información sobre las soluciones a problemas comunes de desarrollo con las bibliotecas de Microsoft Advertising en aplicaciones XAML."
title: "Guía de solución de problemas de XAML y C#"
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: ef9ad8f8056b17793d7ad8230e410e014edf2c95

---

# Guía de solución de problemas de XAML y C#

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema encontrarás soluciones a problemas comunes de desarrollo con las bibliotecas de Microsoft Advertising en aplicaciones de XAML.

-   [XAML](#xaml)

    -   [AdControl no aparece](#xaml-notappearing)

    -   [La caja negra parpadea y desaparece](#xaml-blackboxblinksdisappears)

    -   [Los anuncios no se actualizan](#xaml-adsnotrefreshing)

-   [C#](#csharp)

    -   [AdControl no aparece](#csharp-adcontrolnotappearing)

    -   [La caja negra parpadea y desaparece](#csharp-blackboxblinksdisappears)

    -   [Los anuncios no se actualizan](#csharp-adsnotrefreshing)

<span id="xaml"/>
## XAML

<span id="xaml-notappearing"/>
### AdControl no aparece

1.  Asegúrate de que la funcionalidad **Internet (Client)** esté seleccionada en Package.appxmanifest.

2.  Comprueba el id. de aplicación y el id. de unidad de anuncios. Estos identificadores deben coincidir con el id. de la aplicación y el id. de unidad de anuncios que obtuviste en el Centro de desarrollo de Windows. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Comprueba las propiedades **Height** y **Width**. Estas deben establecerse en uno de los [tamaños de anuncio admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Comprueba la posición de los elementos. [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) debe estar dentro del área visible.

5.  Comprueba la propiedad **Visibility**. La propiedad **Visibility** opcional no se debe establecer en contraída ni oculta. Esta propiedad puede establecerse en línea (como se muestra a continuación) o en una hoja de estilos externa.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Comprueba la propiedad **IsEnabled**. La propiedad `IsEnabled` opcional debe estar establecida en `True`.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  IsEnabled="True"
                  Width="728" Height="90" />
    ```

7.  Comprueba el elemento primario de **AdControl**. Si el elemento **AdControl** reside en un elemento primario, el elemento primario debe estar activo y visible.

    ``` syntax
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

8.  Asegúrate de que **AdControl** no esté oculto en la ventanilla. **AdControl** debe ser visible para que los anuncios se muestren correctamente.

9.  Los valores activos para **ApplicationId** y **AdUnitId** no deben probarse en el emulador. Para garantizar que **AdControl** funcione según lo previsto, usa los identificadores de prueba tanto para **ApplicationId** como para **AdUnitId** que se encuentran en [Valores del modo de prueba](test-mode-values.md).

<span id="xaml-blackboxblinksdisappears"/>
### La caja negra parpadea y desaparece

1.  Vuelve a comprobar todos los pasos de la sección [AdControl no aparece](#xaml-notappearing) anterior.

2.  Controla el evento **ErrorOccurred** y usa el mensaje que se pasa al controlador de eventos para determinar si se produjo un error y de qué tipo de error se trata. Consulta [Tutorial de control de errores en XAML y C#](error-handling-in-xamlc-walkthrough.md) para obtener más información.

    Este ejemplo muestra un controlador de eventos **ErrorOccurred**. El primer fragmento es el marcado de interfaz de usuario de XAML.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />

    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Este ejemplo muestra el código correspondiente.

    ``` syntax
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.Error.Message;
    }
    ```

    El error más común que provoca una caja negra es "No ad available". Este error significa que no hay ningún anuncio disponible para devolver desde la solicitud.

3.  [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) se comporta con normalidad.

    De manera predeterminada, **AdControl** se contraerá cuando no pueda mostrar un anuncio. Si otros elementos son elementos secundarios del mismo elemento principal, pueden moverse para rellenar el espacio de **AdControl** contraído y expandirse cuando se realice la siguiente solicitud.

<span id="xaml-adsnotrefreshing"/>
### Los anuncios no se actualizan

1.  Comprueba la propiedad [IsAutoRefreshEnabled](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx). De manera predeterminada, esta propiedad opcional está establecida en **True**. Cuando se establece en **False**, debe usarse el método [Refresh](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) para recuperar otro anuncio.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Comprueba las llamadas al método **Refresh**. Al usar la actualización automática, **Refresh** no puede usarse para recuperar otro anuncio. Al usar la actualización manual, debe llamarse a **Refresh** solo después de un mínimo de 30 a 60 segundos, en función de la conexión de datos actual del dispositivo.

    Los fragmentos de código siguientes muestran un ejemplo de uso del método **Refresh**. El primer fragmento es el marcado de interfaz de usuario de XAML.

    ``` syntax
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Este fragmento de código muestra un ejemplo del código C# tras el marcado de interfaz de usuario.

    ``` syntax
    public Ads()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

<span id="csharp"/>
## C\# #

<span id="csharp-adcontrolnotappearing"/>
### AdControl no aparece

1.  Asegúrate de que la funcionalidad **Internet (Client)** esté seleccionada en Package.appxmanifest.

2.  Asegúrate de crear la instancia de **AdControl**. Si no se crean instancias de **AdControl**, no estará disponible.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public sealed partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl()
                {
                    ApplicationId = "{ApplicationID}",
                    AdUnitId = "{AdUnitID}",
                    Height = 90,
                    Width = 728
                };
            }
        }
    }
    ```

3.  Comprueba el id. de aplicación y el id. de unidad de anuncios. Estos identificadores deben coincidir con el id. de la aplicación y el id. de unidad de anuncios que obtuviste en el Centro de desarrollo de Windows. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Comprueba los parámetros **Height** y **Width**. Estos deben establecerse en uno de los [tamaños de anuncio admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Asegúrate de que **AdControl** esté agregado a un elemento primario. Para que se muestre, **AdControl** debe agregarse como un elemento secundario a un control primario (por ejemplo, **StackPanel** o **Grid**).

    ``` syntax
    ContentPanel.Children.Add(adControl);
    ```

6.  Comprueba el parámetro **Margin**. **AdControl** debe estar dentro del área visible.

7.  Comprueba la propiedad **Visibility**. La propiedad **Visibility** opcional debe estar establecida en **Visible**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Comprueba la propiedad **IsEnabled**. La propiedad **IsEnabled** opcional debe estar establecida en **True**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsEnabled = True;
    ```

9.  Comprueba el elemento primario de **AdControl**. El elemento primario debe estar activo y visible.

10. Los valores activos para **ApplicationId** y **AdUnitId** no deben probarse en el emulador. Para garantizar que **AdControl** funcione según lo previsto, usa los identificadores de prueba tanto para **ApplicationId** como para **AdUnitId** que se encuentran en [Valores del modo de prueba](test-mode-values.md).

<span id="csharp-blackboxblinksdisappears"/>
### La caja negra parpadea y desaparece

1.  Vuelve a comprobar todos los pasos de la sección [AdControl no aparece](#csharp-adcontrolnotappearing) anterior.

2.  Controla el evento **ErrorOccurred** y usa el mensaje que se pasa al controlador de eventos para determinar si se produjo un error y de qué tipo de error se trata. Consulta [Tutorial de control de errores en XAML y C#](error-handling-in-xamlc-walkthrough.md) para obtener más información.

    Los siguientes ejemplos muestran el código básico necesario para implementar una llamada de error. Este código XAML define un **TextBlock** que se usa para mostrar el mensaje de error.

    ``` syntax
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Este código de C# recupera el mensaje de error y lo muestra en el **TextBlock**.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl();
                adControl.ApplicationId = "{ApplicationID}";
                adControl.AdUnitId = "{AdUnitID}";
                adControl.Height = 90;
                adControl.Width = 728;
                adControl.ErrorOccurred += (s,e) =>
                {
                    TextBlock1.Text = e.Error.Message;
                };
            }
        }
    }
    ```

    El error más común que provoca una caja negra es "No ad available". Este error significa que no hay ningún anuncio disponible para devolver desde la solicitud.

3.  
            **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

<span id="csharp-adsnotrefreshing"/>
### Los anuncios no se actualizan

1.  Comprueba la propiedad **IsAutoRefreshEnabled**. De manera predeterminada, esta propiedad opcional está establecida en **True**. Cuando se establece en **False**, debe usarse el método **Refresh** para recuperar otro anuncio.

    En el ejemplo siguiente se muestra cómo usar la propiedad **IsAutoRefreshEnabled**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsAutoRefreshEnabled = true;
    ```

2.  Comprueba las llamadas al método **Refresh**. Al usar la actualización automática, **Refresh** no puede usarse para recuperar otro anuncio. Al usar la actualización manual, debe llamarse a **Refresh** solo después de un mínimo de 30 a 60 segundos, en función de la conexión de datos actual del dispositivo.

    En el ejemplo siguiente se muestra cómo llamar al método **Refresh**.

    ``` syntax
    public MainPage()
    {
        InitializeComponent();

        adControl = new AdControl();
        adControl.ApplicationId = "{ApplicationID}";
        adControl.AdUnitId = "{AdUnitID}";
        adControl.Height = 90;
        adControl.Width = 728;
        adControl.IsAutoRefreshEnabled = false;

        ContentPanel.Children.Add(adControl);

        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

 

 



<!--HONumber=Jun16_HO4-->



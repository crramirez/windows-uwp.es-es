---
description: Un tipo de pincel que se crea una textura translúcida.
title: Material acrílico
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0600e66c672a28683befdb7b0090f5455a28c948
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624200"
---
# <a name="acrylic-material"></a>Material acrílico

![imagen principal](images/header-acrylic.svg)

Acrílico es un tipo de [pincel](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) que crean una textura translúcida. Puedes aplicar acrílico a las superficies de la aplicación para agregar profundidad y ayudar a establecer una jerarquía visual.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **API importantes**: [Clase AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [en segundo plano de la propiedad](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylic y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Acrylic es un componente de Fluent Design System que agrega textura física (material) y profundidad a tu aplicación. Para obtener más información, consulta la [Introducción a Fluent Design para UWP](../fluent-design-system/index.md).

 ## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Ejemplos

:::row:::
    :::column span:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/Acrylic">here</a> to open the app and see acrylic in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Tipos de fusiones de acrílicos
La característica más destacable de Acrylic es su transparencia. Hay dos tipos de fusiones de acrílicos que cambian lo que se ve a través del material:
 - El **acrílico en el fondo** revela el tapiz del escritorio y otras ventanas que están detrás de la aplicación que está activa, lo que agrega profundidad entre las ventanas de la aplicación, a las vez que sirve de reconocimiento para las preferencias de personalización del usuario.
 - El **acrílico en la aplicación** agrega una sensación de profundidad en el marco de la aplicación y proporciona foco y jerarquía.

 ![Acrílico en el fondo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico en la aplicación](images/AppAcrylic_DarkTheme.png)

 Capas múltiples superficies acrílico con precaución: varias capas de acrílico en segundo plano pueden crear distraer ilusiones óptico.

## <a name="when-to-use-acrylic"></a>Cuándo usar Acrylic

* Utilice acrílico en la aplicación para admitir la interfaz de usuario, como en las superficies que se pueden superponer contenido cuando se desplaza o interactuar con ellos.
* Utilice acrílico en segundo plano para los elementos de interfaz de usuario transitorios, como menús contextuales flotantes y la interfaz de usuario de la luz dimsissable.<br />Uso acrílico en escenarios transitorios ayuda a mantener una relación visual con el contenido que desencadenó la interfaz de usuario transitorio.

Si utilizas acrílico en la aplicación en las superficies de navegación, considere la posibilidad de extender contenido bajo el panel acrílico para mejorar el flujo en la aplicación. Usar el control NavigationView hará esto por usted automáticamente. Sin embargo, para evitar la creación de un efecto de la creación de bandas, intente no colocar varios fragmentos de acrílico de lado a lado - puede crear un límite no deseado entre las dos superficies de difuminado. Acrílico es una herramienta para poner visual armonía a los diseños, pero cuando usa incorrectamente, puede dar lugar a ruido visual.

Tenga en cuenta los siguientes patrones de uso para decidir la mejor manera de incorporar acrílico en su aplicación:

### <a name="horizontal-navigation-or-commanding"></a>Desplazamiento horizontal o comandos

Si la aplicación no es capaz de aprovechar control NavigationView y tiene previsto agregar acrílico por su cuenta, se recomienda usar acrílico relativamente translúcido con una opacidad de un tono 60%.
 - Cuando el panel se abre como una superposición sobre otro contenido de la aplicación, este debe ser [acrílico en la aplicación al 60 %](#acrylic-theme-resources).

![Aplicación de mapas mediante los comandos de horizontal en la aplicación](images/Maps_In_App_Acrylic_1.png)

Además, tener su contenido extender o el desplazamiento en el acrílico en la parte superior le dará la aplicación una experiencia más envolvente y sin problemas.

### <a name="vertical-panes"></a>Paneles verticales

Para paneles verticales o en superficies que ayudan a la sección contenido de la aplicación, se recomienda que usar un fondo opaco en lugar de acrílico. Si los paneles verticales se abrirán encima del contenido, como del control NavigationView **Compact** o **mínimo** modos, se recomienda usar acrílico en la aplicación para ayudar a mantener el contexto de la página cuando el usuario tiene este panel abierto.

### <a name="transient-surfaces"></a>Superficies transitorias

Para las aplicaciones con menús flotantes, menús emergentes no modal, o descarte ligero paneles, se recomienda usar acrílico en segundo plano.

![Patrón de aplicación de correo electrónico con un control flotante informativo](images/Mail_TransientContextMenu.png)

Muchos de nuestros controles usará acrílico de forma predeterminada. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) y controles similares con elementos emergentes de luz dimiss todos usarán el acrílico transitorio cuando se invocan.

> [!Note]
> Representación superficies acrílico consume GPU, que puede aumentar el consumo de energía del dispositivo y acortar la duración de la batería. Efectos acrílico se deshabilitan automáticamente cuando los dispositivos al modo de ahorro de batería, y los usuarios pueden deshabilitar efectos acrílico para todas las aplicaciones, si así lo eligen.

## <a name="usability-and-adaptability"></a>Facilidad de uso y adaptabilidad
Acrylic adapta automáticamente su apariencia a una amplia variedad de dispositivos y contextos.

En el modo de contraste alto, los usuarios siguen viendo el color de fondo conocido de su elección en lugar del acrílico. Además, acrílico en segundo plano y en la aplicación acrílico aparecen como un color sólido:
 - Cuando el usuario desactiva la transparencia en Configuración > personalización > Color
 - Cuando se activa el modo de ahorro de batería
 - Cuando la aplicación se ejecuta en hardware de gama baja

Además, solo los acrílico en segundo plano reemplazará su transparencia y la textura con un color sólido:
 - Cuando se desactiva una ventana de la aplicación en el escritorio
 - Cuando la aplicación para UWP se ejecuta en modo de teléfono, Xbox, HoloLens o tableta

### <a name="legibility-considerations"></a>Consideraciones sobre la legibilidad
Es importante asegurarse de que cualquier texto que la aplicación presente a los usuarios [cumpla con las relaciones de contraste](../accessibility/accessible-text-requirements.md). Hemos optimizado la receta del acrílico para que el texto negro o blanco de color de alta densidad o incluso gris de color de densidad media cumpla con las relaciones de contraste sobre el acrílico. Los recursos de tema que proporciona la plataforma toman, de manera predeterminada, colores con tono de contraste con una opacidad del 80 %. Al colocar texto de cuerpo de color de alta densidad sobre acrílico, puedes reducir la opacidad del tono a la vez que mantienes la legibilidad. En el modo oscuro, la opacidad del tono puede ser del 70 %, mientras que el acrílico en modo claro cumplirá con las relaciones de contraste a una opacidad del 50 %.

No recomendamos colocar texto con color enfatizado en las superficies acrílicas, ya que estas combinaciones probablemente no aprueben los requisitos mínimos de relación de contraste en tamaños de fuente de 15 px. Intenta evitar colocar [hipervínculos](../controls-and-patterns/hyperlinks.md) sobre elementos acrílicos. Además, si decides personalizar el nivel de color u opacidad del tono acrílico fuera de los valores predeterminados de la plataforma proporcionados por el recurso del tema, ten en cuenta el impacto en la legibilidad.

## <a name="acrylic-theme-resources"></a>Recursos de temas de Acrylic
Puedes aplicar fácilmente acrílico a las superficies de la aplicación con los nuevos recursos de temas AcrylicBrush de XAML o AcrylicBrush predefinido. En primer lugar, tendrás que decidir si vas a usar el acrílico en la aplicación o en el fondo. Para ver las recomendaciones, asegúrate de revisar los patrones comunes de la aplicación que se describieron anteriormente en este artículo.

Hemos creado una colección recursos de temas de pincel para los tipos de acrílico en el fondo y en la aplicación que respetan el tema de la aplicación y se revierten a colores sólidos según sea necesario. Los recursos denominados *AcrylicWindow* representan el acrílico en el fondo, mientras que *AcrylicElement* hace referencia al acrílico en la aplicación.

<table>
    <tr>
        <th align="center">Clave del recurso</th>
        <th align="center">Opacidad del tono</th>
        <th align="center"><a href="color.md">Color de reserva</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80 % </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Uso recomendado:</b> Estos son recursos acrílico de uso general que funcionan bien en una amplia variedad de usos. Si la aplicación usa texto secundario de color AltMedium con un tamaño de texto más pequeño que 18 px, coloca un recurso acrílico al 80 % detrás del texto para <a href="../accessibility/accessible-text-requirements.md">cumplir los requisitos de relación de contraste</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70 % </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Si su aplicación usa texto secundario de color AltMedium con un tamaño de texto de 18px o mayor, puede colocar estos recursos acrílico translúcidos más de 70% detrás del texto. Te recomendamos usar estos recursos en las áreas superiores de comandos y navegación horizontal de la aplicación.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60 % </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> Al colocar solo texto primario de color AltHigh sobre acrílico, la aplicación puede usar estos recursos 60%. Te recomendamos pintar el <a href="../controls-and-patterns/navigationview.md">panel de navegación vertical</a> de tu aplicación; es decir, el menú de hamburguesa, con acrílico al 60 %. </td>
    </tr>
</table>

Además de acrílico con color neutro, también hemos agregado recursos que entonan el acrílico con el color de énfasis especificado por el usuario. Te recomendamos usar el acrílico coloreado con moderación. Para las variantes dark1 y dark2 proporcionadas, coloca el texto blanco o con colores claros de forma coherente con el texto de tema oscuro sobre estos recursos.
<table>
    <tr>
        <th align="center">Clave del recurso</th>
        <th align="center">Opacidad del tono</th>
        <th align="center"><a href="color.md">Colores de tinta y de respaldo</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70 % </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80 % </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70 % </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


Para pintar una superficie específica, aplica uno de los recursos de temas anteriores en los fondos de los elementos, tal como lo harías con cualquier otro recurso de pincel.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>Pincel de acrílico personalizado
Puedes elegir agregar un tono de color al acrílico de tu aplicación para mostrar una personalización de marca o para aportar equilibrio visual con los otros elementos de la página. Para mostrar color en lugar de escala de grises, debes definir tus propios pinceles de acrílico mediante las siguientes propiedades.
 - **TintColor**: La capa de superposición de color o tono. Considera la posibilidad de especificar tanto el valor de color RGB como la opacidad del canal alfa.
 - **TintOpacity**: La opacidad de la capa de tono. Se recomienda un 80% de opacidad como punto de partida, aunque colores distintos pueden tener un aspecto más atractivo en otros translucencies.
 - **TintLuminosityOpacity**: controla la cantidad de saturación que se permite a través de la superficie acrílico en segundo plano.
 - **BackgroundSource**: El indicador para especificar si quieres acrílico en el fondo o en la aplicación.
 - **FallbackColor**: el color sólido que reemplaza acrílico en ahorro de batería. Para el acrílico en el fondo, el color de reversión también reemplaza al acrílico cuando la aplicación no está en la ventana activa del escritorio o cuando la aplicación se ejecuta en un teléfono o Xbox.

![Muestras de acrílico con tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Muestras de acrílico con tema oscuro](images/CustomAcrylic_Swatches_DarkTheme.png)

![Opactity luminosidad en comparación con la opacidad del tono](images/LuminosityVersusTint.png)

Para agregar un pincel de acrílico, define los tres recursos para los temas oscuro, claro y contraste alto. Ten en cuenta que en contraste alto, se recomienda usar una clase SolidColorBrush con el mismo atributo x:Key que la clase AcrylicBrush para oscuro o claro.

> [!Note] 
> Si no especifica un valor TintLuminosityOpacity, el sistema ajustará automáticamente su valor según su TintColor y TintOpacity.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

En el siguiente ejemplo se muestra cómo declarar AcrylicBrush en el código. Si tu aplicación admite varios destinos de sistema operativo, asegúrate de comprobar que esta API está disponible en el equipo del usuario.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>Extender Acrylic a la barra de título

Para aportar un aspecto impecable a la ventana de la aplicación, puedes usar Acrylic en el área de barra de título. En este ejemplo se extiende Acrylic en la barra de título estableciendo las propiedades [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar)[ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor)  y [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) del objeto en [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent).

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Este código se puede colocar en el método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) de tu aplicación (_App.xaml.cs_), después de la llamada a [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), tal y como se muestra aquí, o en la primera página de tu aplicación.

```csharp
// Call your extend acrylic code in the OnLaunched event, after
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

Además, deberás dibujar el título de la aplicación, que normalmente aparece automáticamente en la barra de título, con una clase TextBlock mediante `CaptionTextBlockStyle`. Para obtener más información, consulta [Personalización de la barra de título](../shell/title-bar.md).

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar
* Usa el acrílico como material de fondo de superficies no principales de la aplicación, como los paneles de navegación.
* Extiende el acrílico al menos a uno de los bordes de la aplicación para proporcionar una experiencia uniforme al mezclarlo de manera sutil con lo que rodea a la aplicación.
* No incluya arylic escritorio en superficies de fondo de gran tamaño de la aplicación: esto interrumpe el modelo mental de acrílico que se utiliza principalmente para superficies transitorias.
* No coloques acrílicos en la aplicación y en el fondo adyacentes directamente, para evitar tensión visual en las costuras.
* No coloques varios paneles acrílicos con el mismo tono y opacidad uno junto a otro, ya que esto genera una costura visible indeseable.
* No coloques texto de color de énfasis sobre superficies acrílicas.

## <a name="how-we-designed-acrylic"></a>Cómo se diseñó Acrylic

Ajustamos los componentes clave de Acrylic para llegar a sus propiedades y apariencia exclusivas. Comenzamos con translucidez, desenfoque y ruido para agregar profundidad visual y la dimensión a las superficies planas. Agregamos una capa de modo de fusión de exclusión para asegurar el contraste y la legibilidad de la interfaz de usuario colocada en un fondo acrílico. Por último, agregamos tono de color para brindar oportunidades de personalización. En conjunto, estas capas dan como resultado un material fresco y utilizable.

![Receta acrílico](images/AcrylicRecipe_Diagram.jpg)
<br/>La receta de Acrylic: fondo, desenfoque, fusión de exclusión, superposición de color y tono, ruido


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

[**Mostrar resaltado**](reveal.md)

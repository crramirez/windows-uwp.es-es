---
description: Un tipo de pincel que crea una textura translúcida.
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
ms.openlocfilehash: 7ed9feb6edae58b906d4f37f0e1f011d4fa42ee9
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867732"
---
# <a name="acrylic-material"></a>Material acrílico

![imagen principal](images/header-acrylic.svg)

Acrílico es un tipo de [pincel](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) que crea una textura translúcida. Puedes aplicar acrílico a las superficies de la app para agregar profundidad y ayudar a establecer una jerarquía visual.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **API importantes**: [clase AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [propiedad Background](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
Acrílico en el tema claro ![Acrílico en el tema claro](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
Acrílico en el tema oscuro ![Acrílico en el tema oscuro](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylic y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Acrylic es un componente de Fluent Design System que agrega textura física (material) y profundidad a tu app. Para más información, consulta la [Introducción a Fluent Design para UWP](/windows/apps/fluent-design-system).

 ## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Ejemplos

:::row:::
    :::column span:::
![Una imagen](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML Controls Gallery**<br>
Si tienes instalada la app XAML Controls Gallery, haz clic <a href="xamlcontrolsgallery:/item/Acrylic">aquí</a> para abrir la app y ver Acrylic en acción.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Tipos de fusiones de Acrylic
La característica más destacable de Acrylic es su transparencia. Hay dos tipos de fusiones de acrílico que cambian lo que se ve a través del material:
 - El **acrílico en el fondo** revela el fondo de escritorio y otras ventanas que están detrás de la app que está activa, lo que agrega profundidad entre las ventanas de la app, a las vez que sirve de reconocimiento para las preferencias de personalización del usuario.
 - El **acrílico en la app** agrega una sensación de profundidad en el marco de la app y proporciona foco y jerarquía.

 ![Acrílico en el fondo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico en la aplicación](images/AppAcrylic_DarkTheme.png)

 Te cuidado a la hora de agregar varias superficies de acrílico en capas: varias capas de acrílico en el fondo pueden crear ilusiones ópticas que distraen.

## <a name="when-to-use-acrylic"></a>Cuándo usar acrílico

* Usa acrílico en la app para interfaces de usuario que lo admiten, como en las superficies que pueden superponer contenido cuando se desplaza o se interactuar con ellas.
* Usa acrílico en el fondo para los elementos de interfaz de usuario transitorios, como menús contextuales, controles flotantes y elementos de interfaz de usuario con cierre del elemento por cambio de foco.<br />El uso de Acrylic en escenarios transitorios ayuda a mantener una relación visual con el contenido que desencadenó la interfaz de usuario transitoria.

Si usas acrílico en la app en superficies de navegación, considera la posibilidad de extender el contenido debajo del panel acrílico para mejorar el flujo en la app. Usar NavigationView hará esto automáticamente. Sin embargo, para evitar crear de efecto de bandas, intenta no colocar varios fragmentos de acrílico de un borde a otro, ya que esto puede crear una costura no deseada entre las dos superficies desenfocadas. Acrylic es una herramienta que aporta armonía visual a los diseños, pero, cuando se usa de forma incorrecta, puede generar ruido visual.

Ten en cuenta los siguientes patrones de uso para decidir la mejor manera de incorporar el acrílico en la app:

### <a name="horizontal-navigation-or-commanding"></a>Desplazamiento horizontal o comandos

Si la app no es capaz de aprovechar NavigationView y tienes previsto agregar acrílico por tu cuenta, te recomendamos usar un acrílico relativamente translúcido con opacidad de tono del 60 %.
 - Cuando el panel se abre como una superposición sobre otro contenido de la app, este debe ser [acrílico en la app al 60 %](#acrylic-theme-resources).

![App Mapas que usa comandos de horizontal en la app](images/Maps_In_App_Acrylic_1.png)

Además, hacer que el contenido se extienda o desplace por debajo del acrílico en la parte superior le dará a la app una experiencia más envolvente y agilizada.

### <a name="vertical-panes"></a>Paneles verticales

Para paneles verticales o superficies que ayudan a seccionar el contenido de la app, recomendamos que uses un fondo opaco en lugar de acrílico. Si los paneles verticales se abren por encima del contenido, como en los modos **Compacto** o **Mínimo** de NavigationView, recomendamos usar acrílico en la app para ayudar a mantener el contexto de la página cuando el usuario tiene este panel abierto.

### <a name="transient-surfaces"></a>Superficies transitorias

Para las apps con menús flotantes, elementos emergentes no modales o paneles con cierre del elemento por cambio de foco, recomendamos usar acrílico en el fondo.

![App de correo que usa un control flotante informativo](images/Mail_TransientContextMenu.png)

Muchos de nuestros controles usarán acrílico de manera predeterminada. Los controles [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) y similares con elementos emergentes con cierre del elemento por cambio de foco usarán el acrílico transitorio cuando se invoquen.

> [!Note]
> La representación de superficies acrílicas puede consumir muchos recursos de la GPU, lo que puede aumentar el consumo de energía del dispositivo y reducir la duración de la batería en algunos dispositivos. Los efectos acrílicos se deshabilitan automáticamente cuando los dispositivos entran en el modo de ahorro de batería. Además, los usuarios pueden deshabilitar los efectos acrílicos para todas las apps, si lo desean.

## <a name="usability-and-adaptability"></a>Facilidad de uso y adaptabilidad
Acrylic adapta automáticamente su aspecto para una amplia variedad de dispositivos y contextos.

En el modo de contraste alto, los usuarios siguen viendo el color de fondo conocido de su elección en lugar del acrílico. Además, tanto el acrílico en el fondo como el acrílico en la app se muestran como un color sólido:
 - Cuando el usuario desactiva la transparencia en Configuración > Personalización > Color
 - Cuando se activa el modo de ahorro de batería
 - Cuando la app se ejecuta en hardware de gama baja

Además, solo el acrílico en el fondo reemplazará su translucidez y textura con un color sólido:
 - Cuando se desactiva una ventana de la app en el escritorio
 - Cuando la aplicación para UWP se ejecuta en modo de teléfono, Xbox, HoloLens o tableta

### <a name="legibility-considerations"></a>Consideraciones sobre la legibilidad
Es importante asegurarse de que cualquier texto que la app presente a los usuarios [cumpla con las relaciones de contraste](../accessibility/accessible-text-requirements.md). Hemos optimizado la receta del acrílico para que el texto negro o blanco de color de alta densidad o incluso gris de color de densidad media cumpla con las relaciones de contraste sobre el acrílico. Los recursos de tema que proporciona la plataforma toman, de manera predeterminada, colores con tono de contraste con una opacidad del 80 %. Al colocar texto de cuerpo de color de alta densidad sobre acrílico, puedes reducir la opacidad del tono a la vez que mantienes la legibilidad. En el modo oscuro, la opacidad del tono puede ser del 70 %, mientras que el acrílico en modo claro cumplirá con las relaciones de contraste a una opacidad del 50 %.

No recomendamos colocar texto con color enfatizado en las superficies acrílicas, ya que estas combinaciones probablemente no aprueben los requisitos mínimos de relación de contraste en tamaños de fuente de 15 px. Intenta evitar colocar [hipervínculos](../controls-and-patterns/hyperlinks.md) sobre elementos acrílicos. Además, si decides personalizar el nivel de color u opacidad del tono acrílico fuera de los valores predeterminados de la plataforma proporcionados por el recurso del tema, ten en cuenta el impacto en la legibilidad.

## <a name="acrylic-theme-resources"></a>Recursos de temas de Acrylic
Puedes aplicar fácilmente acrílico a las superficies de la app con los nuevos recursos de temas AcrylicBrush de XAML o AcrylicBrush predefinido. En primer lugar, tendrás que decidir si vas a usar el acrílico en la app o en el fondo. Para ver las recomendaciones, asegúrate de revisar los patrones comunes de la app que se describieron anteriormente en este artículo.

Hemos creado una colección recursos de temas de pincel para los tipos de acrílico en el fondo y en la app que respetan el tema de la app y se revierten a colores sólidos según sea necesario. Los recursos denominados *AcrylicWindow* representan el acrílico en el fondo, mientras que *AcrylicElement* hace referencia al acrílico en la app.

<table>
    <tr>
        <th align="center">Clave del recurso</th>
        <th align="center">Opacidad del tono</th>
        <th align="center"><a href="color.md">Color de reversión</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80 % </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Uso recomendado:</b> estos son los recursos de acrílico de uso general que funcionan bien en una amplia variedad de usos. Si la app usa texto secundario de color AltMedium con un tamaño de texto más pequeño que 18 px, coloca un recurso acrílico al 80 % detrás del texto para <a href="../accessibility/accessible-text-requirements.md">cumplir los requisitos de relación de contraste</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70 % </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> si la app usa texto secundario de color AltMedium con un tamaño de texto de 18 px o más grande, puedes colocar estos recursos acrílicos al 70 % más transparentes detrás del texto. Te recomendamos usar estos recursos en las áreas superiores de comandos y navegación horizontal de la app.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60 % </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Uso recomendado:</b> al colocar solo el texto principal de color AltHigh sobre el acrílico, la app puede usar estos recursos al 60 %. Te recomendamos pintar el <a href="../controls-and-patterns/navigationview.md">panel de navegación vertical</a> de tu app; es decir, el menú de hamburguesa, con acrílico al 60 %. </td>
    </tr>
</table>

Además de acrílico con color neutro, también hemos agregado recursos que entonan el acrílico con el color de énfasis especificado por el usuario. Te recomendamos usar el acrílico coloreado con moderación. Para las variantes dark1 y dark2 proporcionadas, coloca el texto blanco o con colores claros de forma coherente con el texto de tema oscuro sobre estos recursos.
<table>
    <tr>
        <th align="center">Clave del recurso</th>
        <th align="center">Opacidad del tono</th>
        <th align="center"><a href="color.md">Colores de tono y de reversión</a> </th>
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
Puedes elegir agregar un tono de color al acrílico de tu app para mostrar una personalización de marca o para aportar equilibrio visual con los otros elementos de la página. Para mostrar color en lugar de escala de grises, debes definir tus propios pinceles de acrílico mediante las siguientes propiedades.
 - **TintColor**: la capa de superposición de color o tono. Considera la posibilidad de especificar tanto el valor de color RGB como la opacidad del canal alfa.
 - **TintOpacity**: la opacidad de la capa de tono. Recomendamos una opacidad del 80 % como punto de partida, aunque los distintos colores pueden tener un aspecto más atractivo en otras transparencias.
 - **TintLuminosityOpacity**: controla la cantidad de saturación que se permite a través de la superficie acrílica desde el fondo.
 - **BackgroundSource**: el indicador para especificar si quieres acrílico en el fondo o en la app.
 - **FallbackColor**: el color sólido que reemplaza al acrílico en el modo de ahorro de batería. Para el acrílico en el fondo, el color de reversión también reemplaza al acrílico cuando la app no está en la ventana activa del escritorio o cuando la app se ejecuta en un teléfono o Xbox.

![Muestras de acrílico con tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Muestras de acrílico con tema oscuro](images/CustomAcrylic_Swatches_DarkTheme.png)

![Opacidad de luminosidad en comparación con la opacidad del tono](images/LuminosityVersusTint.png)

Para agregar un pincel de acrílico, define los tres recursos para los temas oscuro, claro y contraste alto. Ten en cuenta que en contraste alto, recomendamos usar una clase SolidColorBrush con el mismo atributo x:Key que la clase AcrylicBrush para oscuro o claro.

> [!Note] 
> Si no especificas un valor TintLuminosityOpacity, el sistema ajustará automáticamente su valor según TintColor y TintOpacity.

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

En el siguiente ejemplo se muestra cómo declarar AcrylicBrush en el código. Si tu app admite varios destinos de sistema operativo, asegúrate de comprobar que esta API esté disponible en el equipo del usuario.

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

Para aportar un aspecto impecable a la ventana de la app, puedes usar Acrylic en el área de barra de título. En este ejemplo se extiende Acrylic en la barra de título al establecer las propiedades [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) y [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) del objeto [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) en [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent).

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Este código se puede colocar en el método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) de la app (_App.xaml.cs_), después de la llamada a [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), tal y como se muestra aquí, o en la primera página de la app.

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

Además, tendrás que dibujar el título de la app, que normalmente aparece automáticamente en la barra de título, con una clase TextBlock mediante `CaptionTextBlockStyle`. Para más información, consulta [Personalización de la barra de título](../shell/title-bar.md).

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar
* Usa el acrílico como material de fondo de superficies no principales de la app, como los paneles de navegación.
* Extiende el acrílico al menos a uno de los bordes de la app para proporcionar una experiencia uniforme al mezclarlo de manera sutil con lo que rodea a la app.
* No apliques acrílico de escritorio en superficies de fondo de gran tamaño de la app; esto interrumpe el modelo mental del acrílico que se usa principalmente para las superficies transitorias.
* No coloques acrílicos en la app y en el fondo adyacentes directamente, para evitar tensión visual en las costuras.
* No coloques varios paneles acrílicos con el mismo tono y opacidad uno junto a otro, ya que esto genera una costura visible indeseable.
* No coloques texto de color de énfasis sobre superficies acrílicas.

## <a name="how-we-designed-acrylic"></a>Cómo se diseñó Acrylic

Ajustamos los componentes clave de Acrylic para llegar a sus propiedades y aspecto exclusivos. Empezamos con translucidez, desenfoque y ruido para agregar profundidad y dimensión visuales a las superficies planas. Agregamos una capa de modo de fusión de exclusión para asegurar el contraste y la legibilidad de la interfaz de usuario colocada en un fondo acrílico. Por último, agregamos tono de color para proporcionar oportunidades de personalización. En conjunto, estas capas dan como resultado un material fresco y utilizable.

![Receta de Acrylic](images/AcrylicRecipe_Diagram.jpg)
<br/>La receta de Acrylic: fondo, desenfoque, fusión de exclusión, superposición de color y tono, ruido


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

[**Revelar resaltado**](reveal.md)

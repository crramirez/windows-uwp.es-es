---
author: mijacobs
description: Un tipo de Brush que crea una textura parcialmente transparente.
title: Material acrílico
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 21fccc72081d1825cad91e9d44bdc458c62d99d4
ms.sourcegitcommit: 9666ef4cf5bb63dd62ee95f89a6ad0ac1bf7ac9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="acrylic-material"></a>Material acrílico

Acrylic es un tipo de [Brush](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) que crea una textura parcialmente transparente. Puedes aplicar acrílico a las superficies de la aplicación para agregar profundidad y ayudar a establecer una jerarquía visual.  <!-- By allowing user-selected wallpaper or colors to shine through, Acrylic keeps users in touch with the OS personalization they've chosen. -->

> **API importantes**: [clase AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [propiedad Background](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Background)


![Acrylic en tema claro](images/Acrylic_DarkTheme_Base.png)

![Acrylic en tema oscuro](images/Acrylic_LightTheme_Base.png)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Acrylic">abrir la aplicación y ver Acrylic en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylic y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Acrylic es un componente de Fluent Design System que agrega textura física (material) y profundidad a tu aplicación. Para obtener más información, consulta la [Introducción a Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="when-to-use-acrylic"></a>Cuándo usar Acrylic

Te recomendamos que coloques la interfaz de usuario de apoyo, como la navegación en la aplicación o los elementos de comandos, en una superficie acrílica. Este material también resulta útil para los elementos transitorios de la interfaz de usuario, como cuadros de diálogo y controles flotantes, ya que ayuda a mantener una relación visual con el contenido que desencadenó la interfaz de usuario transitoria. Hemos diseñado Acrylic para usarse como material de fondo y que aparezca en paneles visualmente discretos, así que no lo apliques a elementos detallados en primer plano.

Las superficies detrás del contenido principal de la aplicación deben usar fondos sólidos y opacos.

Ten en cuenta la posibilidad de hacer que el acrílico se extienda a uno o más de los bordes de la aplicación, incluida la barra de título de la ventana, para mejorar el flujo visual. Evita crear un efecto fragmentado al apilar acrílicos de distintos tipos de fusión adyacentes unos a otros. Acrylic es una herramienta que aporta armonía visual a los diseños, pero, cuando se usa de forma incorrecta, puede generar ruido visual.

Ten en cuenta los siguientes patrones de uso para decidir la mejor manera de incorporar el acrílico en la aplicación.

### <a name="vertical-acrylic-pane"></a>Panel acrílico vertical

Para las aplicaciones con navegación vertical, se recomienda aplicar Acrylic en el panel secundario que contiene los elementos de navegación.

![Patrón de la aplicación con un solo panel acrílico vertical](images/acrylic_app-pattern_vertical.png)

[NavigationView](../controls-and-patterns/navigationview.md) es un nuevo control común para agregar navegación a la aplicación e incluye el acrílico en su diseño visual. El panel de NavigationView muestra acrílico en el fondo cuando el panel está abierto en paralelo con el contenido principal y se transforma automáticamente a acrílico en la aplicación cuando el panel se abre como una superposición.

Si la aplicación no es capaz de aprovechar NavigationView y piensas en agregar acrílico por tu cuenta, te recomendamos usar un acrílico relativamente transparente con opacidad de tono del 60%.
 - Cuando el panel se abre como una superposición sobre otro contenido de la aplicación, este debe ser [acrílico en la aplicación al 60%](#acrylic-theme-resources).
 - Cuando el panel abre en paralelo con el contenido principal de la aplicación, este debe ser [acrílico en el fondo al 60%](#acrylic-theme-resources).

### <a name="multiple-acrylic-panes"></a>Varios paneles acrílicos

Para las aplicaciones con tres paneles verticales distintos, te recomendamos agregar acrílico al contenido que no es principal.
 - Para el panel secundario más cercano al contenido principal, usa [acrílico en el fondo al 80%](#acrylic-theme-resources).
 - Para el panel terciario más alejado del contenido principal, usa [acrílico en el fondo al 60%](#acrylic-theme-resources).

![Patrón de la aplicación con dos paneles acrílicos verticales](images/acrylic_app-pattern_double-vertical.png)

### <a name="horizontal-acrylic-pane"></a>Panel acrílico horizontal

Para las aplicaciones con navegación horizontal, comandos u otros elementos horizontales destacados en la parte superior de la aplicación, te recomendamos aplicar un [acrílico al 70%](#acrylic-theme-resources) en este elemento visual.

![Patrón de la aplicación con un panel acrílico horizontal](images/acrylic_app-pattern_horizontal.png)

Las aplicaciones de lienzo con énfasis en contenido ampliable continuo deberían usar acrílico en la aplicación en la barra superior para que los usuarios puedan conectarse con este contenido. Entre los ejemplos de aplicaciones de lienzo se incluyen mapas, pinturas y dibujos.

Para las aplicaciones sin un único lienzo continuo, te recomendamos usar acrílico en el fondo para conectar a los usuarios con su entorno de escritorio general.

### <a name="acrylic-in-utility-apps"></a>Acrylic en aplicaciones de utilidades

Los widgets o las aplicaciones ligeras pueden reforzar su uso como aplicaciones de utilidad al dibujar acrílico de extremo a extremo dentro de su aplicación. Las aplicaciones que pertenecen a esta categoría suelen tener tiempos de participación del usuario cortos y es bastante improbable que ocupen toda la pantalla de escritorio del usuario. Algunos ejemplos incluyen la calculadora y el centro de actividades.

![Aplicación de utilidad de calculadora con acrílico en todo el fondo](images/acrylic_app-pattern_full.png)

> [!Note]
> La representación de superficies acrílicas puede consumir mucha GPU, lo que puede aumentar el consumo de energía del dispositivo y reducir la duración de la batería en algunos dispositivos. Los efectos acrílicos se deshabilitan automáticamente cuando los dispositivos entran en el modo de ahorro de batería; además, los usuarios pueden deshabilitar los efectos acrílicos para todas las aplicaciones, si lo desean.


## <a name="acrylic-blend-types"></a>Tipos de fusiones de acrílicos
La característica más destacable de Acrylic es su transparencia. Hay dos tipos de fusiones de acrílicos que cambian lo que se ve a través del material:
 - El **acrílico en el fondo** revela el tapiz del escritorio y otras ventanas que están detrás de la aplicación que está activa, lo que agrega profundidad entre las ventanas de la aplicación, a las vez que sirve de reconocimiento para las preferencias de personalización del usuario.
 - El **acrílico en la aplicación** agrega una sensación de profundidad en el marco de la aplicación y proporciona foco y jerarquía.

 ![Acrílico en el fondo](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrílico en la aplicación](images/AppAcrylic_DarkTheme.png)

 Dispón varias superficies acrílicas con precaución. El acrílico en el fondo, como su nombre lo indica, no debe estar más cercana del usuario en el ordenz. Varias capas de acrílico en el fondo suelen provocar ilusiones ópticas inesperadas, por lo que deben evitarse. Si decides aplicar acrílicos en capa, hazlo con acrílicos en la aplicación y ten en cuenta la posibilidad de que el tono del acrílico tenga un valor más claro para ayudar a que las capas estén visualmente más próximas al usuario.


## <a name="usability-and-adaptability"></a>Facilidad de uso y adaptabilidad
Acrylic adapta automáticamente su apariencia a una amplia variedad de dispositivos y contextos.

En el modo de contraste alto, los usuarios siguen viendo el color de fondo conocido de su elección en lugar del acrílico. Además, tanto el acrílico en el fondo como el acrílico en la aplicación se muestran como un color sólido:
 - Cuando el usuario desactiva la transparencia en Configuración de personalización
 - Cuando se activa el modo de ahorro de batería
 - Cuando la aplicación se ejecuta en hardware de gama baja

Además, solo el acrílico en el fondo reemplazará su transparencia y textura con un color sólido:
 - Cuando se desactiva una ventana de la aplicación en el escritorio
 - Cuando la aplicación para UWP se ejecuta en modo de teléfono, Xbox, HoloLens o tableta

### <a name="legibility-considerations"></a>Consideraciones sobre la legibilidad
Es importante asegurarse de que cualquier texto que la aplicación presente a los usuarios [cumpla con las relaciones de contraste](../accessibility/accessible-text-requirements.md). Hemos optimizado la receta del acrílico para que el texto negro o blanco de color de alta densidad o incluso gris de color de densidad media cumpla con las relaciones de contraste sobre el acrílico. Los recursos de tema que proporciona la plataforma toman, de manera predeterminada, colores con tono de contraste con una opacidad del 80%. Al colocar texto de cuerpo de color de alta densidad sobre acrílico, puedes reducir la opacidad del tono a la vez que mantienes la legibilidad. En el modo oscuro, la opacidad del tono puede ser del 70%, mientras que el acrílico en modo claro cumplirá con las relaciones de contraste a una opacidad del 50%.

No recomendamos colocar texto con color enfatizado en las superficies acrílicas, ya que estas combinaciones probablemente no aprueben los requisitos mínimos de relación de contraste en tamaños de fuente de 15px. Intenta evitar colocar [hipervínculos](../controls-and-patterns/hyperlinks.md) sobre elementos acrílicos. Además, si decides personalizar el nivel de color u opacidad del tono acrílico fuera de los valores predeterminados de la plataforma proporcionados por el recurso del tema, ten en cuenta el impacto en la legibilidad.

## <a name="acrylic-theme-resources"></a>Recursos de temas de Acrylic
Puedes aplicar fácilmente acrílico a las superficies de la aplicación con los nuevos recursos de temas AcrylicBrush de XAML o AcrylicBrush predefinido. En primer lugar, tendrás que decidir si vas a usar el acrílico en la aplicación o en el fondo. Para ver las recomendaciones, asegúrate de revisar los patrones comunes de la aplicación que se describieron anteriormente en este artículo.

Hemos creado una colección recursos de temas de pincel para los tipos de acrílico en el fondo y en la aplicación que respetan el tema de la aplicación y se revierten a colores sólidos según sea necesario. Los recursos denominados *AcrylicWindow* representan el acrílico en el fondo, mientras que *AcrylicElement* hace referencia al acrílico en la aplicación.

<table>
    <tr>
        <th align="center">Clave del recurso</th>
        <th align="center">Opacidad del tono</th>
        <th align="center">[Color de reversión](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> **Uso recomendado:** Estos son los recursos de acrílico de uso general que funcionan bien en una amplia variedad de usos. Si la aplicación usa texto secundario de color AltMedium con un tamaño de texto más pequeño que 18px, coloca un recurso acrílico al 80% detrás del texto para [cumplir los requisitos de relación de contraste](../accessibility/accessible-text-requirements.md). </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> **Uso recomendado:** Si la aplicación usa texto secundario de color AltMedium con un tamaño de texto de 18px o más grande, puedes colocar estos recursos acrílicos al 70% más transparentes detrás del texto. Te recomendamos usar estos recursos en las áreas superiores de comandos y navegación horizontal de la aplicación.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> **Uso recomendado:** Al colocar solo el texto principal de color AltHigh sobre el acrílico, la aplicación puede usar estos recursos al 60%. Te recomendamos pintar el [panel de navegación vertical](../controls-and-patterns/navigationview.md) de tu aplicación; es decir, el menú de hamburguesa, con acrílico al 60%. </td>
    </tr>
</table>

Además de acrílico con color neutro, también hemos agregado recursos que entonan el acrílico con el color de énfasis especificado por el usuario. Te recomendamos usar el acrílico coloreado con moderación. Para las variantes dark1 y dark2 proporcionadas, coloca el texto blanco o con colores claros de forma coherente con el texto de tema oscuro sobre estos recursos.
<table>
    <tr>
        <th align="center">Clave del recurso</th>
        <th align="center">Opacidad del tono</th>
        <th align="center">[Colores de tono y de reversión](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
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
 - **TintOpacity**: La opacidad de la capa de tono. Recomendamos una opacidad del 80% como punto de partida, aunque los distintos colores pueden tener una apariencia más atractiva en otras transparencias.
 - **BackgroundSource**: El indicador para especificar si quieres acrílico en el fondo o en la aplicación.
 - **FallbackColor**: El color sólido que reemplaza al acrílico en el modo de batería baja. Para el acrílico en el fondo, el color de reversión también reemplaza al acrílico cuando la aplicación no está en la ventana activa del escritorio o cuando la aplicación se ejecuta en un teléfono o Xbox.


![Muestras de acrílico con tema claro](images/CustomAcrylic_Swatches_LightTheme.png)

![Muestras de acrílico con tema oscuro](images/CustomAcrylic_Swatches_DarkTheme.png)

Para agregar un pincel de acrílico, define los tres recursos para los temas oscuro, claro y contraste alto. Ten en cuenta que en contraste alto, se recomienda usar una clase SolidColorBrush con el mismo atributo x:Key que la clase AcrylicBrush para oscuro o claro.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
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

Para aportar un aspecto impecable a la ventana de la aplicación, puedes usar Acrylic en el área de barra de título. En este ejemplo se extiende Acrylic en la barra de título estableciendo las propiedades [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar#Windows_UI_ViewManagement_ApplicationViewTitleBar_ButtonBackgroundColor)  y [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar#Windows_UI_ViewManagement_ApplicationViewTitleBar_ButtonInactiveBackgroundColor) del objeto en [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors#Windows_UI_Colors_Transparent). 

```csharp
/// Extend acrylic into the title bar. 
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Este código se puede colocar en el método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) de tu aplicación (_App.xaml.cs_), después de la llamada a [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window#Windows_UI_Xaml_Window_Activate), tal y como se muestra aquí, o en la primera página de tu aplicación. 


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

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer
* Usa el acrílico como material de fondo de superficies no principales de la aplicación, como los paneles de navegación.
* Extiende el acrílico al menos a uno de los bordes de la aplicación para proporcionar una experiencia uniforme al mezclarlo de manera sutil con lo que rodea a la aplicación.
* No coloques acrílicos en la aplicación y en el fondo adyacentes directamente, para evitar tensión visual en las costuras.
* No coloques varios paneles acrílicos con el mismo tono y opacidad uno junto a otro, ya que esto genera una costura visible indeseable.
* No coloques texto de color de énfasis sobre superficies acrílicas.

## <a name="how-we-designed-acrylic"></a>Cómo se diseñó Acrylic

Ajustamos los componentes clave de Acrylic para llegar a sus propiedades y apariencia exclusivas. Empezamos con transparencia, desenfoque y ruido para agregar profundidad y dimensión visuales a las superficies planas. Agregamos una capa de modo de fusión de exclusión para asegurar el contraste y la legibilidad de la interfaz de usuario colocada en un fondo acrílico. Por último, agregamos tono de color para brindar oportunidades de personalización. En conjunto, estas capas dan como resultado un material fresco y utilizable.

![Receta de Acrylic](images/AcrylicRecipe_Diagram.png)
<br/>La receta de Acrylic: fondo, desenfoque, fusión de exclusión, superposición de color y tono, ruido

<!--
<div class="microsoft-internal-note">
When designing your app, please utilize these [design resources](http://uni/DesignDepot.FrontEnd/#/Search?t=Resources%7CNeon%7CToolkit&f=Acrylic%20Material) to show acrylic in comps. The linked templates are the most accurate way to represent acrylic material in Photoshop and Illustrator. The ordering, as noted in the recipe diagram above, should start from the top: <br/>
 - Noise asset (tiled) at 2% opacity <br/>
 - Base color/tint/alpha layer <br/>
 - Exclusion blend (white @ 10% opacity) <br/>
 - Gaussian blur (30px radius) <br/>
</div>
-->

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

[**Características principales de Reveal**](reveal.md)

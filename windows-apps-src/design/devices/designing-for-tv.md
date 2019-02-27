---
Description: Design your app so that it looks good and functions well on your television.
title: Diseño para Xbox y televisión
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, experiencia de 10 pies, controlador para juegos, control remoto, entrada, interacción
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 431b8912e43647bc2678aaab7efc9ec68b866d10
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117585"
---
# <a name="designing-for-xbox-and-tv"></a>Diseño para Xbox y televisión

Diseña tu aplicación para la Plataforma universal de Windows (UWP) de manera que se vea y funcione bien en la Xbox One y las pantallas de televisión.

Para obtener instrucciones sobre experiencias de interacción en aplicaciones para UWP en la experiencia de *10 pies* , consulta [interacciones del controlador para juegos y control remoto](../input/gamepad-and-remote-interactions.md) .

## <a name="overview"></a>Información general

La Plataforma universal de Windows (UWP) te permite crear experiencias agradables en varios dispositivos de Windows 10.
La mayoría de la funcionalidad proporcionada por el entorno de desarrollo UWP les permite a las aplicaciones usar la misma interfaz de usuario (UI) en cualquiera de estos dispositivos, sin realizar ningún trabajo adicional.
Sin embargo, personalizar y optimizar tu aplicación para que funcione bien en Xbox One y pantallas de televisión requiere consideraciones especiales.

La experiencia de estar sentado en tu sofá al otro lado de la sala, con un controlador para juegos o control remoto para interactuar con tu televisor, se llama la **experiencia de 10 pies**.
Se denomina así porque el usuario generalmente está sentado aproximadamente a 10 pies de distancia de la pantalla.
Esto proporciona desafíos que no están presentes, por ejemplo, en la experiencia de *2 pies* o interactuando con un PC.
Si estás desarrollando una aplicación para Xbox One o cualquier otro dispositivo que se muestra en una pantalla de TV y usa un controlador para la entrada, siempre debes tener esto en cuenta.

No todos los pasos de este artículo son necesarios para que tu aplicación funcione bien en experiencias de 10 pies, pero entenderlos y tomar las decisiones apropiadas para tu aplicación dará como resultado una mejor experiencia de 10 pies adaptada a las necesidades específicas de tu aplicación.
Al traer a la vida a tu aplicación en el entorno de 10 pies, ten en cuenta los siguientes principios de diseño.

### <a name="simple"></a>Simple

El diseño para el entorno de 10 pies presenta un conjunto único de desafíos. La resolución y distancia de visualización pueden dificultar a la gente el procesar demasiada información.
Intenta mantener tu diseño limpio y reducido a los componentes más simples posibles. La cantidad de información que se muestra en un televisor debe ser comparable a lo que verías en un teléfono móvil, más que en un equipo de escritorio.

![Pantalla principal de Xbox One](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>Coherente

Las aplicaciones para UWP en el entorno de 10 pies deben ser intuitivas y fáciles de usar. Haz que el enfoque sea claro e inequívoco.
Organiza el contenido para que el movimiento a través del espacio sea consistente y predecible. Dale a las personas la ruta más corta hacia lo quieran hacer.

![Aplicación para películas de Xbox One](images/designing-for-tv/xbox-movies-app.png)

_**Todas las películas que se muestran en la captura de pantalla están disponibles en Películas y TV de Microsoft.**_  

### <a name="captivating"></a>Fascinantes

Las experiencias más envolventes y cinematográficas realizadas en la pantalla grande. Los escenarios de extremo a extremo, el movimiento elegante y un vibrante uso del color y la tipografía llevan tus aplicaciones al siguiente nivel. Sea valiente y hermoso.

![Aplicación para avatares de Xbox One](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Optimizaciones para la experiencia de 10 pies

Ahora que conoces los principios del buen diseño de aplicaciones para experiencia de 10 pies en UWP, lee la siguiente información general sobre las formas específicas en las puedes optimizar tu aplicación y proporcionar una gran experiencia al usuario.

| Función        | Descripción           |
| -------------------------------------------------------------- |--------------------------------|
| [Variación del tamaño del elemento de la interfaz de usuario](#ui-element-sizing)  | La Plataforma universal de Windows usa [píxeles efectivos y ajuste de escala](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) para escalar la interfaz de usuario según la distancia de visualización. Entender la variación de tamaño y aplicarla a toda tu interfaz de usuario te ayudarán a optimizar tu aplicación para el entorno de 10 pies.  |
|  [Zona segura del televisor](#tv-safe-area) | La UWP automáticamente evitará mostrar cualquier interfaz de usuario en áreas no seguras de los televisores (áreas cercanas a los bordes de la pantalla) de forma predeterminada. Sin embargo, esto crea un efecto de "encajonamiento" en la que la interfaz de usuario queda encerrada por un marco. Para que tu aplicación sea verdaderamente envolvente en el televisor, es conveniente modificarla para que se extiende hasta los bordes de la pantalla en los televisores que lo admiten. |
| [Colores](#colors)  |  La UWP admite temas de colores y una aplicación que respeta el tema del sistema tendrá como valor predeterminado **oscuro** en Xbox One. Si tu aplicación tiene un tema de color específico, debes tener en cuenta que algunos colores no funcionan bien en los televisores y deben evitarse. |
| [Sonido](../style/sound.md)    | Los sonidos desempeñan un papel esencial en la experiencia de 10 pies, ya que ayudan a sumergir y a proporcionar información al usuario. El UWP proporciona funciones que activan automáticamente los sonidos de los controles habituales cuando la aplicación se ejecuta en Xbox One. Obtén más información sobre la compatibilidad de audio integrada en el UWP y descubre cómo sacarle provecho.    |
| [Directrices sobre controles de la interfaz de usuario](#guidelines-for-ui-controls)  |  Hay varios controles de interfaz de usuario que funcionan bien en varios dispositivos, pero tienen ciertas consideraciones cuando se usan en televisores. Obtén información sobre algunos procedimientos recomendados para usar estos controles al diseñar la experiencia de 10 pies. |
| [Desencadenador de estado visual personalizado para Xbox](#custom-visual-state-trigger-for-xbox) | A fin de adaptar tu aplicación para UWP a la experiencia de 10 pies, te recomendamos usar un *desencadenador de estado visual* personalizado para realizar cambios de diseño cuando la aplicación detecte que se ha iniciado en una consola Xbox. |

Además del diseño anterior y consideraciones de diseño, hay una serie de las optimizaciones del [controlador para juegos y control remoto interacción](../input/gamepad-and-remote-interactions.md) que debe tener en cuenta al compilar la aplicación.

| Función        | Descripción           |
| -------------------------------------------------------------- |--------------------------------|
| [Interacción y navegación con foco XY](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **Navegación con foco XY** permite al usuario navegar por la interfaz de usuario de la aplicación. Sin embargo, esto limita al usuario a navegar hacia arriba, abajo, izquierda y derecha. En esta sección se describen recomendaciones para lidiar con esta y otras consideraciones. |
| [Modo de mouse](../input/gamepad-and-remote-interactions.md#mouse-mode)|Navegación con foco XY no es práctico, o incluso posible, para algunos tipos de aplicaciones, como mapas o dibujar y las aplicaciones de dibujo. En estos casos, **el modo de mouse** permite a los usuarios navegar libremente con un controlador para juegos o control remoto, al igual que un mouse en un equipo.|
| [Foco visual](../input/gamepad-and-remote-interactions.md#focus-visual)  | El foco visual es un borde que resalta el elemento enfocado actualmente de la interfaz de usuario. Esto ayuda al usuario identificar rápidamente la interfaz de usuario se navega a través de o interactuar con.  |
| [Participación del foco](../input/gamepad-and-remote-interactions.md#focus-engagement) | Participación del foco requiere que el usuario presione el botón **A/seleccionar** en un controlador para juegos o control remoto cuando un elemento de interfaz de usuario tiene el foco con el fin de interactuar con él. |
| [Botones de hardware](../input/gamepad-and-remote-interactions.md#hardware-buttons) | El controlador para juegos y control remoto proporcionan configuraciones y botones muy diferentes. |

> [!NOTE]
> La mayoría de los fragmentos de código de este tema están en XAML/C#; Sin embargo, los principios y conceptos se aplican a todas las aplicaciones para UWP. Si estás desarrollando una aplicación para UWP en HTML/JavaScript para Xbox, echa un vistazo a la excelente biblioteca [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) en GitHub.

## <a name="ui-element-sizing"></a>Variación del tamaño del elemento de la interfaz de usuario

Debido a que el usuario de una aplicación en el entorno de 10 pies usa un control remoto o controlador para juegos y se encuentra a varios pies de la pantalla, hay algunas consideraciones sobre la interfaz de usuario que deben tenerse en cuenta en tu diseño.
Asegúrate de que la interfaz de usuario tenga una densidad de contenido apropiada y que no esté demasiado sobrecargada para que el usuario pueda navegar y seleccionar elementos fácilmente. Recuerda: la simplicidad es clave.

### <a name="scale-factor-and-adaptive-layout"></a>Factor de escala y diseño adaptativo

**El factor de escala** ayuda a garantizar que los elementos de la interfaz de usuario se muestren con el tamaño correcto para el dispositivo en el que se ejecuta la aplicación.
En el escritorio, esta configuración se encuentra en **Configuración > Sistema > Pantalla** como un valor de deslizamiento.
Esta misma configuración existe en el teléfono también si el dispositivo la admite.

![Cambia el tamaño de texto, las aplicaciones y otros elementos](images/designing-for-tv/ui-scaling.png)

En Xbox One, no hay ninguna configuración semejante del sistema; sin embargo, para que los elementos de la interfaz de usuario de la UWP tengan el tamaño apropiado para un televisor, se escalan con un valor predeterminado de **200%** para aplicaciones XAML y **150%** para aplicaciones HTML.
Siempre que los elementos de la interfaz de usuario sean del tamaño apropiado en otros dispositivos, también lo serán en televisores.
Xbox One representa tu aplicación en 1080p (1920 x 1080 píxeles). Por lo tanto, al traer una aplicación desde otros dispositivos tales como un equipo, asegúrate de que la interfaz de usuario se vea bien en 960 x 540 px al 100% de escala (o 1280 x 720 px en 100% de escala para aplicaciones HTML) usando las [técnicas adaptables](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

El diseño para Xbox es un poco diferente al diseño para PC porque solo necesitas preocuparte por una resolución: 1920 x 1080.
No importa si el usuario tiene un televisor con una mayor resolución; las aplicaciones para UWP siempre se ajustarán a 1080p.

Los tamaños correctos de los activos del conjunto de 200% (o 150% para aplicaciones HTML) también se extraerán para tu aplicación cuando se ejecute en Xbox One, independientemente de la resolución del televisor.

### <a name="content-density"></a>Densidad de contenido

Al diseñar tu aplicación, recuerda que el usuario verá la interfaz de usuario desde la distancia e interactuará con ella mediante un control remoto o controlador para juegos, lo que hace que la navegación sea más lenta que con un mouse o entrada táctil.

#### <a name="sizes-of-ui-controls"></a>Tamaños de los controles de la interfaz de usuario

Los elementos interactivos de la interfaz de usuario deben tener una altura mínima de 32 epx (píxeles efectivos). Este es el valor predeterminado de los controles comunes de la UWP, y cuando se usan en la escala del 200%, garantiza que los elementos de la interfaz de usuario serán visibles desde la distancia y ayuda a reducir la densidad de contenido.

![Botón de la UWP en la escala de 100% y 200%](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Número de clics

Cuando el usuario está navegando desde un extremo a otro de la pantalla del televisor, no debería llevarle más de **seis clics** para simplificar tu interfaz de usuario. Nuevamente se aplica aquí el principio de **simplicidad**. 

![6 iconos a lo ancho](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Tamaños del texto

Para hacer que tu interfaz de usuario sea visible desde cierta distancia, usa las siguientes reglas generales:

* Texto principal y contenido de lectura: 15 epx mínimo
* Texto no crítico y contenido adicional: 12 epx mínimo

Cuando uses un texto más grande en tu interfaz de usuario, elige un tamaño que no limite la superficie de la pantalla demasiado, ocupando el espacio que potencialmente podría ocupar otro contenido.

### <a name="opting-out-of-scale-factor"></a>Deshabilitar el factor de escala

Recomendamos que tu aplicación aproveche la compatibilidad con el factor de escala, la cual le ayudará a ejecutarse correctamente en todos los dispositivos mediante un ajuste de escala para cada tipo de dispositivo.
Sin embargo, es posible deshabilitar este comportamiento y diseñar toda tu interfaz de usuario en escala del 100%. Ten en cuenta que no puedes cambiar el factor de escala a un valor distinto de 100%.

En el caso de las aplicaciones XAML, si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código:

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` te notificará si la deshabilitación se ha realizado correctamente.

Para obtener más información, incluido el código de muestra para HTML/JavaScript, consulta [Cómo desactivar el escalado](../../xbox-apps/disable-scaling.md).

Asegúrate de calcular los tamaños adecuados de los elementos de la interfaz de usuario duplicando los valores de píxel *efectivos* mencionados en este tema a los valores de píxel *reales* (o multiplicando por 1,5, en el caso de las aplicaciones HTML).

## <a name="tv-safe-area"></a>Zona segura del televisor

No todos los televisores muestran contenido hasta los extremos de la pantalla, por razones históricas y tecnológicas. De forma predeterminada, la UWP evitará mostrar cualquier contenido de la interfaz de usuario en las áreas no seguras de los televisores y en su lugar, solo dibujará el fondo de la página.

El área no segura de un televisor está representada por el área azul en la siguiente imagen.

![Área no segura para televisores](images/designing-for-tv/tv-unsafe-area.png)

Puedes definir un color estático o de tema, o una imagen para el fondo, como muestran los siguientes fragmentos de código.

### <a name="theme-color"></a>Color del tema

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>Imagen

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

Este será el aspecto de tu aplicación sin realizar ningún trabajo adicional.

![Zona segura del televisor](images/designing-for-tv/tv-safe-area.png)

Esto no es óptimo ya que le proporciona a la aplicación un efecto de "encajonamiento", con elementos de la interfaz de usuario, tales como el panel de navegación y la cuadrícula, que parecen recortados. Sin embargo, puedes realizar optimizaciones para ampliar las partes de la interfaz de usuario hasta los bordes de la pantalla para que la aplicación tenga un efecto más cinematográfico.

### <a name="drawing-ui-to-the-edge"></a>Dibujar la interfaz de usuario hasta el borde

Te recomendamos que uses determinados elementos de interfaz de usuario para llegar hasta los bordes de la pantalla a fin de proporcionar al usuario una mayor inmersión. Estos incluyen [ScrollViewers](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [paneles de navegación](../controls-and-patterns/navigationview.md) y [CommandBars](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar).

Por otro lado, también es importante que el texto y los elementos interactivos eviten siempre los bordes de la pantalla para asegurarte de que no se recortarán en algunos televisores. Te recomendamos que solo dibujes elementos visuales no esenciales dentro del 5% de los bordes de la pantalla. Como se mencionó en [Variación del tamaño del elemento de interfaz de usuario](#ui-element-sizing), una aplicación para UWP que respeta el factor de escala predeterminado de 200% de la consola Xbox One utilizará un área de 960 x 540 epx, por lo que en la interfaz de usuario de tu aplicación debes evitar colocar elementos esenciales en las siguientes áreas:

- a 27 epx de la parte superior e inferior
- a 48 epx de los lados izquierdo y derecho

En las siguientes secciones se describe cómo conseguir que la interfaz de usuario se amplíe hasta los bordes de la pantalla.

#### <a name="core-window-bounds"></a>Límites de la ventana principal

En las aplicaciones para UWP destinadas solo a la experiencia de 10 pies, usar los límites de la ventana principal es la opción más sencilla.

En el método `OnLaunched` de `App.xaml.cs`, agrega el siguiente código:

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Con esta línea de código, la ventana de la aplicación se extenderá hasta los bordes de la pantalla, por lo que tendrás que mover todos los elementos interactivos y esenciales de la interfaz de usuario al área segura del televisor descrita anteriormente. Los elementos de la interfaz de usuario transitorios, tales como los menús contextuales y [ComboBoxes](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) abiertos, permanecerán automáticamente dentro de la zona segura del televisor.

![Límites de la ventana principal](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Fondos del panel

Los paneles de navegación se dibujan normalmente cerca del borde de la pantalla, por lo que debes ampliar el fondo hasta el área no segura del televisor para no generar huecos extraños. Para ello, solo tienes que cambiar el color de fondo del panel de navegación al color de fondo de la aplicación.

El uso de los límites de la ventana principal según lo descrito anteriormente te permitirá dibujar la interfaz de usuario en los bordes de la pantalla, pero deberás usar los márgenes positivos en el contenido de [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) para mantenerlo dentro de la zona segura del televisor.

![Panel de navegación ampliado hasta los bordes de la pantalla](images/designing-for-tv/tv-safe-areas-2.png)

Aquí se ha ampliado fondo del panel de navegación hasta los bordes de la pantalla, mientras que los elementos de navegación se mantienen en la zona segura del televisor.
El contenido de `SplitView` (en este caso, una cuadrícula de elementos) se ha ampliado hasta la parte inferior de la pantalla para que parezca que sigue y no se corte, mientras que la parte superior de la cuadrícula sigue estando dentro de la zona segura del televisor. (Obtén más información sobre cómo hacerlo en [Topes de desplazamiento de listas y cuadrículas](#scrolling-ends-of-lists-and-grids)).

El siguiente fragmento de código logra este efecto:

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) es otro ejemplo de un panel que normalmente se ubica cerca de uno o más de los bordes de la aplicación y, como tal, su fondo se debe ampliar en los televisores hasta los bordes de la pantalla. También suele contener un botón **Más**, representado por "..." en el lado derecho, que debe permanecer en la zona segura del televisor. Las siguientes son algunas estrategias diferentes para lograr las interacciones y los efectos visuales deseados.

**Opción 1**: Cambiar el color de fondo de `CommandBar` a transparente o al mismo color que el fondo de la página:

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

Esto hará parecer que `CommandBar` está sobre el mismo fondo que el resto de la página, por lo tanto, el fondo fluye perfectamente hasta el borde de la pantalla.

**Opción 2**: Agregar un rectángulo de fondo cuyo relleno sea del mismo color que el fondo de `CommandBar` y que quede por debajo de `CommandBar` y cruce el resto de la página:

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> Si usas este enfoque, ten en cuenta que el botón **Más** cambia el alto del elemento `CommandBar` abierto si es necesario, a fin de mostrar las etiquetas de los `AppBarButton` debajo de los iconos. Te recomendamos que muevas las etiquetas a la *derecha* de sus iconos para evitar este cambio de tamaño. Para obtener más información, consulta [Etiquetas de CommandBar](#commandbar-labels).

Ambos enfoques se aplican a los otros tipos de controles que se describen en esta sección.

#### <a name="scrolling-ends-of-lists-and-grids"></a>Topes de desplazamiento de listas y cuadrículas

Es común que las listas y cuadrículas contengan más elementos de los que pueden caber en la pantalla al mismo tiempo. Cuando este es el caso, te recomendamos extender la lista o cuadrícula hasta el borde de la pantalla. Las listas y cuadrículas de desplazamiento horizontal se debe extender hasta el borde derecho y las de desplazamiento vertical se deben extender hasta a la parte inferior.

![Recorte de la cuadrícula del área segura del televisor](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Cuando una lista o cuadrícula se extiende de esta manera, es importante mantener el foco visual y su elemento asociado dentro de la zona segura del televisor.

![El foco de la cuadrícula con desplazamiento debe mantenerse en la zona segura del televisor](images/designing-for-tv/scrolling-grid-focus.png)

El UWP tiene una funcionalidad que conservará el foco visual dentro de [VisibleBounds](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds), pero necesitas agregar espaciado interno para garantizar que los elementos de cuadrícula/lista se puedan desplazar a la vista del área segura. Específicamente, agrega un margen positivo al [ItemsPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter) de la [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) o [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), como en el siguiente fragmento de código:

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

La idea es colocar el fragmento de código anterior en los recursos de la página o en la aplicación y, a continuación, acceder a él de la siguiente manera:

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> Este fragmento de código es específico para `ListView`s; para un estilo `GridView` establece el atributo [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) tanto para [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) como para [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) para conseguir una `GridView`.

Para un mayor control sobre cómo los elementos se incluyen en la vista, si la aplicación está destinada a la versión 1803 o posterior, puedes usar el [evento UIElement.BringIntoViewRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested). Puedes colocarla en la [propiedad ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) para la **ListView**/**GridView** lo capture antes de **ScrollViewer** interno, al igual que en los siguientes fragmentos de código:

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

## <a name="colors"></a>Colores

De manera predeterminada, la plataforma Universal de Windows escala los colores de tu aplicación al intervalo seguro para el televisor, (consulta [Colores seguros para el televisor](#tv-safe-colors) para obtener más información) de modo que la aplicación pueda verse bien en cualquier televisor. Además, hay mejoras que puedes realizar en el conjunto de colores que usa tu aplicación para mejorar la experiencia visual en el televisor.

### <a name="application-theme"></a>Tema de la aplicación

Puedes elegir un **tema de la aplicación** (claro u oscuro) según lo que sea adecuado para tu aplicación, o bien puedes deshabilitar los temas. Obtén más información acerca de las recomendaciones generales para temas en [Temas de colores](../style/color.md).

La UWP también permite que las aplicaciones establezcan dinámicamente el tema en base a la configuración del sistema proporcionada por los dispositivos en los que se ejecutan.
Si bien la UWP siempre respeta la configuración de tema especificada por el usuario, cada dispositivo también proporciona un tema predeterminado adecuado.
Dada la naturaleza de Xbox One, la cual se espera que tengas más experiencias *multimedia* que experiencias de *productividad*, el valor predeterminado es un tema del sistema oscuro.
Si el tema de tu aplicación se basa en la configuración del sistema, espera un valor predeterminado oscuro en Xbox One.

### <a name="accent-color"></a>Color de énfasis

La UWP proporciona una forma cómoda para exponer el **color de énfasis** que el usuario ha seleccionado en su configuración del sistema.

En Xbox One, el usuario es capaz de seleccionar un color de usuario, al igual que puede seleccionar un color de énfasis en un equipo.
Siempre y cuando tu aplicación llame a estos colores de énfasis a través de pinceles o recursos de color, se usará el color que el usuario ha seleccionado en la configuración del sistema. Ten en cuenta que los colores de énfasis en Xbox One son por usuario y no por sistema.

También ten en cuenta que el conjunto de colores del usuario en Xbox One no es el mismo que en los equipos, teléfonos y otros dispositivos.

Siempre que tu aplicación use un recurso de pincel como **SystemControlForegroundAccentBrush** o un recurso de color (**SystemAccentColor**), o en cambio, llame a los colores de énfasis directamente a través de la API [UIColorType.Accent*](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType), estos colores se reemplazarán por los colores de énfasis disponibles en Xbox One. Los colores del pincel de contraste alto también se extraen del sistema del mismo modo que en un equipo o teléfono.

Para obtener más información sobre colores de énfasis en general, consulta [Color de énfasis](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Variación de color entre televisores

Al diseñar para televisores, observe que los colores se muestran de forma muy diferente según el televisor en el que se muestren. No asumas que los colores aparecerán exactamente igual que en el monitor. Si tu aplicación depende de diferencias sutiles en el color para diferenciar las partes de la interfaz de usuario, los colores se podrían mezclar y confundir a los usuarios. Trata de usar colores que sean lo suficientemente distintos para que los usuarios puedan diferenciarlos claramente, independientemente del televisor que utilicen.

### <a name="tv-safe-colors"></a>Colores seguros para el televisor

Los valores RGB de un color representan las intensidades de rojo, verde y azul. Las televisiones no controlan muy bien las intensidades extremas&mdash;pueden producir un extraño efecto de bandas o aparecer más pálidos en determinadas televisiones. Además, los colores de gran intensidad pueden causar un efecto de desbordamiento o "blooming" (los píxeles cercanos comienzan a dibujar los mismos colores). Si bien existen diferentes escuelas de pensamiento sobre lo que se considera colores seguros para el televisor, los colores dentro de los valores RGB de 16-235 (o 10-EB en hexadecimal) son generalmente seguros el televisor.

![Intervalo de colores seguros para el televisor](images/designing-for-tv/tv-safe-colors-2.png)

Históricamente, las aplicaciones en Xbox tenían que adaptar sus colores para entrar dentro de este intervalo de colores seguro para el televisor; sin embargo, a partir de la actualización de Fall Creators Update, Xbox One escala automáticamente el contenido de todo el intervalo en el intervalo seguro para el televisor. Esto significa que la mayoría de los desarrolladores de aplicaciones ya no tienen que preocuparse por los colores seguros para el televisor.

> [!IMPORTANT]
> Todo contenido de vídeo que ya esté en el intervalo de colores seguros para el televisor no tiene este efecto de escalas de colores aplicado cuando se reproduce mediante [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk).

Si vas a desarrollar una aplicación con DirectX 11 o DirectX 12 y crear tu propia cadena de intercambio para representar la interfaz de usuario o vídeo, puedes especificar el espacio de colores que usas llamando a [IDXGISwapChain3::SetColorSpace1](https://docs.microsoft.com/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1), el cual permitirá que el sistema sepa si se debe escalar colores o no.

## <a name="guidelines-for-ui-controls"></a>Directrices sobre controles de la interfaz de usuario

Hay varios controles de interfaz de usuario que funcionan bien en varios dispositivos, pero tienen ciertas consideraciones cuando se usan en televisores. Obtén información sobre algunos procedimientos recomendados para usar estos controles al diseñar la experiencia de 10 pies.

### <a name="pivot-control"></a>Control dinámico

Una clase [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permite la navegación rápida de vistas dentro de una aplicación a través de la selección de encabezados o pestañas diferentes. El control subraya el encabezado que tiene el foco, para destacar el encabezado actualmente seleccionado al usar el controlador para juegos o el control remoto.

![Subrayado dinámico](images/designing-for-tv/pivot-underline.png)

Puedes establecer la propiedad [Pivot.IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) en `true` para que los controles dinámicos mantengan siempre la misma posición, en lugar de que el encabezado dinámico seleccionado se mueva siempre a la primera posición. Esto brinda una mejor experiencia en pantallas de gran tamaño tales como televisores, ya que el encapsulamiento de los encabezados puede resultar confuso para los usuarios. Si todos los encabezados de tabla dinámica no caben en la pantalla al mismo tiempo, habrá una barra de desplazamiento para permitir a los clientes ver los demás encabezados; Sin embargo, debes asegurarte de que todos caben en la pantalla para proporcionar la mejor experiencia. Para obtener más información, consulta [Pestañas y controles dinámicos](/windows/uwp/design/controls-and-patterns/pivot).

### <a name="navigation-pane-a-namenavigation-pane-"></a>Panel de navegación <a name="navigation-pane" />

Un panel de navegación (también conocido como *menú hamburguesa*) es un control de navegación que suele usarse en las aplicaciones para UWP. Normalmente, es un panel con varias opciones para elegir en un menú de estilo lista que llevará al usuario a páginas diferentes. Por lo general, este panel se abre contraído para ahorrar espacio y el usuario puede hacer clic en un botón para abrirlo.

Mientras que los paneles de navegación son muy accesibles con el mouse y la función táctil, con el controlador para juegos y el control remoto son menos accesibles, ya que el usuario tiene que navegar hasta un botón para abrir el panel. Por lo tanto, se recomienda abrir el panel de navegación con el botón **Vista**, así como permitir que el usuario se desplace hacia la izquierda de la página para abrirlo. Para obtener un ejemplo de código sobre cómo implementar este patrón de diseño, consulta el documento [Navegación con foco mediante programación](../input/focus-navigation-programmatic.md#split-view-code-sample). De este modo, el usuario puede acceder muy fácilmente al contenido del panel. Para obtener más información sobre el comportamiento de los paneles de navegación en pantallas de diferentes tamaños, así como sobre los procedimientos recomendados para la navegación con el controlador para juegos o el control remoto, consulta [Paneles de navegación](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>Etiquetas de CommandBar

Es una buena idea tener las etiquetas a la derecha de los iconos en una [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) para que su altura se minimice y mantenga la coherencia. Para ello, establece la propiedad [CommandBar.DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) en `CommandBarDefaultLabelPosition.Right`.

![CommandBar con etiquetas a la derecha de los iconos](images/designing-for-tv/commandbar.png)

Al establecer esta propiedad, las etiquetas se mostrarán de manera permanente, lo que resulta ideal para la experiencia de 10 pies ya que minimiza el número de clics para el usuario. Este también es un modelo excelente que otros tipos de dispositivos pueden seguir.

### <a name="tooltip"></a>Información sobre herramientas

El control [Tooltip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) se introdujo como una manera de proporcionar más información en la interfaz de usuario cuando el usuario mantiene el mouse sobre, o pulsa y mantiene su dedo en, un elemento. Para los controladores para juegos y controles remotos, el `Tooltip` aparece brevemente cuando el elemento obtiene el foco, permanece en la pantalla durante un instante y luego desaparece. Este comportamiento puede causar confusión si se usan demasiados `Tooltip`. Intenta evitar el uso de `Tooltip` cuando diseñes para televisores.

### <a name="button-styles"></a>Estilos de botón

Si bien los botones estándar de la UWP funcionan bien en televisores, algunos estilos visuales de botones llaman mejor la atención sobre la interfaz de usuario, los cuales es conveniente considerar para todas las plataformas, especialmente para la experiencia de 10 pies, la cual se beneficia de comunicar claramente dónde se encuentra el foco. Para obtener más información sobre estos estilos, consulta [Botones](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Elementos de la interfaz de usuario anidada

La interfaz de usuario anidada expone los elementos anidados que pueden requerir acción dentro de un elemento de interfaz de usuario del contenedor en el que tanto el elemento anidado como el elemento contenedor pueden tener un foco independiente entre sí.

La interfaz de usuario anidada funciona bien para algunos tipos de entrada, pero no siempre para el controlador para juegos y el control remoto, que dependen de la navegación XY. Asegúrate de seguir las directrices de este tema para garantizar que el usuario pueda acceder fácilmente a todos los elementos interactivos y que la interfaz de usuario esté optimizada para el entorno de 304,8cm. Una solución habitual es colocar elementos de interfaz de usuario anidados en un `ContextFlyout`.

Para obtener más información sobre la interfaz de usuario anidada, consulta [Interfaz de usuario anidada en elementos de lista](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

El elemento [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) permite a los usuarios interactuar con el contenido multimedia gracias a una experiencia de reproducción predeterminada en la que pueden reproducir y pausar dicho contenido, activar los subtítulos y mucho más. Este control es propiedad de [MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) y ofrece dos opciones de diseño: *fila única* y *file doble*. En el diseño de fila única, los botones de control deslizante y reproducción están dispuestos en la misma fila. Los botones de reproducción/pausa está situados a la izquierda del control deslizante. En el diseño de fila doble, el control deslizante está situado en la primera fila y los botones de reproducción se encuentran en una segunda fila inferior. Al diseñar la experiencia de 3 metros, se deberá usar el diseño de fila doble, ya que permite una mejor navegación del controlador para juegos. Para habilitar el diseño de fila doble, configúrala como `IsCompact="False"` en el elemento `MediaTransportControls` de la propiedad [TransportControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) , propiedad de `MediaPlayerElement`.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

Visita [Reproducción de multimedia](../controls-and-patterns/media-playback.md) para obtener más información sobre cómo agregar contenido multimedia a tu aplicación.

> ![NOTA] `MediaPlayerElement` solo está disponible en Windows 10, versión 1607 y posteriores. Si vas a desarrollar una aplicación para una versión anterior de Windows 10, deberás usar [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) en su lugar. Las recomendaciones anteriores se aplican a `MediaElement` y podrás obtener acceso a la propiedad `TransportControls` de la misma manera.

### <a name="search-experience"></a>Experiencia de búsqueda

Buscar contenido es una de las funciones más habituales en la experiencia de 3 metros. Si tu aplicación proporciona una experiencia de búsqueda, es conveniente que el usuario pueda obtener acceso a ella rápidamente mediante el botón **Y** del controlador para juegos, a modo de acelerador.

La mayoría de los clientes ya estarán familiarizados con este acelerador, pero si lo deseas, puedes agregar un glifo visual **Y** en la interfaz de usuario para indicar que el cliente puede obtener acceso a la funcionalidad de búsqueda mediante ese botón. Si agregas esta indicación, asegúrate de usar el símbolo de la fuente **Segoe Xbox Symbol MDL2 Symbol** (`&#xE3CC;` para aplicaciones XAML apps y `\E426` para aplicaciones HTML) para que sea coherente con el Shell de Xbox y otras aplicaciones.

> [!NOTE]
> Dado que la fuente **Segoe Xbox MDL2 Symbol** solamente está disponible en Xbox, el símbolo no aparecerá correctamente en tu PC. Sin embargo, se mostrará en el televisor una vez que implementes en Xbox.

Dado que el botón **Y** solo está disponible en el controlador para juegos, asegúrate de proporcionar otros métodos de acceso a la búsqueda, como algunos botones en la interfaz de usuario. De lo contrario, es posible que algunos clientes no puedan obtener acceso a la funcionalidad.

En la experiencia de 3 metros, suele ser más fácil para los clientes usar una experiencia de búsqueda de pantalla completa porque el espacio en pantalla es limitado. Si tienes una funcionalidad de búsqueda "local" a pantalla completa o parcial, te recomendamos que, cuando el usuario abra la experiencia de búsqueda, el teclado en pantalla aparezca ya abierto, listo para que el cliente escriba los términos de búsqueda.

## <a name="custom-visual-state-trigger-for-xbox"></a>Desencadenador de estado visual personalizado para Xbox

Para adaptar tu aplicación para UWP a la experiencia de 3 metros, te recomendamos realizar cambios en el diseño cuando la aplicación detecte que se ha iniciado en una consola Xbox. Una manera de hacerlo es mediante un *desencadenador de estado visual* personalizado. Los desencadenadores de estado visual son especialmente útiles para la edición en **Blend para Visual Studio**. El siguiente fragmento de código muestra cómo crear un desencadenador de estado visual para Xbox:

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Para crear el desencadenador, agrega la siguiente clase a tu aplicación. Esta es la clase a la que hace referencia el código XAML anterior:

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

Después de agregar el desencadenador personalizado, tu aplicación hará automáticamente los cambios de diseño que especificaste en el código XAML siempre que detecte que se está ejecutando en una consola Xbox One.

Otra forma de comprobar si la aplicación se ejecuta en Xbox y después realizar los ajustes apropiados es a través de código. Puedes usar la siguiente variable simple para comprobar si la aplicación se ejecuta en Xbox:

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

Luego, puedes realizar los ajustes adecuados para la interfaz de usuario en el bloque de código siguiendo esta comprobación. 

## <a name="summary"></a>Resumen

El diseño para la experiencia de 10 pies presenta consideraciones especiales que hay que tener en cuenta y que lo hacen diferente del diseño para cualquier otra plataforma. Aunque por supuesto que puedes hacer una migración directa de la aplicación para UWP a Xbox One y que también funcione, no necesariamente estará optimizada para la experiencia de 10 pies y puede llevar a la frustración del usuario. Si sigues las instrucciones de este artículo te asegurarás de que la aplicación sea lo mejor posible en televisión.

## <a name="related-articles"></a>Artículos relacionados

- [Dispositivo básico para aplicaciones de la Plataforma universal de Windows (UWP)](index.md)
- [Interacciones con controlador para juegos y control remoto](../input/gamepad-and-remote-interactions.md)
- [Sonido en las aplicaciones para UWP](../style/sound.md)

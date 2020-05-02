---
description: Reveal es un efecto de iluminación que te ayuda a dar profundidad y foco a los elementos interactivos de tu aplicación.
title: Mostrar resaltado
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 63a7ee8550b72356199645f54b587480275c2bcd
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685083"
---
# <a name="reveal-highlight"></a>Mostrar resaltado

![imagen principal](images/header-reveal-highlight.svg)

Mostrar resaltado es un efecto de iluminación que resalta los elementos interactivos, como las barras de comandos, cuando el usuario mueve el puntero cerca de ellos. 

> **API importantes**: [clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [clase RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [clase RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [clase RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [clase VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Cómo funciona
Mostrar resaltado llama la atención respecto a elementos interactivos al mostrar el contenedor del elemento cuando el puntero está cerca, como se muestra en la siguiente ilustración:

![Elemento visual Reveal](images/Nav_Reveal_Animation.gif)

Al exponer los bordes ocultos alrededor de los objetos, Reveal permite a los usuarios comprender mejor el espacio con el que interactúan y los ayuda a comprender las acciones disponibles. Esto es especialmente importante en los controles de lista y agrupaciones de botones.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Reveal">abrir la aplicación y ver Reveal en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Cómo se usa

Reveal funciona automáticamente para algunos controles. Para otros controles, puedes habilitar la función Reveal si asignas un estilo especial al control, como se describe en las secciones [Habilitar Reveal en otros controles comunes](#enabling-reveal-on-other-controls) y [Habilitar Reveal en controles personalizados](#enabling-reveal-on-custom-controls) de este artículo.

## <a name="controls-that-automatically-use-reveal"></a>Controles que usan Reveal automáticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Estas ilustraciones muestran el efecto de Mostrar resaltado en varios controles diferentes:

![Ejemplos de Reveal](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Habilitar Reveal en otros controles

Si tienes un escenario donde debe aplicarse Reveal (estos controles son el contenido principal o se usan en una orientación de lista o colección), dispones de estilos de recursos de inclusión que te permiten habilitar Reveal para esos tipos de situaciones.

Estos controles no tienen Reveal de manera predeterminada, ya que son controles más pequeños que, por lo general, son controles auxiliares a los puntos focales principales de la aplicación; pero cada aplicación es diferente y, si estos controles se usan en la mayor parte de la aplicación, tienes algunos estilos que te ayudarán con esto:

| Nombre del control   | Nombre del recurso |
|----------|:-------------:|
| Botón |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (Reveal encima del contenido) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Para aplicar estos estilos, solo tienes que establecer la propiedad [Style](/uwp/api/Windows.UI.Xaml.Style) del control:

```xaml
<Button Content="Button Content" Style="{ThemeResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>Reveal en temas

Reveal cambia ligeramente en función del tema solicitado del control, aplicación o configuración de usuario. La luz de Reveal del borde y de la acción Mantener el puntero en el tema oscuro es blanca, pero en el tema claro solo se oscurecen los bordes a un gris claro.

![Reveal en tema oscuro y claro](images/Dark_vs_LightReveal.png)

Para habilitar bordes blancos mientras te encuentres en el tema claro, solo tienes que establecer el tema solicitado en el control en Oscuro.

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

O bien, cambiar el TargetTheme en RevealBorderBrush a Oscuro. Recuerda: Si se establece TargetTheme en Oscuro, Reveal será blanco, pero si se establece en Claro, los bordes de Reveal serán grises.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Habilitación de Reveal en controles personalizados

Puedes agregar Reveal a controles personalizados. Antes de hacerlo, es útil saber un poco más sobre cómo funciona el efecto de Reveal. Reveal se compone de dos efectos independientes: **Reveal border** y **Reveal hover**.

- **Border** muestra los bordes de los elementos interactivos cuando hay un puntero cerca. Este efecto muestra que esos objetos cercanos pueden realizar acciones similares al que está enfocado actualmente.
- **Mantener el puntero** aplica una forma de halo suave alrededor del elemento enfocado o sobre el que se mantiene el puntero y reproduce una animación de presión al hacer clic. 

![Capas de Reveal](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Estos efectos se definen mediante dos elementos Brush: 
* Border Reveal se define mediante **RevealBorderBrush**.
* Hover Reveal se define mediante **RevealBackgroundBrush**.

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
En la mayoría de los casos, controlamos el uso de ambos mediante la activación automática de Reveal para determinados controles. Aun así, deberán habilitarse otros controles a través de la aplicación de un estilo o directamente cambiando sus plantillas.

### <a name="when-to-add-reveal"></a>Cuándo agregar Reveal
Puedes agregar Reveal a tus controles personalizados, pero primero ten en cuenta el tipo de control y cómo se comporta. 
* Si tu control personalizado es un solo elemento interactivo y no tiene controles similares que compartan su espacio (por ejemplo, elementos de menú en un menú), es probable que tu control personalizado no necesite Reveal.  
* Si tienes una agrupación de elementos o contenido interactivo relacionado, es probable que esa región de la aplicación necesite Reveal (esto se conoce normalmente como una superficie [dominante](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding)).

Por ejemplo, un botón por sí mismo no debería usar Reveal, pero un conjunto de botones de una barra de comandos debe usar Reveal.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>Uso de la plantilla de control para agregar Reveal 
Para habilitar Reveal en controles personalizados o controles nuevamente basados en modelos, modifica la plantilla del control. La mayoría de las plantillas de control tienen una cuadrícula en la raíz; actualiza la clase [VisualState](/uwp/api/windows.ui.xaml.visualstate) de esa cuadrícula raíz para usar Reveal:

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

Es importante tener en cuenta que Reveal necesita que el elemento Brush y los establecedores estén en su estado visual para que funcionen correctamente. Si simplemente estableces el elemento Brush de un control en uno de nuestros recursos Reveal brush, no habilitarás Reveal para dicho control. Por el contrario, si solo tienes la configuración o los destinos sin los valores establecidos en Reveal brushes, tampoco habilitarás Reveal.

Para obtener más información sobre cómo modificar las plantillas de control, consulta el artículo sobre las [plantillas de control XAML](../controls-and-patterns/control-templates.md).

Hemos creado un conjunto de Reveal brushes del sistema que puedes usar para personalizar tu plantilla. Por ejemplo, puedes usar el elemento Brush **ButtonRevealBackground** para crear un fondo de botón personalizado, o bien el elemento Brush **ListViewItemRevealBackground** para listas personalizadas, etc. (Para obtener información sobre cómo funcionan los recursos en XAML, echa un vistazo al artículo sobre el [diccionario de recursos de XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)).

### <a name="full-template-example"></a>Ejemplo de plantilla completa

Esta es una plantilla completa del aspecto de un botón Reveal:

```xaml
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>Ajuste preciso del efecto Reveal en un control personalizado 

Al habilitar Reveal en un control personalizado o nuevamente basado en modelo o una superficie de comandos personalizada, estas sugerencias pueden ayudarte a optimizar el efecto:
 
* En los elementos adyacentes con tamaños que no se alinean en alto o ancho (especialmente en las listas): quita el comportamiento del enfoque de borde y mantén los bordes mostrados solo al mantener el puntero.
* Para elementos dominantes que a menudo entran y salen del estado deshabilitado: coloca el elemento Brush del enfoque de borde en las placas traseras de los elementos, así como en sus bordes, para hacer hincapié en su estado.
* Para los elementos dominantes adyacentes que están tan cerca que se tocan: agrega un margen de 1 píxel entre los dos elementos. 

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar
### <a name="do"></a>Cosas que hacer
- Usa Reveal en elementos donde el usuario puede realizar muchas acciones (barras de comandos, menús de navegación).
- Usa Reveal en agrupaciones de elementos interactivos que no tienen separadores visuales de manera predeterminada (listas, cintas de opciones).
- Usa Reveal en áreas con una alta densidad de elementos interactivos (escenarios dominantes).
- Pon espacios de márgenes de 1 píxel entre los elementos Reveal.

### <a name="dont"></a>Cosas que evitar
- No uses Reveal en contenido estático (fondos, texto).
- No uses Reveal en elementos emergentes, controles flotantes o listas desplegables.
- No uses Reveal en situaciones aisladas y de uso único.
- No uses Reveal en elementos muy grandes (superiores a 500 epx).
- No uses Reveal en las decisiones de seguridad, ya que puede distraer la atención del mensaje que se quiere dar al usuario.


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplos de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): consulta todos los controles XAML en un formato interactivo.

## <a name="reveal-and-the-fluent-design-system"></a>Reveal y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Reveal es un componente de Fluent Design System que agrega luz a tu aplicación. Para más información, consulta la [Introducción a Fluent Design para UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Artículos relacionados

- [Clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrílico](acrylic.md)
- [Efectos de composición](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design para UWP](/windows/apps/fluent-design-system)
- [Ciencia en el sistema: Fluent Design y profundidad](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciencia en el sistema: Fluent Design y luz](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)

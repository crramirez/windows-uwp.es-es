---
author: mijacobs
description: Reveal es un efecto de iluminación que te ayuda a dar profundidad y foco a los elementos interactivos de tu aplicación.
title: Mostrar resaltado
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 67bd984f4216be9eded51b6175829828e9c332f1
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4309213"
---
# <a name="reveal-highlight"></a>Mostrar resaltado

![imagen principal](images/header-reveal-highlight.svg)

Mostrar que resaltado es un efecto de iluminación que resalta los elementos interactivos, como las barras de comandos, cuando el usuario mueve el puntero cerca de ellos. 

> **API importantes**: [clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [clase RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [clase RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [clase RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [clase VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Así es cómo funciona
Mostrar resaltado llama la atención a los elementos interactivos mostrando el contenedor del elemento cuando el puntero está cercano, como se muestra en la siguiente ilustración:

![Elemento visual de Reveal](images/Nav_Reveal_Animation.gif)

Al exponer los bordes ocultos alrededor de los objetos, Reveal permite a los usuarios comprender mejor el espacio con el que interactúan y los ayuda a comprender las acciones disponibles. Esto es especialmente importante en los controles de lista y agrupaciones de botones.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Reveal">abrir la aplicación y ver Reveal en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Cómo usarla

Mostrar funciona automáticamente para algunos controles. Para otros controles, puedes habilitar Reveal asignando un estilo especial al control, como se describe en las secciones de este artículo [Habilitar Reveal en otros controles](#enabling-reveal-on-other-controls) y [Habilitar Reveal en controles personalizados](#enabling-reveal-on-custom-controls) .

## <a name="controls-that-automatically-use-reveal"></a>Controles que usan Reveal automáticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Estas ilustraciones muestran Mostrar resaltado en varios controles diferentes:

![Ejemplos de Reveal](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Habilitar Reveal en otros controles

Si tienes un escenario donde debe aplicarse Reveal (estos controles son el contenido principal o se usan en una orientación de lista o colección), cuentas con estilos de recursos de inclusión que te permiten habilitar Reveal para esos tipos de situaciones.

Estos controles no tienen Reveal de manera predeterminada, ya que son controles más pequeños que, por lo general, son controles auxiliares a los puntos focales principales de la aplicación; pero cada aplicación es diferente y, si estos controles se usan en la mayor parte de la aplicación, tienes algunos estilos que te ayudarán con esto:

| Nombre del control   | Nombre del recurso |
|----------|:-------------:|
| Botón |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (Reveal encima del contenido) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Para aplicar estos estilos, solo tienes que establecer la propiedad [Estilo](/uwp/api/Windows.UI.Xaml.Style) del control:

```xaml
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>Reveal en temas

Reveal cambia ligeramente en función del tema solicitado del control, aplicación o configuración de usuario. La luz de borde y mantener del puntero de Mostrar de tema oscuro es blanca, pero en el tema claro solo se oscurecen los bordes a un gris claro.

![Mostrar oscuro y claro](images/Dark_vs_LightReveal.png)

Para habilitar bordes blancos mientras te encuentres en el tema claro, solo tienes que establecer el tema solicitado en el control en Oscuro.

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

O bien, cambiar el TargetTheme en RevealBorderBrush a Oscuro. ¡Recuerda! Si se establece TargetTheme en Oscuro, Mostrar será blanco, pero si se establece en Claro, los bordes de Mostrar serán grises.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Habilitar Reveal en controles personalizados

Puedes agregar Reveal a controles personalizados. Antes de hacerlo, es útil saber un poco más acerca de cómo funciona el efecto de Reveal. Reveal se compone de dos efectos independientes: **Reveal border** y **Reveal hover**.

- **Border** muestra los bordes de los elementos interactivos cuando hay un puntero cerca. Este efecto te muestra que esos objetos cercanos pueden realizar acciones similares al que está enfocado actualmente.
- **Mantener el puntero** aplica una forma de halo suave alrededor del elemento enfocado o sobre el que se mantiene el puntero y reproduce una animación de presión al hacer clic. 

![Capas de Reveal](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Estos efectos se definen mediante dos pinceles: 
* Border Reveal se define mediante **RevealBorderBrush**
* Hover Reveal se define mediante **RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
En la mayoría de los casos, controlamos el uso de ambos activando Reveal automáticamente para determinados controles. Sin embargo, otros controles deberán habilitarse a través de la aplicación de un estilo o cambiado sus plantillas directamente.

### <a name="when-to-add-reveal"></a>Cuándo agregar Reveal
Puedes agregar Reveal a tus controles personalizados, pero antes, ten en cuenta el tipo de control y cómo se comporta. 
* Si tu control personalizado es un solo elemento interactivo y no tiene controles similares compartiendo su espacio (por ejemplo, elementos de menú en un menú), es probable que tu control personalizado no necesite Reveal.  
* Si tienes una agrupación de elementos o contenido interactivo relacionado, es probable que esa región de tu aplicación necesite Reveal (esto se conoce normalmente como una superficie de [Comandos](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding).

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

Es importante tener en cuenta que Reveal necesita que el brush y los establecedores estén en su estado visual para que funcionen correctamente. Establecer simplemente el brush de un control en uno de nuestros recursos Reveal brush no habilitará Reveal para dicho control. Por el contrario, con solo la configuración o destinos sin los valores establecidos en Reveal brushes tampoco habilitará Reveal.

Para obtener más información sobre cómo modificar las plantillas de control, consulta el artículo de las [plantillas de control XAML](../controls-and-patterns/control-templates.md).

Hemos creado un conjunto de Reveal brushes del sistema que puedes usar para personalizar tu plantilla. Por ejemplo, puedes usar el brush **ButtonRevealBackground** para crear un fondo de botón personalizado o el brush **ListViewItemRevealBackground** para listas personalizadas, etc. (Para obtener información sobre cómo funcionan los recursos en XAML, echa un vistazo al artículo [Diccionario de recursos de Xaml](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)).

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

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>Ajustar el efecto Reveal en un control personalizado 

Al habilitar Reveal en un control personalizado o nuevamente modelo o una superficie de comandos personalizada, estas sugerencias pueden ayudarte a optimizar el efecto:
 
* En los elementos adyacentes con tamaños que no se alinean en alto o ancho (especialmente en las listas): quita el comportamiento del enfoque de borde y mantén los bordes mostrados solo al mantener el puntero.
* Para elementos dominantes que a menudo entran y salen del estado deshabilitado: coloca el pincel del enfoque de borde en placas traseras de los elementos, así como sus bordes para hacer hincapié en su estado.
* Para los elementos dominantes adyacentes que están tan cerca que se tocan: agrega un margen de 1 píxel entre los dos elementos. 

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer
### <a name="do"></a>Hacer:
- Usar Reveal en elementos donde el usuario puede realizar muchas acciones (barras de comandos, menús de navegación)
- Usar Reveal en agrupaciones de elementos interactivos que no tienen separadores visuales de manera predeterminada (listas, cintas de opciones)
- Usar Reveal en áreas con una alta densidad de elementos interactivos (escenarios dominantes)
- Poner espacios de márgenes de 1 píxel entre los elementos Reveal

### <a name="dont"></a>Cosas que evitar
- No usar Reveal en contenido estático (fondos, texto)
- No usar Reveal en elementos emergentes, controles flotantes o listas desplegables
- No usar Reveal en situaciones aisladas y de uso único
- No usar Reveal en elementos muy grandes (superiores a 500epx)
- No usar Reveal en las decisiones de seguridad, ya que puede distraer la atención del mensaje que se quiere dar al usuario


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="reveal-and-the-fluent-design-system"></a>Reveal y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Reveal es un componente de Fluent Design System que agrega luz a tu aplicación. Para obtener más información, consulta la [Introducción a Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artículos relacionados

- [Clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylic](acrylic.md)
- [Efectos de composición](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design para UWP](../fluent-design-system/index.md)
- [Science in the System: Fluent Design and Depth (Ciencia en el sistema: Fluent Design y profundidad)](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Science in the System: Fluent Design and Depth (Ciencia en el sistema: Fluent Design y luz)](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)

---
author: mijacobs
description: "Reveal es un efecto de iluminación que te ayuda a dar profundidad y foco a los elementos interactivos de tu aplicación."
title: "Características principales de Reveal"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8ba0d9939d7ab1d9826ed2848e476499f09c628f
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2018
---
# <a name="reveal-highlight"></a>Características principales de Reveal

Reveal es un efecto de iluminación que te ayuda a dar profundidad y foco a los elementos interactivos de tu aplicación.

> **API importantes**: [clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [clase RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [clase RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [clase RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [clase VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

Para lograr esto, el comportamiento de Reveal muestra el contenedor del contenido seleccionable cuando el puntero está cerca.

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

## <a name="reveal-and-the-fluent-design-system"></a>Reveal y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Reveal es un componente de Fluent Design System que agrega luz a tu aplicación. Para obtener más información, consulta la [Introducción a Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="how-to-use-it"></a>Cómo se usa

Reveal funciona automáticamente para algunos controles. Para otros controles, puedes habilitar Reveal mediante la asignación de un estilo especial para el control.

## <a name="controls-that-automatically-use-reveal"></a>Controles que usan Reveal automáticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)
- [**ComboBox**](../controls-and-patterns/lists.md)

Estas ilustraciones muestran el efecto de Reveal en varios controles diferentes:

![Ejemplos de Reveal](images/RevealExamples_Collage.png)

## <a name="enabling-reveal-on-other-common-controls"></a>Habilitar Reveal en otros controles comunes

Si tienes un escenario donde debe aplicarse Reveal (estos controles son el contenido principal o se usan en una orientación de lista o colección), cuentas con estilos de recursos de inclusión que te permiten habilitar Reveal para esos tipos de situaciones.

Estos controles no tienen Reveal de manera predeterminada, ya que son controles más pequeños que, por lo general, son controles auxiliares a los puntos focales principales de la aplicación; pero cada aplicación es diferente y, si estos controles se usan en la mayor parte de la aplicación, tienes algunos estilos que te ayudarán con esto:

| Nombre del control   | Nombre del recurso |
|----------|:-------------:|
| Botón |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |

Para aplicar estos estilos, simplemente actualiza la propiedad Style de este modo:

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

## <a name="enabling-reveal-on-custom-controls"></a>Habilitar Reveal en controles personalizados

La regla general en la que hay que pensar al decidir si tu control personalizado debe obtener Reveal o no es que debes tener una agrupación de elementos interactivos que guardan relación con una característica o acción exhaustiva que quieres realizar en tu aplicación.

Por ejemplo, los elementos de NavigationView están relacionados con la navegación de página. Los botones de CommandBar relacionados con acciones de características de página o acciones de menú. Los botones de MediaTransportControl debajo guardan relación con los medios que se están reproduciendo.

Los controles que obtienen Reveal no tienen que estar relacionados, solo tienen que estar en un área de alta densidad y tener un objetivo mayor.

Para habilitar Reveal en controles personalizados o controles vueltos a basar, puedes entrar en el estilo de ese control en los estados visuales de la plantilla de ese control y especificar Reveal en la cuadrícula raíz:

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

Es importante tener en cuenta que Reveal necesita que el brush y los establecedores estén en su estado visual para que funcionen completamente. Establecer simplemente el brush de un control en uno de nuestros recursos Reveal brush no habilitará Reveal para dicho control. Por el contrario, con solo la configuración o destinos sin los valores establecidos en Reveal brushes tampoco habilitará Reveal.

Hemos creado un conjunto de Reveal brushes del sistema que puedes usar para personalizar la interfaz de usuario. Por ejemplo, puedes usar el brush **ButtonRevealBackground** para crear un fondo de botón personalizado o el brush **ListViewItemRevealBackground** para listas personalizadas, etc.

(Para obtener información sobre cómo funcionan los recursos en XAMl, echa un vistazo al artículo [Diccionario de recursos de Xaml](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)).

### <a name="reveal-on-listview-controls-with-nested-buttons"></a>Reveal en controles ListView con botones anidados

Si tienes una [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) y también tienes botones o contenido que puede invocarse anido en sus elementos [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem), debes habilitar Reveal para los elementos anidados.

En el caso de los botones, o controles parecidos a botones de un ListViewItem, tan solo tienes que establecer la propiedad [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Style) del control en el recurso estático **ButtonRevealStyle**. 

![Reveal anidado](images/NestedListContent.png)

Este ejemplo habilita Reveal en varios botones de un ListViewItem. 

```XAML
<ListViewItem>
    <StackPanel Orientation="Horizontal">
        <TextBlock Margin="5">Test Text: lorem ipsum.</TextBlock>
        <StackPanel Orientation="Horizontal">
            <Button Content="&#xE71B;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE728;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE74D;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
         </StackPanel>
    </StackPanel>
</ListViewItem>
```

### <a name="listviewitempresenter-with-reveal"></a>ListViewItemPresenter con Reveal

Las listas son un poco especiales en XAML y en el caso de Reveal tendremos que definir Visual State Manager solo para Reveal en ListViewItemPresenter:

```XAML
<ListViewItemPresenter>
<!-- ContentTransitions, SelectedForeground, etc. properties -->
RevealBackground="{ThemeResource ListViewItemRevealBackground}"
RevealBorderThickness="{ThemeResource ListViewItemRevealBorderThemeThickness}"
RevealBorderBrush="{ThemeResource ListViewItemRevealBorderBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal" />
        <VisualState x:Name="Selected" />
        <VisualState x:Name="PointerOver">
            <VisualState.Setters>
                <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
            </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverPressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PressedSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            </VisualStateGroup>
                <VisualStateGroup x:Name="EnabledGroup">
                    <VisualState x:Name="Enabled" />
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                             <Setter Target="Root.RevealBorderThickness" Value="0"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ListViewItemPresenter>
```

Esto significa anexar al final de la colección de propiedades en ListViewItemPresenter con los establecedores de Estado visual específicos de Reveal.

### <a name="full-template-example"></a>Ejemplo de plantilla completa

Esta es una plantilla completa del aspecto de un botón Reveal:

```XAML
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

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer
- Usar Reveal en elementos donde el usuario puede realizar acciones (botones, selecciones)
- Usar Reveal en agrupaciones de elementos interactivos que no tienen separadores visuales de manera predeterminada (listas, barras de comandos)
- Usar Reveal en áreas con una alta densidad de elementos interactivos
- No usar Reveal en contenido estático (fondos, texto)
- No usar Reveal en situaciones aisladas y de uso único
- No usar Reveal en elementos muy grandes (superiores a 500epx)
- No usar Reveal en las decisiones de seguridad, ya que puede distraer la atención del mensaje que se quiere dar al usuario

## <a name="how-we-designed-reveal"></a>Cómo se diseñó Reveal

Reveal cuenta con dos componentes visuales principales: el comportamiento **Hover Reveal** y el comportamiento **Border Reveal**.

![Capas de Reveal](images/RevealLayers.png)

Hover Reveal se vincula directamente con el contenido por el que se desplaza el puntero (a través de puntero o la entrada del foco), y aplica una forma de halo suave alrededor del elemento activado o enfocado, lo que te permite saber que puedes interactuar con él.

Border Reveal se aplica al elemento enfocado y a los elementos cercanos. Esto te muestra que esos objetos cercanos pueden realizar acciones similares al que está enfocado actualmente.

El desglose de la receta de Reveal es:

- Border Reveal estará encima de todo el contenido, salvo en los bordes designados.
- El texto y el contenido se mostrarán directamente debajo de Border Reveal.
- Hover Reveal estará debajo del contenido y el texto.
- La placa trasera (que se activa y permite Hover Reveal).
- El fondo (el fondo de control)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->  

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylic](acrylic.md)
- [Efectos de composición](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design para UWP](../fluent-design-system/index.md)
- [Science in the System: Fluent Design and Depth (Ciencia en el sistema: Fluent Design y profundidad)](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Science in the System: Fluent Design and Depth (Ciencia en el sistema: Fluent Design y luz)](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)

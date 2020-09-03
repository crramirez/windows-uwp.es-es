---
Description: TwoPaneView es un control de diseño que te ayuda a administrar la presentación de aplicaciones que tienen dos áreas de contenido distintas.
title: Vista de dos paneles
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9c2fd792b9652e38637810b4ccd0aee94075895b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174699"
---
# <a name="two-pane-view"></a>Vista de dos paneles

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) es un control de diseño que te ayuda a administrar la presentación de aplicaciones que tienen dos áreas de contenido distintas, como una vista principal/detalles.

> [!IMPORTANT]
> En este artículo se describen la funcionalidad y las instrucciones que se encuentran en versión preliminar pública, por lo que es posible que puedan modificarse de forma sustancial antes de que estén disponibles con carácter general. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Aunque el control TwoPaneView funciona en todos los dispositivos Windows, está diseñado para ayudarte a sacar el máximo partido de los dispositivos de doble pantalla de manera automática, sin necesidad de ninguna codificación especial. En un dispositivo de doble pantalla, la vista de dos paneles garantiza que la interfaz de usuario (UI) se divida sin problemas cuando abarque la brecha entre las pantallas, de modo que el contenido se presente a alguno de los lados de la brecha.

> [!NOTE]
> Un _dispositivo de doble pantalla_ es un tipo especial de dispositivo con funcionalidades exclusivas. No equivale a un dispositivo de escritorio con varios monitores. Para obtener más información sobre los dispositivos de doble pantalla, consulta [Introducción a los dispositivos de doble pantalla](/dual-screen/introduction). (Consulta [Mostrar varias vistas](../layout/show-multiple-views.md) para más información sobre las formas en que puedes optimizar la aplicación para varios monitores).

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control **TwoPaneView** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de la biblioteca de interfaz de usuario de Windows:** [Clase TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview)

> [!TIP]
> En este documento, se usa el alias **muxc** en XAML para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado lo siguiente a nuestro elemento [Page](/uwp/api/windows.ui.xaml.controls.page): `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>En el código subyacente, se usa el alias **muxc** en C# para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado esta instrucción **using** en la parte superior del archivo: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa la vista de dos paneles cuando tenga dos áreas de contenido distintas y:

- El contenido deba reorganizarse y cambiar de tamaño automáticamente para ajustarse mejor a la ventana.
- El área secundaria del contenido deba mostrarse u ocultarse en función del espacio disponible.
- El contenido deba dividirse sin problemas entre las dos pantallas de un dispositivo de doble pantalla.

## <a name="examples"></a>Ejemplos

Estas imágenes muestran una aplicación que se ejecuta en una pantalla única y se amplía dos pantallas. La vista de dos paneles adapta la UI de la aplicación a configuraciones de varias pantallas.

![Aplicación de vista en dos paneles en una sola pantalla](images/two-pane-view/tpv-single.png)

> _Aplicación en una sola pantalla._

![Aplicación de vista en dos paneles en dos pantallas en modo Wide](images/two-pane-view/tpv-dual-wide.png)

> _Aplicación que abarca un dispositivo de doble pantalla en modo Wide._

![Aplicación de vista en dos paneles en dos pantallas en modo Tall](images/two-pane-view/tpv-dual-tall.png)

> _Aplicación que abarca un dispositivo de doble pantalla en modo Tall._

## <a name="how-it-works"></a>Cómo funciona

La vista de dos paneles tiene dos paneles donde colocas el contenido. Ajusta el tamaño y la disposición de los paneles en función del espacio disponible en la ventana. Los diseños de panel posibles se definen con la enumeración [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode):

| Valor&nbsp;enum | Descripción |
| - | - |
| `SinglePane` | Solo se muestra un panel, según se especifique en la propiedad [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority). |
| `Wide` | Los paneles se muestran uno junto al otro, o se muestra un solo panel, tal y como se especifica en la propiedad [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration). |
| `Tall` | Los paneles se muestran uno sobre el otro, o se muestra un solo panel, tal y como se especifica en la propiedad [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration). |

Para configurar la vista de dos paneles, establece la propiedad [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) para especificar qué panel se muestra cuando hay espacio para un solo panel. Luego, especifica si `Pane1` se muestra en la parte superior o inferior de las ventanas Tall, o a la izquierda o a la derecha para las ventanas Wide.

La vista de dos paneles controla el tamaño y la disposición de los paneles, pero aún tienes que hacer que el contenido dentro del panel se adapte a los cambios de tamaño y orientación. Para más información sobre la creación de una interfaz de usuario adaptable, consulta [Diseños adaptativos con XAML](../layout/layouts-with-xaml.md) y [Paneles de diseño](../layout/layout-panels.md).

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) administra la presentación de los paneles en función del estado de expansión de la aplicación.

- En una sola pantalla

    Cuando la aplicación está en una sola pantalla, `TwoPaneView` ajusta el tamaño y la posición de los paneles en función de los valores de propiedad que especifiques. En la sección siguiente se explican estas propiedades con más detalle. La única diferencia entre los dispositivos es que algunos dispositivos, como los equipos de escritorio, permiten el cambio de tamaño de las ventanas, mientras que otros no.

- Ampliado a dos pantallas

    `TwoPaneView` está diseñado para facilitar la optimización de la UI para ampliarse en dispositivos de doble pantalla. La ventana cambia de tamaño automáticamente para usar todo el espacio disponible en las pantallas. Cuando la aplicación abarca ambas pantallas de un dispositivo de doble pantalla, cada pantalla muestra el contenido de uno de los paneles y divide correctamente el contenido entre la brecha. El reconocimiento de la extensión está integrado al usar la vista de dos paneles. Solo tienes que establecer la configuración Tall/Wide para especificar qué panel se muestra en qué pantalla. La vista de dos paneles se encarga del resto.

## <a name="how-to-use-the-two-pane-view-control"></a>Cómo usar el control de vista de dos paneles

No es necesario que [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) sea el elemento raíz del diseño de página. De hecho, a menudo lo usarás en un control [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) que proporciona la navegación general para la aplicación. `TwoPaneView` se adapta de manera correcta independientemente de dónde se encuentra en el árbol XAML; sin embargo, se recomienda no anidar un control `TwoPaneView` dentro de otro `TwoPaneView`. (Si lo hace, solo el `TwoPaneView` exterior será compatible con la expansión).

### <a name="add-content-to-the-panes"></a>Agregar contenido a los paneles

Cada panel de una vista de dos paneles puede contener un único `UIElement` de XAML. Para agregar contenido, normalmente colocas un panel de diseño XAML en cada panel y, luego, agregas otros controles y contenido al panel. Los paneles pueden cambiar el tamaño y cambiar entre los modos Wide y Tall, por lo que debes asegurarte de que el contenido de cada panel puede adaptarse a estos cambios. Para más información sobre la creación de una interfaz de usuario adaptable, consulta [Diseños adaptativos con XAML](../layout/layouts-with-xaml.md) y [Paneles de diseño](../layout/layout-panels.md).

En este ejemplo, se crea la UI de la aplicación sencilla de imagen/información que se muestra anteriormente en la sección _Ejemplos_. Cuando la aplicación se amplía a dos pantallas, la imagen y la información se muestran en pantallas independientes. En una sola pantalla, el contenido se puede mostrar en dos paneles o combinarse en un solo panel, en función de la cantidad de espacio disponible. (Cuando solo hay espacio para un panel, se mueve el contenido de Pane2 a Pane1, y se permite al usuario desplazarse para ver cualquier contenido oculto. Verás el código más adelante en la sección _Responder a los cambios de modo_).

![Pequeña imagen de la aplicación de ejemplo distribuida en dos pantallas](images/two-pane-view/tpv-left-right.png)

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" MinHeight="40"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <CommandBar DefaultLabelPosition="Right">
        <AppBarButton x:Name="Share" Icon="Share" Label="Share" Click="Share_Click"/>
        <AppBarButton x:Name="Print" Icon="Print" Label="Print" Click="Print_Click"/>
    </CommandBar>

    <muxc:TwoPaneView
        x:Name="MyTwoPaneView"
        Grid.Row="1"
        MinWideModeWidth="959"
        MinTallModeHeight="863"
        ModeChanged="TwoPaneView_ModeChanged">

        <muxc:TwoPaneView.Pane1>
            <Grid x:Name="Pane1Root">
                <ScrollViewer>
                    <StackPanel x:Name="Pane1StackPanel">
                        <Image Source="Assets\LandscapeImage8.jpg"
                               VerticalAlignment="Top" HorizontalAlignment="Center"
                               Margin="16,0"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane1>

        <muxc:TwoPaneView.Pane2>
            <Grid x:Name="Pane2Root">
                <ScrollViewer x:Name="DetailsContent">
                    <StackPanel Padding="16">
                        <TextBlock Text="Mountain.jpg" MaxLines="1"
                                       Style="{ThemeResource HeaderTextBlockStyle}"/>
                        <TextBlock Text="Date Taken:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="8/29/2019 9:55am"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Dimensions:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="800x536"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Resolution:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="96 dpi"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Description:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna."
                                       Style="{ThemeResource SubtitleTextBlockStyle}"
                                       TextWrapping="Wrap"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane2>
    </muxc:TwoPaneView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="TwoPaneViewStates">
            <VisualState x:Name="Normal"/>
            <VisualState x:Name="Wide">
                <VisualState.Setters>
                    <Setter Target="MyTwoPaneView.Pane1Length"
                            Value="2*"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="specify-which-pane-to-display"></a>Especificar qué panel se va a mostrar

Cuando la vista de dos paneles solo puede mostrar un único panel, usa la propiedad [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) para determinar qué panel se va a mostrar. De manera predeterminada, PanePriority se establece en **Pane1**. Aquí se muestra cómo puedes establecer esta propiedad en XAML o en código.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>Cambio de tamaño de los paneles

En una única pantalla, el tamaño de los paneles está determinado por las propiedades [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) y [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length). Usan valores [GridLength](/uwp/api/windows.ui.xaml.gridlength) que admiten el ajuste de tamaño _auto_ y _star_(\*). Consulta la sección _Propiedades de diseño_ de [Diseños adaptativos con XAML](../layout/layouts-with-xaml.md#layout-properties) para ver una explicación de los ajustes de tamaño auto y star.

De manera predeterminada, `Pane1Length` se establece en `Auto` y su tamaño se ajusta a su contenido. `Pane2Length` se establece en `*` y usa todo el espacio restante.

![Vista de dos paneles con paneles establecidos en tamaños predeterminados](images/two-pane-view/tpv-size-default.png)

> _Paneles con ajuste de tamaño predeterminado_

Los valores predeterminados son útiles para un diseño típico de principal/detalles, donde tienes una lista de elementos en `Pane1` y muchos detalles en `Pane2`. Sin embargo, en función del contenido, puede que prefieras dividir el espacio de otra manera. Aquí, `Pane1Length` se establece en `2*` de modo que se obtiene el doble de espacio que `Pane2`.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![Vista de dos paneles con el panel 1 con dos tercios de pantalla, y el panel 2 con un tercio](images/two-pane-view/tpv-size-2.png)

> _Paneles de tamaño 2* y *_

> [!NOTE]
> Como se mencionó anteriormente, cuando la aplicación se expande en dos pantallas, estas propiedades se omiten y cada panel rellena una de las pantallas.

Si establece un panel para usar el ajuste automático de tamaño, puede controlar el tamaño estableciendo el alto y el ancho del panel que contiene el contenido del panel. En este caso, es posible que tenga que controlar el evento ModeChanged y establecer las restricciones de alto y ancho del contenido según corresponda para el modo actual.

### <a name="display-in-wide-or-tall-mode"></a>Pantalla en modo Wide o Tall

En una pantalla única, el [Modo](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) de pantalla de la vista de dos paneles está determinado por las propiedades [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) y [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight). Ambas propiedades tienen un valor predeterminado de 641 px, igual que [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

En esta tabla se muestra cómo el alto y el ancho de TwoPaneView determinan el modo de visualización que se usa.

| Condición TwoPaneView  | Modo |
|---------|---------|
| `Width` > `MinWideModeWidth` | Se usa el modo `Wide` |
| `Width` <= `MinWideModeWidth` y `Height` > `MinTallModeHeight` | Se usa el modo `Tall` |
| `Width` <= `MinWideModeWidth` y `Height` <= `MinTallModeHeight` | Se usa el modo `SinglePane` |


> [!NOTE]
> Como se mencionó anteriormente, cuando la aplicación se expande en dos pantallas, estas propiedades se omiten y el modo de visualización se determina en función de la _posición_ del dispositivo.

#### <a name="wide-configuration-options"></a>Opciones de configuración Wide

La vista de dos paneles entra en el modo `Wide` cuando hay una sola pantalla que es más ancha que la propiedad `MinWideModeWidth`. `MinWideModeWidth` controla cuando la vista de dos paneles entra en el modo Wide. El valor predeterminado es de 641 px, pero puedes cambiarlo a lo que quieras. En general, tienes que establecer esta propiedad en el ancho mínimo que quieras para el panel.

Cuando la vista de dos paneles está en modo Wide, la propiedad [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) determina lo que se debe mostrar:

| [Valor&nbsp;enum](/uwp/api/microsoft.ui.xaml.controls.twopaneviewwidemodeconfiguration) | Descripción |
|---------|---------|
| `SinglePane` | Un solo panel (según lo determinado por `PanePriority`). El panel ocupa el tamaño completo de `TwoPaneView` (es decir, adopta el tamaño star en ambas direcciones). |
| `LeftRight` | `Pane1` a la izquierda/`Pane2` a la derecha. Ambos paneles tienen un tamaño star en sentido vertical, el ancho de `Pane1` se ajusta automáticamente, y el ancho de `Pane2` se ajusta según star. |
| `RightLeft` | `Pane1` a la derecha/`Pane2` a la izquierda. Ambos paneles tienen un tamaño star en sentido vertical, el ancho de `Pane2` se ajusta automáticamente, y el ancho de `Pane1` se ajusta según star. |

El valor de configuración predeterminado es `LeftRight`.

| LeftRight | RightLeft |
| - | - |
| ![Vista de dos paneles configurada de izquierda a derecha](images/two-pane-view/tpv-left-right.png)  | ![Vista de dos paneles configurada de derecha a izquierda](images/two-pane-view/tpv-right-left.png)  |

> **SUGERENCIA:** Cuando el dispositivo usa un idioma de derecha a izquierda (RTL), la vista de dos paneles intercambia el orden automáticamente: `RightLeft` se representa como `LeftRight`, y `LeftRight` se representa como `RightLeft`.

#### <a name="tall-configuration-options"></a>Opciones de configuración de Tall

La vista de dos paneles entra en el modo `Tall` cuando hay una sola pantalla que es más estrecha que `MinWideModeWidth`, y más alta que `MinTallModeHeight`. El valor predeterminado es de 641 px, pero puedes cambiarlo a lo que quieras. En general, tienes que establecer esta propiedad en la altura mínima que quieras para el panel.

Cuando la vista de dos paneles está en modo Tall, la propiedad [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) determina lo que se debe mostrar:

| [Valor&nbsp;enum](/uwp/api/microsoft.ui.xaml.controls.twopaneviewtallmodeconfiguration) | Descripción |
|---------|---------|
| `SinglePane` | Un solo panel (según lo determinado por `PanePriority`). El panel ocupa el tamaño completo de `TwoPaneView` (es decir, adopta el tamaño star en ambas direcciones). |
| `TopBottom` | `Pane1` en la parte superior/`Pane2` en la parte inferior. Ambos paneles tienen un tamaño star en sentido horizontal, la altura de `Pane1` se ajusta automáticamente y la altura de `Pane2` se ajusta según star. |
| `BottomTop` | `Pane1` en la parte inferior/`Pane2` en la parte superior. Ambos paneles tienen un tamaño star en sentido horizontal, la altura de `Pane2` se ajusta automáticamente y la altura de `Pane1` se ajusta según star. |

El valor predeterminado es `TopBottom`.

| TopBottom | BottomTop |
| - | - |
| ![Vista de dos paneles configurada de arriba-abajo](images/two-pane-view/tpv-top-bottom.png)  | ![Vista de dos paneles configurada de abajo-arriba](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Valores especiales para MinWideModeWidth y MinTallModeHeight

Puedes usar la propiedad `MinWideModeWidth` para evitar que la vista de dos paneles entre en el modo Wide, solo tienes que establecer `MinWideModeWidth` en [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0).

Si estableces `MinTallModeHeight` en [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0), esta impide que la vista de dos paneles entre en el modo Tall.

Si estableces `MinTallModeHeight` en 0, esta impide que la vista de dos paneles entre en el modo `SinglePane`.

#### <a name="responding-to-mode-changes"></a>Responder a los cambios de modo

Puede usar la propiedad de solo lectura [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) para obtener el modo de presentación actual. Cada vez que la vista de dos paneles cambia los paneles que se muestran, se produce el evento [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) antes de que represente el contenido actualizado. Puedes controlar el evento para responder a cambios en el modo de presentación.

> [!TIP]
> El evento `ModeChanged` no se produce cuando se carga inicialmente la página, por lo que el código XAML predeterminado debe representar la UI según debe aparecer cuando se carga por primera vez.

Una forma de usar este evento es actualizar la UI de la aplicación para que los usuarios puedan ver todo el contenido en el modo `SinglePane`. Por ejemplo, la aplicación de ejemplo tiene un panel principal (la imagen) y un panel de información.

![Pequeña imagen de la aplicación de ejemplo distribuida en modo Tall](images/two-pane-view/tpv-top-bottom.png)

> _Modo Tall_

Cuando solo hay espacio suficiente para mostrar un panel, puedes mover el contenido de `Pane2` a `Pane1`, de modo que el usuario pueda desplazarse para ver todo el contenido. Tiene esta apariencia.

![Imagen de la aplicación de ejemplo en una pantalla con todo el contenido desplazándose en un único panel](images/two-pane-view/tpv-single-pane.png)

> _Modo SinglePane_

Recuerda que las propiedades `MinWideModeWidth` y `MinTallModeHeight` determinan cuándo cambia el modo de visualización, así que puedes cambiar el momento en que el contenido se mueve entre los paneles si ajustas los valores de estas propiedades.

A continuación se muestra el código del controlador de eventos `ModeChanged` que mueve el contenido entre `Pane1` y `Pane2`. También establece [VisualState](/uwp/api/windows.ui.xaml.visualstate) para restringir el ancho de la imagen en modo Wide.

```csharp
private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
{
    // Remove details content from it's parent panel.
    ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
    // Set Normal visual state.
    Windows.UI.Xaml.VisualStateManager.GoToState(this, "Normal", true);

    // Single pane
    if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
    {
        // Add the details content to Pane1.
        Pane1StackPanel.Children.Add(DetailsContent);
    }
    // Dual pane.
    else
    {
        // Put details content in Pane2.
        Pane2Root.Children.Add(DetailsContent);

        // If also in Wide mode, set Wide visual state
        // to constrain the width of the image to 2*.
        if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
        {
            Windows.UI.Xaml.VisualStateManager.GoToState(this, "Wide", true);
        }
    }
}
```

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer

- Usa la vista de dos paneles siempre que puedas para que la aplicación pueda aprovechar las pantallas dobles y las pantallas de gran tamaño.
- No coloques una vista de dos paneles dentro de otra vista de dos paneles.

## <a name="related-articles"></a>Artículos relacionados

- [Información general sobre diseño](../layout/index.md)
- [Desarrollo para doble pantalla](/dual-screen)
- [Introducción a los dispositivos de doble pantalla](/dual-screen/introduction)
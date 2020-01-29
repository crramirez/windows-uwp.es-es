---
Description: TwoPaneView es un control de diseño que te ayuda a administrar la presentación de aplicaciones que tienen dos áreas de contenido distintas.
title: Vista de dos paneles
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3d12f2aad1d5dffbbad0790e5940699536daf0b
ms.sourcegitcommit: e6a435716799c7bb192b3d5c4d3b8295ec3911d4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76549710"
---
# <a name="two-pane-view"></a>Vista de dos paneles

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) es un control de diseño que te ayuda a administrar la presentación de aplicaciones que tienen dos áreas de contenido distintas, como una vista principal/detalles.

> [!IMPORTANT]
> En este artículo se describen la funcionalidad y las instrucciones que se encuentran en versión preliminar pública, por lo que es posible que puedan modificarse de forma sustancial antes de que estén disponibles con carácter general. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Aunque el control TwoPaneView funciona en todos los dispositivos Windows, está diseñado para ayudarte a sacar el máximo partido de los dispositivos de doble pantalla de manera automática, sin necesidad de ninguna codificación especial. En un dispositivo de doble pantalla, la vista de dos paneles garantiza que la interfaz de usuario (UI) se divida sin problemas cuando abarque la brecha entre las pantallas, de modo que el contenido se presente a alguno de los lados de la brecha.

> **NOTA:** Un _dispositivo de doble pantalla_ es un tipo especial de dispositivo con funcionalidades exclusivas. No equivale a un dispositivo de escritorio con varios monitores. Para obtener más información sobre los dispositivos de doble pantalla, consulta [Introducción a los dispositivos de doble pantalla](/dual-screen/introduction).
>
>En este artículo, se usan los términos _dispositivo de pantalla única_ o _de una pantalla_ para indicar cualquier dispositivo que no sea un dispositivo de doble pantalla, ya sea un monitor único o una parte de una disposición de varios monitores. El control TwoPaneView se comporta en un dispositivo de una pantalla de la misma manera que otros controles XAML. Consulta [Mostrar varias vistas](/windows/uwp/design/layout/show-multiple-views) para más información sobre las formas en que puedes optimizar la aplicación para varios monitores.

| **Obtención de la biblioteca de la interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información, incluidas instrucciones sobre la instalación, consulta la [introducción a la biblioteca de la interfaz de usuario de Windows](/uwp/toolkits/winui/). |

| **API de plataforma** | **API de la biblioteca de la interfaz de usuario de Windows** |
| - | - |
| [Clase TwoPaneView](/uwp/api/windows.ui.xaml.controls.twopaneview) | [Clase TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

En este documento, se usará el alias **muxc** en XAML para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado lo siguiente a nuestro elemento [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page):

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

En el código subyacente, se usará el alias **muxc** en C# para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado esta instrucción **using** en la parte superior del archivo:

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa la vista de dos paneles cuando tenga dos áreas de contenido distintas y:

- El contenido deba reorganizarse y cambiar de tamaño automáticamente para ajustarse mejor a la ventana.
- El área secundaria del contenido deba mostrarse u ocultarse en función del espacio disponible.
- El contenido deba dividirse sin problemas entre las dos pantallas de un dispositivo de doble pantalla.

## <a name="examples"></a>Ejemplos

Estas imágenes muestran una aplicación que se ejecuta en un dispositivo de pantalla única y en un dispositivo de doble pantalla. La vista de dos paneles adapta la interfaz de usuario de la aplicación configuraciones de varios paneles panel en cada dispositivo.

![tpv-single.png](images/two-pane-view/tpv-single.png)

_Aplicación en un dispositivo de una pantalla._

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

_Aplicación que abarca un dispositivo de doble pantalla en modo Wide._

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

_Aplicación que abarca un dispositivo de doble pantalla en modo Tall._

## <a name="how-it-works"></a>Cómo funciona

La vista de dos paneles tiene dos paneles donde colocas el contenido. Ajusta el tamaño y la disposición de los paneles en función del espacio disponible en la ventana. Los diseños de panel posibles se definen con la enumeración [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode):

- **SinglePane**: Solo se muestra un panel, según se especifique en la propiedad [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority).
- **Wide**: Los paneles se muestran uno junto al otro, o se muestra un solo panel, tal y como se especifica en la propiedad [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration).
- **Tall**: Los paneles se muestran uno sobre el otro, o se muestra un solo panel, tal y como se especifica en la propiedad [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration).

Para configurar la vista de dos paneles, establece la propiedad [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) para especificar qué panel se muestra cuando hay espacio para un solo panel. Luego, especifica si Pane1 se muestra en la parte superior o inferior de las ventanas Tall, o a la izquierda o a la derecha para las ventanas Wide.

La vista de dos paneles controla el tamaño y la disposición de los paneles, pero aún tienes que hacer que el contenido dentro del panel se adapte a los cambios de tamaño y orientación. Para más información sobre la creación de una interfaz de usuario adaptable, consulta [Diseños adaptativos con XAML](/windows/uwp/design/layout/layouts-with-xaml) y [Paneles de diseño](/windows/uwp/design/layout/layout-panels).

La vista de dos paneles administra la visualización de los paneles en función del tipo de dispositivo en el que se ejecuta la aplicación:

- En un dispositivo de doble pantalla

    La vista de dos paneles está diseñada para facilitar la optimización de la UI para dispositivos de doble pantalla. La ventana cambiará de tamaño automáticamente para usar todo el espacio disponible en las pantallas. Cuando la aplicación solo está en una de las pantallas del dispositivo, se muestra un panel, tal y como se especifica en la propiedad [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority).

    Cuando la aplicación abarca ambas pantallas de un dispositivo de doble pantalla, cada pantalla muestra el contenido de uno de los paneles y divide correctamente el contenido entre la brecha. El reconocimiento de la extensión está integrado al usar la vista de dos paneles. Solo tienes que establecer la configuración Tall/Wide para especificar qué panel se muestra en qué pantalla. La vista de dos paneles se encarga del resto.


- En dispositivos de una pantalla

    Cuando la vista de dos paneles se ejecuta en un dispositivo de una pantalla, como un equipo portátil o de escritorio, proporciona el comportamiento que se esperaría de cualquier control XAML. Cuando se cambia el tamaño de la ventana, la vista de dos paneles ajusta el tamaño y la posición de los paneles en función del tamaño de la ventana. Tiene propiedades adicionales que estableces para definir el comportamiento del control cuando se muestra en una pantalla en una ventana que puede cambiar de tamaño. En la sección siguiente se explican estas propiedades con más detalle.

## <a name="how-to-use-the-two-pane-view-control"></a>Cómo usar el control de vista de dos paneles

No es necesario que [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) sea el elemento raíz del diseño de página. De hecho, a menudo lo usarás en un control [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) que proporciona la navegación general para la aplicación. TwoPaneView se adapta de manera correcta independientemente de dónde se encuentra en el árbol XAML; sin embargo, se recomienda no anidar un control TwoPaneView dentro de otro TwoPaneView.

### <a name="add-content-to-the-panes"></a>Agregar contenido a los paneles

Cada panel de una vista de dos paneles puede contener un único UIElement de XAML. Para agregar contenido, normalmente colocas un panel de diseño XAML en cada panel y, luego, agregas otros controles y contenido al panel. Los paneles pueden cambiar el tamaño y cambiar entre los modos Wide y Tall, por lo que debes asegurarte de que el contenido de cada panel puede adaptarse a estos cambios. Para más información sobre la creación de una interfaz de usuario adaptable, consulta [Diseños adaptativos con XAML](/windows/uwp/design/layout/layouts-with-xaml) y [Paneles de diseño](/windows/uwp/design/layout/layout-panels).

En este ejemplo, se crea la UI de la aplicación sencilla de imagen/información que se muestra aquí. Cuando hay espacio para dos paneles, la imagen y la información se muestran en paneles independientes. (Cuando solo hay espacio para un panel, se mueve el contenido de Pane2 a Pane1, y se permite al usuario desplazarse para ver cualquier contenido oculto. Verás el código más adelante en la sección _Responder a los cambios de modo_).

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
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

El tamaño de los paneles en un dispositivo de una pantalla viene determinado por las propiedades [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) y [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length). Usan valores [GridLength](/uwp/api/windows.ui.xaml.gridlength) que admiten el ajuste de tamaño _auto_ y _star_(\*). Consulta la sección _Propiedades de diseño_ de [Diseños adaptativos con XAML](/windows/uwp/design/layout/layouts-with-xaml#layout-properties) para ver una explicación de los ajustes de tamaño auto y star.

De manera predeterminada, Pane1Length se establece en **Auto** y su tamaño se ajusta a su contenido. Pane2Length se establece en * y usa todo el espacio restante.

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_Paneles con ajuste de tamaño predeterminado_

Los valores predeterminados son útiles para un diseño típico de principal/detalles, donde tienes una lista de elementos en Pane1 y muchos detalles en Pane2. Sin embargo, en función del contenido, puede que prefieras dividir el espacio de otra manera. En este caso, Pane1Length se establece en 2*, por lo que obtiene el doble de espacio que Pane2.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_Paneles de tamaño 2* y *_

> **NOTA:** Como se mencionó anteriormente, en un dispositivo de doble pantalla se omiten estas propiedades y los paneles se dimensionan y se organizan automáticamente según la _postura_ del dispositivo.

Si establece un panel para usar el ajuste automático de tamaño, puede controlar el tamaño estableciendo el alto y el ancho del panel que contiene el contenido del panel. En este caso, es posible que tenga que controlar el evento ModeChanged y establecer las restricciones de alto y ancho del contenido según corresponda para el modo actual.

### <a name="display-in-wide-or-tall-mode"></a>Pantalla en modo Wide o Tall

En una pantalla de escritorio, el [Modo](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) de pantalla de la vista de dos paneles está determinado por las propiedades [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) y [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight). Ambas propiedades tienen un valor predeterminado de 641 px, igual que [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Si la ventana es:

- Más ancha que MinWideModeWidth, se usa el modo **Wide**.
- Más estrecha que MinWideModeWidth y más alta que MinTallModeHeight, se usa el modo **Tall**.
- Más estrecha que MinWideModeWidth y más baja que MinTallModeHeight, se usa el modo **SinglePane**.

> **NOTA:** Como se mencionó anteriormente, en un dispositivo de doble pantalla se omiten estas propiedades y los paneles se dimensionan y se organizan automáticamente según la _postura_ del dispositivo.

#### <a name="wide-configuration-options"></a>Opciones de configuración Wide

La vista de dos paneles entra en el modo Wide cuando hay una sola pantalla que es más ancha que la propiedad MinWideModeWidth. MinWideModeWidth controla cuando la vista de dos paneles entra en el modo Wide. El valor predeterminado es de 641 px, pero puedes cambiarlo a lo que quieras. En general, tienes que establecer esta propiedad en el ancho mínimo que quieras para el panel.

Cuando la vista de dos paneles está en modo Wide, la propiedad WideModeConfiguration determina lo que se debe mostrar:

- **SinglePane**: Un único panel (según lo determinado por PanePriority). El panel ocupa el tamaño completo de TwoPaneView (es decir, adopta el tamaño star en ambas direcciones).
- **LeftRight**: Pane1 a la izquierda/Pane2 a la derecha. Ambos paneles tienen un tamaño star en sentido vertical, el ancho de Pane1 se ajusta automáticamente y el ancho de Pane2 se ajusta según star.
- **LeftRight**: Pane1 a la derecha/Pane2 a la izquierda. Ambos paneles tienen un tamaño star en sentido vertical, el ancho de Pane2 se ajusta automáticamente y el ancho de Pane1 se ajusta según star.

El valor predeterminado es **LeftRight**.

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **SUGERENCIA:** Cuando el dispositivo usa un idioma de derecha a izquierda (RTL), la vista de dos paneles intercambia el orden automáticamente: RightLeft se representa como LeftRight, y LeftRight se representa como RightLeft.

#### <a name="tall-configuration-options"></a>Opciones de configuración de Tall

La vista de dos paneles entra en el modo Tall cuando hay una sola pantalla que es más estrecha que MinWideModeWidth y más alta que MinTallModeHeight. El valor predeterminado es de 641 px, pero puedes cambiarlo a lo que quieras. En general, tienes que establecer esta propiedad en la altura mínima que quieras para el panel.

Cuando la vista de dos paneles está en modo Wide, la propiedad TallLayout determina lo que se debe mostrar:

- **SinglePane**: Un único panel (según lo determinado por PanePriority). El panel ocupa el tamaño completo de TwoPaneView (es decir, adopta el tamaño star en ambas direcciones).
- **TopBottom**: Pane1 en la parte superior/Pane2 a la derecha. Ambos paneles tienen un tamaño star en sentido horizontal, la altura de Pane1 se ajusta automáticamente y la altura de Pane2 se ajusta según star.
- **BottomTop**: Pane1 a la derecha/Pane2 a la izquierda. Ambos paneles tienen un tamaño star en sentido horizontal, la altura de Pane2 se ajusta automáticamente y la altura de Pane1 se ajusta según star.

El valor predeterminado es **TopBottom**.

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Valores especiales para MinWideModeWidth y MinTallModeHeight

Puedes usar la propiedad MinWideModeWidth para evitar que la vista de dos paneles entre en el modo Wide, solo tienes que establecer MinWideModeWidth en [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0).

Si estableces MinTallModeHeight en [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0), esta impide que la vista de dos paneles entre en el modo Tall.

Si estableces MinTallModeHeight en 0, esta impide que la vista de dos paneles entre en el modo SinglePane.

#### <a name="responding-to-mode-changes"></a>Responder a los cambios de modo

Puede usar la propiedad de solo lectura [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) para obtener el modo de presentación actual. Cada vez que la vista de dos paneles cambia el o los paneles que se muestran, se produce el evento [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) antes de que represente el contenido actualizado. Puedes controlar el evento para responder a cambios en el modo de presentación.

Una forma de usar este evento es actualizar la UI de la aplicación para que los usuarios puedan ver todo el contenido en el modo SinglePane. Por ejemplo, la aplicación de ejemplo tiene un panel principal (la imagen) y un panel de información.

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_Modo Wide_

Cuando solo hay espacio suficiente para mostrar un panel, mueves el contenido de Pane2 a Pane1, de modo que el usuario pueda desplazarse para ver todo el contenido. Tiene esta apariencia.

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_Modo SinglePane_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Usa la vista de dos paneles siempre que puedas para que la aplicación pueda aprovechar los dispositivos de doble pantalla y las pantallas de gran tamaño.
- No coloques una vista de dos paneles dentro de otra vista de dos paneles.

## <a name="related-articles"></a>Artículos relacionados

- [Información general sobre diseño](../layout/index.md)
- [Desarrollo para doble pantalla](/dual-screen)
- [Introducción a los dispositivos de doble pantalla](/dual-screen/introduction)

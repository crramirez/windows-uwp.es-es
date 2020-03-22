---
author: knicholasa
description: La profundidad Z, la profundidad relativa y la sombra son dos maneras de incorporar la profundidad en la aplicación para ayudar a los usuarios a centrarse de forma natural y eficaz.
title: Profundidad Z y sombra para aplicaciones UWP
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081386"
---
# <a name="z-depth-and-shadow"></a>Profundidad Z y sombra

![GIF que muestra cuatro rectángulos grises apilados diagonalmente, uno encima de otro. El GIF se anima de modo que las sombras aparecen y desaparecen.](images/elevation-shadow/shadow.gif)

La creación de una jerarquía visual de los elementos de la interfaz de usuario facilita el análisis de la interfaz de usuario y transmite qué es importante centrarse. Elevación, a menudo se usa la acción de traer los elementos seleccionados de la interfaz de usuario hacia delante, para lograr tal jerarquía en el software. En este artículo se describe cómo crear la elevación en una aplicación de UWP con la profundidad z y la sombra.

La profundidad Z es un término que se usa entre los creadores de aplicaciones 3D para indicar la distancia entre dos superficies a lo largo del eje z. Muestra cómo cerrar un objeto en el visor. Imagínese como un concepto similar a las coordenadas x/y, pero en la dirección z.

## <a name="why-use-z-depth"></a>¿Por qué usar la profundidad z?

En el mundo físico, nos centraremos en los objetos que más se acerquen a nosotros. También podemos aplicar este instinto espacial a la interfaz de usuario digital. Por ejemplo, si trae un elemento más cerca del usuario, el usuario instintivamente se centrará en el elemento. Al mover los elementos de la interfaz de usuario más cerca del eje z, puede establecer una jerarquía visual entre los objetos, ayudando a los usuarios a completar tareas de forma natural y eficaz en la aplicación.

## <a name="what-is-shadow"></a>¿Qué es Shadow?

Shadow es una de las formas en que un usuario percibe la elevación. La luz sobre un objeto con privilegios elevados crea una sombra en la superficie siguiente. Cuanto mayor sea el objeto, mayor será la sombra y más flexible. Los objetos con privilegios elevados en la interfaz de usuario no necesitan tener sombras, pero ayudan a crear la apariencia de la elevación.

En las aplicaciones para UWP, las sombras deben usarse en un propósito en lugar de una manera estética. El uso de demasiadas sombras disminuirá o eliminará la capacidad de la sombra para centrarse en el usuario.

Si utiliza controles estándar, las sombras de ThemeShadow se incorporarán automáticamente a la interfaz de usuario. Sin embargo, puede incluir manualmente las sombras en la interfaz de usuario mediante el ThemeShadow o las API de la sombra. 

## <a name="themeshadow"></a>ThemeShadow

El tipo [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) se puede aplicar a cualquier elemento XAML para dibujar las sombras de forma adecuada en función de las coordenadas x, y, z. ThemeShadow también se ajusta automáticamente para otras especificaciones del entorno:

- Se adapta a los cambios en la iluminación, el tema del usuario, el entorno de la aplicación y el shell.
- Aplica sombras a los elementos automáticamente según su profundidad z. 
- Mantiene los elementos sincronizados mientras se mueven y cambian la elevación.
- Mantiene las sombras coherentes en todas las aplicaciones.

Aquí se muestra cómo se ha implementado ThemeShadow en un MenuFlyout. MenuFlyout tiene una experiencia integrada en la que la superficie principal se eleva a 32px y cada menú en cascada adicional se abre + 8px encima del menú desde el que se abre.

![Captura de pantalla de ThemeShadow aplicada a un MenuFlyout con tres menús anidados abiertos. El primer menú está elevado a 32px y cada menú siguiente que se abre desde el menú anterior se eleva 8px más, de modo que deja una sombra distinta en el fondo.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow en controles comunes

Los controles comunes siguientes usarán automáticamente ThemeShadow para convertir sombras de profundidad de 32px, a menos que se especifique lo contrario:

- [Menú contextual](../controls-and-patterns/menus.md), [barra de comandos](../controls-and-patterns/app-bars.md), [control flotante](../controls-and-patterns/command-bar-flyout.md)de la barra de comandos, barra de [menús](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Cuadros de diálogo y controles flotantes](../controls-and-patterns/dialogs.md) (cuadro de diálogo en 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, splitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Selector de calendario, fecha y hora](../controls-and-patterns/date-and-time.md)
- [Información sobre herramientas](../controls-and-patterns/tooltips.md) (16px)
- [Control de transporte multimedia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animación conectada](../motion/connected-animation.md)

Nota: los controles flotantes solo aplicarán ThemeShadow cuando se compilan en Windows 10 versión 1903 o un SDK más reciente.

### <a name="themeshadow-in-popups"></a>ThemeShadow en elementos emergentes

Suele ser el caso de que la interfaz de usuario de la aplicación use un elemento emergente para escenarios en los que se necesita atención y acción rápida del usuario. Estos son buenos ejemplos en los que se debe usar Shadow para ayudar a crear una jerarquía en la interfaz de usuario de la aplicación.

ThemeShadow convierte automáticamente las sombras cuando se aplican a cualquier elemento XAML en un control [popup](/uwp/api/windows.ui.xaml.controls.primitives.popup). Se convertirán las sombras en el contenido de fondo de la aplicación detrás de ella y en cualquier otro elemento emergente abierto que esté debajo.

Para usar ThemeShadow con elementos emergentes, utilice la propiedad `Shadow` para aplicar ThemeShadow a un elemento XAML. A continuación, eleve el elemento de otros elementos subyacentes, por ejemplo, mediante el componente z de la propiedad `Translation`.
Para la mayoría de la interfaz de usuario emergente, la elevación predeterminada recomendada en relación con el contenido en segundo plano de la aplicación es 32 píxeles efectivos.

En este ejemplo se muestra un rectángulo en un control popup que convierte una sombra en el contenido del fondo de la aplicación y cualquier otro elemento emergente detrás de él:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![Un solo elemento emergente rectangular con una sombra.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Deshabilitar ThemeShadow predeterminado en controles de control flotante personalizados

Los controles basados en [FLYOUT](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) o [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) usan automáticamente ThemeShadow para convertir una sombra.

Si la sombra predeterminada no tiene un aspecto correcto en el contenido del control, puede deshabilitarla estableciendo la propiedad [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) en `false` en el FlyoutPresenter asociado:

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>ThemeShadow en otros elementos

En general, le recomendamos que piense detenidamente en el uso de la sombra y limite su uso a los casos en los que se presenta una jerarquía visual significativa. Sin embargo, se proporciona una manera de convertir una sombra de cualquier elemento de la interfaz de usuario en caso de que tenga escenarios avanzados que lo necesiten.

Para convertir una sombra de un elemento XAML que no está en un elemento emergente, debe especificar explícitamente los otros elementos que pueden recibir la sombra en la colección de `ThemeShadow.Receivers`. Los receptores no pueden ser antecesores de la conversión en el árbol visual.

En este ejemplo se muestran dos rectángulos que proyectan sombras en una cuadrícula detrás de ellos:

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![Dos rectángulos turquesa junto entre sí, ambos con sombras.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Prácticas recomendadas de rendimiento para ThemeShadow

1. El sistema establece un límite de cinco pares de convertidores-receptor y desactivará la sombra si se supera. Ajustar al límite aplicado por el sistema de 5 pares de transmisiones de conversión.

2. Limite el número de elementos de receptor personalizados al mínimo necesario.

3. Si hay varios elementos Receiver con la misma elevación, intente combinarlos con un solo elemento primario en su lugar.

4. Si varios elementos van a convertir el mismo tipo de sombra en los mismos elementos de receptor, agregue la sombra como recurso compartido y vuelva a usarla.

## <a name="drop-shadow"></a>Sombra paralela

La sombra no responde automáticamente a su entorno y no usa fuentes luminosas. Para las implementaciones de ejemplo, vea la clase de la [sombra](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>¿Qué sombra debo usar?

| Propiedad | ThemeShadow | Sombra |
| - | - | - |
| **SDK mínimo** | Windows 10 versión 1903 | 14393 |
| **Adaptabilidad** | Sí | No |
| **Personalización** | No | Sí |
| **Fuente de luz** | Automática (global de forma predeterminada, pero puede invalidar por aplicación) | Ninguno |
| **Compatible con entornos 3D** | Sí | No |

- Tenga en cuenta que el propósito de la sombra es proporcionar una jerarquía significativa, no como un tratamiento visual sencillo.
- Por lo general, se recomienda el uso de ThemeShadow, que se adapta automáticamente a su entorno.
- En lo que respecta al rendimiento, limite el número de sombras, use otro tratamiento visual o use la sombra.
- Si tiene escenarios más avanzados para lograr la jerarquía visual, considere la posibilidad de usar otro tratamiento visual (por ejemplo, color). Si se necesita la sombra, use la sombra.

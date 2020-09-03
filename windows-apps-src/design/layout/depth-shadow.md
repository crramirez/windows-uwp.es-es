---
author: knicholasa
description: La profundidad Z, o profundidad relativa, y la sombra son dos maneras de incorporar profundidad en la aplicación para ayudar a los usuarios a centrarse de forma natural y eficaz.
title: Profundidad Z y sombra en aplicaciones de Windows
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: fc2adb295df97cf1af49608d15c135b9f56b4594
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165569"
---
# <a name="z-depth-and-shadow"></a>Profundidad Z y sombra

![GIF que muestra cuatro rectángulos grises apilados en diagonal, uno encima de otro. El GIF se anima de modo que las sombras aparecen y desaparecen.](images/elevation-shadow/shadow.gif)

Crear una jerarquía visual de los elementos de la interfaz de usuario facilita el análisis de la interfaz de usuario y transmite en qué debe centrarse el usuario. La elevación es la acción de adelantar determinados elementos de la interfaz de usuario, y suele usarse para lograr esta jerarquía en el software. En este artículo se describe cómo crear elevación en una aplicación de Windows con profundidad Z y sombra.

La profundidad Z es un término que se usa entre los creadores de aplicaciones 3D para indicar la distancia entre dos superficies a lo largo del eje Z. Muestra cómo cerrar un objeto en el visor. Es un concepto similar a las coordenadas X e Y, pero en la dirección 
Z.

## <a name="why-use-z-depth"></a>¿Por qué usar la profundidad Z?

En el mundo físico, tendemos a centrarnos en los objetos que están más próximos a nosotros. También podemos aplicar este instinto espacial a la interfaz de usuario digital. Por ejemplo, si acercas un elemento al usuario, este instintivamente se centrará en él. Al acercar los elementos de la interfaz de usuario en el eje Z, puedes establecer una jerarquía visual entre los objetos y ayudar a los usuarios a completar las tareas de forma natural y eficaz en la aplicación.

## <a name="what-is-shadow"></a>¿Qué la sombra?

La sombra es una de las formas en que un usuario percibe la elevación. La luz que se proyecta sobre un objeto elevado crea una sombra en la superficie situada debajo. Cuanto mayor sea el objeto, mayor será la sombra y más suave. No es necesario que los objetos elevados en la interfaz de usuario tengan sombras, pero ayudan a crear la sensación de elevación.

En las aplicaciones de Windows, las sombras deben usarse con un fin más que por estética. Usar demasiadas sombras disminuirá o eliminará la capacidad de la sombra de llamar la atención al usuario.

Si utilizas controles estándar, las sombras de ThemeShadow se incorporarán automáticamente a la interfaz de usuario. Sin embargo, puedes incluir manualmente las sombras en la interfaz de usuario mediante las API ThemeShadow o DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

El tipo [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) se puede aplicar a cualquier elemento XAML para dibujar las sombras en función de las coordenadas X, Y y Z. ThemeShadow también se ajusta automáticamente a otras especificaciones del entorno:

- Se adapta a los cambios en la iluminación, el tema del usuario, el entorno de la aplicación y el shell.
- Aplica sombras a los elementos automáticamente según su profundidad Z. 
- Mantiene los elementos sincronizados mientras se mueven y cambian la elevación.
- Mantiene las sombras coherentes en todas las aplicaciones.

Aquí se muestra cómo se ha implementado ThemeShadow en un control MenuFlyout. De forma predeterminada, la superficie principal de MenuFlyout se eleva a 32 px y cada menú en cascada adicional se abre 8 px por encima del menú desde el cual se abre.

![Captura de pantalla de ThemeShadow aplicado a un control MenuFlyout con tres menús anidados abiertos. El primer menú se eleva 32 px y cada menú siguiente que se abre desde el menú anterior se eleva 8 px más, de modo que deja una sombra distinta en el fondo.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow en controles comunes

Los controles comunes siguientes usarán automáticamente ThemeShadow para proyectar sombras de 32 px de profundidad, a menos que se especifique lo contrario:

- [Menú contextual](../controls-and-patterns/menus.md), [barra de comandos](../controls-and-patterns/app-bars.md), [control flotante de barra de comandos](../controls-and-patterns/command-bar-flyout.md), [barra de menús](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Cuadros de diálogo y controles flotantes](../controls-and-patterns/dialogs-and-flyouts/index.md) (cuadro de diálogo a 64 px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Selectores de calendario, fecha y hora](../controls-and-patterns/date-and-time.md)
- [Información sobre herramientas](../controls-and-patterns/tooltips.md) (16 px)
- [Control de transporte multimedia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animación conectada](../motion/connected-animation.md)

Nota: Los controles flotantes solo aplicarán ThemeShadow cuando se compilan en Windows 10 versión 1903 o un SDK más reciente.

### <a name="themeshadow-in-popups"></a>ThemeShadow en elementos emergentes

Suele ocurrir que la interfaz de usuario de la aplicación usa un elemento emergente para escenarios en los que se necesita la atención y acción rápida del usuario. Estos son buenos ejemplos en los que se deben usar sombras para ayudar a crear una jerarquía en la interfaz de usuario de la aplicación.

ThemeShadow proyecta sombras automáticamente cuando se aplica a cualquier elemento XAML en un control [Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup). Proyecta sombras sobre el contenido de fondo de la aplicación situado detrás, y sobre cualquier otro elemento emergente abierto que haya debajo.

Para usar ThemeShadow con elementos emergentes, utiliza la propiedad `Shadow` para aplicar ThemeShadow a un elemento XAML. A continuación, eleva el elemento por encima de otros elementos, por ejemplo, con el componente Z de la propiedad `Translation`.
En la mayoría de los elementos emergentes de interfaz de usuario, la elevación predeterminada recomendada en relación con el contenido en segundo plano de la aplicación es de 32 píxeles efectivos.

En este ejemplo se muestra un rectángulo en un control Popup que proyecta una sombra sobre el contenido del fondo de la aplicación y cualquier otro elemento emergente que haya detrás de él:

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

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Deshabilitar el comportamiento de ThemeShadow predeterminado en controles Flyout

Los controles basados en [Flyout](/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.menuflyout) o [TimePickerFlyout](/uwp/api/windows.ui.xaml.controls.timepickerflyout) utilizan automáticamente ThemeShadow para proyectar una sombra.

Si la sombra predeterminada no se ve bien sobre el contenido del control, puedes deshabilitarla estableciendo la propiedad [IsDefaultShadowEnabled](/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) en `false`, en el objeto FlyoutPresenter asociado:

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

Por lo general, te recomendamos que pienses detenidamente en el uso de la sombra y limites su uso a los casos en los que cree una jerarquía visual significativa. Sin embargo, si lo necesitas en algún escenario avanzado, hay una manera de proyectar una sombra desde cualquier elemento de la interfaz de usuario.

Para proyectar una sombra de un elemento XAML que no sea un control Popup, debes especificar explícitamente los otros elementos que pueden recibir la sombra en la colección `ThemeShadow.Receivers`. Los receptores no pueden ser antecesores del proyector en el árbol visual.

En este ejemplo se muestran dos rectángulos que proyectan sombras en una cuadrícula situada detrás de ellos:

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

![Dos rectángulos turquesa uno al lado del otro, ambos con sombras.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Procedimientos recomendados para mejorar el rendimiento de ThemeShadow

1. El sistema establece un límite de cinco pares de proyector-receptor, y desactivará la sombra si se supera este límite. Respeta el límite aplicado por el sistema de 5 pares de proyector-receptor.

2. Limita el número de elementos receptores personalizados al mínimo necesario.

3. Si hay varios elementos receptores con la misma elevación, intenta combinarlos dirigiéndote a un solo elemento principal en su lugar.

4. Si varios elementos van a proyectar el mismo tipo de sombra en los mismos elementos receptores, agrega la sombra como recurso compartido y vuelve a usarla.

## <a name="drop-shadow"></a>Sombra paralela

DropShadow no responde automáticamente al entorno y no usa fuentes de luz. Para ver implementaciones de ejemplo, consulta la [clase DropShadow](/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>¿Qué sombra debo usar?

| Propiedad | ThemeShadow | DropShadow |
| - | - | - |
| **SDK mínimo** | Windows 10 versión 1903 | 14393 |
| **Capacidad de adaptación** | Sí | No |
| **Personalización** | No | Sí |
| **Fuente de luz** | Automática (global de forma predeterminada, pero se puede invalidar por aplicación) | Ninguno |
| **Compatible con entornos 3D** | Sí | No |

- Ten en cuenta que el propósito de la sombra es crear una jerarquía significativa; no es un tratamiento visual simple.
- Por lo general, se recomienda usar ThemeShadow, que se adapta automáticamente al entorno.
- En lo que respecta al rendimiento, limita el número de sombras, usa otro tratamiento visual o usa DropShadow.
- Si necesitas una jerarquía visual en escenarios más avanzados, considera la posibilidad de usar otro tratamiento visual (por ejemplo, color). Si necesitas la sombra, usa DropShadow.
---
author: knicholasa
description: Profundidad de la Z o relativa a la profundidad y sombra son dos maneras de incorporar la profundidad en su aplicación para ayudar a los usuarios se centren forma natural y eficaz.
title: Profundidad Z e instantáneas para aplicaciones UWP
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817733"
---
# <a name="z-depth-and-shadow"></a>Profundidad Z y sombra

![Un gif que muestra cuatro rectángulos de color gris que son diagonalmente, apilados uno encima de otro. Se anima el formato gif para que las sombras aparecen y desaparecen.](images/elevation-shadow/shadow.gif)

Creación de una jerarquía visual de elementos en la interfaz de usuario facilita el análisis de la interfaz de usuario y transmite lo que es importante centrarse. Elevación, el acto de Traer elementos seleccionados de la interfaz de usuario al día, a menudo se usa para lograr este tipo de jerarquía en el software. En este artículo se describe cómo crear elevación en una aplicación para UWP con profundidad z e instantáneas.

Profundidad de Z es un término utilizado entre los creadores de aplicaciones 3D para indicar la distancia entre dos superficies a lo largo del eje z. Ilustra cómo cerrar un objeto está en el Visor. Puede considerar como un concepto similar a x / y coordenadas, pero en la dirección z.

## <a name="why-use-z-depth"></a>¿Por qué usar profundidad z?

En el mundo físico, solemos centrarnos en los objetos que están más próximos a nosotros. Podemos aplicar ese instinto espacial a digital también interfaz de usuario. Por ejemplo, si poner un elemento más cercano al usuario, el usuario instintivamente se centrará en el elemento. Al mover elementos de IU más cerca en el eje z, puede establecer una jerarquía visual entre objetos, ayuda a los usuarios completar tareas de forma natural y eficaz de la aplicación.

## <a name="what-is-shadow"></a>¿Qué es instantáneas?

Sombra es una forma de que un usuario percibe la elevación. Luz sobre un objeto con privilegios elevados, crea una sombra en la superficie de debajo. Cuanto mayor sea el objeto, el mayor y más suave se convierte en la sombra. Los objetos con privilegios elevados en la interfaz de usuario no es necesario tener sombras, pero ayudan a crear la apariencia de elevación.

En aplicaciones UWP, sombras deben usarse de forma intencionada en lugar de estéticas. Uso de las sombras demasiados se reducir o eliminar la capacidad de la sombra para centrarse en el usuario.

Si utiliza controles estándar, sombras ThemeShadow se incorporará automáticamente la interfaz de usuario. Sin embargo, puede incluir manualmente las sombras en la interfaz de usuario mediante el ThemeShadow o las APIs DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

El tipo se puede aplicar a cualquier elemento XAML que se va a dibujar de ThemeShadow sombrea adecuadamente en función de x, y, z coordenadas. ThemeShadow también ajusta automáticamente para otras especificaciones medioambientales:

- Se adapta a los cambios de iluminación, el tema de usuario, el entorno de aplicación y shell.
- Se aplica a las sombras para los elementos automáticamente según su profundidad z. 
- Sincroniza los elementos en que se mueven y cambia la elevación.
- Mantiene la coherencia a lo largo de y en todas las aplicaciones de sombras.

Aquí es cómo ThemeShadow se ha implementado en un MenuFlyout. Tiene integradas MenuFlyout en donde se eleva la superficie de principal a 32 px y se abre cada menú en cascada adicionales + 8px por encima del menú que se abre desde.

![Una captura de pantalla de ThemeShadow aplicado a un MenuFlyout con tres menús anidados, abrir. El primer menú es 32 px con privilegios elevados y cada menú subsiguientes que se abre desde el menú anterior es elevado 8px más que deja una sombra distinta en segundo plano.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow controla en común

Los controles comunes siguientes usarán automáticamente ThemeShadow para convertir las sombras de profundidad 32 px a menos que se especifique lo contrario:

- [Menú contextual](../controls-and-patterns/menus.md), [barra de comandos](../controls-and-patterns/app-bars.md), [de comandos de la barra flotante](../controls-and-patterns/command-bar-flyout.md), [barra de menús](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Los cuadros de diálogo y menús emergentes](../controls-and-patterns/dialogs.md) (cuadro de diálogo en 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [Cuadro combinado](../controls-and-patterns/combo-box.md), [ToggleSplitButton DropDownButton, SplitButton,](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Selectores de calendario, fecha y hora](../controls-and-patterns/date-and-time.md)
- [Información sobre herramientas](../controls-and-patterns/tooltips.md) (16px)
- [Control de transporte de medios](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animación conectada](../motion/connected-animation.md)

Nota: Menús emergentes solo aplicarán ThemeShadow cuando compiló Windows 10 versión 1903 o un SDK más reciente.

### <a name="themeshadow-in-popups"></a>ThemeShadow en menús emergentes

Suele ser el caso de que la interfaz de usuario de la aplicación utiliza un elemento emergente para escenarios donde necesita acción rápida y la atención del usuario. Estos son buenos ejemplos de cuándo se deben usar instantáneas para ayudar a crear la jerarquía en la interfaz de usuario de la aplicación.

ThemeShadow convierte automáticamente las sombras cuando se aplica a cualquier elemento XAML en un [emergente](/uwp/api/windows.ui.xaml.controls.primitives.popup). Lo convertirá las sombras en el contenido de aplicación en segundo plano detrás de él y los elementos emergentes otros abiertos debajo de él.

Para usar ThemeShadow con menús emergentes, use el `Shadow` propiedad para aplicar un ThemeShadow a un elemento XAML. A continuación, elevar el elemento desde otros elementos detrás de él, por ejemplo mediante el componente z de la `Translation` propiedad.
Para la mayoría de la interfaz de usuario de elemento emergente, la elevación de valor predeterminado recomendado en relación con el contenido de aplicación en segundo plano es 32 píxeles efectivos.

En este ejemplo se muestra un rectángulo en un cuadro emergente proyectara una sombra en el contenido de fondo de la aplicación y los otros elementos emergentes detrás de él:

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

![Un único elemento rectangular emergente con una sombra.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Deshabilitar predeterminado controla ThemeShadow en flotante personalizada

Los controles según [flotante](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) o [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) automáticamente utilizar ThemeShadow para convertir un sombra.

Si no tiene el aspecto correcta en la sombra de forma predeterminada el contenido del control, a continuación, se puede deshabilitar estableciendo la [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) propiedad `false` en el FlyoutPresenter asociado:

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

En general se recomienda medite concienzudamente sobre el uso de instantáneas y limitar su uso en casos donde introduce significativo jerarquía visual. Sin embargo, se proporciona una manera de convertir una sombra desde cualquier elemento de interfaz de usuario avanzado de escenarios que se imponen.

Para convertir una sombra de un elemento XAML que no se encuentra en una ventana emergente, debe especificar explícitamente los demás elementos que pueden recibir la sombra en el `ThemeShadow.Receivers` colección. Los receptores no pueden ser un antecesor de la caster en el árbol visual.

Este ejemplo muestra dos rectángulos que convierte las sombras en una cuadrícula detrás de ellos:

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

![Dos rectángulos turquesa junto a la otra, ambos con sombras.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Procedimientos recomendados para ThemeShadow

1. El sistema establece un límite de 5 pares caster receptor y se desactivará instantáneas si se supera este límite. Ajustarse al límite de 5 pares caster receptor impuestas por el sistema.

2. Limitar el número de elementos personalizada de receptor al mínimo necesario.

3. Si varios elementos receiver tienen la misma elevación, intente combinarlas estableciendo como destino un elemento primario único en su lugar.

4. Si varios elementos convertirá el mismo tipo de sombra en los mismos elementos de receptor, a continuación, agregue la sombra como un recurso compartido y volver a usarlo.

## <a name="drop-shadow"></a>Sombra paralela

Un objeto DropShadow no está respondiendo automáticamente a su entorno y no utiliza fuentes de luz. Por ejemplo las implementaciones, consulte la [DropShadow clase](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>¿Qué instantánea debe usar?

| Property | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10 versión 1903 | 14393 |
| **Capacidad de adaptación** | Sí | No |
| **Personalización** | No | Sí |
| **Fuente de luz** | Automático (global de forma predeterminada, pero puede invalidar por aplicación) | Ninguno |
| **Admite en entornos de 3D** | Sí | No |

- Tenga en cuenta que el propósito de la sombra es proporcionar una jerarquía significativa, no como un tratamiento visual simple.
- Por lo general, se recomienda usar ThemeShadow, que se adapta automáticamente a su entorno.
- Para problemas de rendimiento, limite el número de sombras, utilice otro tratamiento visual o usar DropShadow.
- Escenarios para lograr una jerarquía visual más avanzada, considere el uso de otro tratamiento visual (por ejemplo, color). Si es necesario instantáneas, a continuación, use un objeto DropShadow.

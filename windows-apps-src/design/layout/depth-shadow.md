---
author: serenaz
description: Profundidad Z o relativa a la profundidad y sombras son dos formas de incorporar profundidad a tu aplicación para ayudar a los usuarios puedan centrarse de forma natural y eficaz.
title: Profundidad Z y sombras para aplicaciones para UWP
template: detail.hbs
ms.author: sezhen
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: chigy
design-contact: balrayit
ms.localizationpriority: medium
ms.openlocfilehash: a1433b131b994ee2b1323909bc7c195e00f43cde
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "4574253"
---
# <a name="z-depth-and-shadow"></a>Profundidad Z y sombra

![profundidad true](images/elevation-shadow/depth.svg)

El sistema de profundidad Fluent usa los conceptos de físicos como 3D posicionamiento, luz, y sombras convertirse la interfaz de usuario de forma digital pueden considerarse en un entorno físico más en capas. Profundidad Z o relativa a la profundidad y sombras son dos formas de incorporar profundidad en tu aplicación para UWP.

## <a name="what-is-z-depth"></a>¿Qué es la profundidad de z?

Profundidad Z es la distancia entre dos superficies a lo largo del eje z, y muestra cómo cerrar un objeto está en el Visor de.

![profundidad z](images/elevation-shadow/elevation.svg)

### <a name="why-use-z-depth"></a>¿Por qué usar profundidad z?

En el mundo físico, se suele centrarse en los objetos que están más próximos a nosotros. Podemos aplicar este impulso espacial a IU digital. Por ejemplo, si se coloca un elemento más próximo al usuario, el usuario instintivamente se centrará en el elemento. Mover la interfaz de usuario elementos más cerca en el eje z, puede establecer una jerarquía visual entre objetos, lo que ayuda a los usuarios a completar las tareas de forma natural y eficaz en la aplicación. 

![profundidad z en el menú de contenido](images/elevation-shadow/whyelevation.svg)

Además proporcionar significativa jerarquía visual, profundidad z también permite crear experiencias que fluyan sin problemas desde 2D para entornos 3D, escalado de la aplicación en todos los dispositivos y factores de forma. 

![profundidad z en 2d a 3d](images/elevation-shadow/elevation-2d3d.svg)

### <a name="how-is-z-depth-perceived"></a>¿Cómo se percibe la profundidad z?

En función de cómo se percibe profundidad en el mundo físico, hay varias técnicas que pueden usarse para mostrar la proximidad en la interfaz de usuario digital.

- **Escala** Los objetos más aparecen más pequeños que objetos más cerca del mismo tamaño. Este es el método es difícil demostrar eficazmente en el espacio 2D, por lo que normalmente no se recomienda. Sin embargo, puedes usar escala y [sombras](#what-is-shadow) para crear una simulación eficaz de objetos que se mueven más cerca del usuario en 2D.

    ![proximidad con escala](images/elevation-shadow/elevation-scale.svg)

- **Atmósfera** Los objetos pueden aparecer más lejos y desenfocada con una superposición de "con humo" u otros efectos atmosférica.

    ![proximidad con atmósfera](images/elevation-shadow/elevation-atmosphere.svg)

- **Movimiento** Velocidad relativa puede usarse para demostrar la proximidad: objetos más cerca se muevan más rápidamente que los objetos distantes en segundo plano. Para obtener información sobre cómo implementar este efecto, consulta el [efecto Parallax](../motion/parallax.md).

    ![proximidad con movimiento](images/elevation-shadow/elevation-motion.svg)

### <a name="recommendations-for-z-depth"></a>Recomendaciones para la profundidad de z

Reducir el número de planos con privilegios elevados para proporcionar el foco visual clara. Para la mayoría de los escenarios, dos planos es suficiente: uno para elementos de primer plano (alto proximidad) y otro para los elementos en segundo plano (bajo proximidad). Si tienes varios elementos con privilegios elevados que no se superpongan, agruparlos el mismo plano (es decir, en primer plano) para reducir el número de planos.

![profundidad z dentro de una aplicación](images/elevation-shadow/app-depth.svg)

## <a name="what-is-shadow"></a>¿Qué es la sombra?

![sombra](images/elevation-shadow/shadow.svg)

Sombra es una forma para percibir la elevación. Cuando hay luz por encima de un objeto con privilegios elevados, existe una sombra en la superficie a continuación. Cuanto mayor sea el objeto, más grande y más suave se convierte en la sombra. Ten en cuenta que los objetos con privilegios elevados no es necesario tener sombras, pero sombras indicar la elevación.

En aplicaciones para UWP, sombras deben estar intencionado y no estético. Si las sombras reducen el foco y la productividad, a continuación, limitar el uso de sombra.

Puedes usar las sombras con la ThemeShadow o DropShadow APIs.

## <a name="themeshadow"></a>ThemeShadow

El ThemeShadow tipo puede aplicarse a cualquier elemento XAML para dibujar sombras adecuado en función de x, y, z de posición. ThemeShadow también se ajusta automáticamente para otras especificaciones del entorno:

- Se adapta a los cambios de iluminación, el tema de usuario, el entorno de la aplicación y el shell.
- Sombras elementos automáticamente en función de su elevación.
- Mantiene los elementos sincronizados cuando se muevan y cambia la elevación.
- Mantiene sombras coherente en toda y aplicaciones.

Estos son algunos ejemplos de ThemeShadow en diferentes elevaciones con los temas claros y oscuros:

![sombras inteligentes con el tema claro](images/elevation-shadow/smartshadow-light.svg)

![sombras inteligentes con tema oscuro](images/elevation-shadow/smartshadow-dark.svg)

### <a name="themeshadow-in-common-controls"></a>Los controles en común ThemeShadow

Los siguientes controles comunes usará automáticamente ThemeShadow para modelar sombras:

- [Cuadros de diálogo y controles flotantes](../controls-and-patterns/dialogs.md)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [Control de transporte de medios](../controls-and-patterns/media-playback.md)
- [Menú contextual](../controls-and-patterns/menus.md)
- [Barra de comandos](../controls-and-patterns/app-bars.md)
- [Sugerencias automáticas](../controls-and-patterns/auto-suggest-box.md), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [selectores de calendario, fecha y hora](../controls-and-patterns/date-and-time.md), [información sobre herramientas](../controls-and-patterns/tooltips.md)
- [Teclas de acceso](../input/access-keys.md)

### <a name="themeshadow-in-popups"></a>ThemeShadow en los elementos emergentes

ThemeShadow automáticamente proyecta sombras cuando se aplican a cualquier elemento XAML en un [elemento emergente](/uwp/api/windows.ui.xaml.controls.primitives.popup). Transmitirá sombras en el contenido de la aplicación en segundo plano detrás de él y los elementos emergentes otros abiertos por debajo de él.

Para usar ThemeShadow con elementos emergentes, usa el `Shadow` propiedad para aplicar un ThemeShadow a un elemento XAML. A continuación, elevar el elemento de otros elementos detrás de ella, por ejemplo mediante el componente z de la `Translation` propiedad.
Para la mayoría de la interfaz de usuario de elemento emergente, la elevación predeterminada recomendada en relación con el contenido de la aplicación en segundo plano es 32 píxeles efectivos.

En este ejemplo se muestra un rectángulo en un elemento emergente con una sombra en el contenido de la aplicación en segundo plano y los otros elementos emergentes detrás de ella:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="White" Height="48" Width="96">
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

![sombra del ejemplo de código](images/elevation-shadow/smartshadow-example.svg)

### <a name="themeshadow-in-other-elements"></a>ThemeShadow en otros elementos

Para realizar la conversión de una sombra de un elemento XAML que no está en un elemento emergente, debes especificar explícitamente los demás elementos que pueden recibir la sombra en el `ThemeShadow.Receivers` colección.

Este ejemplo muestra dos botones que modelar sombras en una cuadrícula detrás de ellos:

```xaml
<Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Button x:Name="Button1" Content="Button 1" Shadow="{StaticResource SharedShadow}" Margin="10" />

    <Button x:Name="Button2" Content="Button 2" Shadow="{StaticResource SharedShadow}" Margin="120" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Button1.Translation += new Vector3(0, 0, 16);
Button2.Translation += new Vector3(0, 0, 32);
```

### <a name="performance-best-practices-for-themeshadow"></a>Procedimientos recomendados de rendimiento para ThemeShadow

1. Limitar el número de elementos personalizada de receptor al mínimo necesario. 

2. Si varios elementos de receptor están en el mismo elevación, a continuación, intenta combinar destinados a un elemento primario único en su lugar.

3. Si varios elementos transmitirá el mismo tipo de sombra a los mismos elementos receptor, a continuación, agrega la sombra como un recurso compartido y reutilizarlo.

## <a name="drop-shadow"></a>Sombra paralela

DropShadow no es automáticamente con capacidad de respuesta a su entorno y no usa fuentes de luz. Por ejemplo las implementaciones, consulta la [Clase de sombra](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>¿Qué sombra debería usar?

| Propiedad | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | RS5 | 14393 |
| **Capacidad de adaptación** | Sí | No |
| **Personalización** | No | Sí |
| **Fuente de luz** | Automática (global de manera predeterminada, pero se puede reemplazar por aplicación) | Ninguno |
| **Se admiten en entornos 3D** | Sí | No |

- Por lo general, te recomendamos que uses ThemeShadow, que se adapta automáticamente a su entorno.
- Si tienen más avanzadas de escenarios de sombras personalizadas, a continuación, usar DropShadow, lo que permite la personalización mayor.
- Para hacia atrás compatibilidad, usar DropShadow.
- Para dudas acerca del rendimiento, limitar el número de sombras o usar DropShadow.
- En HMDs en true 3D, utilice ThemeShadow. Dado que DropShadow dibuja en una posición especificada por el objeto visual es secundario, desde el lado, tendrá un aspecto como es que flote en el espacio. Por otro lado, ThemeShadow se representan por encima de los elementos visuales que se definen como receptores.

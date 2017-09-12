---
author: mijacobs
description: "Reveal es un nuevo modelo de interacción que ayuda a agregar más enfoque y deleite a tu aplicación."
title: Reveal
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
ms.openlocfilehash: d50e3f47faad5fff0ef461a4b5312127a0b9ec9c
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2017
---
# <a name="reveal"></a>Reveal

> [!IMPORTANT]
> En este artículo se describe una funcionalidad que no se ha publicado aún y que puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Reveal es un efecto de iluminación que te ayuda a dar profundidad y foco a los elementos interactivos de tu aplicación.

> **API importantes**: [clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [clase RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [clase RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [clase RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [clase VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

Para lograr esto, el comportamiento de Reveal muestra partes de los elementos alrededor del contenido principal (o focal) cuando el mouse o el rectángulo de foco se encuentran sobre las áreas deseadas.

![Elemento visual de Reveal](images/Nav_Reveal_Animation.gif)

Al exponer los bordes ocultos alrededor de los objetos, Reveal permite a los usuarios comprender mejor el espacio con el que interactúan y los ayuda a comprender las acciones disponibles. Esto es especialmente importante en los controles de lista y en los controles con placas traseras.

## <a name="reveal-and-the-fluent-design-system"></a>Reveal y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Reveal es un componente de Fluent Design System que agrega luz a tu aplicación. 

## <a name="what-is-reveal"></a>¿Qué es Reveal?

Reveal cuenta con dos componentes visuales principales: el comportamiento **Hover Reveal** y el comportamiento **Border Reveal**.

![Capas de Reveal](images/RevealLayers.png)

Hover Reveal se vincula directamente con el contenido por el que se desplaza el puntero (a través de puntero o la entrada del foco), y aplica una forma de halo suave alrededor del elemento activado o enfocado, lo que te permite saber que puedes interactuar con él.

Border Reveal se aplica al elemento enfocado y a los elementos cercanos. Esto te muestra que esos objetos cercanos pueden realizar acciones similares al que está enfocado actualmente.

El desglose de la receta de Reveal es:

- Border Reveal estará encima de todo el contenido, salvo en los bordes designados.
- El texto y el contenido se mostrarán directamente debajo de Border Reveal.
- Hover Reveal estará debajo del contenido y el texto.
- La placa trasera (que se activa y permite Hover Reveal).
- El fondo (el fondo de control).

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->

## <a name="how-to-use-it"></a>Cómo se usa

Reveal expone la geometría alrededor del cursor cuando lo necesitas y se atenúa sin problemas una vez que seguiste de largo.

Reveal se usa mejor cuando se habilita en el contenido principal de la aplicación que tiene límites implícitos y placas traseras. Además, Reveal debe usarse en controles de tipo lista o colección.

## <a name="controls-that-automatically-use-reveal"></a>Controles que usan Reveal automáticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)

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
| ComboBoxItem | ComboxBoxItemRevealStyle |

Para aplicar estos estilos, simplemente actualiza la propiedad Style de este modo:

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

> [!NOTE]
> Estos estilos no están disponibles automáticamente en la versión 16190 del SDK. Para obtenerlos, deberás copiarlos manualmente desde generic.xaml en tu aplicación. Generic.xaml normalmente se ubica en C:\Archivos de programa (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\10.0.16190.0\Generic. Esto se resolverá en una compilación más adelante. 

## <a name="enabling-reveal-on-custom-controls"></a>Habilitar Reveal en controles personalizados

Para habilitar Reveal en controles personalizados o controles vueltos a basar, puedes entrar en el estilo de ese control en los estados visuales de la plantilla de ese control y especificar Reveal en la cuadrícula raíz:

```xaml
<VisualState x:Name="PointerOver">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="PointerOver" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPointerOver}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}"/>
  </VisualState.Setters>
</VisualState>
```

Para aprovechar el efecto completo de Reveal, tendrás que agregar la misma clase RevealBrushHelper también a PressedState:

```xaml
<VisualState x:Name="Pressed">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="Pressed" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPressed}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}"/>
  </VisualState.Setters>
</VisualState>
```


Usar de nuestra recurso del sistema Reveal significa que nos encargaremos de todos los cambios de tema por ti.

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer
- Usa Reveal en los elementos en los que el usuario puede realizar acciones; Reveal no debe aplicarse en el contenido estático.
- Muestra Reveal en listas o colecciones de datos.
- Aplica Reveal a contenido incluido claramente con placas traseras.
- No muestres Reveal en fondos, texto o imágenes estáticos con los que no puedas interactuar.
- No apliques Reveal a contenido flotante no relacionado.
- No uses Reveal en situaciones aisladas y de uso único, como cuadros de diálogo o notificaciones de contenido.
- No uses Reveal en las decisiones de seguridad, ya que puede distraer la atención del mensaje que quieres darle al usuario.

## <a name="related-articles"></a>Artículos relacionados

- [Clase RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [**Acrylic**](acrylic.md)
- [**Efectos de composición**](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)

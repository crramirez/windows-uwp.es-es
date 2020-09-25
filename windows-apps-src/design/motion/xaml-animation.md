---
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: Introducción a las animaciones
description: Usa las animaciones de la Biblioteca de animaciones de Windows Runtime para integrar el aspecto de Windows en la aplicación.
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 40752da17591c9eca16f46fbd244d4507a38b1fb
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220008"
---
# <a name="animations-in-xaml"></a>Animaciones en XAML

Las animaciones pueden mejorar la aplicación agregando movimiento e interactividad. El uso de animaciones de la biblioteca de animaciones de Windows Runtime te permitirá integrar la apariencia de Windows en tu aplicación. En este tema, se ofrece un resumen de las animaciones y algunos ejemplos de escenarios comunes en los que se usa cada una.

> [!TIP]
> Los controles de Windows Runtime para XAML incluyen determinados tipos de animaciones como comportamientos integrados que proceden de una biblioteca de animaciones. Al usar estos controles en tu aplicación, puedes conseguir la apariencia animada sin tener que programarla tú mismo.

Las animaciones de la biblioteca de animaciones de Windows Runtime proporcionan las siguientes ventajas:

-   Movimientos que se ajustan a las [Directrices para animaciones](./index.md)
-   Transiciones rápidas y fluidas entre los estados de interfaz de usuario que informan sin distraer al usuario.
-   Comportamiento visual que muestra al usuario las transiciones dentro de una aplicación

Por ejemplo, cuando el usuario agrega un elemento a una lista, en lugar de aparecer en la lista de forma instantánea, este se coloca en su lugar mediante una animación. Los otros elementos de la lista se animan a su nueva posición rápidamente para hacer lugar para el elemento nuevo. Este comportamiento de transición hace que la interacción de control sea más aparente para el usuario.

Windows 10, versión 1607, presenta una nueva API [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) para implementar animaciones en las que un elemento parece animarse entre distintas vistas durante la navegación. Esta API posee un patrón de uso distinto al de otras API de la biblioteca de animaciones. El uso de **ConnectedAnimationService** se trata en la [página de referencia](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice).

La biblioteca de animaciones no ofrece animaciones para todos los escenarios posibles. Habrá casos en que querrás crear una animación personalizada en XAML. Para obtener más información, consulta [Animaciones con guion gráfico](storyboarded-animations.md).

Además, para determinados escenarios avanzados, como la animación de un elemento basada en la posición de desplazamiento de un elemento ScrollViewer, los desarrolladores pueden optar por usar la interoperación de Capa visual para implementar animaciones personalizadas. Consulta [Capa visual](../../composition/visual-layer.md) para obtener más información.

## <a name="types-of-animations"></a>Tipos de animaciones

El objetivo último del sistema de animaciones de Windows Runtime y de la biblioteca de animaciones es permitir que los controles y otras partes de la interfaz de usuario tengan un comportamiento animado. Existen varios tipos de animaciones diferentes.

-   Las *transiciones de tema* se aplican automáticamente cuando cambian ciertas condiciones en la interfaz de usuario, incluidos los controles o elementos de los tipos de interfaz de usuario de XAML de Windows Runtime predeterminados. Se conocen como *transiciones de tema* porque las animaciones admiten la apariencia de Windows y porque definen el comportamiento de todas las aplicaciones en escenarios de interfaz de usuario determinados cuando cambian de un modo de interacción a otro. Las transiciones de tema forman parte de la biblioteca de animaciones.
-   Las *animaciones de tema* son animaciones de una o más propiedades de tipos de interfaz de usuario de XAML de Windows Runtime predefinidos. Las animaciones de tema no son iguales a las transiciones de tema, ya que las primeras están destinadas a un elemento específico y existen en estados visuales específicos dentro de un control, mientras que las últimas se asignan a propiedades del control que existen fuera de los estados visuales e influyen en las transiciones entre esos estados. Muchos de los controles XAML de Windows Runtime incluyen animaciones de tema dentro de guiones gráficos que forman parte de su plantilla de control y que se desencadenan por medio de estados visuales. Siempre y cuando no modifiques las plantillas, tendrás animaciones de tema integradas disponibles para los controles en la interfaz de usuario. Pero si las reemplazas, eliminarás al mismo tiempo las animaciones de tema de los controles integrados. Para recuperarlas, debes definir un guion gráfico que incluya estas animaciones dentro del conjunto de estados visuales del control. También puedes ejecutar las animaciones de tema desde guiones gráficos que no estén dentro de los estados visuales e iniciarlas con el método [**Begin**](/uwp/api/windows.ui.xaml.media.animation.storyboard.begin), pero esto no es tan común. Las animaciones de tema forman parte de la biblioteca de animaciones.
-   Las *transiciones visuales* se aplican cuando un control cambia de uno de sus estados visuales definidos a otro estado. Son animaciones personalizadas escritas por ti y que suelen estar relacionadas con la plantilla personalizada que escribes para un control y las definiciones de estado visual incluidas en dicha plantilla. La animación solo se ejecuta en el espacio de tiempo entre ambos estados, que suele ser muy breve, de unos pocos segundos a lo sumo. Para más información, consulta la sección ["VisualTransition" de Animaciones con guion gráfico para estados visuales](/previous-versions/windows/apps/jj819808(v=win.10)).
-   *Las animaciones con guion gráfico* animan el valor de una propiedad de dependencia de Windows Runtime a lo largo de un período de tiempo. Los guiones gráficos pueden definirse como parte de una transición visual o bien puede desencadenarlos la aplicación en tiempo de ejecución. Para obtener más información, consulta [Animaciones con guion gráfico](storyboarded-animations.md). Para obtener más información sobre las propiedades de dependencia y dónde se encuentran, consulta [Introducción a las propiedades de dependencia](../../xaml-platform/dependency-properties-overview.md).
-   Las *animaciones conectadas* que proporciona la nueva API [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) permiten a los desarrolladores crear fácilmente un efecto en el que un elemento parece animarse entre distintas vistas durante la navegación. Esta API está disponible a partir de Windows 10, versión 1607. Para obtener más información, consulta [**ConnectedAnimationService**](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice).

## <a name="animations-available-in-the-library"></a>Animaciones disponibles en la biblioteca

En la biblioteca de animaciones podrás encontrar las siguientes animaciones. Haz clic en el nombre de una animación para conocer los escenarios principales de uso y cómo definirla, y para ver un ejemplo.

-   [Transición de página](#page-transition): Anima las transiciones de página en un [**marco**](/uwp/api/Windows.UI.Xaml.Controls.Frame).
-   [Transición de contenido y de entrada](#content-transition-and-entrance-transition): anima una parte o una sección del contenido que aparece o desaparece de la vista.
-   [Fundido de entrada, fundido de salida y encadenado](#fade-in-out-and-crossfade): muestra elementos o controles transitorios, o actualiza un área de contenido.
-   [Puntero arriba y abajo](#pointer-up-down): da comentarios visuales de una pulsación o clic en un icono.
-   [Cambiar la posición](#reposition): mueve un elemento a una nueva posición.
-   [Mostrar u ocultar emergente](#show-hide-popup): Muestra la interfaz de usuario contextual en el frente.
-   [Mostrar u ocultar interfaces de usuario de borde](#show-hide-edge-ui): Desliza interfaces de usuario que aparecen o desaparecen desde el borde, incluidas interfaces grandes, como un panel.
-   [Cambios de elementos de lista](#list-item-changes): agrega o elimina un elemento en una lista, o reordena sus elementos.
-   [Arrastrar y soltar](#drag-drop): muestra comentarios visuales durante una operación de arrastrar y colocar.

### <a name="page-transition"></a>Transición de página

Usa transiciones de página para animar la navegación dentro de una aplicación. Dado que casi todas las aplicaciones usan algún tipo de navegación, las animaciones de transición de página son el tipo más común de animación de tema que se usa en las aplicaciones. Consulta [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) para obtener más información acerca de las API de transición de página.



### <a name="content-transition-and-entrance-transition"></a>Transición de contenido y transición de entrada

Usa las animaciones de transición de contenido ([**ContentThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition)) para hacer que una parte o una sección de contenido aparezca o desaparezca de la vista actual. Por ejemplo, las animaciones de transición de contenido muestran contenido que no estaba listo cuando se cargó la página por primera vez o cuando cambia el contenido de una sección de una página.

[**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) representa un movimiento que se puede aplicar al contenido cuando se carga por primera vez una página o una sección grande de la interfaz de usuario. De este modo, cuando el contenido se muestra por primera vez, el usuario podrá ver información diferente de la que se ve al cambiar el contenido. [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) es equivalente a un [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) con los parámetros predeterminados, pero puede usarse fuera de un [**marco**](/uwp/api/Windows.UI.Xaml.Controls.Frame).
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>Fundido de entrada, fundido de salida y encadenado

Usa animaciones de fundido de entrada y de salida para mostrar u ocultar interfaces de usuario o controles transitorios. En XAML, estas se representan como [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) y [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation). Por ejemplo, en una barra de la aplicación pueden aparecer controles nuevos debido a la interacción del usuario. Otro ejemplo es una barra de deslizamiento o un indicador de movimiento panorámico transitorios con fundido de salida, cuando no se detecta ninguna entrada del usuario durante cierta cantidad de tiempo. Las aplicaciones también deben usar el fundido en una animación cuando realizan una transición de un elemento marcador de posición al elemento final, a medida que el contenido se carga dinámicamente.

Usa una animación de encadenado para suavizar la transición cuando el estado de un elemento cambia, por ejemplo, cuando la aplicación actualiza el contenido actual de una vista. En la biblioteca de animaciones de XAML no se proporciona una animación de encadenado dedicada (no hay un equivalente para [**crossFade**](/previous-versions/windows/apps/br212661(v=win.10))), pero es posible obtener el mismo resultado si se usan los objetos [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) y [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) con tiempos superpuestos.

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>Puntero arriba y abajo

Usa las animaciones [**PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) y [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) para que el usuario sepa que una pulsación o un clic en un icono se realizó correctamente. Por ejemplo, cuando el usuario hace clic o presiona un icono, se reproduce la animación de puntero abajo. Una vez finalizado el clic o la pulsación, se reproduce la animación de puntero arriba.

### <a name="reposition"></a>Cambiar la posición

Usa las animaciones de reposición ([**RepositionThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) o [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition)) para mover un elemento a otra posición. Por ejemplo, podrías usarlas para mover los encabezados en un control de elementos.

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>Mostrar u ocultar emergente

Usa los objetos [**PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) y [**PopOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) para mostrar y ocultar un objeto [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) o elementos de interfaz de usuario contextuales similares en el frente de la vista actual. [**PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) es una transición de tema que resulta útil si se quiere descartar un elemento emergente por un cambio de foco.

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>Mostrar u ocultar interfaces de usuario de borde

Usa la animación [**EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition) para deslizar pequeñas interfaces de usuario de modo que aparezcan y desaparezcan desde el borde. Por ejemplo, podrías usarla para mostrar una barra de la aplicación personalizada en la parte superior o inferior de la pantalla o una superficie de interfaz de usuario para errores y advertencias en la parte superior de la pantalla.

Usa la animación [**PaneThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) para mostrar u ocultar un panel. Se usa para interfaces de usuario de borde de gran tamaño, como un teclado personalizado o un panel de tareas.

### <a name="list-item-changes"></a>Cambios de elementos de lista

Usa la animación [**AddDeleteThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) para agregar comportamiento animado al agregar o eliminar elementos en una lista existente. Para agregarlos, la transición primero cambiará la posición de los elementos existentes en la lista para dar cabida a los elementos nuevos y luego los agregará. Para eliminarlos, la transición los quita de la lista y, si fuera necesario, cambia la posición del resto de los elementos.

También hay una animación [**ReorderThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) independiente que se aplica si cambia la posición de un elemento en una lista. Este comportamiento animado es diferente del de las animaciones de eliminación y adición que eliminan un elemento y luego lo agregan en otro lugar.

Ten en cuenta que estas animaciones se incluyen en las plantillas [**ListView**](/uwp/api/windows.ui.xaml.controls.listview) y [**GridView**](/uwp/api/windows.ui.xaml.controls.gridview) predeterminadas para que no tengas que agregar manualmente estas animaciones si ya estás usando estos controles.

<span id="drag-drop"/>

### <a name="dragdrop"></a>Arrastrar y colocar

Usa las animaciones de arrastrar ([**DragItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation), [**DragOverThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation)) y la animación de colocar ([**DropTargetItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation)) para ofrecer comentarios visuales cuando el usuario arrastra o coloca un elemento.

Cuando están activas, las animaciones muestran al usuario que la lista se puede reordenar alrededor del elemento que se colocará. Esto es útil para los usuarios que saben adónde irá el elemento en la lista si se lo coloca en la ubicación actual. Estas animaciones muestran comentarios visuales para indicar que un elemento que se está arrastrando puede colocarse entre otros dos elementos de la lista que se moverán para darle lugar.

## <a name="using-animations-with-custom-controls"></a>Usar animaciones con controles personalizados

En la tabla siguiente se ofrece un resumen de las recomendaciones sobre qué animación debes usar cuando crees una versión personalizada de estos controles de Windows Runtime:

| Tipo de interfaz de usuario | Animación recomendada |
|---------|-----------------------|
| Cuadro de diálogo | [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) y [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| Flyout | [**PopInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) y [**PopOutThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Información sobre herramientas | [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) y [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| Menú contextual | [**PopInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) y [**PopOutThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Barra de comandos | [**EdgeUIThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| Panel de tareas o panel en el borde | [**PaneThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| Contenido de cualquier contenedor de interfaz de usuario | [**ContentThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| Para controles o en casos en que no se apliquen otras animaciones | [**FadeInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation) y [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |

 

## <a name="transition-animation-examples"></a>Ejemplos de animación de transición

En teoría, tu aplicación usa animaciones para mejorar la interfaz de usuario o para hacerla más atractiva sin molestar a los usuarios. Una forma de hacer esto es aplicar transiciones animadas a la interfaz de usuario para que, cuando algo entre en la pantalla o salga de ella, o se produzca algún otro cambio, la animación capte la atención del usuario para informarle del cambio. Por ejemplo, los botones pueden mostrar un fundido de entrada o de salida en vez de simplemente aparecer o desaparecer. Hemos creado diversas API que se pueden usar para crear transiciones de animación recomendadas o habituales que sean coherentes. En el siguiente ejemplo se muestra cómo aplicar una animación a un botón de forma que este se deslice rápidamente en la pantalla.

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

En este código, agregamos el objeto [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) a la colección de transición del botón. Ahora, cuando el botón se presente por primera vez, se deslizará rápidamente en la pantalla en lugar de aparecer simplemente. Puedes establecer unas cuantas propiedades en el objeto de animación para ajustar la distancia y la dirección en la que se desliza, pero se trata de una sencilla API para un escenario específico: realizar una entrada atractiva.

También puedes definir temas de animación de transición en los recursos de estilo de tu aplicación, lo que te permitirá aplicar el efecto de forma uniforme. Este ejemplo es equivalente al anterior, con la única diferencia de que se aplica con un objeto [**Style**](/uwp/api/Windows.UI.Xaml.Style):

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

En los ejemplos anteriores, se aplica una transición de tema a un control individual, pero las transiciones de tema muestran efectos aún más interesantes cuando se aplican a un contenedor de objetos. Cuando se hace esto, todos los objetos secundarios del contenedor forman parte de la transición. En el ejemplo siguiente, se aplica un objeto [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) a un objeto [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) de rectángulos.

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

Los rectángulos secundarios de la transición del objeto [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) se presentan uno detrás de otro de una forma agradable, en lugar de mostrarse todos a la vez como sucedería si aplicaras esta animación a los rectángulos de forma individual.

Aquí tienes una demostración de esta animación:

![Animación que muestra la transacción de un rectángulo secundario en la vista](./images/animation-child-rectangles.gif)

Los objetos secundarios incluidos en un contenedor también se pueden redistribuir cuando uno o varios de esos objetos cambian de posición. En el ejemplo siguiente, aplicamos un objeto [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) a una cuadrícula de rectángulos. Cuando se quita uno de los rectángulos, el resto se redistribuye a su nueva posición.

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

Puedes aplicar varias animaciones de transición a un solo objeto o contenedor de objetos. Por ejemplo, si quieres que la lista de rectángulos muestre una animación de entrada y otra cuando cambien de posición, puedes aplicar los objetos [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) y [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) de la forma siguiente:

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

Hay varios efectos de transición para crear animaciones en los elementos de tu interfaz de usuario a medida que se agregan, quitan, cambian de orden, etc. Todos los nombres de estas API contienen el objeto "ThemeTransition":

| API | Descripción |
|-----|-------------|
| [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) | Proporciona una animación de personalidad de Windows para la navegación de página en un [**marco**](/uwp/api/Windows.UI.Xaml.Controls.Frame). |
| [**AddDeleteThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) | Ofrece el comportamiento de transición animada cuando los controles agregan o eliminan objetos secundarios o contenido. El control suele ser un contenedor de elementos. |
| [**ContentThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition) | Proporciona el comportamiento de transición animada correspondiente a cuando cambia el contenido de un control. Puedes aplicar esta animación junto con el objeto [**AddDeleteThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition). |
| [**EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition) | Proporciona el comportamiento de transición animada de una interfaz de usuario de borde pequeña. |
| [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) | Ofrece el comportamiento de transición animada cuando los controles aparecen por primera vez. |
| [**PaneThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) | Proporciona el comportamiento de transición animada de una interfaz de usuario de borde de panel (grande). |
| [**PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) | Proporciona el comportamiento de transición animada que se aplica a los componentes emergentes de controles (por ejemplo, interfaces de usuario de tipo información sobre herramientas en un objeto) cuando aparecen. |
| [**ReorderThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) | Proporciona el comportamiento de transición animada cuando los elementos de los controles de vista de lista cambian de orden. Esto suele ocurrir como resultado de una operación de arrastrar y soltar. Los temas y controles diferentes pueden tener características distintas en las animaciones. |
| [**RepositionThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) | Proporciona el comportamiento de transición animada cuando los controles cambian de posición. |

 

## <a name="theme-animation-examples"></a>Ejemplos de animación de tema

Las animaciones de transición son sencillas de aplicar. Pero puede que desees tener un poco más de control sobre la sincronización y el orden de los efectos de tu animación. Puedes usar las animaciones de tema para permitir un mayor control mientras sigues usando un tema coherente en la forma en que se comporta tu animación. Además, las animaciones de tema requieren menos marcado que las personalizadas. En este ejemplo, usamos el objeto [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) para que un rectángulo desaparezca mediante un fundido de salida.

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

Al contrario que las animaciones de transición, una animación de tema no cuenta con un desencadenador integrado (la transición) que la ejecuta automáticamente. Debes usar un objeto [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) para incluir una animación de tema al definirla en XAML. También puedes cambiar el comportamiento predeterminado de la animación. Por ejemplo, puedes ralentizar el fundido de salida aumentando el valor de tiempo del objeto [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) en el objeto [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation).

**Nota:**    Con el fin de mostrar técnicas de animación básicas, usamos el código de la aplicación para iniciar la animación llamando a los métodos de [**guion gráfico**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard). Puedes controlar cómo se ejecutan las animaciones de **Storyboard** mediante los métodos [**Begin**](/uwp/api/windows.ui.xaml.media.animation.storyboard.begin), [**Stop**](/uwp/api/windows.ui.xaml.media.animation.storyboard.stop), [**Pause**](/uwp/api/windows.ui.xaml.media.animation.storyboard.pause) y [**Resume**](/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) **Storyboard**. Pero esa no es la manera más común de incluir animaciones de la biblioteca en las aplicaciones. En cambio, las bibliotecas de animaciones se suelen integrar en los estilos y las plantillas de XAML que se aplican a los controles o elementos. Comprender el uso de plantillas y estados visuales es un poco más complicado. Pero sí explicamos cómo usar las animaciones de la biblioteca en los estados visuales como parte del tema sobre las [animaciones con guion gráfico para estados visuales](/previous-versions/windows/apps/jj819808(v=win.10)).

 

Puedes aplicar otras animaciones de tema a los elementos de la interfaz de usuario para crear efectos de animación. Todos los nombres de estas API contienen el objeto "ThemeAnimation":

| API | Descripción |
|-----|-------------|
| [**DragItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation) | Representa la animación preconfigurada que se aplica a los elementos que se arrastran. |
| [**DragOverThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation) | Representa la animación preconfigurada que se aplica a los elementos que se encuentran debajo de los que se arrastran. |
| [**DropTargetItemThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation) | La animación preconfigurada que se aplica a elementos potenciales de destino para colocar. |
| [**FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) | La animación de opacidad preconfigurada que se aplica a los controles cuando aparecen por primera vez. |
| [**FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) | La animación de opacidad preconfigurada que se aplica a los controles cuando se quitan de la interfaz de usuario o se ocultan. |
| [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) | La animación preconfigurada para la acción de usuario que pulsa o hace clic en un elemento. |
| [**PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) | La animación preconfigurada para la acción de usuario que se ejecuta después de que el usuario pulsa un elemento y se libera la acción. |
| [**PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) | La animación preconfigurada que se aplica a componentes emergentes de los controles según aparecen. Esta animación combina opacidad y traducción. |
| [**PopOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) | La animación preconfigurada que se aplica a componentes emergentes de los controles a medida que se cierran o se quitan. Esta animación combina opacidad y traducción. |
| [**RepositionThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) | La animación preconfigurada de un objeto a medida que cambia de posición. |
| [**SplitCloseThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplitCloseThemeAnimation) | La animación preconfigurada que oculta una interfaz de usuario de destino con una animación de estilo [**ComboBox**](/uwp/api/windows.ui.xaml.controls.combobox) que se abre y cierra. |
| [**SplitOpenThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplitOpenThemeAnimation) | La animación preconfigurada que muestra una interfaz de usuario de destino con una animación de estilo [**ComboBox**](/uwp/api/windows.ui.xaml.controls.combobox) que se abre y cierra. |
| [**DrillInThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation) | Representa una animación preconfigurada que se ejecuta cuando un usuario navega hacia adelante en una jerarquía lógica, como de una página maestra a una página de detalles. |
| [**DrillOutThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.drilloutthemeanimation) | Representa una animación preconfigurada que se ejecuta cuando un usuario navega hacia atrás en una jerarquía lógica, como de una página de detalles a una página maestra. |

 

## <a name="create-your-own-animations"></a>Crear tus propias animaciones

Cuando las animaciones de tema no sean suficientes para cubrir tus necesidades, puedes crear las tuyas propias. Puedes animar objetos al animar uno o más valores de sus propiedades. Por ejemplo, puedes animar el ancho de un rectángulo, el ángulo de un objeto [**RotateTransform**](/uwp/api/Windows.UI.Xaml.Media.RotateTransform) o el valor de color de un botón. Este tipo de animación personalizada se denomina animación con guion gráfico para distinguirla de las animaciones de la biblioteca que Windows Runtime ya proporciona como un tipo de animación preconfigurado. En las animaciones con guion gráfico, usas una animación que puede cambiar los valores de un tipo en particular (por ejemplo, [**DoubleAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) para animar un **Doble**) y la colocas dentro de un objeto [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) para controlarla.

Para que pueda ser animada, la propiedad que te interesa debe ser una *propiedad de dependencia*. Si quieres obtener más información sobre las propiedades de dependencia, consulta [Introducción a las propiedades de dependencia](../../xaml-platform/dependency-properties-overview.md). Para más información sobre la creación de animaciones de guion gráfico personalizadas, lo que incluye cómo orientarlas y controlarlas, consulta [Animaciones con guion gráfico](storyboarded-animations.md).

El área más grande de la definición de la interfaz de usuario de la aplicación en el XAML, donde definirás animaciones con guion gráfico personalizadas, es al definir los estados visuales de los controles en el XAML. Tendrás que hacer esto al crear una nueva clase de control o al crear nuevas plantillas de un control existente que tiene estados visuales en su plantilla de control. Para obtener más información, vea [animaciones con guion gráfico para Estados visuales](/previous-versions/windows/apps/jj819808(v=win.10)).

 

 

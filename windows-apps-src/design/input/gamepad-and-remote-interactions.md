---
Description: Optimice la aplicación para la entrada de la consola Xbox y el control remoto.
title: Interacciones con controlador para juegos y control remoto
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 440f758e5db8bd77d3f26290eb59d7684e5f87a3
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210031"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interacciones con controlador para juegos y control remoto

![Imagen del teclado y el controlador para juegos](images/keyboard/keyboard-gamepad.jpg)

***Muchas experiencias de interacción se comparten entre el controlador de juegos, el control remoto y el teclado***

Cree experiencias de interacción en las aplicaciones Plataforma universal de Windows (UWP) que garantizan que la aplicación es utilizable y accesible a través de los tipos de entrada tradicionales de equipos, portátiles y tabletas (mouse, teclado, toque, etc.), así como los tipos de entrada típicos de la experiencia de TV y *10 pies* , como el controlador de juegos y el control remoto.

Consulte [diseño de Xbox y TV](../devices/designing-for-tv.md) para obtener instrucciones generales de diseño sobre aplicaciones UWP en la experiencia de *10 pies* .

## <a name="overview"></a>Introducción

En este tema, se explica lo que debe tener en cuenta en el diseño de interacción (o lo que no es necesario, si la plataforma lo hace después), y se proporcionan instrucciones, recomendaciones y sugerencias para la creación de aplicaciones para UWP que resulten divertidas de usar independientemente de dispositivo, tipo de entrada o capacidades y preferencias del usuario.

En la parte inferior, la aplicación debe ser tan intuitiva y fácil de usar en el entorno de *2 pies* , ya que se encuentra en el entorno de 3 *metros* (y viceversa). Admita los dispositivos preferidos del usuario, haga que el foco de la interfaz de usuario sea claro y unmistakable, organice el contenido para que la navegación sea coherente y predecible, y proporcione a los usuarios la ruta más corta posible a lo que desean hacer.

> [!NOTE]
> La mayoría de los fragmentos de código de este tema están en XAML/C#; Sin embargo, los principios y conceptos se aplican a todas las aplicaciones para UWP. Si estás desarrollando una aplicación para UWP en HTML/JavaScript para Xbox, echa un vistazo a la excelente biblioteca [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) en GitHub.


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>Optimizar para experiencias de 2 y 3 pies

Como mínimo, se recomienda probar las aplicaciones para asegurarse de que funcionan bien en escenarios de 2 y 10 pies y que todas las funciones son reconocibles y accesibles para el [controlador de juegos de Xbox y el control remoto](#gamepad-and-remote-control).

Estas son algunas otras maneras en que puede optimizar la aplicación para su uso en experiencias de 2 y 3 pies y con todos los dispositivos de entrada (cada uno de los vínculos a la sección correspondiente de este tema).

> [!NOTE]
> Dado que los controladores de juegos de Xbox y los controles remotos admiten muchos comportamientos y experiencias de teclado de UWP, estas recomendaciones son adecuadas para ambos tipos de entrada. Consulte [interacciones de teclado](keyboard-interactions.md) para obtener información más detallada del teclado.

| Característica        | Descripción           |
| -------------------------------------------------------------- |--------------------------------|
| [Navegación e interacción con el foco XY](#xy-focus-navigation-and-interaction) | La navegación con el **foco XY** permite al usuario navegar por la interfaz de usuario de la aplicación. Sin embargo, esto limita al usuario a navegar hacia arriba, abajo, izquierda y derecha. En esta sección se describen recomendaciones para lidiar con esta y otras consideraciones. |
| [Modo del mouse](#mouse-mode)|La navegación con el foco XY no es práctica, ni siquiera posible, para algunos tipos de aplicaciones, como las asignaciones o el dibujo y la representación de aplicaciones. En estos casos, el **modo de mouse** permite a los usuarios navegar libremente con un controlador para juegos o un control remoto, al igual que un mouse en un equipo.|
| [Foco visual](#focus-visual)  | El elemento visual de foco es un borde que resalta el elemento de la interfaz de usuario actualmente enfocado. Esto ayuda a los usuarios a identificar rápidamente la interfaz de usuario a la que navegan o con los que interactúan.  |
| [Interacción con el foco](#focus-engagement) | Focus Engagement requiere que el usuario presione el botón **a/seleccionar** en un controlador para juegos o control remoto cuando un elemento de la interfaz de usuario tiene el foco para interactuar con él. |
| [Botones de hardware](#hardware-buttons) | El controlador de juegos y el control remoto proporcionan botones y configuraciones muy diferentes. |

## <a name="gamepad-and-remote-control"></a>Controlador para juegos y control remoto

Al igual que el teclado y el mouse son para PC y la entrada táctil es para el teléfono y la tableta, el controlador para juegos y el control remoto son los dispositivos de entrada principales para la experiencia de 10 pies.
Esta sección presenta los botones de hardware y qué es lo que hacen.
En [Interacción y navegación con foco XY](#xy-focus-navigation-and-interaction) y [Modo de mouse](#mouse-mode), aprenderás a optimizar tu aplicación al usar estos dispositivos de entrada.

La calidad del controlador para juegos y el comportamiento del control remoto que obtienes a priori depende de qué tanto soporte le dé tu aplicación al teclado. Una buena forma de asegurarte de que tu aplicación funcionará bien con el controlador para juegos/control remoto es asegurarte de que funciona bien con el teclado del equipo y, a continuación, usar el controlador para juegos/control remoto para buscar puntos débiles en tu interfaz de usuario.

### <a name="hardware-buttons"></a>Botones de hardware

En este documento, se hará referencia a los botones mediante los nombres asignados en el siguiente diagrama.

![Diagrama de botones del controlador para juegos y control remoto](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Como puedes ver en el diagrama, hay algunos botones que son compatibles con el controlador para juegos pero que no son compatibles con el control remoto y viceversa. Si bien puedes usar botones que solo se admiten en un dispositivo de entrada para agilizar la navegación por la interfaz de usuario, ten en cuenta que usarlos para las interacciones críticas puede crear una situación en la que el usuario no pueda interactuar con determinadas partes de la interfaz de usuario.

La siguiente tabla enumera todos los botones de hardware compatibles con las aplicaciones para UWP y cuál dispositivo de entrada es compatible con ellos.

| Botón                    | Controlador para juegos   | Control remoto    |
|---------------------------|-----------|-------------------|
| A/Botón de selección           | Sí       | Sí               |
| Botón B/Atrás             | Sí       | Sí               |
| Pad direccional (pad-D)   | Sí       | Sí               |
| Botón de menú               | Sí       | Sí               |
| Botón de vista               | Sí       | Sí               |
| Botones X e Y           | Sí       | No                |
| Palanca izquierda                | Sí       | No                |
| Palanca hacia la derecha               | Sí       | No                |
| Desencadenadores izquierdo y derecho   | Sí       | No                |
| Reboteadores izquierdo y derecho    | Sí       | No                |
| Botón OneGuide           | No        | Sí               |
| Botón de Volumen             | No        | Sí               |
| Botón de canal            | No        | Sí               |
| Botones de control de medios     | No        | Sí               |
| Botón de silencio               | No        | Sí               |

### <a name="built-in-button-support"></a>Soporte para botones incorporado

La UWP asigna automáticamente el comportamiento de entrada del teclado existente al controlador para juegos y al control remoto. La siguiente tabla enumera estas asignaciones integradas.

| Teclado              | Controlador para juegos/control remoto                        |
|-----------------------|---------------------------------------|
| Teclas de dirección            | Pad-D (también la palanca izquierda en el controlador para juegos)    |
| Barra espaciadora              | A/Botón de selección                       |
| Entrar                 | A/Botón de selección                       |
| Escape                | Botón B/Atrás*                        |

\*cuando la aplicación no controla los eventos [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) ni [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) para el botón B, se desencadenará el evento [SystemNavigationManager.](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) allocated, que debe devolver la navegación hacia atrás dentro de la aplicación. Sin embargo, esto deberá ser implementado por ti, como se indica en el siguiente fragmento de código:

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> Si se usa el botón B para volver, no muestres un botón Atrás en la interfaz de usuario. Si vas a usar una [vista de navegación](../controls-and-patterns/navigationview.md), el botón Atrás se ocultará automáticamente. Para obtener más información sobre la navegación hacia atrás, consulta [Historial de navegación y navegación hacia atrás para las aplicaciones para UWP](../basics/navigation-history-and-backwards-navigation.md).

Las aplicaciones para UWP en Xbox One también permiten presionar el botón **Menú** para abrir los menús contextuales. Para obtener más información, consulta [CommandBar y ContextFlyout](#commandbar-and-contextflyout).

### <a name="accelerator-support"></a>Soporte para aceleradores

Los botones aceleradores son botones que pueden usarse para aumentar la velocidad de navegación a través de una interfaz de usuario. Sin embargo, estos botones pueden ser exclusivos para un determinado dispositivo de entrada, por lo tanto, ten en cuenta que no todos los usuarios podrán usar estas funciones. De hecho, el controlador para juegos es el único dispositivo de entrada que actualmente admite las funciones de aceleradores en aplicaciones para UWP en Xbox One.

La siguiente tabla enumera la compatibilidad con aceleradores integrada en la UWP, así como también la que puedes implementar por tu cuenta. Usa estos comportamientos en tu interfaz de usuario personalizada para brindar una experiencia de usuario coherente y agradable.

| Interacción   | Teclado/ratón   | Controlador para juegos      | Integrada para:  | Recomendada para: |
|---------------|------------|--------------|----------------|------------------|
| Retroceder/avanzar página  | Retroceder/avanzar página | Desencadenadores izquierdo/derecho | [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Vistas que admiten el desplazamiento vertical
| Página a la izquierda/derecha | Ninguno | Reboteadores izquierdo/derecho | [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Vistas que admiten el desplazamiento horizontal
| Acercar/alejar        | Ctrl +/- | Desencadenadores izquierdo/derecho | Ninguno | `ScrollViewer`, vistas que permiten acercar y alejar |
| Abrir o cerrar el panel de navegación | Ninguno | Ver | Ninguno | Paneles de navegación |
| Buscar | Ninguno | Botón Y | Ninguno | Combinación de teclas para la función de búsqueda principal de la aplicación |
| [Abrir el menú contextual](#commandbar-and-contextflyout) | Haz clic con el botón secundario | Botón de menú | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Menús contextuales |

## <a name="xy-focus-navigation-and-interaction"></a>Interacción y navegación con foco XY

Si tu aplicación admite una navegación del foco adecuada para el teclado, esto se trasladará bien al controlador para juegos y al control remoto.
La navegación con las teclas de dirección se asigna al **pad-D** (así como a la **palanca izquierda** en el controlador para juegos), y la interacción con los elementos de la interfaz de usuario se asigna a la tecla **Entrar/Seleccionar** (ver [Controlador para juegos y control remoto](#gamepad-and-remote-control)).

Muchos eventos y propiedades se usan tanto en el teclado como en el controlador de juegos. Ambos activan los eventos &mdash; y `KeyDown`, y solo navegarán a los controles que tengan las propiedades `KeyUp` y `IsTabStop="True"`. Para obtener directrices de diseño para teclado, consulta [Interacciones con el teclado](../input/keyboard-interactions.md).

Si la compatibilidad con el teclado se implementa correctamente, tu aplicación funcionará razonablemente bien; sin embargo, esto puede requerir algo más de trabajo para admitir todos los escenarios. Piensa en las necesidades específicas de tu aplicación para proporcionar la mejor experiencia posible para el usuario.

> [!IMPORTANT]
> El modo de mouse está habilitado de manera predeterminada para las aplicaciones para UWP que se ejecutan en Xbox One. Para deshabilitar el modo de mouse y habilitar la navegación con foco XY, establece `Application.RequiresPointerMode=WhenRequested`.

### <a name="debugging-focus-issues"></a>Depurar problemas de foco

El método [FocusManager.GetFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) te indicará qué elemento tiene el foco actualmente. Resulta útil para situaciones donde la ubicación del foco visual puede no ser obvia. Puedes registrar esta información en la ventana de salida de Visual Studio de este modo:

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

Existen tres razones comunes por las que navegación XY puede no funcionar del modo previsto:

* La propiedad [IsTabStop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) o [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) no está correctamente establecida.
* El control que obtiene el foco es realmente más grande de lo que piensas: la navegación XY observa el tamaño total del control (&mdash;ActualWidth[ y ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)ActualHeight[), no solo la parte del control que representa algo interesante.
* Un control activable se encuentra encima de otro: la navegación XY no admite controles que se superponen.

Si la navegación XY sigue sin funcionar de la manera esperada después de solucionar estos problemas, puedes apuntar manualmente al elemento que quieres que tenga el foco mediante el método que se describe en [Reemplazo de la navegación predeterminada](#overriding-the-default-navigation).

Si la navegación XY funciona según lo previsto, pero no se muestra ningún foco visual, la causa podría ser uno de los siguientes problemas:

* Has vuelto a basar el control en el modelo sin incluir un foco visual. Establece `UseSystemFocusVisuals="True"` o agrega un foco visual manualmente.
* Has movido el foco mediante una llamada a `Focus(FocusState.Pointer)`. El parámetro [FocusState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FocusState) controla lo que sucede con el foco visual. Por lo general, se debe establecer en `FocusState.Programmatic`, opción que mantiene el foco visual visible si antes era visible y oculto si estaba oculto anteriormente.

En el resto de esta sección se describen los desafíos de diseño comunes al usar la navegación XY y se ofrecen varias maneras de resolverlos.

### <a name="inaccessible-ui"></a>Interfaz de usuario inaccesible

Dado que la navegación con foco XY limita al usuario a moverse hacia arriba, abajo, izquierda y derecha, puedes terminar con escenarios donde algunas partes de la interfaz de usuario no son accesibles.
El siguiente diagrama ilustra un ejemplo de tipo de diseño de interfaz de usuario que no admite la navegación con foco XY.
Observa cómo el elemento en el medio es inaccesible mediante un controlador para juegos/control remoto porque se le dará prioridad a la navegación vertical y horizontal y el elemento central nunca tendrá la prioridad suficiente para obtener el foco.

![Elementos en cuatro esquinas con un elemento inaccesible en el medio](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Si por algún motivo no es posible reorganizar la interfaz de usuario, usa una de las técnicas que se describen en la siguiente sección para reemplazar el comportamiento predeterminado del foco.

### <a name="overriding-the-default-navigation"></a>Reemplazar la navegación predeterminada

Si bien la Plataforma universal de Windows intenta garantizar que la navegación del pad-D/palanca izquierda tenga sentido para el usuario, no puede garantizar un comportamiento optimizado para las intenciones de tu aplicación.
La mejor manera de garantizar una navegación optimizada para tu aplicación es probarla con un controlador para juegos y confirmar que el usuario pueda acceder a cada elemento de la interfaz de usuario de una manera que tenga sentido para escenarios de tu aplicación. En caso de que alguno de los escenarios de tu aplicación requiera un comportamiento que no se obtiene a través de la navegación con foco XY proporcionada, considera las recomendaciones en las siguientes secciones y/o reemplaza el comportamiento para colocar el foco en un elemento lógico.

El siguiente fragmento de código muestra cómo puedes invalidar el comportamiento de navegación con foco XY:

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

En este caso, cuando el foco esté en el botón `Home` y el usuario navegue a la izquierda, el enfoque se moverá al botón `MyBtnLeft`; si el usuario navega a la derecha, el enfoque se moverá hacia el botón `MyBtnRight`; y así sucesivamente.

Para evitar que el foco se vaya de un control en una dirección determinada, usa la propiedad `XYFocus*` para que apunte al mismo control:

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

Al usar estas propiedades de `XYFocus`, un control primario también puede forzar la navegación de sus elementos secundarios cuando el siguiente candidato de foco está fuera del árbol visual, a no ser que el elemento secundario que tiene el foco utilice la misma propiedad de `XYFocus`.

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

En la muestra anterior si el foco está en el `Button` dos y el usuario navega hacia la derecha, el mejor candidato de foco será el `Button` cuatro. Sin embargo, si el foco se desplaza al botón `Button` tres porque el control primario `UserControl` le fuerza a navegar en esa dirección, estará fuera del árbol visual.

### <a name="path-of-least-clicks"></a>El camino con menos clics

Intenta permitirle al usuario realizar las tareas más comunes en el menor número de clics. En el siguiente ejemplo, el [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) se coloca entre el botón **Reproducir** (que inicialmente obtiene el foco) y un elemento usado frecuentemente, para que se coloque un elemento innecesario entre tareas con prioridad.

![Los procedimientos de navegación recomendados proporcionan el camino con menos clics](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

En cambio, en el siguiente ejemplo, el [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) se coloca encima del botón **Reproducir**.
Simplemente reorganizar la interfaz de usuario para que no queden elementos innecesarios entre tareas con una prioridad mejorará en gran medida la facilidad de uso de tu aplicación.

![TextBlock se movió hacia arriba del botón Reproducir para que no quede entre tareas con prioridad](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar y ContextFlyout

Cuando se usa [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar), ten en cuenta el problema de desplazamiento por una lista, como se mencionó en [Problema: elementos de la interfaz de usuario ubicados después de una cuadrícula/lista de desplazamiento larga](#problem-ui-elements-located-after-long-scrolling-list-grid). La siguiente imagen muestra un diseño de interfaz de usuario con `CommandBar` en la parte inferior de la lista/cuadrícula. El usuario tendría que desplazarse hacia abajo de todo por la lista/cuadrícula para llegar a la `CommandBar`.

![CommandBar en la parte inferior de la lista/cuadrícula](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

¿Qué ocurre si coloca el `CommandBar` *encima* de la lista o cuadrícula? Si bien un usuario que se desplazó hacia abajo de la lista/cuadrícula tendría que desplazarse nuevamente hacia arriba para llegar a la `CommandBar`, se trata de una navegación ligeramente más corta que en la configuración anterior. Ten en cuenta que esto es suponiendo que el foco inicial de tu aplicación se sitúe junto a o encima de `CommandBar`; este enfoque no funcionará si el foco inicial se encuentra por debajo de la lista/cuadrícula. Si estos elementos de la `CommandBar` son elementos de acción global a los que no se accede muy a menudo (como un botón **Sincronizar**), puede ser aceptable que se ubiquen encima de la lista/cuadrícula.

Si bien no se pueden apilar los elementos de una `CommandBar` verticalmente, colocarlos contra la dirección del desplazamiento (por ejemplo, a la izquierda o derecha de una lista de desplazamiento vertical, o en la parte superior o inferior de una lista de desplazamiento horizontal) es otra opción que podrías considerar si funciona bien para el diseño de tu interfaz de usuario.

Si tu aplicación tiene una `CommandBar` cuyos elementos deben ser fácilmente accesibles para los usuarios, es aconsejable que consideres colocar estos elementos dentro de una propiedad [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) y los quites de la `CommandBar`. `ContextFlyout` es una propiedad de [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) y es el [menú contextual](../controls-and-patterns/dialogs-and-flyouts/index.md) asociado a ese elemento. En el equipo, al hacer clic con el botón derecho en un elemento con una propiedad `ContextFlyout`, aparecerá el menú contextual correspondiente. En Xbox One, esto ocurrirá al presionar el botón **Menú** mientras el foco está en dicho elemento.

### <a name="ui-layout-challenges"></a>Desafíos de diseño de la interfaz de usuario

Algunos diseños de interfaz de usuario son más desafiantes debido a la naturaleza de la navegación con foco XY y deben evaluarse caso por caso. Si bien no hay una única forma "correcta" y la solución que elijas depende de las necesidades específicas de tu aplicación, hay algunas técnicas que puedes emplear para brindar una gran experiencia en el televisor.

Para comprender mejor esto, echemos un vistazo a una aplicación imaginaria que muestra algunos de estos problemas y las técnicas para resolverlos.

> [!NOTE]
> Esta aplicación imaginaria pretende ilustrar los problemas de la interfaz de usuario y sus posibles soluciones, y no está pensada para mostrar la mejor experiencia de usuario para tu aplicación en particular.

La siguiente es una aplicación inmobiliaria imaginaria que muestra una lista de viviendas disponibles para la venta, un mapa, una descripción de la propiedad y demás información. Esta aplicación presenta tres desafíos que puedes superar mediante el uso de las siguientes técnicas:

- [Reorganización de la interfaz de usuario](#ui-rearrange)
- [Interacción con el foco](#engagement)
- [Modo del mouse](#mouse-mode)

![Aplicación inmobiliaria imaginaria](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problema: algunos elementos de la interfaz de usuario se encuentran después de una cuadrícula/lista de desplazamiento larga<a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

La [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) de las propiedades que se muestra en la siguiente imagen es una lista con desplazamiento muy larga. Si la [participación](#focus-engagement)*no* se requiere en la `ListView`, cuando el usuario navegue a la lista, el foco se colocará en el primer elemento en la lista. Para que el usuario llegue al botón **anterior** o **siguiente**, deberá pasar por todos los elementos de la lista. En casos como este en el que resulta difícil solicitarle al usuario que recorra toda la lista (es decir, cuando la lista no es lo suficientemente corta como para que esta experiencia sea aceptable) sería conveniente considerar otras opciones.

![Aplicación inmobiliaria: una lista de 50 elementos sencillos requiere 51 clics para llegar a los botones de abajo](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Soluciones

**Reorganización de la interfaz de usuario<a name="ui-rearrange"></a>**

A menos que tu foco inicial se coloque en la parte inferior de la página, los elementos de la interfaz de usuario colocados encima de una lista con desplazamiento larga son por lo general más accesibles que si se encuentran debajo.
Si este nuevo diseño funciona para los otros dispositivos, cambiar el diseño para todas las familias de dispositivos en lugar de hacer cambios especiales a la interfaz de usuario solo para Xbox One puede ser un enfoque menos costoso.
Además, colocar los elementos de la interfaz de usuario en contra de la dirección de desplazamiento (es decir, horizontalmente a una lista de desplazamiento vertical, o verticalmente a una lista de desplazamiento horizontal) mejorará aún más la accesibilidad.

![Aplicación inmobiliaria: botones situados encima de la lista con desplazamiento larga](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Interacción con el foco<a name="engagement"></a>**

Cuando se *requiere* la participación, todo la `ListView` se convierte en un objeto de foco único. El usuario podrá saltear el contenido de la lista para ir al siguiente elemento activable. Lee más acerca de los controles que admiten participación y cómo usarlos en [Participación del foco](#focus-engagement).

![Aplicación inmobiliaria: participación definida como requerida para que solo lleve 1 clic llegar a los botones Anterior/Siguiente](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problema: ScrollViewer sin elementos activables

Como la navegación con foco XY depende de la navegación a un elemento de interfaz de usuario activable cada vez, un [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) que no contiene elementos activables (como con solo texto, al igual que en este ejemplo) puede desencadenar un escenario en el que el usuario no puede ver todo el contenido del `ScrollViewer`.
Consulta las soluciones para este y otros escenarios relacionados en [Participación del foco](#focus-engagement).

![Aplicación inmobiliaria: ScrollViewer solo con texto](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problema: Interfaz de usuario de desplazamiento libre

Cuando tu aplicación requiera una interfaz de usuario de desplazamiento libre, como por ejemplo una superficie de dibujo o, en este ejemplo, un mapa, la navegación con foco XY simplemente no funciona.
En estos casos, puedes activar el [modo de mouse](#mouse-mode) para permitir que el usuario pueda navegar libremente dentro de un elemento de la interfaz de usuario.

![Elemento de mapa de la interfaz de usuario en modo de mouse](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Modo de mouse

Como se describe en [Interacción y navegación con foco XY](#xy-focus-navigation-and-interaction), en Xbox One el foco se mueve mediante un sistema de navegación XY, el cual permite al usuario cambiar el foco desde un control a otro desplazándose hacia arriba, abajo, izquierda y derecha.
Sin embargo, algunos controles, como [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) y [MapControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), requieren una interacción de mouse en la que los usuarios pueden mover libremente el puntero dentro de los límites del control.
También hay algunas aplicaciones en las que tiene sentido que el usuario pueda mover el puntero por toda la página, haciendo que los usuarios tengan con un controlador para juegos/control remoto una experiencia similar a lo que pueden encontrar en un equipo con un mouse.

Para estos escenarios, debes solicitar un puntero (modo de mouse) para toda la página, o para un control dentro de una página.
Por ejemplo, la aplicación podría tener una página que tenga un control `WebView` que use el modo de mouse solo mientras se esté dentro del control y la navegación con foco XY en cualquier otro lado.
Para solicitar un puntero, puedes especificar si lo quieres **cuando se activa una página o control** o **cuando la página tiene el foco**.

> [!NOTE]
> No se admiten solicitudes de un puntero cuando un control obtiene el foco.

Para las aplicaciones web hospedadas y XAML que se ejecutan en Xbox One, el modo de mouse está activado de manera predeterminada para toda la aplicación. Se recomienda encarecidamente desactivarlo y optimizar la aplicación para la navegación XY. Para ello, establece la propiedad `Application.RequiresPointerMode` en `WhenRequested` para que el modo de mouse solo se habilite cuando un control o una página realicen la llamada.

Para hacerlo en una aplicación XAML, usa el siguiente código en la clase `App`:

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Para obtener más información, incluido el código de muestra para HTML/JavaScript, consulta [Cómo deshabilitar el modo de mouse](../../xbox-apps/how-to-disable-mouse-mode.md).

El siguiente diagrama muestra las asignaciones de los botones del controlador para juegos/control remoto en el modo de mouse.

![Asignaciones de botones para el controlador para juegos/control remoto en el modo de mouse](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> El modo de mouse solo se admite en Xbox One con controlador para juegos/control remoto. En las otras familias de dispositivos y tipos de entrada se omite silenciosamente.

Usa la propiedad [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) en un control o una página para activar el modo de mouse en ella. Esta propiedad tiene tres valores posibles: `Never` (valor predeterminado), `WhenEngaged` y `WhenFocused`.

### <a name="activating-mouse-mode-on-a-control"></a>Activación del modo de mouse en un control

Cuando el usuario utiliza un control con `RequiresPointer="WhenEngaged"`, se activa el modo de mouse en el control hasta que el usuario deja de usarlo. El siguiente fragmento de código muestra una sencilla clase `MapControl` que activa el modo de mouse cuando se utiliza un control:

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> Si un control activa el modo de mouse cuando se usa, también debe solicitar la activación de `IsEngagementRequired="true"`; de lo contrario, nunca se activará el modo de mouse.

Cuando un control está en modo de mouse, sus controles anidados también estarán en modo de mouse. El modo solicitado de sus elementos secundarios se ignorará; no es posible que un elemento primario esté en modo de mouse y un elemento secundario no.

Además, el modo solicitado de un control solo se inspecciona cuando obtiene el foco, por lo que el modo no cambiará dinámicamente mientras tiene el foco.

### <a name="activating-mouse-mode-on-a-page"></a>Activación del modo de mouse en una página

Cuando una página tiene la propiedad `RequiresPointer="WhenFocused"`, el modo de mouse se activará para toda la página cuando obtenga el foco. El siguiente fragmento de código muestra cómo se asigna esta propiedad a una página:

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> El valor `WhenFocused` solo se admite en objetos [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page). Si se intenta establecer este valor en un control, se generará una excepción.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>Deshabilitar el modo de mouse para el contenido en pantalla completa

Por normal general, al mostrar vídeos u otro tipo de contenido en pantalla completa, se prefiere ocultar el cursor ya que puede distraer al usuario. Este escenario se produce cuando la aplicación usa el modo de mouse, pero quieres desactivarlo al mostrar el contenido en pantalla completa. Para ello, coloca el contenido de pantalla completa en su propia `Page` y sigue los siguientes pasos.

1. En el objeto de la `App` configúralo como `RequiresPointerMode="WhenRequested"`.
2. En el objeto de cada `Page`, *excepto* para la pantalla completa `Page`, configúralo como `RequiresPointer="WhenFocused"`.
3. Para la pantalla completa `Page`, configúralo como `RequiresPointer="Never"`.

De esta forma, el cursor nunca aparecerá al mostrar contenido en pantalla completa.

## <a name="focus-visual"></a>Foco visual

El foco visual es el borde alrededor del elemento de interfaz de usuario que actualmente tiene el foco. Esto ayuda a orientar al usuario para que pueda navegar fácilmente por tu interfaz de usuario sin perderse.

Con una actualización visual y numerosas opciones de personalización agregadas al foco visual, los desarrolladores pueden confiar en que un foco visual individual funcionará correctamente tanto en equipos como en Xbox One, así como en otros dispositivos con Windows 10 que admiten controlador para juegos/control remoto y/o teclado.

Aunque se puede usar el mismo foco visual en distintas plataformas, el contexto en el que el usuario la encuentra es ligeramente distinto en la experiencia de 10 pies. Debes suponer que el usuario no le está prestando toda su atención a la pantalla del televisor entera y, por lo tanto, es importante que el elemento enfocado actualmente sea claramente visible para el usuario en todo momento para evitar la frustración de buscar el foco visual.

También es importante tener en cuenta que el foco visual se muestra de manera predeterminada al usar un controlador para juegos o control remoto, pero *no* al usar un teclado. Por lo tanto, incluso si no lo implementas, aparecerá al ejecutar tu aplicación en Xbox One.

### <a name="initial-focus-visual-placement"></a>Colocación inicial del foco visual

Cuando se inicia una aplicación o al navegar a una página, coloca el foco en un elemento de la interfaz de usuario que sea lógico que sea el primer elemento sobre el que el usuario podría realizar una acción. Por ejemplo, una aplicación de fotos podría colocar el foco en el primer elemento de la galería, y una aplicación de música en la que se navegó a una vista detallada de una canción podría colocar el foco en el botón de reproducir para facilitar la reproducción de música.

Prueba poniendo el foco inicial en la región superior izquierda de tu aplicación (o en la parte superior derecha en caso de un flujo de derecha a izquierda). La mayoría de los usuarios suele centrarse primero en esa esquina porque es donde comienza por lo general el flujo de contenido de la aplicación.

### <a name="making-focus-clearly-visible"></a>Hacer que el foco sea claramente visible

Siempre debe haber un foco visual visible en la pantalla para que el usuario pueda continuar desde donde estaba sin necesidad de buscar el foco. Del mismo modo, debe haber un elemento activable en la pantalla en todo momento; por ejemplo, no uses ventanas emergentes con solo texto y ningún elemento activable.

Una excepción a esta regla serían las experiencias de pantalla completa, como ver vídeos o imágenes, en cuyo caso no sería adecuado mostrar el foco visual.

### <a name="reveal-focus"></a>Revelar foco

Reveal focus es un efecto de iluminación que anima el borde de los elementos activables, como un botón, cuando el usuario mueve el foco del teclado o del controlador para juegos hacia ellos. Al animar el brillo alrededor del borde de los elementos enfocados, Reveal focus permite que los usuarios entiendan mejor dónde está el foco y hacia dónde va el foco.

La opción Reveal focus está desactivada de manera predeterminada. Para experiencias de 3 metros, deberás habilitarla para revelar el foco configurando la [propiedad Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) en tu constructor de aplicaciones.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Para obtener más información, consulta la guía sobre [Reveal focus](/windows/uwp/design/style/reveal-focus)

### <a name="customizing-the-focus-visual"></a>Personalización del foco visual

Si quieres personalizar el foco visual, puede hacerlo modificando las propiedades relacionadas con el foco visual de cada control. Hay varias propiedades que se pueden aprovechar para personalizar la aplicación.

También puedes descartar los focos visuales proporcionados por el sistema. Para ello, dibuja los tuyos propios mediante los estados visuales. Para obtener más información, consulta [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState).

### <a name="light-dismiss-overlay"></a>Superposición de cierre del elemento por cambio de foco

Para llamar la atención del usuario a los elementos de la interfaz de usuario que el usuario está manipulando actualmente con el controlador para juegos o el control remoto, la UWP agrega automáticamente una capa de "humo" que abarca áreas fuera de la interfaz de usuario emergente cuando la aplicación se ejecuta en Xbox One. Esto no requiere ningún trabajo adicional, pero es algo a tener en cuenta al diseñar la interfaz de usuario. Puedes establecer la propiedad `LightDismissOverlayMode` en cualquier `FlyoutBase` para habilitar o deshabilitar la capa de humo; el valor predeterminado es `Auto`, lo que significa que está habilitada en Xbox y deshabilitada en otros lugares. Para obtener más información, consulta [Cierre del elemento modal frente a cierre del elemento por cambio de foco](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Participación del foco

La participación del foco está pensada para que sea más fácil usar un controlador para juegos o control remoto para interactuar con una aplicación.

> [!NOTE]
> Configurar la participación del foco no afecta al teclado ni a los demás dispositivos de entrada.

Cuando la propiedad `IsFocusEngagementEnabled` en un objeto [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) se establece en `True`, se marca que el control requiere la participación del foco. Esto significa que el usuario debe presionar el botón **A/Seleccionar** para "usar" el control e interactuar con él. Al terminar, puede presionar el botón **B/Atrás** para dejar de usar el control y navegar fuera de él.

> [!NOTE]
> `IsFocusEngagementEnabled` es una nueva API y aún no está documentada.

### <a name="focus-trapping"></a>Captura de foco

La captura de foco es lo que sucede cuando un usuario intenta navegar por la interfaz de usuario de una aplicación pero se queda "atrapado" dentro de un control, lo que le hace difícil o incluso imposible salir de este.

El siguiente ejemplo muestra una interfaz de usuario que crea una captura del foco.

![Botones a la izquierda y derecha de un control deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Si el usuario quiere navegar desde el botón izquierdo hasta el botón derecho, lo lógico sería asumir que todo lo que tiene que hacer es presionar dos veces el pad-D/palanca izquierda hacia la derecha.
Sin embargo, si el [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) no requiere participación, se produciría el siguiente comportamiento: cuando el usuario presiona la primera vez hacia la derecha, el foco se cambiará al `Slider`, y cuando presione de nuevo a la derecha, el controlador del `Slider` se movería hacia la derecha. El usuario seguiría moviendo el controlador hacia la derecha y no sería capaz de llegar al botón.

Hay varios enfoques para evitar este problema. Uno es crear un diseño diferente, similar al ejemplo de la aplicación inmobiliaria en [Interacción y navegación con foco XY](#xy-focus-navigation-and-interaction) en la que reubicamos los botones **anterior** y **siguiente** encima de la `ListView`. Apilar los controles de forma vertical en lugar de horizontal como en la siguiente imagen solucionaría el problema.

![Botones encima y debajo de un control deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Ahora el usuario puede navegar a cada uno de los controles presionando hacia arriba y hacia abajo en el pad-D/palanca izquierda y cuando `Slider` tiene el foco, pueden presionar hacia la izquierda y derecha para mover el controlador de `Slider`, como se espera.

Otro enfoque para resolver este problema es solicitar participación en `Slider`. Si estableces `IsFocusEngagementEnabled="True"`, esto dará como resultado el siguiente comportamiento.

![Solicitar la participación del foco en el control deslizante para que el usuario pueda navegar hasta el botón de la derecha](images/designing-for-tv/focus-engagement-slider.png)

Cuando `Slider` requiera la participación del foco, el usuario puede llegar al botón de la derecha simplemente presionando dos veces el pad-D/palanca izquierda hacia la derecha. Esta solución es excelente porque no precisa de ajustes en la interfaz de usuario y produce el comportamiento esperado.

### <a name="items-controls"></a>Controles de los elementos

Aparte del control [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider), hay otros controles a los cuales conviene solicitar participación, como por ejemplo:

- [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

A diferencia del control `Slider`, estos controles no capturan el foco en su interior; sin embargo, pueden provocar problemas de facilidad de uso cuando contengan grandes cantidades de datos. El siguiente es un ejemplo de una `ListView` que contiene una gran cantidad de datos.

![ListView con gran cantidad de datos y botones por encima y por debajo](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Similar al ejemplo del `Slider`, tratemos de navegar desde el botón en la parte superior hasta el botón en la parte inferior con un controlador para juegos/control remoto.
Comenzando por el foco en el botón superior, al presionar hacia abajo en el pad-D/palanca el foco se coloca en el primer elemento de `ListView` ("elemento 1").
Cuando el usuario presiona nuevamente hacia abajo, el siguiente elemento en la lista obtiene el foco, no el botón en la parte inferior.
Para llegar al botón, el usuario primero debe desplazarse por cada elemento de la `ListView`.
Si `ListView` contiene una gran cantidad de datos, esto puede resultar incómodo y no ser una experiencia óptima para el usuario.

Para resolver este problema, establece la propiedad `IsFocusEngagementEnabled="True"` en `ListView` para que requiera participación en él.
Esto permitirá al usuario saltear la `ListView` rápidamente simplemente presionando hacia abajo. Sin embargo, no podrán desplazarse por la lista o elegir un elemento de ella a menos que lo activen presionando el botón **A/Seleccionar** cuando tenga foco y, a continuación, presionando el botón **B/Atrás** para desactivarlo.

![ListView con participación requerida](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

[ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) es ligeramente diferente a estos controles, ya que tiene sus propias peculiaridades que hay que tener en cuenta. Si tienes un `ScrollViewer` con contenido activable, desplazándose al `ScrollViewer` te permitirá por defecto moverte por sus elementos activables. Como en `ListView`, debes desplazarte por cada elemento para navegar fuera de `ScrollViewer`.

Si el `ScrollViewer`*no* tiene contenido activable (por ejemplo, si solo contiene texto) puedes establecer &mdash; para que el usuario puede activar el &mdash; mediante el uso del botón `IsFocusEngagementEnabled="True"`A/Seleccionar`ScrollViewer`. Después de haberlo activado, pueden desplazarse por el texto mediante el **pad-D/palanca izquierda**y, a continuación, presionar el botón **B/Atrás** para desactivarlo una vez que han terminado.

Otro enfoque sería establecer `IsTabStop="True"` en el `ScrollViewer` para que el usuario no tenga que activar el control; pueden simplemente colocar el foco en él y, a continuación, desplazarse con &mdash;pad-D/palanca izquierda**cuando no haya ningún elemento activable el**.

### <a name="focus-engagement-defaults"></a>Valores predeterminados de participación del foco

Algunos controles causan la captura del foco con frecuencia suficiente como para justificar que su configuración predeterminada requiera participación del foco, mientras que en otros la participación del foco está desactivada de forma predeterminada, pero pueden beneficiarse de su activación. La siguiente tabla enumera estos controles y sus comportamientos de participación del foco predeterminados.

| Control               | Valor predeterminado de participación del foco  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Activado                        |
| FlipView              | Desactivado                       |
| GridView              | Desactivado                       |
| ListBox               | Desactivado                       |
| ListView              | Desactivado                       |
| ScrollViewer          | Desactivado                       |
| SemanticZoom          | Desactivado                       |
| Control deslizante                | Activado                        |

Todos los demás controles de la UWP no darán como resultado ningún cambio visual o del comportamiento cuando `IsFocusEngagementEnabled="True"`.

## <a name="summary"></a>Resumen

Puede compilar aplicaciones para UWP que estén optimizadas para un dispositivo o una experiencia específicos, pero el Plataforma universal de Windows también le permite compilar aplicaciones que se pueden usar correctamente en todos los dispositivos, tanto en experiencias de 2 pies como en de 10 pies, y independientemente de la entrada. capacidad del dispositivo o del usuario. El uso de las recomendaciones de este artículo puede garantizar que la aplicación sea tan buena como para la televisión y un equipo.

## <a name="related-articles"></a>Artículos relacionados

- [Diseño para Xbox y televisión](../devices/designing-for-tv.md)
- [Dispositivos principales para aplicaciones Plataforma universal de Windows (UWP)](index.md)

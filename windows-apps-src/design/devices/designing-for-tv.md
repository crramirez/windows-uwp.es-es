---
author: eliotcowley
Description: Design your app so that it looks good and functions well on your television.
title: Diseñar para Xbox y televisión
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, experiencia de 10 pies, controlador para juegos, control remoto, entrada, interacción
ms.author: elcowle
ms.date: 12/5/2017
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 098bc97de27d58fdc1d582e0db264ef04f0d3e61
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047114"
---
# <a name="designing-for-xbox-and-tv"></a>Diseño para Xbox y televisión

Diseña tu aplicación para la Plataforma universal de Windows (UWP) de manera que se vea y funcione bien en la Xbox One y las pantallas de televisión.

## <a name="overview"></a>Información general

La Plataforma universal de Windows (UWP) te permite crear experiencias agradables en varios dispositivos de Windows 10.
La mayoría de la funcionalidad proporcionada por el entorno de desarrollo UWP les permite a las aplicaciones usar la misma interfaz de usuario (UI) en cualquiera de estos dispositivos, sin realizar ningún trabajo adicional.
Sin embargo, personalizar y optimizar tu aplicación para que funcione bien en Xbox One y pantallas de televisión requiere consideraciones especiales.

La experiencia de estar sentado en tu sofá al otro lado de la sala, con un controlador para juegos o control remoto para interactuar con tu televisor, se llama la **experiencia de 10 pies**.
Se denomina así porque el usuario generalmente está sentado aproximadamente a 10 pies de distancia de la pantalla.
Esto proporciona desafíos que no están presentes, por ejemplo, en la experiencia de *2 pies* o interactuando con un PC.
Si estás desarrollando una aplicación para Xbox One o cualquier otro dispositivo que se muestra en una pantalla de TV y usa un controlador para la entrada, siempre debes tener esto en cuenta.

No todos los pasos de este artículo son necesarios para que tu aplicación funcione bien en experiencias de 10 pies, pero entenderlos y tomar las decisiones apropiadas para tu aplicación dará como resultado una mejor experiencia de 10 pies adaptada a las necesidades específicas de tu aplicación.
Al traer a la vida a tu aplicación en el entorno de 10 pies, ten en cuenta los siguientes principios de diseño.

### <a name="simple"></a>Simple

El diseño para el entorno de 10 pies presenta un conjunto único de desafíos. La resolución y distancia de visualización pueden dificultar a la gente el procesar demasiada información.
Intenta mantener tu diseño limpio y reducido a los componentes más simples posibles. La cantidad de información que se muestra en un televisor debe ser comparable a lo que verías en un teléfono móvil, más que en un equipo de escritorio.

![Pantalla principal de Xbox One](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>Coherente

Las aplicaciones para UWP en el entorno de 10 pies deben ser intuitivas y fáciles de usar. Haz que el enfoque sea claro e inequívoco.
Organiza el contenido para que el movimiento a través del espacio sea consistente y predecible. Dale a las personas la ruta más corta hacia lo quieran hacer.

![Aplicación para películas de Xbox One](images/designing-for-tv/xbox-movies-app.png)

_**Todas las películas que se muestran en la captura de pantalla están disponibles en Películas y TV de Microsoft.**_  

### <a name="captivating"></a>Fascinantes

Las experiencias más envolventes y cinematográficas realizadas en la pantalla grande. Los escenarios de extremo a extremo, el movimiento elegante y un vibrante uso del color y la tipografía llevan tus aplicaciones al siguiente nivel. Sea valiente y hermoso.

![Aplicación para avatares de Xbox One](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Optimizaciones para la experiencia de 10 pies

Ahora que conoces los principios del buen diseño de aplicaciones para experiencia de 10 pies en UWP, lee la siguiente información general sobre las formas específicas en las puedes optimizar tu aplicación y proporcionar una gran experiencia al usuario.

| Función        | Descripción           |
| -------------------------------------------------------------- |--------------------------------|
| [Controlador para juegos y control remoto](#gamepad-and-remote-control)      | Asegurarse de que la aplicación funcione bien tanto con el controlador para juegos como con el control remoto es el paso más importante en la optimización de experiencias de 10 pies. Hay varias mejoras específicas para los controladores para juegos y los controles remotos que puedes hacer para optimizar la interacción con el usuario en un dispositivo en el que sus acciones son un poco limitadas. |
| [Interacción y navegación con foco XY](#xy-focus-navigation-and-interaction) | El UWP proporciona **navegación con foco XY** la cual permite al usuario navegar por la interfaz de usuario de tu aplicación. Sin embargo, esto limita al usuario a navegar hacia arriba, abajo, izquierda y derecha. En esta sección se describen recomendaciones para lidiar con esta y otras consideraciones. |
| [Modo de mouse](#mouse-mode)|En algunas interfaces de usuario, tales como mapas y superficies de dibujo, no es posible o práctico usar la navegación con foco XY. Para estas interfaces, UWP proporciona el **modo de mouse** para permitir al controlador para juegos y al control remoto navegar libremente, como el mouse de un equipo de escritorio.|
| [Foco visual](#focus-visual)  | El foco visual es el borde alrededor del elemento de interfaz de usuario que actualmente tiene el foco. Esto ayuda a orientar al usuario para que pueda navegar fácilmente por tu interfaz de usuario sin perderse. Si el foco no está claramente visible, el usuario podría perderse en tu interfaz de usuario y no tener una gran experiencia.  |
| [Participación del foco](#focus-engagement) | Configurar la participación del foco en un elemento de interfaz de usuario requiere que el usuario presione el botón **A/seleccionar** para interactuar con él. Esto puede ayudar a crear una mejor experiencia para el usuario cuando navegue por la interfaz de usuario de la aplicación.
| [Variación del tamaño del elemento de la interfaz de usuario](#ui-element-sizing)  | La Plataforma universal de Windows usa [píxeles efectivos y ajuste de escala](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) para escalar la interfaz de usuario según la distancia de visualización. Entender la variación de tamaño y aplicarla a toda tu interfaz de usuario te ayudarán a optimizar tu aplicación para el entorno de 10 pies.  |
|  [Zona segura del televisor](#tv-safe-area) | La UWP automáticamente evitará mostrar cualquier interfaz de usuario en áreas no seguras de los televisores (áreas cercanas a los bordes de la pantalla) de forma predeterminada. Sin embargo, esto crea un efecto de "encajonamiento" en la que la interfaz de usuario queda encerrada por un marco. Para que tu aplicación sea verdaderamente envolvente en el televisor, es conveniente modificarla para que se extiende hasta los bordes de la pantalla en los televisores que lo admiten. |
| [Colores](#colors)  |  La UWP admite temas de colores y una aplicación que respeta el tema del sistema tendrá como valor predeterminado **oscuro** en Xbox One. Si tu aplicación tiene un tema de color específico, debes tener en cuenta que algunos colores no funcionan bien en los televisores y deben evitarse. |
| [Sonido](../style/sound.md)    | Los sonidos desempeñan un papel esencial en la experiencia de 10 pies, ya que ayudan a sumergir y a proporcionar información al usuario. El UWP proporciona funciones que activan automáticamente los sonidos de los controles habituales cuando la aplicación se ejecuta en Xbox One. Obtén más información sobre la compatibilidad de audio integrada en el UWP y descubre cómo sacarle provecho.    |
| [Directrices sobre controles de la interfaz de usuario](#guidelines-for-ui-controls)  |  Hay varios controles de interfaz de usuario que funcionan bien en varios dispositivos, pero tienen ciertas consideraciones cuando se usan en televisores. Obtén información sobre algunos procedimientos recomendados para usar estos controles al diseñar la experiencia de 10 pies. |
| [Desencadenador de estado visual personalizado para Xbox](#custom-visual-state-trigger-for-xbox) | A fin de adaptar tu aplicación para UWP a la experiencia de 10 pies, te recomendamos usar un *desencadenador de estado visual* personalizado para realizar cambios de diseño cuando la aplicación detecte que se ha iniciado en una consola Xbox.

> [!NOTE]
> La mayoría de los fragmentos de código de este tema están en XAML/C#; Sin embargo, los principios y conceptos se aplican a todas las aplicaciones para UWP. Si estás desarrollando una aplicación para UWP en HTML/JavaScript para Xbox, echa un vistazo a la excelente biblioteca [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) en GitHub.

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

\*Si la aplicación no controla los eventos [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941.aspx) ni [KeyUp](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) del botón B, se desencadenará el evento [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx), lo que produciría una navegación hacia atrás dentro de la aplicación. Sin embargo, deberás implementarlo tú mismo, como se indica en el siguiente fragmento de código:

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
| Retroceder/avanzar página  | Retroceder/avanzar página | Desencadenadores izquierdo/derecho | [CalendarView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.calendarview.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [ComboBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Vistas que admiten el desplazamiento vertical
| Página a la izquierda/derecha | Ninguna | Reboteadores izquierdo/derecho | [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Vistas que admiten el desplazamiento horizontal
| Acercar/alejar        | Ctrl +/- | Desencadenadores izquierdo/derecho | Ninguna | `ScrollViewer`, vistas que admiten acercamiento y alejamiento |
| Abrir o cerrar el panel de navegación | Ninguna | Vista | Ninguna | Paneles de navegación |
| [Buscar](#search-experience) | Ninguna | Botón Y | Ninguna | Combinación de teclas para la función de búsqueda principal de la aplicación |
| [Abrir menú contextual.](#commandbar-and-contextflyout) | Haz clic con el botón derecho | Botón de menú | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Menús contextuales |

## <a name="xy-focus-navigation-and-interaction"></a>Interacción y navegación con foco XY

Si tu aplicación admite una navegación del foco adecuada para el teclado, esto se trasladará bien al controlador para juegos y al control remoto.
La navegación con las teclas de dirección se asigna al **pad-D** (así como a la **palanca izquierda** en el controlador para juegos), y la interacción con los elementos de la interfaz de usuario se asigna a la tecla **Entrar/Seleccionar** (ver [Controlador para juegos y control remoto](#gamepad-and-remote-control)).

Muchos eventos y propiedades se usan tanto en el teclado como en el controlador de juegos. Ambos activan los eventos `KeyDown` y `KeyUp`, y solo navegarán a los controles que tengan las propiedades `IsTabStop="True"` y `Visibility="Visible"`. Para obtener directrices de diseño para teclado, consulta [Interacciones con el teclado](../input/keyboard-interactions.md).

Si la compatibilidad con el teclado se implementa correctamente, tu aplicación funcionará razonablemente bien; sin embargo, esto puede requerir algo más de trabajo para admitir todos los escenarios. Piensa en las necesidades específicas de tu aplicación para proporcionar la mejor experiencia posible para el usuario.

> [!IMPORTANT]
> El modo de mouse está habilitado de manera predeterminada para las aplicaciones para UWP que se ejecutan en Xbox One. Para deshabilitar el modo de mouse y habilitar la navegación con foco XY, establece `Application.RequiresPointerMode=WhenRequested`.

### <a name="debugging-focus-issues"></a>Depurar problemas de foco

El método [FocusManager.GetFocusedElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.focusmanager.getfocusedelement.aspx) te indicará qué elemento tiene el foco actualmente. Resulta útil para situaciones donde la ubicación del foco visual puede no ser obvia. Puedes registrar esta información en la ventana de salida de Visual Studio de este modo:

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

* La propiedad [IsTabStop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.istabstop.aspx) o [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) no está correctamente establecida.
* El control que obtiene el foco es realmente más grande de lo que piensas: la navegación XY observa el tamaño total del control ([ActualWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualwidth.aspx) y [ActualHeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualheight.aspx)), no solo la parte del control que representa algo interesante.
* Un control activable se encuentra encima de otro: la navegación XY no admite controles que se superponen.

Si la navegación XY sigue sin funcionar de la manera esperada después de solucionar estos problemas, puedes apuntar manualmente al elemento que quieres que tenga el foco mediante el método que se describe en [Reemplazo de la navegación predeterminada](#overriding-the-default-navigation).

Si la navegación XY funciona según lo previsto, pero no se muestra ningún foco visual, la causa podría ser uno de los siguientes problemas:

* Has vuelto a basar el control en el modelo sin incluir un foco visual. Establece `UseSystemFocusVisuals="True"` o agrega un foco visual manualmente.
* Has movido el foco mediante una llamada a `Focus(FocusState.Pointer)`. El parámetro [FocusState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.focusstate.aspx) controla lo que sucede con el foco visual. Por lo general, se debe establecer en `FocusState.Programmatic`, opción que mantiene el foco visual visible si antes era visible y oculto si estaba oculto anteriormente.

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

Intenta permitirle al usuario realizar las tareas más comunes en el menor número de clics. En el siguiente ejemplo, el [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) se coloca entre el botón **Reproducir** (que inicialmente obtiene el foco) y un elemento usado frecuentemente, para que se coloque un elemento innecesario entre tareas con prioridad.

![Los procedimientos de navegación recomendados proporcionan el camino con menos clics](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

En cambio, en el siguiente ejemplo, el [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) se coloca encima del botón **Reproducir**.
Simplemente reorganizar la interfaz de usuario para que no queden elementos innecesarios entre tareas con una prioridad mejorará en gran medida la facilidad de uso de tu aplicación.

![TextBlock se movió hacia arriba del botón Reproducir para que no quede entre tareas con prioridad](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar y ContextFlyout

Cuando se usa [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), ten en cuenta el problema de desplazamiento por una lista, como se mencionó en [Problema: elementos de la interfaz de usuario ubicados después de una cuadrícula/lista de desplazamiento larga](#problem-ui-elements-located-after-long-scrolling-list-grid). La siguiente imagen muestra un diseño de interfaz de usuario con `CommandBar` en la parte inferior de la lista/cuadrícula. El usuario tendría que desplazarse hacia abajo de todo por la lista/cuadrícula para llegar a la `CommandBar`.

![CommandBar en la parte inferior de la lista/cuadrícula](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

¿Qué ocurre si colocas `CommandBar` *por encima* de la lista/cuadrícula? Si bien un usuario que se desplazó hacia abajo de la lista/cuadrícula tendría que desplazarse nuevamente hacia arriba para llegar a la `CommandBar`, se trata de una navegación ligeramente más corta que en la configuración anterior. Ten en cuenta que esto es suponiendo que el foco inicial de tu aplicación se sitúe junto a o encima de `CommandBar`; este enfoque no funcionará si el foco inicial se encuentra por debajo de la lista/cuadrícula. Si estos elementos de la `CommandBar` son elementos de acción global a los que no se accede muy a menudo (como un botón **Sincronizar**), puede ser aceptable que se ubiquen encima de la lista/cuadrícula.

Si bien no se pueden apilar los elementos de una `CommandBar` verticalmente, colocarlos contra la dirección del desplazamiento (por ejemplo, a la izquierda o derecha de una lista de desplazamiento vertical, o en la parte superior o inferior de una lista de desplazamiento horizontal) es otra opción que podrías considerar si funciona bien para el diseño de tu interfaz de usuario.

Si tu aplicación tiene una `CommandBar` cuyos elementos deben ser fácilmente accesibles para los usuarios, es aconsejable que consideres colocar estos elementos dentro de una propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) y los quites de la `CommandBar`. `ContextFlyout` es una propiedad de [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx) y es el [menú contextual](../controls-and-patterns/dialogs-and-flyouts/index.md) asociado a ese elemento. En el equipo, al hacer clic con el botón derecho en un elemento con una propiedad `ContextFlyout`, aparecerá el menú contextual correspondiente. En Xbox One, esto ocurrirá al presionar el botón **Menú** mientras el foco está en dicho elemento.

### <a name="ui-layout-challenges"></a>Desafíos de diseño de la interfaz de usuario

Algunos diseños de interfaz de usuario son más desafiantes debido a la naturaleza de la navegación con foco XY y deben evaluarse caso por caso. Si bien no hay una única forma "correcta" y la solución que elijas depende de las necesidades específicas de tu aplicación, hay algunas técnicas que puedes emplear para brindar una gran experiencia en el televisor.

Para comprender mejor esto, echemos un vistazo a una aplicación imaginaria que muestra algunos de estos problemas y las técnicas para resolverlos.

> [!NOTE]
> Esta aplicación imaginaria pretende ilustrar los problemas de la interfaz de usuario y sus posibles soluciones, y no está pensada para mostrar la mejor experiencia de usuario para tu aplicación en particular.

La siguiente es una aplicación inmobiliaria imaginaria que muestra una lista de viviendas disponibles para la venta, un mapa, una descripción de la propiedad y demás información. Esta aplicación presenta tres desafíos que puedes superar mediante el uso de las siguientes técnicas:

- [Reorganizar la interfaz de usuario](#ui-rearrange)
- [Participación del foco](#engagement)
- [Modo de mouse](#mouse-mode)

![Aplicación inmobiliaria imaginaria](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problema: algunos elementos de la interfaz de usuario se encuentran después de una cuadrícula/lista de desplazamiento larga<a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

La [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) de las propiedades que se muestra en la siguiente imagen es una lista con desplazamiento muy larga. Si la [participación](#focus-engagement) *no* se requiere en la `ListView`, cuando el usuario navegue a la lista, el foco se colocará en el primer elemento en la lista. Para que el usuario llegue al botón **anterior** o **siguiente**, deberá pasar por todos los elementos de la lista. En casos como este en el que resulta difícil solicitarle al usuario que recorra toda la lista (es decir, cuando la lista no es lo suficientemente corta como para que esta experiencia sea aceptable) sería conveniente considerar otras opciones.

![Aplicación inmobiliaria: una lista de 50 elementos sencillos requiere 51 clics para llegar a los botones de abajo](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Soluciones

**Reorganizar la interfaz de usuario <a name="ui-rearrange"></a>**

A menos que tu foco inicial se coloque en la parte inferior de la página, los elementos de la interfaz de usuario colocados encima de una lista con desplazamiento larga son por lo general más accesibles que si se encuentran debajo.
Si este nuevo diseño funciona para los otros dispositivos, cambiar el diseño para todas las familias de dispositivos en lugar de hacer cambios especiales a la interfaz de usuario solo para Xbox One puede ser un enfoque menos costoso.
Además, colocar los elementos de la interfaz de usuario en contra de la dirección de desplazamiento (es decir, horizontalmente a una lista de desplazamiento vertical, o verticalmente a una lista de desplazamiento horizontal) mejorará aún más la accesibilidad.

![Aplicación inmobiliaria: botones situados encima de la lista con desplazamiento larga](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Participación del foco <a name="engagement"></a>**

Cuando se *requiere* la participación, todo la `ListView` se convierte en un objeto de foco único. El usuario podrá saltear el contenido de la lista para ir al siguiente elemento activable. Lee más acerca de los controles que admiten participación y cómo usarlos en [Participación del foco](#focus-engagement).

![Aplicación inmobiliaria: participación definida como requerida para que solo lleve 1 clic llegar a los botones Anterior/Siguiente](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problema: ScrollViewer sin elementos activables

Como la navegación con foco XY depende de la navegación a un elemento de interfaz de usuario activable cada vez, un [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) que no contiene elementos activables (como con solo texto, al igual que en este ejemplo) puede desencadenar un escenario en el que el usuario no puede ver todo el contenido del `ScrollViewer`.
Consulta las soluciones para este y otros escenarios relacionados en [Participación del foco](#focus-engagement).

![Aplicación inmobiliaria: ScrollViewer solo con texto](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problema: Interfaz de usuario de desplazamiento libre

Cuando tu aplicación requiera una interfaz de usuario de desplazamiento libre, como por ejemplo una superficie de dibujo o, en este ejemplo, un mapa, la navegación con foco XY simplemente no funciona.
En estos casos, puedes activar el [modo de mouse](#mouse-mode) para permitir que el usuario pueda navegar libremente dentro de un elemento de la interfaz de usuario.

![Elemento de mapa de la interfaz de usuario en modo de mouse](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Modo de mouse

Como se describe en [Interacción y navegación con foco XY](#xy-focus-navigation-and-interaction), en Xbox One el foco se mueve mediante un sistema de navegación XY, el cual permite al usuario cambiar el foco desde un control a otro desplazándose hacia arriba, abajo, izquierda y derecha.
Sin embargo, algunos controles, como [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) y [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx), requieren una interacción de mouse en la que los usuarios pueden mover libremente el puntero dentro de los límites del control.
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
> El valor `WhenFocused` solo se admite en objetos [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx). Si se intenta establecer este valor en un control, se generará una excepción.

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

### <a name="reveal-focus"></a>Reveal focus

Reveal focus es un efecto de iluminación que anima el borde de los elementos activables, como un botón, cuando el usuario mueve el foco del teclado o del controlador para juegos hacia ellos. Al animar el brillo alrededor del borde de los elementos enfocados, Reveal focus permite que los usuarios entiendan mejor dónde está el foco y hacia dónde va el foco.

La opción Reveal focus está desactivada de manera predeterminada. Para experiencias de 3 metros, deberás habilitarla para revelar el foco configurando la [propiedad Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) en tu constructor de aplicaciones.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Para obtener más información, consulta la guía sobre [Reveal focus](/windows/uwp/design/style/reveal-focus)

### <a name="customizing-the-focus-visual"></a>Personalizar el foco visual

Si quieres personalizar el foco visual, puede hacerlo modificando las propiedades relacionadas con el foco visual de cada control. Hay varias propiedades que se pueden aprovechar para personalizar la aplicación.

También puedes descartar los focos visuales proporcionados por el sistema. Para ello, dibuja los tuyos propios mediante los estados visuales. Para obtener más información, consulta [VisualState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.Aspx).

### <a name="light-dismiss-overlay"></a>Superposición de cierre del elemento por cambio de foco

Para llamar la atención del usuario a los elementos de la interfaz de usuario que el usuario está manipulando actualmente con el controlador para juegos o el control remoto, la UWP agrega automáticamente una capa de "humo" que abarca áreas fuera de la interfaz de usuario emergente cuando la aplicación se ejecuta en Xbox One. Esto no requiere ningún trabajo adicional, pero es algo a tener en cuenta al diseñar la interfaz de usuario. Puedes establecer la propiedad `LightDismissOverlayMode` en cualquier `FlyoutBase` para habilitar o deshabilitar la capa de humo; el valor predeterminado es `Auto`, lo que significa que está habilitada en Xbox y deshabilitada en otros lugares. Para obtener más información, consulta [Cierre del elemento modal frente a cierre del elemento por cambio de foco](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Participación del foco

La participación del foco está pensada para que sea más fácil usar un controlador para juegos o control remoto para interactuar con una aplicación.

> [!NOTE]
> Configurar la participación del foco no afecta al teclado ni a los demás dispositivos de entrada.

Cuando la propiedad `IsFocusEngagementEnabled` en un objeto [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) se establece en `True`, se marca que el control requiere la participación del foco. Esto significa que el usuario debe presionar el botón **A/Seleccionar** para "usar" el control e interactuar con él. Al terminar, puede presionar el botón **B/Atrás** para dejar de usar el control y navegar fuera de él.

> [!NOTE]
> `IsFocusEngagementEnabled` es una nueva API y aún no está documentada.

### <a name="focus-trapping"></a>Captura de foco

La captura de foco es lo que sucede cuando un usuario intenta navegar por la interfaz de usuario de una aplicación pero se queda "atrapado" dentro de un control, lo que le hace difícil o incluso imposible salir de este.

El siguiente ejemplo muestra una interfaz de usuario que crea una captura del foco.

![Botones a la izquierda y derecha de un control deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Si el usuario quiere navegar desde el botón izquierdo hasta el botón derecho, lo lógico sería asumir que todo lo que tiene que hacer es presionar dos veces el pad-D/palanca izquierda hacia la derecha.
Sin embargo, si el [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx) no requiere participación, se produciría el siguiente comportamiento: cuando el usuario presiona la primera vez hacia la derecha, el foco se cambiará al `Slider`, y cuando presione de nuevo a la derecha, el controlador del `Slider` se movería hacia la derecha. El usuario seguiría moviendo el controlador hacia la derecha y no sería capaz de llegar al botón.

Hay varios enfoques para evitar este problema. Uno es crear un diseño diferente, similar al ejemplo de la aplicación inmobiliaria en [Interacción y navegación con foco XY](#xy-focus-navigation-and-interaction) en la que reubicamos los botones **anterior** y **siguiente** encima de la `ListView`. Apilar los controles de forma vertical en lugar de horizontal como en la siguiente imagen solucionaría el problema.

![Botones encima y debajo de un control deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Ahora el usuario puede navegar a cada uno de los controles presionando hacia arriba y hacia abajo en el pad-D/palanca izquierda y cuando `Slider` tiene el foco, pueden presionar hacia la izquierda y derecha para mover el controlador de `Slider`, como se espera.

Otro enfoque para resolver este problema es solicitar participación en `Slider`. Si estableces `IsFocusEngagementEnabled="True"`, esto dará como resultado el siguiente comportamiento.

![Solicitar la participación del foco en el control deslizante para que el usuario pueda navegar hasta el botón de la derecha](images/designing-for-tv/focus-engagement-slider.png)

Cuando `Slider` requiera la participación del foco, el usuario puede llegar al botón de la derecha simplemente presionando dos veces el pad-D/palanca izquierda hacia la derecha. Esta solución es excelente porque no precisa de ajustes en la interfaz de usuario y produce el comportamiento esperado.

### <a name="items-controls"></a>Controles de los elementos

Aparte del control [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), hay otros controles a los cuales conviene solicitar participación, como por ejemplo:

- [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

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

[ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) es ligeramente diferente a estos controles, ya que tiene sus propias peculiaridades que hay que tener en cuenta. Si tienes un `ScrollViewer` con contenido activable, desplazándose al `ScrollViewer` te permitirá por defecto moverte por sus elementos activables. Como en `ListView`, debes desplazarte por cada elemento para navegar fuera de `ScrollViewer`.

Si el `ScrollViewer` *no* tiene contenido activable (por ejemplo, si solo contiene texto) puedes establecer `IsFocusEngagementEnabled="True"` para que el usuario puede activar el `ScrollViewer` mediante el uso del botón **A/Seleccionar**. Después de haberlo activado, pueden desplazarse por el texto mediante el **pad-D/palanca izquierda**y, a continuación, presionar el botón **B/Atrás** para desactivarlo una vez que han terminado.

Otro enfoque sería establecer `IsTabStop="True"` en el `ScrollViewer` para que el usuario no tenga que activar el control; pueden simplemente colocar el foco en él y, a continuación, desplazarse con **pad-D/palanca izquierda** cuando no haya ningún elemento activable el `ScrollViewer`.

### <a name="focus-engagement-defaults"></a>Valores predeterminados de participación del foco

Algunos controles causan la captura del foco con frecuencia suficiente como para justificar que su configuración predeterminada requiera participación del foco, mientras que en otros la participación del foco está desactivada de forma predeterminada, pero pueden beneficiarse de su activación. La siguiente tabla enumera estos controles y sus comportamientos de participación del foco predeterminados.

| Control               | Valor predeterminado de participación del foco  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Activado                        |
| FlipView              | Desactivada                       |
| GridView              | Desactivada                       |
| ListBox               | Desactivada                       |
| ListView              | Desactivada                       |
| ScrollViewer          | Desactivada                       |
| SemanticZoom          | Desactivada                       |
| Control deslizante                | Activado                        |

Todos los demás controles de la UWP no darán como resultado ningún cambio visual o del comportamiento cuando `IsFocusEngagementEnabled="True"`.

## <a name="ui-element-sizing"></a>Variación del tamaño del elemento de la interfaz de usuario

Debido a que el usuario de una aplicación en el entorno de 10 pies usa un control remoto o controlador para juegos y se encuentra a varios pies de la pantalla, hay algunas consideraciones sobre la interfaz de usuario que deben tenerse en cuenta en tu diseño.
Asegúrate de que la interfaz de usuario tenga una densidad de contenido apropiada y que no esté demasiado sobrecargada para que el usuario pueda navegar y seleccionar elementos fácilmente. Recuerda: la simplicidad es clave.

### <a name="scale-factor-and-adaptive-layout"></a>Factor de escala y diseño adaptativo

**El factor de escala** ayuda a garantizar que los elementos de la interfaz de usuario se muestren con el tamaño correcto para el dispositivo en el que se ejecuta la aplicación.
En el escritorio, esta configuración se encuentra en **Configuración > Sistema > Pantalla** como un valor de deslizamiento.
Esta misma configuración existe en el teléfono también si el dispositivo la admite.

![Cambia el tamaño de texto, las aplicaciones y otros elementos](images/designing-for-tv/ui-scaling.png)

En Xbox One, no hay ninguna configuración semejante del sistema; sin embargo, para que los elementos de la interfaz de usuario de la UWP tengan el tamaño apropiado para un televisor, se escalan con un valor predeterminado de **200%** para aplicaciones XAML y **150%** para aplicaciones HTML.
Siempre que los elementos de la interfaz de usuario sean del tamaño apropiado en otros dispositivos, también lo serán en televisores.
Xbox One representa tu aplicación en 1080p (1920 x 1080 píxeles). Por lo tanto, al traer una aplicación desde otros dispositivos tales como un equipo, asegúrate de que la interfaz de usuario se vea bien en 960 x 540 px al 100% de escala (o 1280 x 720 px en 100% de escala para aplicaciones HTML) usando las [técnicas adaptables](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

El diseño para Xbox es un poco diferente al diseño para PC porque solo necesitas preocuparte por una resolución: 1920 x 1080.
No importa si el usuario tiene un televisor con una mayor resolución; las aplicaciones para UWP siempre se ajustarán a 1080p.

Los tamaños correctos de los activos del conjunto de 200% (o 150% para aplicaciones HTML) también se extraerán para tu aplicación cuando se ejecute en Xbox One, independientemente de la resolución del televisor.

### <a name="content-density"></a>Densidad de contenido

Al diseñar tu aplicación, recuerda que el usuario verá la interfaz de usuario desde la distancia e interactuará con ella mediante un control remoto o controlador para juegos, lo que hace que la navegación sea más lenta que con un mouse o entrada táctil.

#### <a name="sizes-of-ui-controls"></a>Tamaños de los controles de la interfaz de usuario

Los elementos interactivos de la interfaz de usuario deben tener una altura mínima de 32 epx (píxeles efectivos). Este es el valor predeterminado de los controles comunes de la UWP, y cuando se usan en la escala del 200%, garantiza que los elementos de la interfaz de usuario serán visibles desde la distancia y ayuda a reducir la densidad de contenido.

![Botón de la UWP en la escala de 100% y 200%](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Número de clics

Cuando el usuario está navegando desde un extremo a otro de la pantalla del televisor, no debería llevarle más de **seis clics** para simplificar tu interfaz de usuario. Nuevamente se aplica aquí el principio de **simplicidad**. Para obtener más información, consulta [El camino de menos clics](#path-of-least-clicks).

![6 iconos a lo ancho](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Tamaños del texto

Para hacer que tu interfaz de usuario sea visible desde cierta distancia, usa las siguientes reglas generales:

* Texto principal y contenido de lectura: 15 epx mínimo
* Texto no crítico y contenido adicional: 12 epx mínimo

Cuando uses un texto más grande en tu interfaz de usuario, elige un tamaño que no limite la superficie de la pantalla demasiado, ocupando el espacio que potencialmente podría ocupar otro contenido.

### <a name="opting-out-of-scale-factor"></a>Deshabilitar el factor de escala

Recomendamos que tu aplicación aproveche la compatibilidad con el factor de escala, la cual le ayudará a ejecutarse correctamente en todos los dispositivos mediante un ajuste de escala para cada tipo de dispositivo.
Sin embargo, es posible deshabilitar este comportamiento y diseñar toda tu interfaz de usuario en escala del 100%. Ten en cuenta que no puedes cambiar el factor de escala a un valor distinto de 100%.

En el caso de las aplicaciones XAML, si quieres deshabilitar el factor de escala, usa el siguiente fragmento de código:

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` te notificará si la deshabilitación se ha realizado correctamente.

Para obtener más información, incluido el código de muestra para HTML/JavaScript, consulta [Cómo desactivar el escalado](../../xbox-apps/disable-scaling.md).

Asegúrate de calcular los tamaños adecuados de los elementos de la interfaz de usuario duplicando los valores de píxel *efectivos* mencionados en este tema a los valores de píxel *reales* (o multiplicando por 1,5, en el caso de las aplicaciones HTML).

## <a name="tv-safe-area"></a>Zona segura del televisor

No todos los televisores muestran contenido hasta los extremos de la pantalla, por razones históricas y tecnológicas. De forma predeterminada, la UWP evitará mostrar cualquier contenido de la interfaz de usuario en las áreas no seguras de los televisores y en su lugar, solo dibujará el fondo de la página.

El área no segura de un televisor está representada por el área azul en la siguiente imagen.

![Área no segura para televisores](images/designing-for-tv/tv-unsafe-area.png)

Puedes definir un color estático o de tema, o una imagen para el fondo, como muestran los siguientes fragmentos de código.

### <a name="theme-color"></a>Color del tema

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>Imagen

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

Este será el aspecto de tu aplicación sin realizar ningún trabajo adicional.

![Zona segura del televisor](images/designing-for-tv/tv-safe-area.png)

Esto no es óptimo ya que le proporciona a la aplicación un efecto de "encajonamiento", con elementos de la interfaz de usuario, tales como el panel de navegación y la cuadrícula, que parecen recortados. Sin embargo, puedes realizar optimizaciones para ampliar las partes de la interfaz de usuario hasta los bordes de la pantalla para que la aplicación tenga un efecto más cinematográfico.

### <a name="drawing-ui-to-the-edge"></a>Dibujar la interfaz de usuario hasta el borde

Te recomendamos que uses determinados elementos de interfaz de usuario para llegar hasta los bordes de la pantalla a fin de proporcionar al usuario una mayor inmersión. Estos incluyen [ScrollViewers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx), [paneles de navegación](../controls-and-patterns/navigationview.md) y [CommandBars](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx).

Por otro lado, también es importante que el texto y los elementos interactivos eviten siempre los bordes de la pantalla para asegurarte de que no se recortarán en algunos televisores. Te recomendamos que solo dibujes elementos visuales no esenciales dentro del 5% de los bordes de la pantalla. Como se mencionó en [Variación del tamaño del elemento de interfaz de usuario](#ui-element-sizing), una aplicación para UWP que respeta el factor de escala predeterminado de 200% de la consola Xbox One utilizará un área de 960 x 540 epx, por lo que en la interfaz de usuario de tu aplicación debes evitar colocar elementos esenciales en las siguientes áreas:

- a 27 epx de la parte superior e inferior
- a 48 epx de los lados izquierdo y derecho

En las siguientes secciones se describe cómo conseguir que la interfaz de usuario se amplíe hasta los bordes de la pantalla.

#### <a name="core-window-bounds"></a>Límites de la ventana principal

En las aplicaciones para UWP destinadas solo a la experiencia de 10 pies, usar los límites de la ventana principal es la opción más sencilla.

En el método `OnLaunched` de `App.xaml.cs`, agrega el siguiente código:

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Con esta línea de código, la ventana de la aplicación se extenderá hasta los bordes de la pantalla, por lo que tendrás que mover todos los elementos interactivos y esenciales de la interfaz de usuario al área segura del televisor descrita anteriormente. Los elementos de la interfaz de usuario transitorios, tales como los menús contextuales y [ComboBoxes](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) abiertos, permanecerán automáticamente dentro de la zona segura del televisor.

![Límites de la ventana principal](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Fondos del panel

Los paneles de navegación se dibujan normalmente cerca del borde de la pantalla, por lo que debes ampliar el fondo hasta el área no segura del televisor para no generar huecos extraños. Para ello, solo tienes que cambiar el color de fondo del panel de navegación al color de fondo de la aplicación.

El uso de los límites de la ventana principal según lo descrito anteriormente te permitirá dibujar la interfaz de usuario en los bordes de la pantalla, pero deberás usar los márgenes positivos en el contenido de [SplitView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.splitview.aspx) para mantenerlo dentro de la zona segura del televisor.

![Panel de navegación ampliado hasta los bordes de la pantalla](images/designing-for-tv/tv-safe-areas-2.png)

Aquí se ha ampliado fondo del panel de navegación hasta los bordes de la pantalla, mientras que los elementos de navegación se mantienen en la zona segura del televisor.
El contenido de `SplitView` (en este caso, una cuadrícula de elementos) se ha ampliado hasta la parte inferior de la pantalla para que parezca que sigue y no se corte, mientras que la parte superior de la cuadrícula sigue estando dentro de la zona segura del televisor. (Obtén más información sobre cómo hacerlo en [Topes de desplazamiento de listas y cuadrículas](#scrolling-ends-of-lists-and-grids)).

El siguiente fragmento de código logra este efecto:

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) es otro ejemplo de un panel que normalmente se ubica cerca de uno o más de los bordes de la aplicación y, como tal, su fondo se debe ampliar en los televisores hasta los bordes de la pantalla. También suele contener un botón **Más**, representado por "..." en el lado derecho, que debe permanecer en la zona segura del televisor. Las siguientes son algunas estrategias diferentes para lograr las interacciones y los efectos visuales deseados.

**Opción 1**: Cambiar el color de fondo de `CommandBar` a transparente o al mismo color que el fondo de la página:

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

Esto hará parecer que `CommandBar` está sobre el mismo fondo que el resto de la página, por lo tanto, el fondo fluye perfectamente hasta el borde de la pantalla.

**Opción 2**: Agregar un rectángulo de fondo cuyo relleno sea del mismo color que el fondo de `CommandBar` y que quede por debajo de `CommandBar` y cruce el resto de la página:

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> Si usas este enfoque, ten en cuenta que el botón **Más** cambia el alto del elemento `CommandBar` abierto si es necesario, a fin de mostrar las etiquetas de los `AppBarButton` debajo de los iconos. Te recomendamos que muevas las etiquetas a la *derecha* de sus iconos para evitar este cambio de tamaño. Para obtener más información, consulta [Etiquetas de CommandBar](#commandbar-labels).

Ambos enfoques se aplican a los otros tipos de controles que se describen en esta sección.

#### <a name="scrolling-ends-of-lists-and-grids"></a>Topes de desplazamiento de listas y cuadrículas

Es común que las listas y cuadrículas contengan más elementos de los que pueden caber en la pantalla al mismo tiempo. Cuando este es el caso, te recomendamos extender la lista o cuadrícula hasta el borde de la pantalla. Las listas y cuadrículas de desplazamiento horizontal se debe extender hasta el borde derecho y las de desplazamiento vertical se deben extender hasta a la parte inferior.

![Recorte de la cuadrícula del área segura del televisor](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Cuando una lista o cuadrícula se extiende de esta manera, es importante mantener el foco visual y su elemento asociado dentro de la zona segura del televisor.

![El foco de la cuadrícula con desplazamiento debe mantenerse en la zona segura del televisor](images/designing-for-tv/scrolling-grid-focus.png)

El UWP tiene una funcionalidad que conservará el foco visual dentro de [VisibleBounds](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx), pero necesitas agregar espaciado interno para garantizar que los elementos de cuadrícula/lista se puedan desplazar a la vista del área segura. Específicamente, agrega un margen positivo al [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) de la [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) o [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), como en el siguiente fragmento de código:

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

La idea es colocar el fragmento de código anterior en los recursos de la página o en la aplicación y, a continuación, acceder a él de la siguiente manera:

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> Este fragmento de código es específico para `ListView`s; para un estilo `GridView` establece el atributo [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) tanto para [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) como para [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx) para conseguir una `GridView`.

## <a name="colors"></a>Colores

De manera predeterminada, la plataforma Universal de Windows escala los colores de tu aplicación al intervalo seguro para el televisor, (consulta [Colores seguros para el televisor](#tv-safe-colors) para obtener más información) de modo que la aplicación pueda verse bien en cualquier televisor. Además, hay mejoras que puedes realizar en el conjunto de colores que usa tu aplicación para mejorar la experiencia visual en el televisor.

### <a name="application-theme"></a>Tema de la aplicación

Puedes elegir un **tema de la aplicación** (claro u oscuro) según lo que sea adecuado para tu aplicación, o bien puedes deshabilitar los temas. Obtén más información acerca de las recomendaciones generales para temas en [Temas de colores](../style/color.md).

La UWP también permite que las aplicaciones establezcan dinámicamente el tema en base a la configuración del sistema proporcionada por los dispositivos en los que se ejecutan.
Si bien la UWP siempre respeta la configuración de tema especificada por el usuario, cada dispositivo también proporciona un tema predeterminado adecuado.
Dada la naturaleza de Xbox One, la cual se espera que tengas más experiencias *multimedia* que experiencias de *productividad*, el valor predeterminado es un tema del sistema oscuro.
Si el tema de tu aplicación se basa en la configuración del sistema, espera un valor predeterminado oscuro en Xbox One.

### <a name="accent-color"></a>Color de énfasis

La UWP proporciona una forma cómoda para exponer el **color de énfasis** que el usuario ha seleccionado en su configuración del sistema.

En Xbox One, el usuario es capaz de seleccionar un color de usuario, al igual que puede seleccionar un color de énfasis en un equipo.
Siempre y cuando tu aplicación llame a estos colores de énfasis a través de pinceles o recursos de color, se usará el color que el usuario ha seleccionado en la configuración del sistema. Ten en cuenta que los colores de énfasis en Xbox One son por usuario y no por sistema.

También ten en cuenta que el conjunto de colores del usuario en Xbox One no es el mismo que en los equipos, teléfonos y otros dispositivos.

Siempre que tu aplicación use un recurso de pincel como **SystemControlForegroundAccentBrush** o un recurso de color (**SystemAccentColor**), o en cambio, llame a los colores de énfasis directamente a través de la API [UIColorType.Accent*](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx), estos colores se reemplazarán por los colores de énfasis disponibles en Xbox One. Los colores del pincel de contraste alto también se extraen del sistema del mismo modo que en un equipo o teléfono.

Para obtener más información sobre colores de énfasis en general, consulta [Color de énfasis](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Variación de color entre televisores

Al diseñar para televisores, observe que los colores se muestran de forma muy diferente según el televisor en el que se muestren. No asumas que los colores aparecerán exactamente igual que en el monitor. Si tu aplicación depende de diferencias sutiles en el color para diferenciar las partes de la interfaz de usuario, los colores se podrían mezclar y confundir a los usuarios. Trata de usar colores que sean lo suficientemente distintos para que los usuarios puedan diferenciarlos claramente, independientemente del televisor que utilicen.

### <a name="tv-safe-colors"></a>Colores seguros para el televisor

Los valores RGB de un color representan las intensidades de rojo, verde y azul. Las televisiones no controlan muy bien las intensidades extremas&mdash;pueden producir un extraño efecto de bandas o aparecer más pálidos en determinadas televisiones. Además, los colores de gran intensidad pueden causar un efecto de desbordamiento o "blooming" (los píxeles cercanos comienzan a dibujar los mismos colores). Si bien existen diferentes escuelas de pensamiento sobre lo que se considera colores seguros para el televisor, los colores dentro de los valores RGB de 16-235 (o 10-EB en hexadecimal) son generalmente seguros el televisor.

![Intervalo de colores seguros para el televisor](images/designing-for-tv/tv-safe-colors-2.png)

Históricamente, las aplicaciones en Xbox tenían que adaptar sus colores para entrar dentro de este intervalo de colores seguro para el televisor; sin embargo, a partir de la actualización de Fall Creators Update, Xbox One escala automáticamente el contenido de todo el intervalo en el intervalo seguro para el televisor. Esto significa que la mayoría de los desarrolladores de aplicaciones ya no tienen que preocuparse por los colores seguros para el televisor.

> [!IMPORTANT]
> Todo contenido de vídeo que ya esté en el intervalo de colores seguros para el televisor no tiene este efecto de escalas de colores aplicado cuando se reproduce mediante [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).

Si vas a desarrollar una aplicación con DirectX 11 o DirectX 12 y crear tu propia cadena de intercambio para representar la interfaz de usuario o vídeo, puedes especificar el espacio de colores que usas llamando a [IDXGISwapChain3::SetColorSpace1](https://msdn.microsoft.com/library/windows/desktop/dn903676), el cual permitirá que el sistema sepa si se debe escalar colores o no.

## <a name="guidelines-for-ui-controls"></a>Directrices sobre controles de la interfaz de usuario

Hay varios controles de interfaz de usuario que funcionan bien en varios dispositivos, pero tienen ciertas consideraciones cuando se usan en televisores. Obtén información sobre algunos procedimientos recomendados para usar estos controles al diseñar la experiencia de 10 pies.

### <a name="pivot-control"></a>Control dinámico

Una clase [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) permite la navegación rápida de vistas dentro de una aplicación a través de la selección de encabezados o pestañas diferentes. El control subraya el encabezado que tiene el foco, para destacar el encabezado actualmente seleccionado al usar el controlador para juegos o el control remoto.

![Subrayado dinámico](images/designing-for-tv/pivot-underline.png)

Puedes establecer la propiedad [Pivot.IsHeaderItemsCarouselEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabled.aspx) en `true` para que los controles dinámicos mantengan siempre la misma posición, en lugar de que el encabezado dinámico seleccionado se mueva siempre a la primera posición. Esto brinda una mejor experiencia en pantallas de gran tamaño tales como televisores, ya que el encapsulamiento de los encabezados puede resultar confuso para los usuarios. Si todos los encabezados de tabla dinámica no caben en la pantalla al mismo tiempo, habrá una barra de desplazamiento para permitir a los clientes ver los demás encabezados; Sin embargo, debes asegurarte de que todos caben en la pantalla para proporcionar la mejor experiencia. Para obtener más información, consulta [Pestañas y controles dinámicos](../controls-and-patterns/tabs-pivot.md).

### <a name="navigation-pane-a-namenavigation-pane"></a>Panel de navegación <a name="navigation-pane">

Un panel de navegación (también conocido como *menú hamburguesa*) es un control de navegación que suele usarse en las aplicaciones para UWP. Normalmente, es un panel con varias opciones para elegir en un menú de estilo lista que llevará al usuario a páginas diferentes. Por lo general, este panel se abre contraído para ahorrar espacio y el usuario puede hacer clic en un botón para abrirlo.

Mientras que los paneles de navegación son muy accesibles con el mouse y la función táctil, con el controlador para juegos y el control remoto son menos accesibles, ya que el usuario tiene que navegar hasta un botón para abrir el panel. Por lo tanto, se recomienda abrir el panel de navegación con el botón **Vista**, así como permitir que el usuario se desplace hacia la izquierda de la página para abrirlo. Para obtener un ejemplo de código sobre cómo implementar este patrón de diseño, consulta el documento [Navegación con foco mediante programación](../input/focus-navigation-programmatic.md#split-view-code-sample). De este modo, el usuario puede acceder muy fácilmente al contenido del panel. Para obtener más información sobre el comportamiento de los paneles de navegación en pantallas de diferentes tamaños, así como sobre los procedimientos recomendados para la navegación con el controlador para juegos o el control remoto, consulta [Paneles de navegación](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>Etiquetas de CommandBar

Es una buena idea tener las etiquetas a la derecha de los iconos en una [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) para que su altura se minimice y mantenga la coherencia. Para ello, establece la propiedad [CommandBar.DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) en `CommandBarDefaultLabelPosition.Right`.

![CommandBar con etiquetas a la derecha de los iconos](images/designing-for-tv/commandbar.png)

Al establecer esta propiedad, las etiquetas se mostrarán de manera permanente, lo que resulta ideal para la experiencia de 10 pies ya que minimiza el número de clics para el usuario. Este también es un modelo excelente que otros tipos de dispositivos pueden seguir.

### <a name="tooltip"></a>Información sobre herramientas

El control [Tooltip](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) se introdujo como una manera de proporcionar más información en la interfaz de usuario cuando el usuario mantiene el mouse sobre, o pulsa y mantiene su dedo en, un elemento. Para los controladores para juegos y controles remotos, el `Tooltip` aparece brevemente cuando el elemento obtiene el foco, permanece en la pantalla durante un instante y luego desaparece. Este comportamiento puede causar confusión si se usan demasiados `Tooltip`. Intenta evitar el uso de `Tooltip` cuando diseñes para televisores.

### <a name="button-styles"></a>Estilos de botón

Si bien los botones estándar de la UWP funcionan bien en televisores, algunos estilos visuales de botones llaman mejor la atención sobre la interfaz de usuario, los cuales es conveniente considerar para todas las plataformas, especialmente para la experiencia de 10 pies, la cual se beneficia de comunicar claramente dónde se encuentra el foco. Para obtener más información sobre estos estilos, consulta [Botones](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Elementos de la interfaz de usuario anidada

La interfaz de usuario anidada expone los elementos anidados que pueden requerir acción dentro de un elemento de interfaz de usuario del contenedor en el que tanto el elemento anidado como el elemento contenedor pueden tener un foco independiente entre sí.

La interfaz de usuario anidada funciona bien para algunos tipos de entrada, pero no siempre para el controlador para juegos y el control remoto, que dependen de la navegación XY. Asegúrate de seguir las directrices de este tema para garantizar que el usuario pueda acceder fácilmente a todos los elementos interactivos y que la interfaz de usuario esté optimizada para el entorno de 304,8cm. Una solución habitual es colocar elementos de interfaz de usuario anidadas en un elemento `ContextFlyout` (consulta [CommandBar y ContextFlyout](#commandbar-and-contextflyout)).

Para obtener más información sobre la interfaz de usuario anidada, consulta [Interfaz de usuario anidada en elementos de lista](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

El elemento [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols.aspx) permite a los usuarios interactuar con el contenido multimedia gracias a una experiencia de reproducción predeterminada en la que pueden reproducir y pausar dicho contenido, activar los subtítulos y mucho más. Este control es propiedad de [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.aspx) y ofrece dos opciones de diseño: *fila única* y *file doble*. En el diseño de fila única, los botones de control deslizante y reproducción están dispuestos en la misma fila. Los botones de reproducción/pausa está situados a la izquierda del control deslizante. En el diseño de fila doble, el control deslizante está situado en la primera fila y los botones de reproducción se encuentran en una segunda fila inferior. Al diseñar la experiencia de 3 metros, se deberá usar el diseño de fila doble, ya que permite una mejor navegación del controlador para juegos. Para habilitar el diseño de fila doble, configúrala como `IsCompact="False"` en el elemento `MediaTransportControls` de la propiedad [TransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.transportcontrols.aspx) , propiedad de `MediaPlayerElement`.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

Visita [Reproducción de multimedia](../controls-and-patterns/media-playback.md) para obtener más información sobre cómo agregar contenido multimedia a tu aplicación.

> ![NOTA] `MediaPlayerElement` solo está disponible en Windows 10, versión 1607 y posteriores. Si vas a desarrollar una aplicación para una versión anterior de Windows 10, deberás usar [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) en su lugar. Las recomendaciones anteriores se aplican a `MediaElement` y podrás obtener acceso a la propiedad `TransportControls` de la misma manera.

### <a name="search-experience"></a>Experiencia de búsqueda

Buscar contenido es una de las funciones más habituales en la experiencia de 3 metros. Si tu aplicación proporciona una experiencia de búsqueda, es conveniente que el usuario pueda obtener acceso a ella rápidamente mediante el botón **Y** del controlador para juegos, a modo de acelerador.

La mayoría de los clientes ya estarán familiarizados con este acelerador, pero si lo deseas, puedes agregar un glifo visual **Y** en la interfaz de usuario para indicar que el cliente puede obtener acceso a la funcionalidad de búsqueda mediante ese botón. Si agregas esta indicación, asegúrate de usar el símbolo de la fuente **Segoe Xbox Symbol MDL2 Symbol** (`&#xE3CC;` para aplicaciones XAML apps y `\E426` para aplicaciones HTML) para que sea coherente con el Shell de Xbox y otras aplicaciones.

> [!NOTE]
> Dado que la fuente **Segoe Xbox MDL2 Symbol** solamente está disponible en Xbox, el símbolo no aparecerá correctamente en tu PC. Sin embargo, se mostrará en el televisor una vez que implementes en Xbox.

Dado que el botón **Y** solo está disponible en el controlador para juegos, asegúrate de proporcionar otros métodos de acceso a la búsqueda, como algunos botones en la interfaz de usuario. De lo contrario, es posible que algunos clientes no puedan obtener acceso a la funcionalidad.

En la experiencia de 3 metros, suele ser más fácil para los clientes usar una experiencia de búsqueda de pantalla completa porque el espacio en pantalla es limitado. Si tienes una funcionalidad de búsqueda "local" a pantalla completa o parcial, te recomendamos que, cuando el usuario abra la experiencia de búsqueda, el teclado en pantalla aparezca ya abierto, listo para que el cliente escriba los términos de búsqueda.

## <a name="custom-visual-state-trigger-for-xbox"></a>Desencadenador de estado visual personalizado para Xbox

Para adaptar tu aplicación para UWP a la experiencia de 3 metros, te recomendamos realizar cambios en el diseño cuando la aplicación detecte que se ha iniciado en una consola Xbox. Una manera de hacerlo es mediante un *desencadenador de estado visual* personalizado. Los desencadenadores de estado visual son especialmente útiles para la edición en **Blend para Visual Studio**. El siguiente fragmento de código muestra cómo crear un desencadenador de estado visual para Xbox:

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Para crear el desencadenador, agrega la siguiente clase a tu aplicación. Esta es la clase a la que hace referencia el código XAML anterior:

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

Después de agregar el desencadenador personalizado, tu aplicación hará automáticamente los cambios de diseño que especificaste en el código XAML siempre que detecte que se está ejecutando en una consola Xbox One.

Otra forma de comprobar si la aplicación se ejecuta en Xbox y después realizar los ajustes apropiados es a través de código. Puedes usar la siguiente variable simple para comprobar si la aplicación se ejecuta en Xbox:

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

Luego, puedes realizar los ajustes adecuados para la interfaz de usuario en el bloque de código siguiendo esta comprobación. Se muestra un ejemplo de esto en [muestra de color UWP](#uwp-color-sample).

## <a name="summary"></a>Resumen

El diseño para la experiencia de 10 pies presenta consideraciones especiales que hay que tener en cuenta y que lo hacen diferente del diseño para cualquier otra plataforma. Aunque por supuesto que puedes hacer una migración directa de la aplicación para UWP a Xbox One y que también funcione, no necesariamente estará optimizada para la experiencia de 10 pies y puede llevar a la frustración del usuario. Si sigues las instrucciones de este artículo te asegurarás de que la aplicación sea lo mejor posible en televisión.

## <a name="related-articles"></a>Artículos relacionados

- [Dispositivo básico para aplicaciones de la Plataforma universal de Windows (UWP)](index.md)
- [Interacciones con controlador para juegos y control remoto](../input/gamepad-and-remote-interactions.md)
- [Sonido en las aplicaciones para UWP](../style/sound.md)

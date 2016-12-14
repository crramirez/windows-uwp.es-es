---
author: Karl-Bridge-Microsoft
Description: "Habilita el acceso de teclado con la navegación por tabulación y las teclas de acceso para que los usuarios puedan navegar por los elementos de la interfaz de usuario con el teclado."
title: Teclas de acceso
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Access keys
template: detail.hbs
keywords: Teclas de acceso, teclado, accesibilidad
translationtype: Human Translation
ms.sourcegitcommit: 2b6b1d7b1755aad4d75a29413d989c6e8112128a
ms.openlocfilehash: dfe89e4d4fd089dde6b7b307325b8fe43de82c10

---

# <a name="access-keys"></a>Teclas de acceso

Los usuarios que tienen dificultades para usar un mouse, como las personas con discapacidades motrices, a menudo dependen del teclado para navegar por una aplicación e interactuar con ella.  El marco XAML te permite proporcionar acceso de teclado a los elementos de la interfaz de usuario gracias a la navegación mediante tabulación y las teclas de acceso.

- La navegación mediante tabulación es una prestación básica de la accesibilidad de teclado (habilitada de manera predeterminada) que permite a los usuarios mover el foco entre los elementos de la interfaz de usuario con las teclas de dirección y el tabulador del teclado.
- Las teclas de acceso son una prestación de accesibilidad adicional (que se implementa en la aplicación) para permitir un acceso rápido a los comandos de la aplicación mediante una combinación del modificador de teclado (tecla Alt) y una o más claves alfanuméricas (normalmente una letra asociada con el comando). Entre las teclas de acceso habituales se incluyen _Alt+A_ para abrir el menú Archivo y _Alt+AL_ para alinear a la izquierda.  

Para obtener más información sobre la navegación por teclado y la accesibilidad de teclado, consulta [Interacciones de teclado](https://msdn.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) y [Accesibilidad de teclado](https://msdn.microsoft.com/windows/uwp/accessibility/keyboard-accessibility). En este artículo se da por hecho que conoces los conceptos que se tratan en los artículos siguientes.

## <a name="access-key-overview"></a>Información general sobre las teclas de acceso

Las teclas de acceso permiten a los usuarios invocar botones directamente o establecer el foco con el teclado sin tener que presionar repetidamente las teclas de dirección y el tabulador. Las teclas de acceso están concebidas para que resulte fácil descubrirlas, por lo que debes documentarlas directamente en la interfaz de usuario; por ejemplo, una notificación flotante sobre el control con la tecla de acceso.

![Ejemplo de teclas de acceso y sugerencias de teclas asociadas en Microsoft Word](images/keyboard/accesskeys-keytips.png)

_Figura 1: Ejemplo de teclas de acceso y sugerencias de teclas asociadas en Microsoft Word._

Una tecla de acceso es uno o varios caracteres alfanuméricos asociados con un elemento de la interfaz de usuario. Por ejemplo, Microsoft Word usa _O_ para la pestaña Inicio, _2_ para el botón Deshacer, o _JR_ para la pestaña Dibujo.

**Ámbito de las teclas de acceso**

Una tecla de acceso pertenece a un ámbito específico. Por ejemplo, en la figura 1 _F_, _H_, _N_, y _JI_, pertenecen al ámbito de la página.  Cuando el usuario presiona _H_, el ámbito cambia al ámbito de la pestaña Home (Inicio) y se muestran sus teclas de acceso, tal como se muestra en la figura 2. Las teclas de acceso, _V_, _FP_, _FF_, y _FS_ pertenecen al ámbito de la pestaña Home (Inicio).

![Ejemplo de teclas de acceso y sugerencias de teclas asociadas para el ámbito de la pestaña Home (Inicio) en Microsoft Word](images/keyboard/accesskeys-keytips-hometab.png)

_Figura 2: Ejemplo de teclas de acceso y sugerencias de teclas asociadas para el ámbito de la pestaña Inicio en Microsoft Word._

Dos elementos pueden tener las mismas teclas de acceso si los elementos que pertenecen a ámbitos diferentes. Por ejemplo, _2_ es la tecla de acceso para Deshacer en el ámbito de la página (figura 1) y también para cursiva en el ámbito de la pestaña Home (Inicio) (figura 2). Todas las teclas de acceso pertenecen al ámbito predeterminado, a menos que se especifique otro ámbito.

**Secuencia de teclas de acceso**

Por lo general, las combinaciones de teclas de acceso se presionan de una en una para lograr la acción, no se presionan de forma simultánea. (Existe una excepción a esto que se tratará en la siguiente sección). La secuencia de pulsaciones de teclas necesaria para lograr la acción es una _secuencia de teclas de acceso_. El usuario presiona la tecla Alt para iniciar la secuencia de teclas de acceso. Se invoca una tecla de acceso cuando el usuario presiona la última tecla de una secuencia de teclas de acceso. Por ejemplo, para abrir la pestaña Vista en Word, el usuario presiona la secuencia de teclas de acceso _Alt, N_.

Un usuario puede invocar varias teclas de acceso en una secuencia de teclas de acceso. Por ejemplo, para abrir Copiar formato en un documento de Word, el usuario presiona Alt para inicializar la secuencia, a continuación, presiona _O_ para navegar a la sección Inicio y cambiar el ámbito de las teclas de acceso, luego _O_ y, finalmente _O_ otra vez. _O_ y _OO_ son las teclas de acceso para la pestaña Inicio y el botón Copiar formato respectivamente.

Algunos elementos finalizan una secuencia de teclas de acceso después su invocación (como el botón Copiar formato) y otros no (como la pestaña Inicio). Invocar una tecla de acceso puede producir que se ejecute un comando, se mueva el foco, se cambie el ámbito de las teclas de acceso o alguna otra acción asociada con ella.

## <a name="access-key-user-interaction"></a>Interacción del usuario con las teclas de acceso

Para comprender las API de las teclas de acceso, es necesario comprender antes el modelo de interacción del usuario. A continuación encontrarás un resumen del modelo de interacción del usuario con las teclas de acceso:

- Cuando el usuario presiona la tecla Alt, se inicia la secuencia de teclas de acceso, incluso cuando el foco está en un control de entrada. A continuación, el usuario puede presionar la tecla de acceso para invocar la acción asociada. Esta interacción del usuario requiere que documentes las teclas de acceso disponibles en la interfaz de usuario con alguna prestación visual, como notificaciones flotantes que se muestren al presionar la tecla Alt
- Cuando el usuario presiona la tecla Alt y la tecla de acceso simultáneamente, la tecla de acceso se invoca inmediatamente. Esto es parecido a disponer de un método abreviado de teclado definido por Alt +_tecla de acceso_. En este caso, no se muestran las prestaciones visuales de las teclas de acceso. Sin embargo, invocar una tecla de acceso podría producir un cambio del ámbito de las teclas de acceso. En este caso, se inicia una secuencia de teclas de acceso y se muestran las prestaciones visuales del nuevo ámbito.
    > [!NOTE]
    > Únicamente las teclas de acceso de un solo carácter pueden aprovechar esta interacción del usuario. La combinación Alt +_tecla de acceso_ no se admite para las teclas de acceso con más de un carácter.    
- Si hay varias claves de acceso de varios caracteres que comparten algunos caracteres, cuando el usuario presiona un carácter compartido, las teclas de acceso se filtran. Por ejemplo, supongamos que se muestran 3 teclas de acceso: _A1_, _A2_, y _C_. Si el usuario presiona _A_, a continuación solo se mostrarán las teclas de acceso _A1_ y _A2_ y se ocultará la prestación visual de C.
- La tecla Esc quita un nivel de filtrado. Por ejemplo, si tenemos las teclas de acceso _B_, _ABC_, _ACD_, y _ABD_ y el usuario presiona _A_, a continuación, solo se mostrarán _ABC_, _ACD_ y _ABD_. Si luego el usuario presiona _B_, solo se mostrarán _ABC_ y _ABD_. Si el usuario presiona la tecla Esc, se quitará un nivel de filtrado y se mostrarán las teclas de acceso _ABC_, _ACD_ y _ABD_. Si el usuario vuelve a presionar la tecla Esc, se quitará otro nivel de filtrado y todas las teclas de acceso (_B_, _ABC_, _ACD_ y _ABD_) estarán habilitadas y se mostrarán sus prestaciones visuales.
- La tecla Esc retrocede al ámbito anterior. Las teclas de acceso pueden pertenecer a distintos ámbitos para que sea más fácil navegar por las aplicaciones que tengan una gran cantidad de comandos. La secuencia de teclas de acceso comienza siempre en el ámbito principal. Todas las teclas de acceso pertenecen al ámbito principal, excepto aquellas que especifican un elemento concreto de la interfaz de usuario como el propietario de su ámbito. Si el usuario invoca la tecla de acceso de un elemento que sea propietario de un ámbito, el marco XAML mueve el ámbito a ella automáticamente y la agrega a una pila de navegación interna de teclas de acceso. La tecla Esc retrocede por la pila de navegación de teclas de acceso.
- Existen varias maneras de descartar la secuencia de teclas de acceso:
    - El usuario puede presionar Alt para descartar una secuencia de teclas de acceso que esté en curso. Recuerda que presionar Alt también inicia la secuencia de teclas de acceso.
    - La tecla Esc descarta la secuencia de teclas de acceso si se está en el ámbito principal y sin filtrar.
        > [!NOTE]
        > La pulsación de la tecla Esc se pasa a la capa de la interfaz de usuario para controlarla también ahí.
- La tecla de tabulador descarta la secuencia de teclas de acceso y vuelve a la navegación mediante tabulación.
- La tecla Entrar descarta la secuencia de teclas de acceso y envía la pulsación de la tecla al elemento que tiene el foco.
- Las teclas de dirección descartan la secuencia de teclas de acceso y envían la pulsación de la tecla al elemento que tiene el foco.
- Un evento de puntero hacia abajo, como un clic del mouse o una función táctil, descarta la secuencia de teclas de acceso.
- De manera predeterminada, cuando se invoca una tecla de acceso, se descarta la secuencia de teclas de acceso.  Sin embargo, este comportamiento puede invalidarse si se establece la propiedad [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) en **false**.
- Se produce una colisión de teclas de acceso cuando no es posible un autómata limitado determinista. Es preferible que estas colisiones de teclas de acceso no existan, pero pueden producirse debido a un gran número de comandos, a problemas de localización o a la generación en tiempo de ejecución de las teclas de acceso.

 Hay dos casos donde se producen colisiones:
 - Cuando dos elementos de la interfaz de usuario tienen el mismo valor de tecla de acceso y pertenecen al mismo ámbito de teclas de acceso. Por ejemplo, una tecla de acceso _A1_ para un `button1` y una tecla de acceso _A1_ para un `button2` que pertenece al ámbito predeterminado. En este caso, el sistema resuelve la colisión mediante el procesamiento de la tecla de acceso del primer elemento agregado al árbol visual. El resto se omiten.
 - Cuando hay más de una opción de cálculo en el mismo ámbito de teclas de acceso. Por ejemplo, _A_ y _A1_. Cuando el usuario presiona _A_, el sistema tiene dos opciones: invocar la tecla de acceso _A_ o seguir y consumir el carácter A de la tecla de acceso _A1_. En este caso, el sistema procesará solo la invocación de la primera tecla de acceso que alcancen los autómatas. En el ejemplo de _A_ y _A1_, el sistema solo invocará la tecla de acceso _A_.
-   Cuando el usuario presiona un valor de tecla de acceso no válido en una secuencia de teclas de acceso, no sucede nada. Existen dos categorías de teclas que se consideran teclas de acceso válidas en una secuencia de teclas de acceso:
 - Teclas especiales para salir de la secuencia de teclas de acceso; es decir, Esc, Alt, las teclas de dirección, Entrar y la tecla de tabulador.
 - Caracteres alfanuméricos asignados a las teclas de acceso.

## <a name="access-key-apis"></a>Las API de teclas de acceso

Para admitir la interacción del usuario con las teclas de acceso, el marco XAML proporciona las API que se describen aquí.

**AccessKeyManager**

[AccessKeyManager](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.aspx) es una clase auxiliar que sirve para administrar la interfaz de usuario cuando se muestran u ocultan teclas de acceso. El evento [IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) se genera cada vez que la aplicación entra y sale de la secuencia de teclas de acceso. Puedes consultar la propiedad [IsDisplayModeEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabled.aspx) para determinar si las prestaciones visuales se muestran o se ocultan.  También se puede llamar a [ExitDisplayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.exitdisplaymode.aspx) para forzar el descarte de una secuencia de teclas de acceso.

> [!NOTE]
> No existe ninguna implementación integrada del objeto visual de las tecla de acceso; tienes que proporcionarla tú.  

**AccessKey**

La propiedad [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) te permite especificar una tecla de acceso en un UIElement o [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.accesskey.aspx). Si dos elementos tienen la misma tecla de acceso y el mismo ámbito, únicamente se procesará el primer elemento agregado al árbol visual.

Para garantizar que el marco XAML procesa las teclas de acceso, los elementos de la interfaz de usuario deben realizarse en el árbol visual. Si no hay ningún elemento en el árbol visual con una tecla de acceso, no se genera ningún evento de tecla de acceso.

Las API de teclas de acceso no admiten caracteres que necesiten dos pulsaciones de teclas para generarse. Un carácter individual debe corresponderse con una tecla de la distribución del teclado nativa de un idioma en concreto.  

**AccessKeyDisplayRequested/Dismissed**

Los eventos [AccessKeyDisplayRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplayrequested.aspx) y [AccessKeyDisplayDismissed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplaydismissed.aspx) se generan cuando la prestación visual de una tecla de acceso debe mostrarse o descartarse. Estos eventos no se generan para elementos cuya propiedad [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) esté establecida en **Collapsed**. El evento AccessKeyDisplayRequested se genera durante una secuencia de teclas de acceso cada vez que el usuario presiona un carácter que la tecla de acceso usa. Por ejemplo, si se establece una tecla de acceso en _AB_, este evento se genera cuando el usuario presiona la tecla Alt y otra vez cuando el usuario presiona _A_. Cuando el usuario presiona _B_, se genera el evento AccessKeyDisplayDismissed

**AccessKeyInvoked**

El evento [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) se genera cuando un usuario alcanza el último carácter de una tecla de acceso. Una tecla de acceso puede tener uno o varios caracteres. Por ejemplo, para las teclas de acceso _A_ y _BC_, cuando el usuario presiona _Alt, A_, o _Alt, B, C_, se genera el evento, pero no se genera cuando el usuario presiona solo _Alt, B_. Este evento se genera cuando se presiona la tecla, no cuando se suelta.

**IsAccessKeyScope**

La propiedad [IsAccessKeyScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.isaccesskeyscope.aspx) te permite especificar que un UIElement sea la raíz de un ámbito de teclas de acceso. El evento de AccessKeyDisplayRequested se genera para este elemento, pero no para sus elementos secundarios. Cuando un usuario invoca este elemento, el marco XAML cambia automáticamente el ámbito y genera el evento AccessKeyDisplayRequested en sus elementos secundarios y el evento de AccessKeyDisplayDismissed en otros elementos de la interfaz de usuario (incluido al elemento primario).  Cuando se cambia el ámbito, no se sale de la secuencia de teclas de acceso.

**AccessKeyScopeOwner**

Para hacer que un elemento participe en el ámbito de otro elemento (el origen) que no sea su elemento primario del árbol visual, puedes establecer la propiedad [AccessKeyScopeOwner](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyscopeowner.aspx). El elemento enlazado a la propiedad AccessKeyScopeOwner debe tener la propiedad IsAccessKeyScope establecida en **true**. De lo contrario, se producirá una excepción.

**ExitDisplayModeOnAccessKeyInvoked**

De manera predeterminada, cuando se invoca una tecla de acceso y el elemento no es propietario de ningún ámbito, se finaliza la secuencia de teclas de acceso y se genera el evento [AccessKeyManager.IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx). Puedes establecer la propiedad [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) en **false** para invalidar este comportamiento y evitar salir de la secuencia de teclas de acceso después de su invocación. (Esta propiedad está tanto en [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) como en [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.exitdisplaymodeonaccesskeyinvoked.aspx)).

> [!NOTE]
> Si el elemento es propietario de un ámbito (`IsAccessKeyScope="True"`), la aplicación entra en un nuevo ámbito de teclas de acceso y el evento IsDisplayModeEnabledChanged no se genera.

**Localización**

Las teclas de acceso pueden localizarse en varios idiomas y cargarse en el tiempo de ejecución mediante la API [ResourceLoader](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.resources.resourceloader.aspx).

## <a name="control-patterns-used-when-an-access-key-is-invoked"></a>Patrones de control usados cuando se invoca una tecla de acceso

Los patrones de control son implementaciones de la interfaz que exponen la funcionalidad de control común; por ejemplo, unos botones implementan el patrón de control **Invoke** y esto provoca el evento **Click**. Cuando se invoca una tecla de acceso, el marco XAML busca si el elemento invocado implementa un patrón de control y, si es así, lo ejecuta. Si el elemento tiene más de un patrón de control, se invoca solo uno de ellas y se omite el resto. Los patrones de control se buscan en el siguiente orden:

1.  Invocar. Por ejemplo, un botón (Button).
2.  Alternancia. Por ejemplo, una casilla (Checkbox).
3.  Selección. Por ejemplo, un botón de selección (RadioButton).
4.  Ampliar/contraer. Por ejemplo, un cuadro combinado (ComboBox).

Si no se encuentra ningún patrón de control, la invocación de la tecla de acceso aparecerá como sin opciones y se registrará un mensaje de depuración para ayudarte a depurar esta situación: "No automation patterns for this component found. (No se han encontrado patrones de automatización para este componente). Implement desired behavior in the event handler for AccessKeyInvoked. (Implementa el comportamiento deseado en el controlador de eventos de AccessKeyInvoked). Setting Handled to true in your event handler will suppress this message. (Si Handled se establece en true en el controlador de eventos, este mensaje se suprimirá)".

> [!NOTE]
> Para ver este mensaje, el tipo de proceso de la aplicación del depurador debe ser _Mixto (administrado y nativo)_ o _Nativo_ en la configuración de depuración de Visual Studio.

Si no deseas que una tecla de acceso ejecute su patrón de control predeterminado, o si el elemento no posee ningún patrón de control, debes controlar el evento [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) e implementar el comportamiento deseado.
```csharp
private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
{
    args.Handled = true;
    //Do something
}
```

Para obtener más información sobre los patrones de control, consulta [UI Automation Control Patterns Overview](https://msdn.microsoft.com/library/windows/desktop/ee671194.aspx) (Introducción a los patrones de control de automatización de la interfaz de usuario).

## <a name="access-keys-and-narrator"></a>Teclas de acceso y Narrador

Windows Runtime posee proveedores de automatización de la interfaz de usuario que exponen las propiedades de los elementos de automatización de la interfaz de usuario de Microsoft. Estas propiedades permiten a las aplicaciones cliente de automatización de la interfaz de usuario obtener información sobre algunas partes de la interfaz de usuario. La propiedad [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) permite a los clientes (por ejemplo, a Narrador) descubrir la tecla de acceso asociada con un elemento. Narrador leerá esta propiedad cada vez que un el foco se sitúe sobre un elemento. Si AutomationProperties.AccessKey no tiene ningún valor, el marco XAML devuelve el valor de la propiedad [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) del UIElement o TextElement. No es necesario configurar AutomationProperties.AccessKey si la propiedad AccessKey ya posee un valor.

## <a name="example-access-key-for-button"></a>Ejemplo: Tecla de acceso de un botón

Este ejemplo muestra cómo crear una tecla de acceso de un botón. Usa la información sobre herramientas como una prestación visual para implementar una notificación flotante que contenga la tecla de acceso.

> [!NOTE]
> La información sobre herramientas se usa por motivos de sencillez, pero te recomendamos crear tu propio control para mostrarla mediante, por ejemplo, la clase [Popup](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.popup.aspx).

El marco XAML llama automáticamente al controlador del evento Click, por lo que no es necesario que controles el evento AccessKeyInvoked. El ejemplo proporciona prestaciones visuales únicamente para los caracteres que quedan para invocar la tecla de acceso mediante la propiedad [AccessKeyDisplayRequestedEventArgs.PressedKeys](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeydisplayrequestedeventargs.pressedkeys.aspx). Por ejemplo, si se muestran tres teclas de acceso, _A1_, _A2_ y _C_, y el usuario presiona _A_, solo las teclas de acceso _A1_ y _A2_ se quedarán sin filtrar y se mostrarán como _1_ y _2_, en lugar de como _A1_ y _A2_.

```xaml
<StackPanel
        VerticalAlignment="Center"
        HorizontalAlignment="Center"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Button Content="Press"
                AccessKey="PB"
                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                Click="DoSomething" />
        <TextBlock Text="" x:Name="textBlock" />
    </StackPanel>
```

```csharp
 public sealed partial class ButtonSample : Page
    {
        public ButtonSample()
        {
            this.InitializeComponent();
        }

        private void DoSomething(object sender, RoutedEventArgs args)
        {
            textBlock.Text = "Access Key is working!";
        }

        private void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(sender, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        private void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(sender, null);
            }
        }
    }
```

## <a name="example-scoped-access-keys"></a>Ejemplo: Teclas de acceso con ámbito

Este ejemplo muestra cómo crear teclas de acceso con ámbito. La propiedad IsAccessKeyScope de PivotItem impide que se muestren las teclas de acceso de los elementos secundarios de PivotItem cuando el usuario presiona Alt. Estas teclas de acceso se muestran solamente cuando el usuario invoca el PivotItem, porque el marco XAML cambia el ámbito automáticamente. El marco también oculta las teclas de acceso de los otros ámbitos.

En este ejemplo también se muestra cómo controlar el evento AccessKeyInvoked. El PivotItem no implementa ningún patrón de control, por lo que el marco XAML no invoca ninguna acción de forma predeterminada. Esta implementación muestra cómo seleccionar el PivotItem que se haya invocado mediante la tecla de acceso.

Por último, el ejemplo muestra el evento IsDisplayModeChanged, donde puedes hacer algo cuando cambia el modo de presentación. En este ejemplo, el control Pivot está contraído hasta que el usuario presiona Alt. Cuando el usuario termina de interactuar con Pivot, se contrae nuevamente. Puedes usar IsDisplayModeEnabled para comprobar si el modo de presentación de la tecla de acceso está habilitado o deshabilitado.

```xaml   
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Pivot x:Name="MyPivot" VerticalAlignment="Center" HorizontalAlignment="Center" >
            <Pivot.Items>
                <PivotItem
                    x:Name="PivotItem1"
                    AccessKey="A"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    IsAccessKeyScope="True">
                    <PivotItem.Header>
                        <TextBlock Text="A Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal" >
                        <Button Content="ButtonAA" AccessKey="A"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested" />
                        <Button Content="ButtonAD1" AccessKey="D1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button Content="ButtonAD2" AccessKey="D2"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
                <PivotItem
                    x:Name="PivotItem2"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    AccessKey="B"
                    IsAccessKeyScope="true">
                    <PivotItem.Header>
                        <TextBlock Text="B Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal">
                        <Button AccessKey="B" Content="ButtonBB"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F1" Content="ButtonBF1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F2" Content="ButtonBF2"  
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
            </Pivot.Items>
        </Pivot>
    </Grid>
```

```csharp
public sealed partial class ScopedAccessKeys : Page
    {
        public ScopedAccessKeys()
        {
            this.InitializeComponent();
            AccessKeyManager.IsDisplayModeEnabledChanged += OnDisplayModeEnabledChanged;
            this.Loaded += OnLoaded;
        }

        void OnLoaded(object sender, object e)
        {
            //To let the framework discover the access keys, the elements should be realized
            //on the visual tree. If there are no elements in the visual
            //tree with access key, the framework won't raise the events.
            //In this sample, if you define the Pivot as collapsed on the constructor, the Pivot
            //will have a lazy loading and the access keys won't be enabled.
            //For this reason, we make it visible when creating the object
            //and we collapse it when we load the page.
            MyPivot.Visibility = Visibility.Collapsed;
        }

        void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
        {
            args.Handled = true;
            MyPivot.SelectedItem = sender as PivotItem;
        }
        void OnDisplayModeEnabledChanged(object sender, object e)
        {
            if (AccessKeyManager.IsDisplayModeEnabled)
            {
                MyPivot.Visibility = Visibility.Visible;
            }
            else
            {
                MyPivot.Visibility = Visibility.Collapsed;

            }
        }

        DependencyObject AdjustTarget(UIElement sender)
        {
            DependencyObject target = sender;
            if (sender is PivotItem)
            {
                PivotItem pivotItem = target as PivotItem;
                target = (sender as PivotItem).Header as TextBlock;
            }
            return target;
        }

        void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);
            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(target, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);

            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(target, null);
            }
        }
    }
```



<!--HONumber=Dec16_HO1-->



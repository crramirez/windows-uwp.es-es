---
Description: Obtenga información sobre cómo las teclas de aceleración pueden mejorar la facilidad de uso y la accesibilidad de las aplicaciones de Windows.
title: Aceleradores de teclado
label: Keyboard accelerators
template: detail.hbs
keywords: teclado, acelerador, tecla de aceleración, métodos abreviados de teclado, accesibilidad, navegación, foco, texto, entrada, interacciones del usuario, controlador de juegos, remoto
ms.date: 10/10/2017
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 785047b3ee5f18fa4f6ea8fd78f6d8ab7a92e8e6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173429"
---
# <a name="keyboard-accelerators"></a>Aceleradores de teclado

![Teclado Surface](images/accelerators/accelerators_hero2.png)

Las teclas de aceleración (o aceleradores de teclado) son métodos abreviados de teclado que mejoran la facilidad de uso y la accesibilidad de las aplicaciones Windows al proporcionar una manera intuitiva de que los usuarios invoquen acciones o comandos comunes sin navegar por la interfaz de usuario de la aplicación.

Consulte el tema [claves de acceso](access-keys.md) para obtener más información sobre cómo navegar por la interfaz de usuario de una aplicación de Windows con métodos abreviados de teclado.

> [!NOTE]
> Un teclado es indispensable para los usuarios con ciertas discapacidades (vea [accesibilidad del teclado](../accessibility/keyboard-accessibility.md)) y también es una herramienta importante para los usuarios que lo prefieren como una forma más eficaz de interactuar con una aplicación.

## <a name="overview"></a>Información general

Normalmente, los aceleradores incluyen las teclas de función F1 a F12 o alguna combinación de una tecla estándar emparejada con una o más teclas modificadoras (CTRL, Mayús).

> [!NOTE]
> Los controles de plataforma UWP tienen aceleradores de teclado integrados. Por ejemplo, ListView admite Ctrl + A para seleccionar todos los elementos de la lista y RichEditBox admite Ctrl + Tab para insertar una pestaña en el cuadro de texto. Estos aceleradores de teclado integrados se conocen como **aceleradores de control** y solo se ejecutan si el foco está en el elemento o en uno de sus elementos secundarios. Los aceleradores definidos por el usuario mediante las API de acelerador de teclado que se describen aquí se denominan **aceleradores de aplicaciones**.

Los aceleradores de teclado no están disponibles para todas las acciones, pero suelen estar asociados a los comandos expuestos en los menús (y se deben especificar con el contenido del elemento de menú).Los aceleradores también se pueden asociar a acciones que no tienen elementos de menú equivalentes. Sin embargo, dado que los usuarios se basan en los menús de una aplicación para detectar y obtener información sobre el conjunto de comandos disponible, debe intentar realizar la detección de los aceleradores lo más fácilmente posible (con etiquetas o patrones establecidos puede ayudarle con esto).

![Aceleradores de teclado descritos en una etiqueta de elemento de menú](images/accelerators/accelerators_menuitemlabel.png)  
*Aceleradores de teclado descritos en una etiqueta de elemento de menú*

## <a name="when-to-use-keyboard-accelerators"></a>Cuándo usar aceleradores de teclado

Se recomienda especificar los aceleradores de teclado siempre que sea adecuado en la interfaz de usuario y los aceleradores de soporte en todos los controles personalizados.

- Los aceleradores de teclado hacen que la aplicación sea más accesible para los usuarios con discapacidades del motor, incluidos aquellos usuarios que pueden presionar solo una tecla a la vez o tener dificultades para usar un mouse. * *

  Una interfaz de usuario de teclado bien diseñada es un aspecto importante de la accesibilidad del software. Permite a usuarios con dificultades visuales o con ciertas discapacidades motrices navegar por una aplicación e interactuar con sus funciones. Es posible que estos usuarios no puedan controlar un mouse y empleen varias tecnologías de ayuda, como herramientas para la mejora del teclado, teclados en pantalla, ampliadores de pantallas, lectores de pantalla y utilidades de entrada de voz. Para estos usuarios, la cobertura de comandos completa es fundamental.

- Los aceleradores de teclado hacen que la aplicación sea más fácil de usar para los usuarios avanzados que prefieren interactuar a través del teclado.

  Los usuarios con experiencia suelen tener una gran preferencia para usar el teclado, ya que los comandos basados en teclado se pueden escribir más rápidamente y no requieren que quiten las manos del teclado. Para estos usuarios, la eficacia y la coherencia son cruciales; la exhaustividad es importante solo para los comandos usados con más frecuencia.

## <a name="specify-a-keyboard-accelerator"></a>Especificar una tecla de aceleración

Use las API de [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) para crear aceleradores de teclado en aplicaciones para UWP. Con estas API, no tiene que controlar varios eventos KeyDown para detectar la combinación de teclas presionada y puede localizar los aceleradores en los recursos de la aplicación.

Se recomienda establecer aceleradores de teclado para las acciones más comunes en la aplicación y documentarlos mediante la etiqueta del elemento de menú o la información sobre herramientas. En este ejemplo, se declaran los aceleradores de teclado solo para los comandos Rename y Copy.

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![Acelerador de teclado descrito en una información sobre herramientas](images/accelerators/accelerators_tooltip.png)  
***Acelerador de teclado descrito en una información sobre herramientas***

El objeto [UIElement](/uwp/api/windows.ui.xaml.uielement) tiene una colección [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) , [KeyboardAccelerators](/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators), donde se especifican los objetos KeyboardAccelerator personalizados y se definen las pulsaciones de tecla para la tecla de aceleración:

-   **[Clave](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** : [VirtualKey](/uwp/api/windows.system.virtualkey) que se usa para la tecla de aceleración.

-   **[Modificadores](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** : [VirtualKeyModifiers](/uwp/api/windows.system.virtualkeymodifiers) que se usa para la tecla de aceleración. Si no se establece el modificador, el valor predeterminado es None.

> [!NOTE]
> Se admiten los aceleradores de clave única (A, eliminación, F2, barra espaciadora, ESC, clave multimedia) y aceleradores de varias teclas (Ctrl + Mayús + M). Sin embargo, no se admiten las teclas virtuales del controlador de juegos.

## <a name="scoped-accelerators"></a>Aceleradores con ámbito

Algunos aceleradores solo funcionan en ámbitos específicos mientras que otros trabajan en toda la aplicación.

Por ejemplo, Microsoft Outlook incluye los siguientes aceleradores:
-   CTRL + B, Ctrl + I y ESC funcionan solo en el ámbito del formulario de envío de correo electrónico
-   Ctrl + 1 y Ctrl + 2 funcionan en toda la aplicación

### <a name="context-menus"></a>Menús contextuales

Las acciones del menú contextual solo afectan a áreas o elementos concretos, como los caracteres seleccionados en un editor de texto o una canción de una lista de reproducción. Por esta razón, se recomienda establecer el ámbito de los aceleradores de teclado para los elementos de menú contextual en el elemento primario del menú contextual.

Use la propiedad [ScopeOwner](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) para especificar el ámbito de la tecla de aceleración. En este código se muestra cómo implementar un menú contextual en un control ListView con aceleradores de teclado de ámbito:

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

El atributo ScopeOwner del elemento MenuFlyoutItem. KeyboardAccelerators marca el acelerador como con ámbito en lugar de global (el valor predeterminado es null o global). Para obtener más información, vea la sección **resolución de aceleradores** más adelante en este tema.

## <a name="invoke-a-keyboard-accelerator"></a>Invocar una tecla de aceleración 

El objeto [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) usa el [patrón de control de UI Automation (UIA)](/windows/desktop/WinAuto/uiauto-controlpatternsoverview) para tomar medidas cuando se invoca un acelerador.

Los [patrones de control] de UIA exponen la funcionalidad de control común. Por ejemplo, el control de botón implementa el patrón de control [Invoke](/windows/desktop/WinAuto/uiauto-implementinginvoke) para admitir el evento click (normalmente, un control se invoca haciendo clic, haciendo doble clic o presionando entrar, un método abreviado de teclado predefinido o alguna otra combinación de pulsaciones de teclas). Cuando se usa una tecla de aceleración para invocar un control, el marco XAML busca si el control implementa el patrón de control Invoke y, en ese caso, lo activa (no es necesario escuchar el evento KeyboardAcceleratorInvoked).

En el ejemplo siguiente, el control + S desencadena el evento de clic porque el botón implementa el patrón de invocación.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Si un elemento implementa varios patrones de control, solo se puede activar uno a través de un acelerador. Los patrones de control se priorizan de la manera siguiente:
1.  Invocar (botón)
2.  Alternar (casilla)
3.  Selección (ListView)
4.  Expandir o contraer (ComboBox) 

Si no se identifica ninguna coincidencia, el acelerador no es válido y se proporciona un mensaje de depuración ("no se encontraron patrones de automatización para este componente. Implemente todo el comportamiento deseado en el evento invocado. Si se establece en true en el controlador de eventos, se suprime este mensaje. ")

## <a name="custom-keyboard-accelerator-behavior"></a>Comportamiento del acelerador de teclado personalizado

El evento invocado del objeto [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) se activa cuando se ejecuta el acelerador. El objeto de evento [KeyboardAcceleratorInvokedEventArgs](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) incluye las siguientes propiedades:

- [**Handled**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.handled) (booleano): si se establece en true, se impide que el evento desencadene el patrón de control y se detenga la propagación de eventos del acelerador. El valor predeterminado es false.
- [**Elemento**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.element) (DependencyObject): el objeto asociado al acelerador.
- [**KeyboardAccelerator**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.keyboardaccelerator): la tecla de aceleración utilizada para generar el evento invocado.

Aquí se muestra cómo definir una colección de aceleradores de teclado para los elementos de un control ListView y cómo controlar el evento invocado para cada acelerador.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>
```

``` csharp
void SelectAllInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectAll();
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectionMode = ListViewSelectionMode.None;
  MyListView.SelectionMode = ListViewSelectionMode.Multiple;
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>Invalidar el comportamiento predeterminado del teclado

Algunos controles, cuando tienen el foco, admiten los aceleradores de teclado integrados que invalidan cualquier acelerador definido por la aplicación. Por ejemplo, cuando un [TextBox](/uwp/api/windows.ui.xaml.controls.textbox) tiene el foco, el acelerador control + C solo copia el texto seleccionado actualmente (los aceleradores definidos por la aplicación se omiten y no se ejecuta ninguna otra funcionalidad).

Aunque no se recomienda invalidar los comportamientos de control predeterminados debido a la familiaridad y a las expectativas del usuario, puede invalidar la tecla de aceleración de un control integrada. En el ejemplo siguiente se muestra cómo invalidar el acelerador del teclado de control + C para un [cuadro de texto](/uwp/api/windows.ui.xaml.controls.textbox) a través del controlador de eventos [PreviewKeyDown](/uwp/api/windows.ui.xaml.uielement.previewkeydown) : 

``` csharp
 private void TextBlock_PreviewKeyDown(object sender, KeyRoutedEventArgs e)
 {
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(Windows.System.VirtualKey.Control);
    var isCtrlDown = ctrlState == CoreVirtualKeyStates.Down || ctrlState 
        ==  (CoreVirtualKeyStates.Down | CoreVirtualKeyStates.Locked);
    if (isCtrlDown && e.Key == Windows.System.VirtualKey.C)
    {
        // Your custom keyboard accelerator behavior.
        
        e.Handled = true;
    }
 }
```  

## <a name="disable-a-keyboard-accelerator"></a>Deshabilitar una tecla de aceleración 

Si un control está deshabilitado, el acelerador asociado también se deshabilita. En el ejemplo siguiente, dado que la propiedad IsEnabled de ListView está establecida en false, no se puede invocar el control asociado + un acelerador.

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A"
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

Los controles primarios y secundarios pueden compartir el mismo acelerador. En este caso, se puede invocar el control primario aunque el elemento secundario tenga el foco y se deshabilite su acelerador.

## <a name="screen-readers-and-keyboard-accelerators"></a>Lectores de pantalla y aceleradores de teclado 

Los lectores de pantalla como narrador pueden anunciar la combinación de teclas de aceleración de teclado a los usuarios. De forma predeterminada, este es el modificador (en el orden de enumeración VirtualModifiers) seguido de la clave (y separados por signos "+"). Puede personalizarlo a través de la propiedad adjunta [AcceleratorKey](/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties. Si se especifica más de un acelerador, solo se anuncia el primero.

En este ejemplo, AutomationProperty. AcceleratorKey devuelve la cadena "Control + Mayús + A":

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> Al establecer AutomationProperties. AcceleratorKey no se habilita la funcionalidad del teclado, solo se indica a UIA Framework qué claves se usan.

## <a name="common-keyboard-accelerators"></a>Aceleradores de teclado comunes

Se recomienda que los aceleradores de teclado sean coherentes en todas las aplicaciones de Windows. Los usuarios deben memorizar los aceleradores de teclado y esperar los mismos resultados (o similares).

Esto podría no ser siempre posible debido a las diferencias de funcionalidad entre las aplicaciones.

| **Edición** | **Acelerador de teclado común** |
| ------------- | ----------------------------------- |
| Iniciar el modo de edición | Ctrl + E |
| Seleccionar todos los elementos de un control o ventana con foco | CTRL+A |
| Buscar y reemplazar | Ctrl + H |
| Deshacer | CTRL+Z |
| Rehacer | CTRL+Y |
| Eliminar la selección y copiarla en el portapapeles | Ctrl + X |
| Copiar la selección en el portapapeles | Ctrl + C, Ctrl + Insert |
| Pegar el contenido del portapapeles | Ctrl + V, Mayús + insertar |
| Pegar el contenido del portapapeles (con opciones) | Ctrl + Alt + V |
| Cambiar el nombre de un elemento | F2 |
| Incorporación de un nuevo elemento | Ctrl + N |
| Agregar un nuevo elemento secundario | Ctrl + Mayús + N |
| Eliminar elemento seleccionado (con deshacer) | Del, Ctrl + D |
| Eliminar elemento seleccionado (sin deshacer) | Mayús + Supr |
| Negrita | Ctrl + B |
| Subrayado | Ctrl + U |
| Cursiva | Ctrl+I |

| **Navegación** | |
| ------------- | ----------------------------------- |
| Buscar contenido en un control o ventana prioritarios | Ctrl + F |
| Ir al siguiente resultado de la búsqueda | F3 |

| **Otras acciones** | |
| ------------- | ----------------------------------- |
| Agregar favoritos | Ctrl+D | 
| Actualizar | F5 o Ctrl + R | 
| Acercar | Ctrl + + | 
| Alejamiento | Ctrl +- | 
| Zoom a la vista predeterminada | Ctrl + 0 | 
| Guardar | Ctrl + S | 
| Cerrar | Ctrl + W | 
| Impresión | Ctrl + P | 

Tenga en cuenta que algunas de las combinaciones no son válidas para las versiones traducidas de Windows. Por ejemplo, en la versión en Español de Windows, se usa Ctrl + N para negrita en lugar de CTRL + B. Se recomienda proporcionar aceleradores de teclado localizados si la aplicación está localizada.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Prestaciones de facilidad de uso para los aceleradores de teclado

### <a name="tooltips"></a>Información sobre herramientas

Como los aceleradores de teclado no se describen normalmente directamente en la interfaz de usuario de la aplicación de Windows, puede mejorar la capacidad de detección a través de la [información sobre herramientas](../controls-and-patterns/tooltips.md), que se muestra automáticamente cuando el usuario mueve el foco a, presiona y mantiene el puntero del mouse sobre un control. La información sobre herramientas puede identificar si un control tiene una tecla de aceleración de teclado asociada y, en caso afirmativo, qué es la combinación de teclas de aceleración.

**Windows 10, versión 1803 (actualización de abril de 2018) y versiones más recientes**

De forma predeterminada, cuando se declaran los aceleradores de teclado, todos los controles (excepto [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) y [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) presentan las combinaciones de teclas correspondientes en una información sobre herramientas.

> [!NOTE] 
> Si un control tiene más de un acelerador definido, solo se presenta el primero.

![Información sobre herramientas de tecla de aceleración](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Cuadro combinado de tecla de aceleración en la información sobre herramientas*

En el caso de los objetos [Button](/uwp/api/windows.ui.xaml.controls.button), [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)y [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) , la tecla de aceleración se anexa a la información sobre herramientas predeterminada del control. En el caso de los objetos [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.appbarbutton) y [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)), se muestra la tecla de aceleración con el texto de control flotante.

> [!NOTE]
> Al especificar una información sobre herramientas (vea button1 en el ejemplo siguiente) se invalida este comportamiento.

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![Información sobre herramientas de tecla de aceleración](images/accelerators/accelerators-button-small.png)

*Combinación de tecla de aceleración anexada a la información sobre herramientas predeterminada del botón*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![Información sobre herramientas de tecla de aceleración](images/accelerators/accelerators-appbarbutton-small.png)

*Combinación de tecla de aceleración anexada a la información sobre herramientas predeterminada de AppBarButton*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![Información sobre herramientas de tecla de aceleración](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Combinación de tecla de aceleración anexada al texto de MenuFlyoutItem*

Controlar el comportamiento de la presentación mediante la propiedad [KeyboardAcceleratorPlacementMode](/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) , que acepta dos valores: [auto](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) u [Hidden](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode).    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

En algunos casos, es posible que tenga que presentar una información sobre herramientas en relación con otro elemento (normalmente un objeto contenedor). Por ejemplo, un control Pivot que muestra la información sobre herramientas de un PivotItem con el encabezado de pivote. 

Aquí se muestra cómo usar la propiedad KeyboardAcceleratorPlacementTarget para mostrar la combinación de teclas de aceleración del teclado de un botón Guardar con el contenedor de cuadrícula en lugar del botón.

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>Etiquetas

En algunos casos, se recomienda usar la etiqueta de un control para identificar si el control tiene una tecla de aceleración de teclado asociada y, en caso afirmativo, qué es la combinación de teclas de aceleración. 

Algunos controles de plataforma hacen esto de forma predeterminada, en concreto, los objetos [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) y [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) , mientras que la [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton) y [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) lo hacen cuando aparecen en el menú de desbordamiento del control [CommandBar](/uwp/api/windows.ui.xaml.controls.commandbar).

![Aceleradores de teclado descritos en una etiqueta de elemento de menú](images/accelerators/accelerators_menuitemlabel.png)  
*Aceleradores de teclado descritos en una etiqueta de elemento de menú*

Puede invalidar el texto del acelerador predeterminado de la etiqueta a través de la propiedad [KeyboardAcceleratorTextOverride](/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) de los controles [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)y [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) (usar un solo espacio para ningún texto). 

> [!NOTE] 
> No se presenta el texto de invalidación si el sistema no puede detectar un teclado adjunto (puede comprobarlo mediante la propiedad [KeyboardPresent](/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) ).

## <a name="advanced-concepts"></a>Conceptos avanzados

Aquí se revisan algunos aspectos de bajo nivel de los aceleradores de teclado.

### <a name="when-an-accelerator-is-invoked"></a>Cuando se invoca un acelerador

Los aceleradores se componen de dos tipos de claves: modificadores y no modificadores. Las teclas modificadoras incluyen Mayús, menú, control y la tecla Windows, que se exponen a través de [VirtualKeyModifiers](/uwp/api/Windows.System.VirtualKeyModifiers). Los no modificadores son cualquier clave virtual, como eliminar, F3, barra espaciadora, ESC, y todas las claves de puntuación y alfanuméricas. Se invoca una tecla de aceleración cuando el usuario presiona una tecla no modificadora mientras presiona y mantiene una o más teclas modificadoras. Por ejemplo, si el usuario presiona Ctrl + Mayús + M, cuando el usuario presiona M, el marco comprueba los modificadores (Ctrl y Mayús) y activa el acelerador, si existe.

> [!NOTE]
> Por diseño, el acelerador se repite con frecuencia (por ejemplo, cuando el usuario presiona Ctrl + Mayús y, a continuación, mantiene presionada la tecla M, el acelerador se invoca repetidamente hasta que se suelta M). Este comportamiento no se puede modificar.

### <a name="input-event-priority"></a>Prioridad del evento de entrada
Los eventos de entrada se producen en una secuencia específica que puede interceptar y controlar en función de los requisitos de la aplicación. 

#### <a name="the-keydownkeyup-bubbling-event"></a>Evento de propagación KeyDown/KeyUp 

En XAML, una pulsación de tecla se procesa como si solo hubiera una canalización de propagación de entrada. La canalización de entrada se usa en los eventos KeyDown/KeyUp y en la entrada de caracteres. Por ejemplo, si un elemento tiene el foco y el usuario presiona una tecla abajo, se genera un evento KeyDown en el elemento, seguido del elemento primario del elemento, y así sucesivamente en el árbol, hasta el objeto args. La propiedad controlada es true.

Algunos controles también usan el evento KeyDown para implementar los aceleradores de control integrados. Cuando un control tiene una tecla de aceleración, controla el evento KeyDown, lo que significa que no habrá una propagación de eventos KeyDown. Por ejemplo, el RichEditBox admite la copia con Ctrl + C. Cuando se presiona Ctrl, se desencadena el evento KeyDown y se produce una burbuja, pero cuando el usuario presiona C al mismo tiempo, el evento KeyDown se marca como controlado y no se genera (a menos que el parámetro handledEventsToo de [UIElement. AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler) esté establecido en true).

#### <a name="the-characterreceived-event"></a>El evento CharacterReceived

Como el evento [CharacterReceived](/uwp/api/windows.ui.core.corewindow.CharacterReceived) se desencadena después del evento [KeyDown](/uwp/api/windows.ui.core.corewindow.KeyDown) para controles de texto como TextBox, puede cancelar la entrada de caracteres en el controlador de eventos KeyDown.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Los eventos PreviewKeyDown y PreviewKeyUp

Los eventos de entrada de la vista previa se activan antes que otros eventos. Si no controla estos eventos, se activa el acelerador del elemento que tiene el foco, seguido del evento KeyDown. Ambos eventos se propagan hasta que se administran.


![Secuencia de eventos Key Event Sequence ](images/accelerators/accelerators_keyevents.png)
 ***key***

Orden de eventos:

Vista previa de eventos KeyDown...
El método OnKeyDown de acelerador de aplicaciones aceleradores de aplicaciones de eventos KeyDown en el método OnKeyDown primario del evento KeyDown primario en el elemento primario (se propaga a la raíz)...
Evento CharacterReceived eventos PreviewKeyUp KeyUpEvents

Cuando se controla el evento de acelerador, el evento KeyDown también se marca como controlado. El evento KeyUp permanece no controlado.

### <a name="resolving-accelerators"></a>Resolución de aceleradores

Un evento de acelerador de teclado se propaga desde el elemento que tiene el foco hasta la raíz. Si no se controla el evento, el marco XAML busca otros aceleradores de aplicaciones sin ámbito fuera de la ruta de acceso de propagación.

Cuando se definen dos aceleradores de teclado con la misma combinación de teclas, se invoca la primera tecla de aceleración que se encuentra en el árbol visual.

Los aceleradores de teclado de ámbito solo se invocan cuando el foco está dentro de un ámbito específico. Por ejemplo, en una cuadrícula que contenga docenas de controles, se puede invocar una tecla de aceleración para un control solo cuando el foco está dentro de la cuadrícula (el propietario del ámbito).

### <a name="scoping-accelerators-programmatically"></a>Los aceleradores de ámbito mediante programación

El método [UIElement. TryInvokeKeyboardAccelerator](/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) invoca todos los aceleradores coincidentes en el subárbol del elemento.

El método [UIElement. OnProcessKeyboardAccelerators](/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) se ejecuta antes que la tecla de aceleración. Este método pasa un objeto [ProcessKeyboardAcceleratorArgs](/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) que contiene la clave, el modificador y un valor booleano que indica si se controla la tecla de aceleración. Si se marca como controlado, el acelerador de teclado se propaga (por lo que nunca se invoca la tecla de aceleración de teclado exterior).

> [!NOTE]
> OnProcessKeyboardAccelerators siempre se activa, ya sea controlado o no (similar al evento OnKeyDown). Debe comprobar si el evento se marcó como controlado.

En este ejemplo, usamos OnProcessKeyboardAccelerators y TryInvokeKeyboardAccelerator para limitar el ámbito de los aceleradores de teclado al objeto de página:

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>Localizar los aceleradores

Se recomienda localizar todos los aceleradores de teclado. Puede hacerlo con el archivo de recursos estándar de UWP (. resw) y el atributo x:Uid en las declaraciones de XAML. En este ejemplo, el Windows Runtime carga automáticamente los recursos.

![Localización del acelerador de teclado con el archivo de recursos de UWP ](images/accelerators/accelerators_localization.png)
 ***localización del acelerador de teclado de archivo con recursos de UWP***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Configuración de un acelerador mediante programación

Este es un ejemplo de definición de un acelerador mediante programación:

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked += handler;
    this.KeyboardAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator no se puede compartir, no se puede Agregar el mismo KeyboardAccelerator a varios elementos.

### <a name="override-keyboard-accelerator-behavior"></a>Invalidar el comportamiento del acelerador del teclado

Puede controlar el evento [KeyboardAccelerator. Invoke](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) para invalidar el comportamiento predeterminado de KeyboardAccelerator.

En este ejemplo se muestra cómo invalidar el comando "seleccionar todo" (Ctrl + A acelerador de teclado) en un control ListView personalizado. También se establece la propiedad Handled en true para detener el evento de propagación.

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de teclado](keyboard-interactions.md)
- [Claves de acceso](access-keys.md)

### <a name="samples"></a>Ejemplos

- [XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery)
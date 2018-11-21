---
author: Karl-Bridge-Microsoft
Description: Learn how accelerator keys can improve the usability and accessibility of UWP apps.
title: Aceleradores de teclado
label: Keyboard accelerators
template: detail.hbs
keywords: teclado, acelerador, tecla de aceleración, accesos directos de teclado, accesibilidad, navegación, foco, texto, entrada, interacciones del usuario, controlador para juegos, control remoto
ms.author: kbridge
ms.date: 10/10/2017
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: dcbb27a87b48a124fe4463578bc32d908f399ccb
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7429027"
---
# <a name="keyboard-accelerators"></a>Aceleradores de teclado

![Surface Keyboard](images/accelerators/accelerators_hero2.png)

Las teclas de aceleración (o aceleradores de teclado) son accesos directos de teclado que mejoran la facilidad de uso y la accesibilidad de tus aplicaciones de Windows ya que ofrecen a los usuarios una forma intuitiva de invocar acciones comunes o comandos sin invocar la interfaz de usuario de la aplicación.

Consulta el tema [Teclas de acceso](access-keys.md) para obtener más información sobre cómo navegar por la interfaz de usuario de una aplicación Windows con métodos abreviados de teclado.

> [!NOTE]
> Un teclado es indispensable para los usuarios que tienen ciertas discapacidades (véase [Accesibilidad del teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)) y también es una herramienta importante para los usuarios que lo prefieren como una forma más eficaz de interactuar con una aplicación.

## <a name="overview"></a>Introducción

Normalmente, los aceleradores incluyen las teclas de función F1a F12 o alguna combinación de una tecla estándar emparejada con una o más teclas modificadoras (CTRL, Mayús).

> [!NOTE]
> Los controles de la plataforma UWP tienen aceleradores de teclado integrados. Por ejemplo, ListView admite CTRL+A para seleccionar todos los elementos de la lista y RichEditBox admite CTRL+TAB para insertar una tabulación en el cuadro de texto. Estos aceleradores de teclado integrado se conocen como **aceleradores de control** y se ejecutan solo si el foco se encuentra en el elemento o en uno de sus elementos secundarios. Los aceleradores definidos por el usuario mediante las API aceleradoras de teclado tratadas aquí se conocen como **aceleradores de la aplicación**.

Los aceleradores de teclado no están disponibles para todas las acciones pero se asocian a menudo con los comandos que se exponen en menús (y se deben especificar con el contenido del elemento de menú).Los aceleradores también pueden asociarse a acciones que no tienen elementos de menú equivalentes. Sin embargo, dado que los usuarios dependen de los menús de una aplicación para descubrir y aprender el conjunto de comandos disponibles, debes intentar descubrir los aceleradores de la manera más sencilla posible (el uso de etiquetas o patrones establecidos puede ayudar con esto).

![Aceleradores de teclado que se describen en una etiqueta de elemento de menú](images/accelerators/accelerators_menuitemlabel.png)  
*Aceleradores de teclado que se describen en una etiqueta de elemento de menú*

## <a name="when-to-use-keyboard-accelerators"></a>Cuándo usar aceleradores de teclado

Te recomendamos que especifiques los aceleradores de teclado siempre que sea adecuado en la interfaz de usuario y admitas aceleradores en todos los controles personalizados.

- Los aceleradores de teclado hacen que tu aplicación más usuarios accessiblefor con discapacidades motrices, incluidos aquellos usuarios que pueden presionar solo una tecla a la vez o tienen dificultades para usar un mouse.* *

  Una interfaz de usuario de teclado bien diseñada es un aspecto importante de la accesibilidad del software. Permite a usuarios con dificultades visuales o con ciertas discapacidades motrices navegar por una aplicación e interactuar con sus funciones. Es posible que estos usuarios no puedan controlar un mouse y empleen varias tecnologías de ayuda, como herramientas para la mejora del teclado, teclados en pantalla, ampliadores de pantallas, lectores de pantalla y utilidades de entrada de voz. Para estos usuarios, es fundamental que la cobertura completa de los comandos.

- Los aceleradores de teclado que tu aplicación más usuarios avanzados de usablefor que prefieren interactuar a través del teclado.

  Los usuarios con experiencia suelen tener una fuerte preferencia por el teclado, ya que los comandos se pueden introducir más rápidamente y no requieren apartar las manos de las teclas. Para estos usuarios, la eficacia y la coherencia son cruciales; la exhaustividad es importante solo para los comandos usados con más frecuencia.

## <a name="specify-a-keyboard-accelerator"></a>Especificar un acelerador de teclado

Usa las API de [KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) API para crear aceleradores de teclado en aplicaciones para UWP. Con estas API, no tienes que controlar varios eventos KeyDown para detectar la combinación de teclas presionadas y puedes localizar aceleradores en los recursos de la aplicación.

Te recomendamos que establezcas aceleradores de teclado para las acciones más comunes en la aplicación y los documente con la etiqueta del elemento de menú o la información sobre herramientas. En este ejemplo, declaramos aceleradores de teclado solo para los comandos de cambio de nombre y copia.

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

El objeto [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) tiene una colección [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator), [KeyboardAccelerators](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators), donde especificas tus objetos KeyboardAccelerator personalizados y defines las pulsaciones de teclas para el acelerador de teclado:

-   **[Key](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)**: el valor de [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey) usado para el acelerador de teclado.

-   **[Modifiers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)**: el valor de [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers) usado para el acelerador de teclado. Si no se establece Modifiers, el valor predeterminado es Ninguno.

> [!NOTE]
> Se admiten los aceleradores de tecla única (A, Supr, F2, Barra espaciadora, Esc, Tecla multimedia) y los aceleradores de varias teclas (Ctrl+Mayús+M). Sin embargo, no se admiten las teclas virtuales del controlador para juegos.

## <a name="scoped-accelerators"></a>Aceleradores con ámbito

Algunos aceleradores solo funcionan en ámbitos mientras que otros funcionan en toda la aplicación.

Por ejemplo, Microsoft Outlook incluye los siguientes aceleradores:
-   Ctrl+B, Ctrl+I y ESC solo funcionan en el ámbito del formulario de envío de correo electrónico
-   Ctrl+1 y Ctrl+2 funcionan en toda la aplicación

### <a name="context-menus"></a>Menús contextuales

Las acciones de menú contextual solo afectan a elementos o áreas específicas, como los caracteres seleccionados en un editor de texto o una canción en una lista de reproducción. Por este motivo, te recomendamos que establezcas el ámbito de los aceleradores de teclado para elementos de menú contextual en el elemento primario del menú contextual.

Usa la propiedad [ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) para especificar el ámbito del acelerador de teclado. Este código muestra cómo implementar un menú contextual en un elemento ListView con aceleradores de teclado con ámbito:

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

El atributo ScopeOwner del elemento MenuFlyoutItem.KeyboardAccelerators marca el acelerador como con ámbito en lugar de global (el valor predeterminado es nulo o global). Para obtener más información, consulta la sección **Resolución de aceleradores** más adelante en este tema.

## <a name="invoke-a-keyboard-accelerator"></a>Invocar un acelerador de teclado 

El objeto [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) usa el [patrón de control de automatización de la interfaz de usuario (UIA)](https://msdn.microsoft.com/library/windows/desktop/ee671194(v=vs.85).aspx) para realizar una acción cuando se invoca un acelerador.

La UIA [patrones de control] expone la funcionalidad de control habitual. Por ejemplo, el control Button implementa el patrón de control [Invoke](https://msdn.microsoft.com/library/windows/desktop/ee671279(v=vs.85).aspx) para admitir el evento Click (normalmente un control se invoca al hacer clic en, doble clic o presionar Entrar, un método abreviado de teclado predefinido o alguna combinación alternativa de pulsaciones de teclas). Cuando se usa un Acelerador de teclado para invocar un control, el marco XAML busca si el control implementa el patrón de control Invoke y, de ser así, lo activa (no es necesario escuchar el evento KeyboardAcceleratorInvoked).

En el ejemplo siguiente, Control+S desencadena el evento Click porque el botón implementa el modelo Invoke.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Si un elemento implementa varios patrones de control, solo se puede activar uno a través de un acelerador. A los patrones de control se les da la siguiente prioridad:
1.  Invocar (botón)
2.  Alternar (casilla)
3.  Selección (ListView)
4.  Ampliar/contraer (ComboBox) 

Si no se identifica ninguna coincidencia, el acelerador no es válido y se proporciona un mensaje de depuración ("No automation patterns for this component found. (No se han encontrado patrones de automatización para este componente.) Implement all desired behavior in the Invoked event. (Implementa todo el comportamiento deseado en el evento Invoked.) Setting Handled to true in your event handler will suppress this message. (Si Handled se establece en true en el controlador de eventos, se suprimirá este mensaje)".

## <a name="custom-keyboard-accelerator-behavior"></a>Comportamiento del acelerador de teclado personalizado

El evento Invoked del objeto [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) se desencadena al ejecutar el acelerador. El objeto de evento [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) incluye las siguientes propiedades:
- **Handled** (booleano): si se establece en True se evita que el evento desencadene el patrón de control y detiene la propagación de eventos de acelerador. El valor predeterminado es False.
- **Element** (DependencyObject): el objeto que contiene el acelerador.

Aquí se muestra cómo definir una colección de aceleradores de teclado y cómo controlar el evento Invoked.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>   
```

``` csharp
void SelectAllInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  CustomSelectAll(MyListView);
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  Refresh(MyListView);
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>Invalidar el comportamiento predeterminado del teclado

En algunos casos, es posible que necesitas invalidar el comportamiento predeterminado de teclas específicas, como la tecla retroceso o la tecla ENTRAR. Por ejemplo: 

## <a name="disable-a-keyboard-accelerator"></a>Deshabilitar un acelerador de teclado 

Si se deshabilita un control, también se deshabilita el acelerador asociado. En el ejemplo siguiente, dado que la propiedad IsEnabled de ListView se establece en False, no se puede invocar el acelerador de Control+A asociado.

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

Los controles primarios y secundarios pueden compartir el mismo acelerador. En este caso, el control primario se puede invocar incluso si el elemento secundario tiene el foco y su acelerador está deshabilitado.

## <a name="screen-readers-and-keyboard-accelerators"></a>Lectores de pantalla y aceleradores de teclado 

Los lectores de pantalla como Narrador pueden anunciar la combinación de teclas del acelerador de teclado a los usuarios. De manera predeterminada, esto es cada modificador (en el orden de enumeración VirtualModifiers) seguido de la clave (y separado por signos "+"). Puedes personalizar esto a través de la propiedad adjunta AutomationProperties [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty). Si se especifica más de un acelerador, solo se anuncia el primero.

En este ejemplo, el valor de AutomationProperty.AcceleratorKey devuelve la cadena "Control+Mayús+A":

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
> El establecimiento de AutomationProperties.AcceleratorKey no habilita la funcionalidad de teclado, solo indica al marco de UIA qué claves se usan.

## <a name="common-keyboard-accelerators"></a>Aceleradores de teclado comunes

Te recomendamos que crees aceleradores de teclado coherentes entre aplicaciones para UWP. Los usuarios tienen que memorizar aceleradores de teclado y esperar los mismo resultados (o similares).

Puede que esto no sea siempre posible debido a las diferencias de funcionalidad entre las aplicaciones.

| **Edición** | **Acelerador de teclado común** |
| ------------- | ----------------------------------- |
| Comenzar el modo de edición | Ctrl+E |
| Seleccionar todos los elementos en un control enfocado o una ventana | Ctrl + A |
| Buscar y reemplazar | Ctrl + H |
| Deshacer | Ctrl + Z |
| Rehacer | Ctrl + Y |
| Eliminar la selección y copiarla en el Portapapeles | Ctrl + X |
| Copiar la selección en el Portapapeles | Ctrl + C, Ctrl + Insertar |
| Pegar el contenido del Portapapeles | Ctrl + V, Mayús + Insertar |
| Pegar el contenido del Portapapeles (con opciones) | Ctrl + Alt + V |
| Cambiar el nombre de un elemento | F2 |
| Agregar un nuevo elemento | Ctrl + N |
| Agregar un nuevo elemento secundario | Ctrl + Mayús + N |
| Eliminar elemento seleccionado (con deshacer) | Supr, Ctrl+D |
| Eliminar elemento seleccionado (sin deshacer) | Mayús + Supr |
| Negrita | Ctrl + B |
| Subrayado | Ctrl + U |
| Cursiva | Ctrl + I |

| **Navegación** | |
| ------------- | ----------------------------------- |
| Buscar contenido en un control enfocado o una ventana | Ctrl + F |
| Ir al siguiente resultado de la búsqueda | F3 |

| **Otras acciones** | |
| ------------- | ----------------------------------- |
| Agregar favoritos | Ctrl + D | 
| Actualizar | F5 o Ctrl + R | 
| Acercar | Ctrl + + | 
| Alejar | Ctrl + - | 
| Ampliar a la vista predeterminada | Ctrl + 0 | 
| Guardar | Ctrl + S | 
| Cerrar | Ctrl + W | 
| Imprimir | Ctrl + P | 

Ten en cuenta que algunas de las combinaciones no son válidas para las versiones localizadas de Windows. Por ejemplo, en la versión de Windows en español, CTRL+N se usa para negrita en lugar de Ctrl+B. Te recomendamos que proporciones los aceleradores de teclado localizado si la aplicación está localizada.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Prestaciones de facilidad de uso para aceleradores de teclado

### <a name="tooltips"></a>Información de herramientas

Como los aceleradores de teclado no suelen detectarse directamente en la interfaz de usuario de tu aplicación para UWP, puedes mejorar la capacidad de detección a través de [información sobre herramientas](../controls-and-patterns/tooltips.md), que se muestra de forma automática cuando el usuario mueve el foco al control, lo mantiene presionado o cuando pasa sobre él con el puntero del mouse. La información sobre herramientas puede identificar si un control tiene un acelerador de teclado asociado y, en caso afirmativo, cuál es la combinación de teclas del acelerador.

**Windows 10, versión 1803 (actualización de abril de 2018) y versiones posteriores**

De manera predeterminada, cuando se declaran los aceleradores de teclado, todos los controles (excepto [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) y [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) presentan las combinaciones de teclas correspondientes en una información sobre herramientas.

> [!NOTE] 
> Si un control tiene más de un acelerador definido, solo el primero se presenta.

![Información sobre herramientas de la tecla aceleradora](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Combinación de teclas aceleradoras en la información sobre herramientas*

Para el [botón](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)y objetos de [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) , el Acelerador de teclado se anexa a información sobre herramientas del control de forma predeterminada. Para [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) y [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) objetos, el Acelerador de teclado se muestra con el texto de control flotante.

> [!NOTE]
> Especificar información sobre herramientas (consulta Button1 en el siguiente ejemplo) reemplaza este comportamiento.

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

![Información sobre herramientas de la tecla aceleradora](images/accelerators/accelerators-button-small.png)

*Teclas aceleradoras anexadas a la información sobre herramientas del botón predeterminado*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![Información sobre herramientas de la tecla aceleradora](images/accelerators/accelerators-appbarbutton-small.png)

*Teclas aceleradoras anexadas a la información sobre herramientas del AppBarButton predeterminada*

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

![Información sobre herramientas de la tecla aceleradora](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Teclas aceleradoras anexadas al texto del MenuFlyoutItem*

Controla el comportamiento de la presentación con la propiedad [KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode), que acepta dos valores: [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) o [Hidden](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode).    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

En algunos casos, es posible que debas presentar una información sobre herramientas relativa a otro elemento (normalmente un objeto contenedor). Por ejemplo, un control dinámico que muestra la información sobre herramientas para un PivotItem con el encabezado dinámico. 

Aquí te enseñamos a usar la propiedad KeyboardAcceleratorPlacementTarget para mostrar la combinación de teclas aceleradoras del teclado para un botón Guardar con el contenedor Cuadrícula en lugar del botón.

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

En algunos casos, recomendamos usar la etiqueta de un control para identificar si el control tiene un acelerador de teclado asociado y, en caso afirmativo, cuál es la combinación de teclas del acelerador. 

Algunos controles de plataforma lo hacen de manera predeterminada, especialmente los objetos [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) y [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), mientras que [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) y [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) lo hacen cuando aparecen en el menú de desbordamiento de [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar).

![Aceleradores de teclado que se describen en una etiqueta de elemento de menú](images/accelerators/accelerators_menuitemlabel.png)  
*Aceleradores de teclado que se describen en una etiqueta de elemento de menú*

Puedes invalidar el texto del acelerador predeterminado para la etiqueta a través de la propiedad [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) de los controles [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) y [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) (usar un solo espacio si no hay texto). 

> [!NOTE] 
> El texto de invalidación no se presenta si el sistema no puede detectar un teclado conectado (puedes comprobarlo por ti mismo a través de la propiedad [KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent)).

## <a name="advanced-concepts"></a>Conceptos avanzados

Aquí revisaremos algunos aspectos de bajo nivel de aceleradores de teclado.

### <a name="when-an-accelerator-is-invoked"></a>Cuando se invoca un acelerador

Los aceleradores se componen de dos tipos de teclas: modificadores y no modificadores. Entre la teclas modificadoras se incluyen Mayús, Menú, Control y la tecla Windows, que se exponen a través de [VirtualKeyModifiers](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.System.VirtualKeyModifiers). Los no modificadores son cualquier tecla virtual, como Supr, F3, barra espaciadora, Esc y todas las teclas de puntuación y alfanuméricas. Un acelerador de teclado se invoca cuando el usuario presiona una tecla no modificadora mientras pulsa y sostiene una o más teclas modificadoras. Por ejemplo, si el usuario presiona Ctrl+Mayús+M, cuando el usuario presiona M, el marco comprueba los modificadores (Ctrl y Mayús) y desencadena el acelerador, si existe.

> [!NOTE]
> Por diseño, el acelerador se repite automáticamente (por ejemplo, cuando el usuario presiona Ctrl+Mayús y, a continuación, mantiene presionado M, el acelerador se invoca de manera repetida hasta que se libere M). No se puede modificar este comportamiento.

### <a name="input-event-priority"></a>Prioridad de los eventos de entrada
Los eventos de entrada se producen en una secuencia específica que puedes interceptar y controlar en función de los requisitos de la app. 

#### <a name="the-keydownkeyup-bubbling-event"></a>El evento de propagación KeyDown o KeyUp 

En XAML, se procesa una pulsación de tecla como si hubiera solo una canalización de propagación de entrada. Esta canalización de entrada se usa por los eventos KeyDown o KeyUp y la entrada de caracteres. Por ejemplo, si un elemento tiene el foco y el usuario presiona una tecla, se genera un evento KeyDown en el elemento, seguido del elemento primario del elemento, y así sucesivamente por el árbol, hasta los argumentos. La propiedad Handled es true.

El evento KeyDown también se usa por algunos controles para implementar los aceleradores de controles integrados. Cuando un control tiene un acelerador de teclado, controla el evento KeyDown, lo que significa que no habrá propagación de eventos KeyDown. Por ejemplo, RichEditBox admite copia con Ctrl+C. Cuando se presiona la tecla Ctrl, se desencadena y se propaga el evento KeyDown, pero cuando el usuario presiona C al mismo tiempo, el evento KeyDown se marca como Handled y no se genera (a menos que el parámetro handledEventsToo de [UIElement.AddHandler](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.AddHandler) se establezca en true).

#### <a name="the-characterreceived-event"></a>El evento CharacterReceived

Como el evento [CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived) se desencadena después del evento [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) para controles de texto como TextBox, puedes cancelar la entrada de caracteres en el controlador de eventos KeyDown.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Los eventos PreviewKeyDown y PreviewKeyUp

Los eventos de entrada de vista previa se desencadenan antes de cualquier otro evento. Si no controlas estos eventos, se desencadena el acelerador para el elemento que tiene el foco, seguido del evento KeyDown. Ambos eventos se propagan hasta que se controlan.


![Secuencia de eventos de teclas](images/accelerators/accelerators_keyevents.png)
***Secuencia de eventos de teclas***

Orden de eventos:

Eventos KeyDown de vista previa …
Acelerador de aplicación Método OnKeyDown Evento KeyDown Aceleradores de aplicación en el método OnKeyDown primario del evento KeyDown primario del elemento primario (se propaga en la raíz)...
Evento CharacterReceived eventos PreviewKeyUp KeyUpEvents

Cuando se controla el evento del acelerador, el evento KeyDown también se marca como controlado. El evento KeyUp continúa siendo no controlado.

### <a name="resolving-accelerators"></a>Resolución de aceleradores

Un evento de acelerador de teclado se propaga desde el elemento que tiene el foco hasta la raíz. Si no se controla el evento, el marco XAML busca otros aceleradores de aplicación sin ámbito fuera de la ruta de acceso de propagación.

Cuando se definen dos aceleradores de teclado con la misma combinación de teclas, se invoca el primer acelerador de teclado en el árbol visual.

Los aceleradores de teclado con ámbito se invocan solo cuando el foco está dentro de un ámbito específico. Por ejemplo, en una cuadrícula que contiene docenas de controles, solo se puede invocar un acelerador de teclado para un control cuando el foco se encuentra dentro de la cuadrícula (el propietario del ámbito).

### <a name="scoping-accelerators-programmatically"></a>Establecimiento del ámbito de los aceleradores mediante programación

El método [UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) invoca los aceleradores coincidentes del subárbol del elemento.

El método [UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) se ejecuta antes del acelerador del teclado. Este método pasa un objeto [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) que contiene la clave, el modificador y un valor booleano que indica si el acelerador de teclado está controlado. Si están marcados como controlados, el acelerador de teclado se propaga (por lo que nunca se invoca el acelerador de teclado exterior).

> [!NOTE]
> OnProcessKeyboardAccelerators siempre se desencadena, ya sea controlado o no (similar al evento OnKeyDown). Debes comprobar si el evento se marcó como controlado.

En este ejemplo, usamos OnProcessKeyboardAccelerators y TryInvokeKeyboardAccelerator para establecer el ámbito de los aceleradores de teclado para el objeto de página:

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

Te recomendamos localizar todos los aceleradores de teclado. Puedes hacerlo con el archivo de recursos (.resw) para UWP estándar y el atributo x:Uid en tus declaraciones XAML. En este ejemplo, Windows Runtime carga los recursos automáticamente.

![Localización del acelerador de teclado con el archivo de recursos UWP](images/accelerators/accelerators_localization.png)
***Localización del acelerador de teclado con el archivo de recursos para UWP***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Configurar un acelerador mediante programación

Este es un ejemplo de la definición mediante programación de un acelerador:

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
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator no se puede compartir; no se puede agregar el mismo KeyboardAccelerator a varios elementos.

### <a name="override-keyboard-accelerator-behavior"></a>Invalidar comportamiento del acelerador de teclado

Puedes controlar el evento [KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) para invalidar el comportamiento KeyboardAccelerator predeterminado.

En este ejemplo se muestra cómo invalidar el comando "Seleccionar todo" (Ctrl+un acelerador de teclado) en un control ListView personalizado. También establecemos la propiedad Handled en true para impedir que el evento se propague más.

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

* [Interacciones de teclado](keyboard-interactions.md)
* [Teclas de acceso](access-keys.md)

**Ejemplos**
* [Galería de controles XAML (también conocido como XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


 


---
author: Karl-Bridge-Microsoft
title: Navegación con foco para teclado, controlador para juegos, control remoto y herramientas de accesibilidad
Description: ''
label: ''
template: detail.hbs
keywords: teclado, dispositivo de juego, control remoto, navegación, navegación interna direccional, área direccional, estrategia de navegación, entrada, interacción del usuario, accesibilidad, facilidad de uso
ms.date: 03/02/2018
ms.author: kbridge
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f4c86847e02c4ba987f8b1dcc42016145b175193
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151700"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Navegación con foco para teclado, controlador para juegos, control remoto y herramientas de accesibilidad

![Teclado, control remoto y pad-D](images/dpad-remote/dpad-remote-keyboard.png)

Usa la navegación con foco para ofrecer experiencias de interacción completas y coherentes en tus aplicaciones para UWP y controles personalizados para usuarios avanzados de teclado, aquellos que tienen discapacidades y otros requisitos de accesibilidad, así como la experiencia de 10 pies de pantallas de televisión y la Xbox One.

## <a name="overview"></a>Introducción

La navegación con foco hace referencia al mecanismo subyacente que permite a los usuarios ir a la interfaz de usuario de una aplicación para UWP e interactuar con ella mediante un teclado, controlador para juegos o control remoto.

> [!NOTE] 
> Los dispositivos de entrada suelen clasificarse como dispositivos señaladores tales como función táctil, panel táctil, lápiz y mouse, y dispositivos no señaladores tales como teclado, controlador para juegos y control remoto.

En este tema se describe cómo optimizar una aplicación para UWP y crear experiencias de interacción personalizadas para usuarios que utilizan tipos de entrada no señaladores. 

Aunque nos centramos en la entrada de teclado para controles personalizados en aplicaciones para UWP en equipos, una experiencia de teclado bien diseñada también es importante para teclados de software como el teclado táctil y el teclado en pantalla (OSK), admitiendo herramientas de accesibilidad como Windows Narrator, así como la experiencia de 10 pies.

Consulta [Controlar la entrada de puntero](handle-pointer-input.md) para obtener instrucciones sobre experiencias personalizadas en aplicaciones para UWP para dispositivos señaladores.

Para obtener más información general sobre la creación de aplicaciones y experiencias para teclado, consulta [Interacción de teclado](keyboard-interactions.md).

## <a name="general-guidance"></a>Instrucciones generales
Solo los elementos de la interfaz de usuario que requieran interacción con el usuario deberán admitir la navegación con foco. Los elementos que no requieran una acción, como imágenes estáticas, no necesitarán el foco de teclado. Los lectores de pantalla y herramientas de accesibilidad similares aún anuncian estos elementos estáticos, incluso cuando no se incluyen en la navegación con foco. 

Es importante recordar que, a diferencia de la navegación con un dispositivo señalador como un mouse o entrada táctil, la navegación con foco es lineal. Al implementar la navegación con foco, ten en cuenta cómo interactuará un usuario con tu aplicación y cuál debe ser la navegación lógica. En la mayoría de los casos, recomendamos que el comportamiento de la navegación con foco personalizado siga el patrón de lectura preferido de la referencia cultural del usuario.

Otras consideraciones de la navegación con foco incluyen:
- ¿Se agrupan los controles de forma lógica?
- ¿Hay grupos de controles de mayor importancia? 
   - En caso afirmativo, ¿contienen esos grupos subgrupos?
- ¿Requiere el diseño navegación direccional personalizada (teclas de dirección) y orden de tabulación?

El libro electrónico sobre [diseño de software para accesibilidad](https://www.microsoft.com/download/details.aspx?id=19262) tiene un excelente capítulo sobre *Designing the Logical Hierarchy* (Diseño de la jerarquía lógica).

## <a name="2d-directional-navigation-for-keyboard"></a>Navegación direccional 2D para teclado

La región de navegación interna 2D de un control o un grupo de controles se conoce como su "área direccional". Cuando el foco cambia a este objeto, las teclas de dirección del teclado (izquierda, derecha, arriba y abajo) se pueden usar para navegar entre elementos secundarios dentro del área direccional.

![área direccional](images/keyboard/directional-area-small.png)

*Región de navegación interna 2D o área direccional de un grupo de controles*

Puedes usar la propiedad [XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) (que tiene valores posibles de [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [Enabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) o [Disabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) para administrar la navegación interna 2D con las teclas de dirección del teclado.

> [!NOTE]  
> El orden de tabulación no se ve afectado por esta propiedad. Para evitar una experiencia de navegación confusa, recomendamos que los elementos secundarios de un área direccional *no* se especifiquen de forma explícita en el orden de navegación mediante tabulación de tu aplicación. Consulta las propiedades [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) y [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para obtener más detalles sobre el comportamiento de desplazamiento para un elemento.  

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (comportamiento predeterminado)

Al establecerse en Auto, el comportamiento de navegación direccional viene determinado por la ascendencia del elemento o la jerarquía de herencia. Si todos los antecesores se encuentran en el modo predeterminado (establecidos en **Auto**), la navegación direccional con el teclado *no* se admite.

### [<a name="disabled"></a>Disabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Establece **XYFocusKeyboardNavigation** en **Disabled** para bloquear la navegación direccional al control y sus elementos secundarios.

![Comportamiento deshabilitado de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-disabled.gif)

*Comportamiento deshabilitado de XYFocusKeyboardNavigation*

En este ejemplo, el [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) principal tiene **XYFocusKeyboardNavigation** establecido en **Enabled**. Todos los elementos secundarios heredan esta configuración, a la que se puede ir con las teclas de dirección. Sin embargo, los elementos B3 y B4 están en un [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary) secundario con **XYFocusKeyboardNavigation** establecido en **Disabled**, lo que invalida el contenedor principal y deshabilita la navegación con teclas de dirección a sí mismo y entre sus elementos secundarios.

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### [<a name="enabled"></a>Enabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Establece **XYFocusKeyboardNavigation** en **Enabled** para admitir la navegación direccional 2D a un control y cada uno de sus objetos secundarios [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement).

Al establecerse, la navegación con las teclas de dirección se restringe a elementos dentro del área direccional. La navegación mediante tabulación no se ve afectada, pues todos los controles permanecen accesibles a través de su jerarquía de orden de tabulación.

![Comportamiento habilitado de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled.gif)

*Comportamiento habilitado de XYFocusKeyboardNavigation*

En este ejemplo, el [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) principal tiene **XYFocusKeyboardNavigation** establecido en **Enabled**. Todos los elementos secundarios heredan esta configuración, a la que se puede ir con las teclas de dirección. Los elementos B3 y B4 están en un [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary) secundario en el que **XYFocusKeyboardNavigation** no está establecido, que luego hereda la configuración del contenedor principal. El elemento B5 no está en un área direccional declarada y no admite la navegación con teclas de dirección, pero admite el comportamiento de navegación mediante tabulación estándar.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

Puedes tener varios niveles de áreas direccionales anidadas. Si todos los elementos primarios tienen XYFocusKeyboardNavigation establecido en Enabled, los límites de la región de navegación interna se omiten.

A continuación se muestra un ejemplo de dos áreas direccionales anidadas dentro de un elemento que no admiten de forma explícita la navegación direccional 2D. En este caso, la navegación direccional no se admite entre las dos áreas anidadas.

![Comportamiento habilitado y anidado de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)

*Comportamiento habilitado y anidado de XYFocusKeyboardNavigation*

Este es un ejemplo más complejo de tres áreas direccionales anidadas donde:

-   Cuando B1 tiene el foco, solo se puede ir a B5 (y viceversa) porque hay un límite de área direccional en el que XYFocusKeyboardNavigation se establece en Disabled, haciendo que B2, B3 y B4 sean inaccesibles con las teclas de dirección
-   Cuando B2 tiene el foco, solo se puede navegar a B3 (y viceversa) porque el límite de área direccional impide la navegación con teclas de dirección a B1, B4 y B5
-   Cuando B4 tiene el foco, la tecla Tabulador debe usarse para navegar entre controles

![Comportamiento habilitado y anidado complejo de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*Comportamiento habilitado y anidado complejo de XYFocusKeyboardNavigation*

## <a name="tab-navigation"></a>Navegación en pestañas

Mientras que las teclas de dirección se pueden usar para la navegación direccional 2D dentro de un control o grupo de controles, la tecla Tabulador se puede usar para navegar entre todos los controles en una aplicación para UWP. 

Todos los controles interactivos admiten la navegación con teclas Tabulador de manera predeterminada (las propiedades [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) e [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) son **true**), con el orden de tabulación lógico derivado del diseño de controles en tu aplicación. No obstante, el orden predeterminado no necesariamente corresponde al orden visual. La posición de visualización real probablemente dependa del contenedor de diseño primario y ciertas propiedades que puedes establecer en los elementos secundarios para influir en el diseño.

Evita un orden de tabulación personalizado que haga que el foco salte sin sentido en la aplicación. Por ejemplo, una lista de controles en un formulario debe tener un orden de tabulación que fluya de arriba a abajo y de izquierda a derecha (dependiendo de la configuración regional).

En esta sección, describimos cómo se puede personalizar este orden de tabulación totalmente para adaptarlo a tu aplicación.

### <a name="set-the-tab-navigation-behavior"></a>Establecer el comportamiento de navegación mediante tabulación

La propiedad [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) de [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) especifica el comportamiento de navegación mediante tabulación de todo su árbol de objetos (o área direccional).

> [!NOTE]
> Usa esta propiedad en lugar de la propiedad [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) para objetos que no utilicen una clase [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) para definir su apariencia.

Como mencionamos en la sección anterior, para evitar una experiencia de navegación confusa, recomendamos que los elementos secundarios de un área direccional *no* se especifiquen de forma explícita en el orden de navegación mediante tabulación de tu aplicación. Consulta las propiedades [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) y [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para obtener más detalles sobre el comportamiento de desplazamiento para un elemento.   
> Para versiones anteriores a Windows 10 Creators Update (compilación 10.0.15063), la configuración de la pestaña se limitaba a los objetos [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate). Para obtener más información, consulta [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) tiene un valor de tipo [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) con los siguientes valores posibles (ten en cuenta que estos ejemplos no son grupos de control personalizados y no requieren navegación interna con las teclas de dirección):

- **Local** (valor predeterminado)   
  Los índices de tabulación se reconocen en el subárbol local dentro del contenedor. Para este ejemplo, el orden de tabulación es B1, B2, B3, B4, B5, B6, B7, B1.

   ![Comportamiento de navegación mediante tabulación "Local"](images/keyboard/tabnav-local.gif)

   *Comportamiento de navegación mediante tabulación "Local"*

- **Una vez**  
  El contenedor y todos los elementos secundarios reciben el foco una vez. Para este ejemplo, el orden de tabulación es B1, B2, B7, B1 (también se muestra la navegación interna mediante teclas de dirección).

   ![Establecer el comportamiento de navegación mediante tabulación "Una vez"](images/keyboard/tabnav-once.gif)

   *Establecer el comportamiento de navegación mediante tabulación "Una vez"*

- **Ciclo**   
  Vuelve a enfocar ciclos en el elemento activable inicial dentro de un contenedor. Para este ejemplo, el orden de tabulación es B1, B2, B3, B4, B5, B6, B2...

   ![Establecer el comportamiento de navegación mediante tabulación "Ciclo"](images/keyboard/tabnav-cycle.gif)

   *Establecer el comportamiento de navegación mediante tabulación "Ciclo"*

Este es el código para los ejemplos anteriores (con TabFocusNavigation ="Ciclo").

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### [<a name="tabindex"></a>TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Usa [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para especificar el orden en que los elementos reciben el foco cuando el usuario navega a través de controles con la tecla Tabulador. Un control con un menor índice de tabulación recibe el foco antes de un control con un mayor índice.

Cuando un control no tiene ningún [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) especificado, se le asigna un valor de índice mayor que el valor de índice mayor actual (y la prioridad más baja) de todos los controles interactivos en el árbol visual, basado en el foco. 

Todos los elementos secundarios de un control se consideran un ámbito y, si uno de estos elementos también tiene elementos secundarios, se consideran otro ámbito. Cualquier ambigüedad se resuelve seleccionando el primer elemento en el árbol visual del ámbito. 

Para excluir un control del orden de tabulación, establece la propiedad [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) en **false**.

Invalida el orden de tabulación predeterminada estableciendo la propiedad [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex).

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) funciona de la misma forma con [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) y [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).


Aquí, mostramos cómo puede la navegación con foco verse afectada por la propiedad [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) en elementos específicos. 

![Navegación mediante tabulación "Local" con comportamiento TabIndex](images/keyboard/tabnav-tabindex.gif)

*Navegación mediante tabulación "Local" con comportamiento TabIndex*

En el ejemplo anterior, hay dos ámbitos: 
- B1, área direccional (B2 - B6) y B7
- área direccional (B2 - B6)

Cuando B3 (en el área direccional) obtiene el foco, cambia el ámbito y la navegación mediante tabulación se transfiere al área direccional donde se identifica al mejor candidato para el foco posterior. En este caso, B2 seguido de B4, B5 y B6. A continuación, el ámbito cambia de nuevo y el foco se mueve a B1.

Este es el código de este ejemplo.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>Navegación direccional 2D para teclado, controlador para juegos y control remoto

Tipos de entrada que no son de puntero tales como teclado, controlador para juegos, control remoto y herramientas de accesibilidad como Windows Narrator, comparten un mecanismo subyacente común para ir a la interfaz de usuario de tu aplicación para UWP e interactuar con ella.

En esta sección, describimos cómo especificar una estrategia de navegación preferida y ajustar la navegación con foco dentro de tu aplicación a través de un conjunto de propiedades de estrategias de navegación que admiten todos los tipos de entrada que no son de puntero basados en el foco.

Para obtener más información general sobre la creación de aplicaciones y experiencias para Xbox y televisión, consulta [Interacción de teclado](keyboard-interactions.md) y [Diseño para Xbox y televisión](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction).

### <a name="navigation-strategies"></a>Estrategias de navegación

> Las estrategias de navegación se aplican al teclado, el controlador para juegos, el control remoto y diversas herramientas de accesibilidad.

Las siguientes propiedades de estrategias de navegación te permiten determinar qué control recibe el foco basado en la tecla de dirección, el botón Pad direccional (pad-D) o similares presionados. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Estas propiedades tienen valores posibles de [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (valor predeterminado), [NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) o [RectilinearDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy).

Si se establece en **Auto**, el comportamiento del elemento se basa en los antecesores del elemento. Si todos los elementos se establecen en **Auto**, se usa **Projection**.

> [!NOTE]
> Otros factores, como el elemento enfocado anteriormente o la proximidad al eje de la dirección de navegación, pueden influir en el resultado.

### <a name="projection"></a>Proyección

La estrategia de proyección mueve el foco al primer elemento encontrado al *proyectar* el borde del elemento enfocado actualmente en la dirección de navegación.

En este ejemplo, cada dirección de navegación con foco se establece en Projection. Ten en cuenta la manera en que el foco se mueve hacia abajo de B1 a B4, omitiendo B3. Esto se debe a que B3 no está en la zona de proyección. Observa también la no identificación de un candidato de foco al moverse hacia la izquierda desde B1. Esto se debe a que la posición de B2 relativa a B1 elimina B3 como candidato. Si B3 estuviera en la misma fila que B2, sería un candidato viable para la navegación izquierda. B2 es un candidato viable debido a su proximidad sin obstáculos al eje de la dirección de navegación.

![Estrategia de navegación de proyección](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Estrategia de navegación de proyección*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

La estrategia NavigationDirectionDistance mueve el foco al elemento más cercano al eje de la dirección de navegación.

El borde del rectángulo delimitador correspondiente a la dirección de navegación se *extiende* y *proyecta* para identificar los objetivos candidatos. El primer elemento encontrado se identifica como el objetivo. En el caso de varios candidatos, el elemento más próximo se identifica como el objetivo. Si sigue habiendo varios candidatos, el elemento más hacia arriba/más a la izquierda se identifica como el candidato.

![Estrategia de navegación NavigationDirectionDistance](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Estrategia de navegación NavigationDirectionDistance*

### <a name="rectilineardistance"></a>RectilinearDistance

La estrategia RectilinearDistance mueve el foco al elemento más cercano según la distancia rectilínea 2D ([geometría Taxicab](https://en.wikipedia.org/wiki/Taxicab_geometry)).

La suma de la distancia principal y la secundaria para cada posible candidato se usa para identificar al mejor candidato. En caso de empate, se selecciona el primer elemento a la izquierda si la dirección solicitada es hacia arriba o hacia abajo, y se selecciona el primer elemento de la parte superior si la dirección solicitada es izquierda o derecha.

![Estrategia de navegación RectilinearDistance](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Estrategia de navegación RectilinearDistance*

En esta imagen se muestra cómo, cuando B1 tiene el foco y la dirección solicitada es hacia abajo, B3 es el candidato de foco RectilinearDistance. Esto se basa en los siguientes cálculos para este ejemplo:
-   La distancia (B1, B3, hacia abajo) es 10+0=10
-   La distancia (B1, B2, hacia abajo) es 0+40=30
-   La distancia (B1, D, hacia abajo) es 30+0=30


## <a name="related-articles"></a>Artículos relacionados
- [Navegación con foco mediante programación](focus-navigation-programmatic.md)
- [Interacciones de teclado](keyboard-interactions.md)
- [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md) 




---
title: Navegación de foco sin mouse
Description: Obtenga información sobre cómo usar la navegación centrada para proporcionar experiencias de interacción completas y coherentes en las aplicaciones de Windows y controles personalizados para los usuarios avanzados de teclado, aquellos con discapacidades y otros requisitos de accesibilidad, así como la experiencia de 10 pies de las pantallas de televisión y la Xbox One.
label: ''
template: detail.hbs
keywords: teclado, dispositivo de juego, control remoto, navegación, navegación interna direccional, área direccional, estrategia de navegación, entrada, interacción del usuario, accesibilidad, facilidad de uso
ms.date: 03/02/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4ad0e986de3f3084cd33f217df7715c955cb6b57
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172579"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Navegación centrada en el teclado, el controlador para juegos, el control remoto y las herramientas de accesibilidad

![Teclado, remoto y PAD D](images/dpad-remote/dpad-remote-keyboard.png)

Use la navegación centrada para proporcionar experiencias de interacción completas y coherentes en las aplicaciones de Windows y controles personalizados para los usuarios avanzados de teclado, los con discapacidades y otros requisitos de accesibilidad, así como la experiencia de 10 pies de las pantallas de televisión y la Xbox One.

## <a name="overview"></a>Información general

La navegación de foco hace referencia al mecanismo subyacente que permite a los usuarios navegar e interactuar con la interfaz de usuario de una aplicación de Windows mediante un teclado, un controlador de juegos o un control remoto.

> [!NOTE]
> Los dispositivos de entrada se clasifican normalmente como dispositivos señaladores, como el toque, el touchpad, el lápiz y el mouse, y los dispositivos que no son de señal, como el teclado, el controlador para juegos y el control remoto.

En este tema se describe cómo optimizar una aplicación de Windows y crear experiencias de interacción personalizadas para los usuarios que se basan en tipos de entrada no señalados. 

Aunque nos centramos en la entrada de teclado para los controles personalizados en las aplicaciones de Windows en los equipos, una experiencia de teclado bien diseñada también es importante para los teclados de software como el teclado táctil y el teclado en pantalla (OSK), la compatibilidad con herramientas de accesibilidad como el narrador de Windows y la compatibilidad con la experiencia de 10 pies.

Consulte [controlar la entrada de puntero](handle-pointer-input.md) para obtener instrucciones sobre cómo crear experiencias personalizadas en aplicaciones Windows para dispositivos señaladores.

Para obtener más información general sobre la creación de aplicaciones y experiencias para el teclado, consulte [interacción del teclado](keyboard-interactions.md).

## <a name="general-guidance"></a>Instrucciones generales

Solo los elementos de la interfaz de usuario que requieren la interacción del usuario deben admitir la navegación del foco, los elementos que no requieren una acción, como las imágenes estáticas, no necesitan el foco de teclado. Los lectores de pantalla y las herramientas de accesibilidad similares todavía anuncian estos elementos estáticos, incluso cuando no están incluidos en la navegación con el foco. 

Es importante recordar que, a diferencia de la navegación con un dispositivo de puntero, como un mouse o una entrada táctil, la navegación del foco es lineal. Al implementar la navegación del foco, tenga en cuenta cómo interactuará un usuario con la aplicación y cuál debe ser la navegación lógica. En la mayoría de los casos, recomendamos que el comportamiento de navegación del foco personalizado siga el patrón de lectura preferido de la referencia cultural del usuario.

Otras consideraciones sobre la navegación de foco incluyen:

- ¿Los controles se agrupan lógicamente?
- ¿Hay grupos de controles con mayor importancia?
  - En caso afirmativo, ¿esos grupos contienen subgrupos?
- ¿Requiere el diseño la navegación direccional personalizada (teclas de dirección) y el orden de tabulación?

El libro electrónico [de Engineering Software for Accessibility](https://www.microsoft.com/download/details.aspx?id=19262) tiene un excelente capítulo sobre *el diseño de la jerarquía lógica*.

## <a name="2d-directional-navigation-for-keyboard"></a>navegación direccional 2D para el teclado

La región de navegación interna 2D de un control o grupo de control se conoce como su "área direccional". Cuando el foco se desplaza a este objeto, se pueden usar las teclas de dirección del teclado (izquierda, derecha, arriba y abajo) para desplazarse por los elementos secundarios dentro del área direccional.

![](images/keyboard/directional-area-small.png)
*región de navegación interna 2D de área direccional, o área direccional, de un grupo de controles*

Puede usar la propiedad [XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) (que tiene los valores posibles de [automático](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [habilitado](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)o [deshabilitado](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) para administrar la navegación interna 2D con las teclas de flecha del teclado.

> [!NOTE]
> El orden de tabulación no se ve afectado por esta propiedad. Para evitar una experiencia de navegación confusa, se recomienda que los elementos secundarios de un área direccional *no* se especifiquen explícitamente en el orden de navegación de la pestaña de la aplicación. Vea las propiedades [UIElement. TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) y [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para obtener más información sobre el comportamiento de tabulación de un elemento.

### <a name="auto-default-behavior"></a>[Auto](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (comportamiento predeterminado)

Cuando se establece en automático, el comportamiento de la navegación direccional viene determinado por la ascendencia del elemento o la jerarquía de herencia. Si todos los antecesores están en modo predeterminado (establecido en **automático**), *no* se admite la navegación direccional con el teclado.

### <a name="disabled"></a>[Deshabilitada](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Establezca **XYFocusKeyboardNavigation** en **Disabled** para bloquear la navegación direccional por el control y sus elementos secundarios.

![Comportamiento deshabilitado de XYFocusKeyboardNavigation ](images/keyboard/xyfocuskeyboardnav-disabled.gif)
 *XYFocusKeyboardNavigation comportamiento deshabilitado*

En este ejemplo, el [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) principal (ContainerPrimary) tiene **XYFocusKeyboardNavigation** establecido en **habilitado**. Todos los elementos secundarios heredan esta configuración y se puede navegar a ellas con las teclas de dirección. Sin embargo, los elementos B3 y B4 están en un [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secundario (ContainerSecondary) con **XYFocusKeyboardNavigation** establecido en **Disabled**, lo que invalida el contenedor principal y deshabilita la navegación por la tecla de dirección a sí misma y entre sus elementos secundarios.

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

### <a name="enabled"></a>[Habilitado](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Establezca **XYFocusKeyboardNavigation** en **habilitado** para admitir la navegación direccional 2D a un control y a cada uno de sus objetos secundarios [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) .

Cuando se establece, la navegación con las teclas de dirección está restringida a los elementos dentro del área direccional. La navegación por tabulación no se ve afectada, ya que todos los controles siguen siendo accesibles a través de su jerarquía de orden de tabulación.

![Comportamiento habilitado de XYFocusKeyboardNavigation de ](images/keyboard/xyfocuskeyboardnav-enabled.gif)
 *XYFocusKeyboardNavigation comportamiento habilitado*

En este ejemplo, el [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) principal (ContainerPrimary) tiene **XYFocusKeyboardNavigation** establecido en **habilitado**. Todos los elementos secundarios heredan esta configuración y se puede navegar a ellas con las teclas de dirección. Los elementos B3 y B4 se encuentran en un [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secundario (ContainerSecondary), donde **XYFocusKeyboardNavigation** no se establece, que luego hereda la configuración del contenedor principal. El elemento B5 no está dentro de un área direccional declarada y no admite la navegación por teclas de dirección, pero admite el comportamiento de navegación por tabulación estándar.

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

Puede tener varios niveles de áreas direccionales anidadas. Si todos los elementos primarios tienen XYFocusKeyboardNavigation establecido en habilitado, se omiten los límites de la región de navegación interna.

A continuación se muestra un ejemplo de dos áreas direccionales anidadas dentro de un elemento que no admite explícitamente la navegación direccional 2D. En este caso, no se admite la navegación direccional entre las dos áreas anidadas.

![XYFocusKeyboardNavigation habilitado y comportamiento anidado ](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)
 *XYFocusKeyboardNavigation habilitado y comportamiento anidado*

Este es un ejemplo más complejo de tres áreas direccionales anidadas en las que:

-   Cuando B1 tiene el foco, solo se puede navegar a B5 (y viceversa) porque hay un límite de área direccional en el que XYFocusKeyboardNavigation se establece en Disabled, lo que hace que B2, B3 y B4 no estén accesibles con las teclas de dirección.
-   Cuando B2 tiene el foco, solo se puede navegar a B3 (y viceversa) porque el límite del área direccional evita la navegación por la tecla de flecha a B1, B4 y B5
-   Cuando B4 tiene el foco, se debe usar la tecla TAB para desplazarse entre los controles.

![XYFocusKeyboardNavigation habilitado y comportamiento anidado complejo](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*XYFocusKeyboardNavigation habilitado y comportamiento anidado complejo*

## <a name="tab-navigation"></a>Navegación por tabulación

Aunque las teclas de dirección se pueden usar para la navegación direccional 2D witin un control o un grupo de controles, la tecla Tab se puede usar para desplazarse por todos los controles de una aplicación Windows. 

Todos los controles interactivos admiten la navegación por la tecla TAB de forma predeterminada (la propiedad[IsEnabled](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) y [IsTabStop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) es **true**), con el orden de tabulación lógico derivado del diseño de control en la aplicación. No obstante, el orden predeterminado no necesariamente corresponde al orden visual. La posición de visualización real probablemente dependa del contenedor de diseño primario y ciertas propiedades que puedes establecer en los elementos secundarios para influir en el diseño.

Evite un orden de tabulación personalizado que haga que el foco se desplace por la aplicación. Por ejemplo, una lista de controles de un formulario debe tener un orden de tabulación que fluya desde la parte superior a la inferior y de izquierda a derecha (según la configuración regional).

En esta sección se describe cómo se puede personalizar completamente el orden de las pestañas para que se adapten a la aplicación.

### <a name="set-the-tab-navigation-behavior"></a>Establecer el comportamiento de navegación por tabulación

La propiedad [TabFocusNavigation](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) de [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) especifica el comportamiento de navegación por tabulación para todo el árbol de objetos (o área direccional).

> [!NOTE]
> Use esta propiedad en lugar de la propiedad [control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) para los objetos que no usan una [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) para definir su apariencia.

Como se mencionó en la sección anterior, para evitar una experiencia de navegación confusa, se recomienda que los elementos secundarios de un área direccional *no* se especifiquen explícitamente en el orden de navegación de la pestaña de la aplicación. Vea las propiedades [UIElement. TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) y [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para obtener más información sobre el comportamiento de tabulación de un elemento.   
> En el caso de las versiones anteriores a Windows 10 Creators Update (compilación 10.0.15063), la configuración de las pestañas se limitaba a los objetos de [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) . Para obtener más información, vea [control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) tiene un valor de tipo [KeyboardNavigationMode](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) con los siguientes valores posibles (tenga en cuenta que estos ejemplos no son grupos de controles personalizados y no requieren navegación interna con las teclas de dirección):

- **Local** (valor predeterminado)   
  Los índices de tabulación se reconocen en el subárbol local dentro del contenedor. En este ejemplo, el orden de tabulación es B1, B2, B3, B4, B5, B6, B7, B1.

   ![Comportamiento de navegación por la pestaña "local"](images/keyboard/tabnav-local.gif)

   *Comportamiento de navegación por la pestaña "local"*

- **Una vez**  
  El contenedor y todos los elementos secundarios reciben el foco una vez. En este ejemplo, el orden de tabulación es B1, B2, B7, B1 (también se muestra la navegación interna con la tecla de dirección).

   ![Comportamiento de navegación de la pestaña "una vez"](images/keyboard/tabnav-once.gif)

   *Comportamiento de navegación de la pestaña "una vez"*

- **Interrumpa**   
  El foco vuelve al elemento que tiene el foco inicial dentro de un contenedor. En este ejemplo, el orden de tabulación es B1, B2, B3, B4, B5, B6, B2...

   ![Comportamiento de navegación por la pestaña "ciclo"](images/keyboard/tabnav-cycle.gif)

   *Comportamiento de navegación por la pestaña "ciclo"*

Este es el código de los ejemplos anteriores (con TabFocusNavigation = "Cycle").

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

### <a name="tabindex"></a>[TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Use [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) para especificar el orden en que los elementos reciben el foco cuando el usuario navega por los controles mediante la tecla TAB. Un control con un índice de tabulación inferior recibe el foco antes de un control con un índice superior.

Cuando un control no tiene ningún [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) especificado, se le asigna un valor de índice más alto que el valor de índice más alto actual (y la prioridad más baja) de todos los controles interactivos del árbol visual, basado en el ámbito. 

Todos los elementos secundarios de un control se consideran un ámbito y, si uno de estos elementos también tiene elementos secundarios, se consideran otro ámbito. Cualquier ambigüedad se resuelve eligiendo el primer elemento en el árbol visual del ámbito. 

Para excluir un control del orden de tabulación, establezca la propiedad [IsTabStop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) en **false**.

Reemplace el orden de tabulación predeterminado mediante el establecimiento de la propiedad [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) .

> [!NOTE] 
> [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) funciona de la misma manera con [UIElement. TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) y [control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).


Aquí se muestra cómo la propiedad [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) puede afectar a la navegación del foco en elementos específicos. 

![Navegación de la pestaña "local" con el comportamiento de TabIndex](images/keyboard/tabnav-tabindex.gif)

*Navegación de la pestaña "local" con el comportamiento de TabIndex*

En el ejemplo anterior, hay dos ámbitos: 
- B1, área direccional (B2-B6) y B7
- área direccional (B2-B6)

Cuando B3 (en el área direccional) recibe el foco, el ámbito cambia y las transferencias de navegación por tabulación al área direccional en la que se identifica el mejor candidato para el foco subsiguiente. En este caso, B2 seguido de B4, B5 y B6. A continuación, el ámbito vuelve a cambiar y el foco se mueve a B1.

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

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>navegación direccional 2D para el teclado, el controlador para juegos y el control remoto

Los tipos de entrada que no son de puntero, como el teclado, el controlador de juegos, el control remoto y las herramientas de accesibilidad como el narrador de Windows, comparten un mecanismo común subyacente para navegar por la interfaz de usuario de la aplicación Windows e interactuar con ella.

En esta sección, se explica cómo especificar una estrategia de navegación preferida y ajustar la navegación del foco dentro de la aplicación a través de un conjunto de propiedades de estrategia de navegación que admiten todos los tipos de entrada que no son de puntero basados en el foco.

Para obtener más información general sobre la creación de aplicaciones y experiencias para Xbox/TV, consulte [interacción](keyboard-interactions.md)con el teclado, [diseño para Xbox y TV](../devices/designing-for-tv.md), y [interacciones de control remoto y controlador para juegos](gamepad-and-remote-interactions.md).

### <a name="navigation-strategies"></a>Estrategias de navegación

> Las estrategias de navegación se aplican al teclado, el controlador para juegos, el control remoto y diversas herramientas de accesibilidad.

Las siguientes propiedades de la estrategia de navegación permiten influir en el control que recibe el foco en función de la tecla de dirección, el botón panel de direcciones (almohadilla D) o similar presionado. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Estas propiedades tienen valores posibles de [auto](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (valor predeterminado), [NavigationDirectionDistance](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projection](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)o [RectilinearDistance ](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy).

Si se establece en **automático**, el comportamiento del elemento se basa en los antecesores del elemento. Si todos los elementos están establecidos en **automático**, se utiliza la **proyección** .

> [!NOTE]
> Otros factores, como el elemento con el foco anterior o la proximidad al eje de la dirección de navegación, pueden influir en el resultado.

### <a name="projection"></a>Proyección

La estrategia de proyección mueve el foco al primer elemento encontrado cuando el borde del elemento enfocado actualmente se *proyecta* en la dirección de navegación.

En este ejemplo, cada dirección de navegación del foco se establece en proyección. Observe cómo el foco se desplaza hacia abajo de B1 a B4, omitiendo B3. Esto se debe a que B3 no está en la zona de proyección. Observe también que no se identifica un candidato de foco al moverse a la izquierda desde B1. Esto se debe a que la posición de B2 relativa a B1 elimina B3 como candidato. Si B3 estuviera en la misma fila que B2, sería un candidato viable para la navegación izquierda. B2 es un candidato viable debido a su proximidad del eje de dirección de navegación.

![Estrategia de navegación de proyección](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Estrategia de navegación de proyección*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

La estrategia NavigationDirectionDistance mueve el foco al elemento más cercano al eje de la dirección de navegación.

El borde del rectángulo delimitador correspondiente a la dirección de navegación se *extiende* y se *proyecta* para identificar los destinos candidatos. El primer elemento encontrado se identifica como el destino. En el caso de varios candidatos, el elemento más cercano se identifica como el destino. Si todavía hay varios candidatos, el elemento superior/izquierdo se identifica como candidato.

![Estrategia de navegación NavigationDirectionDistance](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Estrategia de navegación NavigationDirectionDistance*

### <a name="rectilineardistance"></a>RectilinearDistance

La estrategia RectilinearDistance mueve el foco al elemento más cercano en función de la distancia rectilínea 2D ([geometría taxis](https://en.wikipedia.org/wiki/Taxicab_geometry)).

La suma de la distancia principal y la distancia secundaria a cada posible candidato se usa para identificar el mejor candidtate. En un empate, se selecciona el primer elemento de la izquierda si la dirección solicitada es hacia arriba o hacia abajo, y se selecciona el primer elemento de la parte superior si la dirección solicitada es izquierda o derecha.

![Estrategia de navegación RectilinearDistance](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Estrategia de navegación RectilinearDistance*

En esta imagen se muestra cómo, cuando B1 tiene el foco y hacia abajo es la dirección solicitada, B3 es el candidato para el foco de RectilinearDistance. Esto se basa en los siguientes calcualations para este ejemplo:
-   Distance (B1, B3, Down) es 10 + 0 = 10
-   Distance (B1, B2, Down) es 0 + 40 = 30
-   Distancia (B1, D, Down) es 30 + 0 = 30


## <a name="related-articles"></a>Artículos relacionados
- [Navegación con foco mediante programación](focus-navigation-programmatic.md)
- [Interacciones de teclado](keyboard-interactions.md)
- [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md)
---
author: Jwmsft
Description: You can customize a control's visual structure and visual behavior by creating a control template in the XAML framework.
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Plantillas de control
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ae344e9f10c5d1dbfd530950851e402da4bc2a0d
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5523080"
---
# <a name="control-templates"></a>Plantillas de control

 

Puedes personalizar la estructura y el comportamiento visual de un control al crear una plantilla de control en el marco XAML. Los controles tienen muchas propiedades, como [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) y [**FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209404), que puedes establecer para especificar diferentes aspectos de la apariencia del control. Sin embargo, los cambios que puedes realizar estableciendo estas propiedades son limitados. Puedes especificar personalizaciones adicionales al crear una plantilla con la clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). A continuación te mostramos cómo crear una clase **ControlTemplate** para personalizar la apariencia de un control de [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316).

> **API importantes**: [**Clase ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), [**Propiedad Control.Template**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.template.aspx)


## <a name="custom-control-template-example"></a>Ejemplo de plantilla de control personalizada


De manera predeterminada, un control de [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) coloca su contenido (la cadena u objeto junto a la clase **CheckBox**) a la derecha del cuadro de selección y una marca de verificación indica que el usuario seleccionó la clase **CheckBox**. Estas características representan la estructura visual y el comportamiento visual de la clase **CheckBox**.

Esta es una clase [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) que usa la clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) predeterminada que se muestra con los estados `Unchecked`, `Checked` e `Indeterminate`.

![plantilla checkbox predeterminada](images/templates-checkbox-states-default.png)

Para cambiar estas características, puedes crear una clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) para la clase [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316). Por ejemplo, si quieres que el contenido de la casilla esté debajo del cuadro de selección y quieres usar una **X** para indicar que el usuario seleccionó la casilla. Estas características se especifican en la clase **ControlTemplate** de la clase **CheckBox**.

Para usar una plantilla personalizada con un control, asigna la clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) a la propiedad [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) del control. Esta es una clase [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) con una clase **ControlTemplate** llamada `CheckBoxTemplate1`. En la siguiente sección mostramos el lenguaje de marcado de aplicaciones extensible (XAML) de la clase **ControlTemplate**.

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

Este es el aspecto de la clase [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) con los estados `Unchecked`, `Checked` e `Indeterminate` después de aplicar nuestra plantilla.

![plantilla checkbox personalizada](images/templates-checkbox-states.png)

## <a name="specify-the-visual-structure-of-a-control"></a>Especificar la estructura visual de un control


Al crear una clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), combinas objetos [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) para crear un único control. Una clase **ControlTemplate** debe tener solo una clase **FrameworkElement** como elemento raíz. Normalmente, el elemento raíz contiene otros objetos **FrameworkElement**. La combinación de objetos conforma la estructura visual del control.

Este código XAML crea una clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) para una clase [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) que especifica que el contenido del control se sitúa debajo del cuadro de selección. El elemento raíz es una clase [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250). El ejemplo especifica una clase [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) para crear una **X** que indica que un usuario seleccionó la clase **CheckBox** y una clase [**Elipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) que indica que está en estado indeterminado. Observa que la [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) se estableció en 0 en la clase **Path** y **Ellipse**, por lo que, de manera predeterminada, no aparece ninguna de ellos.

Un [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) es un enlace especial que vincula el valor de una propiedad de una plantilla de control al valor de otra propiedad expuesta en el control con plantilla. TemplateBinding solo se puede usar dentro de una definición de ControlTemplate en XAML. Consulta [Extensión de revisión de TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) para obtener más información.

> [!NOTE]
> A partir de la siguiente actualización importante a Windows 10, puedes usar las extensiones de marcado [**x: Bind**](https://msdn.microsoft.com/library/windows/apps/Mt204783) en lugares usas [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md). Consulta [Extensión de revisión de TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) para obtener más información.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}" 
            BorderThickness="{TemplateBinding BorderThickness}" 
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20" 
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}" 
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph" 
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z" 
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                  FlowDirection="LeftToRight" 
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph" 
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter" 
                              ContentTemplate="{TemplateBinding ContentTemplate}" 
                              Content="{TemplateBinding Content}" 
                              Margin="{TemplateBinding Padding}" Grid.Row="1" 
                              HorizontalAlignment="Center" 
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## <a name="specify-the-visual-behavior-of-a-control"></a>Especifica el comportamiento visual de un control


Un comportamiento visual especifica la apariencia de un control cuando tiene un estado determinado. El control de [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) tiene tres estados de selección: `Checked`, `Unchecked` e `Indeterminate`. El valor de la propiedad [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798) determina el estado de **CheckBox**, y su estado determina lo que aparece en el cuadro.

Esta tabla enumera los posibles valores de [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798), los estados correspondientes de [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) y la apariencia de **CheckBox**.

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| Valor **IsChecked** | Estado de **CheckBox** | Aspecto de **CheckBox** |
| **true**            | `Checked`          | Contiene una "X".        |
| **false**           | `Unchecked`        | Vacío.                  |
| **nulo**            | `Indeterminate`    | Contiene un círculo.      |

 

La apariencia de un control se especifica cuando tiene un estado determinado mediante objetos [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007). Un **VisualState** contiene una clase [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br243053) que cambia la apariencia de los elementos de la clase [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). Cuando el control ingresa en el estado que especifica la propiedad [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031), se aplican los cambios de propiedad en **Setter** o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490). Cuando el control sale del estado, se eliminan los cambios. Agregas objetos **VisualState** a objetos [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014). Agregas objetos **VisualStateGroup** a la propiedad adjunta [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505), que se establece en la raíz [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) de la clase **ControlTemplate**.

Este código XAML muestra los objetos [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) para los estados `Checked`, `Unchecked` e `Indeterminate`. El ejemplo establece la propiedad adjunta [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) en [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), que es el elemento raíz de [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). **VisualState** `Checked` especifica que el valor de [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de la clase [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) denominada `CheckGlyph` (que mostramos en el ejemplo anterior) es 1. **VisualState** `Indeterminate` especifica que el valor de **Opacity** de la clase [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) denominada `IndeterminateGlyph` es 1. **VisualState** `Unchecked` no tiene ninguna clase [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) o [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490), por lo que [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) vuelve a su apariencia predeterminada.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}" 
            BorderThickness="{TemplateBinding BorderThickness}" 
            Background="{TemplateBinding Background}">
            
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1" 
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20" 
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}" 
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph" 
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z" 
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                  FlowDirection="LeftToRight" 
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph" 
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter" 
                              ContentTemplate="{TemplateBinding ContentTemplate}" 
                              Content="{TemplateBinding Content}" 
                              Margin="{TemplateBinding Padding}" Grid.Row="1" 
                              HorizontalAlignment="Center" 
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

Para comprender mejor cómo funcionan los objetos [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), considera lo que sucede cuando [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) pasa del estado `Unchecked` al estado `Checked`, después al estado `Indeterminate` y después de nuevo al estado `Unchecked`. Estas son las transiciones:

|                                      |                                                                                                                                                                                                                                                                                                                                                |                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| Transición de estado                     | Qué sucede                                                                                                                                                                                                                                                                                                                                   | Apariencia de CheckBox cuando la transición finaliza |
| De `Unchecked` a `Checked`.       | Se aplica el valor [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) `Checked`, por lo tanto, el valor de [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de `CheckGlyph` es 1.                                                                                                                                                         | Se muestra una X.                                |
| De `Checked` a `Indeterminate`.   | Se aplica el valor [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) `Indeterminate`, por lo tanto, el valor de [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de `IndeterminateGlyph` es 1. Se quita el valor **Setter** de **VisualState** `Checked`, por lo tanto, el valor de [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br228078) de `CheckGlyph` es 0. | Se muestra un círculo.                            |
| De `Indeterminate` a `Unchecked`. | Se quita el valor [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) `Indeterminate`, por lo tanto, el valor de [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de `IndeterminateGlyph` es 0.                                                                                                                                           | No se muestra nada.                             |

 
Para obtener más información sobre cómo crear estados visuales para los controles (y, más en concreto, sobre cómo usar la clase [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) y los tipos de animación), consulta [Animaciones de guion gráfico para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808).

## <a name="use-tools-to-work-with-themes-easily"></a>Usar herramientas para facilitar el trabajo con temas

Una manera rápida de aplicar temas a los controles es hacer clic con el botón secundario en un control de **Esquema del documento** de Microsoft Visual Studio y seleccionar **Editar tema** o **Editar estilo** (según el control en el que estás haciendo clic con el botón secundario). Después, para aplicar un tema existente, selecciona **Aplicar recurso** o selecciona **Crear vacío** para crear uno nuevo.

## <a name="controls-and-accessibility"></a>Controles y accesibilidad

Cuando creas una plantilla para un control, aparte de estar cambiando el comportamiento y la apariencia visual del control en cierta medida, es probable que estés modificando también el modo en que dicho control se muestra en los marcos de accesibilidad. La Plataforma universal de Windows (UWP) admite el marco de automatización de la interfaz de usuario de Microsoft para la accesibilidad. Todos los controles predeterminados y sus plantillas correspondientes admiten los patrones y tipos de controles comunes de la automatización de la interfaz de usuario que sean apropiados para el propósito y la función del control en cuestión. Los clientes de automatización de la interfaz de usuario interpretan estos patrones y tipos de controles como tecnologías de ayuda, lo que permite que un control esté accesible dentro de una interfaz de usuario de aplicación accesible más grande.

Para separar la lógica de control básica y, asimismo, cumplir algunos de los requisitos de arquitectura de la automatización de la interfaz de usuario, las clases de los controles incorporan la compatibilidad con la accesibilidad en una clase aparte: un sistema de automatización del mismo nivel. A veces, los sistemas de automatización del mismo nivel interactúan con las plantillas de control, ya que esperan que en ellas haya determinados elementos con nombre que permitan habilitar tecnologías de ayuda para invocar acciones de botones.

Cuando crees un control personalizado completamente nuevo, habrá veces en las que también quieras crear un sistema de automatización del mismo nivel que lo complemente. Para más información, consulta [Personalizar sistemas de automatización del mismo nivel](../accessibility/custom-automation-peers.md).

## <a name="learn-more-about-a-controls-default-template"></a>Obtener más información sobre la plantilla predeterminada de un control

En los artículos que documentan los estilos y plantillas de los controles XAML se muestran fragmentos del mismo código XAML de inicio que verías si usaras las técnicas **Editar tema** o **Editar estilo** explicadas anteriormente. Cada artículo enumera los nombres de los estados visuales, los recursos de tema utilizados y el código XAML completo del estilo que contiene la plantilla. Estos artículos pueden resultar útiles si ya has comenzado a modificar una plantilla y quieres ver el aspecto de la plantilla original o comprobar que tu nueva plantilla tenga todos los estados visuales con nombre necesarios.

## <a name="theme-resources-in-control-templates"></a>Recursos de temas en plantillas de control

Para algunos de los atributos de los ejemplos de XAML, quizás hayas observado referencias a recursos que usan la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md). Esta es una técnica que permite a una única plantilla de control usar recursos que pueden ser diferentes valores en función del tema que esté activo en cada momento. Esto resulta especialmente importante para pinceles y colores, porque el principal propósito de los temas es permitir a los usuarios elegir si quieren aplicar un tema oscuro, claro o de alto contraste a todo el sistema. Las aplicaciones que usan el sistema de recursos de XAML pueden usar un conjunto de recursos que sea apropiado para ese tema, de manera que las opciones de temas en la interfaz de usuario de una aplicación reflejen el tema elegido por el usuario para todo el sistema.

 ## Obtener el código de muestra
* [Muestra de conceptos básicos de una interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Muestra de control de edición de texto personalizado](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomEditControl)

 




---
description: Los estilos te permiten establecer propiedades de control y reusar esa configuración para mantener un aspecto uniforme en varios controles.
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
ms.date: 01/03/2019
title: Estilos XAML
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: XAML styles
template: detail.hbs
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6e4e69b87ba78134982032f9ceca826758c1a5ba
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829654"
---
# <a name="xaml-styles"></a>Estilos XAML





El marco XAML te permite personalizar la apariencia de tus aplicaciones de varias maneras. Los estilos te permiten establecer propiedades de control y reusar esa configuración para mantener un aspecto uniforme en varios controles.

## <a name="style-basics"></a>Conceptos básicos de estilos

Los estilos te permiten extraer opciones de configuración de propiedades visuales en recursos reutilizables. Este es un ejemplo en el que se muestran tres botones con un estilo que establece las propiedades [BorderBrush](/uwp/api/windows.ui.xaml.controls.control.borderbrush), [BorderThickness](/uwp/api/windows.ui.xaml.controls.control.borderthickness) y [Foreground](/uwp/api/windows.ui.xaml.controls.control.foreground). Al aplicar un estilo, puedes hacer que todos los controles tengan el mismo aspecto no tener que establecer estas propiedades en cada control de manera independiente.

![Captura de pantalla de tres botones con estilo organizados en paralelo.](images/styles-rainbow-buttons.png)

Puedes definir un estilo en línea en el lenguaje XAML para un control o como una fuente reutilizable. Define recursos en un archivo XAML de una página individual, en el archivo App.xaml o en un archivo XAML de diccionario de recursos independiente. Varias aplicaciones pueden compartir un mismo archivo XAML de diccionario de recursos y se pueden combinar varios diccionarios de recursos en una sola aplicación. El lugar donde se define el recurso determina el ámbito en el que puede usarse. Los recursos de nivel de página se encuentran disponibles solo en la página donde se definen. Si tanto en App.xaml como en la página se definen con la misma clave, el recurso de la página invalida al recurso de App.xaml. Si un recurso se define en un archivo de diccionario de recursos diferente, su ámbito se determina por el lugar donde se hace referencia al diccionario de recursos.

En la definición de [Style](/uwp/api/Windows.UI.Xaml.Style), necesitas un atributo [TargetType](/uwp/api/windows.ui.xaml.style.targettype) y una colección de uno o más elementos [Setter](/uwp/api/Windows.UI.Xaml.Setter). El atributo **TargetType** es una cadena que especifica un tipo [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement) al que aplicarle el estilo. El valor **TargetType** debe especificar un tipo derivado de **FrameworkElement** definido por Windows Runtime o un tipo personalizado que esté disponible en un ensamblado referido. Si intentas aplicar un estilo a un control y el tipo del control no coincide con el atributo **TargetType** de dicho estilo, se produce una excepción.

Cada elemento [Setter](/uwp/api/Windows.UI.Xaml.Setter) requiere una propiedad [Property](/uwp/api/windows.ui.xaml.setter.property) y una propiedad [Value](/uwp/api/windows.ui.xaml.setter.value). Esta configuración de las propiedades indica a qué propiedades del control se aplica la configuración, y el valor que se fija para esa propiedad. Puedes establecer **Setter.Value** con sintaxis de atributo o elemento de propiedad. El XAML muestra el estilo que se aplica a los botones que se mostraron anteriormente. En este XAML, los dos primeros elementos **Setter** usan la sintaxis de atributo, pero el último **Setter**, para la propiedad [BorderBrush](/uwp/api/windows.ui.xaml.controls.control.borderbrush), usa la sintaxis de elemento de propiedad. En el ejemplo no se usa el atributo [x:Key](../../xaml-platform/x-key-attribute.md), por lo que el estilo se aplica implícitamente a los botones. La aplicación de estilos de manera implícita o explícita se explica en la siguiente sección.

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Black" />
        <Setter Property="BorderBrush" >
            <Setter.Value>
                <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                    <GradientStop Color="Yellow" Offset="0.0" />
                    <GradientStop Color="Red" Offset="0.25" />
                    <GradientStop Color="Blue" Offset="0.75" />
                    <GradientStop Color="LimeGreen" Offset="1.0" />
                </LinearGradientBrush>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<StackPanel Orientation="Horizontal">
    <Button Content="Button"/>
    <Button Content="Button"/>
    <Button Content="Button"/>
</StackPanel>
```

## <a name="apply-an-implicit-or-explicit-style"></a>Aplicar un estilo implícito o explícito

Si defines un estilo como un recurso, puedes aplicarlo a los controles de dos maneras:

-   De manera implícita, al especificar únicamente de un [TargetType](/uwp/api/windows.ui.xaml.style.targettype) para [Style](/uwp/api/Windows.UI.Xaml.Style).
-   De manera explícita, al especificar un [TargetType](/uwp/api/windows.ui.xaml.style.targettype) y un [atributo x:Key](../../xaml-platform/x-key-attribute.md) para [Style](/uwp/api/Windows.UI.Xaml.Style) y, después, establecer la propiedad [Style](/uwp/api/windows.ui.xaml.frameworkelement.style) del control de destino con una referencia de [extensión de marcado {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) que usa clave explícita.

Si un estilo contiene el [atributo x:Key](../../xaml-platform/x-key-attribute.md), solo puedes aplicarlo a un control mediante el establecimiento de la propiedad [Style](/uwp/api/windows.ui.xaml.frameworkelement.style) del control en el estilo con clave. Por el contrario, un estilo sin un atributo x:Key se aplica automáticamente a cada control de su tipo de destino si el control no tiene una opción de estilo explícita.

Aquí hay dos botones que muestran los estilos implícitos y explícitos.

![botones con estilos implícitos y explícitos.](images/styles-buttons-implicit-explicit.png)

En este ejemplo, el primer estilo tiene un [atributo x:Key](../../xaml-platform/x-key-attribute.md) y su tipo de destino es [Button](/uwp/api/Windows.UI.Xaml.Controls.Button). La propiedad [Style](/uwp/api/windows.ui.xaml.frameworkelement.style) del primer botón se establece en esta clave, por lo que este estilo se aplica de manera explícita. El segundo estilo se aplica de manera implícita al segundo botón porque su tipo de destino es **Botón** y el estilo no tiene un atributo x:Key.

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="Purple"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Green"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Green"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button"/>
</Grid>
```

## <a name="use-based-on-styles"></a>Usar estilos heredados

Para crear estilos que sean más fáciles de mantener y optimizar la reutilización de estilos, puedes crear estilos que hereden de otros estilos. Usa la propiedad [BasedOn](/uwp/api/windows.ui.xaml.style.basedon) para crear estilos heredados. Los estilos que heredan de otros estilos deben apuntar al mismo tipo de control o a uno que derive del tipo al que apunta el estilo base. Por ejemplo, si un estilo base apunta a [ContentControl](/uwp/api/Windows.UI.Xaml.Controls.ContentControl), los estilos que estén basados en este estilo pueden apuntar a **ContentControl** o a tipos que deriven de **ContentControl**, como [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) y [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). Si no se establece un valor en el estilo heredado, se hereda del estilo base. Para cambiar un valor del estilo base, el estilo heredado invalida ese valor. En el siguiente ejemplo se muestra un elemento **Button** y un elemento [CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) con estilos que heredan del mismo estilo base.

![botones con estilos que usan estilos heredados.](images/styles-buttons-based-on.png)

El estilo base apunta a [ContentControl](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) y establece las propiedades [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width). Los estilos basados en este estilo apuntan a [CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) y [Button](/uwp/api/Windows.UI.Xaml.Controls.Button), que derivan de **ContentControl**. Los estilos heredados establecen distintos colores para las propiedades [BorderBrush](/uwp/api/windows.ui.xaml.controls.control.borderbrush) y [Foreground](/uwp/api/windows.ui.xaml.controls.control.foreground). (Normalmente no se coloca un borde alrededor de una **Casilla**. Lo hacemos aquí para mostrar los efectos del estilo.)

```XAML
<Page.Resources>
    <Style x:Key="BasicStyle" TargetType="ContentControl">
        <Setter Property="Width" Value="130" />
        <Setter Property="Height" Value="30" />
    </Style>

    <Style x:Key="ButtonStyle" TargetType="Button"
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Orange" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Red" />
    </Style>

    <Style x:Key="CheckBoxStyle" TargetType="CheckBox"
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Green" />
    </Style>
</Page.Resources>

<StackPanel>
    <Button Content="Button" Style="{StaticResource ButtonStyle}" Margin="0,10"/>
    <CheckBox Content="CheckBox" Style="{StaticResource CheckBoxStyle}"/>
</StackPanel>
```

## <a name="use-tools-to-work-with-styles-easily"></a>Usar herramientas para trabajar con estilos fácilmente

Una manera rápida de aplicar estilos a los controles es hacer clic con el botón secundario en un control de la superficie de diseño XAML de Microsoft Visual Studio y seleccionar **Editar estilo** o **Editar plantilla** (según el control en el que estás haciendo clic con el botón secundario). Después, puedes aplicar un estilo existente si seleccionas **Aplicar recurso** o definir un estilo nuevo si seleccionas **Crear vacío**. Si creas un estilo vacío, tienes la opción de definirlo en la página, en el archivo App.xaml o en un diccionario de recursos independiente.

## <a name="lightweight-styling"></a>Estilos ligeros

Por norma general, los pinceles del sistema se reemplazan en el nivel de página o aplicación y, en cualquier caso, la sustitución del color afectará a todos los controles que hacen referencia a ese pincel. Además, en XAML, muchos controles pueden hacer referencia al mismo pincel del sistema.

![Captura de pantalla de dos botones: uno en su estado de reposo y otro con un estilo ligero aplicado.](images/LightweightStyling_ButtonStatesExample.png)

```XAML
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                 <SolidColorBrush x:Key="ButtonBackground" Color="Transparent"/>
                 <SolidColorBrush x:Key="ButtonForeground" Color="MediumSlateBlue"/>
                 <SolidColorBrush x:Key="ButtonBorderBrush" Color="MediumSlateBlue"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>
```

Para estados como PointerOver (se sitúa el mouse sobre el botón), **PointerPressed** (se ha invocado el botón), o Disabled (el botón no es interactivos). Estos finales se anexan a los nombres de estilo ligero originales: **ButtonBackgroundPointerOver**, **ButtonForegroundPointerPressed**, **ButtonBorderBrushDisabled**, etc. Al modificar esos pinceles, te asegurarás de que tus controles tengan colores homogéneos en el tema de la aplicación.

Al colocar estos reemplazos de pincel en el nivel de **recursos de la aplicación**, se modificarán todos los botones de toda la aplicación, en lugar de en una sola página.

### <a name="per-control-styling"></a>Estilos según el control

En otros casos se desea modificar un solo control de una página para que tenga un aspecto determinado, sin alterar ninguna otra versión de ese control:

![Captura de pantalla de tres botones con estilo organizados apilados uno encima del otro.](images/LightweightStyling_CheckboxExample.png)

```XAML
<CheckBox Content="Normal CheckBox" Margin="5"/>
<CheckBox Content="Special CheckBox" Margin="5">
    <CheckBox.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary x:Key="Light">
                    <SolidColorBrush x:Key="CheckBoxForegroundUnchecked"
                        Color="Purple"/>
                    <SolidColorBrush x:Key="CheckBoxForegroundChecked"
                        Color="Purple"/>
                    <SolidColorBrush x:Key="CheckBoxCheckGlyphForegroundChecked"
                        Color="White"/>
                    <SolidColorBrush x:Key="CheckBoxCheckBackgroundStrokeChecked"  
                        Color="Purple"/>
                    <SolidColorBrush x:Key="CheckBoxCheckBackgroundFillChecked"
                        Color="Purple"/>
                </ResourceDictionary>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </CheckBox.Resources>
</CheckBox>
<CheckBox Content="Normal CheckBox" Margin="5"/>
```

Esto solo afectaría a esa "casilla especial" en la página donde estuviera el control.

## <a name="modify-the-default-system-styles"></a>Modificar los estilos predeterminados del sistema

Debes usar los estilos que vienen con los recursos XAML predeterminados de Windows Runtime cada vez que puedas. Cuando tengas que definir tus propios estilos, intenta basarte en los predeterminados siempre que sea posible (para ello, usa estilos heredados como se explicó antes o empieza a editar una copia del estilo predeterminado original).

## <a name="the-template-property"></a>La propiedad Template

Se puede usar un establecedor de estilo para la propiedad [Template](/uwp/api/windows.ui.xaml.controls.control.template) de [Control](/uwp/api/Windows.UI.Xaml.Controls.Control). De hecho, esto conforma la mayor parte de un estilo XAML típico y los recursos XAML de una aplicación. Esto se explica con más detalle en el tema [Plantillas de control](control-templates.md).
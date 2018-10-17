---
author: Karl-Bridge-Microsoft
Description: Learn how to improve both the usability and the accessibility of your UWP app by providing an intuitive way for users to quickly navigate and interact with an app's visible UI through a keyboard instead of a pointer device (such as touch or mouse).
title: Directrices de diseño de las teclas de acceso
label: Access keys design guidelines
keywords: teclado, tecla de acceso, KeyTip, sugerencia de teclas, accesibilidad, navegación, foco, texto, entrada, interacción del usuario
template: detail.hbs
ms.author: kbridge
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8e842d6c5b8e62a9c043c97849fdf17f524ccfc7
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4693773"
---
# <a name="access-keys"></a>Teclas de acceso

Las teclas de acceso son accesos directos de teclado que mejoran la facilidad de uso y la accesibilidad de tus aplicaciones Windows si ofreces a los usuarios una forma intuitiva para que naveguen e interactúen rápidamente con la interfaz de usuario visible de la aplicación a través de un teclado en lugar de un dispositivo señalador (por ejemplo, la función táctil o el mouse).

Consulta el tema [Teclas de aceleración](keyboard-accelerators.md) para obtener más información sobre cómo invocar acciones comunes en una aplicación Windows con accesos directos de teclado. 

> [!NOTE]
> Un teclado es indispensable para los usuarios que tienen ciertas discapacidades (véase [Accesibilidad del teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)) y también es una herramienta importante para los usuarios que lo prefieren como una forma más eficaz de interactuar con una aplicación.

La Plataforma universal de Windows(UWP) ofrece compatibilidad integrada en los controles de la plataforma tanto para teclas de acceso basadas en el teclado como para comentarios de la interfaz de usuario asociada a través de indicaciones visuales llamadas sugerencias de teclas.

## <a name="overview"></a>Introducción

Una tecla de acceso es una combinación de la tecla Alt y una o más teclas alfanuméricas, lo que sirve como ayuda *mnemotécnica*, y que en general se presionan en secuencia y no de forma simultánea.

Las sugerencias de teclas son identificadores que se muestran junto a los controles que admiten las teclas de acceso cuando el usuario presiona la tecla Alt. Cada sugerencia de teclas contiene las teclas alfanuméricas que activan el control asociado.

> [!NOTE]
> Los métodos abreviados de teclado se admiten automáticamente para las teclas de acceso con un único carácter alfanumérico. Por ejemplo, al presionar Alt+A simultáneamente en Word, se abre el menú Archivo sin mostrar las sugerencias de teclas.

Al presionar la tecla Alt, se inicializa la funcionalidad de las teclas de acceso y se muestran todas las combinaciones de teclas actualmente disponibles en las sugerencias de teclas. Las pulsaciones de teclas posteriores se controlan mediante el marco de teclas de acceso, que rechaza las teclas no válidas hasta que se presiona una tecla de acceso válida, o se presionan las teclas ENTRAR, Esc, Tab o las flechas para desactivar las teclas de acceso y devolver el control de las pulsaciones de teclas a la aplicación.

Las aplicaciones de Microsoft Office admiten una amplia variedad de teclas de acceso. La siguiente imagen muestra la pestaña Inicio de Word con las teclas de acceso activadas (observa que admite tanto números como varias pulsaciones de teclas).

![Indicaciones de KeyTips de las teclas de acceso en Microsoft Word](images/accesskeys/keytip-badges-word.png)

_Indicaciones de KeyTips de las claves de acceso en Microsoft Word_

Para agregar una tecla de acceso a un control, usa la **propiedad AccessKey**. El valor de esta propiedad especifica la secuencia de teclas de acceso, el acceso directo (si se trata de un único carácter alfanumérico) y la sugerencia de teclas.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Cuándo usar las teclas de acceso

Te recomendamos que especifiques las teclas de acceso siempre que sea adecuado en la interfaz de usuario y admitas las teclas de acceso en todos los controles personalizados.

1.  **Las teclas de acceso hacen que la aplicación sea más accesible** para los usuarios que tienen discapacidades motrices, incluidos aquellos usuarios que se pueden presionar solo una tecla a la vez o tienen dificultades para usar un mouse.

    Una interfaz de usuario de teclado bien diseñada es un aspecto importante de la accesibilidad del software. Permite a usuarios con dificultades visuales o con ciertas discapacidades motrices navegar por una aplicación e interactuar con sus funciones. Es posible que estos usuarios no puedan controlar un mouse y empleen varias tecnologías de ayuda, como herramientas para la mejora del teclado, teclados en pantalla, ampliadores de pantallas, lectores de pantalla y utilidades de entrada de voz. Para estos usuarios, es fundamental que la cobertura completa de los comandos.

2.  **Las teclas de acceso hacen que tu aplicación sea más usable** para usuarios avanzados que prefieren interactuar a través del teclado.

    Los usuarios con experiencia suelen tener una fuerte preferencia por el teclado, ya que los comandos se pueden introducir más rápidamente y no requieren apartar las manos de las teclas. Para estos usuarios, la eficacia y la coherencia son cruciales; la exhaustividad es importante solo para los comandos usados con más frecuencia.

## <a name="set-access-key-scope"></a>Establecer el ámbito de las teclas de acceso

Cuando hay muchos de elementos en la pantalla que admiten teclas de acceso, te recomendamos definir los ámbitos de las teclas de acceso para reducir la **carga cognitiva**. Esto minimiza el número de teclas de acceso en la pantalla, lo que hace que sean más fáciles de encontrar y mejora la eficacia y la productividad.

Por ejemplo, Microsoft Word proporciona dos ámbitos de teclas de acceso: un ámbito principal para las pestañas de la Cinta y un ámbito secundario para los comandos de la pestaña seleccionada.

Las imágenes siguientes muestran los dos ámbitos de teclas de acceso en Word. La primera muestra las teclas de acceso principales que permiten al usuario seleccionar una pestaña y otros comandos de nivel superior, mientras que la segunda muestra las claves de acceso secundarias de la pestaña Inicio.

![Teclas de acceso principal en Microsoft Word](images/accesskeys/primary-access-keys-word.png)
_Teclas de acceso principal en Microsoft Word_

![Teclas de acceso secundario en Microsoft Word](images/accesskeys/secondary-access-keys-word.png)
_Teclas de acceso secundario en Microsoft Word_

Las teclas de acceso pueden duplicarse para elementos de distintos ámbitos. En el ejemplo anterior, "2" es la tecla de acceso para Deshacer en el ámbito principal y también es "Cursiva" en el ámbito secundario.

Aquí te mostramos cómo definir un ámbito de las teclas de acceso.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![Teclas de acceso principales para CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Ámbito principal de CommandBar y teclas de acceso admitidas_

![Teclas de acceso secundarias para CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Ámbito secundario de CommandBar y teclas de acceso admitidas_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Fall Creators Update y anteriores

Antes de Windows 10 Fall Creators Update, algunos controles como CommandBar no admitían ámbitos integrados de teclas de acceso.

En el siguiente ejemplo se muestra cómo admitir SecondaryCommands de CommandBar con las teclas de acceso que están disponibles una vez que se invoca un comando principal (similar a la Cinta en Word).

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>Evitar colisiones de teclas de acceso

Las colisiones de teclas de acceso se producen cuando dos o más elementos en el mismo ámbito tienen teclas de acceso duplicadas o comienzan con los mismos caracteres alfanuméricos.

Para resolver las teclas de acceso duplicadas, el sistema procesa la tecla de acceso del primer elemento agregado al árbol visual y pasa por alto todas las demás.

Cuando varias teclas de acceso comienzan con el mismo carácter (por ejemplo, "A", "A1" y "AB"), el sistema procesa la tecla de acceso de un solo carácter y omite todas las demás.

Para evitar las colisiones, usa teclas de acceso únicas o define ámbitos para los comandos.

## <a name="choose-access-keys"></a>Elegir teclas de acceso

Al elegir las teclas de acceso, ten en cuenta lo siguiente:

-   Usa un único carácter para minimizar las pulsaciones de teclas y admite las teclas de aceleración de manera predeterminada (Alt+AccessKey).
-   Evita usar más de dos caracteres.
-   Evita las colisiones de teclas de acceso.
-   Evita los caracteres que son difíciles de diferenciar de otros caracteres, como la letra "I" y el número "1" o la letra "O" y el número "0".
-   Usa precedentes conocidos de otras aplicaciones populares, como Word ("A" para "Archivo", "O" para "Inicio", etc.).
-   Usa el primer carácter del nombre del comando o un carácter con una asociación cercana al comando que ayude a recordar.
    -   Si la primera letra ya está asignada, usa una letra que esté lo más cerca posible de la primera letra del nombre del comando ("N" para "Insertar").
    -   Usa una consonante que se distinga del nombre del comando ("S" para "Insertar nota al pie").
    -   Usa una vocal del nombre del comando.

## <a name="localize-access-keys"></a>Localizar las teclas de acceso

Si tu aplicación va a localizarse en varios idiomas, también debes **tener en cuenta la localización de las teclas de acceso**. Por ejemplo, "H" para "Home" en en-US e "I" para "Incio" en es-ES.

Usa la extensión de x:Uid en el marcado para aplicar recursos localizados como se muestra aquí:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Los recursos para cada idioma se agregan a las carpetas String correspondientes en el proyecto:

![Carpetas String de recursos para inglés y español](images/accesskeys/resource-string-folders.png)

_Carpetas String de recursos para inglés y español_

Las teclas de acceso localizadas se especifican en el archivo resources.resw del proyecto:

![Especificar la propiedad AccessKey especificada en el archivo resources.resw](images/accesskeys/resource-resw-file.png)

_Especificar la propiedad AccessKey especificada en el archivo resources.resw_

Para obtener más información, consulta [Traducir recursos de la interfaz de usuario](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx).

## <a name="key-tip-positioning"></a>Ubicación de las sugerencias de teclas

Las sugerencias de teclas se muestran como indicaciones flotantes relativas a los elementos correspondientes de la interfaz de usuario, teniendo en cuenta la presencia de otros elementos de la interfaz de usuario, otras sugerencias de teclas y el borde de la pantalla.

Por lo general, alcanza con la ubicación predeterminada de las sugerencias de teclas, que proporciona compatibilidad integrada para la interfaz de usuario adaptable.

![Ejemplo de ubicación automática de sugerencias de teclas](images/accesskeys/auto-keytip-position.png)

_Ejemplo de ubicación automática de sugerencias de teclas_

Sin embargo, en caso de necesitar más control sobre la ubicación de las sugerencias de teclas, se recomienda lo siguiente:

1.  **Principio de asociación obvia**: El usuario puede asociar el control a la sugerencia de teclas fácilmente.

    a.  El KeyTip debe estar **cerca** del elemento que tiene la tecla de acceso (el propietario).  
    b.  El KeyTip debe **evitar cubrir los elementos habilitados** que tienen teclas de acceso.   
    c.  Si no se puede ubicar un KeyTip cerca de su propietario, debe superponerse con este. 

2.  **Detectabilidad**: El usuario puede detectar rápidamente el control con la sugerencia de teclas.

    a.  El KeyTip nunca se **superpone** con otras sugerencias de teclas.  

3.  **Vistazo sencillo:** El usuario puede echar un vistazo fácilmente a las sugerencias de teclas.

    a.  Los KeyTips deben estar **alineados** entre sí y con el elemento de la interfaz de usuario.
    b.  Los KeyTips deben estar **agrupados** tanto como sea posible. 

### <a name="relative-position"></a>Posición relativa

Usa la propiedad **KeyTipPlacementMode** para personalizar la ubicación de la sugerencia de teclas por elemento o por grupo.

Los modos de ubicación son: Top, Bottom, Right, Left, Hidden, Center y Auto.

![Modos de ubicación de sugerencia de teclas](images/accesskeys/keytip-postion-modes.png)

_Modos de ubicación de sugerencia de teclas_

La línea central del control se usa para calcular la alineación vertical y horizontal del KeyTip.

En el siguiente ejemplo se muestra cómo establecer la ubicación de la sugerencia de teclas de un grupo de controles mediante la propiedad KeyTipPlacementMode de un contenedor StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Desplazamientos

Usa las propiedades KeyTipHorizontalOffset y KeyTipVerticalOffset de un elemento para tener un control incluso más detallado de la ubicación de la sugerencia de teclas.

> [!NOTE]
> Los desplazamientos no pueden establecerse cuando la propiedad KeyTipPlacementMode se establece en Auto.

La propiedad KeyTipHorizontalOffset indica cuánto mover la sugerencia de teclas a la izquierda o a la derecha. En el ejemplo se muestra cómo establecer los desplazamientos de la sugerencia de teclas para un botón.

![Modos de ubicación de sugerencia de teclas](images/accesskeys/keytip-offsets.png)

_Establecer el desplazamiento vertical y horizontal de una sugerencia de teclas_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Alineación con el borde de la pantalla {#screen-edge-alignment .ListParagraph}

La ubicación de una sugerencia de teclas se ajusta automáticamente según el borde de la pantalla para garantizar que la sugerencia de teclas se vea por completo. Cuando esto ocurre, la distancia entre el control y el punto de alineación de la sugerencia de teclas puede diferir de los valores especificados para el desplazamiento horizontal y vertical.

![Modos de ubicación de sugerencia de teclas](images/accesskeys/keytips-screen-edge.png)

_El borde de la pantalla hace que la sugerencia de teclas se reubique automáticamente_

## <a name="key-tip-style"></a>Estilos de las sugerencias de teclas

Recomendamos que uses la compatibilidad integrada de sugerencias de teclas para los temas de la plataforma, incluido el contraste alto.

Si necesitas especificar tus propios estilos de sugerencias de teclas, usa los recursos de aplicación, como KeyTipFontSize (tamaño de fuente), KeyTipFontFamily (familia de fuentes), KeyTipBackground (fondo), KeyTipForeground (primer plano), KeyTipPadding (relleno), KeyTipBorderBrush (color de borde) y KeyTipBorderThemeThickness (grosor del borde).

![Modos de ubicación de sugerencia de teclas](images/accesskeys/keytip-customization.png)

_Opciones de personalización de las sugerencias de teclas_

En este ejemplo se muestra cómo cambiar estos recursos de aplicación:

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>Las teclas de acceso y el Narrador

El marco XAML expone las propiedades de automatización que permiten a los clientes de automatización de la interfaz de usuario detectar información sobre los elementos en la interfaz de usuario.

Si especificas la propiedad AccessKey en un control UIElement o TextElement, puedes usar la propiedad [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) para obtener este valor. Los clientes de accesibilidad, como el Narrador, leen el valor de esta propiedad cada vez que un elemento tiene el foco.

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de teclado](keyboard-interactions.md)
* [Aceleradores de teclado](keyboard-accelerators.md)

**Ejemplos**
* [Galería de controles de XAML (también conocido como XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)



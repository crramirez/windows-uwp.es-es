---
Description: Obtenga información acerca de cómo mejorar la facilidad de uso y la accesibilidad de su aplicación de Windows al proporcionar una manera intuitiva de que los usuarios naveguen e interactúen rápidamente con la interfaz de usuario visible de una aplicación a través de un teclado en lugar de un dispositivo de puntero (como toque o mouse).
title: Directrices de diseño de claves de acceso
label: Access keys design guidelines
keywords: teclado, tecla de acceso, KeyTip, sugerencia de teclas, accesibilidad, navegación, foco, texto, entrada, interacción del usuario
template: detail.hbs
ms.date: 06/08/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c0d5808c462beb72341fd83c6fc4c1cfc0178b2f
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970980"
---
# <a name="access-keys"></a>Claves de acceso

Las claves de acceso son métodos abreviados de teclado que mejoran la facilidad de uso y la accesibilidad de las aplicaciones Windows, ya que proporcionan una manera intuitiva de que los usuarios naveguen e interactúen rápidamente con la interfaz de usuario visible de una aplicación a través de un teclado en lugar de un dispositivo de puntero (como toque o mouse).

Consulte el tema [teclas de aceleración](keyboard-accelerators.md) para obtener más información sobre cómo invocar acciones comunes en una aplicación Windows con métodos abreviados de teclado. 

> [!NOTE]
> Un teclado es indispensable para los usuarios con ciertas discapacidades (vea [accesibilidad del teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)) y también es una herramienta importante para los usuarios que lo prefieren como una forma más eficaz de interactuar con una aplicación.

La aplicación de Windows proporciona compatibilidad integrada en los controles de plataforma para las teclas de acceso basadas en teclado y los comentarios de la interfaz de usuario asociados a través de las indicaciones visuales denominadas sugerencias de teclas.

## <a name="overview"></a>Información general

Una tecla de acceso es una combinación de la tecla Alt y una o varias claves alfanuméricas (a veces denominadas *tecla de acceso) que*normalmente se presionan secuencialmente, en lugar de simultáneamente.

Las sugerencias de teclas son los distintivos que se muestran junto a los controles que admiten teclas de acceso cuando el usuario presiona la tecla Alt. Cada sugerencia de clave contiene las claves alfanuméricas que activan el control asociado.

> [!NOTE]
> Los métodos abreviados de teclado se admiten automáticamente para las teclas de acceso con un solo carácter alfanumérico. Por ejemplo, al presionar simultáneamente Alt + F en Word, se abre el menú Archivo sin mostrar las sugerencias de teclas.

Al presionar la tecla Alt se inicializa la funcionalidad de clave de acceso y se muestran todas las combinaciones de teclas disponibles actualmente en la información sobre herramientas. Las pulsaciones de teclas subsiguientes se controlan mediante el marco de claves de acceso, que rechaza las claves no válidas hasta que se presiona una tecla de acceso válida o se presionan las teclas entrar, ESC, TAB o de flecha para desactivar las teclas de acceso y devolver el control de pulsaciones de teclas a la aplicación.

Microsoft Office aplicaciones proporcionan una amplia compatibilidad con las claves de acceso. En la imagen siguiente se muestra la pestaña Inicio de Word con claves de acceso activadas (tenga en cuenta la compatibilidad con los números y con varias pulsaciones de tecla).

![Distintivos de las sugerencias de teclas para las claves de acceso en Microsoft Word](images/accesskeys/keytip-badges-word.png)

_Distintivos de KeyTip para las claves de acceso en Microsoft Word_

Para agregar una tecla de acceso a un control, use la **propiedad Accesskey**. El valor de esta propiedad especifica la secuencia de teclas de acceso, el acceso directo (si es un carácter alfanumérico único) y la información sobre herramientas.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Cuándo usar las teclas de acceso

Se recomienda que especifique las claves de acceso siempre que sea adecuado en la interfaz de usuario y que admita claves de acceso en todos los controles personalizados.

1.  **Las teclas de acceso hacen que la aplicación sea más accesible** para los usuarios con discapacidades del motor, incluidos aquellos usuarios que pueden presionar solo una tecla cada vez o tener dificultades para usar un mouse.

    Una interfaz de usuario de teclado bien diseñada es un aspecto importante de la accesibilidad del software. Permite a usuarios con dificultades visuales o con ciertas discapacidades motrices navegar por una aplicación e interactuar con sus funciones. Es posible que estos usuarios no puedan controlar un mouse y empleen varias tecnologías de ayuda, como herramientas para la mejora del teclado, teclados en pantalla, ampliadores de pantallas, lectores de pantalla y utilidades de entrada de voz. Para estos usuarios, la cobertura de comandos completa es fundamental.

2.  **Las teclas de acceso hacen que la aplicación sea más fácil de usar** para los usuarios avanzados que prefieren interactuar a través del teclado.

    Los usuarios con experiencia suelen tener una gran preferencia para usar el teclado, ya que los comandos basados en teclado se pueden escribir más rápidamente y no requieren que quiten las manos del teclado. Para estos usuarios, la eficacia y la coherencia son cruciales; la exhaustividad es importante solo para los comandos usados con más frecuencia.

## <a name="set-access-key-scope"></a>Establecer el ámbito de la clave de acceso

Cuando hay muchos elementos en la pantalla que admiten las claves de acceso, se recomienda el ámbito de las claves de acceso para reducir la **carga cognitiva**. Esto minimiza el número de teclas de acceso en la pantalla, lo que facilita su búsqueda y mejora la eficacia y la productividad.

Por ejemplo, Microsoft Word proporciona dos ámbitos clave de acceso: un ámbito principal para las pestañas de la cinta de opciones y un ámbito secundario para los comandos de la pestaña seleccionada.

En las siguientes imágenes se muestran los dos ámbitos clave de acceso de Word. El primero muestra las claves de acceso principales que permiten al usuario seleccionar una pestaña y otros comandos de nivel superior, y el segundo muestra las claves de acceso secundarias para la pestaña Inicio.

![Claves de acceso principales en claves](images/accesskeys/primary-access-keys-word.png)
de acceso principal de Microsoft Word_en Microsoft Word_

![Claves de acceso secundarias en](images/accesskeys/secondary-access-keys-word.png)
_las claves de acceso secundarias de Microsoft Word en Microsoft Word_

Se pueden duplicar las claves de acceso para los elementos de distintos ámbitos. En el ejemplo anterior, "2" es la clave de acceso para deshacer en el ámbito principal y también "cursiva" en el ámbito secundario.

Aquí se muestra cómo definir un ámbito de clave de acceso.

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

![Claves de acceso principales para CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Ámbito principal de CommandBar y claves de acceso admitidas_

![Claves de acceso secundarias para CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Ámbito secundario de CommandBar y claves de acceso admitidas_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Creators Update y versiones anteriores

Antes de Windows 10 Fall Creators Update, algunos controles, como CommandBar, no admitían los ámbitos de clave de acceso integrados.

En el ejemplo siguiente se muestra cómo se admite la compatibilidad de SecondaryCommands con las claves de acceso, que están disponibles una vez que se invoca un comando primario (similar a la cinta de opciones de Word).

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

## <a name="avoid-access-key-collisions"></a>Evitar colisiones de claves de acceso

Los conflictos de clave de acceso se producen cuando dos o más elementos del mismo ámbito tienen claves de acceso duplicadas o empiezan con los mismos caracteres alfanuméricos.

El sistema resuelve las claves de acceso duplicadas mediante el procesamiento de la clave de acceso del primer elemento que se agrega al árbol visual, pasando por alto todos los demás.

Cuando varias claves de acceso comienzan con el mismo carácter (por ejemplo, "A", "a1" y "AB"), el sistema procesa la clave de acceso de un solo carácter y omite todas las demás.

Evite colisiones mediante el uso de claves de acceso únicas o mediante el ámbito de comandos.

## <a name="choose-access-keys"></a>Elegir claves de acceso

Tenga en cuenta lo siguiente al elegir las claves de acceso:

-   Use un solo carácter para minimizar las pulsaciones de tecla y las teclas de aceleración de soporte de forma predeterminada (Alt + AccessKey)
-   Evite usar más de dos caracteres
-   Evitar colisiones de claves de acceso
-   Evite caracteres difíciles de diferenciar de otros caracteres, como la letra "I" y el número "1", la letra "o" y el número "0"
-   Usar precedentes conocidos de otras aplicaciones populares como Word ("F" para "File", "H" para "Home", etc.)
-   Use el primer carácter del nombre del comando o un carácter con una asociación de cierre al comando que ayuda con la recuperación.
    -   Si la primera letra ya está asignada, use una letra lo más cercana posible a la primera letra del nombre del comando ("N" para insertar)
    -   Usar una consonante distintiva del nombre del comando ("W" para la vista)
    -   Use una vocal del nombre del comando.

## <a name="localize-access-keys"></a>Localizar claves de acceso

Si la aplicación va a estar localizada en varios idiomas, también debe **considerar la localización de las claves de acceso**. Por ejemplo, para "H" para "Home" en en-US y "I" para "incio" en es-ES.

Use la extensión x:Uid en el marcado para aplicar recursos localizados, como se muestra aquí:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Los recursos de cada idioma se agregan a las carpetas de cadena correspondientes del proyecto:

![Carpetas de cadenas de recursos en inglés y español](images/accesskeys/resource-string-folders.png)

_Carpetas de cadenas de recursos en inglés y español_

Las claves de acceso localizadas se especifican en el archivo resources. resw del proyecto:

![Especifique la propiedad AccessKey especificada en el archivo resources. resw.](images/accesskeys/resource-resw-file.png)

_Especifique la propiedad AccessKey especificada en el archivo resources. resw._

Para obtener más información, consulte [traducir recursos](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10)) de la interfaz de usuario

## <a name="key-tip-positioning"></a>Posición de la sugerencia de teclas

Las sugerencias de teclas se muestran como distintivos flotantes respecto a su correspondiente elemento de la interfaz de usuario, teniendo en cuenta la presencia de otros elementos de la interfaz de usuario, otras sugerencias de teclas y el borde de la pantalla.

Normalmente, la ubicación de la sugerencia de teclas predeterminada es suficiente y proporciona compatibilidad integrada para la interfaz de usuario adaptable.

![Ejemplo de ubicación automática de la sugerencia de teclas](images/accesskeys/auto-keytip-position.png)

_Ejemplo de ubicación automática de la sugerencia de teclas_

Sin embargo, si necesita más control sobre el posicionamiento de la información sobre las teclas, se recomienda lo siguiente:

1.  **Principio de asociación obvio**: el usuario puede asociar fácilmente el control con la información sobre herramientas.

    a.  La información sobre las teclas debe estar **cerca** del elemento que tiene la clave de acceso (el propietario).  
    b.  La sugerencia clave debe **evitar abarcar los elementos habilitados** que tienen claves de acceso.   
    c.  Si una sugerencia de clave no se puede colocar cerca de su propietario, debe superponerse al propietario. 

2.  **Detectabilidad**: el usuario puede detectar rápidamente el control con la información sobre herramientas.

    a.  La sugerencia de teclas nunca se **superpone** a otras sugerencias de teclas.  

3.  **Análisis sencillo:** El usuario puede hojear fácilmente las sugerencias clave.

    a.  Las sugerencias de teclas se deben **alinear** entre sí y con el elemento de la interfaz de usuario.
    b.  Las sugerencias de teclas se deben **agrupar** tanto como sea posible. 

### <a name="relative-position"></a>Posición relativa

Use la propiedad **KeyTipPlacementMode** para personalizar la ubicación de la sugerencia de clave en cada elemento o por grupo.

Los modos de colocación son: superior, inferior, derecha, izquierda, oculto, centro y automático.

![Modos de colocación de las sugerencias de teclas](images/accesskeys/keytip-postion-modes.png)

_Modos de colocación de las sugerencias de teclas_

La línea central del control se usa para calcular la alineación vertical y horizontal de la información sobre herramientas.

En el ejemplo siguiente se muestra cómo establecer la ubicación de la información sobre herramientas de un grupo de controles mediante la propiedad KeyTipPlacementMode de un contenedor StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Desplazamientos

Use las propiedades KeyTipHorizontalOffset y KeyTipVerticalOffset de un elemento para un control aún más granular de la ubicación de la información sobre herramientas.

> [!NOTE]
> No se pueden establecer desplazamientos cuando KeyTipPlacementMode está establecido en auto.

La propiedad KeyTipHorizontalOffset indica hasta dónde se mueve la punta de teclas a la izquierda o a la derecha. en el ejemplo se muestra cómo establecer los desplazamientos de las sugerencias de teclas de un botón.

![Modos de colocación de las sugerencias de teclas](images/accesskeys/keytip-offsets.png)

_Establecer desplazamientos verticales y horizontales para una información sobre herramientas_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Alineación del borde de la pantalla {#screen: alineación del borde. ListParagraph}

La ubicación de una sugerencia de clave se ajusta automáticamente en función del margen de la pantalla para garantizar que la información sobre herramientas esté totalmente visible. Cuando esto ocurre, la distancia entre el control y el punto de alineación de la sugerencia de clave podría diferir de los valores especificados para los desplazamientos horizontal y vertical.

![Modos de colocación de las sugerencias de teclas](images/accesskeys/keytips-screen-edge.png)

_El margen de la pantalla hace que la sugerencia de teclas se cambie automáticamente de posición_

## <a name="key-tip-style"></a>Estilo de la sugerencia de clave

Se recomienda usar la compatibilidad integrada de la sugerencia de clave para los temas de la plataforma, incluido el contraste alto.

Si tiene que especificar sus propios estilos de información sobre herramientas, use recursos de la aplicación como KeyTipFontSize (tamaño de fuente), KeyTipFontFamily (familia de fuentes), KeyTipBackground (fondo), KeyTipForeground (primer plano), KeyTipPadding (relleno), KeyTipBorderBrush (color de borde) y KeyTipBorderThemeThickness (grosor del borde).

![Modos de colocación de las sugerencias de teclas](images/accesskeys/keytip-customization.png)

_Opciones de personalización de la sugerencia de clave_

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

## <a name="access-keys-and-narrator"></a>Teclas de acceso y Narrador

El marco XAML expone propiedades de automatización que permiten a los clientes de UI Automation detectar información sobre los elementos de la interfaz de usuario.

Si especifica la propiedad AccessKey en un control UIElement o TextElement, puede usar la propiedad [AutomationProperties. AccessKey](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.accesskey) para obtener este valor. Los clientes de accesibilidad, como narrador, leen el valor de esta propiedad cada vez que un elemento recibe el foco.

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de teclado](keyboard-interactions.md)
* [Aceleradores de teclado](keyboard-accelerators.md)

**Muestras**
* [Galería de controles XAML (también conocido como XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)



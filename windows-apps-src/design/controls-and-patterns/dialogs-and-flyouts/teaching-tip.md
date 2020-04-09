---
Description: Una sugerencia de enseñanza es un control flotante parcialmente persistente y con mucho contenido que proporciona información contextual.
title: Sugerencias de enseñanza
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: 6276ef9bcb6b01fd557057d3d36939350314015b
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081051"
---
# <a name="teaching-tip"></a>Sugerencia de enseñanza

Una sugerencia de enseñanza es un control flotante parcialmente persistente y con mucho contenido que proporciona información contextual. A menudo se utiliza para informar, recordar y enseñar a los usuarios sobre características importantes y nuevas que pueden mejorar su experiencia.

Una sugerencia de enseñanza puede ser descartable por cambio de foco, o bien puede requerir una acción explícita para cerrarse. Además, puede apuntar a un elemento específico de la interfaz de usuario con su delta y también se puede utilizar sin una delta o destino.

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](../images/winui-logo-64x64.png) | El control **TeachingTip** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library ](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de la biblioteca de interfaz de usuario de Windows:** [Clase TeachingTip](/uwp/api/microsoft.ui.xaml.controls.teachingtip)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control **TeachingTip** para centrar la atención de un usuario en actualizaciones y características nuevas o importantes, para recordarle de opciones no esenciales que podrían mejorar su experiencia, o para enseñarle cómo se debe completar una tarea.

Ya que las sugerencias de enseñanza son transitorias, no es un control recomendado para informar a los usuarios sobre los errores o cambios de estado importantes.


## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TeachingTip">abrir la aplicación y ver TeachingTip en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Una sugerencia de enseñanza puede tener varias configuraciones, incluidas las siguientes menciones destacadas.

Una sugerencia de enseñanza puede dirigirse a un elemento específico de la interfaz de usuario con su delta para dejar más claro el contexto de la información que está presentando.

![Una aplicación de ejemplo con una sugerencia de enseñanza que tiene como destino el botón Guardar. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-targeted.png)

Cuando la información presentada no pertenece a un elemento determinado de la interfaz de usuario, se puede crear una sugerencia de enseñanza no dirigida al eliminar la delta.

![Una aplicación de ejemplo con una sugerencia de enseñanza en la esquina inferior derecha. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-non-targeted.png)

Una sugerencia de enseñanza puede exigir al usuario que use un botón "X" en la esquina superior para descartarla o un botón "Cerrar" en la parte inferior. También es posible que la sugerencia de enseñanza se descarte por cambio de foco si no se agrega un botón Descartar. En su lugar, la sugerencia de enseñanza se cerrará cuando un usuario se desplace o interactúe con otros elementos de la aplicación. Debido a este comportamiento, las sugerencias descartables por cambio de foco son la mejor solución cuando una sugerencia debe colocarse en un área desplazable.

![Una aplicación de ejemplo con una sugerencia de enseñanza descartable por cambio de foco en la esquina inferior derecha. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú).](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Crear una sugerencia de enseñanza

Este es el código XAML para un control de sugerencia de enseñanza dirigida que muestra el aspecto predeterminado de TeachingTip con un título y subtítulo.
Ten en cuenta que la sugerencia de enseñanza puede aparecer en cualquier lugar del árbol de elementos o el código subyacente. En el ejemplo siguiente, se encuentra en un objeto ResourceDictionary.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

Este es el resultado cuando se muestran la página que contiene el botón y la sugerencia de enseñanza:

![Una aplicación de ejemplo con una sugerencia de enseñanza que tiene como destino el botón Guardar. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Sugerencias no dirigidas

No todas las sugerencias están relacionadas con un elemento en pantalla. Para estos casos, no establezcas la propiedad de destino y, en su lugar, la sugerencia de enseñanza se mostrará en relación con los bordes de la raíz de XAML. Sin embargo, una sugerencia de enseñanza puede no tener delta y conservar la colocación relativa a un elemento de la interfaz de usuario si estableces la propiedad TailVisibility en "Collapsed". El ejemplo siguiente es de una sugerencia de enseñanza no dirigida.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</muxc:TeachingTip>
```

Ten en cuenta que en este ejemplo el elemento TeachingTip está en el árbol de elementos, en lugar de en un objeto ResourceDictionary o en el código subyacente. Esto no tiene ningún efecto sobre el comportamiento; TeachingTip solo se muestra cuando se abre y no ocupa espacio en el diseño.

![Una aplicación de ejemplo con una sugerencia de enseñanza en la esquina inferior derecha. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Ubicación preferida

La sugerencia de enseñanza reproduce el comportamiento de ubicación del [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) del control flotante con la propiedad TeachingTipPlacementMode. El modo de selección de ubicación predeterminado intentará colocar una sugerencia de enseñanza dirigida sobre su destino, y colocará las sugerencias de enseñanza no dirigidas centradas en la parte inferior de la raíz de XAML. Al igual que con los controles flotantes, si el modo de selección de ubicación preferido no deja espacio libre para que se muestre la sugerencia de enseñanza, se elegirá otro modo de selección de ubicación automáticamente.

Para las aplicaciones que predicen la entrada del controlador para juegos, consulta [Interacciones con controlador para juegos y control remoto]( https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Se recomienda probar la accesibilidad del controlador para juegos de todas las sugerencias de enseñanza con todas las configuraciones posibles de la interfaz de usuario de una aplicación.

Una sugerencia de enseñanza dirigida con la propiedad PreferredPlacement establecida en "BottomLeft" se mostrará con la delta centrada en la parte inferior de su destino y el cuerpo de la sugerencia estará desplazado hacia la izquierda.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con un botón "Save" (Guardar) que funciona como el destino de una sugerencia de enseñanza ubicada debajo de la esquina izquierda. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-targeted-preferred-placement.png)


Una sugerencia de enseñanza no dirigida que tenga la propiedad PreferredPlacement establecida en "BottomLeft" se mostrará en la esquina inferior izquierda de la raíz de XAML.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</muxc:TeachingTip>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza en la esquina inferior izquierda. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-non-targeted-preferred-placement.png)

El siguiente diagrama muestra el resultado de los 13 modos de PreferredPlacement que se pueden establecer para las sugerencias de enseñanza dirigidas.
![Ilustración que contiene 13 sugerencias de enseñanza, cada una es una demostración de un modo de selección de ubicación dirigida distinto. Cada sugerencia de enseñanza tiene una etiqueta con el modo que representa. La primera palabra de un modo de selección de ubicación indica el lado del destino en el que la sugerencia de enseñanza se mostrará centrada. La delta de la sugerencia de enseñanza siempre se colocará centrada en ese lado del destino, y apuntará hacia el destino. Si hay una segunda palabra en el modo de selección de ubicación, el cuerpo de la sugerencia de enseñanza no estará centrado, sino que se desplazará en la dirección especificada. Por ejemplo, el modo de selección de ubicación "TopRight" hará que la sugerencia de enseñanza se muestre arriba del destino y esté desplazada a la derecha, con la delta apuntando hacia abajo al centro del borde superior del destino. Dado que el cuerpo está desplazado hacia la derecha, la delta se encuentra en el borde izquierdo del cuerpo de la sugerencia de enseñanza, y la sugerencia de enseñanza se extiende más allá del borde derecho del destino. El modo de selección de ubicación "Center" es único y hace que la punta de la delta de la sugerencia de enseñanza se coloque en el centro del destino. Además, la delta de la sugerencia de enseñanza se centra sobre la mitad superior del destino.](../images/teaching-tip-targeted-preferred-placement-modes.png)

El siguiente diagrama muestra el resultado de los 13 modos de PreferredPlacement que se pueden establecer para las sugerencias de enseñanza no dirigidas.
![Ilustración que contiene nueve sugerencias de enseñanza, cada una es una demostración de un modo de selección de ubicación no dirigida distinto. Cada sugerencia de enseñanza tiene una etiqueta con el modo que representa. La primera palabra de un modo de selección de ubicación indica el lado de la raíz de XAML en la que la sugerencia de enseñanza se mostrará centrada. Si hay una segunda palabra en el modo de selección de ubicación, la sugerencia de enseñanza se colocará hacia ese esquina especificada de la raíz de XAML. Por ejemplo, el modo de selección de ubicación "TopRight" hará que la sugerencia de enseñanza se muestre en la esquina superior derecha de la raíz de XAML. Para los modos de selección de ubicación no dirigidos, el orden de las dos palabras no afecta a la selección de ubicación. TopRight es equivalente a RightTop. El modo de selección de ubicación "Center" es único y hace que la sugerencia de enseñanza se muestre en el centro vertical y horizontal de la raíz de XAML.](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Agregar un margen de ubicación

Puedes controlar la distancia a la que se coloca una sugerencia de enseñanza con relación a su destino, al igual que la distancia a la que se coloca una sugerencia de enseñanza no dirigida con relación a los bordes de la raíz de XAML mediante la propiedad PlacementMargin. Al igual que [Margin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin tiene cuatro valores: left, right, top y bottom, por lo que solo se usan los valores pertinentes. Por ejemplo, PlacementMargin.Left se aplica cuando la sugerencia está a la izquierda del destino o en el borde izquierdo de la raíz de XAML.

En el ejemplo siguiente se muestra una sugerencia no dirigida cuyos valores Left/Top/Right/Bottom de PlacementMargin están establecidos en 80.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</muxc:TeachingTip>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza colocada hacia la esquina inferior derecha, aunque no del todo. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Agregar contenido

Puede agregarse contenido a una sugerencia de enseñanza mediante la propiedad Content. Si el tamaño de la sugerencia de enseñanza no permite mostrar todo el contenido, se habilitará automáticamente una barra de desplazamiento para permitir que el usuario se desplace por el área de contenido.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza que tiene como destino el botón Guardar. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). En el área de contenido de la sugerencia de enseñanza se muestra una casilla de verificación con la etiqueta "Don't show tips at startup" (no mostrar sugerencias en el inicio) y debajo se muestra el texto "You can change your tip preferences in Settings if you change your mind" (puedes cambiar tus preferencias de sugerencia en la configuración si cambias de opinión). Allí, "Settings" es un vínculo a la página de configuración de la aplicación. Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Agregar botones

De manera predeterminada, se muestra un botón Cerrar "X" estándar junto al título de una sugerencia de enseñanza. El botón Cerrar puede personalizarse con la propiedad CloseButtonContent, en cuyo caso el botón se mueve a la parte inferior de la sugerencia de enseñanza.

**Nota: Las sugerencias que pueden descartarse por cambio de foco no muestran un botón Cerrar.**

Puedes agregar un botón de acción personalizado si estableces la propiedad ActionButtonContent (y opcionalmente las propiedades ActionButtonCommand y ActionButtonCommandParameter).

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza que tiene como destino el botón Guardar. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). En el área de contenido de la sugerencia de enseñanza se muestra una casilla de verificación con la etiqueta "Don't show tips at startup" (no mostrar sugerencias en el inicio) y debajo se muestra el texto "You can change your tip preferences in Settings if you change your mind" (puedes cambiar tus preferencias de sugerencia en la configuración si cambias de opinión). Allí, "Settings" es un vínculo a la página de configuración de la aplicación. En la parte inferior de la enseñanza hay dos botones, uno gris a la izquierda que dice "Disable" (Deshabilitar) y uno azul a la derecha que dice "Got it!" (Entendido).](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Contenido de elemento principal

Se puede agregar contenido de borde a borde a una sugerencia de enseñanza si se establece la propiedad HeroContent. La ubicación del contenido de elemento principal puede establecerse en la parte superior o inferior de una sugerencia de enseñanza si configuras la propiedad HeroContentPlacement.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <muxc:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </muxc:TeachingTip.HeroContent>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza que tiene como destino el botón Guardar. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). En la parte inferior de la sugerencia de enseñanza hay una imagen de borde a borde de un hombre de dibujos animados colocando archivos en la nube. Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Agregar un icono

Puede agregarse un icono al lado del título y subtítulo mediante la propiedad IconSource. Entre los tamaños de icono recomendados se incluyen 16 píxeles, 24 píxeles y 32 píxeles.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <muxc:TeachingTip.IconSource>
                <muxc:SymbolIconSource Symbol="Save" />
            </muxc:TeachingTip.IconSource>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza que tiene como destino el botón Guardar. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). A la izquierda del título y subtítulo hay un icono de disquete. Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Habilitar el descarte por cambio de foco

La funcionalidad de descarte por cambio de foco está deshabilitada de manera predeterminada, pero se puede habilitar para que, por ejemplo, una sugerencia de enseñanza se cierre cuando un usuario se desplace o interactúe con otros elementos de la aplicación. Debido a este comportamiento, las sugerencias descartables por cambio de foco son la mejor solución cuando una sugerencia debe colocarse en un área desplazable.

El botón Cerrar se quitará automáticamente de una sugerencia de enseñanza que tenga habilitado el descarte por cambio de foco para que los usuarios puedan identificar que su comportamiento es de descarte por cambio de foco.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</muxc:TeachingTip>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza descartable por cambio de foco en la esquina inferior derecha. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú).](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Escaparse de los límites de la raíz de XAML

En Windows versión 19H1 y anteriores, una sugerencia de enseñanza puede escaparse de los límites de la raíz de XAML y la pantalla si se establece la propiedad ShouldConstrainToRootBounds. Cuando esta propiedad está habilitada, una sugerencia de enseñanza no intentará mantenerse en los límites de la raíz de XAML ni de la pantalla y usará siempre la posición especificada en el modo PreferredPlacement. Se recomienda habilitar la propiedad IsLightDismissEnabled y establecer el modo de PreferredPlacement más cercano al centro de la raíz de XAML para garantizar la mejor experiencia para los usuarios.

En versiones anteriores de Windows, esta propiedad se omite y la sugerencia de enseñanza siempre permanece dentro de los límites de la raíz de XAML.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</muxc:TeachingTip>
```

![Una aplicación de ejemplo con una sugerencia de enseñanza fuera de la esquina inferior derecha de la aplicación. El título de la sugerencia dice "Saving automatically" (Guardado automático) y el subtítulo dice "We save your changes as you go - so you never have to" (Tus cambios se guardan sobre la marcha para que no tengas que hacerlo tú). Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>Cancelar y aplazar el cierre

El evento de cierre puede usarse para cancelar o aplazar el cierre de una sugerencia de enseñanza. Puede utilizarse para mantener abierta la sugerencia de enseñanza o para tener tiempo suficiente para que se produzca una acción o animación personalizada. Cuando se cancela el cierre de una sugerencia de enseñanza, IsOpen vuelve a ser true. Sin embargo, seguirá siendo false durante un aplazamiento. También se puede cancelar un cierre mediante programación.

**Nota: Si ninguna de las opciones de selección de ubicación permite que la sugerencia de enseñanza se muestre por completo, esta iterará su ciclo de vida de eventos para forzar un cierre en lugar de mostrarse sin un botón Cerrar accesible. Si la aplicación cancela el evento de cierre, la sugerencia de enseñanza puede permanecer abierta sin un botón Cerrar accesible.**

XAML
```XAML
<muxc:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</muxc:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>Observaciones

### <a name="related-articles"></a>Artículos relacionados

* [Cuadros de diálogo y controles flotantes](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>Recomendaciones
* Las sugerencias son temporales y no deben contener información u opciones que sean críticas para la experiencia de una aplicación.
* Evita mostrar sugerencias de enseñanza con demasiada frecuencia. Es más probable que las sugerencias de enseñanza reciban atención individual cuando están escalonadas a lo largo de sesiones largas o en varias sesiones.
* Mantén las sugerencias concisas y el tema claro. Las investigaciones muestran que, en promedio, los usuarios solo leen entre 3 y 5 palabras y comprenden entre 2 y 3 antes de decidir si van a interactuar con una sugerencia.
* No se garantiza la accesibilidad del controlador para juegos de una sugerencia de enseñanza. Para las aplicaciones que esperan recibir la entrada de un controlador para juegos, consulta [Interacciones con controlador para juegos y control remoto]( https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Se recomienda probar la accesibilidad del controlador para juegos de todas las sugerencias de enseñanza con todas las configuraciones posibles de la interfaz de usuario de una aplicación.
* Cuando la sugerencia de enseñanza está habilitada para escapar la raíz de XAML, se recomienda habilitar la propiedad IsLightDismissEnabled y establecer el modo de PreferredPlacement más cercano al centro de la raíz de XAML.

### <a name="reconfiguring-an-open-teaching-tip"></a>Volver a configurar una sugerencia de enseñanza abierta

Es posible volver a configurar algunos contenidos y propiedades mientras la sugerencia de enseñanza está abierta, y estos cambios surtirán efecto inmediatamente. Otras propiedades y contenido (como la propiedad de icono, los botones de acción y de cierre y volver a configurar los descartes por cambio de foco y los descartes explícitos) requieren que la sugerencia de enseñanza se cierre y se vuelva a abrir para que los cambios de estas propiedades surtan efecto. Ten en cuenta que cambiar el comportamiento de descarte de manual a descarte por cambio de foco mientras una sugerencia de enseñanza está abierta provocará que su botón Cerrar se elimine antes de que el comportamiento de cierre por cambio de foco se habilite, por lo que la sugerencia se quedará bloqueada en la pantalla.

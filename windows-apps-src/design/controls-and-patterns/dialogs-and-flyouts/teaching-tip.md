---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249507"
---
# <a name="teaching-tip"></a>Sugerencia de enseñanza

Una sugerencia de enseñanza es un control flotante parcial persistente y contenido enriquecido que proporciona información contextual. A menudo se utiliza para que le informa, recordando y enseñar a los usuarios sobre importantes y nuevas características que pueden mejorar su experiencia.

**API importantes:** [Clase TeachingTip](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

Puede ser una sugerencia de enseñanza de descarte ligero o requieran una acción explícita para cerrar. Una sugerencia de enseñanza puede tener como destino un elemento específico de la interfaz de usuario con su cola y también se puede utilizar sin una cola o destino.

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado? 

Use un **TeachingTip** control para centrar la atención de un usuario en nueva o importantes actualizaciones y características, recordar un usuario de opciones que no sean esenciales que podría mejorar su experiencia, o enseñar cómo se debe completar una tarea a un usuario. 

Porque la sugerencia de enseñanza es transitorio, no sería el control recomendado para pedir a los usuarios sobre los errores o cambios de estado importante.


## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/TeachingTip">abra la aplicación y vea el TeachingTip en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Una sugerencia de enseñanza puede tener varias configuraciones, incluso estos que notables.

Una sugerencia de enseñanza puede tener como destino un elemento específico de la interfaz de usuario con su cola para mejorar la claridad contextual de la información que van a presentar. 

![Una aplicación de ejemplo con una punta de enseñanza destinadas a la operación de guardar botón. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-targeted.png)

Cuando la información presentada no pertenecen a un elemento determinado de la interfaz de usuario, una sugerencia de enseñanza nontargeted puede crearse mediante la eliminación de la cola.

![Una aplicación de ejemplo con una sugerencia para la enseñanza en la esquina inferior derecha. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-non-targeted.png)

Una sugerencia de enseñanza puede exigir al usuario para descartarla a través de un botón "X" en la esquina superior o un botón "Cerrar" en la parte inferior. Una sugerencia de enseñanza es posible que también se descarte ligero no habilitarse en cuyo caso es ningún botón Descartar y la sugerencia de enseñanza en su lugar, se cerrará cuando un usuario se desplaza o interactúa con otros elementos de la aplicación. Debido a este comportamiento, fácil de descartar las sugerencias son la mejor solución cuando una sugerencia debe ubicarse en un área desplazable. 

![Una aplicación de ejemplo con una sugerencia para la enseñanza en la esquina inferior derecha de descarte ligero. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba."](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Crear una sugerencia de enseñanza

Este es el XAML para una destino para la enseñanza sugerencia control que muestra el aspecto predeterminado de la TeachingTip con un título y subtítulo. Tenga en cuenta que la sugerencia de enseñanza puede aparecer en cualquier lugar en el árbol de elementos o el código subyacente. En el ejemplo siguiente, se encuentra en un objeto ResourceDictionary.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
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

Este es el resultado cuando se muestran la página que contiene el botón y enseñanza Sugerencia:

![Una aplicación de ejemplo con una punta de enseñanza destinadas a la operación de guardar botón. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Sugerencias para que no son de destino

No todas las sugerencias se refieren a un elemento en la pantalla. Para estos escenarios, no establezca la propiedad de destino y en su lugar, se mostrará la sugerencia de enseñanza en relación con los bordes de la raíz de xaml. Sin embargo, una sugerencia de enseñanza puede tener la cola quita conservando la colocación relativa de un elemento de interfaz de usuario estableciendo la propiedad TailVisibility en "Collapsed". El ejemplo siguiente es de una sugerencia de enseñanza sin destino.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

Tenga en cuenta que en este ejemplo el TeachingTip está en el árbol de elementos en lugar de en un objeto ResourceDictionary o en el código subyacente. Esto no tiene ningún efecto sobre el comportamiento; el TeachingTip sólo muestra cuando se abre y ocupa ningún espacio de diseño.

![Una aplicación de ejemplo con una sugerencia para la enseñanza en la esquina inferior derecha. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Selección de ubicación preferida

Sugerencia de enseñanza replica del control flotante [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) comportamiento de selección de ubicación con la propiedad TeachingTipPlacementMode. El modo de selección de ubicación predeterminado intentará colocar una sugerencia de enseñanza dirigidos por encima de su destino y una sugerencia de enseñanza sin destino centrado en la parte inferior de la raíz de xaml. Como con el control flotante, si el modo de selección de ubicación preferida no quedaría espacio para la sugerencia de enseñanza mostrar, otro modo de selección de ubicación automáticamente elegirá. 

Para las aplicaciones que predicen la entrada del controlador para juegos, consulte [interacciones de gamepad y control remoto]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Se recomienda probar la accesibilidad de gamepad de cada sugerencia de enseñanza con todas las configuraciones posibles de la interfaz de usuario de una aplicación.

Aparece una sugerencia de enseñanza de destino con su PreferredPlacement establecido en "BottomLeft" con la cola centrada en la parte inferior de su destino con el cuerpo de la sugerencia de enseñanza desplazado hacia la izquierda.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con un botón "Guardar" que se ha elegido por una sugerencia de enseñanza debajo de la esquina izquierda. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-targeted-preferred-placement.png)


Aparece una sugerencia de enseñanza sin destino con su PreferredPlacement establecido en "BottomLeft" en la esquina inferior izquierda de la raíz de xaml.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![Una aplicación de ejemplo con una sugerencia para la enseñanza en la esquina inferior izquierda. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-non-targeted-preferred-placement.png)

El diagrama siguiente muestra el resultado de todos modos PreferredPlacement 13 que se pueden establecer para enseñar sugerencias de destino.
![Tres objetos con la etiqueta "target" con sugerencias que se usa en torno a ellas para mostrar los modos de destino de selección de ubicación preferida de la sugerencia de enseñanza de enseñanza de destino. Centrados en el medio el primer destino es una sugerencia de enseñanza de destino con la etiqueta "Center" hacia abajo en el destino con su cola. Centrado sobre el primer destino es una sugerencia de enseñanza de destino con la etiqueta "Top" hacia abajo en el destino con su cola. Centrado derecha de la primer destino es una sugerencia de enseñanza de destino con la etiqueta "Right" hacia la izquierda en el destino con su cola. Centrado por debajo del primer objetivo es una sugerencia de enseñanza de destino con la etiqueta "Bottom" que señala hacia arriba en el destino con su cola. Centrado de la izquierda del primer destino es una sugerencia de enseñanza de destino con la etiqueta "Izquierda" que apunta justo en el destino con su cola. A la izquierda del segundo destino tiene una sugerencia de enseñanza la etiqueta "LeftTop" que señala hacia la derecha en el destino y su cuerpo desplazó hacia arriba. Por encima del segundo destino tiene una sugerencia de enseñanza la etiqueta "TopLeft" que señala hacia abajo en el destino y su cuerpo desplazó hacia la izquierda. A la derecha del segundo destino es una sugerencia de enseñanza con la etiqueta "RightBottom" que queda de puntos en el destino y su cuerpo desplazó hacia abajo. A continuación del segundo destino tiene una sugerencia de enseñanza la etiqueta "BottomRight" que señala hacia arriba en el destino y su cuerpo desplazó hacia la derecha. Anteriormente el tercer destino tiene una sugerencia de enseñanza la etiqueta "TopRight" que señala hacia abajo en el destino y su cuerpo desplazó hacia la derecha. A la derecha del tercer destino es una sugerencia de enseñanza con la etiqueta "RightTop" que queda de puntos en el destino y su cuerpo desplazó hacia arriba. Por debajo del objetivo de la tercero es una sugerencia de enseñanza con la etiqueta "BottomLeft" que señala hacia arriba en el destino y su cuerpo desplazó hacia la izquierda. A la izquierda de la tercera de destino tiene una sugerencia de enseñanza la etiqueta "LeftBottom" que señala hacia la derecha en el destino y su cuerpo desplazó hacia abajo.](../images/teaching-tip-targeted-preferred-placement-modes.png)

El diagrama siguiente muestra el resultado de todos modos PreferredPlacement 13 que se pueden establecer para obtener sugerencias de enseñanza sin destino.
![Una ventana de aplicación que muestra nueve sugerencias sin destino enseñanza para demostrar los modos de selección de ubicación preferida sin destino la sugerencia de enseñanza. La sugerencia de enseñanza en la esquina superior izquierda de la aplicación tiene la etiqueta "TopLeft o LeftTop". La sugerencia de enseñanza centrada en el borde superior de la aplicación con la etiqueta "Top". La sugerencia de enseñanza en la esquina superior derecha de la aplicación tiene la etiqueta "TopRight o RightTop". La sugerencia de enseñanza centrada en el borde izquierdo de la aplicación con la etiqueta "Left". La sugerencia de enseñanza centrada en el medio de la aplicación con la etiqueta "Center".
La sugerencia de enseñanza centrada en el extremo derecho de la aplicación con la etiqueta "Right". La sugerencia de enseñanza en la esquina inferior izquierda de la aplicación tiene la etiqueta "BottomLeft o LeftBottom". La sugerencia de enseñanza centrada en el borde inferior de la aplicación con la etiqueta "Bottom". La sugerencia de enseñanza en la esquina inferior derecha de la aplicación tiene la etiqueta "BottomRight o RightBottom".](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Agregar un margen de selección de ubicación  

Puede controlar hasta qué punto se establece una sugerencia de destino en la docencia aparte de su destino y hasta qué punto se establece una sugerencia de enseñanza sin destino aparte de los bordes de la raíz de xaml mediante la propiedad PlacementMargin. Al igual que [margen](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin tiene cuatro valores: derecha, izquierda, superior e inferior: solo se usan los valores pertinentes. Por ejemplo, PlacementMargin.Left se aplica cuando se deja la punta del destino o en el borde izquierdo de la raíz de xaml.

El ejemplo siguiente muestra una sugerencia sin destino con izquierda/superior/derecho o inferior del PlacementMargin establecidos en 80.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![Una aplicación de ejemplo con una punta de enseñanza colocada hacia, pero no totalmente en la esquina inferior derecha. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Agregar contenido

Puede agregarse contenido a una sugerencia de enseñanza mediante la propiedad de contenido. Si hay más contenido para mostrar que lo que permitirá que el tamaño de una sugerencia de enseñanza, una barra de desplazamiento se habilitará automáticamente para permitir que un usuario se desplace el área de contenido. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una punta de enseñanza destinadas a la operación de guardar botón. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." En el área de contenido de la sugerencia de enseñanza es una casilla de verificación con la etiqueta "No mostrar sugerencias en el inicio" y debajo está el texto que dice "Puede cambiar sus preferencias de sugerencia en la configuración si cambia de opinión" donde "Configuración" es un vínculo a la página de configuración de la aplicación. Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Agregar botones

De forma predeterminada, un estándar "X" botón de cierre se muestra junto al título de una sugerencia de enseñanza. El botón Cerrar puede personalizarse con la propiedad CloseButtonContent, en el que caso el botón se mueve a la parte inferior de la sugerencia de enseñanza.

**Nota: Ningún botón Cerrar aparecerá en habilitado sugerencias de descarte ligero**

Estableciendo la propiedad ActionButtonContent (y opcionalmente el ActionButtonCommand y las propiedades ActionButtonCommandParameter) se puede agregar un botón de acción personalizada.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
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
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una punta de enseñanza destinadas a la operación de guardar botón. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." En el área de contenido de la sugerencia de enseñanza es una casilla de verificación con la etiqueta "No mostrar sugerencias en el inicio" y debajo está el texto que dice "Puede cambiar sus preferencias de sugerencia en la configuración si cambia de opinión" donde "Configuración" es un vínculo a la página de configuración de la aplicación. En la parte inferior de la enseñanza, hay dos botones, uno gris a la izquierda que se lee "Deshabilitar" y uno azul a la derecha que se lee "entendido!"](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Contenido de imagen prominente

Edge al contenido de edge se puede agregar a una sugerencia de enseñanza estableciendo la propiedad HeroContent. La ubicación del contenido de imagen prominente puede establecerse en la parte superior o inferior de una sugerencia de enseñanza estableciendo la propiedad HeroContentPlacement.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una punta de enseñanza destinadas a la operación de guardar botón. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." En la parte inferior de la sugerencia de enseñanza es una imagen de borde para el borde de un hombre de dibujos animados poner archivos en la nube. Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Agregar un icono

Puede agregarse un icono al lado el título y subtítulo mediante la propiedad IconSource. Tamaños de icono recomendado incluyen 16px, 24 px y 32 px. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Una aplicación de ejemplo con una punta de enseñanza destinadas a la operación de guardar botón. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." A la izquierda del título y subtítulo es un icono del disquete. Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Habilitación de descarte ligero

Funcionalidad de descarte ligero está deshabilitada de forma predeterminada, pero se puede habilitar para que una sugerencia de enseñanza, se cerrará, por ejemplo, cuando un usuario se desplaza o interactúa con otros elementos de la aplicación. Debido a este comportamiento, fácil de descartar las sugerencias son la mejor solución cuando una sugerencia debe ubicarse en un área desplazable. 

El botón de cierre se quitará automáticamente de una sugerencia de enseñanza habilitado descarte ligero para identificar su comportamiento a los usuarios de descarte ligero. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![Una aplicación de ejemplo con una sugerencia para la enseñanza en la esquina inferior derecha de descarte ligero. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba."](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Secuencias de escape de los límites de la raíz de xaml

En Windows versión 19H 1 y anteriores, una enseñanza la sugerencia puede escapar los límites de la raíz de xaml y la pantalla estableciendo la propiedad ShouldConstrainToRootBounds. Cuando esta propiedad está habilitada, una sugerencia de enseñanza no intentará mantenerse en los límites de la raíz de xaml ni la pantalla y usarán la posición en el conjunto de modo PreferredPlacement. Se recomienda habilitar la propiedad IsLightDismissEnabled y establezca el modo de PreferredPlacement más cercana al centro de la raíz de xaml para garantizar la mejor experiencia para los usuarios.

En versiones anteriores de Windows, esta propiedad se omite y la sugerencia de enseñanza siempre permanece dentro de los límites de la raíz de xaml.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![Una aplicación de ejemplo con una punta de enseñanza fuera inferior derecha de la aplicación. El título de la sugerencia se denomine "Guardar automáticamente" y el subtítulo "Se guarde los cambios a medida que avance - por lo que nunca deba." Hay un botón Cerrar en la esquina superior derecha de la sugerencia de enseñanza.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>La cancelación y el aplazamiento de cierre

El evento de cierre puede usarse para cancelar o aplazar el cierre de una sugerencia de enseñanza. Esto puede utilizarse para mantener la punta de enseñanza abierto o deje tiempo para una acción o una animación personalizada que se produzca. Cuando se cancela el cierre de una sugerencia de enseñanza, IsOpen volverá a true, sin embargo, esta permanecerá false durante el aplazamiento. También se puede cancelar un cierre mediante programación. 

**Nota: Si no hay ninguna opción de selección de ubicación permitiría una sugerencia de enseñanza mostrar completamente, sugerencia de enseñanza iterará a través de su ciclo de vida de evento para forzar un cierre en lugar de mostrar sin un botón Cerrar accesible. Si la aplicación cancela el evento de cierre, la sugerencia de enseñanza puede permanecer abierta sin un botón Cerrar accesible.**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
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

## <a name="remarks"></a>Comentarios

### <a name="related-articles"></a>Artículos relacionados 

* [Cuadros de diálogo y controles flotantes](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>Recomendaciones
* Sugerencias son temporales y no deben contener información u opciones que son críticas para la experiencia de una aplicación. 
* Intente evitar mostrar sugerencias de enseñanza con demasiada frecuencia. Sugerencias para la enseñanza están más probables que cada recepción atención individual cuando están escalonados a lo largo de largas sesiones o en varias sesiones.    
* Mantenga sugerencias concisas y su tema claro. Investigación muestra los usuarios, en promedio, sólo de lectura 3 a 5 palabras y comprender solo palabras 2 y 3 antes de decidir si se debe interactuar con una sugerencia.
* No se garantiza la accesibilidad del controlador para juegos de una sugerencia de enseñanza. Para las aplicaciones que predicen la entrada del controlador para juegos, consulte [interacciones de gamepad y control remoto]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Se recomienda probar la accesibilidad de gamepad de cada sugerencia de enseñanza con todas las configuraciones posibles de la interfaz de usuario de una aplicación.
* Al habilitar una sugerencia de enseñanza anular la raíz de xaml, se recomienda también habilitar la propiedad IsLightDismissEnabled y establezca el modo de PreferredPlacement más cercana al centro de la raíz de xaml. 

### <a name="reconfiguring-an-open-teaching-tip"></a>Volver a configurar una sugerencia de enseñanza abierto

Pueden volver a configurar algunas propiedades y el contenido mientras está abierta de la sugerencia de enseñanza y surtirá efecto inmediatamente. Otras propiedades y el contenido, como la propiedad de icono, los botones de acción y cerrar y volver a configurar entre descarte ligero y explícita descartar todos ellos necesitarán la sugerencia de enseñanza cerrar y volver a abrir para cambios a estas propiedades para que surta efecto. Tenga en cuenta que cambiar el comportamiento de despido de manual de Descartar para descartar luz mientras está abierta una sugerencia de enseñanza provocará que tiene su botón Close quitarse antes de la punta de enseñanza el descarte ligero comportamiento está habilitado y la sugerencia puede permanecer bloqueada en la pantalla.

---
author: Jwmsft
Description: Use a tooltip to reveal more info about a control before asking the user to perform an action.
title: Información de herramientas
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5a61b8bdcfcfad490528cdceed5e732a6f5f3a89
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4950128"
---
# <a name="tooltips"></a>Información sobre herramientas

La información sobre herramientas es una breve descripción que está vinculada con otro control u objeto. Dicha información sobre herramientas ayuda a los usuarios a comprender objetos que no les son familiares y que no están directamente descritos en la UI. Aparece de forma automática cuando el usuario mueve el foco al control, lo mantiene presionado o cuando pasa sobre él con el puntero del mouse. La información sobre herramientas desaparece tras unos segundos o cuando el usuario mueve el dedo, el puntero o el foco del teclado o del controlador para juegos.

![Una información sobre herramientas](images/controls/tool-tip.png)

> **API importantes**: [Clase ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip), [Clase ToolTipService](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa información sobre herramientas para ofrecer más información sobre un control antes de pedirle al usuario que realice una acción. Los controles de información sobre herramientas deben usarse de vez en cuando y solamente cuando aporten un valor determinado para el usuario que intenta finalizar una tarea. Por regla general, si la información se encuentra disponible en todas partes en la misma experiencia, no es necesaria la información sobre herramientas. Esta información será valiosa cuando explique una acción poco clara.

¿Cuándo deberías usar información sobre herramientas? Para decidirte, intenta responder a estas preguntas:

- **¿Debe ser visible la información en función de desplazamiento del puntero?**
    Si no es así, usa otro control. Las sugerencias deben aparecer como resultado de la interacción del usuario, y nunca deben mostrarse de forma indiscriminada.

- **¿Tiene el control una etiqueta de texto?**
    Si no es así, usa la información sobre herramientas para proporcionar la etiqueta que no tiene. Una buena práctica de diseño de la experiencia de usuario consiste en etiquetar la mayoría de los controles en línea, y para esos no necesitarás usar información sobre herramientas. Los controles de la barra de herramientas y los botones de comando que muestran solo iconos necesitan información sobre herramientas.

- **¿Se beneficiaría el objeto de una descripción o de cierta información adicional?**
    Si es así, usa la información sobre herramientas. Pero el texto debe ser complementario, es decir, no debe ser algo esencial para la tarea principal. Si es un concepto esencial, deberías ponerlo directamente en la UI para que los usuarios no tengan que descubrirlo ni buscarlo.

- **¿Es la información complementaria un error, una advertencia o un estado?**
    Si es así, usa otro elemento de UI, como un control flotante.

- **¿Es necesario que los usuarios interactúen con el texto de información?**
    Si es así, usa otro control. Los usuarios no pueden interactuar con el texto de información ya que en cuanto se mueve el mouse el texto desaparece.

- **¿Necesitan los usuarios imprimir la información complementaria?**
    Si es así, usa otro control.

- **¿Es posible que el texto de información estorbe o distraiga a los usuarios?**
    Si es así, plantéate la opción de usar otra solución, sin descartar la idea de no hacer nada. Si vas a usar sugerencias en lugares donde pueden resultar una distracción, permite que los usuarios puedan desactivarlas.

## <a name="example"></a>Ejemplo

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ToolTip">abrir la aplicación y ver ToolTip en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Una información sobre herramientas de la aplicación Mapas de Bing.

![Una información sobre herramientas de la aplicación Mapas de Bing](images/control-examples/tool-tip-maps.png)

## <a name="create-a-tooltip"></a>Crear una información sobre herramientas

Se debe asignar una [información sobre herramientas](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) a otro elemento de interfaz de usuario que sea su propietario. La clase [ToolTipService](/uwp/api/windows.ui.xaml.controls.tooltipservice) proporciona métodos estáticos para mostrar una información sobre herramientas.

En XAML, usa la propiedad adjunta **ToolTipService.Tooltip** para asignar la información sobre herramientas a un propietario.

```xaml
<Button Content="Submit" ToolTipService.ToolTip="Click to submit"/>
```

En código, usa el método [ToolTipService.SetToolTip](/uwp/api/windows.ui.xaml.controls.tooltipservice.settooltip) para asignar la información sobre herramientas a un propietario.

```xaml
<Button x:Name="submitButton" Content="Submit"/>
```

```csharp
ToolTip toolTip = new ToolTip();
toolTip.Content = "Click to submit";
ToolTipService.SetToolTip(submitButton, toolTip);
```

### <a name="content"></a>Contenido

Puedes usar cualquier objeto como el [Contenido](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) de una información sobre herramientas. Este es un ejemplo del uso de una [Imagen](/uwp/api/windows.ui.xaml.controls.image) en una información sobre herramientas.

```xaml
<TextBlock Text="store logo">
    <ToolTipService.ToolTip>
        <Image Source="Assets/StoreLogo.png"/>
    </ToolTipService.ToolTip>
</TextBlock>
```

### <a name="placement"></a>Colocación

De manera predeterminada, una información sobre herramientas aparece centrada encima del puntero. La ubicación no está restringida por la ventana de la aplicación, por lo que es posible que la información sobre herramientas se muestre parcialmente o completamente fuera de los límites de la ventana de la aplicación.

Ajustes de amplia, usa la propiedad de [colocación](/uwp/api/windows.ui.xaml.controls.tooltip.placement) o **ToolTipService.Placement** conectados para especificar si debe dibujar la información sobre herramientas arriba, debajo, a la izquierda o derecha del puntero. Puedes establecer las propiedades [VerticalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.verticaloffset) o [HorizontalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.horizontaloffset) para cambiar la distancia entre el puntero y la información sobre herramientas. Solo uno de los dos valores de desplazamiento, influirá en la posición final - VerticalOffset cuando colocación es superior o inferior, HorizontalOffset cuando se deja la colocación o la derecha.

```xaml
<!-- An Image with an offset ToolTip. -->
<Image Source="Assets/StoreLogo.png">
    <ToolTipService.ToolTip>
        <ToolTip Content="Offset ToolTip."
                 Placement="Right"
                 HorizontalOffset="20"/>
    </ToolTipService.ToolTip>
</Image>
```

Si una información sobre herramientas oculta el contenido que hace referencia a, puedes ajustar su ubicación con precisión con la nueva propiedad **PlacementRect** . PlacementRect ancla la posición de la información sobre herramientas y también sirve como un área que no se tapar la información sobre herramientas, siempre que haya suficiente espacio en pantalla para dibujar la información sobre herramientas fuera del área. Puedes especificar el origen del rectángulo en relación con el propietario de la información sobre herramientas y el alto y ancho del área de exclusión. La propiedad de [colocación](/uwp/api/windows.ui.xaml.controls.tooltip.placement) definirá si se debe dibujar información sobre herramientas arriba, debajo, a la izquierda o derecha de la PlacementRect. 

```xaml
<!-- An Image with a non-occluding ToolTip. -->
<Image Source="Assets/StoreLogo.png" Height="64" Width="96">
    <ToolTipService.ToolTip>
        <ToolTip Content="Non-occluding ToolTip."
                 PlacementRect="0,0,96,64"/>
    </ToolTipService.ToolTip>
</Image>
```

## <a name="recommendations"></a>Recomendaciones

- Usa información sobre herramientas con moderación (o no la uses en absoluto). La información sobre herramientas es una interrupción. La información sobre herramientas puede distraer tanto como un elemento emergente, así que no la uses a no ser que aporte un valor importante.
- El texto de la información sobre herramientas debe ser conciso. La información sobre herramientas son perfectas para frases cortas y fragmentos de frases. Los grandes bloques de texto pueden ser abrumadores y la información sobre herramientas puede agotar el tiempo antes de que el usuario haya terminado de leer.
- Crea texto de información sobre herramientas complementario y de utilidad. El texto de la información sobre herramientas debe ser informativo. Trata de que no sea información obvia ni repetir lo que ya hay en pantalla. Como la información sobre herramientas no está siempre visible, debería ser información complementaria que no sea obligatorio que los usuarios lean. Comunica información importante con etiquetas de control que se expliquen por sí solas o texto complementario en contexto.
- Usa imágenes cuando sea conveniente. En algunas ocasiones resulta mejor usar una imagen en la información sobre herramientas. Por ejemplo, cuando un usuario mantiene el puntero sobre un hipervínculo puedes usar la información sobre herramientas para mostrar una vista previa de la página vinculada.
- No uses la información sobre herramientas para mostrar texto que ya aparece en la interfaz de usuario. Por ejemplo, no pongas información sobre herramientas en un botón si muestra el mismo texto del botón.
- No pongas controles interactivos dentro de la información sobre herramientas.
- No pongas imágenes que parezcan interactivas dentro de la información sobre herramientas.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase ToolTip](https://msdn.microsoft.com/library/windows/apps/br227608)

---
description: Usa información sobre herramientas para ofrecer más información sobre un control antes de pedir al usuario que realice una acción.
title: Información sobre herramientas
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0c9d2f26acfe431eb1be895f1021d544f78b52fb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030808"
---
# <a name="tooltips"></a>Información sobre herramientas

La información sobre herramientas es una breve descripción que está vinculada con otro control u objeto. Dicha información sobre herramientas ayuda a los usuarios a comprender objetos que no les son familiares y que no están directamente descritos en la UI. Aparece de forma automática cuando el usuario mueve el foco al control, lo mantiene presionado o cuando pasa sobre él con el puntero del mouse. La información sobre herramientas desaparece tras unos segundos o cuando el usuario mueve el dedo, el puntero o el foco del teclado o del controlador para juegos.

![Una información sobre herramientas](images/controls/tool-tip.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

:::row:::
   :::column:::
      ![Logotipo de WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de plataforma** : [Clase ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) y [Clase ToolTipService](/uwp/api/windows.ui.xaml.controls.tooltipservice)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa información sobre herramientas para ofrecer más información sobre un control antes de pedir al usuario que realice una acción. Los controles de información sobre herramientas deben usarse de vez en cuando y solamente cuando aporten un valor determinado para el usuario que intenta finalizar una tarea. Por regla general, si la información se encuentra disponible en todas partes en la misma experiencia, no es necesaria la información sobre herramientas. Esta información será valiosa cuando explique una acción poco clara.

¿Cuándo deberías usar información sobre herramientas? Para decidirte, intenta responder a estas preguntas:

- **¿Debe ser visible la información en función del desplazamiento del puntero?**
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
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ToolTip">abrir la aplicación y ver ToolTip en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Una información sobre herramientas de la aplicación Mapas de Windows.

![Una información sobre herramientas de la aplicación Mapas de Windows](images/control-examples/tool-tip-maps.png)

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

Puedes usar cualquier objeto como [Contenido](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) de una información sobre herramientas. Este es un ejemplo del uso de una [Imagen](/uwp/api/windows.ui.xaml.controls.image) en una información sobre herramientas.

```xaml
<TextBlock Text="store logo">
    <ToolTipService.ToolTip>
        <Image Source="Assets/StoreLogo.png"/>
    </ToolTipService.ToolTip>
</TextBlock>
```

### <a name="placement"></a>Selección de ubicación

De manera predeterminada, una información sobre herramientas aparece centrada encima del puntero. La ubicación no está restringida por la ventana de la aplicación, por lo que es posible que la información sobre herramientas se muestre parcial o completamente fuera de los límites de la ventana de la aplicación.

Para realizar ajustes amplios, usa la propiedad [Placement](/uwp/api/windows.ui.xaml.controls.tooltip.placement) o la propiedad adjunta **ToolTipService.Placement** para especificar si la información sobre herramientas se debe dibujar encima, debajo, a la izquierda o la derecha del puntero. Puedes establecer las propiedades [VerticalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.verticaloffset) u [HorizontalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.horizontaloffset) para cambiar la distancia entre el puntero y la información sobre herramientas. Solo uno de los dos valores de desplazamiento influirá en la posición final: VerticalOffset cuando el valor de Placement es Top o Bottom, HorizontalOffset cuando el valor de Placement es Left o Right.

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

Si una información sobre herramientas oculta el contenido al que hace referencia, puedes ajustar su colocación de forma precisa con la nueva propiedad **PlacementRect**. PlacementRect ancla la posición de la información sobre herramientas y también sirve como un área que esa información sobre herramientas no obstruye, siempre y cuando haya suficiente espacio en la pantalla para dibujar la información sobre herramientas fuera de esta área. Puedes especificar el origen del rectángulo con relación al propietario de la información sobre herramientas, además del alto y ancho del área de exclusión. La propiedad [Placement](/uwp/api/windows.ui.xaml.controls.tooltip.placement) definirá si la información sobre herramientas se debe dibujar encima, debajo a la izquierda o a la derecha del área de PlacementRect. 

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

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)

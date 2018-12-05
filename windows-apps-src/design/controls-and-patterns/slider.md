---
Description: Lets the user set a value in a given range.
title: Controles deslizantes
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e13f28d9b82bca04d108bac818faa873567d77f7
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8686065"
---
# <a name="sliders"></a>Controles deslizantes

 

Un control deslizante es un control que permite que el usuario seleccione entre un intervalo de valores moviendo un control de posición por una pista.

> **API importantes**: [Clase Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), [Propiedad Valor](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx), [Evento ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)

![Control deslizante](images/controls/slider.png)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control deslizante cuando quieras que tus usuarios puedan establecer valores contiguos definidos (como el volumen o el brillo) o un intervalo de valores discretos (como la configuración de resolución de pantalla).

Un control deslizante es una buena opción si sabes que los usuarios ven el valor como una cantidad relativa y no como un valor numérico. Por ejemplo, al ajustar el volumen, los usuarios piensan en términos de bajo o medio, y no piensan que deben ajustar el valor de volumen en 2 o 5.

No uses un control deslizante para una configuración binaria. En lugar de ello, usa un [control de alternancia](toggles.md).

A continuación encontrarás algunos factores adicionales que deberás tener en cuenta a la hora de decidirte a usar un control deslizante:

-   **¿Parece que el valor de configuración es una cantidad relativa?** Si no es así, usa [botones de radio](radio-button.md) o un [cuadro de lista](lists.md).
-   **¿Es el valor de configuración un valor numérico exacto y conocido?** Si es así, usa un [cuadro de texto](text-box.md).
-   **¿Sería bueno que el usuario obtuviera una respuesta inmediata sobre el efecto de los cambios realizados en la configuración?** Si es así, usa un control deslizante. Por ejemplo, los usuarios pueden elegir un color más fácilmente si ven de forma inmediata el efecto de los cambios en los valores de matiz, saturación o luminosidad.
-   **¿Tiene la configuración un intervalo de cuatro o más valores?** Si no es así, usa [botones de radio](radio-button.md).
-   **¿Puede el usuario cambiar el valor?** Los controles deslizantes están destinados a la interacción del usuario. Si un usuario no puede cambiar el valor, usa texto de solo lectura.

Si intentas decidir entre un control deslizante y un cuadro de texto numérico, usa el cuadro de texto numérico si:

-   El espacio en pantalla es limitado.
-   Es probable que el usuario prefiera usar el teclado.

Usa el control deslizante si:

-   Los usuarios van a sacar provecho de una respuesta instantánea.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Slider">abrir la aplicación y ver el control deslizante en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un control deslizante para controlar el volumen en Windows Phone.

![Un control deslizante para controlar el volumen en Windows Phone](images/control-examples/slider-phone.png)

Un control deslizante para cambiar el tamaño del texto en la configuración de pantalla de Windows.

![Un control deslizante para cambiar el tamaño del texto en la configuración de pantalla de Windows](images/control-examples/slider-display-settings.png)

## <a name="create-a-slider"></a>Crear un control deslizante

Aquí te mostramos cómo crear un control deslizante en el código XAML.

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

Aquí te mostramos cómo crear un control deslizante en el código.

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

Obtienes y configuras el valor del control deslizante de la propiedad [Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx). Para responder a los cambios de valor, puedes usar el enlace de datos para enlazar la propiedad de valor o controlar los eventos [ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx).

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## <a name="recommendations"></a>Recomendaciones

-   Da al control el tamaño que facilite a los usuarios el establecimiento del valor que quieran. En el caso de opciones con valores discretos, asegúrate de que al usuario le resulte fácil seleccionar cualquier valor mediante el mouse. Asegúrate de que los extremos del control deslizante caben siempre en los límites de una vista.
-   Proporciona una respuesta inmediata mientras el usuario hace una selección, o a continuación (siempre que resulte práctico). Por ejemplo, el control de volumen de Windows emite un sonido para indicar el volumen de audio seleccionado.
-   Usa etiquetas para mostrar el intervalo de valores. Excepción: si el control deslizante está orientado verticalmente y la etiqueta superior es Máximo, Alto, Más o equivalente, en cuyo caso puedes omitir las demás etiquetas porque el significado queda claro.
-   Deshabilita todas las etiquetas asociadas o los indicadores visuales de respuesta cuando deshabilites el control deslizante.
-   Ten en cuenta la dirección del texto al establecer la dirección del flujo y/o la orientación del control deslizante. El texto va de izquierda a derecha en algunos idiomas, y de derecha a izquierda en otros.
-   No uses un control deslizante como indicador de progreso.
-   No cambies el tamaño predeterminado del control de posición del control deslizante.
-   No crees un control deslizante continuo si el intervalo de valores es muy grande y es probable que los usuarios seleccionen uno de los valores representativos del intervalo. En lugar de ello, usa esos valores como únicos pasos permitidos. Por ejemplo, si el valor de tiempo puede durar hasta 1 mes pero los usuarios solo tienen que elegir entre 1 minuto, 1 hora, 1 día o 1 mes, crea un control deslizante con estos 4 puntos como pasos.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

### <a name="choosing-the-right-layout-horizontal-or-vertical"></a>Elegir el diseño adecuado: horizontal o vertical

Puedes orientar el control deslizante de forma horizontal o vertical. Usa estas directrices para determinar qué diseño usar.

-   Usa una orientación natural. Por ejemplo, si el control deslizante representa un valor del mundo real que normalmente se muestra en vertical (como la temperatura), usa una orientación vertical.
-   Si el control se usa para buscar en archivos multimedia, como en una aplicación de vídeo, usa una orientación horizontal.
-   Cuando uses un control deslizante en una página que permita movimiento panorámico en una dirección (horizontal o verticalmente), usa una orientación para el control deslizante que sea diferente a la dirección del movimiento panorámico. En otro caso, es posible que los usuarios muevan el control deslizante y cambien su valor de forma accidental al intentar desplazar la página.
-   Si aún no estás seguro de qué orientación usar, usa aquella que mejor se ajuste a tu diseño de página.

### <a name="range-direction"></a>Dirección del intervalo

La dirección del intervalo es la dirección hacia la que mueves el control deslizante cuando lo deslizas desde su valor actual a su valor máximo.

-   Para el control deslizante vertical, pon el valor más grande en la parte superior del control deslizante, independientemente de la dirección de lectura. Por ejemplo, para un control deslizante de volumen, siempre debes poner el valor de volumen máximo en la parte superior del control deslizante. Para otros tipos de valores (como días de la semana),sigue la dirección de lectura de la página.
-   Para estilos horizontales, pon el valor inferior en el lado izquierdo del control deslizante en caso de un diseño de página de izquierda a derecha, y en el lado derecho en caso de un diseño de página de derecha a izquierda.
-   La única excepción a la directriz anterior es para las barras de búsqueda en archivos multimedia: siempre debes poner el valor inferior en el lado izquierdo del control deslizante.

### <a name="steps-and-tick-marks"></a>Pasos y marcas de graduación

-   Usa puntos de paso si no quieres que el control deslizante admita valores arbitrarios entre el mínimo y el máximo. Por ejemplo, si usas un control deslizante para especificar el número de entradas de cine que comprar, no permitas valores de punto flotante. Dale un valor de paso de 1.
-   Si especificas pasos (también denominados puntos de acoplamiento), asegúrate de que el paso final esté alineado con el valor máximo del control deslizante.
-   Usa marcas de graduación cuando quieras mostrar a los usuarios la ubicación de los valores principales o significativos. Por ejemplo, un control deslizante que controla un zoom podría tener marcas de graduación para 50%, 100% y 200%.
-   Muestra marcas de graduación cuando los usuarios necesiten saber el valor aproximado de la configuración.
-   Muestra marcas de graduación y una etiqueta de valor cuando los usuarios necesiten saber el valor exacto de la configuración que eligen, sin interactuar con el control. En otro caso, pueden usar la información sobre herramientas del valor para ver el valor exacto.
-   Siempre muestra marcas de graduación cuando los puntos de paso no sean obvios. Por ejemplo, si el control deslizante tiene 200 píxeles de anchura y 200 puntos de acoplamiento, puedes ocultar las marcas de graduación porque los usuarios no van a notar el comportamiento de acoplamiento. Pero si solo hay 10 puntos de acoplamiento, muestra las marcas de graduación.

### <a name="labels"></a>Etiquetas

-   **Etiquetas de control deslizante**

    La etiqueta del control deslizante indica para qué se usa el control.

    -   Usa una etiqueta sin punto final (esta es la convención para todas las etiquetas de control).
    -   Coloca las etiquetas encima del control deslizante si el control está en un formulario que ubica la mayoría de sus etiquetas encima de sus controles.
    -   Coloca las etiquetas a los lados si el control deslizante está en un formulario que ubica la mayoría de sus etiquetas al lado de sus controles.
    -   Evita colocar etiquetas debajo del control deslizante porque el dedo del usuario podría tapar la etiqueta cuando el usuario toca el control deslizante.
-   **Etiquetas de intervalo**

    Las etiquetas de intervalo, o de relleno, describen los valores mínimo y máximo del control deslizante.

    -   Etiqueta los dos extremos del intervalo del control deslizante, a menos que la orientación vertical haga que resulte innecesario.
    -   Si es posible, usa solo una palabra para cada etiqueta.
    -   No uses puntuación final.
    -   Asegúrate de que las etiquetas sean descriptivas y estén paralelas. Ejemplos: Máximo/Mínimo, Más/Menos, Bajo/Alto, Suave/Fuerte.
-   **Etiquetas de valor**

    Una etiqueta de valor muestra el valor actual del control deslizante.

    -   Si necesitas una etiqueta de valor, muéstrala debajo del control deslizante.
    -   Centra el texto en relación con el control e incluye las unidades (como pueden ser píxeles).
    -   Como el control de posición del control deslizante queda oculto al deslizarlo, considera la posibilidad de mostrar el valor actual de alguna otra forma, con una etiqueta u otro elemento visual. Un control deslizante que establezca el tamaño del texto podría presentar un texto de muestra del tamaño correcto al lado del control deslizante.

### <a name="appearance-and-interaction"></a>Aspecto e interacción

Un control deslizante está formado por un recorrido y un control de posición. El recorrido es una barra (que, opcionalmente, puede mostrar diversos estilos de marcas de graduación) que representa el intervalo de valores que se pueden especificar. El control de posición es un selector, que el usuario puede mover pulsando en el recorrido o arrastrándolo hacia atrás y hacia adelante.

Un control deslizante tiene un destino táctil grande. Para que la accesibilidad táctil sea buena, un control deslizante se debe poner lo suficientemente lejos del borde de la pantalla.

Al diseñar un control deslizante personalizado, piensa en la manera de presentar toda la información necesaria al usuario de la forma más clara posible. Usa una etiqueta de valor si el usuario tiene que saber cuáles son las unidades para entender la opción. Busca formas creativas de representar valores gráficamente. Por ejemplo, un control deslizante que controle el volumen podría mostrar el gráfico de un altavoz sin ondas de sonido en el extremo inferior del control y el gráfico de un altavoz con ondas de sonido en el extremo superior.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Artículos relacionados
- [Modificadores para alternar](toggles.md)
- [Clase de control deslizante](https://msdn.microsoft.com/library/windows/apps/br209614)

---
author: mijacobs
description: "Como representación visual del lenguaje, la tarea principal de la tipografía es ser clara. Su estilo nunca debe obstaculizar ese objetivo. No obstante, la tipografía también tiene un rol importante como componente del diseño, ya que aporta un gran efecto en la densidad y la complejidad del diseño y en la experiencia del usuario de ese diseño."
title: "Tipografía"
ms.assetid: ca35f78a-e4da-423d-9f5b-75896e0b8f82
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0356d9420d85fbf54718223df77ed501d4b6f5e5
ms.openlocfilehash: 45b4bbc86c69cabae4a2ee83d2d43c7189a710ce

---

# <a name="typography"></a>Tipografía

Como representación visual del lenguaje, la tarea principal de la tipografía es ser clara. Su estilo nunca debe obstaculizar ese objetivo. No obstante, la tipografía también tiene un rol importante como componente de diseño, con un gran efecto en la densidad y la complejidad del diseño, y en la experiencia de usuario de ese diseño.

## <a name="typeface"></a>Tipo de letra

Hemos seleccionado Segoe UI para su uso en todos los diseños digitales de Microsoft. Segoe UI proporciona una amplia gama de caracteres y está diseñada para mantener la legibilidad óptima en distintos tamaños y densidades de píxeles. Ofrece una estética limpia, ligera y abierta que complementa el contenido del sistema.

![Texto de muestra de la fuente Segoe UI](images/segoe-sample.png)

## <a name="weights"></a>Espesores

Nuestra visión de la tipografía tiene en cuenta la simplicidad y la eficiencia. Elegimos usar un tipo de letra, una cantidad mínima de espesores y tamaños y una jerarquía clara. El posicionamiento y la alineación siguen el estilo predeterminado para el idioma especificado. En inglés el texto va de izquierda a derecha y de arriba a abajo. Las relaciones entre el texto y las imágenes son claras y directas.

![Muestra los espesores de fuente admitidos. Delgado, semidelgado, normal, seminegrita y negrita](images/weights.png)

## <a name="line-spacing"></a>Interlineado

![Ejemplo de interlineado de 125%](images/line-spacing.png)

El interlineado debe calcularse en relación al 125% del tamaño de la fuente, con redondeo al múltiplo más cercano de cuatro cuando sea necesario. Por ejemplo, con 15px Segoe UI, 125% de 15 píxeles es 18,75 píxeles. Recomendamos redondear por arriba y establecer la alto de línea en 20 píxeles para permanecer en la cuadrícula de 4 píxeles. Esto garantiza una buena experiencia de lectura y suficiente espacio para los signos diacríticos. Consulta la sección de rampa de tipo a continuación para ver ejemplos específicos.

Cuando se apila un tipo de mayor tamaño encima de otro de menor tamaño, la distancia desde la última línea base del tipo mayor respecto a la primera línea de base del tipo menor debe ser igual que el alto de línea del tipo mayor.

![Muestra cómo se apila un tipo de mayor tamaño encima de otro de menor tamaño](images/line-height-stacking.png)

En XAML, esto se logra apilando dos [TextBlocks](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) y estableciendo el margen adecuado.

```xml
<StackPanel Width="200">
    <!-- Setting a bottom margin of 3px on the header
         puts the baseline of the body text exactly 24px
         below the baseline of the header. 24px is the
         recommended line height for a 20px font size,
         which is what's set in SubtitleTextBlockStyle.
         The bottom margin will be different for
         different font size pairings. -->
    <TextBlock
        Style="{StaticResource SubtitleTextBlockStyle}"
        Margin="0,0,0,3"
        Text="Header text" />
    <TextBlock
        Style="{StaticResource BodyTextBlockStyle}"
        TextWrapping="Wrap"
        Text="This line of text should be positioned where the above header would have wrapped." />
</StackPanel>
```



## <a name="kerning-and-tracking"></a>Interletraje y tracking

Segoe es un tipo de letra humanist con un aspecto nítido y legible, con formas orgánicas y abiertas inspiradas en el texto escrito a mano. Para garantizar la legibilidad óptima y mantener la integridad humanist, la configuración del interletraje y del tracking debe tener valores específicos.

El interletraje se debe establecer en "métrica" y el tracking debe establecerse en "0".


![Muestra la diferencia entre el interletraje y el tracking](images/kerning-tracking.png)



## <a name="word-and-letter-spacing"></a>Espacio entre palabras y entre letras

De forma parecida al interletraje y al tracking, en el espacio entre palabras y letras se usa una configuración específica para garantizar la legibilidad óptima y la integridad humanista.

El espacio entre palabras predeterminado siempre es del 100 % y el espaciado entre letras debe establecerse en "0".


![Muestra las diferencias del espaciado entre palabras y letras](images/word-letter.png)

**Nota**&nbsp;&nbsp;En un control de texto XAML se usa [Typogrphy.Kerning](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.documents.typography.kerning.aspx) para controlar el interletraje [FontStretch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.control.fontstretch.aspx) para controlar el tracking. De forma predeterminada, Typography.Kerning se ha establecido en “true”, y FontStretch se ha establecido en “Normal”; ambos son valores recomendados.




## <a name="alignment"></a>Alineación

Por lo general, recomendamos que los elementos visuales y las columnas de tipo se alineen a la izquierda. En la mayoría de los casos, la alineación del texto a la izquierda con un margen irregular a la derecha proporciona un anclaje coherente del contenido y un diseño uniforme.


![Muestra el texto alineado a la izquierda](images/alignment.png)



## <a name="line-endings"></a>Finales de línea

Cuando la tipografía no esté alineada a la izquierda con un margen irregular a la derecha, intenta que los finales de línea queden alineados y evita la división de palabras.


![Muestra finales de línea igualados](images/line-endings.png)

## <a name="paragraphs"></a>Párrafos

Para alinear los bordes de las columnas, los párrafos deben indicarse omitiendo una línea sin aplicar sangría.

![Muestra una línea completa de espacio entre párrafos](images/paragraphs.png)

## <a name="character-count"></a>Número de caracteres

Si una línea es demasiado corta, el ojo tendrá que desplazarse de izquierda a derecha con demasiada frecuencia, interrumpiendo el ritmo de lectura. Si es posible, 50 a 60 letras por línea es lo ideal para facilitar la lectura.

Segoe proporciona una amplia gama de caracteres y está diseñada para mantener la legibilidad óptima en tamaños pequeños y grandes, así como en píxeles con densidades altas y ajas. Usar el número óptimo de letras en las líneas de una columna de texto garantiza una buena legibilidad en una aplicación.

Las líneas demasiado largas provocan un mayor esfuerzo fisual y pueden desorientar al usuario. Las líneas demasiado cortas hacen que los ojos se tengan que desplazar mucho y se puede causar fatiga.

![Muestra los 3 párrafos con longitudes de línea diferente](images/character-count.png)

## <a name="hanging-text-alignment"></a>Alineación de sangría francesa

La alineación horizontal de los iconos con texto se puede controlar de distintas formas según el tamaño del icono y la cantidad de texto. Cuando el texto tiene una o varias líneas y cabe en la alto del icono, el texto se debe centrar verticalmente.

Cuando el texto se extiende más allá de la alto del icono, la primera línea del texto debe alinearse verticalmente y el texto adicional debe fluir debajo de forma natural. Al usar caracteres cuyas mayúsculas son mayores (según el alto ascendente y descendente), debes tener cuidado y seguir la misma guía de alineación.

![Muestra varios pares de iconos y texto](images/hanging-text-alignment.png)

**Nota**&nbsp;&nbsp;La propiedad de XAML [TextBlock.TextLineBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.textlinebounds.aspx) proporciona acceso al alto de las mayúsculas y a la métrica de los tipos de letra de la base de referencia. Puedes usar esta opción para visualizar de forma vertical, centrar o alinear en la parte superior el contenido.

## <a name="clipping-and-ellipses"></a>Recortes y puntos suspensivos

Recorta de manera predeterminada; el texto pasará a la siguiente línea a menos que la revisión especifique lo contrario. Cuando el texto no pasa a la siguiente línea, recomendamos recortar en lugar de usar puntos suspensivos. El recorte puede producirse en el borde del contenedor, en el borde del dispositivo, en el borde de una barra de desplazamiento, etc.

Excepciones: en contenedores que no están bien definidos (por ejemplo, no hay ningún color diferenciador de fondo), el texto que no se ajuste puede marcarse con puntos suspensivos "...".

![Muestra el marco de un dispositivo con texto recortado](images/clipping.png)

## <a name="type-ramp"></a>Rampa de tipografías
La rampa de tipos establece una relación de diseño crucial entre los títulos y el texto del cuerpo, y garantiza una jerarquía comprensible y clara entre los diferentes niveles. Esta jerarquía crea una estructura que permite a los usuarios navegar fácilmente a través de la comunicación escrita.

![Muestra la rampa de tipos](images/type-ramp.png) Todos los tamaños se muestran en píxeles efectivos. 


**Nota**&nbsp;&nbsp;La mayoría de los niveles de rampa están disponibles como [recursos estáticos](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_type_ramp) de XAML que siguen la convención de nomenclatura `*TextBlockStyle` (por ejemplo, `HeaderTextBlockStyle`).


## <a name="primary-and-secondary-text"></a>Texto principal y secundario

Para crear otras jerarquías no incluidas en la rampa de tipos, establece el texto secundario en el 60 % de opacidad. En la [paleta de colores de temas](color.md#color-theming), se usaría BaseMedium. El texto principal siempre debe tener un 100% de opacidad, o BaseHigh.


## <a name="all-caps-titles"></a>Títulos completamente en mayúsculas

Determinados títulos de página deben estar totalmente en mayúsculas para agregar otra dimensión de jerarquía. Estos títulos deben usar BaseAlt con el espaciado establecido en 75 milésimas de un espacio largo. Este tratamiento también puede usarse para facilitar la navegación por la aplicación.

Sin embargo, en algunos idiomas los nombres propios pueden cambiar su significado cuando se escriben en mayúsculas y, por lo tanto, los títulos de páginas creados con nombres propios o con información procedente del usuario *no* deben escribirse totalmente en mayúsculas.


**Cosas que hacer**



* Usa el área Cuerpo para la mayor parte del texto
* Usa el área Base para los títulos cuando el espacio es limitado
* Incluye SubtítuloAlt para crear contraste y jerarquía al enfatizar el contenido de nivel superior



**Cosas que evitar**



* No uses Título para cadenas largas o cualquier acción principal
* No uses Encabezado o Subencabezado si hay que ajustar el texto
* No combines Subtítulo y SubtítuloAlt en la misma página



## <a name="related-articles"></a>Artículos relacionados

* [Controles de texto](../controls-and-patterns/text-controls.md)



<!--HONumber=Dec16_HO1-->



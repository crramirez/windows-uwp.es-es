---
description: Aprende a usar la tipografía en tu aplicación para ayudar a los usuarios a comprender el contenido con facilidad.
title: Tipografía en aplicaciones de Windows
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5f06d5e804d41b1751c72af4d07224fa346323b2
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860102"
---
# <a name="typography-in-windows-apps"></a>Tipografía en aplicaciones de Windows

![imagen principal](images/header-typography.svg)

Como representación visual del lenguaje, la tarea principal de la tipografía es comunicar la información. Su estilo nunca debe obstaculizar ese objetivo. En este artículo hablaremos de cómo aplicar estilo a la tipografía de tu aplicación de Windows para ayudar a los usuarios a comprender el contenido de forma sencilla y eficaz.

## <a name="font"></a>Fuente

Debes usar una fuente en toda la interfaz de usuario de la aplicación y te recomendamos superponerla con la fuente predeterminada para aplicaciones de Windows, **Segoe UI**. Se ha diseñado para mantener la legibilidad óptima en tamaños y densidades de píxeles y ofrece una estética limpia, ligera y abierta que complementa el contenido del sistema.

![Texto de muestra de la fuente Segoe UI.](images/type/segoe-sample.svg)

Con el fin de mostrar idiomas que no son inglés o seleccionar una fuente diferente para tu aplicación, consulta [Idiomas](#languages) y [Fuentes](#fonts) para nuestras fuentes recomendadas para aplicaciones de Windows.

:::row:::
    :::column:::
![Primera captura de pantalla de una barra verde con una marca de verificación verde y la palabra Do.](images/do.svg)
Elija una fuente para la interfaz de usuario.
    :::column-end:::
    :::column:::
![No](images/dont.svg) No mezcles varias fuentes.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>Tamaño y escalado

Los tamaños de fuente en las aplicaciones para UWP se escalan automáticamente en todos los dispositivos. El algoritmo de escalado garantiza que una fuente de 24px en un dispositivo Surface Hub a 3 metros de distancia sea tan legible como una fuente de 24px en un teléfono de 5 pulgadas a unos centímetros de distancia.

![Distancias de visualización para diferentes dispositivos](images/type/scaling-chart.svg)

Debido a cómo funciona el sistema de escalado, diseñas en píxeles efectivos, no píxeles físicos reales y no tienes por qué modificar los tamaños de fuente para las resoluciones y los tamaños de pantallas diferentes.

:::row:::
    :::column:::
![Segunda captura de pantalla de una barra verde con una marca de verificación verde y la palabra Do.](images/do.svg)
Siga el tamaño de la [rampa de tipos](#type-ramp) de Windows.
    :::column-end:::
    :::column:::
![No](images/dont.svg) No uses un tamaño de fuente menor de 12 px.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>Jerarquía

:::row:::
    :::column:::
Los usuarios dependen de la jerarquía visual cuando analizan una página: los encabezados resumen contenido y el texto del cuerpo ofrece más detalles. Para crear una jerarquía visual clara en la aplicación, sigue la rampa de tipos de Windows.
    :::column-end:::
    :::column:::
![Captura de pantalla de tres líneas de texto en la que el tamaño de fuente se reduce de una línea a la siguiente.](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>Rampa de tipografías

La rampa de tipos de Windows establece relaciones cruciales entre los estilos de tipos de una página, lo que ayuda a los usuarios a leer fácilmente el contenido. Todos los tamaños se encuentran en píxeles efectivos y están optimizados para aplicaciones para UWP que se ejecutan en todos los dispositivos.

![Rampa de tipos de Windows.](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>Uso de la rampa de tipos

:::row:::
    :::column:::
Puedes acceder a los niveles de la rampa de tipos como [recursos estáticos](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp) XAML. Los estilos siguen la convención de nomenclatura `*TextBlockStyle` que se muestra aquí.

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```
    :::column-end:::
    :::column:::
![Captura de pantalla de los estilos de texto de encabezado, subencabezado, título, subtítulo, base, cuerpo y descripción.](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::



:::row:::
    :::column:::
![Tercera captura de pantalla de una barra verde con una marca de verificación verde y la palabra Do.](images/do.svg)
Usa "Cuerpo" para la mayor parte del texto.

Usa el área Base para los títulos cuando el espacio es limitado.
    :::column-end:::
    :::column:::
![No](images/dont.svg) No uses Título para cadenas largas ni acciones principales.

Usa Encabezado o Subencabezado si hay que ajustar el texto.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Asociación

El valor predeterminado [TextAlignment](/uwp/api/windows.ui.xaml.textalignment) es Left y, en la mayoría de los casos, la alineación del texto a la izquierda con un margen irregular a la derecha proporciona un anclaje coherente del contenido y un diseño uniforme. Para idiomas de derecha a izquierda, consulta [Ajustar el diseño y las fuentes para admitir globalización](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Muestra el texto alineado a la izquierda.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Número de caracteres

:::row:::
    :::column:::
![[Cuarta captura de pantalla de una barra verde con una marca de verificación verde y la palabra Do.](images/do.svg)
Mantenga de 50 a 60 letras por línea para facilitar la lectura.
    :::column-end:::
    :::column:::
![No](images/dont.svg) Una línea con menos de 20 caracteres o más de 60 caracteres resulta difícil de leer.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>Recortes y puntos suspensivos

Cuando la cantidad de texto se extiende más allá del espacio disponible, se recomienda recortar texto, que es el comportamiento predeterminado de la mayor parte de [controles de texto de UWP](../controls-and-patterns/text-controls.md).

![Muestra el marco de un dispositivo con texto recortado.](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![[Quinta captura de pantalla de una barra verde con una marca de verificación verde y la palabra Do.](images/do.svg)
Recorte el texto y ajústelo si hay varias líneas habilitadas.
    :::column-end:::
    :::column:::
![No](images/dont.svg) No uses puntos suspensivos para evitar la aglomeración visual.
    :::column-end:::
:::row-end:::

**Nota**: Si los contenedores no están bien definidos (por ejemplo, no hay ningún color de fondo diferenciador), o cuando hay un vínculo para ver más texto, usa puntos suspensivos.

## <a name="languages"></a>Idiomas 

Segoe UI es nuestra fuente para inglés, los idiomas europeos, griego, hebreo, armenio, georgiano y árabe. Para otros idiomas, consulta las siguientes recomendaciones.

### <a name="globalizinglocalizing-fonts"></a>Globalizar y localizar fuentes

Usa las [API de asignación de fuentes LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont) para acceder mediante programación al estilo, el espesor, el tamaño y la familia de fuentes recomendados para un idioma en particular. El objeto LanguageFont proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, texto del cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar. Para obtener más información, consulta [Ajustar el diseño y las fuentes, y admitir la escritura RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

### <a name="fonts-for-non-latin-languages"></a>Fuentes para idiomas no latinos

<table>
<thead>
<tr class="header">
<th align="left">Familia de fuentes</th>
<th align="left">Estilos</th>
<th align="left">Notas</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Normal, negrita</td>
<td align="left">Fuente de interfaz de usuario para alfabetos africanos (etiópico, N'Ko, osmaniya, tifinagh, vai)</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Normal, negrita</td>
<td align="left">Fuente de interfaz de usuario para alfabetos norteamericanos (silábico canadiense, cheroqui).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Normal, semidelgada, negrita</td>
<td align="left">Fuente de interfaz de usuario para alfabetos del sureste asiático (buginés, lao, khmer, tailandés).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Normal</td>
<td align="left">Fuente de interfaz de usuario para coreano.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, negrita, delgada</td>
<td align="left">Fuente de interfaz de usuario para chino tradicional.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, negrita, delgada</td>
<td align="left">Fuente de interfaz de usuario para chino simplificado.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para el alfabeto birmano.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Normal, semidelgada, negrita</td>
<td align="left">Fuente de interfaz de usuario para alfabetos del sur asiático (bengalí, devanagari, gujarati, gurmukhi, kannada, malayalam, odia, ol chiki, cingalés, sora sompeng, tamil, telugu)</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Normal</td>
<td align="left">Fuente de interfaz de usuario china heredada. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Delgada, semidelgada, normal, seminegrita, negrita</td>
<td align="left">Fuente de interfaz de usuario para japonés.</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>Fuentes

### <a name="sans-serif-fonts"></a>Fuentes sin serifa

Las fuentes sin serifa son una buena opción para encabezados y elementos de interfaz de usuario. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Familia de fuentes</th>
<th align="left">Estilos</th>
<th align="left">Notas</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita, gruesa</td>
<td align="left">Admite alfabetos europeos y de Oriente Medio (latino, griego, cirílico, árabe, armenio y hebreo). El espesor grueso solo admite alfabetos europeos.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Normal, cursiva, negrita, negrita cursiva, delgada, cursiva delgada</td>
<td align="left">Admite alfabetos europeos y de Oriente Medio (latino, griego, cirílico, árabe y hebreo). El árabe solo está disponible en formato vertical.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Normal, cursiva, negrita, cursiva negrita</td>
<td>Fuente de ancho fijo que admite alfabetos europeos (latino, griego y cirílico).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Normal, cursiva, cursiva delgada, cursiva gruesa, negrita, negrita cursiva, delgada, semidelgada, seminegrita, gruesa</td>
<td>Fuente de interfaz de usuario para alfabetos europeos y de Oriente Medio (árabe, armenio, cirílico, georgiano, griego, hebreo, latino) y también para el alfabeto lisu.</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, semidelgada, delgada, negrita, seminegrita</td>
<td align="left">Una fuente de código abierto que es métricamente compatible con Segoe UI, diseñada para aplicaciones en otras plataformas que no incluyen Segoe UI. <a href="https://github.com/Microsoft/Selawik">Obtén Selawik en GitHub.</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Fuentes con serifa

Las fuentes con serifa son útiles para presentar grandes cantidades de texto. 

<table>
<thead>
<tr class="header">
<th align="left">Familia de fuentes</th>
<th align="left">Estilos</th>
<th align="left">Notas</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Normal</td>
<td align="left">Fuente con serifa que admite alfabetos europeos (latino, griego, cirílico).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left">La fuente de ancho fijo con serifa admite alfabetos europeos y de Oriente Medio (latino, griego, cirílico, árabe, armenio y hebreo).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left">Admite alfabetos europeos (latino, griego y cirílico).</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left">Fuente heredada que admite alfabetos europeos (latino, griego, cirílico, árabe, armenio, hebreo).</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>Iconos y símbolos

<table>
<thead>
<tr class="header">
<th align="left">Familia de fuentes</th>
<th align="left">Estilos</th>
<th align="left">Notas</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Normal</td>
<td align="left">Fuente de interfaz de usuario para iconos de aplicación. Para obtener más información, consulta el <a href="segoe-ui-symbol-font.md">artículo sobre Segoe MDL2 Assets</a>.</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para símbolos</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>Artículos relacionados

* [Controles de texto](../controls-and-patterns/text-controls.md)
* [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [Estilos XAML](../controls-and-patterns/xaml-styles.md)
* [Tipografía de Microsoft](/typography/)

---
description: Aprende a usar la tipografía en tu aplicación para ayudar a los usuarios a comprender el contenido con facilidad.
title: Tipografía en aplicaciones para UWP
ms.date: 04/06/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0943273dab239669be75b30070222d698246aa41
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7698674"
---
# <a name="typography"></a>Tipografía

![imagen principal](images/header-typography.svg)

Como representación visual del lenguaje, la tarea principal de la tipografía es comunicar la información. Su estilo nunca debe obstaculizar ese objetivo. En este artículo hablaremos de cómo aplicar estilo a la tipografía de tu aplicación para UWP para ayudar a los usuarios a comprender el contenido de forma sencilla y eficaz.

## <a name="font"></a>Fuente

Debes usar una fuente en toda la interfaz de usuario de la aplicación y te recomendamos superponerla con la fuente predeterminada para aplicaciones para UWP, **Segoe UI**. Se ha diseñado para mantener la legibilidad óptima en tamaños y densidades de píxeles y ofrece una estética limpia, ligera y abierta que complementa el contenido del sistema.

![Texto de muestra de la fuente Segoe UI](images/type/segoe-sample.svg)

Para mostrar idiomas que no son inglés o para seleccionar una fuente diferente para tu aplicación, consulta [Idiomas](#Languages) y [Fuentes](#Fonts) para nuestras fuentes recomendadas para aplicaciones para UWP.

:::row:::
    :::column:::
        ![do](images/do.svg)
        Pick one font for your UI.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Don't mix multiple fonts.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>Tamaño y ajuste de escala

Los tamaños de fuente en las aplicaciones para UWP ajustan la escala automáticamente en todos los dispositivos. El algoritmo de escalado garantiza que una fuente de 24 px en un dispositivo Surface Hub a 3 metros de distancia sea tan legible como una fuente de 24 px en un teléfono de 5 pulgadas a unos centímetros de distancia.

![distancias de visualización para diferentes dispositivos](images/type/scaling-chart.svg)

Debido a cómo funciona el sistema de ajuste de escala, estás diseñando en píxeles efectivos, no píxeles físicos reales y no debes tener que modificar los tamaños de fuente para las resoluciones y los tamaños de pantallas diferentes.

:::row:::
    :::column:::
        ![do](images/do.svg)
        Follow the UWP [type ramp](#type-ramp) sizing.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use a font size smaller than 12 px.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>Jerarquía

:::row:::
    :::column:::
        Users rely on visual hierarchy when scanning a page: headers summarize content, and body text provides more detail. To create a clear visual hierarchy in your app, follow the UWP type ramp.
    :::column-end:::
    :::column:::
        ![text block styles](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>Rampa de tipografías

La tabla de tipos UWP establece relaciones cruciales entre los estilos de tipos de una página, ayudando a los usuarios a leer fácilmente el contenido. Todos los tamaños se encuentran en píxeles efectivos y están optimizados para aplicaciones para UWP que se ejecutan en todos los dispositivos.

![Rampa de tipografías](images/type/type-ramp.svg)

### <a name="using-the-type-ramp"></a>Usar la rampa de tipos

:::row:::
    :::column:::
        You can access levels of the type ramp as XAML [static resources](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp). The styles follow the `*TextBlockStyle` naming convention.
    :::column-end:::
    :::column:::
        ![text block styles](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row:::
    :::column:::
        ![do](images/do.svg)
        Use "Body" for most text.

        Use "Base" for titles when space is constrained.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use "Caption" for primary action or any long strings.

        Use "Header" or "Subheader" if text needs to wrap.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Alineación

El valor predeterminado [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) es Left y, en la mayoría de los casos, la alineación del texto a la izquierda con un margen irregular a la derecha proporciona un anclaje coherente del contenido y un diseño uniforme. Para idiomas RTL, consulta [Ajustar el diseño y las fuentes, y admitir la escritura RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Muestra el texto alineado a la izquierda.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Número de caracteres

:::row:::
    :::column:::
        ![do](images/do.svg)
        Keep to 50–60 letters per line for ease of reading.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Less than 20 characters or more than 60 characters per line is difficult to read.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>Recortes y puntos suspensivos

Cuando la cantidad de texto se extiende más allá del espacio disponible, se recomienda recortar texto, que es el comportamiento predeterminado de la mayor parte de [controles de texto UWP](../controls-and-patterns/text-controls.md).

![Muestra el marco de un dispositivo con texto recortado](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
        ![do](images/do.svg)
        Clip text, and wrap if multiple lines are enabled.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use ellipses to avoid visual clutter.
    :::column-end:::
:::row-end:::

**Nota**: Si los contenedores no están bien definidos (por ejemplo, no hay ningún color diferenciador de fondo), o cuando hay un vínculo para ver más texto, usa puntos suspensivos.

## <a name="languages"></a>Idiomas 

Segoe UI es nuestra fuente para inglés, los idiomas europeos, griego, hebreo, armenio, georgiano y árabe. Para otros idiomas, consulta las siguientes recomendaciones.

### <a name="globalizinglocalizing-fonts"></a>Globalizar y localizar fuentes

Usa las [API de asignación de fuentes LanguageFont](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont) para acceder mediante programación al estilo, el espesor, el tamaño y la familia de fuentes recomendados para un idioma en particular. El objeto LanguageFont proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, texto del cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar. Para obtener más información, consulta [Ajustar el diseño y las fuentes, y admitir la escritura RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

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
<td align="left">Normal, Semilight, negrita</td>
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
<td align="left">Fuente de interfaz de usuario para alfabetos del sur asiático (bangla, devanagari, gujarati, gurmukhi, kannada, malayalam, odia, ol chiki, sinhala, sora sompeng, tamil, telugu)</td>
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
* [Tipografía de Microsoft](https://docs.microsoft.com/typography/)

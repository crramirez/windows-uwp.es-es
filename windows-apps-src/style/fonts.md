---
author: Jwmsft
Description: "Sigue estas directrices para seleccionar fuentes y especificar tamaños y colores de fuente para aplicaciones para UWP."
title: Fuentes para aplicaciones para UWP
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 0b25dc91a5ec82a83ae24a41854e9eeab8990128

---


# <a name="fonts-for-uwp-apps"></a>Fuentes para aplicaciones para UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

En este artículo se enumeran las fuentes recomendadas para aplicaciones para UWP. Se garantiza que estas fuentes estarán disponibles en todas las ediciones de Windows 10 que admitan aplicaciones para UWP.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Propiedad FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)</li>
</ul>
</div>

La [guía de tipografía para UWP](typography.md) recomienda que las aplicaciones usen la fuente Segoe UI, pero, aunque Segoe UI es una buena opción para la mayoría de las aplicaciones, no tienes que usarla para todo. Puedes usar otras fuentes para determinados escenarios, como la lectura o al mostrar texto en algunos idiomas distintos del inglés. 
 
## <a name="sans-serif-fonts"></a>Fuentes sin serifa

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

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para alfabetos históricos</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, semidelgada, delgada, negrita, seminegrita</td>
<td align="left">Una fuente de código abierto que es métricamente compatible con Segoe UI, diseñada para aplicaciones en otras plataformas que no incluyen Segoe UI. [Obtén Selawik en GitHub.](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left">Admite alfabetos europeos (latino, griego, cirílico y armenio).</td>
</tr>

</tbody>
</table>


## <a name="serif-fonts"></a>Fuentes con serifa

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

## <a name="symbols-and-icons"></a>Iconos y símbolos


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
<td align="left">Fuente de interfaz de usuario para iconos de aplicación. Para obtener más información, consulta el [artículo sobre Segoe MDL2 Assets](segoe-ui-symbol-font.md).</td>
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



## <a name="fonts-for-non-latin-languages"></a>Fuentes para idiomas no latinos

Muchas de estas fuentes también proporcionan caracteres latinos.

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
<tr class="even">
<td align="left">Fuente de reserva normal de texto javanés para el alfabeto javanés</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para script javanés</td>
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
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para el alfabeto tibetano.</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, negrita, delgada</td>
<td align="left">Fuente de interfaz de usuario para chino tradicional.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para el alfabeto New Tai Lue.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para el alfabeto Phags-pa.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para alfabeto Tai Le.</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, negrita, delgada</td>
<td align="left">Fuente de interfaz de usuario para chino simplificado.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para el alfabeto yi.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para el alfabeto mongol.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">Normal</td>
<td align="left">Fuente de reserva para alfabeto Thaana.</td>
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
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">Media gruesa</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Normal</td>
<td align="left">Fuente de interfaz de usuario para japonés.</td>
</tr>
</tbody>
</table>


## <a name="globalizinglocalizing-fonts"></a>Globalizar y localizar fuentes
Usa las [API de asignación de fuentes LanguageFont](https://msdn.microsoft.com/library/windows/apps/br206864) para acceder mediante programación al estilo, el espesor, el tamaño y la familia de fuentes recomendados para un idioma en particular. El objeto LanguageFont proporciona acceso a la información de fuente correcta para diversas categorías de contenido, como encabezados de interfaz de usuario, notificaciones, texto del cuerpo y fuentes de cuerpo de documento que los usuarios pueden editar. Para obtener más información, consulta [Ajustar el diseño y las fuentes, y admitir la escritura RTL](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl).


## <a name="get-the-samples"></a>Obtener muestras

* [Muestra de fuentes descargable](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [Muestra de conceptos básicos de interfaz de usuario](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Espaciado entre líneas con la muestra de DirectWrite](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## <a name="related-articles"></a>Artículos relacionados

* [Ajustar el diseño y las fuentes para admitir la globalización](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [Controles de texto](../controls-and-patterns/text-controls.md)
* [Recursos de temas XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Dec16_HO2-->



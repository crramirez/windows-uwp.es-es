---
author: Jwmsft
Description: Sigue estas directrices cuando selecciones fuentes y especifiques tamaños y colores de fuente.
title: Fuentes
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
---

# Directrices sobre fuentes

**API importantes**

-   [**Propiedad FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)

El uso correcto del tamaño, espesor, color, interlineado y espaciado de las fuentes puede conferir a tu aplicación para la Plataforma universal de Windows (UWP) un aspecto ordenado y organizado que facilite su uso. Sigue estas directrices cuando selecciones fuentes y especifiques tamaños y colores de fuente.

Si buscas una lista de iconos Segoe UI Symbol, consulta [**Directrices para iconos de Segoe UI Symbol**](segoe-ui-symbol-font.md).

## <span id="The_Windows_10_type_ramp"></span><span id="the_windows_10_type_ramp"></span><span id="THE_WINDOWS_10_TYPE_RAMP"></span>La rampa de tipos de Windows 10


La rampa de tipos establece una relación de diseño crucial entre los títulos y el texto del cuerpo, y garantiza una jerarquía comprensible y clara entre los diferentes niveles. Los usuarios comprenden de inmediato dónde pueden encontrar la información y cómo analizar la página.

Esta es la tabla de tipos que se recomienda para aplicaciones UWP:

| Estilo de texto | Tipo de letra | Grosor    | Tamaño (epx) | Espaciado entre líneas (epx) | Espacio entre palabras | Ajuste de espaciado entre caracteres (1/1000 em) | Clave de estilo XAML          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| Encabezado     | Segoe UI | Light     | 46         | 56                 | 100%         | 0                    | HeaderTextBlockStyle    |
| Subencabezado  | Segoe UI | Light     | 34         | 40                 | 100%         | 0                    | SubheaderTextBlockStyle |
| Título      | Segoe UI | Semilight | 24         | 28                 | 100%         | 0                    | TitleTextBlockStyle     |
| Subtítulo   | Segoe UI | Regular   | 20         | 24                 | 100%         | 0                    | SubtitleTextBlockStyle  |
| Base       | Segoe UI | Semibold  | 15         | 20                 | 100%         | 0                    | BaseTextBlockStyle      |
| Cuerpo       | Segoe UI | Regular   | 15         | 20                 | 100%         | 0                    | BodyTextBlockStyle      |
| Subtítulo    | Segoe UI | Regular   | 12         | 14                 | 100%         | 0                    | CaptionTextBlockStyle   |

 

## <span id="Recommended_fonts"></span><span id="recommended_fonts"></span><span id="RECOMMENDED_FONTS"></span>Fuentes recomendadas


No tienes que usar la fuente Segoe UI para todo. Puedes usar otras fuentes para determinados escenarios, como la lectura o al mostrar texto en idiomas distintos del inglés.

Esta es la lista de fuentes que se garantiza que estarán disponibles en todas las ediciones de Windows 10 que admitan aplicaciones para UWP.

**Nota**  Si usas una fuente que no está en esta lista, es posible que la aplicación descargue automáticamente los datos de fuente de un servicio Microsoft. Esto puede tener influencia en el rendimiento y en otros aspectos que pueden ser un problema, especialmente para dispositivos móviles. En particular, ten en cuenta que esto podría consumir parte del plan de datos móviles de un usuario o provocar la aplicación de costos de uso de datos móviles. Las aplicaciones para UWP que estarán disponibles en los dispositivos móviles nunca deben usar fuentes para el contenido de la interfaz de usuario que no aparezcan en esta lista.

 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Familia de tipos de letra</th>
<th align="left">Estilos</th>
<th align="left">Comentario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita, gruesa</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">Normal, cursiva, negrita, negrita cursiva, delgada, cursiva delgada</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comic Sans MS</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">Normal, negrita</td>
<td align="left">Fuente de la interfaz de usuario de escrituras africanas (etíope, N'Ko, Osmanya, Tifinagh, Vai)</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">Regular</td>
<td align="left">Fuente de la interfaz de usuario de escrituras norteamericanas (Silábica canadiense, Cherokee)</td>
</tr>
<tr class="odd">
<td align="left">Georgia</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Texto javanés Normal Fuente de reserva de escritura javanesa</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para script javanés</td>
</tr>
<tr class="odd">
<td align="left">Leelawadee UI</td>
<td align="left">Normal, Semilight, negrita</td>
<td align="left">Fuente de la interfaz de usuario de escrituras del sureste asiático (buginesa, Lao, Khmer, tailandés)</td>
</tr>
<tr class="even">
<td align="left">Lucida Console</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">Fuente de la interfaz de usuario para coreano</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para escritura tibetana</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">Regular</td>
<td align="left">Fuente de la interfaz de usuario para chino tradicional</td>
</tr>
<tr class="odd">
<td align="left">Microsoft New Tai Lue</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para la escritura New Tai Lue</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para la escritura Phags-pa</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para la escritura tailandesa</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">Regular</td>
<td align="left">Fuente de la interfaz de usuario para chino simplificado</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para escritura yi</td>
</tr>
<tr class="odd">
<td align="left">Mongolian Baiti</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para escritura mongola</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para escritura Thaana</td>
</tr>
<tr class="odd">
<td align="left">Texto en birmano</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva escritura birmana</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">Normal, Semilight, negrita</td>
<td align="left">Fuente de la interfaz de usuario para scripts del sur asiático (bengalí, devanagari, gujarati, gurmukhi, kannada, malayalam, odia, ol chiki, cingalés, sora sompeng, tamil, telugu)</td>
</tr>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Regular</td>
<td align="left">Fuente de la interfaz de usuario para los iconos de aplicación</td>
</tr>
<tr class="even">
<td align="left">Segoe Print</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita, delgada, semidelgada, seminegrita, gruesa</td>
<td align="left">Fuente de la interfaz de usuario de escrituras europeas y de Oriente Medio (árabe, armenio, cirílico, georgiano, griego, hebreo, latino) y también escritura Lisu</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Regular</td>
<td align="left">La versión que se incluye en Windows Phone incluye un contorno blanco alrededor de cada emoji para garantizar que se mostrará en cualquier fondo de color. Es métricamente compatible con la versión que se incluye en Windows.</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI Historic</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva para escrituras históricas</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fuente de reserva de símbolos</td>
</tr>
<tr class="odd">
<td align="left">SimSun</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Verdana</td>
<td align="left">Normal, cursiva, negrita, cursiva negrita</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">Medio</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Yu Gothic UI</td>
<td align="left">Normal</td>
<td align="left">Fuente de la interfaz de usuario para japonés</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Temas relacionados

**Para diseñadores**
* [Etiqueta (o bloque de texto)](labels.md)
* [Iconos de Segoe UI Symbol](segoe-ui-symbol-font.md)
            
          
            **Para desarrolladores (XAML)**
* [Recursos de temas en XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [Diseñar una página de la aplicación](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Iconos de Segoe UI Symbol](segoe-ui-symbol-font.md)
* [**Propiedad TextBlock.FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)

**Muestras**
* [XAML text display sample (Muestra de visualización de texto XAML)](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [CSS stying: branding your app sample (Estilo CSS: personalización de marca de la muestra de la aplicación)](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [Muestra de asignación de fuentes por idioma](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 






<!--HONumber=May16_HO2-->



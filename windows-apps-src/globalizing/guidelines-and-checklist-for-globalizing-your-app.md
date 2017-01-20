---
author: DelfCo
Description: "Sigue estos procedimientos recomendados cuando globalices las aplicaciones para un público más amplio o cuando localices las aplicaciones para un mercado específico."
Search.Refinement.TopicID: 180
title: "Directrices para globalización y localización"
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 72849c304d2150fd7fe6768181a504f94ef98d5f

---

# <a name="globalization-and-localization-dos-and-donts"></a>Globalización y localización: qué hacer y qué no hacer
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Sigue estos procedimientos recomendados cuando globalices las aplicaciones para un público más amplio o cuando localices las aplicaciones para un mercado específico.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Globalización**](https://msdn.microsoft.com/library/windows/apps/br206813)</li>
<li>[**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)</li>
<li>[**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**Recursos**](https://msdn.microsoft.com/library/windows/apps/br206022)</li>
<li>[**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)</li>
</ul>
</div>



## <a name="globalization"></a>Globalización

Prepara la aplicación para que se adapte fácilmente a los diversos mercados eligiendo términos e imágenes adecuados de forma global para la interfaz de usuario, mediante las API de [**globalización**](https://msdn.microsoft.com/library/windows/apps/br206813) para dar formato a los datos de la aplicación y evitar presunciones basadas en la ubicación o el idioma.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Recomendación</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Usa los formatos correctos para números, fechas, horas, direcciones y números de teléfono.</p></td>
<td align="left"><p>El formato usado para números, fechas, horas y demás formas de datos varía de una cultura, una región, un idioma y un mercado a otro. Si estás mostrando números, fechas, horas u otros datos, usa las API de [<strong>Globalization</strong>](https://msdn.microsoft.com/library/windows/apps/br206813) para obtener el formato adecuado para un público determinado.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Admite tamaños de papel internacionales.</p></td>
<td align="left"><p>El tamaño de papel más común difiere de un país a otro. Por ello, si incluyes funciones que dependen del tamaño de papel, como la impresión, asegúrate de admitir y probar tamaños internacionales comunes.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Admite unidades de medida y monedas internacionales.</p></td>
<td align="left"><p>Se usan diferentes unidades y escalas en diferentes países, aunque las más populares son el sistema métrico y el sistema imperial. Si trabajas con medidas, como longitud, temperatura o área, obtén el sistema de medida correcto con la propiedad [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Muestra el texto y las fuentes de forma correcta.</p></td>
<td align="left"><p>La dirección del texto, el tamaño de fuente y la fuente ideales varían de un mercado a otro.</p>
<p>Para más información, consulta [<strong>Ajustar el diseño y las fuentes y admitir RTL</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Usa Unicode para la codificación de caracteres.</p></td>
<td align="left"><p>De manera predeterminada, la versiones recientes de Microsoft Visual Studio usan la codificación de caracteres Unicode para todos los documentos. Si estás usando un editor diferente, asegúrate de guardar los archivos de origen con las codificaciones de caracteres Unicode apropiadas. Todas las API de Windows en tiempo de ejecución ejecutan cadenas codificadas mediante UTF-16.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Registra el idioma de entrada.</p></td>
<td align="left"><p>Cuando tu aplicación pide a los usuarios entrada de texto, registra el idioma de la entrada. Esto garantiza que cuando se muestre la entrada más adelante, se presente al usuario con el formato apropiado. Usa la propiedad [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658) para obtener el idioma de entrada actual.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>No te guíes por el idioma para deducir la ubicación de un usuario y no uses la ubicación para deducir su idioma.</p></td>
<td align="left"><p>En Windows, la ubicación y el idioma del usuario son conceptos diferentes. Un usuario puede hablar una variedad regional de un idioma en particular como en-GB para el inglés de Gran Bretaña, pero puede estar en una región o país totalmente diferente. Considera si las aplicaciones necesitan obtener información sobre idioma del usuario, por ejemplo para el texto de la interfaz de usuario, o sobre su ubicación, por ejemplo para los problemas de concesión de licencias.</p>
<p>Para más información, consulta [<strong>Administrar idiomas y regiones</strong>](manage-language-and-region.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>No uses coloquialismos ni metáforas.</p></td>
<td align="left"><p>El lenguaje que es específico de un grupo demográfico, como una referencia cultural o una edad, puede ser difícil de entender o traducir porque solamente lo usan las personas de ese grupo demográfico. Del mismo modo, las metáforas pueden tener sentido para una persona, pero no significar nada para otra. Por ejemplo &quot;rizo&quot; significa algo para quienes pertenecen al ámbito de la aviación, pero aquellos que no pertenecen a este ámbito no comprenden la referencia. Si planeas localizar tu aplicación y usas un tono de voz informal, asegúrate de explicar correctamente a los localizadores el significado y el tono en el que debe traducirse.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>No uses una jerga técnica, abreviaturas ni acrónimos.</p></td>
<td align="left"><p>Es muy poco probable que aquellas personas que no pertenecen al ámbito técnico o que provienen de otras culturas o regiones comprendan el lenguaje técnico, como también es difícil que puedan traducirlo. En las conversaciones de la vida diaria no se usan este tipo de palabras. El lenguaje técnico generalmente aparece en los mensajes de error para identificar problemas de software y hardware. En ocasiones, esto podría ser necesario, pero deberías volver a escribir las cadenas para que no sean técnicas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>No uses imágenes que puedan considerarse ofensivas.</p></td>
<td align="left"><p>Las imágenes que probablemente sean apropiadas en tu cultura, tal vez se consideren ofensivas o no se interpreten correctamente en otras. Evita el uso de combinaciones de colores, animales y símbolos religiosos que estén asociados con movimientos políticos o banderas de naciones.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Evita agresiones políticas en mapas o cuando hagas referencia a regiones.</p></td>
<td align="left"><p>Los mapas pueden incluir fronteras nacionales e internacionales controvertidas, lo que frecuentemente es un origen de ofensa política. Ten cuidado de hacer que cualquier UI usada para seleccionar una nación haga referencia a ella como &quot;país o región&quot;. Si colocas un territorio en disputa en una lista llamada &quot;Países&quot;, como en un formulario de direcciones, puede generarte problemas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>No uses la comparación de cadenas por sí sola para comparar etiquetas de idioma.</p></td>
<td align="left"><p>Las etiquetas de idioma BCP-47 son complejas. Existen algunos problemas al comparar etiquetas de idioma, como problemas con la coincidencia de información del script, etiquetas heredadas y múltiples variedades regionales. El sistema de administración de recursos de Windows se encarga de las coincidencias por ti. Puedes especificar un conjunto de recursos en cualquier idioma, y el sistema elige el apropiado para el usuario y la aplicación.</p>
<p>Para conocer más sobre la administración de recursos, consulta [<strong>Definir recursos de aplicación</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>No des por hecho que el orden siempre sea alfabético.</p></td>
<td align="left"><p>Para los idiomas que no usan un alfabeto latino, la ordenación se basa en factores como la pronunciación, el número de trazos y otros. Incluso los idiomas que usan el alfabeto latino no siempre usan la ordenación alfabética. Por ejemplo, en algunas culturas, una libreta de teléfonos probablemente no se ordene alfabéticamente. El sistema puede controlar la ordenación por ti, pero si creas tu propio algoritmo de ordenación, asegúrate de tener en cuenta los métodos de ordenación usados en las regiones de comercialización de destino.</p></td>
</tr>
</tbody>
</table>

 

## <a name="localization"></a>Localización

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Recomendación</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Separa los recursos como imágenes y cadenas de UI del código.</p></td>
<td align="left"><p>Diseña tus aplicaciones para que los recursos, como las cadenas y las imágenes, estén separados del código. Esto permite que se mantengan y localicen de forma independiente, como también se personalicen para diferentes factores de ajuste de escala, opciones de accesibilidad y un gran número de otros contextos de máquina y usuario.</p>
<p>Separa los recursos de cadena del código de tu aplicación para crear un único código base independiente del idioma. Separa siempre las cadenas del código de aplicación y de marcado, y colócalas en un archivo de recursos, como un archivo ResW o ResJSON.</p>
<p>Usa la infraestructura de recursos de Windows para controlar la selección de los recursos más apropiados que mejor se adapten al entorno en tiempo de ejecución del usuario.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Aísla otros archivos de recursos localizables.</p></td>
<td align="left"><p>Identifica otros archivos que requieran localización, como imágenes que contengan texto que se debe traducir o que deban cambiarse debido a sensibilidades culturales, y colócalos en carpetas etiquetadas con nombres de idioma.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Establece tu idioma predeterminado y marca todos tus recursos, incluso aquellos de tu idioma predeterminado.</p></td>
<td align="left"><p>Establece siempre el idioma predeterminado de las aplicaciones de manera correcta en el manifiesto de la aplicación (package.appxmanifest). El idioma predeterminado determina el idioma que se usa cuando el usuario no habla ninguno de los idiomas admitidos de la aplicación. Marca los recursos de idioma predeterminados (por ejemplo, en-us/Logo.png) con su idioma para que el sistema pueda determinar en qué idioma está el recurso y cómo se usa en situaciones particulares.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Determina los recursos de tu aplicación que requieren localización.</p></td>
<td align="left"><p>¿Qué tiene que cambiarse si tu aplicación debe localizarse para otras regiones de comercialización? Las cadenas de texto requieren de traducción a otros idiomas. Las imágenes probablemente deban adaptarse para otras culturas. Considera cómo la localización afecta otros recursos que usa tu aplicación, como audio o vídeo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Usa identificadores de recursos en el código y marcado para hacer referencia a los recursos.</p></td>
<td align="left"><p>En lugar de tener literales de cadena o nombres de archivo específicos en tu marcado, usa referencias a los recursos. Asegúrate de usar identificadores únicos para cada recurso. Para obtener más información, consulta [<strong>Cómo asignar nombre a los recursos mediante calificadores</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324).</p>
<p>Presta atención a los eventos que se desencadenan cuando el sistema cambia y empieza a usar un conjunto distinto de calificadores. Vuelve a procesar el documento para que se puedan cargar los recursos correctos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Permite que el tamaño de texto aumente.</p></td>
<td align="left"><p>Asigna búferes de texto de forma dinámica, ya que el tamaño del texto puede aumentar cuando se traduce. Si debes usar búferes estáticos, haz que sean extra grandes (tal vez, duplicando la longitud del alfabeto inglés) para dar lugar a una posible expansión cuando se traduzcan las cadenas. Probablemente también se disponga de espacio limitado para una interfaz de usuario. Para dar lugar a los idiomas localizados, asegúrate de que la longitud de tu cadena sea aproximadamente un 40% más larga que lo que necesitarás para el idioma inglés. Para las cadenas que son realmente cortas, como las de una sola palabra, probablemente necesites un 300% más de espacio. Además, si se permite una compatibilidad multilínea y ajuste de texto en un control, se proporcionará más espacio para mostrar cada cadena.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Admite la creación de reflejos.</p></td>
<td align="left"><p>La alineación de texto y el orden de lectura pueden ser de izquierda a derecha, como en inglés, o de derecha a izquierda, como en árabe o hebreo. Si estás localizando tu producto a idiomas que usan un orden de lectura diferente del tuyo, asegúrate de que el diseño de los elementos de tu UI admita la creación de reflejos. Es posible que incluso los elementos como los botones de retroceso, los efectos de transición de interfaz de usuario y las imágenes deban reflejarse.</p>
<p>Para más información, consulta [<strong>Ajustar el diseño y las fuentes y admitir RTL</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Comenta las cadenas.</p></td>
<td align="left"><p>Asegúrate de que las cadenas estén correctamente comentadas y de que solo las cadenas que deben traducirse se entreguen a los localizadores. La sobrelocalización es una causa común de problemas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Usa cadenas cortas.</p></td>
<td align="left"><p>Las cadenas más cortas son más fáciles de traducir y permiten reciclar la traducción. El reciclaje de traducción ahorra dinero porque la misma cadena no se envía al localizador dos veces.</p>
<p>Probablemente, algunas herramientas de localización no admitan cadenas que tengan más de 8192 caracteres. Por ello, mantén la longitud de cadena en 4000 caracteres o menos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Proporciona cadenas que contengan una oración completa.</p></td>
<td align="left"><p>Proporciona cadenas que contengan una oración completa en lugar de cortar la oración en palabras individuales, porque la traducción de palabras puede depender de su posición en una oración. Además, no asumas que una frase con múltiples parámetros conservará esos parámetros en el mismo orden para todos los idiomas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Optimiza los archivos de imagen y de audio para la localización.</p></td>
<td align="left"><p>Reduce los costos de localización evitando el uso de texto en imágenes o de fragmentos hablados en archivos de audio. Si localizas a un idioma que tiene una dirección de lectura diferente de la del tuyo, el uso de efectos e imágenes simétricas facilita la admisión de la creación de reflejos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>No vuelvas a usar cadenas en contextos diferentes.</p></td>
<td align="left"><p>No vuelvas a usar cadenas en contextos diferentes, porque hasta las palabras más simples, como &quot;on&quot; y &quot;off&quot; pueden traducirse de manera diferente según el contexto.</p></td>
</tr>
</tbody>
</table>

 

## <a name="related-articles"></a>Artículos relacionados


**Ejemplos**
* [Ejemplo de recursos de aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [Ejemplo de preferencias de globalización](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 






<!--HONumber=Dec16_HO2-->



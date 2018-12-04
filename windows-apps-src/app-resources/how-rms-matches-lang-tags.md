---
Description: The previous topic (How the Resource Management System matches and chooses resources) looks at qualifier-matching in general. This topic focuses on language-tag-matching in more detail.
title: Cómo el sistema de administración de recursos compara etiquetas de idioma
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 4914a448432206e2418fe110c0b49517a7145e0b
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469424"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Cómo el sistema de administración de recursos compara etiquetas de idioma

El tema anterior (Cómo compara y elige recursos el sistema de administración de recursos[](how-rms-matches-and-chooses-resources.md)) se ocupa de la coincidencia de calificadores en general. Este tema se centra en la coincidencia de etiqueta de idiomas con más detalle.

## <a name="introduction"></a>Introducción

Los recursos con calificadores de etiqueta de idioma se comparan y califican según la lista de idiomas del tiempo de ejecución de la aplicación. Para ver definiciones de las listas de idiomas diferentes, consulta [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](../design/globalizing/manage-language-and-region.md). La coincidencia con el primer idioma de la lista se produce antes de la coincidencia con el segundo, incluso para otras variantes regionales. Por ejemplo, se prefiere un recurso para en-GB sobre fr-CA si el idioma del tiempo de ejecución de la aplicación es en-US. Solo si no hay recursos para un formato de en, se elige un recurso para fr-CA (ten en cuenta que, en ese caso, no se pudo establecer el idioma predeterminado de la aplicación en ningún formato de en).

El mecanismo de puntuación usa los datos incluidos en el registro de subetiquetas de [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302), así como otros orígenes de datos. Permite establecer un gradiente de puntuación con distintas calidades de coincidencia y, cuando hay varios candidatos disponibles, selecciona el candidato con la mejor puntuación de coincidencia.

Por ello, puedes etiquetar el contenido de idioma en términos genéricos, pero sigues pudiendo especificar contenido específico cuando sea necesario. Por ejemplo, tu aplicación puede tener muchas cadenas en inglés comunes para las versiones de Estados Unidos, Gran Bretaña y de otras regiones. El etiquetado de estas cadenas simplemente como "en" (inglés) ahorra espacio y reduce la carga de localización. Cuando se deban realizar distinciones, como en una cadena que contenga la palabra "color/colour", las versiones de Estados Unidos y de Gran Bretaña se pueden etiquetar por separado usando las subetiquetas de idioma y región, como "en-US" y "en-GB", respectivamente.

## <a name="language-tags"></a>Etiquetas de idioma

Los idiomas se identifican con etiquetas de idioma BCP-47, bien formadas y normalizadas. Los componentes de subetiquetas se definen en el registro de subetiquetas de BCP-47. La estructura normal de una etiqueta de idioma BCP-47 consiste en uno o más de los siguientes elementos de subetiqueta.

- Subetiqueta de idioma (obligatoria).
- Subetiqueta de script (que puede inferirse con el valor predeterminado especificado en el registro de subetiquetas).
- Subetiqueta de región (opcional).
- Subetiqueta de variante (opcional).

Pueden estar presentes elementos de subetiquetas adicionales, pero tendrán un efecto insignificante en la coincidencia de idiomas. No hay gamas de idiomas definidas con el carácter comodín ("*"), por ejemplo, "en-*".

## <a name="matching-two-languages"></a>Coincidencia de dos idiomas

Cuando en Windows se comparan dos idiomas, generalmente se realiza dentro del contexto de un proceso más grande. Puede tratarse de un contexto donde se evalúan varios idiomas, como cuando Windows genera la lista de idiomas de la aplicación (consulta [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](../design/globalizing/manage-language-and-region.md)). Windows lleva esto a cabo estableciendo coincidencias de varios idiomas entre las preferencias del usuario con los idiomas especificados en el manifiesto de la aplicación. La comparación también podría producirse en el contexto de evaluación de un idioma junto con otros calificadores de un recurso en particular. Un ejemplo de ello es cuando Windows resuelve un recurso de archivo concreto en un contexto de recurso concreto, con la ubicación principal del usuario o la escala actual del dispositivo o ppp como factores adicionales (además del idioma) que se tienen en cuenta en la selección de recurso.

Cuando dos etiquetas de idioma se comparan, se asigna una puntuación a la comparación en función de la cercanía de la coincidencia.

| Coincidencia | Puntuación | Ejemplo |
| ----- | ----- | ------- |
| Coincidencia exacta | Más alta | en-AU : en-AU |
| Coincidencia de variante (idioma, script, región, variante) |  | en-AU-variant1 : en-AU-variant1-t-ja |
| Coincidencia de región (idioma, script, región) |  | en-AU : en-AU-variante1 |
| Coincidencia parcial (idioma, script) |  |  |
| - Coincidencia de macrorregión |  | en-AU : en-053 |
| - Coincidencia independiente de la región |  | en-AU : en |
| - Coincidencia de afinidad ortográfica (compatibilidad limitada) |  | en-AU : en-GB |
| - Coincidencia de región preferida |  | en-AU : en-US |
| - Cualquier coincidencia de región |  | en-AU : en-CA |
| Idioma no determinado (cualquier coincidencia de idioma) |  | en-AU : und |
| Sin coincidencia (sin coincidencia de script o sin coincidencia de etiqueta de idioma principal) | Más baja | en-AU : fr-FR |

### <a name="exact-match"></a>Coincidencia exacta

Las etiquetas son exactamente iguales (coinciden todos los elementos de subetiquetas). Una comparación puede promoverse a este tipo de coincidencia desde una coincidencia de variante o de región. Por ejemplo, en-US coincide con en-US.

### <a name="variant-match"></a>Coincidencia de variante

Las etiquetas coinciden en las subetiquetas de idioma, de script, de región y de variante, pero difieren en algún otro sentido.

### <a name="region-match"></a>Coincidencia de región

Las etiquetas coinciden en las subetiquetas de idioma, de script, de región y de variante, pero difieren en algún otro sentido. Por ejemplo, de-DE-1996 coincide con-DE y en-US-x-Pirate coincide con en-US.

### <a name="partial-matches"></a>Coincidencias parciales

Las etiquetas coinciden en las subetiquetas de idioma y de script, pero difieren en la región o en alguna otra subetiqueta. Por ejemplo, en-US coincide con en o en-US coincide con en-\*.

#### <a name="macro-region-match"></a>Coincidencia de macrorregión

Las etiquetas coinciden en las subetiquetas de idioma y script; ambas etiquetas tienen subetiquetas de región, siendo una de ellas una macrorregión que engloba a la otra región. Las etiquetas de macrorregión son siempre numéricas y se derivan de los códigos de país y zona M.49 de la División de Estadística de las Naciones Unidas. Para obtener más información sobre las relaciones englobadoras, consulta [Composición de regiones macrogeográficas (continentales), subregiones geográficas, grupos económicos seleccionados y otras agrupaciones](http://go.microsoft.com/fwlink/p/?LinkId=247929).

**Nota** Los códigos de Naciones Unidas para "grupos económicos" u "otras agrupaciones" no se admiten en BCP-47.
 
**Nota** Una etiqueta con la subetiqueta de macrorregión "001" se considera equivalente a una etiqueta independiente de la región. Por ejemplo, "es-001" y "es" se consideran sinónimas.

#### <a name="region-neutral-match"></a>Coincidencia independiente de la región

Las etiquetas coinciden en las subetiquetas de idioma y de script, y solo una tiene una etiqueta de región. Se prefiere una coincidencia principal a otras coincidencias parciales.

#### <a name="orthographic-affinity-match"></a>Coincidencia de afinidad ortográfica

Las etiquetas coinciden en las subetiquetas de idioma y de script, y las subetiquetas de región tienen afinidad ortográfica. La afinidad depende de los datos mantenidos en Windows que definen las regiones con afinidad específica del idioma, por ejemplo, "en-IE" y "en-GB".

#### <a name="preferred-region-match"></a>Coincidencia de región preferida

Las etiquetas coinciden en las subetiquetas de idioma y de script, y una de las subetiquetas de región es la subetiqueta de región predeterminada del idioma. Por ejemplo, "fr-FR" es la región predeterminada de la subetiqueta "fr". Por tanto, fr-FR es una coincidencia mejor para fr-BE que fr-CA. Esto depende de los datos mantenidos en Windows que definen una región predeterminada para cada idioma en el que se localice Windows.

#### <a name="sibling-match"></a>Coincidencia de elementos del mismo nivel

Las etiquetas coinciden en las subetiquetas de idioma y de script, y ambas tienen subetiquetas de región, pero no hay ninguna otra relación definida entre ellas. En el caso de varias coincidencias de elementos del mismo nivel, el último elemento del mismo nivel enumerado será el prioritario, en ausencia de una coincidencia superior.

### <a name="undetermined-language"></a>Idioma sin determinar

Un recurso puede etiquetarse como "und" para indicar que coincide con cualquier idioma. Esta etiqueta también puede usarse con una etiqueta de script para filtrar coincidencias por script. Por ejemplo, "und-Latn" coincidirá con cualquier etiqueta de idioma que use escritura latina. Consulta más adelante para más información.

### <a name="script-mismatch"></a>Sin coincidencia de script

Cuando las etiquetas coinciden solo en la etiqueta de idioma principal pero no en el script, se considera que el par no coincide y la puntuación se establece por debajo del nivel de una coincidencia válida.

### <a name="no-match"></a>Sin coincidencia

La puntuación de las subetiquetas de idioma principal que no coinciden se establece por debajo del nivel de una coincidencia válida. For example, zh-Hant no coincide con zh-Hans.

## <a name="examples"></a>Ejemplos

El idioma "zh-Hans-CN" (Chino simplificado de China) de un usuario coincidiría con los siguientes recursos en el orden de prioridad indicado. Una X indica sin coincidencia.

![Coincidencia con chino simplificado de China](images/language_matching_1.png)

1. Coincidencia exacta; 2. y 3. Coincidencia de región; 4. Coincidencia principal; 5. Coincidencia de elementos del mismo nivel.

Cuando una subetiqueta de idioma tiene un valor de script de supresión definido en el registro de subetiquetas de BCP-47, se produce la coincidencia correspondiente, tomando el valor del código de script de supresión. Por ejemplo, en-Latn-US coincide con en-US. En el siguiente ejemplo, el idioma del usuario es "en-AU" (inglés de Australia).

![Coincidencia con inglés de Australia](images/language_matching_2.png)

1. Coincidencia exacta; 2. Coincidencia de macrorregión; 3. Coincidencia independiente de la región; 4. Coincidencia de afinidad ortográfica; 5. Coincidencia de región preferida; 6. Coincidencia de elementos del mismo nivel.

## <a name="matching-a-language-to-a-language-list"></a>Coincidencia de un idioma con la lista de idiomas

En ocasiones, la coincidencia se produce como parte de un proceso más grande de coincidencia de un solo idioma con una lista de idiomas; por ejemplo, puede que surja una coincidencia entre un recurso basado en un solo idioma y la lista de idiomas de una aplicación. La puntuación de la coincidencia se mide conforme a la posición del primer idioma que coincide en la lista. Cuanto más abajo está el idioma en la lista, más baja será la puntuación.

Cuando la lista de idiomas contiene dos o más variantes regionales con las mismas etiquetas de idioma y de script, la puntuación de las comparaciones de la primera etiqueta de idioma solo se establece para las coincidencias exactas, de variante y de región. La puntuación de las coincidencias parciales se pospone para la última variante regional. Esto permite a los usuarios controlar con precisión el comportamiento de coincidencia de su lista de idiomas El comportamiento de las coincidencias puede incluir que se prefiera una coincidencia exacta para un elemento secundario de la lista antes que una coincidencia parcial para el primer elemento de la lista, de haber un tercer elemento que coincida con el idioma y el script del primero. Aquí tienes un ejemplo.

- Lista de idiomas (en orden): "pt-PT" (portugués de Portugal), "en-US" (inglés de Estados Unidos), pt-BR (portugués de Brasil).
- Recursos: "en-US", "pt-BR".
- Recurso con la puntuación más alta: "en-US".
- Descripción: la comparación comienza con "pt-PT", pero no encuentra una coincidencia exacta. Debido a la presencia de "pt-BR" en la lista de idiomas del usuario, la coincidencia parcial se pospone hasta la comparación con "pt-BR". La siguiente comparación de idiomas es "en-US", que tiene coincidencia exacta. De esta manera, el recurso prioritario es "en-US".

O

- Lista de idiomas (en orden): "es-MX" (español de México), "es-HO" (español de Honduras).
- Recursos: "en-ES", "es-HO".
- Recurso con la puntuación más alta: "es-HO".

## <a name="undetermined-language-und"></a>Idioma sin determinar ("und")

La etiqueta de idioma "und" puede usarse para especificar un recurso que coincidirá con cualquier idioma en ausencia de una mejor coincidencia. Puede considerarse similar a la gama de idiomas de BCP-47 "*" or "*-&lt;script&gt;". Aquí tienes un ejemplo.

- Lista de idiomas: "en-US", "zh-Hans-CN".
- Recursos: "zh-Hans-CN", "und".
- Recurso con la puntuación más alta: "und".
- Descripción: La comparación comienza con "en-US" pero no encuentra una coincidencia basada en "en" (parcial o mejor). Dado que hay un recurso etiquetado con "und", lo usará el algoritmo de coincidencia.

La etiqueta "und" permite que varios idiomas compartan un único recurso y que los idiomas individuales se traten como excepciones. Por ejemplo.

- Lista de idiomas: "zh-Hans-CN", "en-US".
- Recursos: "zh-Hans-CN", "und".
- Recurso con la puntuación más alta: "zh-Hans-CN".
- Descripción: La comparación encuentra una coincidencia exacta para el primer elemento y, por lo tanto, no comprueba el recurso etiquetado como "und".

Puedes usar "und" con una etiqueta de script para filtrar recursos por script. Por ejemplo.

- Lista de idiomas: "ru".
- Recursos: "und-Latn", "und-Cyrl", "und-Arab".
- Recurso con la puntuación más alta: "und-Cyrl".
- Descripción: la comparación no encuentra una coincidencia para "ru" (parcial o mejor) y, por lo tanto, coincide con la etiqueta de idioma "und". El valor de script de supresión "Cyrl" asociado con la etiqueta de idioma "ru" coincide con el recurso "und-Cyrl".

## <a name="orthographic-regional-affinity"></a>Afinidad ortográfica regional

Cuando dos etiquetas de idioma con diferencias de subetiqueta de región se comparan, determinados pares de regiones pueden tener una mayor afinidad entre sí que otros. Los únicos grupos de afinidad admitidos son para el idioma inglés ("en"). Las subetiquetas de región "PH" (Filipinas) y "LR" (Liberia) tienen afinidad ortográfica con la subetiqueta de región "US". Todas las otras subetiquetas de región tienen afinidad con la subetiqueta de región "GB" (Reino Unido). Por lo tanto, cuando ambos recursos ("en-US" y "en-GB") están disponibles, una lista de idiomas de "en-HK" (inglés de la RAE de Hong Kong) obtendrá una puntuación más alta con recursos de "en-GB" que con recursos de "en-US".

## <a name="handling-languages-with-many-regional-variants"></a>Manejo de idiomas con muchas variantes regionales

Determinados idiomas tienen amplias comunidades de hablantes en distintas regiones que usan variedades diferentes de dicho idioma: lenguajes como el inglés, el francés y el español, que están entre los más compatibles con aplicaciones multilingües. Las diferencias regionales pueden incluir diferencias ortográficas (por ejemplo, "color" frente a "colour") o diferencias dialécticas, como el vocabulario (por ejemplo, "truck" frente a "lorry").

Estos idiomas con variantes regionales significativas presentan algunos desafíos durante la creación de una aplicación internacional: "¿Cuántas variantes regionales distintas deberían admitirse?" "¿Cuáles?" "¿Cuál es la forma más rentable de administrar estos activos de variantes regionales para mi aplicación?" Responder a estas cuestiones va más allá del ámbito de este tema. Sin embargo, los mecanismos de coincidencia de idiomas de Windows sí que proporcionan capacidades que te ayudan en el manejo de las variantes regionales.

Las aplicaciones suelen admitir una única variedad de un determinado idioma. Supongamos que una aplicación tiene recursos para una sola variedad de inglés que se supone que usarán los anglófonos, sea cual sea su región de procedencia. En este caso, la etiqueta "en" sin subetiqueta de región, reflejaría tales expectativas. Pero puede ser que en el pasado las aplicaciones hayan usado una etiqueta como "en-US", que incluye una subetiqueta de región. En este caso, lo anterior también funcionará, ya que la aplicación usa una sola variedad de inglés y Windows controla de manera adecuada la coincidencia entre un recurso con una etiqueta de variante regional y una preferencia de idioma del usuario para una variante regional distinta.

Sin embargo, si se van a admitir dos o más variedades regionales, la diferencia que supone "en" frente a "en-US" puede afectar significativamente a la experiencia del usuario y resulta necesario plantearse qué subetiquetas de región se van a usar.

Supongamos que quieres ofrecer productos localizados en francés independientes para los usuarios de francés canadiense y los usuarios de francés europeo. Para el francés canadiense, se puede usar "fr-CA". Para los hablantes de Europa, la localización usará francés (Francia), por lo que se puede usar "fr-FR". Pero, ¿qué ocurre con los usuarios procedentes de Bélgica cuya preferencia de idioma es "fr-BE"?; ¿qué es lo que obtendrán? La región "BE" es distinta de "FR" y "CA", lo que sugiere una coincidencia de "cualquier región" para ambas. Sin embargo, Francia es la región preferida para el idioma francés, de modo que se considerará que "fr-FR" es la mejor coincidencia en este caso.

Supongamos que primero localizaste tu aplicación para una sola variante de francés y usaste cadenas de francés (Francia) pero las calificaste de manera genérica como "fr", y ahora quieres agregar compatibilidad con francés canadiense. Es probable que solo deban volver a traducirse algunos recursos para francés canadiense. Puedes seguir usando todos los activos originales, manteniéndolo calificados "fr" y simplemente agregar el pequeño conjunto de activos nuevos, usando "fr-CA". Si la preferencia de idioma del usuario es "fr-CA", entonces el activo "fr-CA" tendrá una mayor puntuación de coincidencia que el activo "fr". Pero si la preferencia de idioma del usuario es para cualquier variedad de francés, el activo independiente de la región "fr" será una mejor coincidencia que el activo "fr-CA".

Veamos otro ejemplo: supongamos que quieres proporcionar localizaciones en español independientes para hablantes de España y hablantes de América Latina. Supongamos además que las traducciones para América Latina las proporcionó un proveedor de México. ¿Deberías usar "es-ES" (España) y "es-MX" (México) para dos conjuntos de recursos? Si lo hicieras, podría resultar problemático para los hablantes de otras regiones de América Latina, como Argentina o Colombia, ya que obtendrían los recursos "es-ES". En este caso, hay una alternativa mejor: puedes usar una subetiqueta de macrorregión, "es-419", para reflejar que destinas los activos para su uso por parte de los hablantes de cualquier parte de América Latina o el Caribe.

Las etiquetas de idioma independientes de la región y las subetiquetas de macrorregión pueden ser muy eficaces si quieres admitir diversas variedades regionales. Para minimizar el número de activos separados necesarios, puedes calificar un determinado activo de modo que tenga la mayor cobertura de aplicación posible. Luego puedes complementar un activo de aplicación extensa con una variante más específica si es necesario. Un activo con un calificador de idioma independiente de la región se utilizará para usuarios de cualquier variedad regional, a menos que haya otro activo con un calificador de región más específico aplicable a ese usuario. Por ejemplo, un activo "en" coincidirá para un usuario de inglés australiano, pero un activo con "en-053" (inglés usado en Australia o Nueva Zelanda) será una coincidencia mejor para ese usuario, mientras que un activo con "en-AU" será la mejor coincidencia posible.

El inglés es un caso aparte. Si una aplicación agrega localización para dos variedades de inglés, probablemente serán el inglés de Estados Unidos y el del Reino Unido, o el inglés "internacional". Como se indicó anteriormente, determinadas regiones de fuera de Estados Unidos siguen las convenciones ortográficas de Estados Unidos y la coincidencia de idiomas de Windows lo tiene en cuenta. En este escenario, no se recomienda usar la etiqueta independiente de la región "en" para una de las variantes, sino utilizar "en-GB" y "en-US". (Sin embargo, si un determinado recurso no requiere variantes separadas, puede usarse "en"). Si se reemplaza "en-GB" o "en-US" por "en", eso interferirá en la afinidad regional ortográfica proporcionada por Windows. Si se agrega una tercera localización en inglés, se puede usar una subetiqueta de macrorregión o específica para las variantes adicionales (por ejemplo, "en-CA", "en-AU" o "en-053"), pero se debe seguir usando "en-GB" y "en-US".

## <a name="related-topics"></a>Temas relacionados

* [Cómo compara y elige recursos el sistema de administración](how-rms-matches-and-chooses-resources.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](../design/globalizing/manage-language-and-region.md)
* [Composición de regiones macrogeográficas (continentales), subregiones geográficas, grupos económicos seleccionados y otras agrupaciones.](http://go.microsoft.com/fwlink/p/?LinkId=247929)

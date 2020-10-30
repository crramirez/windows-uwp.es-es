---
description: En el tema anterior (Cómo compara y elige recursos el sistema de administración de recursos) se examina la coincidencia de calificadores en general. Este tema se centra con mayor detalle en la coincidencia de etiquetas de idioma.
title: Cómo el sistema de administración de recursos compara etiquetas de idioma
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 3c5b2d595585cabdfb9f2983f2ad87f05b7fa5a4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031888"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Cómo el sistema de administración de recursos compara etiquetas de idioma

En el tema anterior ([Cómo compara y elige recursos el sistema de administración de recursos](how-rms-matches-and-chooses-resources.md)) se examina la coincidencia de calificadores en general. Este tema se centra con mayor detalle en la coincidencia de etiquetas de idioma.

## <a name="introduction"></a>Introducción

Los recursos con calificadores de etiqueta de idioma se comparan y puntuan en función de la lista de idioma de tiempo de ejecución de la aplicación. Para ver las definiciones de las distintas listas de idiomas, consulte comprender los lenguajes de [Perfil de usuario y los lenguajes de manifiesto de aplicación](../design/globalizing/manage-language-and-region.md). La coincidencia con el primer idioma de la lista se produce antes de la coincidencia con el segundo, incluso para otras variantes regionales. Por ejemplo, se elige un recurso para en-GB en un recurso de FR-CA si el idioma de tiempo de ejecución de la aplicación es en-US. Solo si no hay recursos para un formulario de en, es un recurso para FR-CA elegido (tenga en cuenta que el idioma predeterminado de la aplicación no se pudo establecer en ninguna forma de en en ese caso).

El mecanismo de puntuación utiliza datos que se incluyen en el registro de subetiqueta [BCP-47](https://tools.ietf.org/html/bcp47) y otros orígenes de datos. Permite un degradado de puntuación con diferentes cualidades de coincidencia y, cuando hay varios candidatos disponibles, selecciona el candidato con la mejor puntuación de coincidencia.

Por lo tanto, puede etiquetar el contenido del lenguaje en términos genéricos, pero puede especificar contenido específico cuando sea necesario. Por ejemplo, la aplicación podría tener muchas cadenas en inglés que son comunes al Estados Unidos, la Bretaña y otras regiones. El etiquetado de estas cadenas como "en" (Inglés) ahorra espacio y sobrecarga de localización. Cuando es necesario realizar las distinciones, como en una cadena que contiene la palabra "color/color", el Estados Unidos y las versiones británicas se pueden etiquetar por separado mediante subetiquetas de idioma y región, como "en-US" y "en-GB", respectivamente.

## <a name="language-tags"></a>Etiquetas Language

Los idiomas se identifican mediante etiquetas de lenguaje BCP-47 normalizadas y con formato correcto. Los componentes de subetiqueta se definen en el registro de subetiqueta BCP-47. La estructura normal de una etiqueta de idioma BCP-47 se compone de uno o varios de los siguientes elementos de la subetiqueta.

- Subetiqueta del lenguaje (obligatorio).
- Subetiqueta de script (que se puede deducir mediante el valor predeterminado especificado en el registro de subetiqueta).
- Subetiqueta region (opcional).
- Subetiqueta Variant (opcional).

Pueden estar presentes elementos de subetiqueta adicionales, pero tendrán un efecto insignificante en la coincidencia de idioma. No hay ningún rango de idioma definido mediante el carácter comodín (" *"), por ejemplo, "en-* ".

## <a name="matching-two-languages"></a>Coincidencia con dos idiomas

Cada vez que Windows compara dos idiomas, normalmente se realiza en el contexto de un proceso más grande. Puede estar en el contexto de la evaluación de varios idiomas, como cuando Windows genera la lista de idiomas de la aplicación (consulte comprender los lenguajes [de Perfil de usuario y los lenguajes de manifiesto de aplicación](../design/globalizing/manage-language-and-region.md)). Para ello, Windows compara varios idiomas desde las preferencias del usuario con los idiomas especificados en el manifiesto de la aplicación. La comparación también puede estar en el contexto de la evaluación del idioma junto con otros calificadores para un recurso determinado. Un ejemplo es cuando Windows resuelve un recurso de archivo determinado a un contexto de recurso determinado. con la ubicación principal del usuario o la escala o el PPP actual del dispositivo como otros factores (además de idioma) que se factorizan en la selección de recursos.

Cuando se comparan dos etiquetas de idioma, se asigna una puntuación a la comparación en función de la proximidad de la coincidencia.

| Match | Score | Ejemplo |
| ----- | ----- | ------- |
| Coincidencia exacta | Highest (el más alto) | en-AU: en-AU |
| Coincidencia de variante (idioma, script, región, variante) |  | en-AU-variant1: en-AU-variant1-t-ja |
| Coincidencia de región (idioma, script, región) |  | en-AU: en-AU-variant1 |
| Coincidencia parcial (idioma, script) |  |  |
| -Coincidencia de región de macro |  | en-AU: en-053 |
| -La coincidencia independiente de la región |  | en-AU: en |
| -Coincidencia de afinidad ortográfica (compatibilidad limitada) |  | en-AU: en-GB |
| -Coincidencia de región preferida |  | en-AU: en-US |
| -Cualquier coincidencia de región |  | en-AU: en-CA |
| Idioma no determinado (cualquier coincidencia de idioma) |  | en-AU: und |
| No coincide (no coincide el script o no coincide con la etiqueta de idioma principal) | Mínima | en-AU: fr-FR |

### <a name="exact-match"></a>Coincidencia exacta

Las etiquetas son exactamente iguales (todos los elementos de la subetiqueta coinciden). Una comparación se puede promover a este tipo de coincidencia a partir de una coincidencia de variante o región. Por ejemplo, en-US coincide con en-US.

### <a name="variant-match"></a>Coincidencia de variante

Las etiquetas coinciden con las subetiquetas de idioma, script, región y variante, pero difieren en otro sentido.

### <a name="region-match"></a>Coincidencia de región

Las etiquetas coinciden con las subetiquetas de idioma, script y región, pero difieren en otro sentido. Por ejemplo, de-DE-1996 coincide con de-DE y en-US-x-Pirate coincide con en-US.

### <a name="partial-matches"></a>Coincidencias parciales

Las etiquetas coinciden con las subetiquetas de lenguaje y script, pero difieren en la región o en alguna otra subetiqueta. Por ejemplo, en-US coincide con en, o en-US coincide con en- \* .

#### <a name="macro-region-match"></a>Coincidencia de región de macro

Las etiquetas coinciden con las subetiquetas de lenguaje y script; ambas etiquetas tienen subetiquetas region, una de las cuales denota una región de macro que abarca la otra región. Las subetiquetas de la región de macro son siempre numéricas y se derivan de la división de estadísticas de las Naciones Unidas M. 49 códigos de país y de área. Para obtener más información sobre las relaciones englobales, consulte [composición de regiones geográficas de macros (continental), subregiones geográficas y otras agrupaciones económicas y de otro tipo](https://unstats.un.org/unsd/methods/m49/m49regin.htm).

**Nota:** No se admiten códigos para "agrupaciones económicas" u "otras agrupaciones" en BCP-47.
 
**Nota:** Una etiqueta con la subetiqueta de la región de macro "001" se considera equivalente a una etiqueta independiente de la región. Por ejemplo, "es-001" y "es" se tratan como sinónimos.

#### <a name="region-neutral-match"></a>Coincidencia independiente de la región

Las etiquetas coinciden con las subetiquetas de idioma y script, y una sola etiqueta tiene una etiqueta region. Se prefiere una coincidencia principal con respecto a otras coincidencias parciales.

#### <a name="orthographic-affinity-match"></a>Coincidencia de afinidad ortográfica

Las etiquetas coinciden con las subetiquetas de idioma y script, y las subetiquetas de región tienen afinidad ortográfica. La afinidad se basa en los datos que se mantienen en Windows y que define regiones afines específicas del idioma, por ejemplo, "en-IE" y "en-GB".

#### <a name="preferred-region-match"></a>Coincidencia de región preferida

Las etiquetas coinciden con las subetiquetas de idioma y script, y una de las subetiquetas de región es la subetiqueta de región predeterminada del idioma. Por ejemplo, "fr-FR" es la región predeterminada de la subetiqueta "fr". Por lo tanto, fr-FR es una coincidencia mejor para fr-is que FR-CA. Esto se basa en los datos mantenidos en Windows que definen una región predeterminada para cada idioma en el que se localiza Windows.

#### <a name="sibling-match"></a>Coincidencia del mismo nivel

Las etiquetas coinciden con las subetiquetas de lenguaje y script, y ambas tienen subetiquetas region, pero no hay ninguna otra relación definida entre ellas. En el caso de varias coincidencias del mismo nivel, el último elemento relacionado enumerado será el ganador, en ausencia de una coincidencia mayor.

### <a name="undetermined-language"></a>Idioma no determinado

Un recurso se puede etiquetar como "und" para indicar que coincide con cualquier idioma. Esta etiqueta también se puede usar con una etiqueta de script para filtrar las coincidencias en función del script. Por ejemplo, "und-latn" coincidirá con cualquier etiqueta de idioma que use el alfabeto latino. Consulte a continuación para más información.

### <a name="script-mismatch"></a>Error de coincidencia de script

Cuando las etiquetas coinciden solo en la etiqueta de idioma principal pero no en el script, se considera que el par coincide y se Puntua por debajo del nivel de una coincidencia válida.

### <a name="no-match"></a>Ninguna coincidencia

Las subetiquetas de idioma principal no coincidentes se puntuan por debajo del nivel de una coincidencia válida. Por ejemplo, ZH-hant no coincide con ZH-Hans.

## <a name="examples"></a>Ejemplos

Un idioma de usuario "ZH-Hans-CN" (chino simplificado (China)) coincide con los siguientes recursos en el orden de prioridad que se muestra. Una X indica que no hay coincidencia.

![Coincidencia para chino simplificado (China)](images/language_matching_1.png)

1. Coincidencia exacta; dos. & 3. Coincidencia de región; 4. Coincidencia primaria; 5. Coincidencia del mismo nivel.

Cuando una subetiqueta del lenguaje tiene un valor Suppress-Script definido en el registro de subetiqueta BCP-47, se produce la coincidencia correspondiente, tomando el valor del código de script suprimido. Por ejemplo, en-Latn-US coincide con en-US. En el siguiente ejemplo, el idioma del usuario es "en-AU" (Inglés (Australia)).

![Búsqueda de coincidencias para inglés (Australia)](images/language_matching_2.png)

1. Coincidencia exacta; dos. Coincidencia de región de macro; 3. Coincidencia independiente de la región; 4. Coincidencia de afinidad ortográfica; 5. Coincidencia de región preferida; 1,8. Coincidencia del mismo nivel.

## <a name="matching-a-language-to-a-language-list"></a>Coincidencia de un idioma con una lista de idiomas

En ocasiones, la coincidencia se produce como parte de un proceso mayor de coincidencia de un solo idioma con una lista de idiomas. Por ejemplo, puede haber una coincidencia de un solo recurso basado en el lenguaje con la lista de idiomas de la aplicación. La puntuación de la coincidencia se pondera con la posición del primer idioma coincidente de la lista. Cuanto menor sea el idioma de la lista, menor será la puntuación.

Cuando la lista de idiomas contiene dos o más variantes regionales con las mismas etiquetas de lenguaje y script, las comparaciones de la primera etiqueta de idioma solo se puntuan para las coincidencias exactas, variantes y de regiones. La puntuación de coincidencias parciales se pospone a la última variante regional. Esto permite a los usuarios controlar con precisión el comportamiento de la búsqueda de coincidencias en su lista de idiomas. El comportamiento de búsqueda de coincidencias puede incluir permitir una coincidencia exacta de un elemento secundario de la lista para que sea preferible una coincidencia parcial para el primer elemento de la lista, si hay un tercer elemento que coincida con el idioma y el script de la primera. Aquí tiene un ejemplo.

- Lista de idiomas (en orden): "pt-PT" (Portugués (Portugal)), "en-US" (Inglés (Estados Unidos)), "pt-BR" (Portugués (Brasil)).
- Recursos: "en-US", "pt-BR".
- Recurso con la puntuación más alta: "en-US".
- Descripción: la comparación comienza con "pt-PT" pero no encuentra una coincidencia exacta. Debido a la presencia de "pt-BR" en la lista de idiomas del usuario, la coincidencia parcial se pospone a la comparación con "pt-BR". La siguiente comparación de lenguaje es "en-US", que tiene una coincidencia exacta. Por lo tanto, el recurso ganador es "en-US".

O BIEN

- Lista de idiomas (en orden): "es-MX" (Español (México)), "es-HO" (Español (Honduras)).
- Recursos: "en-ES", "es-HO".
- Recurso con la puntuación más alta: "es-HO".

## <a name="undetermined-language-und"></a>Idioma no determinado ("und")

La etiqueta de idioma "und" se puede usar para especificar un recurso que se corresponda con cualquier idioma en ausencia de una coincidencia mejor. Puede considerarse similar al intervalo de lenguaje BCP-47 " *" o "* - &lt; script &gt; ". Aquí tiene un ejemplo.

- Lista de idiomas: "en-US", "ZH-Hans-CN".
- Recursos: "ZH-Hans-CN", "und".
- Recurso con la puntuación más alta: "und".
- Descripción: la comparación comienza con "en-US", pero no encuentra ninguna coincidencia basada en "en" (parcial o superior). Dado que hay un recurso etiquetado con "und", el algoritmo de coincidencia lo usa.

La etiqueta "und" permite que varios idiomas compartan un único recurso y permite que los lenguajes individuales se traten como excepciones. Por ejemplo.

- Lista de idiomas: "ZH-Hans-CN", "en-US".
- Recursos: "ZH-Hans-CN", "und".
- Recurso con la puntuación más alta: "ZH-Hans-CN".
- Descripción: la comparación encuentra una coincidencia exacta para el primer elemento y, por tanto, no comprueba el recurso con la etiqueta "und".

Puede usar "und" con una etiqueta de script para filtrar los recursos por script. Por ejemplo.

- Lista de idiomas: "ru".
- Recursos: "und-latn", "und-Cyrl", "und-árabe".
- Recurso con la puntuación más alta: "und-Cyrl".
- Descripción: la comparación no encuentra ninguna coincidencia para "ru" (parcial o mejor), por lo que coincide con la etiqueta de idioma "und". El valor de suprimir script "Cyrl" asociado a la etiqueta de idioma "ru" coincide con el recurso "und-Cyrl".

## <a name="orthographic-regional-affinity"></a>Afinidad regional ortográfica

Cuando se buscan coincidencias con dos etiquetas de idioma con diferencias de subetiqueta de región, determinados pares de regiones pueden tener una afinidad mayor entre ellas que otras. Los únicos grupos afines admitidos son para inglés ("en"). Las subetiquetas de región "PH" (Filipinas) y "LR" (Liberia) tienen afinidad ortográfica con la subetiqueta de la región "EE. UU.". Todas las demás subetiquetas de región se encontrarán con la subetiqueta de región "GB" (Reino Unido). Por lo tanto, cuando estén disponibles los recursos "en-US" y "en-GB", una lista de idiomas de "en-HK" (Inglés (RAE de Hong Kong)) obtendrá una puntuación más alta con recursos "en-GB" que los recursos "en-US".

## <a name="handling-languages-with-many-regional-variants"></a>Control de idiomas con muchas variantes regionales

Algunos idiomas tienen grandes comunidades de oradores en distintas regiones que usan diferentes variedades de &mdash; idiomas de idioma, como el inglés, el francés y el español, que son entre los que se admiten con más frecuencia en aplicaciones multilingües. Las diferencias regionales pueden incluir diferencias en orthography (por ejemplo, "color" frente a "color") o diferencias de dialecto como el vocabulario (por ejemplo, "Truck" frente a "camión").

Estos lenguajes con variantes regionales significativas presentan ciertos desafíos al crear una aplicación de uso internacional: "¿Cuántas variantes regionales diferentes deben admitirse?" "¿Cuáles?" "¿Cuál es la manera más rentable de administrar estos activos variantes de la región para la aplicación?" Queda fuera del ámbito de este tema responder a todas estas preguntas. Sin embargo, los mecanismos de coincidencia de lenguajes de Windows proporcionan funciones que pueden ayudarle a administrar variantes regionales.

A menudo, las aplicaciones solo admiten una única variedad de cualquier lenguaje determinado. Supongamos que una aplicación tiene recursos para solo una variedad de inglés que se espera que usen los hablantes en inglés, independientemente de la región desde la que se encuentren. En este caso, la etiqueta "en" sin ninguna subetiqueta de región reflejaría la expectativa. Sin embargo, es posible que las aplicaciones hayan usado históricamente una etiqueta, como "en-US", que incluye una subetiqueta region. En este caso, esto también funcionará: la aplicación usa solo una variedad de inglés, y los identificadores de Windows que coinciden con un recurso etiquetado para una variante regional con una preferencia de idioma de usuario para una variante regional diferente de una manera adecuada.

Sin embargo, si se van a admitir dos o más variedades regionales, una diferencia como "en" frente a "en-US" puede tener un impacto significativo en la experiencia del usuario y es importante tener en cuenta qué subetiquetas de región usar.

Supongamos que desea proporcionar localizaciones en francés independientes para francés, tal y como se usa en Canadá frente al francés europeo. En francés canadiense, se puede usar "FR-CA". En el caso de los oradores de Europa, la localización usará francés (Francia) y, por tanto, se puede usar "fr-FR" para eso. Pero, ¿qué ocurre si un usuario determinado es de Bélgica, con una preferencia de idioma de "fr-is"; ¿qué obtendrá? La región "ser" es diferente de "FR" y "CA", lo que sugiere una coincidencia de "cualquier región" para ambos. Sin embargo, Francia es la región preferida para el francés y, por lo tanto, el "fr-FR" se considerará la mejor coincidencia en este caso.

Supongamos que la aplicación se ha localizado en primer lugar solo para una variedad de francés, con cadenas en francés (Francia), pero que se califican genéricamente como "fr" y, a continuación, se desea agregar compatibilidad con francés canadiense. Probablemente solo algunos recursos deben volver a traducirse para francés canadiense. Puede seguir usando todos los recursos originales, manteniéndolos calificados como "fr" y simplemente agregando el pequeño conjunto de recursos nuevos mediante "FR-CA". Si la preferencia de idioma del usuario es "FR-CA", el recurso "FR-CA" tendrá una puntuación de coincidencia mayor que el recurso "fr". Pero si la preferencia de idioma del usuario es para cualquier otra variedad de francés, el recurso "fr" de la región neutra será una coincidencia mejor que el recurso "FR-CA".

Otro ejemplo: Supongamos que desea proporcionar localizaciones en Español independientes para los altavoces de España frente a los altavoces de América Latina. Supongamos que las traducciones de América Latina se proporcionaron de un proveedor en México. ¿Debe usar "es-ES" (España) y "es-MX" (México) para dos conjuntos de recursos? En caso de que lo hiciera, podría crear problemas para los altavoces de otras regiones de América Latina, como Argentina o Colombia, ya que obtendría los recursos "es-ES". En este caso, hay una mejor alternativa: puede usar una subetiqueta de la región de macro, "es-419" para reflejar que desea que los recursos se usen para los oradores de cualquier parte de América Latina o del Caribe.

Las etiquetas de lenguaje neutros de la región y las subetiquetas de la región de macro pueden ser muy eficaces si desea admitir varias variedades regionales. Para minimizar el número de recursos independientes que necesita, puede calificar un recurso determinado de forma que refleje la cobertura más amplia para la que es aplicable. A continuación, complemente un activo ampliamente aplicable con una variante más específica según sea necesario. Se utilizará un recurso con un calificador de lenguaje neutro para los usuarios de cualquier variedad regional, a menos que haya otro recurso con un calificador más específico de la región que se aplique a ese usuario. Por ejemplo, un recurso "en" coincidirá con un usuario de Australia en inglés, pero un recurso con "en-053" (Inglés tal como se usa en Australia o Nueva Zelanda) será una coincidencia mejor para ese usuario, mientras que un recurso con "en-AU" será la mejor coincidencia posible.

El inglés necesita una consideración especial. Si una aplicación agrega localización para dos variedades en inglés, es probable que sea para Inglés de EE. UU. y para Reino Unido, o "internacional", en inglés. Como se indicó anteriormente, en algunas regiones fuera de EE. UU. se sigue Estados Unidos convenciones ortográficas, y la coincidencia de idioma de Windows tiene esto en cuenta. En este escenario, no se recomienda usar la etiqueta independiente de región "en" para una de las variantes; en su lugar, use "en-GB" y "en-US". (Si un recurso determinado no requiere variantes independientes, se puede usar "en"). Si "en-GB" o "en-US" se reemplaza por "en", eso interferirá con la afinidad regional ortográfica que proporciona Windows. Si se agrega una localización de tercer inglés, use una subetiqueta de región de macro o específica para las variantes adicionales según sea necesario (por ejemplo, "en-CA", "en-AU" o "en-053"), pero siga usando "en-GB" y "en-US".

## <a name="related-topics"></a>Temas relacionados

* [Cómo el sistema de administración de recursos compara y elige recursos](how-rms-matches-and-chooses-resources.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](../design/globalizing/manage-language-and-region.md)
* [Composición de regiones geográficas de macros (continental), subregiones geográficas y otras agrupaciones económicas y de otro tipo](https://unstats.un.org/unsd/methods/m49/m49regin.htm)

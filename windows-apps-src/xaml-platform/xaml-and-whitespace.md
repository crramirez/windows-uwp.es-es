---
author: jwmsft
description: Obtén información sobre las reglas de procesamiento de espacios en blanco que usa XAML.
title: XAML y espacio en blanco
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 560f820ec2ecc7f28145ec29c31a60c1e4573d7e
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7568976"
---
# <a name="xaml-and-whitespace"></a>XAML y espacio en blanco


Obtén información sobre las reglas de procesamiento de espacios en blanco que usa XAML.

## <a name="whitespace-processing"></a>Procesamiento de espacios en blanco

Coherente con XML, caracteres de espacio en blanco en XAML son espacios, saltos de línea y tabulaciones. Estos corresponden a los valores Unicode 0020, 000A y 0009 respectivamente. De manera predeterminada, se produce la siguiente normalización de los espacios en blanco cuando un procesador XAML procesa el texto interno que se encuentra entre los elementos de un archivo XAML:

-   Se eliminan los saltos de línea entre los caracteres de Asia oriental.
-   Todos los espacios en blanco (espacio, salto de línea, tabulación) se convierten en espacios.
-   Se eliminan todos los espacios consecutivos y se reemplazan por un espacio.
-   Se elimina el espacio que sigue inmediatamente a la etiqueta de inicio.
-   Se elimina el espacio que precede a la etiqueta de cierre.
-   Los *caracteres de Asia Oriental* se definen como un conjunto de intervalos de caracteres Unicode: de U+20000 a U+2FFFD y de U+30000 a U+3FFFD. Este subconjunto también se denomina *ideogramas CJK* Para obtener más información, consulta http://www.unicode.org.

"Predeterminado" corresponde al estado que indica el valor predeterminado del atributo **xml:space**.

### <a name="whitespace-in-inner-text-and-string-primitives"></a>Espacio en blanco en texto interno y primitivos de cadena

Las reglas de normalización anteriores se aplican al texto interno que se encuentra dentro de los elementos XAML. Después de la normalización, el procesador XAML convierte el texto interno en un tipo apropiado de la siguiente manera:

-   Si el tipo de la propiedad no es una colección, pero no es directamente un tipo **Object**, el procesador XAML intenta convertirlo a ese tipo con el convertidor de tipos. Aquí, un error de conversión generará un error de análisis de XAML.
-   Si el tipo de la propiedad es una colección y el texto interno es contiguo (no intervienen etiquetas de elementos), el texto interno se analiza como una sola **String**. Si el tipo de colección no acepta **String**, también se genera un error del analizador de XAML.
-   Si el tipo de la propiedad es **Object**, el texto interno se analiza como una sola **String**. Si intervienen etiquetas de elementos, se genera un error del analizador de XAML porque el tipo **Object** implica un solo objeto (**String** u otro).
-   Si el tipo de la propiedad es una colección y el texto interno no es contiguo, la primera subcadena se convierte en **String** y se agrega como un elemento de colección, el elemento interviniente se agrega como elemento de colección y, por último, se agrega la subcadena final (si existe) a la colección como tercer elemento **String**.

### <a name="whitespace-and-text-content-models"></a>Modelos de espacio en blanco y contenido de texto

En la práctica, la conservación de los espacios en blanco solo interesa para un subconjunto de todos los posibles modelos de contenido. Ese subconjunto se compone de modelos de contenido que aceptan un tipo de **String** singleton con cierto formato, una colección de **String** dedicada o una mezcla de **String** y otros tipos en listas, colecciones o diccionarios.

Incluso para los modelos de contenido que aceptan cadenas, el comportamiento predeterminado en esos modelos de contenido es que cualquier espacio en blanco que quede no se trate como significativo.

### <a name="preserving-whitespace"></a>Conservación del espacio en blanco

Hay varias técnicas para conservar los espacios en blanco en el código XAML de origen para la presentación final que no se ven afectadas por la normalización de espacios en blanco del procesador XAML.

`xml:space="preserve"`: especifica este atributo en el nivel del elemento donde se desea conservar el espacio en blanco. Ten en cuenta que esto conservará todos los espacios en blanco, incluidos los espacios que podrían agregar las aplicaciones de edición de código o las superficies de diseño para alinear los elementos de marcado como un anidamiento visualmente intuitivo. Una vez más, que esos espacios se representen depende del modelo de contenido del elemento contenedor. No se recomienda especificar `xml:space="preserve"` en el nivel raíz ya que la mayoría de los modelos de objetos no consideran los espacios en blanco significativos de una u otra forma. Se recomienda establecer únicamente el atributo de forma específica en el nivel de elementos que representan los espacios en blanco dentro de las cadenas o que son colecciones de espacios en blanco significativas.

Entidades y los espacios de no separación: XAML admite la colocación de cualquier entidad Unicode dentro de un modelo de objetos de texto. Puedes usar entidades dedicadas como espacios de no separación (en la codificación UTF-8). También puedes usar controles de texto enriquecido que admitan espacios de no separación. Ten cuidado si vas a usar entidades para simular características de diseño como sangrías, ya que la salida en tiempo de ejecución de las entidades se ve afectada por un número mayor de factores que con las utilidades de diseño generales, como el uso correcto de paneles y márgenes.


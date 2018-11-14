---
author: stevewhims
Description: A localized app is one that can be localized to other markets, languages, or regions without uncovering any functional defects in the app. The most essential property of a localizable app is that its executable code has been cleanly separated from its localizable resources.
title: Haz que tu aplicación sea localizable
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localización
ms.localizationpriority: medium
ms.openlocfilehash: 48244889dd927f41d0998214cf1120377c4bb251
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6455767"
---
# <a name="make-your-app-localizable"></a>Haz que tu aplicación sea localizable

Una aplicación localizada es aquella que puede localizarse para otros mercados, idiomas o regiones sin descubrir defectos funcionales en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado limpiamente de sus recursos localizables. Por lo tanto, debes determinar qué recursos de la aplicación deben localizarse. Pregúntate qué tiene que cambiarse si tu aplicación debe localizarse para otros mercados.

También recomendamos familiarizarse con las [Directrices para la globalización](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="put-your-strings-into-resources-files-resw"></a>Coloca las cadenas en archivos de recursos (.resw)

No codifiques literales de cadena en el código imperativo, la revisión XAML, ni en el manifiesto del paquete de aplicación. En su lugar, coloca las cadenas en archivos de recursos (.resw) para que se puedan adaptar a diferentes mercados locales, independientemente de los binarios integrados de la aplicación. Para más información, consulta [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de aplicación](../../app-resources/localize-strings-ui-manifest.md).

Este tema también muestra cómo agregar comentarios a los archivos de recursos predeterminados (.resw). Por ejemplo, si adoptas una voz o tono informal, a continuación, asegúrate de explicarlo en los comentarios. Además, para reducir los gastos, confirma que solo las cadenas que deben traducirse se entregan a los traductores.

Establece el idioma predeterminado de la aplicación de manera correcta en el manifiesto de paquete de aplicación (archivo `Package.appxmanifest`). El idioma predeterminado determina el idioma que se usa cuando el idioma preferido del usuario no coincide con ninguno de los idiomas admitidos de la aplicación. Marca todos los recursos con su idioma (incluso aquellos en tu idioma predeterminado, por ejemplo `\Assets\en-us\Logo.png`) para que el sistema pueda determinar en qué idioma está el recurso y cómo se usa en situaciones particulares.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Adaptar las imágenes y otros archivos de recursos al idioma

En teoría, podrás globalizar tus imágenes, es decir, hacerlas culturalmente independientes. Para las imágenes y otros recursos de archivos para los que esto no sea posible, crea todas las variantes diferentes que necesites y coloca los calificadores de idioma apropiados en sus nombres de archivo o carpeta. Para saber más, consulta [Adaptar los recursos para idioma, escala, contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md).

Para reducir los costes de localización, no pongas texto ni material culturalmente sensible en las imágenes. Una imagen que sea apropiada en tu cultura, tal vez se considere ofensiva o no se interprete correctamente en otras culturas. Evita el uso de imágenes culturalmente específicas como los buzones, que no son iguales en todo el mundo. Evita símbolos religiosos, animales, políticos o imágenes sexistas. La visualización de la piel, las partes del cuerpo o los gestos con las manos también puede ser un tema delicado. Si no puedes evitar todo esto, las imágenes deberán localizarse cuidadosamente. Si localizas a un idioma que tiene una dirección de lectura diferente de la del tuyo, el uso de efectos e imágenes simétricas facilita la admisión de la creación de reflejos.

También evita el uso de texto en imágenes y los fragmentos hablados en archivos de audio y vídeo.

## <a name="the-use-of-color-in-your-app"></a>El uso del color en la aplicación

Sé cuidadoso al usar el color. Usar combinaciones de colores que estén asociados con movimientos políticos o banderas de naciones puede ser problemático. Las opciones de color tal vez se deban revisar por expertos culturales. También hay problemas de accesibilidad al usar el color. Si usas el color para transmitir la información, también debes transmitir esa misma información por otros medios, como el tamaño, la forma o una etiqueta.

## <a name="consider-factoring-your-strings-into-sentences"></a>Considerar la factorización de las cadenas en frases

Usar cadenas de tamaño apropiado. Las cadenas cortas son más fáciles de traducir y permiten reciclar la traducción (lo que ahorra gastos porque la misma cadena no se envía al localizador más de una vez). Además, es posible que las cadenas muy largas no sean compatibles con las herramientas de localización.

Pero, en tensión con esta directriz, está el riesgo de volver a usar una cadena en contextos diferentes. Incluso las palabras más simples como &quot;activado&quot; y &quot;desactivado&quot; pueden traducirse de manera diferente según el contexto. En inglés "on" y "off" pueden usarse para alternar entre el modo de vuelo, la red Bluetooth y los dispositivos. No obstante, en italiano, la traducción depende del contexto de lo que se activa o desactiva. Debes crear un par de cadenas para cada contexto. Puedes volver a usar las cadenas si dos contextos son iguales. Por ejemplo, puedes volver a usar la cadena "Volume" tanto para el volumen de los efectos de sonido como para el volumen de la música porque ambos hacen referencia a la intensidad del sonido. En cambio, no debes volver a usar la misma cadena cuando hagas referencia a "Volume" cuando se refiera al disco duro, porque el contexto y el significado son diferentes y la palabra puede traducirse de manera diferente.

Igualmente, una cadena como "text" o "fax" puede usarse tanto como verbo como sustantivo en inglés, lo que puede causar confusión en el proceso de traducción. En su lugar, crea una cadena separada para el formato de verbo y el de sustantivo. Cuando no estés seguro de si el contexto es el mismo, por si acaso, usa una cadena diferente.

En resumen, factorizar las cadenas en las partes que funcionan en todos los contextos. Habrá casos donde una cadena deberá ser una oración completa.

Ten en cuenta la siguiente cadena: "el {0} no pudo sincronizarse."

Hay varias palabras que podrían reemplazar {0}, como "una cita", "task" o "documento". Si bien este ejemplo funciona para el inglés, no lo hará en todos los casos para la misma oración en alemán, por ejemplo. Ten en cuenta que en las siguientes oraciones en alemán, algunas de las palabras de la cadena de plantilla ("Der", "Die", "Das") tienen que coincidir con la palabra parametrizada:

| Inglés                                    | Alemán                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| No pudo sincronizarse la tarea.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| No pudo sincronizarse el documento.    | Das Dokument konnte nicht synchronisiert werden. |

Como otro ejemplo, considera la posibilidad de la frase "Remind me en {0} Minute (s)." "Minute(s)" funciona perfectamente en inglés, pero es más que probable que otros idiomas usen términos diferentes. Por ejemplo, en polaco, se usa "minuta", "minuty" o "minut" según el contexto.

Para resolver este problema, localiza toda la oración en lugar de una única palabra. Aunque hacer esto parezca demandar trabajo extra y que no sea una solución elegante, es la mejor solución porque:

-   Se mostrará un mensaje gramaticalmente correcto para todos los idiomas.
-   El traductor no tendrá que preguntar qué es lo que reemplazará las cadenas.
-   De todos modos, no necesitarás implementar una engorrosa corrección de código cuando aparezca un problema como este después de que completes la aplicación.

## <a name="other-considerations-for-strings"></a>Otras consideraciones para las cadenas

Evita coloquialismos y metáforas en las cadenas que creas en el idioma predeterminado. El lenguaje que es específico de un grupo demográfico, como una referencia cultural o una edad, puede ser difícil de entender o traducir porque solamente lo usan las personas de ese grupo demográfico. Del mismo modo, las metáforas pueden tener sentido para una persona, pero no significar nada para otra. Por ejemplo &quot;rizo&quot; significa algo para quienes pertenecen al ámbito de la aviación, pero aquellos que no pertenecen a este ámbito no comprenden la referencia.

No uses una jerga técnica, abreviaturas ni acrónimos. Es muy poco probable que aquellas personas que no pertenecen al ámbito técnico o que provienen de otras culturas o regiones comprendan el lenguaje técnico, como también es difícil que puedan traducirlo. En las conversaciones de la vida diaria no se usan este tipo de palabras. El lenguaje técnico generalmente aparece en los mensajes de error para identificar problemas de hardware y software, pero las cadenas deberían ser técnicas *solo si el usuario necesita ese nivel de información y puede realizar estas acciones o encontrar alguien que pueda*.

Usar una voz o tono informal en las cadenas es una opción válida. Puedes usar los comentarios en los archivos de recursos (.resw) predeterminados para indicar esa intención.

## <a name="pseudo-localization"></a>Pseudolocalización

Pseudolocaliza tu aplicación para descubrir los problemas de localización. Pseudolocalización es un tipo de localización de ejecución en seco o prueba de divulgación. Se produce un conjunto de recursos que no están traducidos realmente, solo lo parecen. Por ejemplo, las cadenas son aproximadamente un 40% más largas que en el idioma predeterminado y tienen delimitadores para que puedas ver de un solo vistazo si se han truncado en la interfaz de usuario.

## <a name="geopolitical-awareness"></a>Reconocimiento geopolítico

Evita agresiones políticas en mapas o cuando hagas referencia a regiones. Los mapas pueden incluir fronteras nacionales e internacionales controvertidas, lo que frecuentemente es un origen de ofensa política. Ten cuidado de hacer que cualquier interfaz de usuario usada para seleccionar una nación haga referencia a ella como &quot;país o región&quot;. Colocar un territorio en disputa en una lista llamada &quot;países&quot;, como en un formulario de direcciones, puede generarte problemas.

## <a name="language--and-region-changed-events"></a>Eventos de cambio de idioma y región

Suscríbete a eventos que se generan cuando cambia la configuración del sistema de idioma y región. Haz esto para que se puedan volver a cargar los recursos, si corresponde. Para obtener más información, consulta [Actualizar cadenas en respuesta a eventos de cambio de valor de calificador](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) y [Actualización de imágenes en respuesta a eventos de cambio de valor de calificador](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Garantizar el orden de parámetro correcto cuando se le da formato a las cadenas

No supongas que todos los idiomas expresan los parámetros en el mismo orden. Por ejemplo, observa este formato.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

La cadena de formato de este ejemplo funciona para el inglés (Estados Unidos). Pero no es apropiado para el alemán (Alemania), por ejemplo, donde el día y el mes se muestran en orden inverso. Asegúrate de que el traductor sepa el propósito de cada uno de los parámetros para que pueda invertir el orden de los elementos de formato de la cadena de formato (por ejemplo, "{1} {0}") según sea adecuado para el idioma de destino.

## <a name="dont-over-localize"></a>No localices en exceso.

Enviar solo lenguaje natural a los traductores, no lenguaje de programación ni de marcado. Una etiqueta `<link>` no es lenguaje natural. Observa estos ejemplos.

| No localices esto                   | Localiza esto |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;términos de uso&lt;/link&gt;   | términos de uso               |
| &lt;link&gt;directiva de privacidad&lt;/link&gt; | directiva de privacidad             |

Incluir la etiqueta `<link>` en el archivo de recursos (.resw) significa que, además, es probable que deba traducirse. Esto podría representar la etiqueta como no válida. Si tienes cadenas largas que deben incluir marcado para mantener el contexto y garantizar la ordenación, deja claro en los comentarios lo que no debe traducirse.

## <a name="choose-an-appropriate-translation-approach"></a>Elegir un enfoque de traducción apropiado

Después de que las cadenas se separen en archivos de recursos, pueden traducirse. El momento ideal para traducir cadenas es después de que se finalizan las cadenas de un proyecto, que generalmente sucede hacia el final de éste. Puedes abordar el proceso de traducción de diferentes maneras, según el volumen de cadenas por traducir, el número de idiomas por traducir y el modo en que se realizará la traducción (como internamente en contraposición a mediante la contratación de un proveedor externo).

Ten en cuenta estas opciones.

- **Puedes traducir los archivos de recursos abriéndolos directamente en el proyecto.** Este enfoque funciona correctamente en un proyecto que tenga un pequeño volumen de cadenas que tengan que traducirse a dos o tres idiomas. Puede ser apropiado para un escenario en el que un desarrollador habla más de un idioma y quiere controlar el proceso de traducción. Este enfoque es beneficioso ya que es rápido, no requiere herramientas y minimiza el riesgo de malas traducciones. Pero no es escalable. En particular, los recursos de diferentes idiomas pueden perder la sincronización con facilidad, lo que causa malas experiencias de usuario y dolores de cabeza en el mantenimiento.
- **El formato del texto de los archivos de recursos de cadena es de tipo XML o ResJSON, así que puedes traducirlos mediante cualquier editor de texto. Una vez hecho esto, los archivos que hayas traducido se copiarán en el proyecto.** Debes tener en cuenta que, si los traductores usan este método, es posible que acaben traduciendo por accidente las etiquetas XML, pero podrán trabajar en un entorno que no sea el proyecto Microsoft Visual Studio. Puedes usar este método para aquellos proyectos que debas traducir en unos pocos idiomas. El formato XLIFF es un formato XML que se diseñó específicamente para la localización, y que es compatible con varias de herramientas y proveedores de localización. Asimismo, puedes usar el [Kit de herramientas para aplicaciones multilingües](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) para crear archivos XLIFF a partir de otros archivos de recursos como, por ejemplo, .resw o .resjson.

Probablemente sea necesario realizar entregas a los localizadores en el caso de otros archivos, como los archivos de audio y vídeo.

Además, considera estas sugerencias.

- **Usa una herramienta de localización.** Tienes a tu disposición un gran número de herramientas de localización que puedes usar para analizar archivos de recursos y permitir que los traductores editen solamente las cadenas traducibles. Este enfoque reduce el riesgo de que un traductor edite las etiquetas XML por error, pero tiene la desventaja de introducir un nuevo proceso y herramienta al proceso de localización. Una herramienta de localización es un buen recurso para organizar proyectos que tengan un gran volumen de cadenas pero que solo necesiten traducirse a una pequeña cantidad de idiomas. Para obtener más información, consulta [Cómo usar el kit de herramientas para aplicaciones multilingües](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
- **Usa un proveedor de localización.** Puedes usar un proveedor de localización si el proyecto contiene un gran volumen de cadenas y debe traducirse a muchos idiomas. Un proveedor de localizaciones puede darte consejo sobre las herramientas y los procesos, así como traducir tus archivos de recursos. Esta es una solución ideal, pero también la opción más costosa y puede aumentar los tiempos de entrega para el contenido traducido.

## <a name="keep-access-keys-and-labels-consistent"></a>Mantener la coherencia de las claves de acceso y las etiquetas

Es todo un desafío sincronizar las claves de acceso que se usan en los métodos de accesibilidad con el modo de visualizar las claves de acceso localizadas, ya que los dos recursos de cadena se clasifican en dos secciones separadas. Asegúrate de proporcionar comentarios para la cadena de etiqueta, como: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Admitir furigana para las cadenas en japonés que puedan ordenarse

Los caracteres en kanji japonés tienen la propiedad de tener más de una lectura (pronunciación) dependiendo de la palabra en la que se usen. Esto supone un problema cuando intentas ordenar objetos con nombres en japonés, como nombres de aplicaciones, archivos, canciones, etc. En el pasado, el kanji japonés se solía ordenar siguiendo un orden que las máquinas pueden comprender llamado XJIS. Desafortunadamente, debido a que este orden de clasificación no es fonético, no es muy útil para los usuarios.

*Furigana* proporciona una solución a este problema al permitir que el usuario o creador especifique la fonética para los caracteres que está usando. Si usas el siguiente procedimiento para agregar furigana al nombre de tu aplicación, puedes asegurarte de que está ordenado en la ubicación correcta de la lista de aplicaciones. Si el nombre de tu aplicación contiene caracteres kanji y no se proporciona furigana cuando el idioma de la interfaz de usuario o el orden de clasificación se establece en japonés, Windows busca la mejor manera de generar la pronunciación apropiada. No obstante, existe la posibilidad de que los nombres de las aplicaciones que contienen texto por leer poco común o exclusivo se ordenen conforme a una lectura más común. Por consiguiente, el procedimiento recomendado para las aplicaciones en japonés (especialmente las que contienen caracteres kanji en sus nombres) es proporcionar una versión en furigana del nombre de la aplicación como parte del proceso de localización al japonés.

1.  Agrega "ms-resource:Appname" como nombre para mostrar del paquete y nombre para mostrar de la aplicación.
2.  Crea una carpeta denominada ja-JP bajo el elemento Strings y agrega dos archivos de recursos tal como sigue:

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  En el elemento Resources.resw de la capeta ja-JP general: agrega un recurso de cadena para el elemento Appname "希蒼"
4.  En el elemento Resources.altform-msft-phonetic.resw de los recursos de japonés en furigana: agrega el valor furigana del elemento AppName "のあ"

De esta manera, el usuario puede buscar el nombre de la aplicación "希蒼" usando tanto el valor furigana "のあ" (noa) como el valor fonético (mediante la función **GetPhonetic** del Editor de métodos de entrada (IME)) "まれあお" (mare-ao).

El método de ordenación sigue el formato del **Panel de control regional**:

-   En una configuración regional del usuario en japonés,
    -   Si está habilitado el furigana, "希蒼" se ordena conforme a "の".
    -   Si falta el furigana, "希蒼" se ordena conforme a "ま".
-   En una configuración regional del usuario que no sea japonés,
    -   Si está habilitado el furigana, "希蒼" se ordena conforme a "の".
    -   Si falta el furigana, "希蒼" se ordena conforme a "漢字".

## <a name="related-topics"></a>Artículos relacionados

* [Directrices sobre globalización](guidelines-and-checklist-for-globalizing-your-app.md)
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de aplicación](../../app-resources/localize-strings-ui-manifest.md)
* [Adaptar los recursos al idioma, escala, contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Ajustar el diseño y las fuentes y admitir la escritura de derecha a izquierda](adjust-layout-and-fonts--and-support-rtl.md)
* [Actualización de imágenes en respuesta a eventos de cambio de valor de calificador](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Ejemplos

* [Ejemplo de recursos de aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=254478)

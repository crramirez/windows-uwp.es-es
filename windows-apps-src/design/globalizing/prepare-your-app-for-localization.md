---
Description: Una aplicación localizada es aquella que se puede localizar en otros mercados, idiomas o regiones sin que se detecten defectos funcionales en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado limpiamente de sus recursos localizables.
title: Hacer que la aplicación sea localizable
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 4b914c0a2bcfae630b8b491ed702b237ce0eaaee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172539"
---
# <a name="make-your-app-localizable"></a>Hacer que la aplicación sea localizable

Una aplicación localizada es aquella que se puede localizar en otros mercados, idiomas o regiones sin que se detecten defectos funcionales en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado limpiamente de sus recursos localizables. Por lo tanto, debe determinar qué recursos de la aplicación deben localizarse. Pregúntese lo que necesita cambiar si la aplicación se va a localizar en otros mercados.

También se recomienda familiarizarse con las [directrices para la globalización](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="put-your-strings-into-resources-files-resw"></a>Coloque las cadenas en archivos de recursos (. resw)

No codifique literales de cadena en el código imperativo, marcado XAML ni en el manifiesto del paquete de la aplicación. En su lugar, coloque las cadenas en archivos de recursos (. resw) para que se puedan adaptar a distintos mercados locales independientemente de los binarios compilados de la aplicación. Para obtener más información, consulte [localizar cadenas en el manifiesto del paquete de la interfaz de usuario y de la aplicación](../../app-resources/localize-strings-ui-manifest.md).

En ese tema también se muestra cómo agregar comentarios a un archivo de recursos predeterminado (. resw). Por ejemplo, si va a adoptar una voz o un tono informal, asegúrese de explicarlo en los comentarios. Además, para minimizar los gastos, confirme que solo se proporcionan a los traductores las cadenas que se deben traducir.

Establezca el idioma predeterminado de la aplicación correctamente en el archivo de origen del manifiesto del paquete de la aplicación (el `Package.appxmanifest` archivo). El idioma predeterminado determina el idioma que se usa cuando los idiomas preferidos del usuario no coinciden con ninguno de los idiomas admitidos de la aplicación. Marque todos los recursos con su idioma (incluso los del idioma predeterminado, por ejemplo `\Assets\en-us\Logo.png` ) para que el sistema pueda saber en qué idioma se encuentra el recurso y cómo se usa en determinadas situaciones.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Adapte sus imágenes y otros recursos de archivo para el idioma

Idealmente, podrá globalizar sus imágenes, de manera que sean &mdash; independientes de la referencia cultural. En el caso de las imágenes y otros recursos de archivo en los que no sea posible, cree tantas variantes diferentes como necesite y coloque los calificadores de idioma adecuados en sus nombres de archivo o carpeta. Para obtener más información, consulte [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md).

Para minimizar los costos de localización, no incluya en las imágenes el texto ni el material que sea culturalmente sensible. Una imagen adecuada en su propia referencia cultural podría ser ofensiva o malinterpretada en otras referencias culturales. Evite el uso de imágenes específicas de la referencia cultural, como los buzones, que no son comunes en todo el mundo. Evite símbolos religiosos, animales, políticos o imágenes específicas de sexo. La presentación de la piel, las partes del cuerpo o los gestos de mano también puede ser un tema confidencial. Si no puede evitar todos estos, las imágenes deberán estar localizadas de una vez. Si localizas a un idioma que tiene una dirección de lectura diferente de la del tuyo, el uso de efectos e imágenes simétricas facilita la admisión de la creación de reflejos.

Evite también el uso de texto en imágenes y voz en archivos de audio y vídeo.

## <a name="the-use-of-color-in-your-app"></a>El uso de color en la aplicación

Tenga en cuentan al usar el color. El uso de combinaciones de colores que están asociadas a marcas nacionales o movimientos políticos puede ser problemático. Es posible que los expertos de la cultura deban revisar las opciones de color. También hay problemas de accesibilidad con el uso de color. Si utiliza el color para transmitir el significado, también debe transmitir esa misma información por otros medios, como el tamaño, la forma o una etiqueta.

## <a name="consider-factoring-your-strings-into-sentences"></a>Considere la posibilidad de factorizar las cadenas en frases

Use cadenas de tamaño adecuado. Las cadenas cortas son más fáciles de traducir y permiten el reciclaje de traducciones (lo que ahorra gastos porque la misma cadena no se envía al localizador más de una vez). Además, es posible que las herramientas de localización no admitan cadenas extremadamente largas.

Pero en la tensión con esta directriz se corre el riesgo de volver a usar una cadena en contextos diferentes. Incluso las palabras simples, como &quot; on &quot; y &quot; OFF &quot; , se pueden traducir de forma diferente, dependiendo del contexto. En inglés "on" y "off" pueden usarse para alternar entre el modo de vuelo, la red Bluetooth y los dispositivos. No obstante, en italiano, la traducción depende del contexto de lo que se activa o desactiva. Debes crear un par de cadenas para cada contexto. Puedes volver a usar las cadenas si dos contextos son iguales. Por ejemplo, puedes volver a usar la cadena "Volume" tanto para el volumen de los efectos de sonido como para el volumen de la música porque ambos hacen referencia a la intensidad del sonido. En cambio, no debes volver a usar la misma cadena cuando hagas referencia a "Volume" cuando se refiera al disco duro, porque el contexto y el significado son diferentes y la palabra puede traducirse de manera diferente.

Igualmente, una cadena como "text" o "fax" puede usarse tanto como verbo como sustantivo en inglés, lo que puede causar confusión en el proceso de traducción. En su lugar, crea una cadena separada para el formato de verbo y el de sustantivo. Cuando no estés seguro de si el contexto es el mismo, por si acaso usa una cadena diferente.

En Resumen, factorizar las cadenas en partes que funcionan en todos los contextos. Habrá casos en los que una cadena tendrá que ser una oración completa.

Considere la siguiente cadena: " {0} no se pudo sincronizar."

Una variedad de palabras podría reemplazar {0} , como "appointment", "Task" o "Document". Aunque este ejemplo funciona para el idioma inglés, no funcionará en todos los casos de la oración correspondiente en, por ejemplo, alemán. Ten en cuenta que en las siguientes oraciones en alemán, algunas de las palabras de la cadena de plantilla ("Der", "Die", "Das") tienen que coincidir con la palabra parametrizada:

| Inglés                                    | Alemán                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| No pudo sincronizarse la tarea.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| No pudo sincronizarse el documento.    | Das Dokument konnte nicht synchronisiert werden. |

Como otro ejemplo, considere la frase "Recordármelo en {0} minuto (s)". El uso de "minute (s)" funciona para el idioma inglés, pero otros idiomas podrían usar términos diferentes. Por ejemplo, en polaco, se usa "minuta", "minuty" o "minut" según el contexto.

Para resolver este problema, localiza toda la oración en lugar de una única palabra. Aunque hacer esto parezca demandar trabajo extra y que no sea una solución elegante, es la mejor solución porque:

- Se mostrará un mensaje gramaticalmente correcto para todos los idiomas.
- No es necesario que el traductor le pregunte en qué se reemplazarán las cadenas.
- De todos modos, no necesitarás implementar una engorrosa corrección de código cuando aparezca un problema como este después de que completes la aplicación.

## <a name="other-considerations-for-strings"></a>Otras consideraciones para las cadenas

Evite colloquialisms y metáforas en las cadenas que cree en el idioma predeterminado. El idioma específico de un grupo demográfico, como la referencia cultural y la edad, puede ser difícil de entender o traducir porque solo los usuarios de ese grupo demográfico usan ese idioma. Del mismo modo, las metáforas pueden tener sentido para una persona, pero no significar nada para otra. Por ejemplo &quot;rizo&quot; significa algo para quienes pertenecen al ámbito de la aviación, pero aquellos que no pertenecen a este ámbito no comprenden la referencia.

No uses una jerga técnica, abreviaturas ni acrónimos. Es muy poco probable que aquellas personas que no pertenecen al ámbito técnico o que provienen de otras culturas o regiones comprendan el lenguaje técnico, como también es difícil que puedan traducirlo. En las conversaciones de la vida diaria no se usan este tipo de palabras. El idioma técnico suele aparecer en los mensajes de error para identificar los problemas de hardware y software, pero debe ser necesario que las cadenas sean técnicas *solo si el usuario necesita ese nivel de información y puede realizar alguna acción o buscar a alguien que pueda*.

El uso de una voz informal o de un tono en las cadenas es una opción válida. Puede usar comentarios en el archivo de recursos predeterminado (. resw) para indicar ese propósito.

## <a name="pseudo-localization"></a>Pseudo-localización

Localizar la aplicación para descubrir cualquier problema de localizabilidad. La superlocalización es un tipo de prueba de localización o de divulgación. Genera un conjunto de recursos que no están traducidos realmente. solo tienen ese propósito. Las cadenas son aproximadamente 40% más largas que en el idioma predeterminado, por ejemplo, y tienen delimitadores para que pueda ver de un vistazo si se han truncado en la interfaz de usuario.

## <a name="deployment-considerations"></a>Consideraciones de implementación

Cuando se instala una aplicación que contiene datos de idioma localizados, es posible que solo esté disponible el idioma predeterminado para la aplicación aunque se hayan incluido inicialmente recursos para varios idiomas. Esto se debe a que el proceso de instalación está optimizado para instalar solo los recursos de idioma que coinciden con el idioma y la referencia cultural actuales del dispositivo. Por lo tanto, si el dispositivo está configurado para en-US, solo los recursos de idioma en-US se instalan con la aplicación.

> [!NOTE]
> No es posible instalar compatibilidad de idioma adicional para la aplicación después de la instalación inicial. Si cambia el idioma predeterminado después de instalar una aplicación, la aplicación continúa usando solo los recursos de idioma originales.

Si desea asegurarse de que todos los recursos de idioma están disponibles después de la instalación, cree un archivo de configuración para el paquete de aplicación que especifique que se requieren determinados recursos durante la instalación (incluidos los recursos de idioma). Esta característica de instalación optimizada se habilita automáticamente cuando se genera el. appxbundle de la aplicación durante el empaquetado. Para obtener más información, consulte [asegurarse de que los recursos se instalan en un dispositivo independientemente de si un dispositivo los requiere](/previous-versions/dn482043(v=vs.140)).

Opcionalmente, para asegurarse de que todos los recursos están instalados (no solo un subconjunto), puede deshabilitar la generación de. appxbundle al empaquetar la aplicación. Sin embargo, esto no es recomendable, ya que puede aumentar el tiempo de instalación de la aplicación.

Deshabilite la generación automática del. appxbundle estableciendo el atributo "generar lote de aplicaciones" en "Never":

1. En Visual Studio, haga clic con el botón derecho en el nombre del proyecto.
2. Seleccione **tienda**  ->  **crear paquetes de aplicaciones...**
3. En el cuadro de diálogo **crear paquetes** , seleccione **deseo crear paquetes para cargarlos en el Microsoft Store con un nuevo nombre de aplicación** y, a continuación, haga clic en **siguiente**.
4. En el cuadro de diálogo **seleccionar un nombre de aplicación** , seleccione o cree un nombre de aplicación para el paquete.
5. En el cuadro de diálogo **seleccionar y configurar paquetes** , establezca **generar agrupación de aplicaciones** en **nunca**.

## <a name="geopolitical-awareness"></a>Reconocimiento geopolítico

Evita agresiones políticas en mapas o cuando hagas referencia a regiones. Los mapas pueden incluir límites regionales o nacionales polémicos, y son una fuente frecuente de ofensa política. Ten cuidado de hacer que cualquier UI usada para seleccionar una nación haga referencia a ella como &quot;país o región&quot;. La enumeración de un territorio conflictivo en una lista con la etiqueta &quot; países como &quot; &mdash; en un formulario de dirección &mdash; podría ofender a algunos usuarios.

## <a name="language--and-region-changed-events"></a>Eventos de idioma y cambio de región

Suscríbase a los eventos que se generan cuando cambia la configuración de la región y el idioma del sistema. Haga esto para poder volver a cargar los recursos, si es necesario. Para obtener más información, consulte [Actualizar cadenas en respuesta a los eventos de cambio de valor de calificador](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) y [Actualizar imágenes en respuesta a eventos de cambio de valor de calificador](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Garantizar el orden de los parámetros correcto al dar formato a las cadenas

No asuma que todos los lenguajes expresan los parámetros en el mismo orden. Por ejemplo, considere este formato.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

La cadena de formato de este ejemplo funciona para inglés (Estados Unidos). Pero no es adecuado para alemán (Alemania), por ejemplo, donde el día y el mes se muestran en el orden inverso. Asegúrese de que el traductor conoce el intento de cada uno de los parámetros para que puedan invertir el orden de los elementos de formato en la cadena de formato (por ejemplo, " {1} {0} ") según corresponda para el lenguaje de destino.

## <a name="dont-over-localize"></a>No localizar en exceso

Envíe solo lenguaje natural a los traductores; no es lenguaje de programación ni marcado. Una `<link>` etiqueta no es un lenguaje natural. Tenga en cuenta estos ejemplos.

| No localizar este                   | Localizar esto |
|:--------------------------------------- |:-------------------------- |
| &lt;vincular los &gt; términos de uso &lt; /Link&gt;   | términos de uso               |
| &lt;vinculación de &gt; Directiva de privacidad &lt; /Link&gt; | directiva de privacidad             |

La inclusión `<link>` de la etiqueta en el archivo de recursos (. resw) significa que es probable que se traduzca. Esto representaría la etiqueta no válida. Si tiene cadenas largas que necesitan incluir marcas para mantener el contexto y garantizar la ordenación, desactívela en los comentarios qué no traducir.

## <a name="choose-an-appropriate-translation-approach"></a>Elegir un enfoque de traducción adecuado

Después de que las cadenas se separan en archivos de recursos, pueden traducirse. El momento ideal para traducir cadenas es después de que se finalizan las cadenas de un proyecto, que generalmente sucede hacia el final de éste. Puedes abordar el proceso de traducción de diferentes maneras, según el volumen de cadenas por traducir, el número de idiomas por traducir y el modo en que se realizará la traducción (como internamente en contraposición a mediante la contratación de un proveedor externo).

Ten en cuenta estas opciones.

- **Puedes traducir los archivos de recursos abriéndolos directamente en el proyecto.** Este enfoque funciona bien para un proyecto que tiene un pequeño volumen de cadenas que se deben traducir en dos o tres idiomas. Puede ser apropiado para un escenario en el que un desarrollador habla más de un idioma y quiere controlar el proceso de traducción. Este enfoque se beneficia de ser rápido, no requiere ninguna herramienta y minimiza el riesgo de que se procesen las traducciones. Pero no es escalable. En particular, los recursos de diferentes idiomas pueden perder la sincronización con facilidad, lo que causa malas experiencias de usuario y dolores de cabeza en el mantenimiento.
- **Los archivos de recursos de cadena están en formato de texto XML o ResJSON, por lo que se puede entregar para su traducción mediante cualquier editor de texto. Los archivos traducidos se copiarán de nuevo en el proyecto.** Debes tener en cuenta que, si los traductores usan este método, es posible que acaben traduciendo por accidente las etiquetas XML, pero podrán trabajar en un entorno que no sea el proyecto Microsoft Visual Studio. Puedes usar este método para aquellos proyectos que debas traducir en unos pocos idiomas. El formato XLIFF es un formato XML que se diseñó específicamente para la localización, y que es compatible con varias de herramientas y proveedores de localización. Asimismo, puedes usar el [Kit de herramientas para aplicaciones multilingües](/previous-versions/windows/apps/jj572370(v=win.10)) para crear archivos XLIFF a partir de otros archivos de recursos como, por ejemplo, .resw o .resjson.

> [!NOTE]
> La localización también podría ser necesaria para otros recursos, como imágenes y archivos de audio.

También debe tener en cuenta lo siguiente:

- **Herramientas de localización** Hay varias herramientas de localización disponibles para analizar archivos de recursos y permitir que los traductores solo editen las cadenas traducibles. Este enfoque reduce el riesgo de que un traductor edite las etiquetas XML por error, pero tiene la desventaja de introducir un nuevo proceso y herramienta al proceso de localización. Una herramienta de localización es adecuada para los proyectos con un gran volumen de cadenas, pero con un número pequeño de idiomas. Para obtener más información, consulta el artículo acerca de [Cómo usar el Kit de herramientas para aplicaciones multilingües](/previous-versions/windows/apps/jj572370(v=win.10)).
- **Proveedores de localización** Considere la posibilidad de usar un proveedor de localización si la aplicación contiene grandes cadenas que deben traducirse en un gran número de idiomas. Un proveedor de localizaciones puede darte consejo sobre las herramientas y los procesos, así como traducir tus archivos de recursos. Esta es una solución ideal, pero también la opción más costosa, y puede aumentar los tiempos de entrega para el contenido traducido.

## <a name="keep-access-keys-and-labels-consistent"></a>Mantener coherentes las claves de acceso y las etiquetas

Es todo un desafío sincronizar las claves de acceso que se usan en los métodos de accesibilidad con el modo de visualizar las claves de acceso localizadas, ya que los dos recursos de cadena se clasifican en dos secciones separadas. Asegúrate de proporcionar comentarios para la cadena de etiquetas, como: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Compatibilidad con furigana para las cadenas japonesas que se pueden ordenar

Los caracteres Kanji japoneses tienen la propiedad de tener más de una lectura (Pronunciación) en función de la palabra en la que se usan. Esto supone un problema cuando intentas ordenar objetos con nombres en japonés, como nombres de aplicaciones, archivos, canciones, etc. En el pasado, el kanji japonés tiene normalmente un orden comprensible que se conoce como XJIS. Desafortunadamente, debido a que este orden de clasificación no es fonético, no es muy útil para los usuarios.

*Furigana* soluciona este problema al permitir que el usuario o creador especifique los fonéticas para los caracteres que están usando. Si usa el siguiente procedimiento para agregar furigana al nombre de la aplicación, puede asegurarse de que se ordene en la ubicación correcta en la lista de aplicaciones. Si el nombre de la aplicación contiene caracteres kanji y no se proporciona furigana cuando el idioma de la interfaz de usuario del usuario o el criterio de ordenación se establece en japonés, Windows realiza el mejor esfuerzo para generar la pronunciación adecuada. No obstante, existe la posibilidad de que los nombres de las aplicaciones que contienen texto por leer poco común o exclusivo se ordenen conforme a una lectura más común. Por lo tanto, el procedimiento recomendado para las aplicaciones japonesas (especialmente aquellos que contienen caracteres kanji en sus nombres) es proporcionar una versión furigana del nombre de la aplicación como parte del proceso de localización en japonés.

1. Agrega "ms-resource:Appname" como nombre para mostrar del paquete y nombre para mostrar de la aplicación.
2. Crea una carpeta denominada ja-JP bajo el elemento Strings y agrega dos archivos de recursos tal como sigue:

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3. En el elemento Resources.resw de la capeta ja-JP general: agrega un recurso de cadena para el elemento Appname "希蒼"
4. En Resources. altform-msft-Phonetic. resw para los recursos en japonés furigana: agregar valor furigana para AppName "のあ"

El usuario puede buscar el nombre de la aplicación "希蒼" con el valor furigana "のあ" (NOA) y el valor fonético (mediante la función **GetPhonetic** desde el editor de métodos de entrada (IME)) "まれあお" (Mare-AO).

El método de ordenación sigue el formato del **panel de control regional**:

- En una configuración regional de usuario japonés,
  - Si se habilita furigana, "希蒼" se ordena bajo "の".
  - Si falta furigana, "希蒼" se ordena bajo "ま".
- En una configuración regional de usuario que no sea en japonés,
  - Si se habilita furigana, "希蒼" se ordena bajo "の".
  - Si falta furigana, "希蒼" se ordena bajo "漢字".

## <a name="related-topics"></a>Temas relacionados

- [Directrices sobre globalización](guidelines-and-checklist-for-globalizing-your-app.md)
- [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md)
- [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
- [Ajustar el diseño y las fuentes y admitir la escritura RTL](adjust-layout-and-fonts--and-support-rtl.md)
- [Actualizar imágenes en respuesta a eventos de cambio de valor de calificador](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Ejemplos

- [Ejemplo de recursos de aplicación y localización](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208))
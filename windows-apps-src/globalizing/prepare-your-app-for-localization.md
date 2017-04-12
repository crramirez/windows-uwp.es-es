---
author: DelfCo
Description: "Prepara la aplicación para la localizarla a otros idiomas, mercados o regiones."
title: "Preparar la aplicación para la localización"
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
label: Prepare your app for localization
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 923f6ac6ec656c668eef662cb254b2b28d445548
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="prepare-your-app-for-localization"></a>Preparar la aplicación para la localización


<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Prepara la aplicación para localizarla a otros idiomas, mercados o regiones. Antes de comenzar, asegúrate de leer el [conjunto de instrucciones que debes seguir](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="use-resource-files-and-qualifiers"></a>Usar calificadores y archivos de recurso.


Asegúrate de especificar las cadenas de interfaz de usuario de la aplicación en los archivos de recursos, en lugar de colocarlas en el código. Para obtener más información, consulta [Colocar las cadenas de UI en los recursos](put-ui-strings-into-resources.md).

Especifica imágenes y otros recursos de archivo con la etiqueta de idioma apropiada en su archivo o carpeta. Ten en cuenta que localizar imágenes, audio y vídeo demanda una importante cantidad de recursos del sistema, por lo que es mejor usar activos multimedia independientes siempre que se pueda. Para obtener más información, consulta [Cómo asignar nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

## <a name="add-contextual-comments"></a>Agregar comentarios contextuales.


Agrega comentarios de localización a los archivos de recursos de tu aplicación. Los comentarios están visibles para el localizador y deben proporcionar información contextual que lo ayude a traducir correctamente los recursos. Los comentarios deben proporcionar también información de restricción suficiente en el recurso para que la traducción no interrumpa el software. Los comentarios se pueden registrar de manera opcional mediante la herramienta Makepri.exe.

**XAML:** los archivos .resw (recursos creados en Visual Studio para aplicaciones que usan XAML) tienen un elemento de comentario. Por ejemplo:

```XML
<data name="String1">
    <value>Hello World</value>
    <comment>A greeting (This is a comment to the localizer)</comment>
</data>
```

## <a name="localize-sentences-instead-of-words"></a>Localizar oraciones en lugar de palabras.


Échale un vistazo a la siguiente cadena: "The {0} could not be synchronized".

Hay varias palabras que podrían reemplazar a {0}, como una cita, una tarea o un documento. Si bien este ejemplo funciona para el inglés, no lo hará en todos los casos para la misma oración en alemán. Ten en cuenta que en las siguientes oraciones en alemán, algunas de las palabras de la cadena de plantilla ("Der", "Die", "Das") tienen que coincidir con la palabra parametrizada:

| Inglés                                    | Alemán                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| No pudo sincronizarse la tarea.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| No pudo sincronizarse el documento.    | Das Dokument konnte nicht synchronisiert werden. |

 

Échale un vistazo a la frase de este otro ejemplo: "Remind me in {0} minute(s)." El elemento "minute(s)" funciona perfectamente en inglés, pero es más que probable que otros idiomas usen términos diferentes. Por ejemplo, en polaco, se usa "minuta", "minuty" o "minut" según el contexto.

Para resolver este problema, localiza toda la oración en lugar de una única palabra. Aunque hacer esto parezca demandar trabajo extra y que no sea una solución elegante, es la mejor solución porque:

-   Se mostrará un mensaje de error claro para todos los idiomas.
-   El localizador no tendrá que preguntar qué es lo que reemplazará las cadenas.
-   De todos modos, no necesitarás implementar una engorrosa corrección de código cuando aparezca un problema como este después de que completes la aplicación.

## <a name="ensure-the-correct-parameter-order"></a>Garantizar el orden correcto de los parámetros.


No supongas que todos los idiomas usan los parámetros en el mismo orden. Por ejemplo, en la cadena "Every %s %s", el primer %s se reemplaza con el nombre de un mes y el segundo %s se reemplaza con el día del mes. Este ejemplo funciona para el inglés, pero generará un error cuando se localice la aplicación en alemán, donde la fecha y el mes se muestran en el orden inverso.

Para resolver este problema, cambia la cadena por "Every %1 %2", para que el orden pueda intercambiarse según el idioma.

## <a name="dont-over-localize"></a>No localizar en exceso.


Localiza cadenas específicas, no etiquetas. Revisa los siguientes ejemplos:

| Cadena sobrelocalizada                   | Cadena localizada correctamente |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;términos de uso&lt;/link&gt;   | términos de uso               |
| &lt;link&gt;directiva de privacidad&lt;/link&gt; | directiva de privacidad             |

 

Incluir la etiqueta &lt;link&gt; anterior en los recursos significa que también se localizará, y solo conseguirás que se invalide. Solamente la cadena en sí debe localizarse. Generalmente, debes pensar que las etiquetas son código que debes mantener separado del texto localizable. No obstante, algunas cadenas largas deben incluir marcado para mantener el contexto y garantizar la ordenación.

## <a name="do-not-use-the-same-strings-in-dissimilar-contexts"></a>No usar las mismas cadenas en contextos diferentes.


Volver a usar una cadena probablemente parezca la mejor solución, pero puede causar problemas de localización si la misma palabra o frase puede tener contextos y significados diferentes.

Puedes volver a usar las cadenas si dos contextos son iguales. Por ejemplo, puedes volver a usar la cadena "Volume" tanto para el volumen de los efectos de sonido como para el volumen de la música porque ambos hacen referencia a la intensidad del sonido. En cambio, no debes volver a usar la misma cadena cuando hagas referencia a "Volume" cuando se refiera al disco duro, porque el contexto y el significado son diferentes y la palabra puede traducirse de manera diferente.

Otro ejemplo es el uso de las cadenas "on" y "off". En inglés "on" y "off" pueden usarse para alternar entre el modo de vuelo, la red Bluetooth y los dispositivos. No obstante, en italiano, la traducción depende del contexto de lo que se activa o desactiva. Debes crear un par de cadenas para cada contexto.

Igualmente, una cadena como "text" o "fax" puede usarse tanto como verbo como sustantivo en inglés, lo que puede causar confusión en el proceso de traducción. En su lugar, crea una cadena separada para el formato de verbo y el de sustantivo. Cuando no estés seguro de si el contexto es el mismo, por si acaso usa una cadena diferente.

## <a name="identify-resources-with-unique-attributes"></a>Identificar recursos con atributos únicos.


Los identificadores de recursos distinguen entre mayúscula y minúscula y deben ser exclusivos para cada archivo de recursos. Cuando obtienes acceso a un recurso, te recomendamos que uses el identificador de recursos y no el valor real de este. Los identificadores de recursos no cambian, pero los valores reales de los recursos cambian según el idioma.

Asegúrate de usar identificadores de recursos significativos para proporcionar contexto adicional a la traducción.

No cambies los identificadores de recursos después de que los recursos de cadena se envíen para su traducción. Los equipos de localización usan el identificador de recursos para realizar un seguimiento de las adiciones, eliminaciones y actualizaciones en los recursos. Los cambios realizados en los identificadores de recursos (también conocidos como "variaciones en los identificadores de recursos") requieren que las cadenas vuelvan a traducirse, ya que se indicará que se eliminaron y que se agregaron otras diferentes.

## <a name="choose-an-appropriate-translation-approach"></a>Elegir un enfoque de traducción apropiado.


Después de que las cadenas se separan en archivos de recursos, pueden traducirse. El momento ideal para traducir cadenas es después de que se finalizan las cadenas de un proyecto, que generalmente sucede hacia el final de éste. Puedes abordar el proceso de traducción de diferentes maneras, según el volumen de cadenas por traducir, el número de idiomas por traducir y el modo en que se realizará la traducción (como internamente en contraposición a mediante la contratación de un proveedor externo).

Ten en cuenta las siguientes opciones:

-   **Puedes traducir los archivos de recursos abriéndolos directamente en el proyecto.** Este enfoque funcionará correctamente en un proyecto que tenga un pequeño volumen de cadenas que deban traducirse en dos o tres idiomas. Puede ser apropiado para un escenario en el que un desarrollador habla más de un idioma y quiere controlar el proceso de traducción. Este enfoque es beneficioso ya que es rápido, no requiere herramientas y minimiza el riesgo de malas traducciones, pero no es escalable. En particular, los recursos de diferentes idiomas pueden perder la sincronización con facilidad, lo que causa malas experiencias de usuario y dolores de cabeza en el mantenimiento.
-   **El formato del texto de los archivos de recursos de cadena es de tipo XML o ResJSON, así que puedes traducirlos mediante cualquier editor de texto. Una vez hecho esto, los archivos que hayas traducido se copiarán en el proyecto.** Debes tener en cuenta que, si los traductores usan este método, es posible que acaben traduciendo por accidente las etiquetas XML, pero podrán trabajar en un entorno que no sea el proyecto Microsoft Visual Studio. Puedes usar este método para aquellos proyectos que debas traducir en unos pocos idiomas. El formato XLIFF es un formato XML que se diseñó específicamente para la localización, y que es compatible con varias de herramientas y proveedores de localización. Asimismo, puedes usar el [Kit de herramientas para aplicaciones multilingües](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) para crear archivos XLIFF a partir de otros archivos de recursos como, por ejemplo, .resw o .resjson.

Probablemente sea necesario realizar entregas a los localizadores en el caso de otros archivos, como los archivos de audio y vídeo. Generalmente, no recomendamos crear archivos culturalmente dependientes porque pueden ser difíciles de localizar.

Además, considera las siguientes sugerencias:

-   **Usa una herramienta de localización.** Tienes a tu disposición un gran número de herramientas de localización que puedes usar para analizar archivos de recursos y permitir que los traductores editen solamente las cadenas traducibles. Este enfoque reduce el riesgo de que un traductor edite las etiquetas XML por error, pero tiene la desventaja de introducir un nuevo proceso y herramienta al proceso de localización. Una herramienta de localización es un buen recurso para organizar proyectos que tengan un gran volumen de cadenas, pero que solo necesiten traducirse a una pequeña cantidad de idiomas. Para obtener más información, consulta el artículo acerca de [Cómo usar el Kit de herramientas para aplicaciones multilingües](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
-   **Usa un proveedor de localización.** Puedes usar un proveedor de localización si el proyecto contiene un gran volumen de cadenas y debe traducirse a muchos idiomas. Un proveedor de localizaciones puede darte consejo sobre las herramientas y los procesos, así como traducir tus archivos de recursos. Esta es una solución ideal, pero también la opción más costosa, y puede aumentar los tiempos de entrega para el contenido traducido.
-   **Mantén informados a tus localizadores.** Informa a tus localizadores acerca de qué cadenas pueden considerarse sustantivos o verbos. Explícales aquellas palabras inventadas usando herramientas de terminología. Mantén la corrección gramatical de tus cadenas, y haz que no sean ambiguas y sean lo menos técnicas posibles para evitar confusiones.

## <a name="keep-access-keys-and-labels-consistent"></a>Mantener la coherencia de las claves de acceso y las etiquetas.


Es todo un desafío sincronizar las claves de acceso que se usan en los métodos de accesibilidad con el modo de visualizar las claves de acceso localizadas, ya que los dos recursos de cadena se clasifican en dos secciones separadas. Asegúrate de proporcionar comentarios para la cadena de etiqueta, como: `Make sure that the emphasized shortcut key  is synchronized with the access key.`


## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Admitir furigana para las cadenas en japonés que pueden ordenarse.


Los caracteres en kanji japonés tienen la propiedad exclusiva de tener más de una pronunciación dependiendo de la palabra o el contexto en el que se usen. Esto supone un problema cuando intentas ordenar objetos con nombres en japonés, como nombres de aplicaciones, archivos, canciones, etc. En el pasado, el kanji japonés se solía ordenar siguiendo un orden que las máquinas pueden comprender llamado XJIS. Desafortunadamente, debido a que este orden de clasificación no es fonético, no es muy útil para los usuarios.

Furigana proporciona una solución a este problema al permitir que el usuario o creador especifique la fonética para los caracteres que está usando. Si usas el siguiente procedimiento para agregar furigana al nombre de tu aplicación, puedes asegurarte de que está ordenado en la ubicación correcta de la lista de aplicaciones. Si el nombre de tu aplicación contiene caracteres kanji y furigana no se proporciona cuando el idioma de la UI del usuario o el orden de clasificación se establece en japonés, Windows busca la mejor manera de generar la pronunciación apropiada. No obstante, existe la posibilidad de que los nombres de las aplicaciones que contienen texto por leer poco común o exclusivo se ordenen conforme a una lectura más común. Por consiguiente, el procedimiento recomendado para las aplicaciones en japonés (especialmente las que contienen caracteres Kanji en sus nombres) es proporcionar una versión en furigana del nombre de aplicación como parte del proceso de localización al japonés.

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

De esta manera, el usuario puede buscar el nombre de la aplicación "希蒼" usando tanto el valor furigana "のあ" (noa) como el valor fonético (mediante la función **GetPhonetic** del Editor de métodos de entrada [IME]) "まれあお" (mare-ao).

El método de ordenación sigue el formato del **panel de control regional**:

-   En la configuración regional del usuario en japonés:
    -   Si está habilitado el furigana, "希蒼" se ordena conforme a "の".
    -   Si falta el furigana, "希蒼" se ordena conforme a "ま".
-   En la configuración regional del usuario que no es japonés:
    -   Si está habilitado el furigana, "希蒼" se ordena conforme a "の".
    -   Si falta el furigana, "希蒼" se ordena conforme a "漢字".

## <a name="related-topics"></a>Temas relacionados


* [Directrices para globalización y localización: qué hacer y qué no hacer](guidelines-and-checklist-for-globalizing-your-app.md)
* [Colocar las cadenas de la interfaz de usuario en los recursos](put-ui-strings-into-resources.md)
* [Cómo asignar un nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
 

 




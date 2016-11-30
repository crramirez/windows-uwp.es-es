---
author: Karl-Bridge-Microsoft
Description: "Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz."
title: Especificar el idioma del reconocedor de voz
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 526493614666424089199063013b5fc72d9bc5d8
ms.openlocfilehash: 9abde4aa5489780f3ee2e9b5e7ecbdfa4dd53b93

---

# Especificar el idioma del reconocedor de voz


Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz.




**API importantes**

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251)
-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250)
-   [**Idioma**](https://msdn.microsoft.com/library/windows/apps/br206804)


Aquí se enumeran los idiomas instalados en un sistema, se identifica cuál es el idioma predeterminado y se selecciona otro idioma de reconocimiento.

**Requisitos previos:  **

Este tema se basa en [Reconocimiento de voz](speech-recognition.md).

Debes tener un conocimiento básico del reconocimiento de voz y de las restricciones del reconocimiento.

Si acabas de empezar a desarrollar aplicaciones para la Plataforma universal de Windows (UWP), consulta estos temas para familiarizarte con las tecnologías que te presentamos aquí.

-   [Crear tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Para obtener sugerencias útiles sobre el diseño de una aplicación habilitada para voz que sea útil y atractiva, consulta [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121).

## Identificar el idioma predeterminado


Un reconocedor de voz usa el idioma de voz del sistema como idioma de reconocimiento predeterminado. El usuario establece este idioma en la pantalla del dispositivo Configuración &gt; Sistema &gt; Voz &gt; Idioma de voz.

Identificamos el idioma predeterminado comprobando la propiedad estática [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; </code></pre></td>
</tr>
</tbody>
</table>
```

## Confirmar un idioma instalado


Los idiomas instalados pueden variar entre dispositivos. Debes comprobar la existencia de un idioma si dependes de él para una restricción concreta.

**Nota** Es necesario reiniciar el equipo después de instalar un nuevo paquete de idioma. Si el idioma especificado no es compatible o la instalación no ha finalizado, se genera una excepción con código de error SPERR\_NOT\_FOUND (0x8004503a).

 

Determina los idiomas admitidos en un dispositivo al comprobar una de las dos propiedades estáticas de la clase [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226):

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251): es la colección de objetos [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) que se usa con gramáticas predefinidas de dictado y búsqueda en Internet.

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250): es la colección de objetos [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) que se usa con una restricción de lista o un archivo de Especificación de gramática de reconocimiento de voz (SRGS).

## Especificar un idioma


Para especificar un idioma, pasa un objeto [**Idioma**](https://msdn.microsoft.com/library/windows/apps/br206804) en el constructor [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

Aquí se especifica "en-US" como el idioma de reconocimiento.


```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## Observaciones


Una restricción de tema puede configurarse mediante la adición de [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) a la colección [**Restricciones**](https://msdn.microsoft.com/library/windows/apps/dn653241) de [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) y, luego, llamando a [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). Un [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** se devuelve si el reconocedor no se inicializa con un idioma de tema admitido.

Una restricción de lista se configura mediante la adición de [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) a la colección [**Restricciones**](https://msdn.microsoft.com/library/windows/apps/dn653241) de [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) y, luego, llamando a [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). No se puede especificar el idioma de una lista personalizada directamente. En su lugar, la lista se procesará mediante el idioma del reconocedor.

Una gramática SRGS es un formato XML estándar abierto representado por la clase [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412). A diferencia de las listas personalizadas, puedes especificar el idioma de la gramática en el marcado SRGS. Se produce un error en [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) con un objeto [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** si no se inicializa el reconocedor en el mismo idioma que el marcado SRGS.

## Artículos relacionados

**Desarrolladores**

* [Interacciones de voz](speech-interactions.md)

**Diseñadores**

* [Directrices para el diseño de voz] (https://msdn.microsoft.com/library/windows/apps/dn596121)

**Muestras**

* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Nov16_HO1-->



---
Description: Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz.
title: Especificar el idioma del reconocedor de voz
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 200fe265390d10a12a8e1b3a1abf7cd8164238d6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258238"
---
# <a name="specify-the-speech-recognizer-language"></a>Especificar el idioma del reconocedor de voz


Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz.

> **API importantes**: [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


Aquí se enumeran los idiomas instalados en un sistema, se identifica cuál es el idioma predeterminado y se selecciona otro idioma de reconocimiento.

**Requisitos previos**

Este tema se basa en [Reconocimiento de voz](speech-recognition.md).

Debes tener un conocimiento básico del reconocimiento de voz y de las restricciones del reconocimiento.

Si acabas de empezar a desarrollar aplicaciones para la Plataforma universal de Windows (UWP), consulta estos temas para familiarizarte con las tecnologías que te presentamos aquí.

-   [Crea tu primera aplicación.](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Instrucciones para la experiencia del usuario:**

Para obtener sugerencias útiles sobre el diseño de una aplicación habilitada para voz que sea útil y atractiva, consulta [Directrices para el diseño de voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions).

## <a name="identify-the-default-language"></a>Identificar el idioma predeterminado


Un reconocedor de voz usa el idioma de voz del sistema como idioma de reconocimiento predeterminado. El usuario establece este idioma en la pantalla del dispositivo Configuración &gt; Sistema &gt; Voz &gt; Idioma de voz.

Identificamos el idioma predeterminado comprobando la propiedad estática [**SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmar un idioma instalado


Los idiomas instalados pueden variar entre dispositivos. Debes comprobar la existencia de un idioma si dependes de él para una restricción concreta.

**Tenga en cuenta**  se requiere un reinicio después de instalar un nuevo paquete de idioma. Se produce una excepción con el código de error SPERR\_\_encuentra (0x8004503a) si el idioma especificado no se admite o no ha terminado de instalarse.

 

Determina los idiomas admitidos en un dispositivo al comprobar una de las dos propiedades estáticas de la clase [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer):

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages): la colección de objetos de [**lenguaje**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) que se usan con la gramática de dictado y búsqueda web predefinida.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages): la colección de objetos de [**idioma**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) que se usa con una restricción de lista o un archivo de especificación de gramática de reconocimiento de voz (SRGS).

## <a name="specify-a-language"></a>Especificar un idioma


Para especificar un idioma, pasa un objeto [**Idioma**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) en el constructor [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).

Aquí se especifica "en-US" como el idioma de reconocimiento.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Observaciones


Una restricción de tema puede configurarse mediante la adición de [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) a la colección [**Restricciones**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) y, luego, llamando a [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Un [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** se devuelve si el reconocedor no se inicializa con un idioma de tema admitido.

Una restricción de lista se configura mediante la adición de [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) a la colección [**Restricciones**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) y, luego, llamando a [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). No se puede especificar el idioma de una lista personalizada directamente. En su lugar, la lista se procesará mediante el idioma del reconocedor.

Una gramática SRGS es un formato XML estándar abierto representado por la clase [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint). A diferencia de las listas personalizadas, puedes especificar el idioma de la gramática en el marcado SRGS. [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) produce un error con un [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** si el Reconocedor no se ha inicializado en el mismo idioma que el marcado SRGS.

## <a name="related-articles"></a>Artículos relacionados

**Desarrolladores**

* [Interacciones de voz](speech-interactions.md)

**Diseñadores**

* [Directrices para el diseño de voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**Ejemplos**

* [Ejemplo de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 





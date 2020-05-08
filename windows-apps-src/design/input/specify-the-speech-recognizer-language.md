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
ms.openlocfilehash: 9cd347b115a920c71ca1eb9b5f466adf05c69c64
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968250"
---
# <a name="specify-the-speech-recognizer-language"></a>Especificar el idioma del reconocedor de voz


Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz.

> **API importantes**: [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


Aquí se enumeran los idiomas instalados en un sistema, se identifica cuál es el idioma predeterminado y se selecciona otro idioma de reconocimiento.

**Requisitos previos:**

Este tema se basa en el [reconocimiento de voz](speech-recognition.md).

Debes tener un conocimiento básico del reconocimiento de voz y de las restricciones del reconocimiento.

Si no está familiarizado con el desarrollo de aplicaciones de aplicaciones de Windows, consulte estos temas para familiarizarse con las tecnologías que se describen aquí.

-   [Creación de la primera aplicación](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
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

**Tenga en cuenta**  que es necesario reiniciar después de instalar un nuevo paquete de idioma. Se produce una excepción con el\_código\_de error SPERR no encontrado (0x8004503a) si el idioma especificado no se admite o no ha terminado de instalarse.

 

Determina los idiomas admitidos en un dispositivo al comprobar una de las dos propiedades estáticas de la clase [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer):

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages): es la colección de objetos [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) que se usa con gramáticas predefinidas de dictado y búsqueda en Internet.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages): La colección de objetos [**Idioma**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) que se usa con una restricción de lista o un archivo de Especificación de gramática de reconocimiento de voz (SRGS).

## <a name="specify-a-language"></a>Especificar un idioma


Para especificar un idioma, pasa un objeto [**Idioma**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) en el constructor [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).

Aquí se especifica "en-US" como el idioma de reconocimiento.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Comentarios


Una restricción de tema puede configurarse mediante la adición de [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) a la colección [**Restricciones**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) y, luego, llamando a [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Un [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** se devuelve si el reconocedor no se inicializa con un idioma de tema admitido.

Una restricción de lista se configura mediante la adición de [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) a la colección [**Restricciones**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) y, luego, llamando a [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). No se puede especificar el idioma de una lista personalizada directamente. En su lugar, la lista se procesará mediante el idioma del reconocedor.

Una gramática SRGS es un formato XML estándar abierto representado por la clase [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint). A diferencia de las listas personalizadas, puedes especificar el idioma de la gramática en el marcado SRGS. Se produce un error en [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) con un objeto [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** si no se inicializa el reconocedor en el mismo idioma que el marcado SRGS.

## <a name="related-articles"></a>Artículos relacionados

**Desarrolladores**

* [Interacciones de voz](speech-interactions.md)

**Diseñadores**

* [Directrices para el diseño de voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**Muestras**

* [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 





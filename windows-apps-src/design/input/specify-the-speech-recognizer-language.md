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
ms.openlocfilehash: 7d4eb10bff1bbbda3bc10a8a1d770fecf6b4042e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173319"
---
# <a name="specify-the-speech-recognizer-language"></a>Especificar el idioma del reconocedor de voz


Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz.

> **API importantes**: [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](/uwp/api/Windows.Globalization.Language)


Aquí se enumeran los idiomas instalados en un sistema, se identifica cuál es el idioma predeterminado y se selecciona otro idioma de reconocimiento.

**Requisitos previos:**

Este tema se basa en el [reconocimiento de voz](speech-recognition.md).

Debes tener un conocimiento básico del reconocimiento de voz y de las restricciones del reconocimiento.

Si no está familiarizado con el desarrollo de aplicaciones de Windows, consulte estos temas para familiarizarse con las tecnologías que se describen aquí.

-   [Crear la primera aplicación](../../get-started/your-first-app.md)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](../../xaml-platform/events-and-routed-events-overview.md).

**Instrucciones para la experiencia del usuario:**

Para obtener sugerencias útiles sobre el diseño de una aplicación habilitada para voz que sea útil y atractiva, consulta [Directrices para el diseño de voz](./speech-interactions.md).

## <a name="identify-the-default-language"></a>Identificar el idioma predeterminado


Un reconocedor de voz usa el idioma de voz del sistema como idioma de reconocimiento predeterminado. El usuario establece este idioma en la pantalla del dispositivo Configuración &gt; Sistema &gt; Voz &gt; Idioma de voz.

Identificamos el idioma predeterminado comprobando la propiedad estática [**SystemSpeechLanguage**](/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmar un idioma instalado


Los idiomas instalados pueden variar entre dispositivos. Debes comprobar la existencia de un idioma si dependes de él para una restricción concreta.

**Nota:**    Se requiere un reinicio después de instalar un nuevo paquete de idioma. Se produce una excepción con el código de error SPERR \_ no \_ encontrado (0x8004503a) si el idioma especificado no se admite o no ha terminado de instalarse.

 

Determina los idiomas admitidos en un dispositivo al comprobar una de las dos propiedades estáticas de la clase [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer):

-   [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages): es la colección de objetos [**Language**](/uwp/api/Windows.Globalization.Language) que se usa con gramáticas predefinidas de dictado y búsqueda en Internet.

-   [**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages): La colección de objetos [**Idioma**](/uwp/api/Windows.Globalization.Language) que se usa con una restricción de lista o un archivo de Especificación de gramática de reconocimiento de voz (SRGS).

## <a name="specify-a-language"></a>Especificar un idioma


Para especificar un idioma, pasa un objeto [**Idioma**](/uwp/api/Windows.Globalization.Language) en el constructor [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).

Aquí se especifica "en-US" como el idioma de reconocimiento.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Observaciones


Una restricción de tema puede configurarse mediante la adición de [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) a la colección [**Restricciones**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) y, luego, llamando a [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Un [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** se devuelve si el reconocedor no se inicializa con un idioma de tema admitido.

Una restricción de lista se configura mediante la adición de [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) a la colección [**Restricciones**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) y, luego, llamando a [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). No se puede especificar el idioma de una lista personalizada directamente. En su lugar, la lista se procesará mediante el idioma del reconocedor.

Una gramática SRGS es un formato XML estándar abierto representado por la clase [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint). A diferencia de las listas personalizadas, puedes especificar el idioma de la gramática en el marcado SRGS. Se produce un error en [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) con un objeto [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** si no se inicializa el reconocedor en el mismo idioma que el marcado SRGS.

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de voz](speech-interactions.md)

**Muestras**

* [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 
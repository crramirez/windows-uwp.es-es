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
ms.openlocfilehash: 9e23cb9c01178640bfa1519d8df369ec76ed2a6c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593840"
---
# <a name="specify-the-speech-recognizer-language"></a>Especificar el idioma del reconocedor de voz


Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz.

> **API importantes**: [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251), [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250), [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)


Aquí se enumeran los idiomas instalados en un sistema, se identifica cuál es el idioma predeterminado y se selecciona otro idioma de reconocimiento.

**Requisitos previos:**

Este tema se basa en [Reconocimiento de voz](speech-recognition.md).

Debes tener un conocimiento básico del reconocimiento de voz y de las restricciones del reconocimiento.

Si acabas de empezar a desarrollar aplicaciones para la Plataforma universal de Windows (UWP), consulta estos temas para familiarizarte con las tecnologías que te presentamos aquí.

-   [Crea tu primera aplicación.](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Instrucciones para la experiencia de usuario:**

Para obtener sugerencias útiles sobre el diseño de una aplicación habilitada para voz que sea útil y atractiva, consulta [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <a name="identify-the-default-language"></a>Identificar el idioma predeterminado


Un reconocedor de voz usa el idioma de voz del sistema como idioma de reconocimiento predeterminado. El usuario establece este idioma en la pantalla del dispositivo Configuración &gt; Sistema &gt; Voz &gt; Idioma de voz.

Identificamos el idioma predeterminado comprobando la propiedad estática [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmar un idioma instalado


Los idiomas instalados pueden variar entre dispositivos. Debes comprobar la existencia de un idioma si dependes de él para una restricción concreta.

**Tenga en cuenta**  es necesario reiniciar después de instala un nuevo paquete de idioma. Una excepción con el código de error SPERR\_no\_FOUND (0x8004503a) se produce si el idioma especificado no es compatible o no ha terminado de instalarse.

 

Determina los idiomas admitidos en un dispositivo al comprobar una de las dos propiedades estáticas de la clase [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226):

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251): la colección de [ **lenguaje** ](https://msdn.microsoft.com/library/windows/apps/br206804) objetos usados con dictado predefinida y las gramáticas de búsqueda web.

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250): la colección de [ **lenguaje** ](https://msdn.microsoft.com/library/windows/apps/br206804) objetos usados con una restricción de la lista o un archivo de especificación de gramática de reconocimiento de voz (SRGS).

## <a name="specify-a-language"></a>Especificar un idioma


Para especificar un idioma, pasa un objeto [**Idioma**](https://msdn.microsoft.com/library/windows/apps/br206804) en el constructor [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

Aquí se especifica "en-US" como el idioma de reconocimiento.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Observaciones


Una restricción de tema puede configurarse mediante la adición de [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) a la colección [**Restricciones**](https://msdn.microsoft.com/library/windows/apps/dn653241) de [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) y, luego, llamando a [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). Un [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** se devuelve si el reconocedor no se inicializa con un idioma de tema admitido.

Una restricción de lista se configura mediante la adición de [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) a la colección [**Restricciones**](https://msdn.microsoft.com/library/windows/apps/dn653241) de [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) y, luego, llamando a [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). No se puede especificar el idioma de una lista personalizada directamente. En su lugar, la lista se procesará mediante el idioma del reconocedor.

Una gramática SRGS es un formato XML estándar abierto representado por la clase [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412). A diferencia de las listas personalizadas, puedes especificar el idioma de la gramática en el marcado SRGS. [**CompileConstraintsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn653240) se produce un error con un [ **SpeechRecognitionResultStatus** ](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** si el reconocedor no se ha inicializado en el mismo idioma que el marcado SRGS.

## <a name="related-articles"></a>Artículos relacionados

**Desarrolladores**

* [Interacciones de voz](speech-interactions.md)

**Diseñadores**

* [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Ejemplos**

* [Reconocimiento de voz y ejemplo de síntesis de voz](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





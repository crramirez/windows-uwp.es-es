---
description: Aprende a definir y usar restricciones personalizadas para el reconocimiento de voz.
title: Definir restricciones de reconocimiento personalizadas
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5cef63bab911f46e34d337957011556a0c420763
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032168"
---
# <a name="define-custom-recognition-constraints"></a>Definir restricciones de reconocimiento personalizadas

Aprende a definir y usar restricciones personalizadas para el reconocimiento de voz.

> **API importantes** : [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint), [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint), [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)

El reconocimiento de voz requiere como mínimo una restricción para definir un vocabulario reconocible. Si no se especifica ninguna restricción, se usa la gramática de dictado predefinida de las aplicaciones universales de Windows. Consulta la información sobre [Reconocimiento de voz](speech-recognition.md)

## <a name="add-constraints"></a>Incorporación de restricciones

Usa la propiedad [**SpeechRecognizer.Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) para agregar restricciones a un reconocedor de voz.

Vamos a describir los tres tipos de restricciones de reconocimiento de voz que se usan en una aplicación. (Para las restricciones de los comandos de voz de Cortana, consulte [iniciar una aplicación en primer plano con comandos de voz en Cortana](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana)).

- [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint): Una restricción basada en una gramática predefinida (de dictado o búsqueda web).
- [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint): Una restricción basada en una lista de palabras u oraciones.
- [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint): una restricción definida en un archivo de Especificación de gramática de reconocimiento de voz (SRGS).

Cada reconocedor de voz puede tener una colección de restricciones. Solo son válidas estas combinaciones de restricciones:

- Una restricción de un solo tema (dictado o búsqueda web)
- En Windows 10 Fall Creators Update (10.0.16299.15) y versiones más recientes, una restricción de un solo tema se puede combinar con una restricción de lista.
- Una combinación de restricciones de lista y/o restricciones de archivo de gramática.

> [!Important]
> Llame al método **[SpeechRecognizer. CompileConstraintsAsync](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)** para compilar las restricciones antes de iniciar el proceso de reconocimiento.

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>Especificar una gramática de búsqueda en Internet (SpeechRecognitionTopicConstraint)

Deben agregarse restricciones de tema (dictado o gramática de búsqueda en Internet) a la colección de restricciones de un reconocedor de voz.

> [!NOTE]
> Puede usar un [SpeechRecognitionListConstraint](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) junto con un [SpeechRecognitionTopicConstraint](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) para aumentar la precisión del dictado proporcionando un conjunto de palabras clave específicas del dominio que cree que es probable que se usen durante el dictado.

Aquí hemos agregado una gramática de búsqueda en Internet a la colección de restricciones.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>Especificar una restricción de lista mediante programación (SpeechRecognitionListConstraint)

Deben agregarse restricciones de lista a la colección de restricciones de un reconocedor de voz.

No olvides estas cuestiones:

- Puedes agregar varias restricciones de lista a la colección de restricciones.
- Puedes usar cualquier colección que implemente **IIterable&lt;String&gt;** para los valores de cadena.

Aquí hemos especificado mediante programación una matriz de palabras como restricción de lista y la hemos agregado a la colección de restricciones de un reconocedor de voz.

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'yes', 'no'";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>Especificar una restricción de gramática SRGS (SpeechRecognitionGrammarFileConstraint)

Deben agregarse archivos de gramática SRGS a la colección de restricciones de un reconocedor de voz.

El SRGS versión 1.0 es el lenguaje de marcado estándar del sector que se usa para crear gramáticas en formato XML para el reconocimiento de voz. Aunque las aplicaciones para Windows universales ofrecen alternativas al uso de SRGS para crear gramáticas de reconocimiento de voz, tal vez el uso de SRGS para crear gramáticas produzca los mejores resultados, sobre todo en los casos más complejos de reconocimiento de voz.

Las gramáticas SRGS ofrecen un conjunto de funciones que te ayudan a planificar la arquitectura de una interacción de voz compleja para tus aplicaciones. Por ejemplo, con las gramáticas SRGS puedes:

- Especificar el orden en el que deben decirse las palabras y las frases para que puedan reconocerse.
- Combinar palabras de varias listas y frases para que sean reconocibles.
- Enlazar a otras gramáticas.
- Asignar un peso a una palabra o frase alternativas para aumentar o disminuir la probabilidad de que se utilice con el fin de adaptarse a la entrada de voz.
- Incluir palabras o frases opcionales.
- Usar reglas especiales que ayuden a filtrar las entradas sin especificar o sin anticipar, como los fragmentos hablados aleatorios que no siguen las reglas gramaticales o el ruido de fondo.
- Usar la semántica para definir lo que significa el reconocimiento de voz para tu aplicación.
- Especificar pronunciaciones, ya sea en línea en una gramática o a través de un enlace a un léxico.

Para obtener más información sobre los elementos y atributos SRGS, consulta la [Referencia XML de la gramática SRGS](/previous-versions/office/developer/speech-technologies/hh361653(v=office.14)). Para empezar a crear una gramática SRGS, consulta [Cómo crear una gramática XML básica](/previous-versions/office/developer/speech-technologies/hh361658(v=office.14)).

No olvides estas cuestiones:

- Puedes agregar varias restricciones de archivo de gramática a la colección de restricciones.
- Usa la extensión de archivo .grxml para los documentos gramáticos basados en XML que cumplen las reglas de SRGS.

Este ejemplo usa una gramática SRGS definida en un archivo llamado srgs.grxml (que se describirá más adelante). En las propiedades del archivo, la **Acción del paquete** está establecida en **Contenido** , con **Copia en el directorio de salida** establecido en **Copiar siempre** :

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'blue background', 'green text'";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

El archivo SRGS (srgs.grxml) incluye etiquetas de interpretación semántica. Dichas etiquetas proporcionan un mecanismo para devolver datos coincidentes con la gramática a tu aplicación. Las gramáticas deben cumplir la especificación de [Interpretación semántica para el reconocimiento de voz (SISR) 1.0](https://www.w3.org/TR/semantic-interpretation/) del World Wide Web Consortium (W3C).

Aquí hemos considerado las variantes de "yes" y "no".

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>Administrar las restricciones

Después de cargar una colección de restricciones para su reconocimiento, tu aplicación puede determinar qué restricciones activar para operaciones de reconocimiento configurando la propiedad [**IsEnabled**](/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) de una restricción en **true** o **false** . El valor predeterminado es **true** .

Normalmente, es más eficiente cargar las restricciones una vez y activarlas o desactivarlas según sea necesario, en lugar de cargar, descargar y compilar las restricciones para cada operación de reconocimiento. Usa la propiedad [**IsEnabled**](/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) según sea necesario.

El hecho de reducir el número de restricciones permite limitar la cantidad de datos que el reconocedor de voz necesita para encontrar una coincidencia con una entrada de voz. Esto puede mejorar tanto el rendimiento como la precisión del reconocimiento de voz.

Decide qué restricciones activar en función de las frases que tu aplicación espera en el contexto de la actual operación de reconocimiento. Por ejemplo, si el contexto de la aplicación actual es mostrar un color, probablemente no necesitarás activar una restricción que reconozca los nombres de animales.

Para indicarle al usuario lo que puede decir, usa las propiedades [**SpeechRecognizerUIOptions.AudiblePrompt**](/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.audibleprompt) y [**SpeechRecognizerUIOptions.ExampleText**](/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.exampletext), que se establecen por medio de la propiedad [**SpeechRecognizer.UIOptions**](/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions). El hecho de preparar a los usuarios para que sepan lo que pueden decir durante la operación de reconocimiento aumenta la probabilidad de que digan una frase que coincida con una restricción activa.

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de voz](speech-interactions.md)

### <a name="samples"></a>Ejemplos

- [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)

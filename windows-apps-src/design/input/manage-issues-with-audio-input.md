---
description: Aprende a administrar los problemas con la precisión del reconocimiento de voz causados por la calidad de la entrada de audio.
title: Administrar los problemas con la entrada de audio
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5d74346aec4784cea9be2dfcef3ec2efccd8b56
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034698"
---
# <a name="manage-issues-with-audio-input"></a>Administrar los problemas con la entrada de audio


Aprende a administrar los problemas con la precisión del reconocimiento de voz causados por la calidad de la entrada de audio.

> **API importantes** : [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading), [**SpeechRecognitionAudioProblem**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem)


## <a name="assess-audio-input-quality"></a>Evaluar la calidad de entrada de audio


Cuando el reconocimiento de voz esté activado, usa el evento [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading) del reconocedor de voz para determinar si uno o más problemas de audio pueden interferir con la entrada de voz. El argumento de evento ( [**SpeechRecognitionQualityDegradingEventArgs**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs)) proporciona la propiedad [**Problem**](/uwp/api/windows.media.speechrecognition.speechrecognitionqualitydegradingeventargs.problem), que describe los problemas detectados con la entrada de audio.

El reconocimiento puede verse afectado si hay demasiado ruido de fondo, un micrófono silenciado o por el volumen o la velocidad del altavoz.

Aquí se configura un reconocedor de voz y se comienza a escuchar el evento [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading).

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
    speechRecognizer.UIOptions.ExampleText = "Ex. 'weather for London'";
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

## <a name="manage-the-speech-recognition-experience"></a>Administrar la experiencia de reconocimiento de voz


Usa la descripción que proporciona la propiedad [**Problem**](/uwp/api/windows.media.speechrecognition.speechrecognitionqualitydegradingeventargs.problem) para ayudar al usuario a mejorar las condiciones de reconocimiento.

A continuación, se crea un controlador para el evento [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading) que comprueba si hay un nivel de volumen bajo. Luego, se usa un objeto [**SpeechSynthesizer**](/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) para sugerir que el usuario hable más alto.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>Artículos relacionados


* [Interacciones de voz](speech-interactions.md)

**Muestras**
* [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 

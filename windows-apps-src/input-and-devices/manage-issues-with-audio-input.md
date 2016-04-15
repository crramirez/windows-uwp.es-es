---
Description: Aprende a administrar los problemas con la precisión del reconocimiento de voz causados por la calidad de la entrada de audio.
title: Administrar los problemas con la entrada de audio
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Administrar los problemas con la entrada de audio
template: detail.hbs
---

# Administrar los problemas con la entrada de audio


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)
-   [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)
-   [**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)

Aprende a administrar los problemas con la precisión del reconocimiento de voz causados por la calidad de la entrada de audio.


## <span id="Assess_audio-input_quality"></span><span id="assess_audio-input_quality"></span><span id="ASSESS_AUDIO-INPUT_QUALITY"></span>Evaluar la calidad de entrada de audio


Cuando el reconocimiento de voz esté activado, usa el evento [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) del reconocedor de voz para determinar si uno o más problemas de audio pueden interferir con la entrada de voz. El argumento de evento ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) proporciona la propiedad [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431), que describe los problemas detectados con la entrada de audio.

El reconocimiento puede verse afectado si hay demasiado ruido de fondo, un micrófono silenciado o por el volumen o la velocidad del altavoz.

Aquí se configura un reconocedor de voz y se comienza a escuchar el evento [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243).

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
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

## <span id="Manage_the_speech-recognition_experience"></span><span id="manage_the_speech-recognition_experience"></span><span id="MANAGE_THE_SPEECH-RECOGNITION_EXPERIENCE"></span>Administrar la experiencia de reconocimiento de voz


Usa la descripción que proporciona la propiedad [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) para ayudar al usuario a mejorar las condiciones de reconocimiento.

A continuación, se crea un controlador para el evento [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) que comprueba si hay un nivel de volumen bajo. Luego, se usa un objeto [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) para sugerir que el usuario hable más alto.

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

## <span id="related_topics"></span>Artículos relacionados


* [Interacciones de voz](speech-interactions.md)

**Muestras**
* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 






<!--HONumber=Mar16_HO1-->



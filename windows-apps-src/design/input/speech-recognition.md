---
description: Usa el reconocimiento de voz para proporcionar datos de entrada, especificar una acción o un comando y realizar tareas.
title: Reconocimiento de voz
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ad721bc64de87fc8bb1a56f687860738bebed56c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030058"
---
# <a name="speech-recognition"></a>Reconocimiento de voz


Usa el reconocimiento de voz para proporcionar datos de entrada, especificar una acción o un comando y realizar tareas.

> **API importantes** : [ **Windows. Media. SpeechRecognition**](/uwp/api/Windows.Media.SpeechRecognition)

El reconocimiento de voz incluye un tiempo de ejecución de voz, varias API de reconocimiento para programar el tiempo de ejecución, gramáticas listas para usar para el dictado y la búsqueda en Internet, y una interfaz de usuario predeterminada del sistema que ayuda a los usuarios a descubrir y usar las funciones de reconocimiento de voz.

## <a name="configure-speech-recognition"></a>Configurar el reconocimiento de voz

Para admitir el reconocimiento de voz con la aplicación, el usuario debe conectarse y habilitar un micrófono en el dispositivo y aceptar el permiso de concesión de la Directiva de privacidad de Microsoft para que la aplicación lo use.

Para preguntar automáticamente al usuario con un cuadro de diálogo del sistema que solicita permiso para acceder y usar la fuente de audio del micrófono (por ejemplo, en el [ejemplo de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis) que se muestra a continuación), solo tiene que establecer la capacidad del [dispositivo](/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) de **micrófono** en el manifiesto del paquete de la [aplicación](/uwp/schemas/appxpackage/appx-package-manifest). Para obtener más información, consulte [declaraciones de funcionalidades de aplicaciones](../../packaging/app-capability-declarations.md).

![Directiva de privacidad para el acceso al micrófono](images/speech/privacy.png)

Si el usuario hace clic en sí para conceder acceso al micrófono, se agregará la aplicación a la lista de aplicaciones aprobadas en la página Configuración-> el micrófono de > de privacidad. Sin embargo, como el usuario puede optar por desactivar esta opción en cualquier momento, debe confirmar que la aplicación tiene acceso al micrófono antes de intentar usarla.

Si también desea admitir dictado, Cortana u otros servicios de reconocimiento de voz (como una [gramática predefinida](#predefined-grammars) definida en una restricción de tema), también debe confirmar que el **reconocimiento de voz en línea** (configuración-> privacidad-> Speech) está habilitado.

Este fragmento de código muestra cómo la aplicación puede comprobar si hay un micrófono presente y si tiene permiso para usarlo.

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>Reconocer la entrada de voz

Una *restricción* define las palabras y las frases (vocabulario) que una aplicación reconoce en una entrada de voz. Las restricciones son el núcleo del reconocimiento de voz y ofrecen a la aplicación un mayor control sobre la precisión del reconocimiento de voz.

Puede usar los siguientes tipos de restricciones para reconocer la entrada de voz.

### <a name="predefined-grammars"></a>Gramáticas predefinidas

Las gramáticas predefinidas de dictado y búsqueda en Internet proporcionan a tu aplicación la funcionalidad de reconocimiento de voz sin necesidad de crear una gramática. Al usar estas gramáticas, un servicio web remoto se encarga de llevar a cabo el reconocimiento de voz y los resultados se devuelven al dispositivo.

La gramática predeterminada de dictado de texto libre tiene la capacidad de reconocer la mayoría de las palabras y frases que un usuario puede decir en un idioma en particular y está optimizada para reconocer frases cortas. La gramática de dictado predefinida se usa si no especificas una restricción para el objeto [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer). El dictado de texto libre es útil si no quieres limitar los tipos de términos que puede decir un usuario. Entre los usos típicos se incluyen la creación de notas o el dictado del contenido de un mensaje.

La gramática de búsqueda web, como una gramática de dictado, contiene un gran número de palabras y frases que puede decir un usuario. Sin embargo, está optimizada para reconocer los términos que suelen usar las personas cuando buscan en la web.

> [!NOTE]
> Debido a que las gramáticas predefinidas de dictado y búsqueda en Internet pueden ser grandes y se accede a ellas a través de Internet (no se encuentran en el dispositivo), su rendimiento puede no ser tan rápido como el de una gramática personalizada instalada en el dispositivo.     

Estas gramáticas predefinidas pueden usarse para reconocer hasta 10 segundos de entrada de voz y no requieren ningún esfuerzo de edición por su parte. Sin embargo, sí requieren una conexión a una red.

Para usar restricciones de servicio Web, la compatibilidad con la entrada y la dictado de voz debe estar habilitada en la **configuración** activando la opción "ir a la información" en  **configuración-> privacidad-> voz, entrada manuscrita y escritura** .

Aquí se muestra cómo probar si la entrada de voz está habilitada y abrir la página Configuración-> privacidad-> voz, entrada manuscrita y escritura, si no lo está.

En primer lugar, inicializamos una variable global (HResultPrivacyStatementDeclined) en el valor de HResult de 0x80045509. Vea [control de excepciones para en C \# o Visual Basic](/previous-versions/windows/apps/dn532194(v=win.10)).

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

A continuación, se detectan las excepciones estándar durante recogntion y se prueba si el valor [**HRESULT**](/uwp/api/Windows.Foundation.HResult) es igual al valor de la variable HResultPrivacyStatementDeclined. Si es así, se muestra una advertencia y `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` se llama a para abrir la página de configuración.

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
    // Open the privacy/speech, inking, and typing settings page.
    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
  }
  else
  {
    var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
    await messageDialog.ShowAsync();
  }
}
```

Vea [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint).

### <a name="programmatic-list-constraints"></a>Restricciones de lista mediante programación 

Las restricciones de lista mediante programación ofrecen un enfoque ligero para la creación de gramáticas sencillas como, por ejemplo, una lista de palabras o frases. Una restricción de lista es efectiva para reconocer frases cortas y distintas. Especificar explícitamente todas las palabras en una gramática también mejora la precisión del reconocimiento, porque el motor de reconocimiento de voz debe procesar la voz únicamente para confirmar una coincidencia. La lista también se puede actualizar mediante programación.

Una restricción de lista consta de una matriz de cadenas que representa la entrada de voz que tu aplicación aceptará para llevar a cabo una operación de reconocimiento. Puedes crear una restricción de lista en tu aplicación creando un objeto de restricción de lista de reconocimiento de voz y pasando una matriz de cadenas. A continuación, agrega ese objeto a la colección de restricciones del reconocedor. El reconocimiento se realiza correctamente cuando el reconocedor de voz reconoce cualquiera de las cadenas de la matriz.

Vea [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint).

### <a name="srgs-grammars"></a>Gramáticas de SRGS

Una gramática SRGS (Especificación de gramática de reconocimiento de voz) es un documento estático que, a diferencia de una restricción de lista mediante programación, usa el formato XML definido por [SRGS versión 1.0](https://www.w3.org/TR/speech-grammar/). Una gramática SRGS proporciona el máximo control sobre la funcionalidad de reconocimiento de voz al permitir capturar varios significados semánticos en un solo reconocimiento.

 Vea [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint).

### <a name="voice-command-constraints"></a>Restricciones de comandos de voz

Usa un archivo XML de definición de comando de voz (VCD) para definir los comandos que el usuario puede decir para iniciar acciones al activar tu aplicación. Para obtener más información, consulte [activación de una aplicación en primer plano con comandos de voz a través de Cortana](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).

Consulte [ **SpeechRecognitionVoiceCommandDefinitionConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**Nota:**  El tipo de restricción que use dependerá de la complejidad de la experiencia de reconocimiento que desee crear. Cualquier enfoque puede ser la mejor opción para una tarea de reconocimiento determinada y puedes encontrar usos para todos los tipos de restricción en tu aplicación.
Para comenzar con las restricciones, consulta [Definir restricciones de reconocimiento personalizadas](define-custom-recognition-constraints.md).

La gramática de dictado predefinida de la aplicación para Windows universal reconoce la mayoría de las palabras y frases cortas de un idioma. Se activa de manera predeterminada cuando se crea una instancia a un objeto reconocedor de voz sin restricciones personalizadas.

En este ejemplo, te mostramos cómo:

- Crear un reconocedor de voz.
- Compila las restricciones predeterminadas de la aplicación Windows universal (no se ha agregado ninguna gramática al conjunto de gramáticas del reconocedor de voz).
- Empezar a escuchar voz mediante la interfaz de usuario de reconocimiento básico y los comentarios de TTS proporcionados por el método [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync). Usa el método [**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) si no se requiere la interfaz de usuario predeterminada.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>Personalizar la interfaz de usuario de reconocimiento de voz


Cuando la aplicación intenta el reconocimiento de voz llamando a [**SpeechRecognizer.RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync), aparecen varias pantallas en el siguiente orden.

Si usas una restricción basada en una gramática predefinida (de dictado o búsqueda web):

-   La pantalla **Escucha** .
-   La pantalla **Procesando** .
-   La pantalla **Te he oído** o la pantalla de error.

Si usas una restricción basada en una lista de palabras o frases, o bien una restricción basada en un archivo de gramática SRGS:

-   La pantalla **Escucha** .
-   La pantalla **Has dicho** , si lo que el usuario ha dicho pudiera interpretarse como más de un posible resultado.
-   La pantalla **Te he oído** o la pantalla de error.

La siguiente imagen muestra un ejemplo del flujo entre pantallas de un reconocedor de voz que usa una restricción basada en un archivo de gramática SRGS. En este ejemplo, el reconocimiento de voz fue correcto.

![initial reconocimiento screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![pantalla de reconocimiento intermedio para una restricción basada en un archivo de gramática SGRS](images/speech-listening-intermediate.png)

![pantalla de reconocimiento final para una restricción basada en un archivo de gramática SGRS](images/speech-listening-complete.png)

La pantalla **Escucha** puede proporcionar ejemplos de palabras o frases que la aplicación pueda reconocer. Aquí te mostramos cómo usar las propiedades de la clase [**SpeechRecognizerUIOptions**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions) (obtenida con una llamada a la propiedad [**SpeechRecognizer.UIOptions**](/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions)) para personalizar el contenido de la pantalla **Escucha** .

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

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de voz](speech-interactions.md)

**Muestras**

* [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)

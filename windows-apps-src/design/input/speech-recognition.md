---
author: Karl-Bridge-Microsoft
Description: Use speech recognition to provide input, specify an action or command, and accomplish tasks.
title: Reconocimiento de voz
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e8abeb4cb690f39a5d9145c3975edbb5a5252765
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654524"
---
# <a name="speech-recognition"></a>Reconocimiento de voz


Usa el reconocimiento de voz para proporcionar datos de entrada, especificar una acción o un comando y realizar tareas.

> **API importantes**: [**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)

El reconocimiento de voz incluye un tiempo de ejecución de voz, varias API de reconocimiento para programar el tiempo de ejecución, gramáticas listas para usar para el dictado y la búsqueda en Internet, y una interfaz de usuario predeterminada del sistema que ayuda a los usuarios a descubrir y usar las funciones de reconocimiento de voz.


## <a name="set-up-the-audio-feed"></a>Configurar la fuente de audio


Comprueba que el dispositivo tenga un micrófono o un equivalente.

Configura la función **Micrófono** del dispositivo ([**DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/br211430)) en el [Manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474) (archivo **package.appxmanifest**) para obtener acceso a la fuente de audio del micrófono. Esto permite que la aplicación grabe audio con los micrófonos conectados.

Consulta el tema sobre [declaraciones de funcionalidades de aplicación](https://msdn.microsoft.com/library/windows/apps/mt270968).

## <a name="recognize-speech-input"></a>Reconocer la entrada de voz


Una *restricción* define las palabras y las frases (vocabulario) que una aplicación reconoce en una entrada de voz. Las restricciones son fundamentales para el reconocimiento de voz y mejoran la precisión del reconocimiento de voz de tu aplicación.

Puedes usar varios tipos de restricciones para el reconocimiento de voz:

1.  **Gramáticas predefinidas** ([**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)).

    Las gramáticas predefinidas de dictado y búsqueda en Internet proporcionan a tu aplicación la funcionalidad de reconocimiento de voz sin necesidad de crear una gramática. Al usar estas gramáticas, un servicio web remoto se encarga de llevar a cabo el reconocimiento de voz y los resultados se devuelven al dispositivo.

    La gramática predeterminada de dictado de texto libre tiene la capacidad de reconocer la mayoría de las palabras y frases que un usuario puede decir en un idioma en particular y está optimizada para reconocer frases cortas. La gramática de dictado predefinida se usa si no especificas una restricción para el objeto [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226). El dictado de texto libre es útil si no quieres limitar los tipos de términos que puede decir un usuario. Entre los usos típicos se incluyen la creación de notas o el dictado del contenido de un mensaje.

    La gramática de búsqueda web, como una gramática de dictado, contiene un gran número de palabras y frases que puede decir un usuario. Sin embargo, está optimizada para reconocer los términos que suelen usar las personas cuando buscan en la web.

    **Nota** Debido a que las gramáticas predefinidas de dictado y búsqueda en Internet pueden ser grandes y se accede a ellas a través de Internet (no se encuentran en el dispositivo), su rendimiento puede no ser tan rápido como el de una gramática personalizada instalada en el dispositivo.     

    Estas gramáticas predefinidas pueden usarse para reconocer hasta 10 segundos de entrada de voz y no requieren ningún esfuerzo de edición por su parte. Sin embargo, sí requieren una conexión a una red.

    Para usar las restricciones de servicios web, la compatibilidad del dictado y la entrada de voz deben estar habilitadas en **Configuración**, activando la opción "Conocerme" en **Configuración -> Privacidad -> Voz, entrada manuscrita y escritura por teclado**.

    Aquí se muestra cómo comprobar si la entrada de voz está habilitada y, si no lo está, cómo abrir la página Configuración -> Privacidad -> Voz, entrada manuscrita y escritura por teclado.

    En primer lugar, inicializamos una variable global (HResultPrivacyStatementDeclined) en el valor de HResult de 0x80045509. Consulta [Control de excepciones para aplicaciones de Windows en tiempo de ejecución con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

    ```csharp
    private static uint HResultPrivacyStatementDeclined = 0x80045509;
    ```

    A continuación, capturamos las excepciones estándar durante el reconocimiento y probamos si el valor [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) es igual al valor de la variable HResultPrivacyStatementDeclined. Si es así, mostramos una advertencia y llamamos a `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` para abrir la página Configuración.
    
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

2.  **Restricciones de lista mediante programación** ([**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)).

    Las restricciones de lista mediante programación ofrecen un enfoque ligero para la creación de gramáticas sencillas como, por ejemplo, una lista de palabras o frases. Una restricción de lista es efectiva para reconocer frases cortas y distintas. Especificar explícitamente todas las palabras en una gramática también mejora la precisión del reconocimiento, porque el motor de reconocimiento de voz debe procesar la voz únicamente para confirmar una coincidencia. La lista también se puede actualizar mediante programación.

    Una restricción de lista consta de una matriz de cadenas que representa la entrada de voz que tu aplicación aceptará para llevar a cabo una operación de reconocimiento. Puedes crear una restricción de lista en tu aplicación creando un objeto de restricción de lista de reconocimiento de voz y pasando una matriz de cadenas. A continuación, agrega ese objeto a la colección de restricciones del reconocedor. El reconocimiento se realiza correctamente cuando el reconocedor de voz reconoce cualquiera de las cadenas de la matriz.

3.  **Gramáticas SRGS** ([**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)).

    Una gramáticaSRGS (Especificación de gramática de reconocimiento de voz) es un documento estático que, a diferencia de una restricción de lista mediante programación, usa el formatoXML definido por [SRGS versión 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302). Una gramática SRGS proporciona el máximo control sobre la funcionalidad de reconocimiento de voz al permitir capturar varios significados semánticos en un solo reconocimiento.

4.  **Restricciones de los comandos de voz** ([**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220))

    Usa un archivo XML de definición de comando de voz (VCD) para definir los comandos que el usuario puede decir para iniciar acciones al activar tu aplicación. Para obtener más detalles, consulta [Iniciar una aplicación en primer plano con comandos de voz en Cortana (XAML)](https://msdn.microsoft.com/cortana/voicecommands/launch-a-foreground-app-with-voice-commands-in-cortana).

**Nota**  El tipo de restricción que uses dependerá de la complejidad de la funcionalidad de reconocimiento que quieras crear. Cualquier enfoque puede ser la mejor opción para una tarea de reconocimiento determinada y puedes encontrar usos para todos los tipos de restricción en tu aplicación.
Para comenzar con las restricciones, consulta [Definir restricciones de reconocimiento personalizadas](define-custom-recognition-constraints.md).

 

La gramática de dictado predefinida de la aplicación para Windows universal reconoce la mayoría de las palabras y frases cortas de un idioma. Se activa de manera predeterminada cuando se crea una instancia a un objeto reconocedor de voz sin restricciones personalizadas.

En este ejemplo, te mostramos cómo:

-   Crear un reconocedor de voz.
-   Compila las restricciones predeterminadas de la aplicación Windows universal (no se ha agregado ninguna gramática al conjunto de gramáticas del reconocedor de voz).
-   Empezar a escuchar voz mediante la interfaz de usuario de reconocimiento básico y los comentarios de TTS proporcionados por el método [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245). Usa el método [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) si no se requiere la interfaz de usuario predeterminada.

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


Cuando la aplicación intenta el reconocimiento de voz llamando a [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245), aparecen varias pantallas en el siguiente orden.

Si usas una restricción basada en una gramática predefinida (de dictado o búsqueda web):

-   La pantalla **Escucha**.
-   La pantalla **Procesando**.
-   La pantalla **Te he oído** o la pantalla de error.

Si usas una restricción basada en una lista de palabras o frases, o bien una restricción basada en un archivo de gramática SRGS:

-   La pantalla **Escucha**.
-   La pantalla **Has dicho**, si lo que el usuario ha dicho pudiera interpretarse como más de un posible resultado.
-   La pantalla **Te he oído** o la pantalla de error.

La siguiente imagen muestra un ejemplo del flujo entre pantallas de un reconocedor de voz que usa una restricción basada en un archivo de gramáticaSRGS. En este ejemplo, el reconocimiento de voz fue correcto.

![initial reconocimiento screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![pantalla de reconocimiento intermedio para una restricción basada en un archivo de gramática SGRS](images/speech-listening-intermediate.png)

![pantalla de reconocimiento final para una restricción basada en un archivo de gramática SGRS](images/speech-listening-complete.png)

La pantalla **Escucha** puede proporcionar ejemplos de palabras o frases que la aplicación pueda reconocer. Aquí te mostramos cómo usar las propiedades de la clase [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234) (obtenida con una llamada a la propiedad [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254)) para personalizar el contenido de la pantalla **Escucha**.

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


**Desarrolladores**
* [Interacciones de voz](speech-interactions.md)
**Diseñadores**
* [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121)
**Ejemplos**
* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





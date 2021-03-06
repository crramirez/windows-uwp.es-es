---
description: Obtén información sobre cómo capturar y reconocer la entrada de voz de dictado continuo de larga duración.
title: Habilitar el dictado continuo
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8fc3bd385c623ddd962c37fb27eb20712e9ac4c6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032148"
---
# <a name="continuous-dictation"></a>Dictado continuo

Obtén información sobre cómo capturar y reconocer la entrada de voz de dictado continuo de larga duración.

> **API importantes** : [**SpeechContinuousRecognitionSession**](/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession), [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)

En el [Reconocimiento de voz](speech-recognition.md) aprendiste a capturar y reconocer entradas de voz relativamente cortas usando los métodos [**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) o [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) de un objeto [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer); por ejemplo, al redactar un mensaje SMS o al realizar una pregunta.

Para las sesiones de reconocimiento de voz más largas y continuas, como el dictado o el envío de correos electrónicos, puedes usar la propiedad [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) de la clase [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) para obtener un objeto [**SpeechContinuousRecognitionSession**](/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession).

> [!NOTE]
> La compatibilidad con el lenguaje de dictado depende del [dispositivo](../devices/index.md) en el que se ejecuta la aplicación. En el caso de equipos y portátiles, solo se reconoce en-US, mientras que Xbox y teléfonos pueden reconocer todos los idiomas compatibles con el reconocimiento de voz. Para obtener más información, vea [especificar el idioma del reconocedor de voz](specify-the-speech-recognizer-language.md).

## <a name="set-up"></a>Configurar

La aplicación necesita unos pocos objetos para administrar una sesión de dictado continuo:

- Una instancia de un objeto [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).
- Una referencia a un distribuidor de interfaz de usuario, para actualizar la interfaz de usuario durante el dictado.
- Una manera de realizar el seguimiento de las palabras acumuladas que haya dicho el usuario.

En este apartado, debemos declarar una instancia [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) a modo de campo privado de la clase de código subyacente. Si deseas que el dictado continuo dure más allá de una sola página de lenguaje XAML, la aplicación necesitará almacenar una referencia en otra parte.

```CSharp
private SpeechRecognizer speechRecognizer;
```

Durante el dictado, el reconocedor genera eventos desde un subproceso en segundo plano. Dado que un subproceso en segundo plano no puede actualizar directamente la interfaz de usuario en XAML, la aplicación debe usar un distribuidor para actualizar la interfaz de usuario en respuesta a eventos de reconocimiento.

Aquí declaramos un campo privado que se inicializará más tarde con el distribuidor de la interfaz de usuario.

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

Para realizar un seguimiento de lo que dice el usuario, necesitas controlar eventos de reconocimiento generados por el reconocedor de voz. Estos eventos proporcionan los resultados del reconocimiento de fragmentos de voz del usuario.

En este apartado, usaremos un objeto [**StringBuilder**](/dotnet/api/system.text.stringbuilder) para guardar todos los resultados del reconocimiento obtenidos durante la sesión. Los resultados nuevos se anexan a **StringBuilder** cuando se procesan.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>Inicialización

Durante la inicialización del reconocimiento de voz continuo, debes:

- Obtener el distribuidor del subproceso de interfaz de usuario si actualizas la interfaz de usuario de la aplicación en los controladores de eventos de reconocimiento continuo.
- Inicializar el reconocedor de voz.
- Compilar la gramática de dictado integrada.
    **Nota**   El reconocimiento de voz requiere, como mínimo, una restricción para definir un vocabulario reconocible. Si no se especifica ninguna restricción, se usa una gramática de dictado predefinida. Consulta la información sobre [Reconocimiento de voz](speech-recognition.md)
- Configura las escuchas de eventos para eventos de reconocimiento.

En este ejemplo, inicializamos el reconocimiento de voz en el evento de página [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto).

1. Como los eventos generados por el reconocedor de voz se producen en un subproceso en segundo plano, se crea una referencia al distribuidor para efectuar actualizaciones en el subproceso de interfaz de usuario. [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) siempre se invoca en el subproceso de interfaz de usuario.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  A continuación, inicializamos la instancia [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  A continuación, agregamos y compilamos la gramática que define todas las palabras y frases que el [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)puede reconocer.

    Si no se especifica explícitamente una gramática, se usará una gramática de dictado predefinida de forma predeterminada. Por lo general, la gramática predeterminada es la mejor para el dictado general.

    A continuación, llamamos inmediatamente a [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) sin agregar una gramática.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>Controlar eventos de reconocimiento

Puede capturar una sola utterance o frase breve llamando a [**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) o [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync). 

No obstante, para capturar una sesión de reconocimiento continua y más prolongada, especificaremos escuchas de eventos para que se ejecuten en segundo plano mientras el usuario habla y definiremos los controladores para crear la cadena de dictado.

A continuación, usamos la propiedad [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) de nuestro reconocedor para obtener un objeto [**SpeechContinuousRecognitionSession**](/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession) que proporcione métodos y eventos para administrar una sesión de reconocimiento continua.

En particular, hay dos eventos que son fundamentales:

- [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated), que se crea cuando el reconocedor genera algunos resultados.
- [**Completed**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed), que se crea cuando finaliza la sesión de reconocimiento continua.

El evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) se genera a medida que el usuario habla. El reconocedor escucha continuamente al usuario y genera periódicamente un evento que pasa un fragmento de entrada de voz. Debes examinar la entrada de voz mediante la propiedad [**Result**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) del argumento del evento y realizar las acciones correspondientes en el controlador de eventos como, por ejemplo, agregar el texto a un objeto StringBuilder.

Como una instancia de [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult), la propiedad [**result**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) es útil para determinar si desea aceptar la entrada de voz. Una clase [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) proporciona dos propiedades para esto:

- [**Status**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) indica si el reconocimiento se realizó correctamente. Recuerda que el reconocimiento puede crear un error por diversos motivos.
- [**Confidence**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence) indica la confianza relativa en que el reconocedor comprendió las palabras correctas.

Estos son los pasos básicos para admitir el reconocimiento continuo:  

1. A continuación, registramos el controlador para el evento de reconocimiento continuo [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) en el evento de página [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto).
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Igualmente, comprobamos la propiedad [**Confidence**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence). Si el valor de la propiedad Confidence es [**medio**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionConfidence) o mejor, anexamos el texto a StringBuilder. También actualizaremos la interfaz de usuario a medida que recopilemos entradas.

    **Nota**  El evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) se genera en un subproceso en segundo plano que no puede actualizar la interfaz de usuario directamente. Si un controlador necesita actualizar la interfaz de usuario (como \[ hace el ejemplo de voz y TTS \] ), debe enviar las actualizaciones al subproceso de la interfaz de usuario mediante el método [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) del distribuidor.
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  A continuación, controlamos el evento [**Completed**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed), que indica el final del dictado continuo.

    La sesión finaliza cuando se llama a los métodos [**StopAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) o [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync), los cuales se describen en la sección siguiente. La sesión también puede finalizar cuando se produce un error o cuando el usuario deja de hablar. Comprueba la propiedad [**Status**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) del argumento de evento para determinar por qué finalizó la sesión ( [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)).

    A continuación, registramos el controlador para el evento de reconocimiento continuo [**Completed**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed) en el evento de página [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto).
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  El controlador de eventos comprueba la propiedad Status para determinar si el reconocimiento se realizó correctamente. También controla el caso en el que el usuario ha dejado de hablar. A menudo, el elemento [**TimeoutExceeded**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) se considera como un reconocimiento correcto, ya que significa que el usuario ha terminado de hablar. Debes controlar este caso en el código para obtener una buena experiencia.

    **Nota**  El evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) se genera en un subproceso en segundo plano que no puede actualizar la interfaz de usuario directamente. Si un controlador necesita actualizar la interfaz de usuario (como \[ hace el ejemplo de voz y TTS \] ), debe enviar las actualizaciones al subproceso de la interfaz de usuario mediante el método [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) del distribuidor.
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>Proporcionar comentarios sobre reconocimiento continuos


Cuando la gente habla, a menudo se basan en el contexto para comprender lo que se dice. Del mismo modo, el reconocedor de voz a menudo necesita contexto para proporcionar resultados de reconocimiento de alta confianza. Por ejemplo, por sí mismas, las palabras "rebelar" y "revelar" no se distinguen si no se obtiene más contexto de las palabras de alrededor. Hasta que el reconocedor tenga la certeza de que una palabra o palabras se han reconocido correctamente, no generará el evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

Esto puede provocar una experiencia que no resulte ideal para el usuario mientras continúa hablando, y no se proporcionarán resultados hasta que el reconocedor tenga la suficiente confianza para generar el evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

Controla el evento [**HypothesisGenerated**](/uwp/api/windows.media.speechrecognition.speechrecognizer.hypothesisgenerated) para mejorar esta aparente falta de capacidad de respuesta. Este evento se crea siempre que el reconocedor genera un nuevo conjunto de posibles coincidencias de la palabra que se está procesando. El argumento del evento proporciona una propiedad [**Hypothesis**](/uwp/api/windows.media.speechrecognition.speechrecognitionhypothesisgeneratedeventargs.hypothesis) que contiene las coincidencias actuales. Este muestra estas al usuario mientras continúa hablando y le asegura que el procesamiento todavía está activo. En cuanto la confianza sea alta y se haya determinado un resultado de reconocimiento, reemplaza los resultados de la propiedad **Hypothesis** provisionales por la propiedad [**Result**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) final proporcionada en el evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

En este apartado, agregamos el texto hipotético y puntos suspensivos ("...") al valor actual de la clase [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) de salida. El contenido del cuadro de texto se actualiza a medida que se generan nuevas hipótesis y hasta que se obtienen los resultados finales del evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>Iniciar y detener el reconocimiento


Antes de iniciar una sesión de reconocimiento, comprueba el valor de la propiedad [**State**](/uwp/api/windows.media.speechrecognition.speechrecognizer.state) del reconocedor de voz. El reconocedor de voz debe estar en estado [**Idle**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerState).

Después de comprobar el estado del reconocedor de voz, empezamos la sesión mediante una llamada al método [**StartAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.startasync) de la propiedad [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) del reconocedor de voz.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

El reconocimiento puede detenerse de dos maneras:

-   [**StopAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) permite completar los eventos de reconocimiento pendientes (ten en cuenta que [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) continúa generándose hasta que se completan todas las operaciones de reconocimiento pendientes).
-   [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) finaliza la sesión de reconocimiento inmediatamente y descarta los resultados pendientes.

Después de comprobar el estado del reconocedor de voz, finalizamos la sesión mediante una llamada al método [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) de la propiedad [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) del reconocedor de voz.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> Un evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) puede producirse después de una llamada a [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync).  
> Debido al multithreading, es posible que un evento [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) permanezca en la pila cuando se llame a [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync). Si es así, todavía se desencadenará el evento **ResultGenerated** .  
> Si estableces campos privados al cancelar la sesión de reconocimiento, confirma siempre sus valores en el controlador [**ResultGenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated). Por ejemplo, no des por hecho que un campo se inicializa en el controlador si estableces su valor como nulo cuando canceles la sesión.

 

## <a name="related-articles"></a>Artículos relacionados


* [Interacciones de voz](speech-interactions.md)

**Muestras**
* [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 

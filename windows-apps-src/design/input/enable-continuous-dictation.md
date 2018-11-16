---
author: Karl-Bridge-Microsoft
Description: Learn how to capture and recognize long-form, continuous dictation speech input.
title: Habilitar el dictado continuo
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ea7c0b92c5900e468023dd5b972942a89c2833c3
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6991631"
---
# <a name="continuous-dictation"></a>Dictado continuo

Obtén información sobre cómo capturar y reconocer la entrada de voz de dictado continuo de larga duración.

> **API importantes**: [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896), [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)

En el [Reconocimiento de voz](speech-recognition.md) aprendiste a capturar y reconocer entradas de voz relativamente cortas usando los métodos [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) o [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) de un objeto [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226); por ejemplo, al redactar un mensaje SMS o al realizar una pregunta.

Para las sesiones de reconocimiento de voz más largas y continuas, como el dictado o el envío de correos electrónicos, puedes usar la propiedad [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) de la clase [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) para obtener un objeto [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896).

> [!NOTE]
> Compatibilidad de idiomas de dictado depende en el [dispositivo](https://docs.microsoft.com/windows/uwp/design/devices/) donde se ejecuta la aplicación. Para PC y portátiles, se reconocen solo en-US, mientras que Xbox y teléfonos pueden reconocer todos los idiomas compatibles con el reconocimiento de voz. Para obtener más información, vea [especificar el idioma del reconocedor de voz](specify-the-speech-recognizer-language.md).

## <a name="set-up"></a>Configuración

La aplicación necesita unos pocos objetos para administrar una sesión de dictado continuo:

- Una instancia de un objeto [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).
- Una referencia a un distribuidor de interfaz de usuario, para actualizar la interfaz de usuario durante el dictado.
- Una manera de realizar el seguimiento de las palabras acumuladas que haya dicho el usuario.

En este apartado, debemos declarar una instancia [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) a modo de campo privado de la clase de código subyacente. Si deseas que el dictado continuo dure más allá de una sola página de lenguaje XAML, la aplicación necesitará almacenar una referencia en otra parte.

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

En este apartado, usaremos un objeto [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx) para guardar todos los resultados del reconocimiento obtenidos durante la sesión. Los resultados nuevos se anexan a **StringBuilder** cuando se procesan.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>Inicialización

Durante la inicialización del reconocimiento de voz continuo, debes:

- Obtener el distribuidor del subproceso de interfaz de usuario si actualizas la interfaz de usuario de la aplicación en los controladores de eventos de reconocimiento continuo.
- Inicializar el reconocedor de voz.
- Compilar la gramática de dictado integrada.
    **Nota**  el reconocimiento de voz requiere al menos una restricción para definir un vocabulario reconocible. Si no se especifica ninguna restricción, se usa una gramática de dictado predefinida. Consulta la información sobre [Reconocimiento de voz](speech-recognition.md)
- Configura las escuchas de eventos para eventos de reconocimiento.

En este ejemplo, inicializamos el reconocimiento de voz en el evento de página [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).

1. Como los eventos generados por el reconocedor de voz se producen en un subproceso en segundo plano, se crea una referencia al distribuidor para efectuar actualizaciones en el subproceso de interfaz de usuario. [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) siempre se invoca en el subproceso de interfaz de usuario.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  A continuación, inicializamos la instancia [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  Una vez hecho esto, agregamos y compilamos la gramática que define todas las palabras y frases que puede reconocer [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

    Si no se especifica explícitamente una gramática, se usará una gramática de dictado predefinida de forma predeterminada. Por lo general, la gramática predeterminada es la mejor para el dictado general.

    A continuación, llamamos inmediatamente a [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) sin agregar una gramática.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>Controlar eventos de reconocimiento

Puedes capturar una sola expresión o frase breve si llamas al método [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) o al método [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245). 

No obstante, para capturar una sesión de reconocimiento continua y más prolongada, especificaremos escuchas de eventos para que se ejecuten en segundo plano mientras el usuario habla y definiremos los controladores para crear la cadena de dictado.

A continuación, usamos la propiedad [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) de nuestro reconocedor para obtener un objeto [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) que proporcione métodos y eventos para administrar una sesión de reconocimiento continua.

En particular, hay dos eventos que son fundamentales:

- [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900), que se crea cuando el reconocedor genera algunos resultados.
- [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), que se crea cuando finaliza la sesión de reconocimiento continua.

El evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) se genera a medida que el usuario habla. El reconocedor escucha continuamente al usuario y genera periódicamente un evento que pasa un fragmento de entrada de voz. Debes examinar la entrada de voz mediante la propiedad [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) del argumento del evento y realizar las acciones correspondientes en el controlador de eventos como, por ejemplo, agregar el texto a un objeto StringBuilder.

Como instancia de [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432), la propiedad [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) es útil para determinar si quieres aceptar la entrada de voz: Una clase [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) proporciona dos propiedades para esto:

- [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) indica si el reconocimiento se realizó correctamente. Recuerda que el reconocimiento puede crear un error por diversos motivos.
- [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) indica la confianza relativa en que el reconocedor comprendió las palabras correctas.

Estos son los pasos básicos para admitir el reconocimiento continuo:  

1. A continuación, registramos el controlador para el evento de reconocimiento continuo [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) en el evento de página [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Igualmente, comprobamos la propiedad [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434). Si el valor de la propiedad Confidence es [**medio**](https://msdn.microsoft.com/library/windows/apps/dn631409) o mejor, anexamos el texto a StringBuilder. También actualizaremos la interfaz de usuario a medida que recopilemos entradas.

    **Nota**se genera el evento de [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) en un subproceso en segundo plano que no se puede actualizar directamente la interfaz de usuario. Si un controlador necesita actualizar la interfaz de usuario (igual que lo hace la opción [\Muestra de voz y TTS\]), debes enviar las actualizaciones al subproceso de interfaz de usuario a través del método [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) del distribuidor.
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

3.  A continuación, controlamos el evento [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), que indica el final del dictado continuo.

    La sesión finaliza cuando se llama a los métodos [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) o [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898), los cuales se describen en la sección siguiente. La sesión también puede finalizar cuando se produce un error o cuando el usuario deja de hablar. Comprueba la propiedad [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) del argumento de evento para determinar por qué finalizó la sesión ([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)).

    A continuación, registramos el controlador para el evento de reconocimiento continuo [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) en el evento de página [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  El controlador de eventos comprueba la propiedad Status para determinar si el reconocimiento se realizó correctamente. También controla el caso en el que el usuario ha dejado de hablar. A menudo, el elemento [**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433) se considera como un reconocimiento correcto, ya que significa que el usuario ha terminado de hablar. Debes controlar este caso en el código para obtener una buena experiencia.

    **Nota**se genera el evento de [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) en un subproceso en segundo plano que no se puede actualizar directamente la interfaz de usuario. Si un controlador necesita actualizar la interfaz de usuario (igual que lo hace la opción [\Muestra de voz y TTS\]), debes enviar las actualizaciones al subproceso de interfaz de usuario a través del método [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) del distribuidor.
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


Cuando la gente habla, a menudo se basan en el contexto para comprender lo que se dice. Del mismo modo, el reconocedor de voz a menudo necesita contexto para proporcionar resultados de reconocimiento de alta confianza. Por ejemplo, por sí mismas, las palabras "rebelar" y "revelar" no se distinguen si no se obtiene más contexto de las palabras de alrededor. Hasta que el reconocedor tenga la certeza de que una palabra o palabras se han reconocido correctamente, no generará el evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Esto puede provocar una experiencia que no resulte ideal para el usuario mientras continúa hablando, y no se proporcionarán resultados hasta que el reconocedor tenga la suficiente confianza para generar el evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Controla el evento [**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914) para mejorar esta aparente falta de capacidad de respuesta. Este evento se crea siempre que el reconocedor genera un nuevo conjunto de posibles coincidencias de la palabra que se está procesando. El argumento del evento proporciona una propiedad [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911) que contiene las coincidencias actuales. Este muestra estas al usuario mientras continúa hablando y le asegura que el procesamiento todavía está activo. En cuanto la confianza sea alta y se haya determinado un resultado de reconocimiento, reemplaza los resultados de la propiedad **Hypothesis** provisionales por la propiedad [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) final proporcionada en el evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

En este apartado, agregamos el texto hipotético y puntos suspensivos ("...") al valor actual de la clase [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) de salida. El contenido del cuadro de texto se actualiza a medida que se generan nuevas hipótesis y hasta que se obtienen los resultados finales del evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

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


Antes de iniciar una sesión de reconocimiento, comprueba el valor de la propiedad [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915) del reconocedor de voz. El reconocedor de voz debe estar en estado [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227).

Después de comprobar el estado del reconocedor de voz, empezamos la sesión mediante una llamada al método [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913901) de la propiedad [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) del reconocedor de voz.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

El reconocimiento puede detenerse de dos maneras:

-   [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) permite completar los eventos de reconocimiento pendientes (ten en cuenta que [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) continúa generándose hasta que se completan todas las operaciones de reconocimiento pendientes).
-   [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) finaliza la sesión de reconocimiento inmediatamente y descarta los resultados pendientes.

Después de comprobar el estado del reconocedor de voz, finalizamos la sesión mediante una llamada al método [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) de la propiedad [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) del reconocedor de voz.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> Puede producirse un evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) después de una llamada a [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898).  
> Debido al multithreading, es posible que un evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) permanezca en la pila cuando se llame a [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898). Si es así, todavía se desencadenará el evento **ResultGenerated**.  
> Si estableces campos privados al cancelar la sesión de reconocimiento, confirma siempre sus valores en el controlador [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900). Por ejemplo, no des por hecho que un campo se inicializa en el controlador si estableces su valor como nulo cuando canceles la sesión.

 

## <a name="related-articles"></a>Artículos relacionados


* [Interacciones de voz](speech-interactions.md)

**Muestras**
* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





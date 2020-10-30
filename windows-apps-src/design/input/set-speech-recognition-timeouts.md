---
description: Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz.
title: Establecer tiempos de espera de reconocimiento de voz
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 78c351941b1b6703c28f249afcd119cf267f3d0a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031358"
---
# <a name="set-speech-recognition-timeouts"></a>Establecer tiempos de espera de reconocimiento de voz


Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz.

> **API importantes** : [**tiempos de espera**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts), [**SpeechRecognizerTimeouts**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>Establecer un tiempo de espera


Aquí especificamos diversos valores de [**Timeouts**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts):

-   InitialSilenceTimeout: el período de tiempo en que un SpeechRecognizer detecta silencio (antes de que se han generado resultados de reconocimiento) y supone que la entrada de voz no estará disponible próximamente.
-   BabbleTimeout: el período de tiempo que un SpeechRecognizer continúa escuchando sonidos no reconocibles (hablante) antes de que se suponga que la entrada de voz ha finalizado y finaliza la operación de reconocimiento.
-   EndSilenceTimeout: el período de tiempo en que un objeto SpeechRecognizer detecta silencio (después de que se hayan generado los resultados de reconocimiento) y da por hecho que ha finalizado la entrada de voz.

**Nota**  Los tiempos de espera se pueden establecer por reconocedor.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de voz](speech-interactions.md)

**Muestras**

* [Muestra de reconocimiento de voz y síntesis de voz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)

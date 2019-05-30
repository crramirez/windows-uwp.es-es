---
Description: Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz.
title: Establecer tiempos de espera de reconocimiento de voz
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f321b9ec43f2c844854600b8260a7fdc189c0446
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365392"
---
# <a name="set-speech-recognition-timeouts"></a>Establecer tiempos de espera de reconocimiento de voz


Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz.

> **API importantes**: [**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts), [**SpeechRecognizerTimeouts**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>Establecer un tiempo de espera


Aquí especificamos diversos valores de [**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts):

-   InitialSilenceTimeout: el período de tiempo en que un SpeechRecognizer detecta silencio (antes de que se han generado resultados de reconocimiento) y supone que la entrada de voz no estará disponible próximamente.
-   BabbleTimeout: el período de tiempo que un SpeechRecognizer continúa escuchando sonidos no reconocibles (hablante) antes de que se suponga que la entrada de voz ha finalizado y finaliza la operación de reconocimiento.
-   EndSilenceTimeout: el período de tiempo en que un objeto SpeechRecognizer detecta silencio (después de que se hayan generado los resultados de reconocimiento) y da por hecho que ha finalizado la entrada de voz.

**Tenga en cuenta**  tiempos de espera se pueden establecer en una base por reconocedor.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Artículos relacionados


* [Interacciones de voz](speech-interactions.md)
**Ejemplos**
* [Reconocimiento de voz y ejemplo de síntesis de voz](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





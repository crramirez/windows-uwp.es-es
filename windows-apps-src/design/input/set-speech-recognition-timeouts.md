---
author: Karl-Bridge-Microsoft
Description: Set how long a speech recognizer ignores silence or unrecognizable sounds (babble) and continues listening for speech input.
title: Establecer tiempos de espera de reconocimiento de voz
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 00923b4448d96943cf00eade46c39c42e87c4f96
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7290797"
---
# <a name="set-speech-recognition-timeouts"></a>Establecer tiempos de espera de reconocimiento de voz


Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz.

> **API importantes**: [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253), [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)

## <a name="set-a-timeout"></a>Establecer un tiempo de espera


Aquí especificamos diversos valores de [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253):

-   InitialSilenceTimeout: el período de tiempo en que un SpeechRecognizer detecta silencio (antes de que se han generado resultados de reconocimiento) y supone que la entrada de voz no estará disponible próximamente.
-   BabbleTimeout: el período de tiempo que un SpeechRecognizer continúa escuchando sonidos no reconocibles (hablante) antes de que se suponga que la entrada de voz ha finalizado y finaliza la operación de reconocimiento.
-   EndSilenceTimeout: el período de tiempo en que un objeto SpeechRecognizer detecta silencio (después de que se hayan generado los resultados de reconocimiento) y da por hecho que ha finalizado la entrada de voz.

**Nota**tiempos de espera se pueden establecer por reconocedor.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Artículos relacionados


* [Interacciones de voz](speech-interactions.md)
**Ejemplos**
* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





---
author: Karl-Bridge-Microsoft
Description: "Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz."
title: Establecer tiempos de espera de reconocimiento de voz
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 99ead02e8886ae79b8e8423ac0f78609b4fa1f6f

---

# Establecer tiempos de espera de reconocimiento de voz
Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz.

**API importantes**

-   [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)
-   [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)


## Establecer un tiempo de espera


Aquí especificamos diversos valores de [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253):

-   InitialSilenceTimeout: el período de tiempo en que un SpeechRecognizer detecta silencio (antes de que se han generado resultados de reconocimiento) y supone que la entrada de voz no estará disponible próximamente.
-   BabbleTimeout: el período de tiempo que un SpeechRecognizer continúa escuchando sonidos no reconocibles (hablante) antes de que se suponga que la entrada de voz ha finalizado y finaliza la operación de reconocimiento.
-   EndSilenceTimeout: el período de tiempo en que un objeto SpeechRecognizer detecta silencio (después de que se hayan generado los resultados de reconocimiento) y da por hecho que ha finalizado la entrada de voz.

**Nota** Los tiempos de espera se pueden establecer por reconocedor.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## Artículos relacionados


* [Interacciones de voz](speech-interactions.md)
            
          
            **Muestras**
* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO5-->



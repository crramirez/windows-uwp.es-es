---
title: Voz, voz y conversación en Windows 10
description: En esta página se proporciona información para empezar a desarrollar aplicaciones de Windows habilitadas para voz.
ms.topic: article
ms.date: 09/12/2019
keywords: Voz en Windows 10, voz, voz, conversación, Win32 Speech Apps, aplicaciones de voz para UWP, aplicaciones de voz de WPF, WinForms Speech apps
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: d810f08a2db60309e4528167bcb4bddc95d850c6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174159"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Voz, voz y conversación en Windows 10

![Imagen del héroe de voz](images/hero-speech-composite-small.png)

La voz puede ser una forma eficaz, natural y agradable de que los usuarios puedan interactuar con las aplicaciones de Windows, complementar, o incluso reemplazar, experiencias de interacción tradicionales basadas en el mouse, teclado, toque, controlador o gestos.

Las características basadas en voz como reconocimiento de voz, dictado, síntesis de voz (también conocida como texto a voz o TTS) y asistentes de voz de conversación (como Cortana o Alexa) pueden proporcionar experiencias de usuario accesibles e inclusivas que permitan a los usuarios usar sus aplicaciones cuando otros dispositivos de entrada no sean suficientes.

En esta página se proporciona información sobre cómo los distintos marcos de desarrollo de Windows proporcionan reconocimiento de voz, síntesis de voz y soporte de conversación para desarrolladores que crean aplicaciones de Windows.

## <a name="platform-specific-documentation"></a>Documentación específica de la plataforma

:::row:::
   :::column:::
      ![Plataforma universal de Windows (UWP)](images/platform-uwp.png)

      **Plataforma universal de Windows (UWP)**

      Cree aplicaciones habilitadas para voz en la plataforma moderna para aplicaciones y juegos de Windows 10, en cualquier dispositivo de Windows (incluidos PC, teléfonos, Xbox One, HoloLens, etc.) y publíquelos en el Microsoft Store.

      [Interacciones de voz](/windows/uwp/design/input/speech-interactions)

      [Reconocimiento de voz](/windows/uwp/design/input/speech-recognition)

      [Dictado continuo](/windows/uwp/design/input/enable-continuous-dictation)

      [Síntesis de voz](/uwp/api/windows.media.speechsynthesis)

      [Agentes de conversación](/uwp/api/windows.applicationmodel.conversationalagent)

      [Comandos de voz de Cortana](/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Aplicaciones de la plataforma Win32](images/platform-win32.png)

      **Plataforma Win32**

      Desarrolle aplicaciones habilitadas para voz para escritorio de Windows y Windows Server con las herramientas, la información y los motores de ejemplo y las aplicaciones que se proporcionan aquí.

      [Microsoft Speech Platform: kit de desarrollo de software (SDK) (versión 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [Microsoft Speech SDK, versión 5.1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      Desarrolla aplicaciones y herramientas accesibles en la plataforma establecida para las aplicaciones de Windows administradas con un modelo de interfaz de usuario de XAML y .NET Framework.

      [Guía de programación de System.Speech para .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Servicios de voz de Azure](images/platform-azure-speech.png)

      **Servicios de voz de Azure**

      Diseñe, compile y pruebe sitios web accesibles con Azure Speech Services.

      [Speech to Text](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [Texto a voz](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [Traducción de voz](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [Asistentes virtuales de primera voz](/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Características heredadas**

      Versiones heredadas, en desuso o no compatibles de la tecnología de voz de Microsoft.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Agente de Microsoft](/windows/win32/lwef/microsoft-agent)

      [Kit de desarrollo de software de aplicaciones de Microsoft Speech (SASDK), versión 1,0](https://www.microsoft.com/download/details.aspx?id=2200)
   :::column-end:::
   :::column:::
      [Microsoft Speech API (SAPI) 5,3](/previous-versions/windows/desktop/ms723627(v=vs.85))

      [Microsoft Speech API (SAPI) 5,4](/previous-versions/windows/desktop/ee125663(v=vs.85))

      [Control de reconocimiento de Bing Speech](/previous-versions/bing/speech/dn434583(v=msdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Ejemplos

Descarga y ejecuta ejemplos completos de Windows que muestren varias características y funcionalidades de accesibilidad.

:::row:::
   :::column:::
      [Código de ejemplo de explorador](/samples/browse/?term=speech)

      El nuevo explorador de ejemplos (reemplaza a la galería de código de MSDN).
   :::column-end:::
   :::column:::
      [Ejemplos de Windows clásico en GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      En estos ejemplos se muestra el modelo de programación y la funcionalidad de Windows y Windows Server. 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Ejemplos de la Plataforma universal de Windows (UWP) en GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      En estos ejemplos se muestran los patrones de uso de la API para el Plataforma universal de Windows (UWP) en el Kit de desarrollo de software (SDK) de Windows para Windows 10.
   :::column-end:::
   :::column:::
      [XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery)

      Esta aplicación muestra los distintos controles XAML admitidos en el sistema Fluent Design.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vídeos

Varios vídeos que describen cómo crear aplicaciones de Windows que incorporan interacciones de voz.

:::row:::
   :::column:::
      **Cortana y la plataforma de voz en profundidad**
   :::column-end:::
   :::column:::
      **Extensibilidad de Cortana en aplicaciones universales de Windows**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Otros recursos

:::row:::
   :::column:::
      **Blogs y noticias**

      La versión más reciente del mundo de Microsoft Speech.
   :::column-end:::
   :::column:::
      **Comunidad y soporte técnico**

      Donde los desarrolladores y usuarios de Windows reúnen y aprenden juntos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [En las noticias](https://news.microsoft.com/?s=speech)

      [Blogs de voz](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Comunidad de Windows: voz](https://community.windows.com/search?q=speech)

      [Foro del desarrollador de Windows Speech](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::
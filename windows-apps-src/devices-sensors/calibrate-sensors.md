---
author: DBirtolo
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Calibrar los sensores
description: "Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales."
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: d67146cd0382032eddf8cfe1b47f9348727cb79a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="calibrate-sensors"></a>Calibrar los sensores

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales. La enumeración [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) puede ayudar a determinar un curso de acción cuando el dispositivo necesite calibración.

## <a name="when-to-calibrate-the-magnetometer"></a>Cuándo se debe calibrar el magnetómetro

La enumeración [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) tiene cuatro valores que sirven para determinar si el dispositivo donde se ejecuta tu aplicación necesita calibración. Si este es el caso, debes permitir que el usuario sepa que se necesita calibración. Pero no debes solicitarla con demasiada frecuencia. Recomendamos que el usuario reciba una solicitud de calibración cada 10 minutos como máximo.

| Valor           | Descripción                                                                                                                                                      |-----------------|-------------------|                                                                                                                                              | **Unknown**     | El controlador del sensor no puede informar de la precisión actual. Esto no necesariamente significa que el dispositivo no esté calibrado. Será la aplicación la que decida el mejor curso de acción si se devuelve el valor **Unknown**. Si la aplicación depende de una lectura de sensor precisa, quizás quieras solicitarle al usuario que calibre el dispositivo. | | **Unreliable**  | Actualmente hay un alto grado de imprecisión en el magnetómetro. La aplicación siempre debe solicitar la calibración por parte del usuario cuando este valor se devuelve primero. | | | **Approximate** | Los datos son lo suficientemente precisos para algunas aplicaciones. Una aplicación de realidad virtual puede continuar sin calibración cuando solo necesita saber si el usuario movió el dispositivo hacia arriba o abajo, o hacia la derecha o izquierda. Las aplicaciones que necesitan un encabezado absoluto; por ejemplo, una aplicación que debe saber hacia dónde te estás moviendo para poder dar direcciones, necesitan pedir la calibración. | | **High**        | Los datos son precisos. No se necesita calibración, ni siquiera para las aplicaciones que deben conocer un encabezado absoluto como, por ejemplo, las aplicaciones de navegación o de realidad aumentada. |

## <a name="how-to-calibrate-the-magnetometer"></a>Calibración del magnetómetro

En el siguiente vídeo breve se ofrece información general sobre la calibración del magnetómetro.<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/727bd0e3-9116-49c3-8af6-0b4339324b71/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">Un minuto para el desarrollador: calibración de sensores</iframe>

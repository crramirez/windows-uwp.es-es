---
author: muhsinking
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Calibrar los sensores
description: Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales.
ms.author: jken
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c02a6ec88bcce2e81637184270b5910af15430de
ms.sourcegitcommit: cceaf2206ec53a3e9155f97f44e4795a7b6a1d78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2018
ms.locfileid: "1700551"
---
# <a name="calibrate-sensors"></a>Calibrar los sensores


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales. La enumeración [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) puede ayudar a determinar un curso de acción cuando el dispositivo necesite calibración.

## <a name="when-to-calibrate-the-magnetometer"></a>Cuándo se debe calibrar el magnetómetro

La enumeración [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) tiene cuatro valores que sirven para determinar si el dispositivo donde se ejecuta tu aplicación necesita calibración. Si este es el caso, debes permitir que el usuario sepa que se necesita calibración. Pero no debes solicitarla con demasiada frecuencia. Recomendamos que el usuario reciba una solicitud de calibración cada 10 minutos como máximo.

| Valor           | Descripción    |
| ----------------- | ------------------- |
| **Desconocido**     | El controlador del sensor no puede informar la precisión actual. Esto no necesariamente significa que el dispositivo esté fuera de calibración. Será la aplicación la que decida el mejor curso de acción si se devuelve el valor **Unknown**. Si la aplicación depende de una lectura de sensor precisa, quizás quieras solicitarle al usuario que calibre el dispositivo. |
| **No confiable**  | Actualmente hay un alto grado de imprecisión en el magnetómetro. La aplicación siempre debe solicitar la calibración por parte del usuario cuando este valor se devuelve primero. |
| **Aproximado** | Los datos son lo suficientemente precisos para algunas aplicaciones. Una aplicación de realidad virtual puede continuar sin calibración cuando solo necesita saber si el usuario movió el dispositivo hacia arriba o abajo, o hacia la derecha o izquierda. Las aplicaciones que necesitan un encabezado absoluto; por ejemplo, una aplicación que debe saber hacia dónde te estás moviendo para poder dar direcciones, necesitan pedir la calibración. |
| **Alto**        | Los datos son precisos. No se necesita calibración, ni siquiera para las aplicaciones que deben conocer un encabezado absoluto como, por ejemplo, las aplicaciones de navegación o de realidad aumentada. |
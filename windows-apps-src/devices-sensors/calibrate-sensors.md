---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Calibrar los sensores
description: Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ef2c1cfbe949e5f850a494e4f1ed9c79faf6050
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168579"
---
# <a name="calibrate-sensors"></a>Calibrar los sensores


**API importantes**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**Windows. Devices. Sensors. Custom**](/uwp/api/Windows.Devices.Sensors.Custom)

Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales. La enumeración [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) puede ayudar a determinar un curso de acción cuando el dispositivo necesite calibración.

## <a name="when-to-calibrate-the-magnetometer"></a>Cuándo se debe calibrar el magnetómetro

La enumeración [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) tiene cuatro valores que sirven para determinar si el dispositivo donde se ejecuta tu aplicación necesita calibración. Si este es el caso, debes permitir que el usuario sepa que se necesita calibración. Pero no debes solicitarla con demasiada frecuencia. Recomendamos que el usuario reciba una solicitud de calibración cada 10 minutos como máximo.

| Value           | Descripción    |
| ----------------- | ------------------- |
| **Unknown**     | El controlador del sensor no pudo notificar la precisión actual. Esto no necesariamente significa que el dispositivo no esté calibrado. Será la aplicación la que decida el mejor curso de acción si se devuelve el valor **Unknown**. Si la aplicación depende de una lectura de sensor precisa, quizás quieras solicitarle al usuario que calibre el dispositivo. |
| **Poco confiables**  | Actualmente hay un alto grado de inexactitud en el magnetómetro. La aplicación siempre debe solicitar la calibración por parte del usuario cuando este valor se devuelve primero. |
| **Aproximado** | Los datos son lo suficientemente precisos para algunas aplicaciones. Una aplicación de realidad virtual puede continuar sin calibración cuando solo necesita saber si el usuario movió el dispositivo hacia arriba o abajo, o hacia la derecha o izquierda. Las aplicaciones que necesitan un encabezado absoluto; por ejemplo, una aplicación que debe saber hacia dónde te estás moviendo para poder dar direcciones, necesitan pedir la calibración. |
| **Alta**        | Los datos son precisos. No se necesita calibración, ni siquiera para las aplicaciones que deben conocer un encabezado absoluto como, por ejemplo, las aplicaciones de navegación o de realidad aumentada. |
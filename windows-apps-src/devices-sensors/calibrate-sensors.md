---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Calibrar los sensores
description: Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 21e902daac01d8ed2645625320dec27bf7805fba
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370314"
---
# <a name="calibrate-sensors"></a>Calibrar los sensores


**API importantes**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**Windows.Devices.Sensors.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

Es probable que sea necesario calibrar los sensores basados en el magnetómetro de un dispositivo, es decir, la brújula, el inclinómetro y el sensor de orientación, debido a factores ambientales. La enumeración [**MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) puede ayudar a determinar un curso de acción cuando el dispositivo necesite calibración.

## <a name="when-to-calibrate-the-magnetometer"></a>Cuándo se debe calibrar el magnetómetro

La enumeración [**MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) tiene cuatro valores que sirven para determinar si el dispositivo donde se ejecuta tu aplicación necesita calibración. Si este es el caso, debes permitir que el usuario sepa que se necesita calibración. Pero no debes solicitarla con demasiada frecuencia. Recomendamos que el usuario reciba una solicitud de calibración cada 10 minutos como máximo.

| Valor           | Descripción    |
| ----------------- | ------------------- |
| **Unknown**     | El controlador del sensor no puede informar la precisión actual. Esto no necesariamente significa que el dispositivo no esté calibrado. Será la aplicación la que decida el mejor curso de acción si se devuelve el valor **Unknown**. Si la aplicación depende de una lectura de sensor precisa, quizás quieras solicitarle al usuario que calibre el dispositivo. |
| **Unreliable**  | Actualmente hay un alto grado de imprecisión en el magnetómetro. La aplicación siempre debe solicitar la calibración por parte del usuario cuando este valor se devuelve primero. |
| **Aproximado** | Los datos son lo suficientemente precisos para algunas aplicaciones. Una aplicación de realidad virtual puede continuar sin calibración cuando solo necesita saber si el usuario movió el dispositivo hacia arriba o abajo, o hacia la derecha o izquierda. Las aplicaciones que necesitan un encabezado absoluto; por ejemplo, una aplicación que debe saber hacia dónde te estás moviendo para poder dar direcciones, necesitan pedir la calibración. |
| **Alta**        | Los datos son precisos. No se necesita calibración, ni siquiera para las aplicaciones que deben conocer un encabezado absoluto como, por ejemplo, las aplicaciones de navegación o de realidad aumentada. |
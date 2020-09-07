---
title: API experimentales
description: Obtenga información sobre cómo las API experimentales se usan en el modo piloto externamente con los SDK de Windows Insider para que los desarrolladores puedan probarlas.
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, experimental, api
ms.localizationpriority: medium
ms.openlocfilehash: 5a4813e7b4ae1e3dd16017066758aa8a35d0570a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170809"
---
# <a name="experimental-apis"></a>API experimentales

Las API experimentales están en las primeras fases de diseño y es probable que cambien a medida que los propietarios incorporen comentarios y agreguen compatibilidad con escenarios adicionales.

Estas API se usan en el modo piloto externamente con los [SDK de Windows Insider](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) para que los desarrolladores puedan probarlas y proporcionar comentarios antes de que entren a formar parte de la plataforma oficial. Mientras están en el modo piloto, no hay ninguna promesa ni compromiso de estabilidad.

## <a name="consuming-experimental-apis"></a>Uso de las API experimentales
IntelliSense te permitirá saber si una API es experimental. También recibirás una advertencia del compilador al usar una API experimental, como "... es solo con fines de evaluación y está sujeta a cambios o a su eliminación en actualizaciones futuras".

Estas advertencias ayudan a evitar que crees dependencias en las API experimentales en los proyectos de producción. Para los proyectos experimentales, puedes ignorar o deshabilitar estas advertencias.

De manera predeterminada, estas API se deshabilitan en tiempo de ejecución y, al llamarlas, se genera una excepción en tiempo de ejecución. Se trata de otra medida de seguridad para evitar dependencias involuntarias y la distribución generalizada de aplicaciones que usan API experimentales.

Para habilitar estas API con fines de experimentación, usa el [complemento de características del Portal de dispositivos Windows (WDP)](../debug-test-perf/device-portal.md) en el dispositivo de destino para habilitar la característica correspondiente a la API que desees llamar.

La documentación de una API experimental determinada depende del equipo al que pertenece.

## <a name="providing-feedback"></a>Comentarios

Si has probado una API experimental y quieres dar tu opinión, usa la categoría **Plataforma de desarrollador** en el [Centro de opiniones sobre Windows](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub).
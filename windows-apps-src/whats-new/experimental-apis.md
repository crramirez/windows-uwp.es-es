---
author: TylerMSFT
title: API experimentales
description: Conocer las API experimentales
ms.author: twhitney
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, experimental, api
ms.localizationpriority: medium
ms.openlocfilehash: fe5fa437c5a1e564be07b7277de0f190d6eab862
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7306076"
---
# <a name="experimental-apis"></a>API experimentales

Las API experimentales están en las primeras fases de diseño y es probable que cambien a medida que los propietarios incorporen comentarios y agreguen compatibilidad con escenarios adicionales.

Estas API se utilizan en el modo piloto externamente con [SDK de Windows Insider](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) para que los desarrolladores puedan probarlas y proporcionar comentarios antes de que entren a formar parte de la plataforma oficial. Mientras están en el modo piloto, no hay ninguna promesa ni compromiso de estabilidad.

## <a name="consuming-experimental-apis"></a>Uso de API experimentales
IntelliSense te permitirá saber si una API es experimental. También recibirás una advertencia del compilador al usar una API experimental, como "... es solo con fines de evaluación y está sujeta a cambios o a su eliminación en actualizaciones futuras".

Estas advertencias ayudan a evitar que crees dependencias en las API experimentales en los proyectos de producción. Para los proyectos experimentales, puedes omitir o deshabilitar estas advertencias.

De manera predeterminada, estas API se deshabilitan en tiempo de ejecución y, al llamarlas, se genera una excepción en tiempo de ejecución. Se trata de otra medida de seguridad para evitar dependencias involuntarias y la distribución generalizada de aplicaciones que usan API experimentales.

Para habilitar estas API para experimentación, usa el [complemento de características del Portal de dispositivos Windows (WDP)](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal) en el dispositivo de destino para habilitar la característica correspondiente a la API que desees llamar.

La documentación de una API experimental determinada depende del equipo al que pertenece.

## <a name="providing-feedback"></a>Opiniones

Si has probado una API experimental y deseas dar tu opinión, usa la categoría **Plataforma de desarrollador** en el [Centro de opiniones sobre Windows](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).
---
Description: Troubleshoot Microsoft Take a Test events and errors with the event viewer.
title: Soluciona problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos.
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, educación
ms.localizationpriority: medium
ms.openlocfilehash: 3193525316d085e56244d6f03da99e3e07c6539f
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "4060289"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Solucionar problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos

Puedes usar el Visor de eventos para ver los eventos y errores de Hacer un examen. Registros de eventos de Hacer un examen al recibir una solicitud de bloqueo, suscripción del dispositivo realizada con éxito, las políticas de bloqueo aplicadas correctamente y mucho más.

Para habilitar la visualización de eventos en el Visor de eventos:
1. Abre `Event Viewer`
2. Ve a  `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. Haz clic con el botón secundario del ratón en `Operational` y selecciona `Enable Log`

Para guardar los registros de eventos:
1. Haz clic con el botón secundario `Operational`
2. Haz clic `Save All Events As…`

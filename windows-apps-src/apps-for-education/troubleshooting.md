---
author: TylerMSFT
Description: Soluciona problemas de los eventos y errores de Hacer un examen de Microsoft con el Visor de eventos.
title: Soluciona problemas de los eventos de Hacer un examen de Microsoft con el Visor de eventos.
ms.openlocfilehash: 1b99b959cfdde997f7995c1bdf40d51921b2f1d5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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

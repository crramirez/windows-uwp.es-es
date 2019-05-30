---
title: Aplicaciones y dispositivos conectados (Project Rome)
description: Esta sección describe cómo usar la plataforma de sistemas remotos para descubrir dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.
ms.date: 06/08/2018
ms.topic: article
keywords: dispositivos Windows 10, uwp, conectados, los sistemas remotos, Roma, proyecto Roma
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: ae9229378f75adeb215a881bdaf955b010cd7806
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366372"
---
# <a name="connected-apps-and-devices-project-rome"></a>Aplicaciones y dispositivos conectados (Project Rome)

En esta sección se explica cómo conectar las aplicaciones en dispositivos y plataformas mediante [proyecto Roma](https://developer.microsoft.com/en-us/windows/project-rome). Para obtener información sobre cómo implementar el proyecto Roma en un escenario de multiplataforma, visite la [página principal de docs para proyecto Roma](https://docs.microsoft.com/en-us/windows/project-rome/).

La mayoría de los usuarios tiene varios dispositivos y con frecuencia comienzan una actividad en un dispositivo y la finalizan en otro. Para ello, las aplicaciones necesitan abarcar dispositivos y plataformas. Roma proyecto le permite detectar dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.

Las [API de sistemas remotos](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems) introducidas en Windows 10, versión 1607 te permiten crear aplicaciones que permiten a los usuarios iniciar una tarea en un dispositivo y finalizarla en otro. La tarea sigue siendo el punto central y los usuarios pueden hacer su trabajo en el dispositivo que resulte más cómodo. Por ejemplo, un usuario escuchar la radio del teléfono en el coche, pero, cuando llegue a casa, querrá transferir la reproducción a tu Xbox One, que está conectada a su equipo de música doméstico.

También puedes usar Project Rome para dispositivos complementarios o escenarios de control remoto. Usa las API de mensajería de servicio de aplicaciones para crear un canal de la aplicación entre dos dispositivos para enviar y recibir mensajes personalizados. Por ejemplo, puedes escribir una aplicación para el teléfono que controle la reproducción en tu televisión o una aplicación complementaria que proporcione información sobre los personajes de un programa de televisión que estés viendo a través de otra aplicación.  

Los dispositivos se pueden conectar de manera próxima a través de Bluetooth o de forma inalámbrica, o bien remotamente mediante la nube; se vinculan a través de la cuenta de Microsoft (MSA) de la persona que los usa.

Consulta la [muestra de UWP de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para ver ejemplos sobre cómo detectar el sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.

Para obtener más información sobre el proyecto Rome en general, incluidos los recursos para la integración entre plataformas, visita [aka.ms/project-rome](https://aka.ms/project-rome).

| Tema | Descripción |
|-------|-------------|
| [Iniciar una aplicación en un dispositivo remoto](launch-a-remote-app.md) | Aprende a iniciar una aplicación en un dispositivo remoto. Este tema aborda el caso de uso más simple y la instalación preliminar.  |
| [Detectar dispositivos remotos](discover-remote-devices.md)  | Obtén información sobre cómo detectar dispositivos a los que te puedas conectar. |
| [Comunicarse con un servicio de aplicaciones remoto](communicate-with-a-remote-app-service.md) | Obtén información sobre cómo interactuar con una aplicación en un dispositivo remoto. |
| [Conectar dispositivos mediante sesiones remotas](remote-sessions.md) | Cree experiencias compartidas en varios dispositivos uniéndolos en una sesión remota. |
| [Continuar la actividad del usuario, incluso en diferentes dispositivos](useractivities.md)| Ayudar a los usuarios reanudar lo que estabas haciendo en su aplicación, incluso en varios dispositivos.|
| [Procedimientos recomendados de las actividades de usuario](useractivities-best-practices.md)| Obtenga información sobre los procedimientos recomendados para crear y actualizar las actividades del usuario.|

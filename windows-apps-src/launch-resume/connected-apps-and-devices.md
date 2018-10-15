---
author: PatrickFarley
title: Aplicaciones y dispositivos conectados (Project Rome)
description: Esta sección describe cómo usar la plataforma de sistemas remotos para descubrir dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.
ms.author: pafarley
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, proyecto rome
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: d3efb7e094ce1464028dadaa14c6f0bfb3f3b214
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4615006"
---
# <a name="connected-apps-and-devices-project-rome"></a>Aplicaciones y dispositivos conectados (Project Rome)

En esta sección se explica cómo conectar aplicaciones entre dispositivos y plataformas con Project Rome. Obtén información sobre cómo detectar dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.

La mayoría de los usuarios tiene varios dispositivos y con frecuencia comienzan una actividad en un dispositivo y la finalizan en otro. Para ello, las aplicaciones necesitan abarcar dispositivos y plataformas.

Las [API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) introducidas en Windows 10, versión 1607 te permiten crear aplicaciones que permiten a los usuarios iniciar una tarea en un dispositivo y finalizarla en otro. La tarea sigue siendo el punto central y los usuarios pueden hacer su trabajo en el dispositivo que resulte más cómodo. Por ejemplo, un usuario escuchar la radio del teléfono en el coche, pero, cuando llegue a casa, querrá transferir la reproducción a tu Xbox One, que está conectada a su equipo de música doméstico.

También puedes usar Project Rome para dispositivos complementarios o escenarios de control remoto. Usa las API de mensajería de servicio de aplicaciones para crear un canal de la aplicación entre dos dispositivos para enviar y recibir mensajes personalizados. Por ejemplo, puedes escribir una aplicación para el teléfono que controle la reproducción en tu televisión o una aplicación complementaria que proporcione información sobre los personajes de un programa de televisión que estés viendo a través de otra aplicación.  

Los dispositivos se pueden conectar de manera próxima a través de Bluetooth o de forma inalámbrica, o bien remotamente mediante la nube; se vinculan a través de la cuenta de Microsoft (MSA) de la persona que los usa.

Consulta la [muestra de UWP de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para ver ejemplos sobre cómo detectar el sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.

Para obtener más información sobre el proyecto Rome en general, incluidos los recursos para la integración entre plataformas, visita [aka.ms/project-rome](https://aka.ms/project-rome).

| Tema | Descripción |
|-------|-------------|
| [Iniciar una aplicación en un dispositivo remoto](launch-a-remote-app.md) | Aprende a iniciar una aplicación en un dispositivo remoto. Este tema aborda el caso de uso más simple y la instalación preliminar.  |
| [Detectar dispositivos remotos](discover-remote-devices.md)  | Obtén información sobre cómo detectar dispositivos a los que te puedas conectar. |
| [Comunicarse con un servicio de aplicaciones remoto](communicate-with-a-remote-app-service.md) | Obtén información sobre cómo interactuar con una aplicación en un dispositivo remoto. |
| [Conectar dispositivos a través de sesiones remotas](remote-sessions.md) | Cree experiencias compartidas en varios dispositivos uniéndolos en una sesión remota. |
| [Continuar la actividad del usuario, incluso en diferentes dispositivos](useractivities.md)| Ayudar a los usuarios a reanudar lo que estaban haciendo en tu aplicación, incluso en varios dispositivos.|
| [Procedimientos recomendados de las actividades de usuario](useractivities-best-practices.md)| Obtén información sobre los procedimientos recomendados para crear y actualizar las actividades del usuario.|

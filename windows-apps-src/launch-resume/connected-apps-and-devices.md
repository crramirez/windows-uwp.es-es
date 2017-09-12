---
author: TylerMSFT
title: Aplicaciones y dispositivos conectados (Project Rome)
description: "Esta sección describe cómo usar la plataforma de sistemas remotos para descubrir dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.openlocfilehash: 29b7db48f2dbd699f9c4f674a8870fe8f8ca446d
ms.sourcegitcommit: 73c61e8e409b071365a2f6ebd89bd8a769b2a7c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2017
---
# <a name="connected-apps-and-devices-project-rome"></a>Aplicaciones y dispositivos conectados (Project Rome)

En esta sección se explica cómo conectar aplicaciones entre dispositivos y plataformas con Project Rome. Obtén información sobre cómo detectar dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.

La mayoría de los usuarios tiene varios dispositivos y con frecuencia comienzan una actividad en un dispositivo y la finalizan en otro. Para ello, las aplicaciones necesitan abarcar dispositivos y plataformas.

Las [API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) introducidas en Windows 10, versión 1607, te permiten crear aplicaciones que permiten a los usuarios iniciar una tarea en un dispositivo y finalizarla en otro. La tarea sigue siendo el punto central y los usuarios pueden hacer su trabajo en el dispositivo que resulte más cómodo. Por ejemplo, puedes escuchar la radio del teléfono en el coche, pero, cuando llegues a casa, querrás transferir la reproducción a tu Xbox One, que está conectada a tu equipo de música doméstico.

También puedes usar Project Rome para dispositivos complementarios o escenarios de control remoto. Usa las API de mensajería de aplicaciones para crear un canal de la aplicación entre dos dispositivos para enviar y recibir mensajes personalizados. Por ejemplo, puedes escribir una aplicación para el teléfono que controle la reproducción en tu televisión o una aplicación complementaria que proporcione información sobre los personajes de un programa de televisión que estés viendo en otra aplicación.  

Los dispositivos se pueden conectar de manera próxima a través de Bluetooth o de forma inalámbrica, o bien remotamente mediante la nube, y se conectan a través de la cuenta de Microsoft de la persona que los usa.

Consulta la [muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para ver ejemplos sobre cómo detectar el sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.

| Tema | Descripción |
|-------|-------------|
| [Detectar dispositivos remotos](discover-remote-devices.md)  | Obtén información sobre cómo detectar dispositivos a los que te puedas conectar. |
| [Iniciar una aplicación en un dispositivo remoto](launch-a-remote-app.md) | Aprende a iniciar una aplicación en un dispositivo remoto.  |
| [Comunicarse con un servicio de aplicaciones remoto](communicate-with-a-remote-app-service.md) | Obtén información sobre cómo interactuar con una aplicación en un dispositivo remoto. |

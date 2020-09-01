---
title: Aplicaciones y dispositivos conectados (Proyecto Roma)
description: Esta sección describe cómo usar la plataforma de sistemas remotos para descubrir dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP, dispositivos conectados, sistemas remotos, Roma, proyecto Roma
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: 08004f22575be69fff3f8d8017ea34327d9a0b7a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156039"
---
# <a name="connected-apps-and-devices-project-rome"></a>Aplicaciones y dispositivos conectados (Proyecto Roma)

En esta sección se explica cómo conectar aplicaciones entre dispositivos y plataformas mediante el [proyecto Roma](https://developer.microsoft.com/windows/project-rome). Para obtener información sobre cómo implementar el proyecto Roma en un escenario multiplataforma, visite la [Página principal de docs para el proyecto Roma](/windows/project-rome/).

La mayoría de los usuarios tienen varios dispositivos y a menudo inician una actividad en un dispositivo y los finalizan en otro. Para ello, las aplicaciones necesitan abarcar dispositivos y plataformas. Project Roma permite detectar dispositivos remotos, iniciar una aplicación en un dispositivo remoto y comunicarse con un servicio de aplicaciones en un dispositivo remoto.

Las [API de sistemas remotos](/uwp/api/Windows.System.RemoteSystems) que se introdujeron en Windows 10, versión 1607 permiten escribir aplicaciones que permiten a los usuarios iniciar una tarea en un dispositivo y finalizar en otro. La tarea sigue siendo el punto central y los usuarios pueden hacer su trabajo en el dispositivo que resulte más cómodo. Por ejemplo, un usuario podría estar escuchando la radio en su teléfono en el coche, pero cuando lleguen a casa, es posible que quiera transferir la reproducción a su Xbox One, que está enlazada al sistema estéreo de inicio.

También puede usar el proyecto Roma para dispositivos complementarios o escenarios de control remoto. Use las API de mensajería de App Service para crear un canal de aplicación entre dos dispositivos para enviar y recibir mensajes personalizados. Por ejemplo, puede escribir una aplicación para el teléfono que controle la reproducción en el televisor o una aplicación complementaria que proporcione información sobre los caracteres de un programa de TV que se está viendo en otra aplicación.  

Los dispositivos se pueden conectar proximally a través de Bluetooth e inalámbrico, o de forma remota a través de la nube; están vinculadas por el cuenta de Microsoft (MSA) de la persona que las utiliza.

Vea el [ejemplo de UWP de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para ver ejemplos de cómo detectar el sistema remoto, iniciar una aplicación en un sistema remoto y usar App Services para enviar mensajes entre aplicaciones que se ejecutan en dos sistemas.

Para obtener más información sobre el proyecto Roma en general, incluidos los recursos para la integración entre plataformas, vaya a [aka.MS/Project-Rome](https://developer.microsoft.com/windows/project-rome).

| Tema | Descripción |
|-------|-------------|
| [Iniciar una aplicación en un dispositivo remoto](launch-a-remote-app.md) | Aprende a iniciar una aplicación en un dispositivo remoto. En este tema se trata el caso de uso más sencillo y el programa de instalación preliminar.  |
| [Detectar dispositivos remotos](discover-remote-devices.md)  | Obtén información sobre cómo detectar dispositivos a los que te puedas conectar. |
| [Comunicarse con un servicio de aplicaciones remoto](communicate-with-a-remote-app-service.md) | Obtén información sobre cómo interactuar con una aplicación en un dispositivo remoto. |
| [Conectar dispositivos mediante sesiones remotas](remote-sessions.md) | Crea experiencias compartidas entre varios dispositivos uniéndolos en una sesión remota. |
| [Continuar la actividad del usuario, incluso en diferentes dispositivos](useractivities.md)| Ayude a los usuarios a reanudar lo que hacían en la aplicación, incluso en varios dispositivos.|
| [Procedimientos recomendados de actividades de usuario](useractivities-best-practices.md)| Conozca los procedimientos recomendados para crear y actualizar actividades de usuario.|
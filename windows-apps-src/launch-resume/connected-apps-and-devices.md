---
author: TylerMSFT
title: Aplicaciones y dispositivos conectados (proyecto &quot;Roma&quot;)
description: "En esta sección se describe cómo usar el proyecto &quot;Roma&quot; para detectar dispositivos conectados, iniciar una aplicación en otro dispositivo y comunicarse con una aplicación en un dispositivo remoto."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: 4f49acfd7efcb10d99f9d23884d20c0fc51e5a4a

---

# Aplicaciones y dispositivos conectados (proyecto "Roma")

En esta sección se explica cómo conectar aplicaciones en dispositivos y plataformas mediante el proyecto "Roma". Obtén información sobre cómo detectar dispositivos conectados, iniciar una aplicación en otro dispositivo y comunicarse con una aplicación en un dispositivo remoto.

La mayoría de los usuarios tiene varios dispositivos y suelen comenzar una actividad en un dispositivo y finalizarla en otro. Para ello, las aplicaciones necesitan abarcar dispositivos y plataformas.

Las [API de sistemas remotos](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems) introducidas en Windows 10, versión 1607, te permiten crear aplicaciones que permiten a los usuarios iniciar una tarea en un dispositivo y finalizarla en otro. La tarea sigue siendo el punto central y los usuarios pueden hacer su trabajo en el dispositivo que resulte más cómodo. Por ejemplo, puedes escuchar la radio del teléfono en el coche, pero, cuando llegues a casa, querrás transferir la reproducción a tu Xbox One, que está conectado a tu equipo de música en casa.

También puedes usar el proyecto "Roma" para dispositivos complementarios o escenarios de control remoto. Usa las API de mensajería de aplicaciones para crear un canal de la aplicación entre dos dispositivos para enviar y recibir mensajes personalizados. Por ejemplo, puedes escribir una aplicación para el teléfono que controle la reproducción en tu televisión o una aplicación complementaria que proporcione información sobre los personajes de un programa de televisión que estés viendo en otra aplicación.  

Los dispositivos se pueden conectar de manera próxima a través de Bluetooth o de forma inalámbrica, o bien remotamente mediante la nube, y se conectan a través de la cuenta de Microsoft de la persona que los usa.

Consulta la [muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) para ver ejemplos de cómo detectar el sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.

| Actividad remota | Descripción                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Descubrir dispositivos remotos](discover-remote-devices.md)  | Obtén información sobre cómo detectar dispositivos a los que te puedas conectar. |
| [Iniciar una aplicación en un dispositivo remoto](launch-a-remote-app.md) | Aprende a iniciar una aplicación en un dispositivo remoto.  |
| [Comunicarse con un servicio de aplicaciones remoto](communicate-with-a-remote-app-service.md) | Obtén información sobre cómo interactuar con una aplicación en un dispositivo remoto. |



<!--HONumber=Aug16_HO5-->



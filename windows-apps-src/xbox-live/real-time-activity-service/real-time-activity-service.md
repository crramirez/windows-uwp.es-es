---
title: Servicio de la actividad en tiempo real
description: Obtenga información sobre el servicio de la actividad de Xbox Live en tiempo real.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, servicio de la actividad en tiempo real.
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592090"
---
# <a name="real-time-activity-service"></a>Servicio de la actividad en tiempo real

El servicio de la actividad en tiempo real (ATR) permite que una aplicación en cualquier dispositivo para suscribirse a los datos de estado, las estadísticas de usuario y presencia. El sistema permite las suscripciones a sus datos y datos de otros usuarios en cualquier título según su configuración de privacidad. Esto permite que un flujo de información sin necesidad de sondear continuamente para obtener los datos más recientes.


## <a name="developer-scenarios"></a>Escenarios de desarrollo

Hay muchos escenarios que admite la ATR. Solo algunos de ellos se muestran aquí, pero la verdadera eficacia de ATR es los muchos escenarios que aportarán también que no hemos considerábamos todavía. Podría ayudar a definir la próxima generación de juego donde los usuarios suelen tener a mano su iPad de Apple o de Microsoft Surface mientras que interactúen con el título de la consola. ATR usa tecnología de WebSocket, por lo que los diversos tutoriales subtema incluye una visión general de la implementación mediante la API de WebSockets proporcionada por Windows.

Estos son algunos escenarios sencillos que se pueden crear para el título utilizando ATR:

-   Logros progreso de la aplicación
-   Aplicación de juegos ayuda
-   Aplicación de visor escuadrón
-   Visor de estadísticas
-   Visor de presencia


## <a name="achievements-progress-app"></a>Logros progreso de la aplicación

Los usuarios casi siempre tiene curiosidad sobre su progreso hacia ciertos logros, especialmente los logros que requieren realizar una acción de un número determinado de veces. Con el acceso en tiempo real a las estadísticas de un usuario (que se agregan en el servicio de las estadísticas de Xbox Live Player), puede presentar el progreso en tiempo real para los jugadores y sus amigos a logros e hitos, en Xbox One o en un dispositivo complementario, mientras que el usuario se trata de su título.


## <a name="game-help-app"></a>Aplicación de juegos ayuda

Como los usuarios navegan por su título, en tiempo real acceso a datos permite presentar ayuda juego bien junto a la experiencia en Xbox One o en cualquier dispositivo complementario. Usuarios podrían llegar hasta una nueva asignación, nueva pista o un desafío lucha jefe, y puede mostrar su juego ayuda complementaria vídeos generados por el usuario o generado por el desarrollador y el texto para ayudar al usuario a través de la experiencia en el título.


## <a name="squad-viewer-app"></a>Aplicación de visor escuadrón

En los juegos multijugador Co-op, un reproductor y sus compañeros de equipo funcionan conjuntamente para un objetivo compartido. Con tantos reproductores, puede resultar difícil realizar un seguimiento de la imagen más grande. Con acceso a datos en tiempo real, podría crear una aplicación complementaria que muestra los mapas de calor y mapa de alto nivel de donde podría ser la acción.


## <a name="statistics-viewer"></a>Visor de estadísticas

Si bien es típico para pensar en aplicaciones complementarias al pensar en ATR, también puede usar ATR en el título de la principal. Por ejemplo, puede usar ATR para proporcionar un Reproductor de jugadores con una pantalla de todo el mundo actual de las estadísticas de dentro del juego, quizás el Reproductor simplemente al presionar el botón de vista en el controlador mientras se encuentra en la coincidencia de varios jugadores.


## <a name="presence-viewer"></a>Visor de presencia

Cuando en una feria, resulta útil disponer de una vista actualizada de los amigos que están en línea y si está reproduciendo el mismo título. Puede suscribirse a la presencia de los amigos del usuario y mostrar los amigos que vienen en línea, si empezará a reproducir el título, todo en tiempo real.


## <a name="subscription-privacy-and-authorization"></a>Autorización y la privacidad de la suscripción

La versión más reciente de la ATR incluye comprobaciones de privacidad y el aislamiento de autorización/content. Siempre y cuando se cumplen las comprobaciones de autorización y la privacidad, la aplicación puede suscribirse a cualquier estadística que está marcada como habilitada para ATR. (Para obtener más información sobre cómo realizar un habilitado para ATR, vea estadística [registrarse para recibir notificaciones stat](register-for-stat-notifications.md). No existen limitaciones en los tipos de estadísticas que pueden realizarse ATR habilitada, es responsabilidad del desarrollador. Sin embargo, hay un límite para el *número* de estadísticas que se puede suscribir un usuario por sesión de la aplicación. Si un usuario alcanza ese límite, recibirá un error en la siguiente suscripción.


## <a name="in-this-section"></a>En esta sección

[Registrarse para recibir notificaciones de cambio stat del Reproductor](register-for-stat-notifications.md)  
Describe cómo habilitar la actividad en tiempo real (ATR) para obtener información de estado o las estadísticas.

[El servicio de la actividad en tiempo real (ATR) mediante las API de winrt de programación](programming-the-real-time-activity-service.md)  
Describe cómo programar el servicio de la actividad en tiempo real (ATR) con WinRT APIs.

[El servicio de la actividad en tiempo real (ATR) mediante la interfaz restful de programación](programming-the-real-time-activity-service.md)  
Describe cómo programar el servicio de la actividad en tiempo real (ATR) mediante la interfaz RESTful.

[Procedimientos recomendados de la actividad en tiempo real (ATR)](rta-best-practices.md)  
Procedimientos recomendados para usar el servicio de actividad en tiempo Real (ATR) de servicios de Xbox para suscribirse a los datos de estado y estadísticas de la plataforma de datos de Xbox.

---
title: Apéndice multijugador
description: Proporciona vínculos para obtener más información sobre el servicio de Xbox Live de 2015 para varios jugadores.
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 703e48c0f200c59fa6f9181905756ef834aabe4a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643390"
---
# <a name="multiplayer-appendix"></a>Apéndice multijugador

Permite que el sistema de varios jugadores en Xbox One juego y el ensamblado de jugadores en grupos. El sistema es segura, fácil de usar y flexible, lo que le permite no sólo para crear características simples de rápidamente, pero también generar características más complejas y conectar sus propios servicios.

> **Note**  
En este artículo es para uso avanzado de API.  Como punto de partida, eche un vistazo a la [API del Administrador de varios jugadores](../multiplayer-manager.md) que simplifican enormemente el desarrollo.  Comunique su madre si encuentra un escenario no admitido en el Administrador de varios jugadores.

La versión actual del sistema para varios jugadores es 2015 participan varios jugadores. Esta versión abstrae el concepto de "entidad juego" y usa el directorio de sesión de varios jugadores (MPSD) para controlar las sesiones de juego.

> **Note**  
La versión anterior del sistema para varios jugadores es 2014 participan varios jugadores. Esta versión se basa en el concepto de la entidad de juego y participación en juegos a través de las partes. Esta versión está en desuso, aunque todavía se proporciona código fuente con el XDK. La documentación de varios jugadores 2014 ya no se incluye con el XDK. Si necesita esta documentación, use una versión de 2014 del XDK.


## <a name="in-this-section"></a>En esta sección

[Introducción](introduction-to-the-multiplayer-system.md)  
Presenta el sistema de varios jugadores.

[Directorio de sesión de varios jugadores (mpsd)](multiplayer-session-directory.md)  
Describe el directorio de sesión de varios jugadores (MPSD), que centraliza la creación de metadatos de un juego multijugador API a través de todos los títulos y administra las sesiones de juego.

[Detalles de la sesión MPSD](mpsd-session-details.md)  
Proporciona detalles de una sesión MPSD.

[Problemas comunes al adaptar los títulos para varios jugadores de 2015](common-issues-when-adapting-multiplayer.md)  
Describe los problemas comunes que debe considerar al adaptar títulos para que funcione con varios jugadores de 2015.

[SmartMatch contactos](smartmatch-matchmaking.md)  
Describe el sistema de contactos usando Xbox Live.

[Migrar un árbitro](migrating-an-arbiter.md)  
Describe el proceso MPSD para la migración de arbiter.

[Uso de contactos SmartMatch](using-smartmatch-matchmaking.md)  
Describe cómo usar SmartMatch contactos.

[Cómo multijugador del](multiplayer-how-tos.md)  
Proporciona procedimientos para usar 2015 participan varios jugadores en títulos.

[Códigos de estado de sesión de varios jugadores](multiplayer-session-status-codes.md)  
Define códigos de estado de sesión de varios jugadores de Xbox One.

[Preguntas más frecuentes de 2015 para varios jugadores y solución de problemas](multiplayer-2015-faq.md)  
Define las preguntas más frecuentes y solución de problemas de varios jugadores.

[Directorio de sesión de varios jugadores Xbox One](xbox-one-multiplayer-session-directory.md) proporciona información general de creación de sesiones de varios jugadores con el nuevo servicio Xbox uno multijugador sesión directorio (MPSD).

[Flujos para varios jugadores invita](flows-for-multiplayer-game-invites.md) describe el flujo de experiencias implicados en la invitación de otro reproductor a un juego multijugador.

[Sesión de juego y juego de terceros visibilidad y joinability](game-session-and-game-party-visibility-and-joinability.md) se describen las diferencias entre visibilidad y joinability referente a una sesión de varios jugadores.

---
title: Unirse a un torneo
description: Obtenga información sobre cómo crear una interfaz de jugadores se unan a torneos tu juego.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneo, experiencia de usuario
ms.localizationpriority: medium
ms.openlocfilehash: 1cdcfc7243463a35eccfaeb13a3b9b92b616952b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613510"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>Unir un torneo mediante la interfaz de usuario de Arena

Las API de Arena puede proporcionar el título con datos sobre el registro, en el repositorio e información del equipo, pero se *no* proporcionan la funcionalidad para *ejecutar* las tareas relacionadas. Para usar la interfaz de usuario de Arena o un organizador de torneo de terceros (TO), o para crear su propio soporte de administración de torneo, se espera su título.

## <a name="xbox-arena-ui-team-formation"></a>Xbox Arena UI: Entrenamiento de equipos

La interfaz de usuario de Arena proporciona un método para jugadores al formulario o una combinación de un equipo. No hay ningún requisito para el título.

###### <a name="ui-example-create-a-team"></a>Ejemplo de la interfaz de usuario: Creación de un equipo

![Formar una pantalla de equipo](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>Al formar un equipo, hacer un jugador lo siguiente:

* Enviar invitaciones a unir a amigos o los miembros del club.
* Buscar a los miembros del equipo mediante la publicación de un anuncio LFG.
* Registrar o anular el registro de un equipo.
* Quitar a un miembro de un equipo.

## <a name="xbox-arena-ui-registration"></a>Xbox Arena UI: Registro

La interfaz de usuario de Arena proporciona un método para jugadores registrar un equipo. No hay ningún requisito para el título.

###### <a name="ui-example-register-a-team"></a>Ejemplo de la interfaz de usuario: Registrar un equipo

![Registrar una pantalla de equipo](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>Al registrarse para un torneo, un usuario puede:

* Anular el registro de un torneo *antes* ha cerrado el registro.
* Anular el registro de un equipo en el repositorio o cuando el torneo está en curso.
* Anular el registro de un equipo completo. *Tenga en cuenta que un usuario individual, dejando un equipo anula el registro de todo el equipo.*
* Registrar como un capitán.
* Comprender los requisitos de torneo y reglas de contratación antes de registrar.
* Recibir una notificación que el registro se realizó correctamente.
* Ver el estado de torneo cambia a "registrado" en tiempo real.
* Obtener una vista previa del corchete en el momento en que se inicia un torneo.
* Consulte los jugadores que ya se han registrado en el torneo.
* Podrá registrar si no cumplen los requisitos de torneo.
* Podrá registrar si se ha prohibido un reproductor.

> [!div class="nextstepaction"]
> [Contratación de coincidencia](arena-ux-match-engagement.md)

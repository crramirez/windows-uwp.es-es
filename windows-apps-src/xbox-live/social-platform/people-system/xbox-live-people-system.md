---
title: Sistema de Xbox Live personas
description: Obtenga información sobre el sistema de personas de Xbox Live.
ms.assetid: f1881a52-8e65-4364-9937-d2b8b8476cbf
ms.date: 03/19/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, social, sistema de personas, amigos
ms.localizationpriority: medium
ms.openlocfilehash: 39fd9283e169efa04f9c61d1c5e1d206617ab0c8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660270"
---
# <a name="xbox-live-people-system"></a>Sistema de Xbox Live personas

Antes de empezar a integrar servicios sociales de Xbox Live en el título puede ser útil para comprender el sistema que está trabajando. Para ello, se describirá brevemente el funcionamiento del sistema personas Xbox Live. El sistema de las personas dicta y mantiene las relaciones entre los jugadores en el ecosistema Xbox Live. El sistema de las personas permite jugadores reconocer fácilmente y organizar sus contactos en Xbox Live, ya sean amigos o conocidos en línea. Puede mostrar su nombre real a las personas de confianza y ocultarla a los que no. Puede reproducir favoritos con sus amigos, por lo que siempre sabrá lo están hasta el primer mientras está en línea. El sistema de personas de Xbox Live permite agregar sus amigos buena y aquellos que le dio buena juegos todo en uno seguro, fácil de organizar el paquete.

## <a name="one-and-two-way-relationships"></a>Una y dos relaciones de forma

El sistema de las personas le permite tener una o dos relaciones de manera por medio del concepto de *siguiente*. Antes de que solo podría *friend* otros jugadores mediante el envío de una invitación y, a continuación, esperar para aceptar esa solicitud amigo, lo que resulta en una relación bidireccional, donde cada seguir entre sí. Este proceso podría tardar días, según la frecuencia jugadores comprueban sus solicitudes de confianza. Con la adición de una manera de relaciones puede *siga* un usuario y vea su actividad en línea inmediatamente en lugar de esperar que respondan. En esta fase es un *seguidores*. La persona que tenga *seguido* recibirán todavía reciben una notificación y tendrá la oportunidad de seguir, una acción que le permitirán *amigos* en una relación bidireccional. Relaciones de una manera de reducen el tiempo de espera a la interacción con la ventaja adicional de lo que le permite mantenerse al día con jugadores se indicó que transmitir a los visores o gozan de una continuación para su habilidad competitivo.

## <a name="gametags-and-real-names"></a>Nombres Gametags y real

Crear y mantener un mapa mental de los que el usuario del mundo real es qué nombre de jugador pueden convertirse en una tarea herculean cuando tienen el potencial para amigos ilimitados y siguiendo las relaciones. Que es por eso le ofrecemos la opción para mostrar su nombre real para jugadores sabe personal para que pueden reconocer más fácilmente. Que muestra los nombres reales es opcional y debe ser elegido por el usuario. Nunca se muestran los nombres reales a todos los usuarios de Xbox Live, solo se muestran a personas que el usuario sabe que se expresan a través de conexiones manualmente indicadas por el usuario o a través de una red social, que el usuario ha asociado con la cuenta. Los usuarios siempre aparecerá como su nombre de jugador cuando se interactúa con extraños aleatorios en la coincidencia de decisiones o de otras experiencias; Además, no hay ninguna opción para que aparezca como su nombre real con extraños.

Los usuarios pueden controlar cómo aparecen estas en juegos y pueden decidir que quiere que otros usuarios siempre verlos como su nombre de jugador de juegos, aunque algunas de esas personas tengan acceso a su nombre real. En el tiempo a los usuarios suscribirse a la funcionalidad que permite que su nombre real mostrar (solo para aquellos que saben), se presentan con la opción siempre aparecen como su nombre de jugador en juegos. Al seleccionar esta opción significa que el nombre que aparece en la pantalla por encima de su cabeza en juego siempre será su nombre de jugador.

## <a name="privacy-and-people-i-know"></a>Privacidad y la gente que conozco

Una característica fundamental del sistema personas es que proporciona para las relaciones anónimas y no anónimos; los usuarios pueden tener amigos real-name así como relaciones de nombre de jugador aleatorias con usuarios que se ha encontrado en Xbox Live. Esto significa que hay dos conjuntos diferentes de los derechos que deben ser compatibles: uno para permitir cualquier cosa confidenciales amigos de confianza reales ver información confidencial y otro para permitir que los usuarios agregar relaciones aleatorias de nombre de jugador, mientras safe sensación que no revelarán .
Además de esta a los usuarios, puede colocar restricciones en partes individuales de información sobre sí mismos como su perfil, el estado en línea y lista de amigos. Acceso a esta información puede colocarse en tres depósitos.

- Todo el mundo: Esto hace que la información pública para cualquier persona que viene a través de ese usuario.
- Amigos: Esto hace que la información disponible solo para aquellos que el usuario considera a un amigo.
- Bloque: Esto hace que la información accesible.

Ver la lista completa de la configuración de privacidad en [xbox.com](https://account.xbox.com/Settings).

## <a name="favorite-people"></a>Personas favoritas

El sistema de personas de Xbox Live proporciona el usuario final una categoría simple de su propia lista de personas. Los usuarios pueden marcar todos los usuarios de su lista como favorito y esas personas aparecerán primero y mayor prioridad en todas las características. Los favoritos aparecerán primeros en los títulos de cada vez que se muestra una lista de personas. No hay ningún límite al número de favoritos, que puede tener un usuario. Los usuarios pueden alternar todos los usuarios de su lista como favorito, independientemente de si la persona es un nombre real o el nombre de jugador relación sin implicaciones de privacidad. Personas favoritas son un subconjunto de la lista de personas del usuario y no se comportan como una lista de personas independiente.
---
title: Flujos actualizados para invitaciones juegos multijugador
description: Proporciona flujos actualizados para la implementación de Xbox Live invita varios jugadores.
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, 2015 multijugador
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596890"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>Flujos actualizados para invitaciones juegos multijugador

Como resultado de comentarios de Xbox One beta, el flujo de la experiencia de usuario para varios jugadores invita ha cambiado a partir de Xbox una recuperación actualización 24, lanzado en 6 de noviembre de 2013. Se trata de un cambio en el **experiencia de usuario (UX) solo** y no afectará a ningún comportamiento o la funcionalidad desde la perspectiva de un título de juego. Los desarrolladores de título no debe realizar ningún cambio de código.

## <a name="summary-of-changes"></a>Resumen de cambios

-   La notificación inicial invitación ha cambiado de "unir mi entidad" a "&lt;juegos título&gt; Practiquemos" y ahora tiene un botón que permite a los usuarios iniciar el juego y comenzar de inmediato en juego.

-   La aplicación de terceros no se ajusta de forma predeterminada cuando el usuario elige el nuevo "&lt;juegos título&gt; Practiquemos" opción. Este cambio también se realiza para que el usuario puede comenzar de inmediato en juego.

-   Se ha agregado una nueva notificación en el lado del emisor que dice "Adding \[ *número* \] amigos del juego". Esto deja claro que se envíen invitaciones cuando una sesión está asociada a las partes de un usuario.

En los ejemplos siguientes se describen los flujos de experiencia de usuario detallada. Cada tabla muestra un ejemplo de flujo de dos usuarios, David y Laura. Estos flujos se muestran en cada columna y se producen en paralelo. El <b style="background-color: #FFFF00">texto resaltado</b> muestra los ajustes que se han realizado desde los flujos de experiencia de usuario anteriores.

## <a name="inviting-users-from-within-a-game"></a>Invitar a los usuarios desde dentro de un juego

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
Juan está en el vestíbulo para un juego que se está reproduciendo con varios jugadores.<p><br>David elige <b>invitar a un amigo</b>.<p><br>David selecciona a Laura.<p><br>Notificación emergente que indica que se ha enviado una invitación. | Laura es un juego diferente.
    </td>
    <td style="border-bottom:solid 1px #fff">
Laura es un juego diferente.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Notificación del sistema Emerge que indica una invitación de David, y <b style="background-color: #FFFF00">muestra el nombre del juego y el icono</b>. (La aplicación de terceros no auto-snap.) <p><br>
En el centro de notificaciones, <b style="background-color: #FFFF00">Laura puede elegir iniciar y aceptar la invitación</b>, <b>Aceptar invitación</b>, o <b style="background-color: #FFFF00">invitar a rechazar</b>.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">Caso 1: Laura selecciona iniciar y aceptar la invitación</b> (es decir, una nueva opción)</td>
  </tr>
  <tr>
    <td>
Notificación emergente que indica que ha unido Laura de entidad de David.             
    <p><br>
David inicia el juego de lobby de varios jugadores.                              
    <p><br>
    <b style="background-color: #FFFF00">Notificación emergente que indica que se envió una invitación para jugar a Laura.</b>
    </td>
    <td>
Inicia el juego y no se ajusta la aplicación de terceros.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>Caso 2: Laura selecciona Aceptar invitación</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura une a la entidad.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">Notificación emergente que indica que se envió una invitación para jugar a Laura.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>Notificación emergente que indica que se ha encontrado un juego para la entidad.
    <p><br>
En el centro de notificaciones, puede elegir Laura: <ul>
    <li>   <b>Aceptar la invitación de juegos:</b> Lanzamientos de juego.
    <li>   <b>Rechazar la invitación de juegos:</b> No hay ningún juego se inicia. Laura aún está en la entidad y recibirá invitaciones juego posteriores.         
    <li>   <b style="background-color: #FFFF00">Deje la entidad: No hay ningún juego se inicia. Laura se quita de la entidad.</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>Flujo de invitación en el juego: En una entidad y cambiar los títulos

<table>
  <tr>
    <td>
Un juego juntos.
    <p><br>
David cambia a un juego diferente y entra en el menú de varios jugadores.
     <p><br>
La interfaz de usuario del sistema Xbox pide David si quiere cambiar su entidad para el nuevo juego al que puede responder <b>Sí</b> o <b>No</b>.
    </td>
    <td style="text-align:top">
Un juego juntos.
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Caso 1: Sí
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
Trata el nuevo título de entidad.
    <p><br>
En el vestíbulo de varios jugadores, David inicia el juego.
    <p><br>
    <b style="background-color: #FFFF00">Notificación emergente que indica que se ha enviado una invitación para jugar a Laura.
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>Notificación emergente que indica que se ha encontrado un juego para la entidad.
    <p><br>
Desde el centro de notificaciones, puede elegir Laura: <ul>
     <li><b>Aceptar invitación juego</b>: Inicia de nuevo juego <li><b>Rechazar la invitación de juegos:</b> No hay ningún juego se inicia, pero Laura aún está en la entidad y recibirá invitaciones juego posteriores.
     <li><b style="background-color: #FFFF00"><b>Deje la entidad:</b> Ningún juego lanzamientos y Laura se quita de la entidad.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Caso 2: No
    </td>
  </tr>
  <tr>
    <td>
Entidad no se incluye en el nuevo título.
David desempeña en modo de varios jugadores sin sus miembros de entidad.
David todavía está en la entidad.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>Invitar a flow desde casa

<table>
  <tr>
    <td>
David está explorando <b>inicio</b>y en su <b>amigos</b> lista, ve que Laura está en línea.
    <p><br>
David elige invitar a Laura a su entidad. Notificación emergente que indica que se envió la invitación. Acopla la aplicación de terceros para David.
    </td>
    <td>
Laura es un juego.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Notificación emergente que indica que David invitó a Laura a su entidad.
    <p><br>
En el centro de notificaciones, Laura tiene la opción de <b>acepte la invitación</b>.
    <p><br>
Cuando acepta Laura, se ajusta la aplicación de terceros.                                                                         </td>
  </tr>
  <tr>
    <td>
Notificación emergente que indica que ha unido Laura de la entidad.
    <p><br>
David y Laura describen qué juego que deseen jugar. David inicia el juego y entra en el modo de juego multijugador.
    <p><br>
Juego bien proporcionará una opción para invitar a amigos o extracción automáticamente los miembros de entidad.
    <p><br>
    <b style="background-color: #FFFF00">Notificación emergente que indica que se ha enviado una invitación para jugar.</b>
    </td>
    <td>
Notificación emergente que indica que se ha encontrado un juego para la entidad.
    <p><br>
En el centro de notificaciones, Laura hacer lo siguiente: <ul>
    <li>   <b>Aceptar la invitación de juegos:</b> Lanzamientos de juegos <li>   <b>Rechazar la invitación de juegos:</b> Ningún juego lanzamientos, Laura es todavía se encuentra en la entidad y recibirá invitaciones posteriores.
    <li>   <b style="background-color: #FFFF00">Deje la entidad: No hay ningún juego se inicia, Laura se quita de la entidad.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>Más información sobre el juego invitar enviados del sistema

El **invitar a juegos envían** toast sólo aparecerá la primera vez que se establece una sesión de juego con miembros de la parte remota. Las solicitudes posteriores que se envían a los miembros de la parte remota no generará esta notificación del sistema. Esto impide que el usuario que se va a spam con repite **invitar envían juego** notificaciones del sistema si el título realiza varias llamadas a **PullReservedPlayersAsync**.

**Tenga en cuenta** el procedimiento recomendado consiste en Agregar todos deseado amigos como reservado y, a continuación, llamar a **PullReservedPlayersAsync** sólo una vez.

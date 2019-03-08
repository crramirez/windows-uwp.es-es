---
title: Ámbito de Xbox
description: Aprenda a usar Xbox Arena para ejecutar torneos para su juego.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneo, experiencia de usuario
ms.localizationpriority: medium
ms.openlocfilehash: b08da01323d05c961005d562b70667dbbdf85437
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643770"
---
# <a name="xbox-arena"></a>Ámbito de Xbox

Xbox Arena es una plataforma para crear y ejecutar en línea torneos en Xbox One y Windows 10 a través de Xbox Live, con el fin de volver a poner la diversión del juego competitivo para un público amplio.
Cada editor tiene distintas necesidades en cuanto a juegos competitivo y con Arena queremos ser tan flexible como sea posible. Por lo tanto, hay tres maneras diferentes de ejecutar torneos para los títulos en el campo:

* Con torneos franquicia de ejecución, Xbox Live le proporciona las herramientas y servicios para configurar y ejecutar sus propio torneos.

* Xbox colabora con los organizadores de torneo líderes del sector (TOs), que le permitan ejecutar torneos en nombre de su título a través de Arena. (Para una lista de los organizadores del torneo compatibles, póngase en contacto con su administrador de cuentas de Microsoft).

* Los jugadores pueden crear y ejecutar sus propias torneos Arena con nuestro torneos generados por el usuario o UGTs.

Lo más importante, agregar compatibilidad con ámbito en los títulos de le permite aprovechar las ventajas de las tres. Proporcionamos API robustas que permiten a los títulos para promover torneos en juego y para facilitar la transición de los participantes a y de las coincidencias. El concentrador de Arena Xbox admite tareas de administración del torneo como registro, las notificaciones, corchetes y la propagación y notificar los resultados.

> [!IMPORTANT]  
> Xbox Arena solo está disponible para socios administrados y ID@Xbox a los desarrolladores. No está disponible para el programa de creadores de Xbox Live.

## <a name="a-titles-baseline-tournament-experience"></a>Experiencia de torneo de línea de base de un título

Si el título se integra con uno o varios de los organizadores de torneo, debe proporcionar una experiencia de torneo de línea base. Tiene la libertad integrar más profundamente con un organizador específico torneo si lo desea, proporcionar una experiencia más enriquecida para las competiciones. Pero aún será necesario proporcionar la experiencia de línea base para otros organizadores de torneo y para UGTs y torneos franquicia de ejecución si decide ejecutarlos.

### <a name="baseline-requirements-for-a-title"></a>Requisitos básicos para un título

* Para obtener una lista completa de requisitos, póngase en contacto con su administrador de cuentas de Microsoft.

### <a name="ui-recommendations"></a>Recomendaciones de la interfaz de usuario

* Identificar que la coincidencia es un torneo de la interfaz de usuario.

* En la interfaz de usuario introductoria, incluir un elemento de interfaz de usuario que vincula a los usuarios a su centro de torneo o la página de detalles del torneo en el shell de Xbox Arena la interfaz de usuario o una aplicación de organizador del torneo.



La experiencia del usuario de línea base que debe proporcionar el título es sencillo y lo suficientemente flexible para trabajar con una gran cantidad de formatos posibles de competencia. Tiene la libertad adaptar las instrucciones de experiencia de usuario y la experiencia de los requisitos para que quepa el flujo de la interfaz de usuario de su título y para garantizar que un usuario sin problemas.

Por ejemplo:

* No tiene la información necesaria que se mostrará en la pantalla principal, siempre que esté disponible en algún lugar, como en una página de detalles o emergente.

* Puede haber muchas versiones de cada pantalla o se pueden combinar entre sí o con pantallas de juegos existentes. Por ejemplo, muchos juegos incluyen una "informe carnicería" de coincidencia posterior a la pantalla, que puede hallarse adaptar para satisfacer los "en espera de resultados" y "Ready" requisitos de la pantalla.

* Las pantallas no se deben cambiar tan pronto como la fase. Por ejemplo, si fase del equipo cambia de "Listo" al "Juego" mientras el usuario está en una pantalla "Ready", el título no es necesario para pasar inmediatamente en juego. Puede (y probablemente debe) pasar la "en espera de coincidencia..." indicador a un botón, por ejemplo, "listo para reproducir", para que el usuario tiene el control del flujo y, por tanto, puede tener una mejor comprensión de la misma. No pasa nada para los requisitos de la fase de "Juego" para el retraso hasta que el usuario confirma la transición.


## <a name="arena-vs-title-roles"></a>Arena frente a título "roles"

Mover dentro y fuera del juego, su progreso a través de las fases del torneo, puede resultar complicado para los usuarios. Cuando el proceso es diferente para cada juego que juegan, es incluso menos probable que memorizar dónde y qué esperar.

> [!TIP]
> **Recomendación de la experiencia del usuario**  
>
> Simplificar roles funcionales entre el juego y la interfaz de usuario de la Arena Xbox, manteniéndolos pueden distinguir claramente. Por ejemplo, se hayan completado todas las tareas relacionadas con la administración de Arena, y todas las tareas relacionadas con la reproducción de juegos se completan dentro del juego.

Rol de Xbox Arena (configuración de un torneo)   | Rol del título (juegos)
--- | ---
<ul><li>Registro y en el repositorio</li><li>Notificaciones</li><li>Inicialización y corchetes</li><li>Entrenamiento de equipos</li></ul> |     <ul><li>Transición de los participantes a y desde la interfaz de usuario de Arena</li><li>Identificación a específica del torneo coincidencias en varios jugadores vestíbulo de la interfaz de usuario</li><li>La promoción o la exploración de torneos en juego</li></ul>

## <a name="engineering-guidance"></a>Guía de diseño

Artículo | description
--- | ---
[Integración de título de campo](arena-title-integration.md) | Obtenga información sobre cómo integrar compatibilidad para Xbox Arena en su título.

## <a name="operations-guidance"></a>Guía de operaciones

Artículo | description
--- | ---
[Portal de Operations de Arena Xbox](operations-portal.md) | Se describe en el portal de las operaciones que puede usar para crear y administrar torneos oficiales para un título que se integra con ámbito de Xbox.

## <a name="user-experience-guidance"></a>Guía de experiencia del usuario

Artículo | description
--- | ---
[Detectar torneos de Xbox](discovering-xbox-tournaments.md) | Proporciona sugerencias y recomendaciones para crear un usuario excelente experimentan para detectar torneos existentes.
[Unirse a un torneo](arena-ux-join-tournament.md)  |  Proporciona sugerencias y recomendaciones para crear un usuario excelente experimentan para el registro y unión de un torneo.
[Contratación de coincidencia](arena-ux-match-engagement.md) | Describe las fases de la experiencia de usuario de jugadores progresa a través de un torneo.
[Metadatos de la interfaz de usuario de la API de arena](arena-apis-metadata.md)  | Describe los metadatos devueltos por las que puede usar para mostrar información en el juego sobre el estado actual del torneo de la API de Arena.
[Notificaciones de arena](arena-notifications.md)  | Describe las condiciones al campo Xbox envía una notificación a los participantes del torneo.
[Escenarios de usuario de arena](arena-user-scenarios.md)  | Describe escenarios de Arena para jugadores según sus motivaciones más comunes.

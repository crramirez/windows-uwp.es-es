---
title: Detectar torneos de Xbox
description: Aprenda a crear la interfaz de usuario para la detección del torneo de tu juego.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneo, experiencia de usuario
ms.localizationpriority: medium
ms.openlocfilehash: 71a065d6d4da290f2ce3345c7da0026d4c116335
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632480"
---
# <a name="discovering-xbox-tournaments"></a>Detectar torneos de Xbox

Jugadores tienen varias opciones en la interfaz Xbox o la aplicación Xbox en un equipo, para obtener más información sobre los que participan en o se ve un torneo. También hay oportunidades para aumentar la detección de torneo desde dentro de su título.  

Puede encontrar torneos Arena habilitados por un jugador:

* Apertura torneos concentrador de un juego recientemente reproducido desde casa de Xbox.
* Visita un perfil de Reproductor para torneos reproduce, reproducción en curso o registrado para.
* Responder a un equipo en busca de grupo (LFG) se registra en un club que pertenecen.
* Viendo una secuencia de torneo en la aplicación de Mixer, sitio Web o la dinamización de inspección en el concentrador o Arena juego.
* Visitar un centro de juegos en la interfaz Xbox.

## <a name="promoting-tournaments-in-game"></a>Promover torneos de juego

Las personas ven torneos porque están interesados en el título. Juegos de Xbox tienen una oportunidad única para detectar los jugadores en ese momento fundamental cuando buscan nuevas formas de ampliar su experiencia de juego.

> [!TIP]
> **Recomendación de la experiencia del usuario:** Promover torneos en momentos de toma de decisiones.
>
> Conciencia para torneos a veces cuando jugadores deben decidir si desea mantener la reproducción en curso o salir (por ejemplo, al iniciar juegos, menú principal, menú multijugador o al final de coincidencia).

### <a name="1--use-a-dynamic-announcement-feature-example-message-of-the-day"></a>1.  Usar una característica de anuncios dinámicos (ejemplo: Mensaje del día)

Los títulos que han creado una característica de anuncios en su juego UI suelen usan un sistema de administración de contenido que permite las actualizaciones de instantáneas. Esto es útil para insertar contenido nuevo para jugadores sin tener que depender de una actualización del contenido o una versión descargable de contenido. Por ejemplo, Halo usa una característica de "Mensajes del día" para anuncios de la Comunidad de superficie y sugerencias. Promoción de torneo es perfecto para este escenario.

**Impacto en el usuario**

* Jugadores se introducen en una nueva oportunidad de juegos competitivo antes de iniciar una sesión de juego.
* Exposición es intermitente (en lanzamiento, combinado con otros anuncios).
* Para obtener más información, jugadores se dirigirán a dejar el juego y vaya al centro de Arena.
* La característica no requiere una actualización de contenido para rellenar.
* Relevancia y temporización de promoción sigue siendo flexible.

###### <a name="ui-example-message-of-the-day"></a>Ejemplo de la interfaz de usuario: 'El mensaje del día'

![Mensaje de torneo del día](../../images/arena/arena-ux-motd.png)

#### <a name="a-main-heading-ex-play-watch-learn"></a>A. Main encabezado p. ej.: Reproducir, ver, aprender  

Informa al jugadores torneos todas o uno específico.

#### <a name="b-go-to-arena-hub"></a>B. Vaya al concentrador de Arena  

Vínculo profundo para juegos Hub/torneos - recomendados al promocionar torneos todos relacionados con su juego.  
O bien  
Vínculo profundo a juego/torneo/detalles del centro de-recomendados al anunciar un torneo específico.

#### <a name="c-game-exit-confirmation"></a>C. Confirmación de salir del juego

Elementos de la pantalla A y B guían al usuario fuera del juego a la interfaz Xbox. Establezca esta expectativa en interfaz de usuario del título de la antes del jugador facilita una decisión.

###### <a name="ui-example-exit-game-confirmation"></a>Ejemplo de la interfaz de usuario: Confirmación de juegos de salida
![Confirmación de juegos de torneo salir](../../images/arena/arena-ux-exit-confirm.png)

### <a name="2--promote-tournaments-on-the-main-menu"></a>2.  Promover torneos en el menú principal

En este ejemplo, el anuncio es una publicidad interactiva de un evento próximo. Proporciona información suficiente para atraer el jugador obtener más información. En **seleccione**el jugador está dirigido a una página de detalles torneos en el concentrador de Arena, donde puede administrar su registro.

###### <a name="ui-example-a-tournament-ad-displayed-alongside-the-main-menu"></a>Ejemplo de la interfaz de usuario: Un anuncio del torneo junto con el menú principal

![](../../images/arena/arena-ux-promo.png)

> [!TIP]
> **Recomendación de la experiencia del usuario**  
> El anuncio debe incluir el nivel de detalle suficiente para ayudar a los jugadores a decidir si desea obtener más información. Por ejemplo, ***descripción***, ***popularidad***, y ***tiempo***.

## <a name="browsing-tournaments-in-game"></a>En el juego de exploración torneos

Las API de Arena proporcionar suficientes datos para el título crear una experiencia de exploración dentro del juego. Si tiene previsto crear una característica de exploración, agregue un **torneos** punto de entrada en la sección de varios jugadores.

> [!TIP]
> **Recomendaciones de la experiencia del usuario**  
> Torneos presentes como un modo de juego multijugador.  
> Dado que Xbox Arena es una nueva característica, asegúrese de mantener visibles en el nivel de nivel 1 o nivel 2.

###### <a name="ui-example-tournaments-listed-as-an-additional-mode-in-the-multiplayer-section"></a>Ejemplo de la interfaz de usuario: Torneos aparece como un modo adicional en la sección multijugador

![Modo de torneo](../../images/arena/arena-ux-tournament-mode.png)

### <a name="provide-a-comprehensive-list-of-tournaments"></a>Proporciona una lista completa de torneos

Capacite a los jugadores para comprobar rápidamente el estado de torneos que actualmente se registren en y para examinar torneos entre sesiones de juego.

#### <a name="user-impact"></a>Impacto en el usuario

* Minimiza la necesidad del jugador a dejar el juego para comprobar el estado de torneo en el panel.
* Actúa como una excelente solución de copia de seguridad si jugadores han perdido las notificaciones del sistema de Xbox Arena.
* Implica un costo adicional para el desarrollador de juegos a administrar.
* Lleva a jugadores a la interfaz de usuario de Arena para unir, registro, la comprobación, formación de propagación y el equipo.

> [!NOTE]  
> Las API de Arena no proporcionar una consulta para torneos generados por el usuario (club generado).


> [!TIP]
> **Recomendación de la experiencia del usuario**  
> Crear una interfaz de usuario que se puede escalar y proporcionar métodos para jugadores administrar una amplia lista de torneos.

### <a name="filters"></a>Filtros

Filtrar torneos por **todas**, **Active** (que abarca todo, hasta el torneo termina), **Canceled**, y **completado** Estados, y torneos jugador tiene o va a participar en (por ejemplo, mi torneos).

###### <a name="ui-example"></a>Ejemplo de la interfaz de usuario:

![Pantalla de filtros de torneo](../../images/arena/arena-ux-filters.png)

> [!NOTE]  
> Es posible, agregar filtros personalizados, fuera de la API admite *pero no se recomienda*.

El título puede filtrar por otras propiedades después de realiza la solicitud, pero no puede especificarlos en el parámetro de consulta. Esto hace que el filtrado por otras propiedades poco confiables para este tipo de interfaz de usuario. Las API de Arena recuperar un número especificado de título de resultados a la vez, por ejemplo, puede especificar el título de un **maxItems** valor de 10 torneos. Esto permite que el título de la administración del rendimiento y minimiza el riesgo de sobrecargar el usuario con una lista grande. Sin embargo, los filtros personalizados proporcionados por su juego se aplicaría solo a los 10 artículos solicitados en ese momento. Esto podría provocar un número incoherente de los resultados filtrados después de cada llamada API. Por ejemplo, el primer conjunto de 10 torneos devuelto podría contener 2 que están relacionados con un filtro personalizado, como "Juego". Pero los 10 siguientes puede mostrar estos 9 artículos y así sucesivamente. La única manera confiable para el título rellenar por completo cada filtro personalizado es solicitar cada torneo activo y, a continuación, filtrar todos ellos, que no se recomienda.

#### <a name="filter-recommendations"></a>Filtrar recomendaciones:

##### <a name="all"></a>ALL

Todos los ver **Active**, **Canceled**, y **completado** torneos, ordenados por recientes.

Resultados:

* Estado del torneo
* Fecha de inicio del torneo
* Estado del torneo, por ejemplo, un registro abierto
* Vínculo profundo a la página de detalles del torneo en el panel

##### <a name="my-tournaments"></a>MI TORNEOS

Torneos de vista actualmente registrado en un participante.

Resultados:

* Torneos que participa en el usuario ha iniciado sesión actualmente
* Estado del torneo
* Un método para especificar a una coincidencia de torneo (para el estado 'listo match')
* Vínculo profundo a la página en el panel de detalles del torneo

##### <a name="active"></a>ACTIVE

Ver todos los torneos que se han creado: próximos, abra para registro, en el repositorio y reproducción en curso.

Resultados:

* Torneos que están abiertos para registrar para que el usuario actual no ya se ha unido
* Tiempo restante hasta que se cierra el registro

##### <a name="completedcanceled"></a>COMPLETADO O CANCELADO

Ver resultados recientes. Pueden jubilar torneos que acaba de terminar con el tiempo, en lugar de simplemente desaparecer una vez que se completa.

Resultados:

* Vínculo profundo a la página en el panel de detalles del torneo
* Estado del torneo
* Fecha/hora de finalización

##### <a name="canceled"></a>CANCELADO

Torneos de vista que se han cancelado.

Resultados:

* Vínculo profundo a la página en el panel de detalles del torneo
* Fecha y hora de cancelación

> [!div class="nextstepaction"]
> [Unir un torneo mediante la interfaz de usuario de Arena](arena-ux-join-tournament.md)

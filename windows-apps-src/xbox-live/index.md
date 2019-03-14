---
title: "Guía del desarrollador de Xbox\_Live"
description: "Aprende usar los servicios de Xbox\_Live para conectar tu juego a la red de juegos de Xbox\_Live."
ms.date: 08/22/2017
ms.topic: article
keywords: 'windows 10, uwp, games, xbox, xbox live'
ms.localizationpriority: medium
---
# <a name="what-is-xbox-live"></a>¿Qué es Xbox Live?

Xbox Live es una red de juegos líder que conecta a millones de jugadores de todo el mundo. Puedes agregar Xbox Live a tu juego de Windows 10 o Xbox One para así aprovechar las ventajas de las características y servicios de Xbox Live.

Con el Programa de creadores de Xbox Live, cualquier persona con una cuenta del [Centro de partners](https://partner.microsoft.com/dashboard) puede crear un juego de la Plataforma universal de Windows (UWP) habilitado para Xbox Live que funcione tanto en un equipo con Windows 10 como en una consola Xbox One.

Los desarrolladores de juegos que quieran aprovechar las ventajas de la experiencia completa de Xbox Live, como la opción multijugador, los logros y el desarrollo nativo para consolas Xbox, tienen a su disposición programas adicionales que se detallan en [Información general del programa para desarrolladores](developer-program-overview.md).

Algunas de las razones para agregar Xbox Live a tu juego son estas:

- Xbox Live une a los jugadores de Xbox One y Windows 10, por lo que pueden jugar con sus amigos y conectarse con una enorme comunidad de jugadores.
- Xbox Live permite a los jugadores traspasar su recorrido de juego al desbloquear logros, compartir clips de juegos épicos, acumular puntuaciones y perfeccionar su avatar.
- Xbox Live permite que los jugadores jueguen y reanuden la partida donde lo dejaron en otra Xbox One o equipo, dado que se pueden llevar todo lo guardado desde otro dispositivo.
- Con más de mil millones de partidas multijugador jugadas al mes, Xbox Live se ha creado pensando en el rendimiento, la velocidad y la confiabilidad.
- Gracias a la opción multijugador entre dispositivos, los jugadores pueden jugar con sus amigos tanto en una Xbox One como en un equipo con Windows 10.

> [!note]
> Estos temas están dirigidos a los desarrolladores de juegos que desean agregar a sus juegos compatibilidad con Xbox Live. Para información sobre Xbox Live de consumo, consulte [Xbox Live](https://www.xbox.com/live/).

## <a name="how-xbox-live-works"></a>Cómo funciona Xbox Live

Desde el punto de vista técnico, Xbox Live es una colección de microservicios que exponen características de Xbox Live, como perfil, amigos y presencia, estadísticas, marcadores, logros, opción multijugador y matchmaking. Los datos de Xbox Live se almacenan en la nube y es posible acceder a ellos mediante puntos de conexión REST y WebSockets seguros que son accesibles desde un conjunto de API del lado cliente diseñadas para los desarrolladores de juegos.

Además de las API REST, existen API del lado cliente que encapsulan la funcionalidad REST. Para más información, consulta [Introduction to Xbox Live APIs](introduction-to-xbox-live-apis.md) (Introducción a las API de Xbox Live).

### <a name="get-started-with-xbox-live"></a>Introducción a Xbox Live

Las siguientes guías pueden servirte de introducción al desarrollo para Xbox Live, tanto si eres un desarrollador de UWP o de la consola Xbox.  También hay guías para la configuración con motores de juegos.

| Tema                                                                                                                                             | Descripción                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Información general del programa para desarrolladores](developer-program-overview.md) | Se describen los diversos programas para desarrolladores que permiten el desarrollo para Xbox Live. |
| [Introducción al Programa de creadores de Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) | Cómo empezar con Xbox Live en el Programa de creadores de Xbox Live. |
| [Introducción a Xbox Live como ID@Xbox o desarrollador administrado](get-started-with-partner/get-started-with-xbox-live-partner.md) | Cómo empezar con Xbox Live como desarrollador en el Programa ID@Xbox. |

### <a name="using-xbox-live"></a>Uso de Xbox Live

Cuando hayas creado el título y tengas definidos los aspectos básicos, en esta sección se proporciona el contexto necesario antes de pasar a la creación del código.

| Tema                                                                                                                                             | Descripción                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Uso de Xbox Live](using-xbox-live/using-xbox-live.md) | Después de configurar el título y el SDK integrado de Xbox Live, estás listo para implementar el inicio de sesión y aprender más sobre la programación de Xbox Live.
| [Procedimientos recomendados para llamar a Xbox Live](using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) | Familiarízate con los aspectos básicos sobre los patrones de llamada de Xbox Live y los procedimientos recomendados para garantizar que el título funciona bien y no se limita su velocidad.
| [Solución de problemas de la API de los Servicios de Xbox Live](using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md) | Problemas comunes que pueden producirse y sugerencias sobre cómo corregirlos.

### <a name="xbox-live-social-platform"></a>Plataforma social de Xbox Live

Las funciones sociales de Xbox Live pueden hacer crecer tu público de forma sistemática y hacer que te conozcan más de 55 millones de usuarios activos.  En esta sección se describe cómo empezar a usar las características sociales de Xbox Live.

| Tema                                                                                                                                             | Descripción                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma Social de Xbox Live](social-platform/social-platform.md) | Si puedes iniciar la sesión de un usuario, puedes comenzar a usar las características sociales de Xbox Live, como el gráfico social de un usuario, características avanzadas de presencia, etc. |

### <a name="xbox-live-data-platform"></a>Plataforma de datos de Xbox Live

La plataforma de datos de Xbox Live controla el uso de estadísticas de jugador, logros y marcadores.  Lee esta serie de temas para aprender más sobre el uso de estas características en tu título.

| Tema                                                                                                                                             | Descripción                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma de datos de Xbox Live](data-platform/data-platform.md) | Breve descripción de la plataforma de datos, así como instrucciones sobre cómo incorporar estadísticas, marcadores y logros al título.
| [Estadísticas de jugadores](leaderboards-and-stats-2017/player-stats.md) | Las estadísticas son la base de los marcadores.  Aprende aquí a definirlas y usarlas.
| [Marcadores](leaderboards-and-stats-2017/leaderboards.md) | Incorpora marcadores de forma inteligente para sacar el lado competitivo de tus usuarios.
| [Logros](achievements-2017/achievements.md) | Los logros son una de las características más conocidas de Xbox Live y un excelente motor de la participación de los jugadores. Aprende a usarlos en el título.

### <a name="xbox-live-multiplayer-platform"></a>Plataforma multijugador de Xbox Live

La opción multijugador es una manera excelente de ampliar la duración del título y mantener actualizadas las experiencias de juego.  Xbox Live proporciona numerosas características multijugador y matchmaking.  También cuentas con varias opciones de API que proporcionan diversos niveles de simplicidad frente a flexibilidad.

| Tema                                                                                                                                             | Descripción                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma multijugador de Xbox Live](multiplayer/multiplayer-intro.md) | Si no estás familiarizado con el desarrollo multijugador de Xbox Live, o no conoces las nuevas API, como Multiplayer Manager y Xbox Integrated Multiplayer, entonces empieza aquí. |
| [Escenarios multijugador](multiplayer/multiplayer-scenarios.md) | Sugerencias y consejos sobre cómo puedes incorporar la opción multijugador al título. |
| [Xbox Integrated Multiplayer](multiplayer/xbox-integrated-multiplayer.md) | Xbox Integrated Multiplayer (XIM) es una sencilla interfaz independiente para agregar la opción multijugador, redes en tiempo real y chat a tu título. |
| [Multiplayer Manager](multiplayer/multiplayer-manager.md) | Multiplayer Manager proporciona una API centrada en los escenarios multijugador habituales. |

### <a name="xbox-live-storage-platform"></a>Plataforma de almacenamiento de Xbox Live

La plataforma de almacenamiento de Xbox Live proporciona almacenamiento de títulos y almacenamiento conectado.  Se trata de dos servicios diferentes pero complementarios.  El almacenamiento conectado permite implementar los elementos guardados del juego en la nube, de forma que se puede pasar de un dispositivo a otro sin importar dónde el usuario inicia la sesión.  El almacenamiento de títulos permite almacenar blobs de datos por usuario o por título y compartirlos entre distintos usuarios.

| Tema                                                                                                                                             | Descripción                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Plataforma de almacenamiento de Xbox Live](storage-platform/storage-platform.md) | Usa los servicios de almacenamiento de Xbox Live para almacenar los elementos guardados del juego, las reproducciones de instantes, las preferencias de usuario y otros datos en la nube. |
| [Almacenamiento conectado](storage-platform/connected-storage/connected-storage-technical-overview.md) | Guía de información general y programación sobre el almacenamiento conectado. |
| [Almacenamiento de títulos](storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) | Guía de información general y programación sobre el almacenamiento de títulos. |

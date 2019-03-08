---
title: Información general del programa para desarrolladores
description: Obtenga información acerca de los programas de desarrollador diferentes disponibles para su uso de Xbox Live.
ms.assetid: 1166308a-4079-41b4-8550-ce04b82b4f72
ms.date: 05/30/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, programa para desarrolladores, creadores
ms.localizationpriority: medium
ms.openlocfilehash: 05abd3f28328f4418f5a8a772049b3869b488ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603890"
---
# <a name="developer-program-overview"></a>Información general del programa para desarrolladores

Si desea desarrollar los títulos de Xbox Live habilitado, hay varias opciones disponibles para usted. Cada uno ofrece diferentes niveles de inversión de tiempo por su parte, las características disponibles para usted y opciones de soporte técnico.

## <a name="xbox-live-creators-program"></a>Programa de creadores de Xbox Live

El programa de creadores de Xbox Live es un buen punto de partida para Xbox Live si desea familiarizarse con el desarrollo de Xbox Live. No hay ningún proceso de aprobación de Microsoft es necesaria para unirse a este programa, y hay certificación mínima y requisitos de publicación.

El programa de creadores de Xbox Live solo admite la creación de títulos para el [Universal Windows Platform](https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) (UWP).  Los siguientes ejemplares creados como juegos UWP que se ejecutan en equipos con Windows 10 y en las consolas Xbox One.  Para obtener más información sobre cómo ejecutar juegos UWP en Xbox One, consulte [UWP en Xbox One](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/index).  

En Xbox One, que ofrece una experiencia exclusiva del almacén de jugadores, los juegos publicados a través del programa de creadores de Xbox Live se venderá en la nueva sección de la colección de los creadores de la Microsoft Store en Xbox. Esto ofrece un equilibrio entre lo que garantiza una plataforma abierta que cualquiera puede desarrollar y envíe un juego y una consola de experiencia de almacén elaboradas jugadores han llegado a conocer y esperar. En Windows 10, se publicará el título de entre todos los otros juegos de Xbox Live en la Microsoft Store.

### <a name="publishing-and-certification"></a>Publicación y certificación
Debe estar inscrito en la [programa para desarrolladores de centro de partners](https://developer.microsoft.com/store/register) para liberar un juego como parte del programa de creadores de Xbox Live. Hay dos conjuntos de requisitos que debe seguir su juego:

1. Integración de Xbox Live inicio de sesión y mostrar la identidad del usuario (nombre de jugador, Gamerpic, etcetera.). Todos los demás servicios de Xbox Live son opcionales.
2. Siga con el estándar [las directivas de Microsoft Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx).

### <a name="supported-xbox-live-services"></a>Servicios de Xbox Live admitidos
Marcadores, destacados de estadísticas, título de almacenamiento, almacenamiento conectado y un conjunto restringido de las características sociales, pueden usar títulos habilitados en el programa de creadores de Xbox Live. Logros, juego multijugador en línea y muchas características sociales son **no** admite para los títulos en el programa de creadores de Xbox Live.

Para obtener una lista completa de los servicios admitidos, vea el [tabla de características](#feature-table).

### <a name="supported-third-party-game-development-engines"></a>Motores de desarrollo de juegos de terceros compatibles
Los títulos del programa de creadores de Xbox Live son los juegos UWP que se pueden crear con un número de motores de juegos más populares. Microsoft ofrece documentación para integrar servicios de Xbox Live en UWP juegos creados con la [motor de juego Unity](https://unity.com). Puede encontrar [documentación](get-started-with-creators/develop-creators-title-with-unity.md) detallar Xbox Live integración con juegos de Unity aquí en este sitio, así como descargar y obtenga información sobre Microsoft creado [complemento de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin).

Los títulos del programa de creadores de Xbox Live también se pueden crear con los motores de juegos [construcción (2 & 3)](https://www.scirra.com/construct2), y [GameMaker Studio 2](https://www.yoyogames.com/gamemaker). Ambos motores de juegos han agregado compatibilidad con Xbox Live, sin embargo, que se controla la compatibilidad con el juego de motores de creadores y no Microsoft. Para obtener detalles y compatibilidad con la adición de Xbox Live a su proyecto de construcción o GameMaker Studio 2, tendrá que consultar cada documentación de motores de juegos, respectivamente.

[Aprenda a integrar Xbox Live en su proyecto de construcción.](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)

[Aprenda a integrar Xbox Live en su proyecto de GameMaker Studio 2.](https://www.yoyogames.com/gamemaker/xblc)

Para otros motores de desarrollo de juegos, como [MonoGame](https://www.monogame.net/) o [Xenko](https://xenko.com/), que no haya incorporada en Xbox Live funcionalidad o un complemento, todavía puede usar las API de Xbox Live para agregar Xbox Live en su título. Para usar la API de Xbox Live desde tu proyecto, puedes agregar referencias a los archivos binarios con paquetes de NuGet o agregar el origen de la API. Agregar paquetes de NuGet agiliza la compilación mientras que agregar el origen hace que la depuración sea más sencilla.

### <a name="support-and-feedback"></a>Soporte técnico y comentarios
Se pueden responder las preguntas que es posible que tenga en el [foros de MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev).  También puede hacer preguntas relacionadas de programación [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) mediante la etiqueta "xbox live".  El equipo de Xbox Live se implica con la comunidad y mejora de forma continua nuestras API, herramientas y documentación en función de los comentarios recibidos allí.

Para los desarrolladores en el programa de creadores de Xbox Live, puede [enviar una nueva idea](https://xbox.uservoice.com/forums/363186--new-ideas?category_id=196261) o [votar una idea existente](https://xbox.uservoice.com/forums/251649?category_id=210838) en nuestra [Xbox User Voice](https://xbox.uservoice.com/forums/363186--new-ideas).

## <a name="idxbox"></a>ID@Xbox

El programa de creadores de Xbox Live es excelente para muchos juegos y los desarrolladores. Sin embargo, si desea tener acceso a la pila completa de Xbox Live, incluidos varios jugadores en línea y logros, puntuación, o desea tener acceso a toda la eficacia de la familia de dispositivos con los kits de desarrollo de hardware, Xbox One el [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) programa es para usted.

Juegos en la ID@Xbox programa debe ser aprobado el concepto y vaya a través de certificación completa en Xbox One y Windows 10, que es un compromiso de tiempo mayor por su parte.
ID@Xbox títulos de obtención la selección de ubicación en la sección principal de Store, frente a la colección de creadores, que puede permitir la mayor exposición a los clientes.

Los desarrolladores en el ID@Xbox programa también obtener acceso a soporte técnico developer y asistencia promocional de Microsoft, así como el complemento completo de privada notas del producto y desarrollador foros técnicos. Aún puede usar [foros de MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev) o formular preguntas relacionadas de programación en [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) mediante la etiqueta "xbox live" Si lo desea.

## <a name="microsoft-partners"></a>Asociados de Microsoft

Los desarrolladores que trabajan con un editor del juego que es un Partner de Microsoft tienen acceso a todo el conjunto de características de Xbox Live y dedicado a los representantes de Microsoft para ayudar en su proceso de lanzamiento, certificación y desarrollo.

## <a name="feature-table"></a>Tabla de características

La siguiente tabla muestra las características disponibles para el programa Xbox Live creadores, y [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) programas.  

<table>

<tr>
<th>Área de características</th>
<th>Característica</th>
<th>Descripción</th>
<th> ID@Xbox </th>
<th>Programa de creadores de Xbox Live</th>
</tr>

<tr>
<td rowspan="2" class="dev-program-feature-name">Identidad</td>
<td>Inicio de sesión / registro</td>
<td>Permitir que los reproductores para iniciar sesión en Xbox Live en el título o crear una nueva cuenta de Xbox Live si es necesario</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-required">Requerido</td>
</tr>

<tr>
<td>Identidad del usuario</td>
<td>Usar identidad de Xbox Live mostrando el nombre de jugador, Gamerpic, etcetera</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-required">Requerido</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="13" class="dev-program-feature-name">Redes sociales</td>

<td>Presencia básica</td>
<td>Mostrar cadenas de presencia básica con la actividad del usuario dentro de un título.  Por ejemplo: "Trata de Minecraft Steve"</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Recientemente se reproduce</td>
<td>Aparecen en los títulos reproducidos recientemente en la Xbox App o en Xbox One</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Fuente de actividades</td>
<td>Aparecen en la actividad de la fuente en la Xbox App o en Xbox One</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Centro de juegos</td>
<td>Dispone de un centro de juego asociada con el título de la visualización de estadísticas, vídeos y otros contenidos en una fuente específica para el título de la</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Clubes</td>
<td>Reproductores pueden usar la Xbox App o en Xbox One para crear o clubes que se pueden, opcionalmente, asociar con el título.</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Busca el grupo (LFG)</td>
<td>LFG permite que los reproductores buscar otros fuera de juego para programar un juego multijugador.</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>GameDVR</td>
<td>Los reproductores puedan capturar vídeo de sus sesiones de juego y compartir estos en la fuente de actividades.</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Transmisión</td>
<td>Los jugadores pueden difusión en directo su juego a través de servicios como Mixer y Twitch de streaming</td>
<td class="xbl-features-automatic">Automático</td>
<td class="xbl-features-automatic">Automático</td>
</tr>

<tr>
<td>Presencia avanzada</td>
<td>Muestra información más detallada acerca de los jugadores en su título.  Mientras que la presencia básica podría mostrar "Usuario está en el automóvil de carreras Game", presencia enriquecida le permite especificar una cadena más detallada, como "usuario está impulsando Superautomóvil en RainyForest"</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr>
<td>Amigos</td>
<td>Recuperar la lista de amigos el inicio de sesión del usuario para habilitar escenarios de juego social en su título.</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-limited">Limitado / opcional (se exponen solo los amigos que hayan jugado su título)</td>
</tr>

<tr>
<td>Privacidad</td>
<td>Permitir que los jugadores en silencio o bloquear u otros reproductores</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional</td>
</tr>

<tr>
<td>Reputación</td>
<td>Los jugadores obtienen o perder reputación a través de su comportamiento. Comportamiento se utiliza en contactos y puede usarse por su título de manera personalizada.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr>
<td>Administrador de redes sociales</td>
<td>Recuperar eficazmente información sobre el gráfico social de un jugador</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-limited">Limitado / opcional (se exponen solo los amigos que hayan jugado su título)</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="4" class="dev-program-feature-name">Plataforma de datos</td>

<td>Estadísticas de los jugadores</td>
<td>Cargue las estadísticas acerca de los jugadores que se pueden usar en tablas de clasificación.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional (solo plataforma de datos de 2017)</td>
</tr>

<tr>
<td>Estadísticas de destacados</td>
<td>Designar determinadas estadísticas como "Stats destacadas" que se mostrará en el centro de juegos.</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-optional">Opcional (solo plataforma de datos de 2017)</td>
</tr>

<tr>
<td>Marcadores</td>
<td>Recuperar y mostrar estadísticas del Reproductor de forma ordenada a fomentar la competencia.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional (solo plataforma de datos de 2017)</td>
</tr>

<tr>
<td>Logros con puntuación</td>
<td>Designar determinadas estadísticas como "Stats destacadas" que se mostrará en el centro de juegos.</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="1" class="dev-program-feature-name">Multimedia</td>

<td>Búsqueda contextual</td>
<td>Anotar GameDVR clips con palabras clave para que sea más fácil para reproductores buscar los clips correspondientes a lo que va a inspeccionar.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>


<tr class="dev-program-feature-start">
<td rowspan="2" class="dev-program-feature-name">Almacenamiento</td>

<td>Almacenamiento conectado</td>
<td>Juegos guardados itinerancia entre equipos y consolas de una Xbox</td>
<td class="xbl-features-required">Requerido</td>
<td class="xbl-features-optional">Opcional</td>
</tr>

<tr>
<td>Almacenamiento de títulos</td>
<td>Almacenamiento de grandes cantidades de por usuario en la nube o de datos por título.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-optional">Opcional</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="6" class="dev-program-feature-name">Juego multijugador en línea</td>

<td>Directorio de sesión multijugador (MPSD)</td>
<td>Almacena información sobre una sesión de varios jugadores, como lista de jugadores, estado, etcetera.</td>
<td class="xbl-features-optional">Requerido</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr>
<td>Contactos</td>
<td>Xbox Live puede hacer que diferentes jugadores juntos para una sesión de varios jugadores.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr>
<td>Arena</td>
<td>Los jugadores pueden compitan entre sí estilo torneo.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr>
<td>Chat de juegos</td>
<td>Chat de voz para los agentes de un juego multijugador</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

<tr>
<td>Proceso de Xbox Live</td>
<td>Implementar los archivos ejecutables y activos que puede comunicarse el título, para descargar el cálculo desde el cliente.</td>
<td class="xbl-features-optional">Opcional</td>
<td class="xbl-features-notavailable">No se admite</td>
</tr>

</table>

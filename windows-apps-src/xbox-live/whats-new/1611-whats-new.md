---
title: Novedades para el SDK de Xbox Live - noviembre de 2016
description: Novedades para el SDK de Xbox Live - noviembre de 2016
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a9efeec63399916444de9ba33d4e587a914f2a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658570"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>Novedades para el SDK de Xbox Live - noviembre de 2016

Consulte la [What's New - agosto de 2016](1608-whats-new.md) artículo para lo que se agregó en la versión de agosto de 2016.

## <a name="xbox-services-api"></a>API de servicios de Xbox

### <a name="simplified-achievements"></a>Logros simplificadas

* [Simplificado logros](../achievements-2017/simplified-achievements.md) son una manera nueva y más sencilla para configurar y usar los logros.  Ya no tiene que enviar eventos y hacer que los servicios de Xbox Live decidir si se desbloquea el logro.  El título está a cargo de esa decisión.
* Si han formado parte de la primera prueba piloto de logros simplificado también hemos agregado compatibilidad sin conexión.
* Hay un ejemplo nuevo llamado SimplifiedAchievements que resalta lo fácil que es usar.

### <a name="multiplayer"></a>Varios jugadores

* [Examinar sesiones](../multiplayer/session-browse.md) es una nueva forma a los usuarios a encontrar un juego multijugador.  Examinar sesiones permite que los reproductores buscar una lista de sesiones juegos multijugador abiertas que cumplen los criterios especificados.
* El [Manager participan varios jugadores](../multiplayer/multiplayer-manager.md) ahora tiene capacidades de rellenado automático.  Si está habilitada, el Administrador de varios jugadores encontrará los miembros a través de contactos para rellenar espacios abiertos durante el juego.
* Una versión de preproducción de [XIM (varios jugadores integrada de Xbox)](../multiplayer/xbox-integrated-multiplayer.md) ahora está disponible para el desarrollo XDK.  XIM es una interfaz independiente para agregar fácilmente varios jugadores comunicación de redes y chat en tiempo real para su juego a través de la eficacia de los servicios de Xbox Live.

### <a name="social-manager"></a>Administrador de redes sociales

* Numerosos [Social Manager](../social-platform/intro-to-social-manager.md) cambios en la API:
    * El Administrador de redes sociales ha dejado el espacio de nombres experimental. Agradecimientos especiales a aquellos que fueron los usuarios pioneros y dio comentarios!
    * `xbox_social_user`
        * `string_t` los objetos se han cambiado a `char*` objetos
        * Registro de presencia personalizada con el límite de seis `social_manager_presence_title_record` por `social_manager_presence_record`
    * `social_event`
        * Devuelve un `std::vector<xbox_user_id_container>` en lugar de `std::vector<xbox_social_user>`
        * Este vector se puede pasar a la nueva API `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API ahora devuelve un `std::vector<xbox_social_user*>`. Estos punteros se invalidan en la siguiente llamada a `social_manager::do_work()`
        * `get_copy_of_users` devolver un `std::vector<xbox_social_user>` como una copia de los usuarios actuales en el social_user_group al llamador. Llamar a esta función puede afectar a `do_work` tiempo de finalización.
* Ahora el Administrador de redes sociales nunca generará un error después de la inicialización. El Administrador de redes sociales volverá a intentar ATR automáticamente en la desconexión. El `error` y `rta_disconnect_error` eventos han sido desusados y quitados
* Título puede especificar los asignadores de memoria personalizados. Consulte la documentación nueva: [Memoria del Administrador de redes sociales y el rendimiento](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Una UWP Xbox
* Las API TCUI agrega compatibilidad con varios usuarios para aplicaciones UWP una Xbox.  El C++ XSAPI agrega nuevas Windows::System::User ^ los parámetros especifican el usuario y la API de WinRT XSAPI agrega ForUserAsync APIs.
* Ejemplos UWP actualizados para admitir varios usuarios en Xbox

### <a name="other"></a>Otras

* C++ / c++ / WinRT verticalmente.   Pueden encontrar más detalles [aquí](../introduction-to-xbox-live-apis.md)
* Nueva funcionalidad en Agregar y quitar sus propias devoluciones de llamada de registro.  El nivel de diagnóstico se pasarán a la devolución de llamada, por lo que puede ajustar su comportamiento.  Consulte `add_logging_handler` y `remove_logging_handler` en el `microsoft::xbox::services::system::xbox_live_services_settings` espacio de nombres

## <a name="documentation"></a>Documentación
* Hay documentación nueva en el [Manager participan varios jugadores](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md), y [conceptos multijugador](../multiplayer/multiplayer-concepts.md) para Xbox Live.
* El [introducción de Xbox Live](../get-started-with-partner/get-started-with-xbox-live-partner.md) se han reescrito secciones.  Si está creando un nuevo título de Xbox Live habilitado, o si tiene curiosidad acerca de cómo incorporar otras funcionalidades de Xbox Live en tu juego, puede ver los documentos nuevos [aquí](../get-started-with-partner/get-started-with-xbox-live-partner.md).

## <a name="tools"></a>Herramientas
* La consola Xbox Live analizador de seguimiento que puede encontrar en [ https://aka.ms/XboxLiveTools ](https://aka.ms/XboxLiveTools) ahora tiene un modo de certificado.  
* También en el analizador de seguimiento de Xbox Live es la compatibilidad con la consola de varios.  Si se pasa en un seguimiento de Fiddler que contiene las solicitudes HTTP de la consola de varios, se generarán informes independientes para cada uno de ellos.

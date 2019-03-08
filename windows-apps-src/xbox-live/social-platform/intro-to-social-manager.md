---
title: Introducción al administrador de redes sociales
description: Obtenga información acerca de Xbox Live Social Manager API para realizar un seguimiento de los amigos en línea.
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.date: 03/26/2018
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dff3dcfd79fe43ff8af1513a4358bd0ff98b8d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609010"
---
# <a name="introduction-to-social-manager"></a>Introducción al administrador de redes sociales

## <a name="description"></a>Descripción

Xbox Live proporciona un gráfico social enriquecido que pueden usar títulos para diversos escenarios.

Mediante las API de redes sociales en Xbox Live API (XSAPI) para obtener y mantener información acerca de un gráfico social es compleja y mantener actualizada la información puede ser complicado.  No hacerlo correctamente puede provocar problemas de rendimiento, datos obsoletos o está limitando por una llamada a los servicios sociales de Xbox Live con más frecuencia que es necesario.

El Administrador de redes sociales soluciona este problema mediante:

* Creación de una API sencilla para llamar a
* Creación de información actualizada con el servicio en tiempo real de la actividad en segundo plano
* Los desarrolladores pueden llamar a la API del Administrador de redes sociales sincrónicamente sin ningún esfuerzo adicional en el servicio

El Administrador de redes sociales enmascara la complejidad de trabajar con varias suscripciones de ATR y actualizar los datos para los usuarios y permite a los desarrolladores obtener fácilmente el gráfico actualizado que desean crear escenarios interesantes.

Para un vistazo a la memoria del Administrador de redes sociales y el rendimiento características Eche un vistazo a [información general de rendimiento y memoria del Administrador de redes sociales](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>Características

El Administrador de redes sociales proporciona las siguientes características:

* API sociales simplificada
* Gráfico de redes sociales al día
* Control sobre el nivel de detalle de la información mostrada
* Reducir el número de llamadas a servicios de Xbox Live
  * Éste se correlaciona directamente a la reducción de la latencia general de adquisición de datos
* Es seguro para subprocesos
* Eficaz mantener datos actualizados

## <a name="core-concepts"></a>Conceptos básicos

**Gráfico social**: Un *gráfico social* se crea para un usuario local en el dispositivo. Esto crea una estructura que mantiene información acerca de todos un amigos de los usuarios al día.

> [!NOTE]
> En Windows que solo puede haber un usuario local

**Usuario de Xbox Social**: Un *usuario sociales Xbox* es un completo conjunto de datos sociales asociados con un usuario de un grupo

**Grupo de usuarios de Xbox Social**: Un grupo es una colección de usuarios que se usa para cosas como rellenar la interfaz de usuario. Hay dos tipos de grupos

* **Filtrar grupos**: Un grupo de filtro toma de un usuario (llamado) local *gráfico social* y devuelve un conjunto nuevo de forma coherente de usuarios en función de los parámetros de filtro especificado
* **Grupos de usuarios**: Un grupo de usuarios toma una lista de usuarios y devuelve una vista coherente de esos usuarios. Los usuarios pueden estar fuera de la lista de amigos del usuario.

Con el fin de mantener un *grupo de usuarios de redes sociales* arriba hasta la fecha de la función `social_manager::do_work()` debe llamarse cada fotograma.

## <a name="api-overview"></a>Introducción a la API

Las siguientes clases claves se usará con más frecuencia:

### <a name="social-manager"></a>Administrador de redes sociales

* Nombre de clase de la API de C++: social_manager
* WinRT (C#) nombre de clase de API: [SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

Se trata de una clase singleton que puede usarse para obtener **grupos de usuarios de redes sociales de Xbox** que son las vistas descritas anteriormente.

El Administrador de redes sociales mantendrá xbox grupos de usuarios de redes sociales al día y puede filtrar grupos de usuarios mediante la presencia o la relación que el usuario.  Por ejemplo, podría crearse un grupo de usuarios de redes sociales de xbox que contiene todos los amigos del usuario que están en línea y reproducir el título de la actual.  Esto podría mantenerse al día como amigos iniciar o detener la reproducción del título.

### <a name="xbox-social-user-group"></a>Grupo de usuario de redes sociales de Xbox

* Nombre de clase de la API de C++: xbox_social_user_group
* WinRT (C#) nombre de clase de API: [XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

Un grupo de usuarios que cumplen determinados criterios, como se describió anteriormente. Grupos de usuarios de redes sociales de Xbox exponen el tipo de un grupo son, qué usuarios se realiza el seguimiento o el filtro establecido es en ellos y el usuario local que pertenece el grupo.

Puede encontrar una descripción completa de las API del Administrador de redes sociales en la [referencia de API de Xbox Live](https://aka.ms/xboxliveuwpdocs).
También puede encontrar las APIs WinRT en la [Microsoft.Xbox.Services.Social.Manager.Namespace documentación](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)

## <a name="usage"></a>Uso

### <a name="creating-a-social-user-group-from-filters"></a>Creación de un grupo de usuarios de redes sociales de filtros

En este escenario, desea una lista de usuarios de un filtro, como todas las personas que este usuario es posterior o etiquetado como favorito.

#### <a name="source-example-using-the-c-api"></a>Ejemplo de código fuente mediante la API de C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Ejemplo de origen mediante el C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>Eventos devueltos

`local_user_added`(C++) | `LocalUserAdded`(C#)-Desencadena una vez completada la carga del gráfico social de los usuarios. Indica si se ha producido algún error durante la inicialización

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#): Se desencadena cuando se ha creado el grupo de usuarios de redes sociales

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#): Se desencadena cuando se cargan los usuarios en

#### <a name="additional-details"></a>Detalles adicionales

El ejemplo anterior muestra cómo inicializar el Administrador de redes sociales para un usuario, cree un grupo de usuario de redes sociales para ese usuario y mantenerse al día.

Las opciones de filtrado se pueden ver en el `presence_filter` y `relationship_filter` enumeraciones

En el bucle de juego, la `do_work` vistas creadas todas las actualizaciones de función con la instantánea más reciente de los usuarios de ese grupo.

Los usuarios de la vista se pueden obtener mediante una llamada a la `xbox_social_user_group::get_users()` función que devuelve una lista de `xbox_social_user` objetos.  El `xbox_social_user` contiene la información de redes sociales, como nombre de jugador, uri gamerpic, etcetera.

### <a name="create-and-update-a-social-user-group-from-list"></a>Crear y actualizar un grupo de usuarios de redes sociales de lista

En este escenario, desea que la información de una lista de usuarios como usuarios social en una sesión de varios jugadores.

#### <a name="source-example-using-the-c-api"></a>Ejemplo de código fuente mediante la API de C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Ejemplo de origen mediante el C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Eventos devueltos

`local_user_added`(C++) | `LocalUserAdded`(C#)-Desencadena una vez completada la carga del gráfico social de los usuarios. Indica si se ha producido algún error durante la inicialización

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#): Se desencadena cuando se ha creado el grupo de usuarios de redes sociales

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#): Se desencadena cuando se cargan los usuarios en

### <a name="updating-social-user-group-from-list"></a>Actualizar grupo de usuarios de redes sociales de lista

También puede cambiar la lista de usuarios marcados en el grupo de usuarios de redes sociales mediante una llamada a update_social_user_group()

#### <a name="source-example-using-the-c-api"></a>Ejemplo de código fuente mediante la API de C++

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>Ejemplo de origen mediante el C# API

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Eventos devueltos

`social_user_group_updated`(C++) | `SocialUserGroupUpdated`(C#): Se desencadena cuando se complete la actualización de grupo de usuario de redes sociales.

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#): Se desencadena cuando los usuarios se cargan en. Si los usuarios agregados a través de la lista ya están en el gráfico, no se desencadenará este evento

### <a name="using-social-manager-events"></a>Uso de eventos del Administrador de redes sociales

El Administrador de redes sociales también le indicará lo que sucedió en forma de eventos.  Puede usar esos eventos para actualizar la interfaz de usuario o realizar otra lógica.

#### <a name="source-example-using-the-c-api"></a>Ejemplo de código fuente mediante la API de C++

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>Ejemplo de origen mediante el C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>Eventos devueltos

`local_user_added`(C++) | `LocalUserAdded`(C#)-Desencadena una vez completada la carga del gráfico social de los usuarios. Indica si se ha producido algún error durante la inicialización

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#): Se desencadena cuando se ha creado el grupo de usuarios de redes sociales

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#): Se desencadena cuando se cargan los usuarios en

#### <a name="additional-details"></a>Detalles adicionales

Este ejemplo muestra el control adicional que ofrece.  En lugar de depender de los filtros de grupo de usuario de redes sociales para proporcionar una lista actualizada de usuario durante el bucle de juego, se ha inicializado el grafo social fuera del bucle de juego.  A continuación, el título se basa en el `events` devuelto por la `socialManager->do_work()` función.  `events` es una lista de `social_event`y cada `social_event` contiene un cambio en el grafo social que se produjeron durante el último fotograma.  Por ejemplo `profiles_changed`, `users_added`, etcetera.  Puede encontrar más información en el `social_event` documentación de API.

### <a name="cleanup"></a>Limpieza

#### <a name="cleaning-up-social-user-groups"></a>Limpieza de grupos de usuarios de redes sociales

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

Limpia el grupo de usuarios de redes sociales que se creó. Autor de llamada también debe quitar las referencias que tienen a cualquier grupo creado sociales ya que ahora contiene datos obsoletos.

#### <a name="cleaning-up-local-users"></a>Limpieza de los usuarios locales

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

Quitar usuario local elimina el grafo social de los usuarios de carga, así como los grupos de usuario de redes sociales que se crearon con ese usuario.

#### <a name="events-returned"></a>Eventos devueltos

`local_user_removed`(C++) | `LocalUserRemoved`(C#): Se desencadena cuando un usuario local se ha quitado correctamente
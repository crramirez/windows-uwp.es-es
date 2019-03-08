---
title: Roles multijugador
description: Obtenga información sobre el uso de roles para definir los roles de Reproductor de varios jugadores de Xbox Live.
ms.date: 06/29/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, roles de varios jugadores, one, xbox
ms.localizationpriority: medium
ms.openlocfilehash: ac5e7758bd8e068681d1c8dab2d47d11374c2616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662090"
---
# <a name="roles"></a>Roles

Para algunas sesiones de juegos, puede especificar que determinados miembros tengan determinados roles de juego, como soporte técnico, medic, agresión, etcetera. También puede wat para reservar las ranuras de juegos para jugadores que va a rellenar un rol específico de juego. Mediante la característica de roles de Xbox Live, el servicio puede realizar un seguimiento de los jugadores se asignan los roles del juego y aplicar un número máximo de reproductores que puede seleccionar un rol específico de juego.

El uso más común de funciones es determinar el juego roles específicos para esa sesión de juego. Por ejemplo, puede tener un modo de juego que requiere entre 1 y 2 clases de soporte técnico, al menos 1 clase depósito/pesado y no más de 5 clases agresión.

En otro escenario posible, puede especificar que una sesión puede tener exactamente 4 jugadores, hasta 8 espectadores y 1 presentador.

También puede usar roles para reservar las ranuras para amigos al rellenar las ranuras restantes a través de otros medios, como examinar sesiones.

## <a name="role-types"></a>Tipos de rol

Un tipo de rol representa un grupo de definiciones de roles. Cada rol debe definirse como parte de un tipo de rol. Tipos de rol se definen en el documento de la sesión de varios jugadores.

Un miembro solo tener asignado un rol de un tipo de rol determinado. Por ejemplo, si un tipo de rol "class" incluye healers, los depósitos y daños, a continuación, un miembro puede solo asignarse a uno de esos roles.

Puede definir varios tipos de rol y un miembro puede tener asignado un rol de cada tipo de rol. En el escenario anterior, es posible que haya elegido el rol curandero un miembro, pero también se pueden asignar un escuadrón rol de líder, si el rol de líder escuadrón se define en un tipo de rol independiente.

> **Importante:** El SDK de Xbox Live actualmente solo admite un tipo de rol único y una única función por cada miembro.

## <a name="role-type-properties"></a>Propiedades de tipo de rol

Al definir un tipo de rol, debe especificar la siguiente información para los tipos de rol:

* El nombre del tipo de rol. El nombre debe ser minúscula y alfanuméricos y no más de 100 caracteres.
* Si los roles definidos en el tipo de rol propietario administrado o no.
* Si las propiedades de los roles definidos en el tipo de rol se pueden cambiar durante la vigencia de la sesión.
* Las definiciones de roles que se incluyen en el tipo de rol.

Si un tipo de rol es propietario administrada, que significa que solo los miembros que son propietarios de la sesión pueden asignar roles de ese tipo a los miembros. Si el tipo de rol no es propietario administrada, los miembros pueden asignar roles a sí mismos.

Solo puede especificar que un tipo de rol es administrado en las sesiones que tienen la capacidad de "hasOwners" establecer el propietario.

> El SDK de Xbox Live no admite actualmente los propietarios de asignación de roles a otros miembros.

## <a name="role-properties"></a>Propiedades de rol

Al definir un rol, debe especificar la siguiente información para cada rol:

* El nombre del rol. El nombre debe ser minúscula y alfanuméricos y no más de 100 caracteres.
* El número máximo de miembros que se permiten para asumir el rol. Debe ser mayor que cero.
* Número de miembros que se debe rellenar el rol de destino. El destino debe ser mayor que cero y menor o igual que el número máximo de miembros permitido para asumir el rol.

Cuando un miembro de la sesión se le asigna un rol, esa información se registra en las funciones miembro en el documento de la sesión de varios jugadores.

El servicio aplica el número máximo de miembros que se pueden asignar a un rol, pero no aplica el número de destino.

## <a name="create-roles"></a>Crear roles

Funciones y tipos de rol suelen definirse en el [plantilla sesión](service-configuration/session-templates.md). El servicio admite el rol y la definición de tipo de rol durante la creación de la sesión, pero no lo hace el SDK de Xbox Live.

### <a name="define-role-types-and-roles-in-a-session-template"></a>Definir tipos de funciones y roles en una plantilla de sesión

Puede definir roles y tipos de rol cuando se crea una plantilla de sesión durante la configuración de Xbox Live.

La información de tipo y el rol del rol se especifica como un elemento de nivel de base "roleTypes" en la plantilla de sesión, en el formato siguiente:

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>Recuperar información de roles para una sesión de varios jugadores

Puede obtener la información sobre el rol de tipos, funciones y cuántos miembros están asignados a cada función desde una sesión de varios jugadores, o desde un identificador de búsqueda con varios jugadores.

En el SDK de Xbox Live, se almacena información sobre los tipos de rol y roles dentro de una estructura de mapa. El uso de C++ APIs el `unordered_map` clase y las APIs de WinRT usa la `IMapView` clase.

### <a name="get-the-role-information-from-a-search-handle"></a>Obtener la información de roles de un identificador de búsqueda

En el `multiplayer_search_handle_details` objeto devuelto desde una solicitud de búsqueda, puede obtener la información de tipo de rol al indizar el `role_types` mapa con el nombre del tipo de rol que le interesa.

Esto devuelve un `multiplayer_role_type` objeto. Puede obtener los roles mediante la indización de la `roles` asignar, que devuelve un `multiplayer_role_info` objeto.

El `multiplayer_role_info` objeto contiene información acerca de la función, incluidos `max_members_count`, `member_xbox_user_ids`, `members_count`, y `target_count`.

### <a name="get-the-role-information-from-a-search-handle"></a>Obtener la información de roles de un identificador de búsqueda

El flujo para obtener información de la función de una sesión es similar al flujo de obtención de información de un identificador de búsqueda, pero se utilizan algunas clases diferentes.

En el `multiplayer_session` , puede obtener la información de tipo de rol haciendo referencia al objeto la `session_role_types` objeto, que es un `multiplayer_session_role_types` clase. En este objeto, se puede indizar el `role_types` mapa con el nombre del tipo de rol que le interesa.

Esto devuelve un `multiplayer_role_type` objeto. Puede obtener los roles mediante la indización de la `roles` asignar, que devuelve un `multiplayer_role_info` objeto.

El `multiplayer_role_info` objeto contiene información acerca de la función, incluidos `max_members_count`, `member_xbox_user_ids`, `members_count`, y `target_count`.

## <a name="change-mutable-role-settings"></a>Cambiar la configuración de rol mutable

Si un tipo de función indica que algunas opciones de rol se pueden cambiar (mutable), puede usar el `multiplayer_role_type.set_roles()` método para modificar la configuración mutable. Solo los miembros que están marcados como propietarios de la sesión pueden cambiar la configuración de roles.

## <a name="assign-a-role-to-a-member"></a>Asignar un rol a un miembro

Actualmente, solo un miembro puede asignar su propio rol en el SDK de Xbox Live. En el `multiplayer_session` objeto, puede llamar a la `set_current_user_role_info(role_type, role_name)` método para especificar el tipo de rol y el rol para el miembro actual.

Si el rol ya está completo cuando se intenta escribir la sesión en el servicio, MPSD rechazará la operación de escritura.

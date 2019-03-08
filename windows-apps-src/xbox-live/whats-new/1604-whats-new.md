---
title: Novedades para el SDK de Xbox Live - abril de 2016
description: Novedades para el SDK de Xbox Live - abril de 2016
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9ce63a0174fa0c4158764b8bca2443d58d0aefd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660770"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>Novedades para el SDK de Xbox Live - abril de 2016

Consulte la [What's New - marzo de 2016](1603-whats-new.md) artículo para lo que se agregó en 1603

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico
El SDK de Xbox Live es compatible con Windows 10 RTM [versión 10.0.10240] y Visual Studio 2015 RTM [versión 14.0.23107.0].

## <a name="tournaments"></a>Torneos
- La herramienta de torneos de Xbox Live es ahora incluye con el SDK.  Puede verlo en el directorio de herramientas, junto con información sobre cómo usarlo.
- Las API para torneos también están disponibles.  Vea el espacio de nombres Xbox::Services::Tournaments
- Documentación también está disponible en la Guía de programación.

## <a name="documentation"></a>Documentación
- [Guía de solución de problemas de inicio de sesión de](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md) se enumeran algunas estrategias generales para depurar errores de inicio de sesión, así como los pasos a seguir en función de código de error.
- El [Marketplace](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) documentos para los desarrolladores de Xbox One sólo se encuentra ahora en la Guía de programación.  Los desarrolladores de UWP deben continuar consultar el centro de partners para obtener documentación sobre el almacén.
- Hay un [XDK a UWP porting guía](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) puede hacer referencia a si está interesado en poner un título de Xbox One a la plataforma Universal de Windows.
- Consulte la [limitación específica](../using-xbox-live/best-practices/fine-grained-rate-limiting.md) artículo para obtener una descripción de cómo estos se aplican para varios puntos de conexión de servicio de Xbox Live y escenarios, así como información acerca de cuáles son los límites.

## <a name="multiplayer-manager"></a>Administrador de varios jugadores
El [Manager participan varios jugadores](../multiplayer/multiplayer-manager.md) ya no está en experimental.  Hemos incorporado los comentarios de los desarrolladores que usan esta API y realizar algunas de las API más coherentes entre sí.  Use el Administrador de varios jugadores como punto de partida al realizar el desarrollo de varios jugadores, ya que proporciona una API más sencilla que muchas de las complejidades de la API de varios jugadores 2015 administra automáticamente.

En las siguientes secciones, se muestran algunas de las nuevas características de la API, así como un número pequeño de cambios importantes.

#### <a name="completed-events"></a>Eventos completados
Todas las API ahora tienen su correspondiente``` _competed``` eventos y todos los eventos se activan independientemente del éxito o error. El comportamiento anterior era que solo desencadena tras un error, realizar acciones en el título. Y puesto que se procesan por lotes las llamadas, significa que al finalizar, el título obtendrán varios ```_competed``` eventos.

| API | Evento devuelto |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

Lo mismo se aplica ```game_session()```.

#### <a name="application-defined-context"></a>Contexto definido de la aplicación
Ahora puede pasar en un contexto opcional definido por la aplicación para cada métodos set_ * correlacionar el evento multijugador a la llamada de iniciador.
Por ejemplo

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

El contexto, a continuación, se devolvería al título a través de la ```_completed``` eventos.

#### <a name="breaking-changes"></a>Cambios importantes

1.  Ambos invitar a las API (```invite_friends``` & ```invite_users```) ahora son sincrónicas. Tras la finalización, devuelve un evento invite_sent.

2.  ```write_synchronized_properties_and_commit``` se ha cambiado a ```set_synchronized_properties```. Tras la finalización, devuelve un ```session_synchronized_property_write_completed``` eventos.

3.  ```write_synchronized_host_and_commit``` se ha cambiado a ```set_synchronized_host```. Tras la finalización, devuelve un ```synchronized_host_write_completed``` eventos.

4.  En ```lobby_session()```

  *Removed*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *Added*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  En ```game_session()```

  *Removed*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *Added*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  Cambiar de tipos de evento:

  *Removed*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *Renamed*

  ```write_synchronized_properties_completed``` ha cambiado a ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` ha cambiado a ```synchronized_host_write_completed```

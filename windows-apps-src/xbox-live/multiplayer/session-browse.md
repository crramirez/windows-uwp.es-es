---
title: Examinar sesiones de varios jugadores
description: Obtenga información sobre cómo implementar examinar sesiones de varios jugadores mediante el uso de Xbox Live para varios jugadores.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.date: 10/16/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 579c71ef9266fb9a1ee4ef0538d1beffec0bb4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660680"
---
# <a name="multiplayer-session-browse"></a>Examinar sesiones de varios jugadores

Sesión de varios jugadores examinar es una característica nueva presentada en noviembre de 2016 que permite un título al consultar una lista de sesiones juegos multijugador abiertas que cumplen los criterios especificados.

## <a name="what-is-session-browse"></a>¿Qué es examinar sesiones?

En un escenario de exploración de la sesión, es capaz de recuperar una lista de las sesiones de juego puede unir un reproductor en un juego. Cada entrada de la sesión de esta lista contiene algunos metadatos adicionales acerca del juego, que puede usar un reproductor que les ayuden a seleccionar qué sesión para unirse.  También puede filtrar la lista de sesiones basándose en los metadatos. Una vez que el Reproductor ve una sesión de juego que resulte atractiva para ellos, puede unirse a la sesión.

Un Reproductor también puede crear una nueva sesión de juego y utiliza Examinar sesiones para contratar reproductores adicionales en lugar de depender de contactos.

Examinar sesiones difiere de los escenarios de contactos tradicional en que el Reproductor selecciona automáticamente qué sesión de juego que desean unirse, mientras que en los contactos, el Reproductor normalmente hace clic en un botón "Buscar un juego" que intenta colocar automáticamente el Reproductor en un sesión de juego adecuado. Aunque examinar sesiones es un proceso manual y más lento que no siempre puede seleccionar el juego objetivamente mejor, proporciona un mayor control para el Reproductor y se puede percibir como elegir el juego de mejor forma subjetiva.

Es habitual incluir ambos escenarios de sesión de exploración y contactos en juegos. Normalmente los contactos se usan para reproducir habitualmente los modos de juego, mientras examinar sesiones se utiliza para juegos personalizados.

**Ejemplo:** Juan puede estar interesado en reproducción en curso una batalla hero arena estilo de varios jugadores, pero desea jugar a un juego donde todos los reproductores de seleccionan su héroe aleatoriamente. Puede recuperar una lista de las sesiones abiertas de juego y buscar las que incluyen a "héroes aleatorios" en su descripción, o si la interfaz de usuario de juego lo permite, puede seleccionar el modo de juego "aleatorio hero" y recuperar solo las sesiones que están etiquetadas para indicar que son "RandomHero" gam es.

Cuando se descubre un juego que le gusta, une el juego. Cuando demasiadas personas han unido a la sesión, el host de la sesión de juego puede iniciar el juego.

### <a name="roles"></a>Roles

Un juego en Examinar sesiones desear reclutar los reproductores para roles específicos. Por ejemplo, un reproductor desear crear una sesión de juego que especifica que la sesión contiene las clases de agresión no más de 5, pero debe contener al menos 2 roles curandero y rol de los depósitos de al menos 1.

Cuando se aplica otro reproductor para la sesión, previamente pueden seleccionar su rol y el servicio no permitirá que se unan a la sesión si no hay ninguna ranura abierta para el rol que han seleccionado.

Otro ejemplo sería si desea que un reproductor reservar 2 ranuras para sus amigos a unirse, el juego puede especificar un rol de "friends" y solo los jugadores que son amigos con el host de sesión pueden rellenar el 2 ranuras dedicadas a la función "friends".

Para obtener más información acerca de los roles, consulte [roles multijugador](multiplayer-roles.md).



## <a name="how-does-session-browse-work"></a>¿Cómo funciona examinar sesiones?

Examinar sesiones funciona principalmente sobre el uso de identificadores de búsqueda. Un identificador de búsqueda es un paquete de flujo de datos que contiene una referencia a la sesión, así como metadatos adicionales acerca de la sesión, es decir, los atributos de búsqueda.

Cuando crea un título examinar una nueva sesión de juego que es apta para la sesión, crea un identificador de búsqueda para la sesión. El identificador de búsqueda se almacena en varios jugadores servicio de directorio (MPSD), que mantiene los identificadores de búsqueda para el título.

Cuando se necesita un título recuperar una lista de sesiones, el título puede enviar una consulta de búsqueda a MPSD, que devolverá una lista de identificadores de búsqueda que cumplen los criterios de búsqueda. El título, a continuación, puede usar la lista de sesiones para mostrar una lista de juegos puede unir al Reproductor.

Cuando una sesión esté lleno o, en caso contrario, no se pueden combinar, un título puede quitar el identificador de búsqueda de MPSD para que la sesión ya no se mostrará en las consultas de exploración de sesión.

>[!NOTE]
> Identificadores de búsqueda están diseñados para utilizarse cuando se muestra una lista de sesiones que se presentan al usuario. Uso de identificadores de búsqueda para contactos en segundo plano no es válido y en su lugar, considere el uso de [SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>Configurar una sesión para examinar sesiones

Para poder usar los identificadores de búsqueda para una sesión, la sesión debe tener el siguiente conjunto de capacidades en true:

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> El `userAuthorizationStyle` capacidad sólo es necesarios para juegos UWP, pero se recomienda su implementación para Xbox Live de todos los juegos, incluidos los juegos XDK, ya que garantiza la portabilidad futuras.

>[!NOTE]
> Establecer el `userAuthorizationStyle` valores predeterminados de la capacidad del `readRestriction` y `joinRestriction` de la sesión para `local` en lugar de `none`. Esto significa que los títulos deben usar identificadores de búsqueda o transferir identificadores para participar en una sesión de juego.

Puede establecer estas funcionalidades en la plantilla de sesión al configurar los servicios de Xbox Live.

Para examinar sesiones, solo se deben crear identificadores de búsqueda en las sesiones que se usará para el juego real, no para las sesiones de lobby.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>¿Qué significa ser propietario de una sesión?

Mientras la sesión de juego muchos tipos, como SmartMatch o un solo juego, amigos no requieren un propietario, para las sesiones de exploración de sesión puede tener un propietario. 

Tener una sesión administrada por el propietario tiene algunas ventajas para el propietario. Los propietarios pueden quitar a otros miembros de la sesión o cambiar el estado de la propiedad de otros miembros.

Para poder usar los propietarios de una sesión, la sesión debe tener el siguiente conjunto de capacidades en true:

* `hasOwners`

Si un propietario de una sesión tiene un miembro de Xbox Live bloqueado, ese miembro no puede unirse a la sesión.

Cuando se usa [roles multijugador](multiplayer-roles.md), puede establecerla, solo los propietarios pueden asignar roles a los usuarios.

Si todos los propietarios de abandonar una sesión, el servicio realiza la acción en la sesión que se basa el `ownershipPolicy.migration` directiva que se define para la sesión. Si la directiva es "más antigua", el Reproductor que ha sido más tiempo en la sesión se establece para ser el propietario de la nueva. Si la directiva es "endsession" (el valor predeterminado si no se proporciona), el servicio finaliza la sesión y quita todos los jugadores restantes de la sesión.


## <a name="search-handles"></a>Identificadores de búsqueda

Un identificador de búsqueda se almacena en MSPD como una estructura JSON. Además de contener una referencia a la sesión, identificadores de búsqueda también contienen metadatos adicionales para realizar búsquedas, conocido como atributos de búsqueda.

Una sesión solo puede tener un identificador de búsqueda que se creó para él en cualquier momento.

Para crear un identificador de búsqueda para una sesión mediante las API de Xbox Live, cree primero un `multiplayer::multiplayer_search_handle_request` de objetos y, a continuación, pasar ese objeto a la `multiplayer::multiplayer_service::set_search_handle()` método.

### <a name="search-attributes"></a>Atributos de búsqueda

Atributos de búsqueda consta de los siguientes componentes:

`tags` -Las etiquetas son descriptores de cadena que se pueden usar para clasificar una sesión de juego, similar a un hashtag. Etiquetas deben empezar por una letra, no pueden contener espacios y deben tener menos de 100 caracteres.
Etiquetas de ejemplo: "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` -Las cadenas son variables de texto, y los nombres de la cadena deben comenzar con una letra, no pueden contener espacios y deben tener menos de 100 caracteres.

Metadatos de la cadena de ejemplo: "Armas"="cuchillos + pistolas + fusiles", "Nombredemapa" = "UrbanCityAssault", "Descripción"="divertido juego ocasional, le damos la bienvenida de nuevas personas".

`numbers` -Números son variables numéricas, y los nombres de número deben empezar por una letra, no pueden contener espacios y deben tener menos de 100 caracteres. Las API de Xbox Live recuperar valores numéricos como tipo float.

Metadatos de número de ejemplo: "MinLevel" = 25, "MaxRank" = 10.

>**Nota:** Se conservan las mayúsculas y minúsculas de las etiquetas y valores de cadena en el servicio, pero debe usar la función tolower() al consultar las etiquetas. Esto significa que las etiquetas y valores de cadena son actualmente todos los trata como en minúsculas, incluso si contienen caracteres en mayúsculas.

En las API de Xbox Live, puede establecer los atributos de búsqueda mediante el `set_tags()`, `set_stringsmetadata()`, y `set_numbers_metadata()` métodos de un `multiplayer_search_handle_request` objeto.


### <a name="additional-details"></a>Detalles adicionales

Cuando se recupera un identificador de búsqueda, los resultados también incluyen más datos útiles acerca de la sesión, tal como si se cierra la sesión, ¿hay alguna restricción de combinación en la sesión, etcetera.

En las API de Xbox Live, estos detalles, junto con los atributos de búsqueda, se incluyen en el `multiplayer_search_handle_details` que se devuelven después de una consulta de búsqueda.

### <a name="remove-a-search-handle"></a>Quitar un identificador de búsqueda

Cuando desea quitar una sesión de exploración de la sesión, como cuando se completa, la sesión o se cierra la sesión, puede eliminar el identificador de búsqueda.

En las API de Xbox Live, puede usar el `multiplayer_service::clear_search_handle()` método para quitar un identificador de búsqueda.

### <a name="example-create-a-search-handle-with-metadata"></a>Por ejemplo: Crear un identificador de búsqueda con metadatos

El código siguiente muestra cómo crear un identificador de búsqueda para una sesión mediante las API de varios jugadores C++ Xbox Live.

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>Crear una consulta de búsqueda para las sesiones

Al recuperar una lista de identificadores de búsqueda, puede usar una consulta de búsqueda para restringir los resultados a las sesiones que cumplen criterios específicos.

La sintaxis de consulta de búsqueda es un [OData](https://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) estilo de sintaxis, con solo los siguientes operadores admitidos:

 Operador | Descripción
 --- | ---
 eq | Es igual a
 ne | No es igual a
 gt | Mayor que
 ge | Mayor o igual
 lt | Menor que
 le | Menor o igual
 y | AND lógico
 o bien | lógico o (vea la nota siguiente)

También puede utilizar expresiones lambda y `tolower` canónica. No hay otras funciones de OData se admiten actualmente.

Cuando se buscan las etiquetas o los valores de cadena, debe usar la función 'tolower' en la consulta de búsqueda, como el servicio actualmente solo admite la búsqueda de cadenas en minúsculas.

El servicio Xbox Live solo devuelve los 100 primeros resultados que coinciden con la consulta de búsqueda. El juego debe permitir que los reproductores para refinar su consulta de búsqueda si los resultados son demasiado amplios.

>[!NOTE]
>  OR lógico se admite en consultas de la cadena de filtro; Sin embargo, solo un OR se permite y debe ser en la raíz de la consulta. No puede tener varios ORs en la consulta, ni puede crear una consulta que daría lugar a o no está en la parte superior nivel la mayoría de la estructura de consulta.

### <a name="search-handle-query-examples"></a>Ejemplos de consultas de identificador de búsqueda

En una llamada REST, "Filtro" es donde se especificaría una cadena de lenguaje de filtro de OData que se ejecutará en la consulta con todos los identificadores de búsqueda.  
En las API de varios jugadores 2015, puede especificar la cadena de filtro de búsqueda en el *searchFilter* parámetro de la `multiplayer_service.get_search_handles()` método.  

Actualmente, se admiten los siguientes escenarios de filtro:

 Filtrar por | Cadena de filtro de búsqueda
 --- | ---
 Un xuid único miembro '1234566' | "session/memberXuids/any(d:d eq '1234566')"
 Un xuid único propietario '1234566' | "session/ownerXuids/any(d:d eq '1234566')"
 Una cadena 'forzacarclass' igual a 'classb' | "tolower(strings/forzacarclass) eq 'classb'"
 Un número 'forzaskill' igual a 6 | "números/forzaskill eq 6"
 Un número 'halokdratio' es mayor que 1,5 | "números/halokdratio gt 1.5"
 Una etiqueta 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 Sesiones que no contienen la etiqueta 'cursingallowed' | "tags/any(d:tolower(d) ne 'cursingallowed')"
 Las sesiones que no contienen un número 'rank', que es igual a 0 | "numbers/rank ne 0"
 Las sesiones que no contienen una cadena 'forzacarclass' que es igual a 'classa' | "tolower(strings/forzacarclass) ne 'classa'"
 Una etiqueta coolpeopleonly y un número 'halokdratio' igual a 7,5 | "tags/any(d:tolower(d) eq 'coolpeopleonly') eq true and numbers/halokdratio eq 7.5"
 Un número 'halodkratio' mayor o igual a 1.5, un número 'rank' menor que 60 y un número 'customnumbervalue' menor o igual a 5 | "números/halokdratio ge 1,5 y números y la clasificación lt 60 y números/customnumbervalue le 5"
 Un identificador de logro '123456' | "achievementIds/any(d:d eq '123456')"
 El código de idioma 'en' | "lenguaje eq 'en'"
 Hora programada, devuelve todos los programado veces menor o igual a la hora especificada | "le sesión/scheduledTime" 2009-06-15T13:45:30.0900000Z'"
 Una vez registrada, registrado devuelve todas las veces menor que el tiempo especificado | "sesión/postedtime lt ' 2009-06-15T13:45:30.0900000Z'"
 Estado de sesión de registro | "registrationState/sesión eq 'registrada'"
 Donde el número de miembros de la sesión es igual a 5 | "session/membersCount eq 5"
 Donde el número de destino de miembros de sesión es mayor que 1 | "gt sesión/targetMembersCount 1"
 Donde el número máximo de miembros de la sesión es menor que 3 | "sesión/maxMembersCount lt 3"
 Donde la diferencia entre el recuento de destino del miembro de sesión y el número de miembros de la sesión es menor o igual a 5 | "session/targetMembersCountRemaining le 5"
 Donde la diferencia entre el recuento máximo de miembros de la sesión y el número de miembros de la sesión es mayor que 2 | "gt sesión/maxMembersCountRemaining 2"
 Donde la diferencia entre el número de destino de miembros de sesión y el número de miembros de la sesión es menor o igual que 15.</br> Si el rol no tiene un destino especificado, a continuación, esta consulta filtra contra la diferencia entre el recuento máximo de miembros de la sesión y el número de miembros de la sesión. | "sesión o porque necesitan el 15"
 Rol "confirmado" del tipo de rol "lfg" donde el número de miembros con ese rol es igual a 5 | "/ roles/lfg/confirmar/recuento de sesiones eq 5"
 Rol de "confirmado" del tipo de rol "lfg" donde el destino de ese rol es mayor que 1.</br> Si el rol no tiene un destino especificado, se usa el valor máximo de la función. | "gt/roles/lfg/confirmar/destino de la sesión 1"
 Rol "confirmado" de la función de tipo "lfg" donde la diferencia entre el destino de la función y el número de miembros con ese rol es menor o igual que 15.</br> Si el rol no tiene un destino especificado, a continuación, esta consulta filtra contra la diferencia entre el número máximo de la función y el número de miembros con ese rol. | "el 15 de sesión/roles/lfg/confirmar/necesidades"
 Todos los identificadores de búsqueda que apuntan a una sesión que contiene una palabra clave determinada | "session/keywords/any(d:tolower(d) eq 'level2')"
 Todos los identificadores de búsqueda que apuntan a una sesión que pertenecen a un ¿scid determinado | "session/scid eq '151512315'"
 Todos los identificadores de búsqueda que apuntan a una sesión que usa un nombre de plantilla en particular | "session/templateName eq 'mytemplate1'"
 Todos los identificadores que tienen la etiqueta 'elite' o tienen un número 'pistolas' es mayores que 15 y 'clan igual a 'púrpura' de la cadena de búsqueda | "tags/any(a:tolower(a) eq 'elite') o número/pistolas gt 15 y cadena/clan eq 'púrpura'"

### <a name="refreshing-search-results"></a>Actualizar los resultados de búsqueda

 El juego debe evitar la actualización automática de una lista de sesiones, pero en su lugar, proporcionan la interfaz de usuario que permite que un reproductor actualizar manualmente la lista (posiblemente después refinar los criterios de búsqueda para filtrar los resultados de mejor).

 Si un Reproductor intenta unirse a una sesión, pero esa sesión está completa o cerrada, su juego debe actualizar también los resultados de búsqueda.

 Demasiadas de búsqueda de actualizaciones pueden dar lugar a limitación del servicio, por lo que el título debe limitar la velocidad a la que se puede actualizar la consulta.

 Para reducir el volumen de llamadas de servicio, identificadores de búsqueda que incluyen propiedades de sesión personalizada que se pueden usar para almacenar y consultar los atributos cambian con rapidez de sesión. Estos atributos no deben almacenarse en atributos de búsqueda.

### <a name="example-query-for-search-handles"></a>Ejemplo: consulta para los identificadores de búsqueda

 El código siguiente muestra cómo consultar los identificadores de búsqueda. La API devuelve una colección de `multiplayer_search_handle_details` objetos que representan todos los identificadores de búsqueda que coinciden con la consulta.

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>Participar en una sesión mediante el uso de un identificador de búsqueda

Una vez que haya recuperado un identificador de búsqueda para una sesión que desea unirse, el título debe usar `MultiplayerService::WriteSessionByHandleAsync()` o `multiplayer_service::write_session_by_handle()` se agreguen a la sesión.

> [!NOTE]
> El `WriteSessionAsync()` y `write_session()` no se puede usar los métodos para participar en una sesión de exploración de la sesión.

El código siguiente muestra cómo unirse a una sesión después de recuperar un identificador de búsqueda.

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```

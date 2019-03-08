---
title: Marcadores
description: Obtenga información sobre cómo usar marcadores de Xbox Live para comparar los jugadores.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.date: 09/28/2018
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7e30b99418fda614a888d9269548cdc57a88a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662030"
---
# <a name="leaderboards"></a>Marcadores

## <a name="introduction"></a>Introducción

Como se describe en [información general sobre la plataforma de datos](../data-platform/data-platform.md), marcadores son una excelente manera de fomentar la competencia entre los jugadores y mantener los jugadores implicados en probar atacar su mejor resultado anterior, así como de sus amigos.

Marcadores para [destacados estadísticas](stats2017.md#configured-stats-and-featured-leaderboards) son siempre se muestran en juego de concentrador de un título y a veces se muestra como parte de la interfaz de usuario para un título cuando está anclado a la página principal. También puede utilizar las estadísticas con las características configuradas para crear tablas de clasificación dentro de su título.

## <a name="choosing-good-leaderboards"></a>Elegir buena marcadores

Como se describe en [Reproductor estadísticas](player-stats.md), un marcador corresponde a un estado que ha definido.  Debe elegir los marcadores que corresponden a un logro que un Reproductor puede trabajar para mejorar.

Por ejemplo, mejor tiempo de vuelta en un juego de carreras es un buen marcador, dado que los jugadores querrá trabajar para mejorar su mejor momento de vuelta.  Otros ejemplos son Kill antepasado relación de tiro al blanco, o tamaño máximo del cuadro combinado en un juego de combate.

## <a name="when-to-display-leaderboards"></a>Cuándo se debe mostrar marcadores

Tiene la capacidad para mostrar marcadores en cualquier momento en el título.  Debe elegir una hora cuando una tabla de clasificación no interfiera con el juego o el flujo de su título.  Entre los ciclos y después las coincidencias son ambos disfruten!.

## <a name="how-to-display-leaderboards"></a>Cómo mostrar marcadores

Hay numerosas opciones para mostrar marcadores proporcionado en el SDK de Xbox Live.  Si usas Unity con el programa de creadores de Xbox Live, puede comenzar con un prefabricado de marcador para mostrar los datos de marcador.  Consulte la [configurar Xbox Live en Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) artículo para obtener información específica.

Si está codificando directamente con el SDK de Xbox Live, siga leyendo para obtener información acerca de la API que puede usar.

## <a name="programming-guide"></a>Guía de programación

Hay varias APIs marcador puede usar para obtener el estado actual de un marcador.  Todas las API son asincrónicas y no se bloquean.  Podría realizar una solicitud para obtener datos de la tabla de clasificación y continuar el procesamiento de juego habitual.  Cuando los resultados de la tabla de clasificación se devuelven desde el servicio, puede mostrar los resultados en el momento adecuado.

Debe solicitar los datos de marcador en el servicio, ligeramente delante de cuando desea mostrar, para que los jugadores no estén bloqueados esperando el marcador mostrar.

## <a name="leaderboards-2013-apis"></a>API de marcadores de 2013

Puede ver el `leaderboard_service` espacio de nombres para todas las API de marcador de estadísticas de 2013.

<table>

<tr>
<td>C++ API</td><td>Descripción</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>Versión más básica de la API.  Esto devolverá los valores de marcador para el marcador especificado, empezando desde el Reproductor en la parte superior del marcador.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRT C# de código: obtener un marcador para un único marcador que especifica un identificador de configuración de servicio y un nombre de marcador.</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>Esta API proporciona más flexibilidad, puede especificar el rango (posición) que desea mostrar, así como un valor máximo de elementos que se va a devolver.  Por ejemplo usaría esta API si desea mostrar el marcador a partir de la posición 1000.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRT C# de código: obtener una página de resultados de la tabla de clasificación para un único marcador asigne un identificador de configuración de servicio y un nombre de marcador, tabla de clasificación de resultados se inician en el rango de "skipToRank".</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

Utilícelo si desea omitir el marcador a un determinado usuario.  Un `XUID` es un identificador único para cada usuario de Xbox.  Puede obtener para el usuario ha iniciado sesión, o cualquiera de sus amigos y pasar a esta función.

</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRT C# de código: obtener un marcador comenzando en un reproductor especificado, independientemente del jugador rango o puntuación, ordenados por el rango percentil del Reproductor</td>

</tr>

</table>

## <a name="2013-c-example"></a>Ejemplo de C++ de 2013

Cuando se usa la capa de API de C++, a continuación, puede establecer una devolución de llamada que se invocará una vez que los resultados de la tabla de clasificación se devuelven desde el servicio.  Le mostraremos un ejemplo de este tema.

Si no está familiarizado con el `pplx::task` se devuelven de estas API, este es un objeto de tarea asincrónica desde la biblioteca de programación paralela en Microsoft (PPL).  Puede aprender más sobre esto en [ https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks ](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks).

La sección siguiente muestra cómo puede recuperar los resultados de la tabla de clasificación y usarlos.

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. Crear una tarea asincrónica para recuperar los resultados de la tabla de clasificación

El primer paso es llamar al servicio de marcadores para recuperar los resultados de un marcador determinado.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. Programa de instalación de una devolución de llamada

Puede configurar un [tarea de continuación](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) llamarse una vez que se devuelven los resultados de la tabla de clasificación.  Hacerlo como sigue a continuación.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

Se llama a esta tarea de continuación en el contexto del objeto que lo invocó originalmente y recibe el ```leaderboard_result``` que puede mostrarse de manera que se adapte a su título.


### <a name="3-display-leaderboard"></a>3. Mostrar marcador

Los datos de marcador se encuentran en ```leaderboard_result``` y los campos son autoexplicativos.  Consulte a continuación para obtener un ejemplo.

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```

## <a name="2013-winrt-c-example"></a>2013 WinRT C# ejemplo

Cuando se usa el WinRT C# capa no deberá realizar una devolución de llamada independiente de la tarea y le basta con necesidad de usar el `await` palabra clave al llamar al servicio de la tabla de clasificación.

### <a name="1-access-the-leaderboardservice"></a>1. Obtener acceso a la LeaderboardService

El `LeaderboardService` pueden obtenerse de la `XboxLiveContext` creado al iniciar sesión en un usuario para el juego, ya que lo necesitará para llamar a los datos de marcador.

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2. Llame a la LeaderboardService

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3. Recuperar datos de marcador

`GetLeaderboardAsync()` Devuelve un `LeaderboardResult` que contendrá las estadísticas de rellenar el marcador con nombre.

`LeaderboardResult` tiene varias funciones y propiedades para facilitar la lectura de datos de marcador.

|Propiedad  |Descripción  |
|---------|---------|
|IAsyncOperation pública<LeaderboardResult> GetNextAsync (uint maxItems);     |Recupera el siguiente conjunto de rangos hasta el número del parámetro maxItems. Esto es esencialmente otra llamada a `GetLeaderboard()`         |
|LeaderboardQuery GetNextQuery() pública;     |Recupera el LeaderboardQuery que podría utilizarse para realizar la llamada de marcador para recuperar el siguiente conjunto de datos.         |
|public bool HasNext {get;}    |designa si hay más filas de tabla de clasificación para recuperar         |
|IReadOnlyList pública<LeaderboardRow> filas {get;}     | Filas que contienen datos de tabla de líderes por rango        |
|IReadOnlyList pública<LeaderboardColumn> columnas {get;}     | La lista de columnas que componen la tabla de líderes        |
|uint pública TotalRowCount {get;}     | Cantidad total de filas de tabla de clasificación        |
|cadena pública DisplayName {get;}     | Nombre que se mostrará para el marcador       |

Datos de marcador se proporcionará una página a la vez. Puede recorrer en bucle la `LeaderboardResult` filas y columnas para recuperar los datos.  
Use la `HasNext` booleano y `GetNextAsync()` función para recuperar las páginas posteriores de los datos de marcador.

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>Tabla de líderes de 2017

Para realizar llamadas al servicio de marcador de 2017 de estadísticas que usará el `StatisticManager` marcador API en lugar de la `LeaderboardService` las API de marcador.  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>Ejemplo de C++ de 2017

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1. Obtener una instancia Singleton de la stats_manager

Para poder invocar la `stats_manager` funciones deberá establecer una variable a su instancia de Singleton.

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2. Crear un LeaderboardQuery

El `leaderboard_query` determinará la cantidad, en orden, y el punto de inicio de los datos devueltos por la llamada del marcador.

Un `leaderboard_query` tiene unos cuantos atributos que se pueden establecer que esto afectará a los datos devueltos:

|Propiedad |Descripción  |
|---------|---------|
|m_skipResultToRank     |Esta variable uint determinará qué clasificación de los datos de marcador se iniciará con cuando se devuelven. Las clasificaciones se inician en el rango 1.         |
|m_skipResultToMe     |Si establece este valor booleano de True hará que los datos de la tabla de clasificación devueltos al principio de la `XboxLiveUser` usado en el `get_leaderboard()` llamar.  |
|m_order     |Enumeraciones de tipo `xbox::services::leaderboard::sort_order` tienen dos valores posibles, en orden ascendente y descendente. Si establece esta variable para la consulta determina el criterio de ordenación de la tabla de clasificación.        |
|m_maxItems     |Este uint determina el número máximo de filas que se devolverán por llamada a `get_leaderboard` o `get_social_leaderboard()`.         |

`leaderboard_query` tiene varias función de conjunto puede usar para asignar el valor para estas propiedades. El código siguiente le mostrará cómo configurar su `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

Esta consulta devolvería diez filas de la tabla de líderes comenzando en el 100 clasifican individuales.

> [!WARNING]
> Establecer SkipResultToRank mayor que el número de jugadores dentro de la tabla de líderes, hará que los datos de marcador que se devolverán con cero filas.

### <a name="3-call-getleaderboard"></a>3. Llamada get_leaderboard

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> El `statName` usado en el `GetLeaderboard()` llamada debe ser el mismo que el nombre de un estado configurado para el título en [centro de partners](https://partner.microsoft.com/dashboard), que distingue mayúsculas de minúsculas.

### <a name="4-read-the-leaderboard-data"></a>4. Leer los datos de marcador

Para poder leer los datos de marcador, deberá llamar a la `stats_manager::do_work()` función que devuelve una lista de `stat_event` valores. Datos de marcador se ubicará en un `stat_event` del tipo `stat_event_type::get_leaderboard_complete`. Cuando llegue a través de un evento de este tipo en la lista de `stat_event`s se puede buscar a través de la `leaderboard_result` contenidos en el `stat_event` para tener acceso a los datos.

Ejemplo `do_work()` controlador

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

Leer los datos de marcador de los resultados del marcador  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

Recuperar más páginas de datos de la tabla de líderes.  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRT C# ejemplo

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1. Obtener una instancia singleton de la StatisticManager

Para poder invocar la `StatisticManager` funciones deberá establecer una variable a su instancia de Singleton.

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2. Crear un LeaderboardQuery

El `LeaderboardQuery` determinará la cantidad, en orden, y el punto de inicio de los datos devueltos por la llamada del marcador.  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

Un `LeaderboardQuery` tiene unos cuantos atributos que se pueden establecer que esto afectará a los datos devueltos:

|Propiedad |Descripción  |
|---------|---------|
|SkipResultToRank     |Esta variable uint determinará qué clasificación de los datos de marcador se iniciará con cuando se devuelven. Las clasificaciones se inician en el rango 1.         |
|SkipResultToMe     |Si establece este valor booleano de True hará que los datos de la tabla de clasificación devueltos al principio de la `XboxLiveUser` usado en el `GetLeaderboard()` llamar.  |
|Orden     |Enumeraciones de tipo `Microsoft.Xbox.Services.Leaderboard.SortOrder` tienen dos valores posibles, en orden ascendente y descendente. Si establece esta variable para la consulta determina el criterio de ordenación de la tabla de clasificación.        |
|MaxItems     |Este uint determina el número máximo de filas que se devolverán por llamada a `GetLeaderboard()` o `GetSocialLeaderboard()`.         |

Que forman su `LeaderboardQuery` puede ser similar al siguiente:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Esta consulta devuelve cinco filas de la tabla de líderes comenzando en el 100 clasifican individuales.

> [!WARNING]
> Establecer SkipResultToRank mayor que el número de jugadores dentro de la tabla de líderes, hará que los datos de marcador que se devolverán con cero filas.

### <a name="3-call-getleaderboard"></a>3. Llamada GetLeaderboard()

Ahora puede llamar a `GetLeaderboard()` con su `XboxLiveUser`, el nombre de la estadística y un `LeaderboardQuery`.

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> El `statName` usado en el `GetLeaderboard()` llamada debe ser el mismo que el nombre de un estado configurado para el título en [centro de partners](https://partner.microsoft.com/dashboard), que distingue mayúsculas de minúsculas.

### <a name="4-read-leaderboard-data"></a>4. Datos de marcador de lectura

Para poder leer los datos de marcador, deberá llamar a la `StatisticManager.DoWork()` función que devuelve una lista de `StatisticEvent` valores. Datos de marcador se ubicará en un `StatisticEvent` del tipo `GetLeaderboardComplete`. Cuando llegue a través de un evento de este tipo en la lista de `StatisticEvent`s se puede buscar a través de la `LeaderboardResult` contenidos en el `StatisticEvent` para tener acceso a los datos.

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

En el código de título `StatisticManager.DoWork()` debe usarse para controlar todos los eventos del Administrador de estadística entrante y no solo para los marcadores. 

> [!NOTE]
> Con el fin de recuperar el `LeaderboardResultEventArgs` , deberá convertir la `StatisticEvent.EventArgs` como un `LeaderboardResultEventArgs` variable.

### <a name="5-retrieve-more-leaderboard-data"></a>5. Recuperar más datos de marcador

Con el fin de recuperar las páginas posteriores de los datos de marcador deberá usar el `LeaderboardResult.HasNext` propiedad y el `LeaderboardResult.GetNextQuery()` función para recuperar el `LeaderboardQuery` que le llevará la siguiente página de datos.

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```
---
title: Secuencia de comandos de un marcador en Unity
description: Guía sobre la creación de su propia tabla de clasificación en Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity, marcadores
ms.openlocfilehash: 6e73ffd9b55f3638eb3cf4245c6f7943fe92dc48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608760"
---
# <a name="script-a-leaderbaord-gameobject"></a>Secuencia de comandos de un GameObject leaderbaord

Para aquellos que deseen una experiencia de marcador personalizado, en este artículo le proporcionará las herramientas para implementar su propia tabla de clasificación para ello, vaya a través de las API disponibles para los desarrolladores de Unity. Una vez que comprenda cómo extraer los datos de marcador podrá aplicarlo a la interfaz de usuario de su elección.

## <a name="call-for-leaderboard-data"></a>Llamada para los datos de marcador

Hay dos llamadas de API para recuperar los datos de marcador.

- `void GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)`
- `void GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)`

Con el fin de realizar correctamente cualquiera de estas llamadas devuelven datos, deberá adquirir un `XboxLiveUser` por [inicio de sesión](unity-prefabs-and-sign-in.md), tiene un [configurado stat](add-stats-and-leaderboards-in-unity.md) con el valor para al menos uno de los jugadores y forma un `LeaderboardQuery`. Si aún no sabe cómo un usuario de inicio de sesión o debe inicializar una estadística para el marcador se pueden leer los artículos vinculados. Una vez que tenga una estadística inicializada la manera más fácil para asociarlo con el script leaderboard consiste en incluir uno de los prefabricados estadística: `IntegerStat`, `DoubleStat`, o `StringStat` como una variable pública. Deben tener su propiedad de identificador configurado en el mínimo como los Estados de esto es lo que se usará para la **statName** cuando llamamos a nuestros datos de marcador de parámetro. Finalmente deberá formulario un `LeaderboardQuery` objeto.
Un `LeaderboardQuery` tiene unos cuantos atributos que se pueden establecer que esto afectará a los datos devueltos:

- **SkipResultToRank**: si se establece, esta variable uint determinará qué clasificación de los datos de marcador se iniciará con cuando se devuelven. Las clasificaciones se inician en el rango 1.
- **SkipResultToMe**: si establece este valor booleano de True hará que los datos de la tabla de clasificación devueltos al principio de la `XboxLiveUser` usado en el `GetLeaderboard()` llamar.
- **Order**: Enumeraciones de tipo `Microsoft.Xbox.Services.Leaderboard.SortOrder` tienen dos valores posibles, en orden ascendente y descendente. Si establece esta variable para la consulta determina el criterio de ordenación de la tabla de clasificación.
- **MaxItems**: Este uint determina el número máximo de filas que se devolverán por llamada a `GetLeaderboard()` o `GetSocialLeaderboard()`.

Que forman su leaderboardQuery puede ser similar al siguiente:

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

Ahora que tenemos todas las piezas juntas podemos llamar a la `GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)` función.

El `GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)` función tiene un parámetro adicional llamado socialGroup. Esta cadena actúa como un filtro de relación en los datos devueltos. Los valores aceptables para socialGroup son como sigue:

- "all": Esto devolverá un marcador que se filtran a amigos del XboxLiveUser
- "en Favoritos": Esto devolverá un marcador que se filtran a amigos favoritos del XboxLiveUser

Puede usar el `LeaderboardTypes` enum en la `Microsoft.Xbox.Services.Client` espacio de nombres para etiquetar su socialGroup marcadores y, a continuación, utilice el `LeaderboardHelper` clase función `GetSocialGroupFromLeaderboardType(LeaderboardTypes leaderboardType)` para extraer la cadena adecuada.

> [!NOTE]
> Pasar una cadena vacía para el parámetro socialGroup devolverá los mismos resultados que una llamada a la `GetLeaderboard()` función. Recibirá una sin filtrar *global* marcador que muestra todos los usuarios con una clasificación de la tabla de clasificación que se reproduce el juego.

```csharp
using Microsoft.Xbox.Services.Leaderboard;
using Microsoft.Xbox.Services.Statistics.Manager;
using Microsoft.Xbox.Services;

public void LoadLeaderboard()
{

    if (this.stat == null)
    {
        // TO DO: Display "Stat not specified" error message!
        return;
    }

    if (this.xboxLiveUser == null)
    {
        if (SignInManager.Instance.GetCurrentNumberOfPlayers() > 0)
        {
            this.xboxLiveUser = SignInManager.Instance.GetPlayer(1);
            this.isLocalUserAdded = true;
        }
        else
        {
            // TO DO: Display "No user signed-in" error message!
            return;
        }
    }

    LeaderboardQuery query = new LeaderboardQuery
    {
        MaxItems = 5,
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, otherQuery);
}

```

Ahora es posible que haya observado que nuestros dos funciones de recuperación de marcador devuelven void y, por tanto, no devuelven los datos de la tabla de clasificación que estamos buscando. En realidad, se recuperará los datos de marcador en una función de eventos descrito en la sección siguiente.

## <a name="receive-the-leaderboard-data"></a>Recibir los datos de marcador

Con el fin de recuperar los datos de marcador, deberá agregar una función de escucha para el `StatsManagerComponent` instancia para el título. Debe agregar la siguiente línea de código para el `Awake()` función del código: `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`. El `StatsManagerComponent` en el `Microsoft.Xbox.Services.Client` espacio de nombres escucha los eventos de finalización de marcador. Al ejecutar esta línea de código, agregará una función a la lista de funciones que se llamará cuando se produce un evento de finalización de la tabla de clasificación. En este ejemplo que se llama función MyGetLeaderBoardCompletedFunction, puede asignar la función como sea necesario en su propio script. La función "MyGetLeaderboardCompletedFunction" tendrá que tomar dos parámetros, un objeto que representa al remitente, y un `Microsoft.Xbox.Services.Client.StatEventArgs` parámetro. El shell de la función puede tener un aspecto similar al siguiente:

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

Lo primero que debe hacer esta función es comprobar si hay errores que se encuentra en la `StatEventArgs` statArgs de parámetro. StatArgs contiene un `StatisticEvent` EventData, que contiene los datos de error. Si se produjo un error al recuperar los datos de marcador puede encontrarlo en `statArgs.EventData.ErrorCode` o `statArgs.EventData.ErrorMessage`. Si se ha producido ningún error, la propiedad ErrorCode será 0 y el mensaje de error será una cadena vacía "". Puede agregar un simple if instrucción en el código anterior para comprobar si hay errores.

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0) //if there is an error
        {
            // TO DO: Display error message
            return;
        }
    }
```

Después de confirmar que no hay ningún error, almacenar los resultados de la solicitud de marcador que se encuentran en `statArgs.EventData.EventArgs.Result`. `Result` es un `LeaderBoardResult` objeto que contiene los datos que necesita para rellenar la tabla de clasificación. En nuestro ejemplo de código se extraerá estos datos y enviarlos a otra función llamada `LoadResult()`.

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0)
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

El `LeaderboardResult` resultado que enviamos a la `LoadResult()` función tendrá todos los datos que necesitamos tanto leer los datos de marcador que se devolvieron como realizar llamadas adicionales para recuperar rangos aún no se han devueltos por la llamada original. Desea almacenar los resultados en una variable de clase para el script leaderboard así:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Esto es importante porque el `LeaderboardResult` contiene un `HasNext` propiedad que determina si hay una sección posterior del marcador que se puede recuperar, el resultado también contiene un recuento total de filas que componen el marcador. Estas propiedades será importantes para navegar por el marcador. Para extraer datos de su `LeaderBoardResult` simplemente se implementa un para el bucle con la `LeaderboardResults` lista de `LeaderboardRow` llamado `Rows`. En nuestro código de ejemplo simplemente vamos para concatenar los valores en cada `LeaderboardRow` en una cadena que se mostrará.


```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

En nuestro ejemplo se usan las propiedades de rango, el nombre de jugador y los valores de LeaderBoardResult para rellenar nuestra cadenas, así como la propiedad DisplayName de la estadística asociada con el marcador.

Estoy seguro, podrá hacer algo más creativo con todos estos datos de marcador.

## <a name="navigating-the-leaderboard-data"></a>Navegar por los datos de marcador

En los casos más comunes que no cargará cada rango único en la tabla de clasificación a la vez y deberá poder navegar por los rangos para mostrar las diferentes secciones de la tabla de clasificación para el usuario. Supongamos que tiene un marcador con 100 jugadores una clasificación. En la llamada inicial a `GetLeaderboard()` o `GetSocialLeaderboard` recuperar 10 `LeaderboardRows` y mostrarlos para el Reproductor. El Reproductor puede ver más de los mejores diez jugadores. Es la manera más fácil de obtener el siguiente conjunto de diez usuarios a determinar si el `LeaderboardResult` guardado desde la última vez consulta tiene más filas para recuperar y, a continuación, que realiza la llamada `GetLeaderboard()` con la siguiente consulta del ese LeaderboardResult. Para usar un LeaderBoardResult *nextQuery* debe usar la función `LeaderBoardResult.GetNextQuery()`. El código para recuperar el siguiente conjunto de rangos tendría un aspecto similar al siguiente.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.GetNextQuery();
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

Hacia atrás en la tabla de clasificación es un poco más difícil porque no hay ninguna función para extraer la X número de rangos desde el marcador anterior. Con el fin de recuperar las clasificaciones anteriores, tendrá que escribir su propia lógica. Un método sería almacenar su `MaxItems` por `LeaderboardQuery` y calcular qué clasificación debe omitir al uso de la `SkipToRank` atributo de su `LeaderboardQuery`. Ese código podría ser algo parecido a esto:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
}
```

El último escenario más común es que un Reproductor puede simplemente desea ver su zona en el marcador. Esto se logra fácilmente mediante una llamada a la `GetLeaderboard()` función con una consulta donde el `SkipResultToMe` está establecido en true.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
    }
```

Si desea profundizar en un ejemplo más detallado de marcador siempre puedan leer la secuencia de comandos Leaderboard.cs en la carpeta XboxLive Plugin en activos >> XboxLive >> Scripts >> Leaderboard.cs.
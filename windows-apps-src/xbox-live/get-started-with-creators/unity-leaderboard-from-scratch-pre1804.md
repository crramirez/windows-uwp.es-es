---
title: Crear un marcador en Unity
description: Guía sobre la creación de su propia tabla de clasificación en Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, unity, marcadores
ms.openlocfilehash: a62cb2b3bd4afebcc7aa9060db60ac167f052977
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657830"
---
# <a name="leaderboards-data-in-unity"></a>Datos de tablas de clasificación en Unity

Para aquellos que deseen una experiencia de marcador personalizado, en este artículo le proporcionará las herramientas para implementar su propia tabla de clasificación para ello, vaya a través de las API disponibles para los desarrolladores de Unity. Una vez que comprenda cómo extraer los datos de marcador podrá aplicarlo a la interfaz de usuario de su elección.

[!IMPORTANT]
> En este artículo se aplica a una versión del complemento antes de una actualización realizada en mayo de 2018 (versión 1804). Si instaló el complemento de Xbox Live después de ese momento o, aún no ha descargado, puede tener una versión más reciente que utiliza llamadas diferentes para recopilar datos de marcador. Además, encontrará que las capturas de pantalla en este complemento no coinciden con los de la versión más reciente. En su lugar, consulte el [marcadores más recientes de unity de artículo temporal](unity-leaderboard-from-scratch.md).


## <a name="call-for-leaderboard-data"></a>Llamada para los datos de marcador

Para solicitar datos de marcador desde el servicio Xbox Live debe llamar a  `void GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`

Para realizar correctamente esta llamada, deberá adquirir un `XboxLiveUser` desde [inicio de sesión](unity-prefabs-and-sign-in.md), tiene un [configurado stat](add-stats-and-leaderboards-in-unity.md) con el valor para al menos uno de los jugadores y formulario un `LeaderboardQuery`. Si aún no sabe cómo un usuario de inicio de sesión o debe inicializar una estadística para el marcador se pueden leer los artículos vinculados. Una vez que tenga un inicializado una estadística es la manera más fácil para asociarlo con el script leaderboard incluir uno de los prefabricados estadística: `IntegerStat`, `DoubleStat`, o `StringStat` como una variable pública. Deben tener su propiedad de identificador configurado en el mínimo como los Estados de esto es lo que se usará para la **statName** cuando llamamos a nuestros datos de marcador de parámetro. Finalmente deberá formulario un `LeaderboardQuery` objeto.
Un `LeaderboardQuery` tiene unos cuantos atributos que se pueden establecer que esto afectará a los datos devueltos:

- **StatName(Required):** no es el identificador de la estadística de la tabla de clasificación recuperará los datos, si no se establece no se establece en un valor de Id. de estadísticas, se devolverá ningún dato.
- **SocialGroup:** dependiendo del valor de esta cadena se filtrarán los datos devueltos de la tabla de clasificación para sus amigos, sus amigos favoritos o si no se establece, recuperará un tablero de puntuaciones global sin filtrar. El valor "" se devuelve una lista sin filtrar, el valor "" devolverá una lista con sólo sus amigos en él, "favorite" devolverá una lista con sus favoritos amigos presentes.
- **SkipResultToRank:** si se establece, esta variable uint determinará qué clasificación de los datos de marcador se iniciará con cuando se devuelven. Las clasificaciones se inician en el rango 1.
- **SkipResultToMe:** si establece este valor booleano de True hará que los datos de la tabla de clasificación devueltos al principio de la `XboxLiveUser` usado en el `GetLeaderboard()` llamar.
- **Orden:** Enumeraciones de tipo `Microsoft.Xbox.Services.Leaderboard.SortOrder` tienen dos valores posibles, en orden ascendente y descendente. Si establece esta variable para la consulta determina el criterio de ordenación de la tabla de clasificación. De forma predeterminada marcadores devuelven datos en orden descendente.
- **MaxItems:** Este uint determina el número máximo de filas que se devolverán por llamada a `GetLeaderboard()`.

Que forman su LeaderboardQuery puede ser similar al siguiente:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            StatName = stat.ID,
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Con esta consulta devolverá cinco filas de la tabla de líderes comenzando en el 100 clasifican individuales para la estadística especificada.

> [!WARNING]
> Establecer SkipResultToRank mayor que el número de jugadores dentro de la tabla de líderes, hará que los datos de marcador que se devolverán con cero filas.

Ahora que tenemos todas las piezas juntas podemos llamar a la `GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)` función, que se usará nuestra firmado en `XboxLiveUser` (probablemente una `XboxLiveUserInfo.User`) obtenido de inicio de sesión, como el usuario y el `LeaderboardQuery` que acabamos de crear como nuestra consulta.

La tabla de clasificación de una llamada a función puede tener el siguiente aspecto:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

public void LoadLeaderboard()
    {
        // check to make sure you have a valid stat
        if (this.stat == null)
        {
            // TO DO: Display "Stat not specified" error message!
            return;
        }

        // check to make sure you have an XboxLiveUser
        if (this.xboxLiveUser == null)
        {
            if (XboxLiveUserManager.Instance.UserForSingleUserMode != null
                && XboxLiveUserManager.Instance.UserForSingleUserMode.User != null)
            {
                // set the XboxLiveUser if one is available
                this.xboxLiveUser = XboxLiveUserManager.Instance.UserForSingleUserMode.User;
            }
            else // if you don't have a user signed in display an error message and exit. 
            {
                // TO DO: Display "User Not logged In" error message
                return;
            }
        }

        LeaderboardQuery query;

        query = new LeaderboardQuery
        {
            StatName = this.stat.ID,
            MaxItems = this.maxItems,

        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
    }
```

Ahora, puede que haya observado que nuestro marcador recuperar función `GetLeaderboard()` devuelve void y, por tanto, no devuelve los datos de la tabla de clasificación que estamos buscando. En realidad, se recuperará los datos de marcador en una función de eventos descrito en la sección siguiente.

## <a name="receive-the-leaderboard-data"></a>Recibir los datos de marcador

Con el fin de recuperar los datos de marcador, deberá agregar una función de escucha para el `StatsManagerComponent` instancia para el título. Debe agregar la siguiente línea de código para el `Awake()` función del código: `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`.

```csharp
private void Awake()
    {
        this.EnsureEventSystem();
        XboxLiveServicesSettings.EnsureXboxLiveServicesSettings();

        StatsManagerComponent.Instance.GetLeaderboardCompleted += this.GetLeaderboardCompleted;
    }
```

El `StatsManagerComponent` escucha los eventos de finalización de marcador. Al ejecutar esta línea de código, agregará una función a la lista de funciones que se llamará cuando se produce un evento de finalización de la tabla de clasificación. En este ejemplo que se llama a la función `MyGetLeaderBoardCompletedFunction()`, puede asignar el nombre de la función como sea necesario en su propio script. La función "MyGetLeaderboardCompletedFunction" tendrá que tomar dos parámetros, un objeto que representa al remitente, y un `Microsoft.Xbox.Services.Client.StatEventArgs` parámetro. El shell de la función puede tener un aspecto similar al siguiente:

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

Lo primero que debe hacer esta función es comprobar si hay errores que se encuentra en la `StatEventArgs` statArgs de parámetro. StatArgs contiene un `StatisticEvent` ha llamado a EventData, que contiene el `System.Exception` llamado ErrorInfo. Si se produjo un error al recuperar los datos de marcador se puede encontrar en statArgs.EventData.ErrorInfo. Puede agregar un simple if instrucción en el código anterior para comprobar si hay errores.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }
    }
```

Después de confirmar que no hay ningún error, almacenar los resultados de la solicitud de marcador que se encuentran en `statArgs.EventData.EventArgs.Result`. `Result` es un `LeaderBoardResult` objeto que contiene los datos que necesita para rellenar la tabla de clasificación. En nuestro ejemplo de código se extraerá estos datos y enviarlos a otra función llamada LoadResult.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

El `LeaderboardResult` resultado que enviamos a la función LoadResult tendrá todos los datos que necesitamos tanto leer los datos de marcador que se devolvieron como realizar llamadas adicionales para recuperar rangos aún no se han devueltos por la llamada original. Desea almacenar los resultados en una variable de clase para el script leaderboard así:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Esto es importante porque el `LeaderboardResult` contiene una propiedad HasNext que determina si hay una sección posterior del marcador que se puede recuperar. El `LeaderboardResult` también almacena una variable para el total de filas que componen el marcador. Estas propiedades será importantes para navegar por el marcador. Para extraer datos de su `LeaderBoardResult` simplemente se implementa un para el bucle con la `LeaderboardResults` lista de `LeaderboardRow` llamado `Rows`. En nuestro código de ejemplo simplemente vamos para concatenar los valores en cada `LeaderboardRow` en una cadena que se mostrará.

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

En nuestro ejemplo se usan las propiedades de rango, el nombre de jugador y los valores de `LeaderBoardResult` para rellenar nuestra cadenas, así como la propiedad DisplayName de la estadística asociada con el marcador.

Estoy seguro, podrá hacer algo más creativo con todos estos datos de marcador.

## <a name="navigating-the-leaderboard-data"></a>Navegar por los datos de marcador

En los casos más comunes que no cargará cada rango único en la tabla de clasificación a la vez y deberá poder navegar por los rangos para mostrar las diferentes secciones de la tabla de clasificación para el usuario. Supongamos que tiene un marcador con cien clasifican los jugadores. En la llamada inicial a `GetLeaderboard()` recupera diez `LeaderboardRows` y mostrarlos para el Reproductor. El Reproductor puede ver más de los mejores diez jugadores. Es la manera más fácil de obtener el siguiente conjunto de diez usuarios a determinar si el `LeaderboardResult` guardado desde la última vez consulta tiene más filas para recuperar y, a continuación, que realiza la llamada `GetLeaderboard()` con ese LeaderboardResult `NextQuery` propiedad. El código siguiente recuperará el siguiente conjunto de datos de marcador.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.NextQuery;
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
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
        StatName = stat.ID,
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
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
            StatName = stat.ID,
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
    }
```

Si desea profundizar en un ejemplo más detallado de marcador siempre puedan leer la secuencia de comandos Leaderboard.cs en la carpeta XboxLive Plugin en activos >> XboxLive >> Scripts >> Leaderboard.cs.
---
title: Agregar estadísticas de Reproductor y marcadores en el proyecto de Unity
description: Obtenga información sobre cómo usar el complemento de Xbox Live Unity para agregar estadísticas de Reproductor y marcadores en el proyecto de Unity.
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.date: 10/19/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, Unity, creadores
ms.localizationpriority: medium
ms.openlocfilehash: 17af9afc8a9048e7222115d2afdc6108d25df1d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658140"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>Agregar estadísticas de Reproductor y marcadores en el proyecto de Unity

> [!IMPORTANT]
> El complemento de Xbox Live Unity no es compatible con logros o juego multijugador en línea y solo se recomienda para [programa de creadores de Xbox Live](../developer-program-overview.md) miembros.

Cuando haya agregado [Xbox Live inicie sesión en](unity-prefabs-and-sign-in.md) a su proyecto Unity, el siguiente paso es agregar estadísticas de Reproductor y marcadores basados en esas estadísticas del Reproductor.

Con el [complemento de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin), puede agregar fácilmente las estadísticas del Reproductor y marcadores en el proyecto de Unity. Al igual que el inicio de sesión en los pasos, puede elegir usar el incluye prefabricados o adjuntar los scripts incluidos en sus propio juego objetos personalizados.

## <a name="prerequisites"></a>Requisitos previos
1. [Configurar Xbox Live en Unity](configure-xbox-live-in-unity.md)
2. [Inicie sesión en Xbox Live en Unity](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>Estadísticas del Reproductor

Un stat Reproductor es cualquier estadística interesante que se desea realizar el seguimiento de los jugadores. Las estadísticas que realizan el seguimiento con Xbox Live deben ser estadísticas que son relevantes para el Reproductor y se muestran de alguna manera. Estas estadísticas del Reproductor se usan normalmente para crear marcadores, que pueden ver los reproductores para determinar cómo clasifica su rendimiento frente a otros jugadores. Todas las estadísticas del Reproductor se consideran "stats destacados", lo que significa que el estado se mostrará en la página GameHub del juego.

Estadísticas de Reproductor deben representar un único valor. El complemento de Xbox Live Unity contiene prefabricados para integer, double y las estadísticas de la cadena. Además, se proporciona una secuencia de comandos para un objeto stat base que se puede extender a otros tipos de datos.

Para obtener más información acerca de las estadísticas del Reproductor, consulte [estadísticas Reproductor](../leaderboards-and-stats-2017/player-stats.md).

> [!NOTE]
> Para poder usar las estadísticas del Reproductor o marcadores con el servicio Xbox Live, debe tener un usuario que ha iniciado correctamente antes de que se pueden acceder a los datos.

## <a name="using-the-player-stat-prefabs"></a>Utilizando el prefabricados stat Reproductor

Hay varias prefabricados proporcionados en el complemento de Xbox Live Unity que puede usar relativas a las estadísticas del Reproductor:

* IntegerStat: Un estado que se puede expresar como un valor entero, como el número total de derribos en un ciclo.
* DoubleStat: Una estadística que se puede expresar como un flotante seleccione valor, como una proporción kill/muerte.
* StringStat: Un estado que se puede expresar como un valor de cadena, normalmente una enumeración, por ejemplo, un rango que se otorgan en una fase, como "Gold", "Silver" o "Bronce".
* StatPanel: Ejemplo de interfaz de usuario que puede usar para mostrar el valor actual de una estadística.

Para agregar un stat Reproductor, simplemente arrastre el recurso prefabricado que coincida con el tipo de datos de la estadística a la escena. En el inspector de Unity para la estadística, puede especificar tres valores:

* El identificador de la estadística. Esto debe coincidir con el identificador configurado en el centro de partners y distingue mayúsculas de minúsculas.
* El nombre para mostrar de la estadística (este nombre se muestra en la interfaz de usuario StatPanel prefabricado).
* El valor inicial de la estadística cuando se inicia la escena.

Puede usar el **StatPanel** prefabricado para mostrar el valor de una estadística. Para ello, arrastre un **StatPanel** prefabricado a la escena. Puede especificar la estadística para mostrar arrastrando el gameobject stat a la **Stat** campo de la **StatPanel** objeto en el Inspector de Unity.

### <a name="manipulating-the-player-stat-values"></a>Manipular los valores de stat Reproductor

Los objetos stat jugador tienen un número de funciones que se pueden llamar para ajustar el valor de la estadística. Estas funciones se pueden llamar desde otras rutinas o enlazadas a elementos de interfaz de usuario. Puede mirar la **DoubleStat**, **IntegerStat**, y **StringStat** secuencias de comandos para ver ejemplos de funciones para cambiar el valor de la estadística. Puede modificar o crear nuevos scripts para representar las estadísticas con funciones y la lógica más compleja. Nuevas clases stat deben extender el `StatBase` (clase), definido en el `StatBase` secuencia de comandos.

Por ejemplo, como una prueba sencilla, puede agregar un botón de la interfaz de usuario a la escena y, en el `OnClick` eventos del botón, en el inspector de Unity, agregue un **IntegerStat** de objetos y llamar a la `Increment()` función para aumentar el valor de la estadística en uno cada vez que haga clic en el botón.

Si tiene la stat también está enlazado a un **StatPanel** objeto, puede ver el valor stat actualizar cada vez que haga clic en el botón.

Cada vez que actualice las estadísticas (incremento, decremento, etc.), los valores se actualizan localmente. Para que estas actualizaciones stat reflejados en Xbox Live deben suceder dos cosas. En primer lugar, deberá establecer el valor de estadísticas con una de las funciones StatisticManager.SetStatistic. Hay tres `StatisticManager` funciones para establecer las estadísticas, `StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`, `StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)`, y `StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`. Cada una de estas funciones se usa para establecer el valor adecuado para el tipo de datos de su estado. Es la segunda cosa que debe hacer para actualizar las estadísticas en el servidor, *vaciar* los datos locales. Obtiene vaciar los datos automáticamente cada 5 minutos por la `StatManagerComponent` secuencia de comandos.  Si su juego finaliza antes de los 5 minutos, deberá asegurarse de que al vaciar los datos manualmente en primer lugar para asegurarse de que no pierda ese curso. Para ello, deberá llamar a la `statManagerComponent.RequestFlushToService()` método, asegurándose de llamarlo para la **XboxLiveUser** se está escribiendo la estadística para.

> [!TIP]
> Se considera una práctica recomendada para vaciar siempre los datos antes de que finalice su juego para asegurarse de que no se pierda el progreso.

### <a name="checking-and-verifying-stats"></a>Comprobación y estadísticas

El `StatisticManager` clase tiene dos funciones que son útiles para la comprobación de las estadísticas configuradas para un `XboxLiveUser`, `StatisticManager.GetStatisticNames(XboxLiveUser user)` y `StatisticManager.GetStatistic(XboxLiveUser user, String statName)`. `GetStatisticNames()` proporcionará un `list<string>` rellena por los nombres de las estadísticas para el XboxLiveUser proporcionado. Esos nombres se pueden utilizar para llamar para el valor actual de una estadística mediante una llamada a la `GetStatistic()` función. Es importante tener en cuenta que mientras puede leer las estadísticas del servicio Xbox Live estadísticas no se recomienda usarlo para la lógica del juego, sino a comprobar el estado de la estadística después de que se ha insertado. El servicio solo está pensado para ayudar a ejecutar otros servicios como marcadores y no pretende ser una fuente de verdad para las estadísticas en el juego. Es importante que el título de controlar toda la lógica de estadísticas, como se realiza ninguna comprobación en la estadística por el servicio de Xbox y simplemente acepta cualquier valor dado a él como el estado actual.

## <a name="leaderboards"></a>Marcadores

Un marcador representa una lista numerada ordenada de los jugadores que alcanzaron el valor de un estado "mejor". Por ejemplo, un marcador podría enumerar las personas que han logrado el tiempo más rápido en una vuelta de carrera, para que los jugadores pueden comparar su mejor momento carrera contra los mejores tiempos de carrera logrado al resto de los jugadores.

Tablas de clasificación se basan en las estadísticas del Reproductor que se envían al servicio de Xbox Live, el juego. Por lo tanto, los datos de marcador es de solo lectura, como no puede modificar directamente.

El complemento de Xbox Live Unity proporciona un prefabricado de marcador de ejemplo que puede usar para aprender a implementar marcadores en el juego.

Para obtener más información sobre los marcadores, vea [marcadores](../leaderboards-and-stats-2017/leaderboards.md).

## <a name="using-the-leaderboard-prefabs"></a>Utilizando el marcador de prefabricados

El complemento de Xbox Live Unity contiene dos prefabricados para tablas de clasificación:

* Tabla de clasificación: Un objeto que representa un marcador y contiene la interfaz de usuario simple para mostrar los valores de la tabla de líderes.
* LeaderboardEntry: Un objeto que representa una sola fila de un marcador.

Puede arrastrar un **marcador** prefabricado a la escena. En el Inspector de Unity, puede establecer los siguientes atributos:

* Stat: El gameobject stat que está asociado este marcador.
* Tipo de marcador: El ámbito de los resultados que se deben devolver para las entradas de marcador.
* Número de entradas: El número de filas de datos que se muestran por página.

> [!NOTE]
> La parte de la prefabricado de marcador Stat está inicialmente en blanco. Intente arrastrar uno de los prefabricados stat mencionados anteriormente en la ranura gameobject para las pruebas.

En el editor de Unity, la **marcador** prefabricado siempre mostrará los mismos datos ficticios independientemente de la configuración de inspector. Debe crear y exportar el proyecto en Visual Studio e inicie sesión con un usuario autorizado para ver los valores de datos reales. Para obtener más información, consulte [configurar Xbox Live en Unity](configure-xbox-live-in-unity.md).

## <a name="see-also"></a>Consulte también

* [Inicio de sesión en Xbox Live en Unity](unity-prefabs-and-sign-in.md)
* [Configurar Xbox Live en Unity](configure-xbox-live-in-unity.md)
* [La escena de ejemplo de marcador](setup-leaderboard-example-scene.md)
* [Obtener datos de marcador](unity-leaderboard-from-scratch.md)

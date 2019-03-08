---
title: Actualizar estadísticas de 2017
description: Obtenga información sobre cómo actualizar las estadísticas del Reproductor de Xbox Live mediante el uso de estadísticas de 2017.
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, estadísticas del Reproductor, estadísticas de 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631830"
---
# <a name="updating-stats-2017"></a>Actualizar estadísticas de 2017

Actualizar estadísticas mediante el envío el valor más reciente para el servicio de Xbox Live mediante el `StatsManager` API que se analizará a continuación.

Depende de su título a realizar un seguimiento de estadísticas del Reproductor y se llama `StatsManager` actualizarla según corresponda.  `StatsManager` creará los cambios del búfer y vaciar éstas al servicio periódicamente.  El título puede vaciar también manualmente.

> [!NOTE]
> No se vacíe estadísticas con demasiada frecuencia.  En caso contrario, el título será limitada.  Es una práctica recomendada vaciar como máximo una vez cada 5 minutos.

### <a name="multiple-devices"></a>Varios dispositivos

Es posible que un reproductor reproducir el título en varios dispositivos.  En este caso, deberá realizar ciertos esfuerzos para mantener sincronizadas las cosas.

Por ejemplo, si un jugador tiene 15 headshots en su Xbox en casa.  Más adelante, tiene 10 headshots más en su Xbox en casa de un amigo.  Debe enviar el valor de 25 stat en el segundo dispositivo.  Pero no tendría ninguna manera de saber esto sin sincronizar esta información de algún modo.

Hay varias maneras de hacer esto:

1. Store ellos mediante [almacenamiento conectado](../storage-platform/connected-storage/connected-storage-technical-overview.md).  Normalmente usaría almacenamiento conectado para cada usuario guardar los datos.  Estos datos se mantienen sincronizados entre diferentes dispositivos para un usuario determinado.
2. Use su propio servicio web para mantener sincronizados los estadísticas si ya tiene una para realizar tareas de auxiliar para el título.

### <a name="offline"></a>Desconectado

Como se mencionó anteriormente, el título es responsable de realizar un seguimiento de estadísticas del Reproductor y, por lo tanto, responsbile para admitir escenarios sin conexión. 

### <a name="examples"></a>Ejemplos

Se pasará a través de un ejemplo para unir estos conceptos.

Un estado común en un juego de carreras es el momento de vuelta.  Normalmente, menor es mejor para estas estadísticas.  Por lo que crearía un stat y el marcador asociado, donde menor es mejor.  En otras palabras, este marcador se ordenan en orden ascendente.

El título de la podría realizar un seguimiento de tiempos de vuelta de un usuario durante su sesión de reproducción.  Debe actualizar el Administrador de estadísticas sólo si tuvieran un tiempo de vuelta inferior a su mejor anterior.

Puede realizar un seguimiento de forma óptima anterior en una de las maneras siguientes:
1. Desde la operación de guardar archivos mediante el almacenamiento conectado.
2. Su propio servicio web.

El servicio reemplazará el valor stat independientemente.  Por lo que incluso si tuviera que actualizar con un tiempo de vuelta es mayor que su mejor anterior, se sobrescribirán maravillosas anterior.

Por lo tanto, asegúrese en su título, que solo envía los valores stat adecuados en función del escenario de juego.  En algunos casos podría ser mejores los valores más bajos, en algunos casos superior podría ser mejor, o alguna otra cosa completamente.

## <a name="programming-guide"></a>Guía de programación

Normalmente es el flujo para el uso de estadísticas:

1. Inicializar el `StatsManager` API pasando un usuario local.
1. Mientras se reproduce un usuario a través de su título, actualizar valores stat mediante el `set_stat` funciones.
1. Estas actualizaciones stat se vacían periódicamente y se escriben en Xbox Live.  También se puede hacer manualmente.

### <a name="initialization"></a>Inicialización

Se llama a la `StatsManager` con un usuario local para inicializar la API con la información necesaria.

Consulte a continuación para obtener un ejemplo

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>Estadísticas de escritura

Escribir estadísticas mediante el `stats_manager::set_stat` familia de funciones.  Hay tres variantes de esta función para cada tipo de datos:

* `set_stat_number` para los elementos flotantes.
* `set_stat_integer` para números enteros.
* `set_stat_string` para las cadenas.

Cuando se llama a estos, las actualizaciones de estadísticas se almacenan en caché localmente en el dispositivo.  Periódicamente, estos se vacía a Xbox Live.

Tiene la opción de vaciar manualmente estadísticas mediante el `stats_manager::request_flush_to_service` API.  Tenga en cuenta que si se llama a esta función con demasiada frecuencia, será velocidad limitada.  Esto no significa que nunca se actualizará la estadística.  Simplemente significa que la actualización se realizará cuando expira el tiempo de espera.

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>Ejemplo

Supongamos que tiene una acción en primera persona.  Durante una coincidencia que se acumulen las estadísticas siguientes:

| Nombre STAT | Formato |
|-----------|--------|
| Mejor derribos por Round | Enteros |
| Duración derribos | Enteros |
| Muertes de duración | Enteros |
| Proporción de Kill/muerte de duración | Número |

Como el Reproductor entra a través de la coincidencia, incrementaría el *termina por Round*, *termina el período de duración* y *las muertes de duración* localmente.

Al final de la coincidencia haría lo siguiente:
1. Compare el derribos que obtuvo en la Ronda, con sus mejores anteriores.  Si es mayor, a continuación, actualice `StatsManager`.
2. Actualizar sus termina la duración y las muertes con los valores nuevos y actualizar `StatsManager`.
3. Calcular derribos/muertes y actualizar `StatsManager`

Tenga en cuenta que para 1 y 2, deberá conocer sus valores stat anteriores.  Consulte las secciones anteriores para los procedimientos recomendados sobre cómo recuperar estos.

Cualquiera de estas estadísticas podría corresponder a un marcador, que se tratará en el siguiente artículo.

### <a name="flushing-stats"></a>Estadísticas de vaciado

Puede vaciar manualmente estadísticas mediante `stats_manager::request_flush_to_service`.  Es posible que desee hacer esto si desea mostrar un marcador.

Por ejemplo, si tuviera un marcador para `Lifetime Kills` en el ejemplo anterior, querría asegurarse de que las actualizaciones de estadísticas correspondiente a este estado tenían se han vaciado en el servidor antes de mostrar el marcador.  De este modo, los marcadores refleja el progreso más reciente del Reproductor.

### <a name="cleanup"></a>Limpieza
Cuando se cierra el título, quite el usuario Administrador de estadísticas. Esto hará que vacíe los valores de estadísticas más recientes para el servicio.

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```

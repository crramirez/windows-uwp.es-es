---
title: Uso de contactos SmartMatch
description: Aprenda a usar Xbox Live SmartMatch para hacer coincidir los jugadores en un juego multijugador.
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, contactos de varios jugadores, one, xbox, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592100"
---
# <a name="using-smartmatch-matchmaking"></a>Uso de contactos SmartMatch

El siguiente diagrama de flujo ilustra el proceso de contactos de SmartMatch.

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Creación de una sesión de vale de coincidencia y un vale de coincidencia

Antes de comenzar los contactos, un scout"contactos" establece una sesión de vale de coincidencia para representar el grupo de personas que le gustaría escribir contactos juntos. Todos los usuarios de este grupo de unirse a la sesión mediante el **MultiplayerSession.Join método (String, Boolean, Boolean)**.

Una vez creada y rellenada con los reproductores de la sesión de vale, el título envía la sesión en el servicio de contactos mediante la **MatchmakingService.CreateMatchTicketAsync método**. Este método crea un vale de coincidencia que representa la sesión de vale y actualiza el campo /servers/matchmaking/properties/system/status en la sesión de vale para "Buscar". Para obtener más información, vea [Cómo: Cree una incidencia de coincidencia](multiplayer-how-tos.md).

La respuesta desde el método de creación del vale de coincidencia es un **CreateMatchTicketResponse clase** objeto. La respuesta contiene el Id. de vale de coincidencia, puede utilizarse un GUID que se puede usar para cancelar los contactos mediante la eliminación de la incidencia. La respuesta también contiene un tiempo promedio de espera para la hopper, que puede usarse para establecer las expectativas del usuario.


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Establecer atributos de contactos en la sesión y los jugadores

Al enviar una sesión de contactos, el título puede establecer los atributos que el servicio se usa para agrupar la sesión con otras sesiones. Atributos se pueden especificar en el nivel de vale o en el nivel de cada miembro.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Establecer atributos de contactos en el nivel de vale de coincidencia

El título envía atributos en el nivel de vale de coincidencia en el *ticketAttributesJson* parámetro de la **MatchmakingService.CreateMatchTicketAsync** método. Los atributos especificados en el nivel de vale invalidan los mismos atributos especificados en el nivel de cada miembro.


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Establecer atributos de contactos en el nivel por miembro

El título especifica los atributos y los miembros en cada miembro dentro de la sesión de vale de coincidencia. Se establecen mediante una llamada a la **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson método**, utilizando un nombre de propiedad de "matchAttrs". Esta llamada pone los atributos en /members/ {index} / propiedades/personalizado/matchAttrs campo en cada jugador dentro de la sesión de vale.

El proceso de contactos "aplana" por cada miembro de cada en un solo atributo de nivel de vale, según el método flatten especificado para ese atributo en la configuración de Xbox Live para el hopper. Esto se puede configurar en [XDP](https://xdp.xboxlive.com) o [centro de partners](https://partner.microsoft.com/dashboard).


## <a name="making-the-match"></a>Realizar la coincidencia

Con la sesión de vale y la coincidencia de vale configurado, los contactos servicio coincide con la sesión de vale representado con otras sesiones de vale que representa otros grupos y crea o identifica una sesión de destino de coincidencia. El servicio también crea reservas de direcciones para los jugadores coincidentes en la sesión de destino y, a continuación, marca las sesiones de vale según las coincidencias. MPSD notifica el título de este cambio a la sesión de vale.

Ahora el título debe, a continuación, tomar medidas para inicializar la sesión de destino con el fin de confirmar que han aparecido suficiente reproductores y realizar la calidad de servicio (QoS) comprobaciones para asegurarse de que puedan conectarse entre sí correctamente. Si se produce un error en la inicialización o la calidad de servicio, el título de la marca la sesión de vale para volver a enviar a los contactos para que se puede encontrar otro grupo. Para obtener más información sobre los procesos, consulte [inicialización de la sesión de destino y QoS](smartmatch-matchmaking.md).

Durante la actividad de coincidencia, los siguientes cambios se realizan en los objetos JSON para la sesión:

-   campo /Servers/matchmaking/Properties/System/Status establecido en "encontrado"
-   campo /Servers/matchmaking/Properties/System/targetSessionRef establecido en la sesión de destino
-   /Members/ {index} / propiedades/personalizado/campo matchAttrs para cada sesión de vale se copia en el /members/ {index} / constantes/personalizado/matchmakingResult/playerAttrs campo
-   Para cada jugador, vale atributos que se copian desde el campo ticketAttributes en el vale de coincidencia en /members/ {index} / constantes/personalizado/matchmakingResult/ticketAttrs campo


## <a name="maintaining-the-match-ticket"></a>Mantener el vale de coincidencia

El servicio utiliza una instantánea de la sesión de vale en el momento cuando se crea el vale de coincidencia para la sesión. Por lo tanto, si cualquier reproductores unirse o abandonar la sesión de vale, el título debe usar el servicio para eliminar y volver a crear el vale de coincidencia.


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>Reutilización de la sesión de juego como una sesión de vale de coincidencia

| Importante                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Es importante tener en cuenta que las dos sesiones con *preserveSession* establecido en siempre no será capaz de hacer coincidir entre sí, ya que no puede combinarse. El flujo de contactos que usan el título debe tener este hecho en consideración. |

Un título puede reutilizar una sesión de juegos existente como una sesión de vale de coincidencia para buscar más jugadores se unan a un juego que ya está en curso. Para habilitar esta opción, el título debe crear el vale de coincidencia mediante una llamada a **CreateMatchTicketAsync** con el *preserveSession* parámetro establecido en **PreserveSessionMode.Always** . El servicio, a continuación, se garantiza que la sesión existente que se usa para el vale se conserva en todo el proceso de contactos y se convierte en la sesión de destino resultante.


## <a name="deleting-the-match-ticket"></a>Eliminando el vale de coincidencia

Para eliminar el vale de coincidencia, las llamadas de título **MatchmakingService.DeleteMatchTicketAsync método**. Eliminación del vale:

1.  Detiene los contactos para los usuarios en la sesión de vale.
2.  Las actualizaciones en la sesión de vale para el campo de /servers/matchmaking/properties/system/status "Cancelado".


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>Realización de contactos para juegos con el proceso de Xbox Live

Estos son los pasos de alto nivel que tienen lugar para obtener un matchmade de usuario en un juego basado en Xbox Live Compute. Debe aplicar un flujo similar para los juegos hospedados por terceros.
1.  El scout crea una sesión de vale para representar el grupo. Esta sesión contiene una lista de centros de datos potencial, ubicado en la configuración de sesión en /constants/system/measurementServerAddresses. Se trata de la plantilla de sesión si la lista del centro de datos es estática, o desde el cliente escribir durante la creación de la sesión después de obtenerlo de Xbox Live Compute primero. Esta sesión también contiene gsiSetId gameVariantId y maxAllowedPlayers valores del objeto personalizado/targetSessionConstants/gameServerPlatform.
2.  Todos los demás clientes en el grupo de unirse a la sesión de vale.
3.  Todos los miembros del grupo de descarga de los valores de measurementServerAddresses desde el objeto /constants/system para la sesión de vale, ping en ellos mediante la API de plataforma y cargar una lista ordenada de los centros de datos preferido para la sesión, tal como se define en /members/ {index} /Properties/System/serverMeasurements.

| Nota                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Puede establecer y recuperar los valores de measurementServerAddresses desde la sesión con el título del **MultiplayerSession.SetMeasurementServerAddresses** método y el  **Propiedad MultiplayerSessionConstants.MeasurementServerAddressesJson**. |

4.  Las llamadas de scout **CreateMatchTicketAsync**, pasando una referencia a la sesión de vale.

| Nota                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si los objetos de sesión de vale tienen constantes no coincidentes, el método de vale create podría no realizarse correctamente. Esto puede evitarse mediante la adición de un requisito INDISPENSABLE regla hopper, para evitar que coincide con los jugadores que coinciden las constantes. |

Si el **MatchTicketDetailsResponse.PreserveSession propiedad** se establece en nunca, el servicio copia las mediciones de servidor de cada miembro en la representación interna del vale. Aplana la las medidas de servidor de los miembros de la incidencia en una colección de medidas de servidor único para el vale, almacenado en la representación interna del vale de como un atributo del vale "especiales".

Si **MatchTicketDetailsResponse.PreserveSession** está establecido en siempre, el servidor no se usan medidas. En su lugar, el servicio copia el valor de /properties/system/matchmaking/serverConnectionString para la sesión en la representación interna del vale, como una colección serverMeasurements de tamaño 1 con una latencia cero.

5.  El servicio coincide con la sesión de vale con otras personas que representan otros grupos, tomar al servidor de colecciones de medición en la cuenta. Intenta hacer coincidir el grupo con otros grupos que tienen los mismos centros de datos altamente preferido.
6.  Una vez que se ha encontrado un grupo coincidente, el servicio crea o identifica una sesión de destino y agrega todos los participantes de las sesiones de vale que coinciden entre sí. El servicio escribe las mediciones de servidor sin formato final para el grupo coincidente en /properties/system/serverConnectionStringCandidates. Escribe las mediciones de servidor envió inicialmente para todos los miembros recién agregados en la sesión de destino en /members/ {index} / constantes/sistema/matchmakingResult/serverMeasurements.
7.  Todos los clientes realizan la inicialización en la sesión de destino, según se explicó anteriormente. Sin embargo, dado que los clientes van a conectar a Xbox Live Compute, no realizan QoS entre sí para confirmar la conectividad.
8.  Algunas o todas las llamadas de los clientes la **GameServerPlatformService.AllocateClusterAsync método**. La primera de ellas desencadena la asignación, mientras que los demás sólo una confirmación de recepción. El método obtiene la sesión de destino de MPSD y elige un centro de datos según las preferencias de centro de datos en la sesión de destino, así como la carga y otra información específica de proceso de Xbox Live.
9.  Reproducen todos los clientes.


## <a name="see-also"></a>Consulte también

[Operaciones en tiempo de ejecución SmartMatch](smartmatch-matchmaking.md)

[SmartMatch contactos](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking Namespace**

**Microsoft.Xbox.Services.Multiplayer Namespace**

---
title: Asociación de SmartMatch
description: Obtenga información sobre el servicio para los agentes de búsqueda de coincidencias de un juego multijugador SmartMatch de Xbox Live.
ms.assetid: ba0c1ecb-e928-4e86-9162-8cb456b697ff
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, contactos de varios jugadores, one, xbox, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 6bc5f15e6fdb7d2f393daef8d21c134579610a9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647300"
---
# <a name="smartmatch-matchmaking"></a>Asociación de SmartMatch

En este artículo contiene las siguientes secciones
* Introducción a SmartMatch
* Operaciones en tiempo de ejecución SmartMatch
* Configurar SmartMatch del título
* Definir reglas de equipo durante la configuración de SmartMatch
* Inicialización de la sesión de destino y la calidad de servicio

## <a name="introduction-to-smartmatch"></a>Introducción a SmartMatch

El XSAPI proporciona un servicio de contactos, denominado SmartMatch, que es ajustado por el [API del Administrador de varios jugadores](../multiplayer-manager/multiplayer-manager-api-overview.md).  Para uso avanzado de API, puede hacer referencia a la **MatchmakingService clase**, pero si observa que tiene un escenario de contactos que no se puede implementar mediante el Administrador de varios jugadores, proporcione comentarios para ayudarnos a través de su madre.  Independientemente de qué API que se utiliza, la información conceptual en este artículo se aplica.

Contactos SmartMatch agrupa reproductores según la información de usuario y la solicitud de contactos para los usuarios que deseen jueguen juntos. Contactos está basada en servidor, lo que significa que los usuarios proporcionen una solicitud al servicio y, más adelante se les notifica cuando se encuentra una coincidencia. Al compilar un título para Xbox One, puede usar SmartMatch como se describe en [utilizando SmartMatch contactos](using-smartmatch-matchmaking.md). como alternativa, puede su propio servicio de emparejamiento, como se describe en [mediante su propio servicio de emparejamiento](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/multiplayer-and-networking/using-your-own-matchmaking-service). Tenga en cuenta que el acceso a este vínculo requiere que haya un [centro de partners](https://partner.microsoft.com/dashboard) cuenta que está habilitada para el desarrollo de Xbox Live.

### <a name="about-smartmatch"></a>Acerca de SmartMatch

El servicio trabaja en estrecha colaboración con MPSD para simplificar los contactos. Permite que los títulos realizar fácilmente los contactos en segundo plano, por ejemplo mientras se reproduce solo jugador dentro del título.

Individuos o grupos que desean escribir contactos creación una sesión de vale de coincidencia, a continuación, solicitan el servicio para encontrar otros jugadores con la que se va a configurar una coincidencia. Esto da como resultado la creación de un archivo temporal "vale de coincidencia" que residen en el servicio durante un período de tiempo.

El servicio elige sesiones para reproducir en función de la configuración, las estadísticas almacenadas para cada jugador y cualquier información adicional proporcionado en el momento de la solicitud de coincidencia. El servicio, a continuación, crea una sesión de destino de coincidencia que contiene todos los jugadores que han coincidido y notifica a los títulos de los usuarios de la coincidencia.

Cuando la sesión de destino esté lista, títulos realizan comprobaciones de calidad de servicio para confirmar que el grupo puede reproducir juntos y, a continuación, empezar a reproducir si todo es correcto. Durante la calidad de servicio en el proceso y matchmade juego, títulos de mantener el estado de sesión al día en MPSD y recibir notificaciones de MPSD acerca de los cambios a la sesión. Estos cambios incluyen los usuarios ir y venir y los cambios en el árbitro de sesión.


### <a name="match-ticket-session"></a>Sesión de vale de coincidencia

Una sesión de vale coincidencia representa a los clientes de los jugadores que desean realizar una coincidencia. Se crea basándose en un juego o en un grupo de extraños que están en una sala de juntas, o en otras agrupaciones específicas de título de los jugadores. En algunos casos, la sesión de vale podría ser una sesión ya está en curso que está buscando más jugadores.


### <a name="match-ticket"></a>Vale de coincidencia

Envío de una sesión de vale para contactos da como resultado la creación de un vale de coincidencia que realiza un seguimiento el intento de contactos. Los atributos en el vale, por ejemplo, juegos mapa o un nivel de Reproductor, junto con los atributos de los jugadores en la sesión de vale, se usan para determinar a la coincidencia.
#### <a name="hoppers"></a>Hoppers

Hoppers son lugares lógicos donde se recopilan los vales de coincidencia. Solo los vales en el mismo hopper pueden coincidir. Un título puede tener varios hoppers. Por ejemplo, un título puede crear uno hopper qué Reproductor de habilidad es el elemento más importante para la coincidencia. Puede usar otro hopper en el que solo se confrontan reproductores si se ha comprado el mismo contenido descargable.


#### <a name="hopper-rules"></a>Reglas de Hopper

Las reglas de Hopper proporcionan definiciones de los criterios que el servicio utiliza para decidir los reproductores para agrupar. Hay dos tipos de reglas:   DEBE reglas--tiene que se debe cumplir para vales de coincidencia se consideren compatibles.
-   DEBE reglas: una incidencia de coincidencia de una regla de coincidencia se prefiere que no lo hace.

Dentro de cada una de estas categorías, hay varios tipos específicos de reglas. Para obtener más información, consulte la información de configuración de la operación de tiempo de ejecución en **las operaciones en tiempo de ejecución de SmartMatch**.


### <a name="match-target-session"></a>Sesión de destino de coincidencia

Una vez que se ha encontrado un grupo coincidente, el servicio crea una sesión de destino de coincidencia y lugares reserva para todos los participantes de las sesiones de vale que coinciden entre sí.


## <a name="smartmatch-runtime-operations"></a>Operaciones en tiempo de ejecución SmartMatch



| Nota                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Para obtener información sobre cómo usar SmartMatch contactos en su título, consulte [utilizando SmartMatch contactos](using-smartmatch-matchmaking.md). |


### <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Creación de una sesión de vale de coincidencia y un vale de coincidencia

Un cliente que se designa como scout"contactos" establece una sesión de vale de coincidencia para representar un grupo de los jugadores que desea escribir contactos. Todos los jugadores en este grupo de unirse a la sesión mediante el método MultiplayerSession.Join. Una vez que la sesión se rellena con los reproductores, el título envía la sesión para el servicio mediante el método MatchmakingService.CreateMatchTicketAsync, que crea un vale de coincidencia que representa la sesión.

| Nota                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| El servicio de SmartMatch actualiza el campo /servers/matchmaking/properties/system/status en la sesión de vale para "Buscar" durante la operación CreateMatchTicketAsync. |

La respuesta desde el método de creación del vale de coincidencia es un objeto CreateMatchTicketResponse. Esta respuesta contiene el identificador de la incidencia de coincidencia, un GUID que puede usarse para cancelar los contactos mediante la eliminación de la incidencia. Además, un tiempo promedio de espera para la hopper se incluye en la respuesta a usar al establecer las expectativas del Reproductor.

| Nota                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A menos que haya datos stat juegos específicos que debe conservarse, se recomienda que los títulos de usar el valor de PreserveSessionMode.Never al llamar a CreateMatchTicketAsync. Con PreserveSessionMode.Never permite contactos más rápido y elimina la necesidad de tener las opciones de interfaz de usuario para el hospedaje de un juego en lugar de a la búsqueda de un juego. |


### <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Establecer atributos de contactos en la sesión y los jugadores

Al enviar la sesión de vale, el título establece sesión y el Reproductor de los atributos que el servicio se usa para agrupar la sesión con otras sesiones. Atributos se pueden especificar en el nivel de vale o en el nivel de cada miembro.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Establecer atributos de contactos en el nivel de vale de coincidencia

El título envía atributos en el nivel de vale de coincidencia en el parámetro ticketAttributesJson del método MatchmakingService.CreateMatchTicketAsync. Los atributos especificados en el nivel de vale invalidan los mismos atributos especificados en el nivel de cada miembro.


#### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Establecer atributos de contactos en el nivel por miembro

El título especifica los atributos y los miembros en cada miembro dentro de la sesión de vale de coincidencia. Se establecen llamando al método MultiplayerSession.SetCurrentUserMemberCustomPropertyJson, utilizando un nombre de propiedad de "matchAttrs". Esta llamada pone los atributos en /members/ {index} / propiedades/personalizado/matchAttrs campo en cada jugador dentro de la sesión de vale.

El proceso de contactos "aplana" por cada miembro en un solo atributo de nivel de vale, según el método flatten especificado para ese atributo en la configuración de Xbox Live la interfaz de usuario para el hopper. Esto se puede configurar en [XDP](https://xdp.xboxlive.com) o [centro de partners](https://partner.microsoft.com/dashboard).


### <a name="making-the-match"></a>Realizar la coincidencia

Con la sesión de vale y la coincidencia de vale configurado, los contactos servicio coincide con la sesión de vale representado con otras sesiones de vale que representa otros grupos y crea o identifica una sesión de destino de coincidencia. El servicio también crea reservas de direcciones para los jugadores coincidentes en la sesión de destino y, a continuación, marca las sesiones de vale según las coincidencias. MPSD, a continuación, notifica el título de este cambio a la sesión de vale.

Ahora el título debe inicializar la sesión de destino con el fin de confirmar que han aparecido suficiente reproductores y realizar la calidad de servicio (QoS) comprobaciones para asegurarse de que puedan conectarse entre sí correctamente. Si se produce un error en la inicialización o la calidad de servicio, el título de la marca de la sesión de vale para enviarse a los contactos para que se puede encontrar otro grupo. Para obtener más información sobre este proceso, consulte **inicialización de la sesión de destino y QoS**.

Durante la actividad de coincidencia, los siguientes cambios se realizan en los objetos JSON para la sesión:

-   /Servers/matchmaking/Properties/System/Status campo establecido en "encontrado".
-   /Servers/matchmaking/Properties/System/targetSessionRef campo se establece en la sesión de destino.
-   /Members/ {index} / propiedades/personalizado/campo matchAttrs para cada sesión de vale se copia en los miembros / {index} / constantes/personalizado/matchmakingResult/playerAttrs campo.
-   Para cada jugador, vale atributos que se copian desde el campo ticketAttributes en el vale de coincidencia en /members/ {index} / constantes/personalizado/matchmakingResult/ticketAttrs campo.


### <a name="maintaining-the-match-ticket"></a>Mantener el vale de coincidencia

El servicio utiliza una instantánea de la sesión de vale en el momento cuando se crea el vale de coincidencia para la sesión. Por lo tanto, si cualquier reproductores unirse o abandonar la sesión de vale, el título debe usar el servicio para eliminar y volver a crear el vale de coincidencia.


### <a name="filling-spots-in-an-in-progress-game-session"></a>Rellenar las zonas en una sesión de juego en curso

Un título puede reutilizar una sesión de juegos existente como una sesión de vale de coincidencia para buscar más jugadores para unirse a un juego que ya está en curso, o una sesión una vez completada una ronda de relleno y algunos reproductores izquierdas. Este proceso se conoce como "reposición".

Una manera de realizar es crear un vale de coincidencia que "conservará" la sesión en curso, mediante una llamada a reposición **MatchmakingService.CreateMatchTicketAsync método** con el parámetro preserveSession establecido en PreserveSessionMode.Always. El servicio se convierte en la sesión de destino resultante y, a continuación, asegura de que la sesión existente que se conserva durante todo el proceso de contactos.

Existen desventajas potenciales de este patrón, como no será capaces de hacer coincidir entre sí, ya que no puede combinarse dos sesiones con preserveSession establecida en Always. Esto puede dar lugar a contactos de reposición muy lento. Para evitar esta situación, un título solo debe usar PreserveSessionMode.Always si requiere la conservación del estado del juego. Otherwis establecer PreserveSessionMode.Never y migrar los jugadores a la nueva targetSession cuando se encuentra una coincidencia.


### <a name="deleting-the-match-ticket"></a>Eliminando el vale de coincidencia

Para eliminar el vale de coincidencia del título llama **MatchmakingService.DeleteMatchTicketAsync método**. En la eliminación del vale del servicio de emparejamiento:

1.  Detiene los contactos para los usuarios en la sesión de vale.
2.  Las actualizaciones en la sesión de vale para el campo de /servers/matchmaking/properties/system/status "Cancelado".


### <a name="matchmaking-for-games-using-xbox-live-compute"></a>Contactos de juegos con el proceso de Xbox Live

El ejemplo siguiente muestra los contactos de alto nivel mediante servicios de Live Compute. Puede aplicar un enfoque similar a juegos hospedadas en recursos de terceros.

1.  El cliente 'scout' crea una sesión de vale para representar el grupo. Esta sesión contiene una lista de centros de datos potencial, ubicado en la configuración de sesión en /constants/system/measurementServerAddresses. Se trata desde la plantilla de la sesión, si la lista del centro de datos es estática, o desde el cliente durante la creación de la sesión después de recibirlo desde el servicio de Live Compute. Esta sesión también contiene gsiSetId gameVariantId y maxAllowedPlayers valores del objeto personalizado/targetSessionConstants/gameServerPlatform.
2.  Todos los demás clientes en el grupo de unirse a la sesión de vale.
3.  Todos los miembros del grupo de descargar los valores de measurementServerAddresses desde el objeto /constants/system para la sesión de vale. Cada miembro, a continuación, haga ping a cada dirección mediante la API de plataforma y cargar una lista ordenada de los centros de datos preferido para la sesión, tal como se define en /members/ {index} / propiedades/sistema/serverMeasurements.

| Nota                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Puede establecer y recuperar los valores de measurementServerAddresses desde la sesión con el título **MultiplayerSession.SetMeasurementServerAddresses método** y  **Propiedad MultiplayerSessionConstants.MeasurementServerAddressesJson**. |

4.  Las llamadas de cliente 'scout' **MatchmakingService.CreateMatchTicketAsync método**, pasando una referencia a la sesión de vale.

| Nota                                                                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si los objetos de sesión de vale tienen constantes no coincidentes, el método CreateMatchTicket puede producir un error. Esto puede evitarse mediante la adición de un requisito INDISPENSABLE de regla para el hopper y evitar que la coincidencia de los jugadores con constantes no coincidentes. |

5.  Si el **MatchTicketDetailsResponse.PreserveSession propiedad** se establece en nunca, el servicio copia las mediciones de servidor de cada miembro en la representación interna del vale antes de aplanarlo el se recopilan en una sola colección para el vale y se almacenan como un atributo del vale "especiales".
6.  Si **MatchTicketDetailsResponse.PreserveSession propiedad** está establecido en siempre, el servidor no se usan medidas. En su lugar, el servicio copia el valor de /properties/system/matchmaking/serverConnectionString para la sesión en la representación interna del vale, como una colección serverMeasurements de tamaño 1 con una latencia cero.
7.  El servicio coincide con la sesión de vale con otras personas que representan otros grupos, tomar al servidor de colecciones de medición en la cuenta. Intenta hacer coincidir el grupo con otros grupos que tienen los mismos centros de datos altamente preferido.
8.  Una vez que se ha encontrado un grupo coincidente, el servicio crea o identifica una sesión de destino y agrega todos los participantes de las sesiones de vale que coinciden entre sí. El servicio escribe las mediciones de servidor sin formato final para el grupo coincidente en /properties/system/serverConnectionStringCandidates. A continuación, escribe las mediciones de servidor original para cada nuevo miembro en la sesión de destino a /members/ {index} / constantes/sistema/matchmakingResult/serverMeasurements.
9.  Todos los clientes realizan la inicialización en la sesión de destino, según se explicó anteriormente. Sin embargo, dado que los clientes se va a conectar en directo de proceso, no realizan QoS entre sí para confirmar la conectividad.
10. Algunas o todas las llamadas de clientes **GameServerPlatformService.AllocateClusterAsync método**. La primera de ellas desencadena la asignación, mientras que los demás sólo una confirmación de recepción. El método obtiene la sesión de destino de MPSD y elige un dataceter según las preferencias de centro de datos en la sesión de destino, así como la carga y otros conocimientos específicos de proceso en vivo.
11. Reproducen todos los clientes.

## <a name="configuring-smartmatch-for-your-title"></a>Configurar SmartMatch del título


### <a name="configuration-of-smartmatch-matchmaking-runtime-operations"></a>Configuración de las operaciones en tiempo de ejecución de SmartMatch contactos

Toda la configuración de contactos SmartMatch se produce a través de la [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) o [centro de partners](https://partner.microsoft.com/dashboard). Usa la configuración ServiceConfiguration -&gt;sección participan varios jugadores y contactos para un título.


#### <a name="matchmaking-session-template-configuration"></a>Configuración de plantilla de sesión de contactos

Como se describe en [SmartMatch contactos](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking), hay dos tipos de sesión de relacionadas con contactos, la sesión de vale de coincidencia y la sesión de destino de coincidencia. Básicamente, una sesión de vale es la entrada para el servicio, mientras que la sesión de destino es el resultado. Al configurar plantillas de sesión, debe crear una plantilla para cada tipo de sesión.

Para una sesión de vale, puede usar una plantilla dedicada. Como alternativa, puede volver a usar una plantilla para una sesión introductoria o en otra sesión no está pensado para usarse para el juego.

| Importante                                                                                      |
|-------------------------------------------------------------------------------------------------------------|
| La sesión de vale no debe tener habilitado comprobaciones de calidad de servicio y no debe marcarse con la capacidad de "juego". |

Para una sesión de destino, debe usar una plantilla que está pensada para el juego matchmade. Debe tener valores que habilitar las comprobaciones de calidad de servicio entre interlocutores antes del inicio del juego y deben estar marcados con la capacidad de "juego".

Con la configuración de la interfaz de usuario para XDP o centro de partners, puede asignar cada sesión a uno o varios hoppers, cada contenedor reglas que determinan cómo coinciden entre sí las sesiones en ese hopper. Para obtener más información, vea Configuración Hopper básica para los contactos.


#### <a name="basic-hopper-configuration-for-matchmaking"></a>Configuración básica Hopper para contactos

En esta sección define los campos utilizados para configurar campos hopper básica. Después de esta configuración, debe configurar las reglas hopper, como se describe en la configuración de las reglas de Hopper.

![](../../images/multiplayer/session_template_hopper_edit.png)


###### <a name="name"></a>Nombre

El nombre de la hopper que se usa al enviar una sesión de contactos. Este nombre debe coincidir con el valor pasado como parámetro a la **CreateMatchTicketAsync** método durante la creación del vale de coincidencia.


###### <a name="minmax-group-size"></a>Tamaño de grupo mín./máx.

Los tamaños mínimos y máximo para el grupo de Reproductor que debe crearse de sesiones en el hopper. El servicio intenta crear un grupo coincidente que es tan grande como sea posible, hasta alcanzar el tamaño máximo del grupo. Sin embargo, cree un grupo coincidente, si pueden ensamblar los reproductores suficiente para cumplir con el tamaño mínimo del grupo.


###### <a name="should-rule-expansion-cycles"></a>Debe ciclos de expansión de regla

Para un debería de regla, el servicio intenta aumentar el espacio de búsqueda y relajar las reglas de contactos proporcionado con el tiempo si se encuentra ninguna coincidencia correcta. Este proceso se realiza en varios ciclos, especificados mediante el campo debería ciclos de expansión de regla. En el último ciclo de expansión, se quitan las reglas debería para que ya no evitan que vales de coincidencia. Sin embargo, todavía se utilizan para determinar a la mejor coincidencia si existen varias entradas. Sólo número y tipos de QoS se expanden antes de que se quitan. Vea también reglas de configuración de Hopper en este tema.

Aumentar el valor de los ciclos de Explansion regla debe configuración proporciona más ciclos de debe regla de expansión, pero también aumenta la duración de contactos. El valor predeterminado es 3, que es normalmente suficiente para la mayoría de las configuraciones.

| Importante                                                                                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ciclos de expansión se producen en intervalos de tiempo fijo de 5 segundos. En el último ciclo de expansión, todas las reglas de "Debería" ya no se tienen en cuenta para el resto del intento de contactos. |

##### <a name="ranked-hopper"></a>Una clasificación Hopper
Normalmente SmartMatch impedirá que los jugadores bloqueados buscan. Si está activada Hopper clasificados, esta lógica se omite para evitar que los jugadores usando este sistema para evitar los encargados de mayor capacidad.

#### <a name="configuration-of-hopper-rules"></a>Configuración de reglas Hopper

En esta sección define los campos utilizados para configurar reglas para un hopper.

###### <a name="common-rule-fields"></a>Campos comunes de regla

Los campos definidos en esta sección son comunes a todas las reglas de hopper.

**Nombre de la regla**

El nombre descriptivo que se muestra para la regla para la configuración.

**Tipo de regla**

El tipo de regla. Las opciones son debe y debería. DEBEN tener reglas que se debe cumplir para contactos correcta. DEBEN puede relajar las reglas o quitadas para buscar a una coincidencia correcta. Para obtener más detalles sobre este proceso, consulte Configuración de la expansión de regla debe.


**Tipo de datos**

El tipo de datos del atributo de la regla de contactos. Los valores posibles son:   Número. Especifique un valor numérico de 32 bits simple.
-   Cadena. Especifique una cadena Unicode de hasta 128 caracteres.
-   colección. Especifica una matriz de cadenas. Use este valor para identificar contenido descargable (DLC), pertenencia escuadrón o preferencia de rol para reproductores.
-   Calidad de servicio. Especifique un tipo de datos personalizado para incluir datos de calidad de servicio de latencia en contactos. Debe usarse sólo una de estas reglas por hopper contactos.

| Nota |
|----------|
| Póngase en contacto con su administrador de cuentas de desarrollador si este límite es problemático para el título. |

-   Valor total. Especifique un tipo de datos personalizado que suma los valores enviados contactos. Puede usar este valor para asegurarse de que la suma resultante es dentro de un intervalo específico o un valor exacto.
-   Equipo. Especifique un tipo de datos personalizado para los equipos de los jugadores que se incluyen en las solicitudes de contactos. Puede usar este valor para evitar la división de los jugadores dentro de una incidencia de coincidencia única entre varios equipos.


###### <a name="data-type-specific-rule-fields"></a>Campos de la regla específica del tipo de datos

En esta sección define los campos que se usan para definir reglas que se aplican a algunos tipos de datos, pero no a otros usuarios. La interfaz de usuario debe ser capaz de aclarar qué tipos se aplican a reglas específicas de datos.

**Permitir caracteres comodín**

Un valor que indica si se puede omitir el atributo en el vale de coincidencia. Si se omite, el vale pasa a ser compatible con cualquier vale, independientemente del valor de este atributo.


**Origen de atributo**

El origen del valor de tipo de datos. Los orígenes posibles son:
- Proporcionados el título. El valor de datos se envía en el vale de coincidencia.
-   Instancia de estado de usuario. El valor de datos se recupera automáticamente desde el servicio UserStatistics.


**Nombre de atributo**

El nombre de origen del valor del atributo. Es el nombre de propiedad en el vale de coincidencia o el nombre de una estadística de usuario.


**Valor predeterminado**

El valor predeterminado para el tipo de datos, si ningún valor está especificado o está disponible para la solicitud de contactos. El valor predeterminado no se aplica cuando se selecciona el campo permiten caracteres comodín y se especifica ningún valor.


**Weight**

La importancia de la regla. La ponderación puede utilizarse para indicar qué reglas se priorizan durante la expansión de la regla y contactos. El valor del peso debe ser positivo y el valor predeterminado es 1.


**Flatten (método)**

Solo los tipos de datos de número. Un valor que indica cómo varios valores se combinan para satisfacer a una coincidencia. Se aplica a varios valores para diferentes jugadores en un vale de coincidencia única y a través de varias entradas. Los valores posibles son:
-  Min/Max. Use el valor mínimo o máximo de varios valores de vales de coincidencia diferentes.
-   Promedio. Utilice el valor medio de varios valores de vales de coincidencia diferentes.


**Diferencia máxima**

Solo los tipos de datos de número. La diferencia máxima aceptable numérica entre dos valores comparados para satisfacer una regla. Para un debería regla, este valor es el punto de partida para la expansión de la regla.


**Operación de establecimiento**

Colección solo tipos de datos. Operación que se va a realizar en el grupo de valores del conjunto de coincidencia. Las opciones posibles son:
- Intersección. Coincide con dos colecciones basadas en la cantidad de intersección entre ellos. Esta configuración da como resultado valores de la colección similares o idénticos.
-   Diferencia. Coincide con dos colecciones basadas en la cantidad de diferencia entre ellos.
-   Preferencia de rol. Coincide con las colecciones según las preferencias para el rol de un reproductor en los modos de juego basado en roles.


**Intersección de destino**

Parte de la configuración de la operación de conjunto. La intersección mínima o diferencia máxima de dos colecciones antes de que se hacen coincidir.


**Topología de red**

Calidad de servicio del tipo de datos únicamente. La topología de red que se usa para la calidad de servicio. Los valores posibles son de punto a punto, igual al Host y cliente/servidor.


**Latencia máxima/escalado máximo**  
Calidad de servicio del tipo de datos únicamente. La latencia máxima de contactos correcta dentro de la topología de red especificado. Este valor se trata como un valor de escala (en lugar de una latencia necesaria) cuando debe regla utilizando un cliente / servidor de calidad de servicio.


| Nota                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Además, las reglas de reputación de forma predeterminada también se aplican a un hopper. Estas reglas no se puede quitar y se usan para garantizar el control correcto de la reputación durante los contactos. |


**Permitir la espera de Roles**

Solo tipo de datos de las preferencias de rol de la colección. Especifica si el servicio de la coincidencia contiene el vale de contactos con el fin de rellenar todos los roles disponibles.


###### <a name="expansion-delta"></a>Delta de expansión

Valor que indica cuánto Relaje la regla enviada por cada generación de expansión. El delta de expansión se aplica además del valor de Max Diff. Para obtener más información, vea Ejemplo 1 (expansión de regla).

También puede usar el delta de expansión para expandir varios valores numéricos a distintas velocidades. Esto no es posible a través de la expansión ciclo configuración porque se aplica a todas las reglas. En su lugar, el enfoque es usar valores decimales de expansión, por ejemplo, 0.4. Una expansión solo se produce cuando se alcanza un entero de nuevo, lo que permite la expansión diferentes velocidades, incluso para el mismo número de ciclos de expansión.


###### <a name="qos-expansion-peer-to-peer-peer-to-host"></a>Expansión de QoS (Peer-to-Peer, del mismo nivel para el Host)
Para la calidad de la expansión de tipo de servicio para los juegos del mismo nivel, no se puede configurar el delta de expansión. En su lugar, debe usar una de las siguientes estrategias de expansión.

<table>

<tr>

<tr>
<td>
<b>1. MaxLatency < 256.</b><p/><p/>

Expansión se realiza en MaxLatency * ciclo de expansión.<p/>
Por ejemplo, si el valor inicial es 200, 200 se utiliza en el primer ciclo, 400 en el segundo ciclo y así sucesivamente.
</td>
</tr>

<tr>
<td>
<b>2. MaxLatency > o igual a 256</b><p/><p/>

Expansión se escala linealmente desde 50 hasta MaxLatency - 256.<p/>
Por ejemplo, si el valor inicial es 556, el valor se amplía linealmente entre 50 y 300 sobre el número de ciclos. Es decir, si se eligieron seis ciclos, los valores sería 50, 100, 150, 200, 250 y 300. Si se eligieron cinco ciclos, los valores sería 50, 112,5, 175, 237.5 y 300.
</td>
</tr>

</tr>
</table>

##### <a name="qos-expansion-client-server"></a>Expansión de QoS (cliente / servidor)
Cuando se usa servidores dedicados, la expansión se basa en la preferencia relativa. Solo preferida en los servidores se considerará en ciclos de expansión al principio. Con el tiempo, sí, menos servidores preferidos se usará. Para asegurarse de expansión adecuados, se requiere un valor similar a MaxLatency, llama a escala máxima. Este escalado máximo todavía se debe establecer en el tiempo de ping aceptable más grande, sin embargo, este valor proporciona una escala relativa para los tiempos de ping de otro servidor proporcionados por un Reproductor, en lugar de proporcionar un requisito indispensable para tiempos de ping. Tenga en cuenta que puede excluir servidores con tiempos de ping inaceptable intencionadamente quitándolos de la lista en la solicitud.

#### <a name="example-1-rule-expansion"></a>Ejemplo 1 (expansión de regla)

Nivel del Reproductor se utiliza para contactos y reproductores cumplen flexible, según la precisión de sus niveles. Se prefieren los reproductores con la menor cantidad de diferencia entre sus niveles.

-   Regla de nivel del Reproductor
-   Tipo de regla: DEBE
-   Tipo de datos: Número
-   Diferencia máxima: 1
-   Expansión Delta: 2
-   Flatten (método): Media

De forma predeterminada, la diferencia entre los niveles de Reproductor necesaria es 1 o menos. Si dentro de esta diferencia se encuentra una coincidencia, se cotejan los jugadores. Si se encuentra ninguna coincidencia inicial, el valor de nivel del Reproductor se expande en 2 para cada iteración (tres iteraciones de forma predeterminada). Este escenario provoca un comportamiento de contactos para un Reproductor de nivel 20, tal como se muestra en la tabla siguiente.

| Paso                    | Valor de nivel de los posibles candidatos de coincidencia | Distancia del nivel efectiva para coincidencia encontrada |
|-----|
| Inicial enviado valor | 19-21                                      | 1                                             |
| Ciclo de expansión 1       | 17-23                                      | 3                                             |
| Ciclo de expansión 2       | 15-25                                      | 5                                             |
| Ciclo de expansión 3       | 13-27                                      | 7                                             |

A medida los ciclos de expansión, aumenta la distancia del nivel efectiva para una coincidencia correcta sin modificar el valor de Max Diff. Solo el valor de nivel de Reproductor es más flexible.


#### <a name="example-2-collection-rule"></a>Ejemplo 2 (regla de recopilación)

El juego libera tres tipos de DLC que están disponibles para los jugadores. Se aplica esta regla de contactos a contactos de juego "Sólo DLC" y un reproductor debe poseer al menos un DLC para ser matchmade con otros jugadores.

-   Regla de Reproductor DLC
-   Tipo de regla: DEBE
-   Tipo de datos: Colección
-   Operación establecer: Intersección
-   Intersección de destino: 1

Reproductores de evaluación sus DLCs y enviar los valores mostrados en la siguiente tabla en sus vales de coincidencia.

| Nota                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| En la tabla siguiente, el valor de colección indica la propiedad de DLC. Si DLC está disponible para el Reproductor y en 0 si no se establece en 1. |

| Reproductor   | Valor de colección | Coincide con los jugadores | Nota           |
|----------|------------------|----------------------|----------------|
| Jugador 1 | { "1", "1", "1"} | 3                    | Posee todas DLCs  |
| Reproductor de 2 | { "0", "0", "0"} | Ninguno                 | No posee ningún DLC    |
| Reproductor de 3 | { "1", "0", "0"} | 1                    | Posee primera DLC |

Si la intersección de destino en el ejemplo se establece en 2, reproductores de 1 y 3 no coincidirá, ya que la intersección entre ellas es tan solo 1.

#### <a name="example-3-avoid-previous-players"></a>Ejemplo 3 (evite los jugadores anteriores)
El título prefiere evitar un juego con el reproductor reproduce más recientemente.
* Tipo de regla: DEBE
* Tipo de datos: Colección
* Operación establecer: Diferencia
* Intersección de destino: 0

## <a name="defining-team-rules-during-smartmatch-configuration"></a>Definir reglas de equipo durante la configuración de SmartMatch

### <a name="configuring-team-rules"></a>Configuración de reglas de equipo

Para configurar la regla de equipo, empiece por crear uno en su plataforma de configuración elegido (XDP o centro de partners). Rellene los tamaños de equipo espera que su juego a crear a partir de los vales que se hacen coincidentes en este hopper. Por ejemplo, si su juego espera 4v4, debe crear dos entradas, se espera un tamaño máximo de 4 cada uno y un nombre diferente. Hay un mínimo tamaño del equipo: Utilice esta opción si se puede reproducir un juego con menos jugadores en un equipo. En caso contrario, los valores mínimo y máximo deben ser el mismo valor.


#### <a name="using-team-rules"></a>Uso de reglas de equipo

Una vez que se configura la regla de equipo, vales dentro de la hopper se impedirá coincidencia si no hay ninguna manera para ajustarse a sus grupos en los equipos sin causar una división. La regla escribirá la asignación de equipo resultante en la sesión de destino, en los miembros, constantes, personalizado, matchmakingresult/initialTeam. Tenga en cuenta que esto es simplemente una asignación sugerida, el título es posible que reorganizando los jugadores puede crear un juego de más calidad, al tiempo que evitan los vales de la división en distintos equipos.

Si se crea un vale para un juego que está en curso, la regla de equipo necesitará información adicional. Por ejemplo, suponga que 8 jugadores están en una lista de los jugadores 4v4 juego cuando dos o están desconectados. El título que rellene estas ranuras vacías, pero no puede reorganizar los equipos mientras se reproduce el juego.

Si intenta rellenar juegos en curso se representa mediante vales con el campo PreserveSession establecido en "siempre". En casos como éste, porque ya se han asignado los equipos a los jugadores, el título debe especificar la asignación de equipo actual, para coincidencia sepa cuántos espacios están abiertos en cada equipo. Para proporcionar los nombres de los equipos de cada jugador está activado, cada jugador escribe su nombre de equipo en la sesión de juego en miembros/me/propiedades/sistema/grupos. Este campo es un JArray.

Una vez que las propiedades anteriores se escriben en la sesión de juego, un jugador crea un vale para la sesión, en un intento de buscar varios jugadores. Cuando se cumple el vale, coincidencia nuevo escribirá el equipo de los jugadores que se unan a sugeridos en miembros, constantes, personalizado, matchmakingresult/initialTeam


###### <a name="preferring-even-teams"></a>Preferir incluso equipos

Además, las coincidencias se realizará con los equipos más grandes en primer lugar. Esto significa que en una hipotética 4v4 hopper, vales de 4 jugadores que coincidirá primero, hasta que no queden incidencias de 4. Vales de 3, a continuación, con, extrayendo singletons como necesite y así sucesivamente. De esta manera, por lo general se reproducirán vales de tamaño similar comparándolas entre sí, si están presentes y no evita que otras reglas. Tenga en cuenta que esto proporciona la prioridad bastante fuerte equipo regla a través de otras reglas. Por ejemplo, suponga que tiene una población limitada, que consta de un vale de tamaño 4 con skill(A) alta, un vale de tamaño 4 con skill(B) baja y cuatro incidencias de tamaño 1 con skill(C-F) alta. La regla de equipo hará que la coincidencia que prefiere la coincidencia A y B, en lugar de A, C, D, E y f el.


###### <a name="should-variant"></a>Debe Variant

Debe regla evita la división de todas las generaciones de vale y proporciona la ordenación de los equipos incluso prefer. La debe regla es idéntica hasta la última generación, una vez allí, se pueden dividir vales, aunque la ordenación de los equipos incluso prefer estará todavía activa.

## <a name="target-session-initialization-and-qos"></a>Inicialización de la sesión de destino y la calidad de servicio

Un grupo de jugadores coincide con una sesión de destino SmartMatch contactos, como se describe en [SmartMatch contactos](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking). El título debe realizar los pasos para confirmar que suficientes jugadores han unido que puede conectar correctamente entre sí si es necesario. Este proceso se conoce como la inicialización de la sesión de destino.

Para juegos con topologías de red punto a punto, un aspecto importante de la inicialización de la sesión de destino es la evaluación y valoración de calidad de servicio. Operaciones asociadas son la medición de la latencia y ancho de banda entre las consolas Xbox One (o entre las consolas y los servidores) y la evaluación de las medidas resultantes para determinar si la conexión de red entre los nodos es buena.

El diagrama de flujo siguiente muestra cómo implementar la inicialización de la sesión de destino y las operaciones de calidad de servicio.

![](../../images/multiplayer/Multiplayer_2015_Matchmaking_QoS.png)

### <a name="managed-initialization"></a>Inicialización administrada

MPSD admite una característica llamada "administrados inicialización" a través del cual coordina el proceso de inicialización de la sesión de destino a través de los clientes que participan en una sesión. MPSD automáticamente realiza un seguimiento de las fases de inicialización y los tiempos de espera asociado para la sesión y se evalúa como la conectividad entre los clientes si es necesario. Inicialización administrada está representada por la **MultiplayerManagedInitialization clase**.

| Nota                                                                                                                  |
|------------------------------------------------------------------------------------------------------------------------------------|
| Se recomienda para el título con SmartMatch contactos para aprovechar las ventajas de la MPSD administrada característica de inicialización. |


#### <a name="managed-initialization-episodes-and-stages"></a>Episodios de inicialización administrada y las fases

La inicialización administrada siempre contactos nuevos jugadores que agrega a la sesión se somete a una sesión de destino. SmartMatch agrega a los miembros de la sesión como el estado de usuario reservado, lo que significa que cada miembro ocupa un espacio, pero no ha unido todavía la sesión. Cada grupo de nuevos jugadores que desencadena un nuevo episodio de inicialización.

Una vez completada la inicialización, cada jugador tiene éxito o produce un error en el proceso. Puede reproducir un jugador que se realiza correctamente en la inicialización del uso de la sesión de destino. Un reproductor que no debe enviarse a los contactos que se debe coincidir en otra sesión. Para los casos donde una sesión se envía a los contactos con la *preserveSession* parámetro establecido en siempre, los miembros de la sesión ya existentes no sufren la inicialización, como MPSD se da por supuesto que se ha configurado correctamente.

Cada episodio de la inicialización administrada consta de estas fases:

-   Unir--los miembros de la sesión escriben ellos mismos en la sesión para mover su estado de usuario de reservado a Active y cargar los datos básicos, como la dirección de dispositivo segura.
-   Medida--para las topologías punto a punto, los miembros de la sesión medir la calidad de servicio entre sí y carguen los resultados en la sesión.
-   Evaluación--MPSD evalúa los resultados de las dos últimas fases y determina si la sesión o los miembros han inicializado correctamente.

El código de título opera en la sesión para avanzar a cada usuario (y, por tanto, la sesión) a través de las fases de unión y medición. A continuación, puede iniciar reproducir o volver a contactos después de la fase de evaluación ha tenido éxito o error.


### <a name="configuring-the-target-session-for-initialization"></a>Configuración de la sesión de destino para la inicialización

El título puede configurar el proceso de inicialización administrada usar constantes en la sesión de destino que se está inicializarla. Estas constantes se establecen en /constants/system en la plantilla de sesión con la versión 107, la versión de plantilla recomendada. Se pueden realizar dos tipos de valores de configuración: opciones que permiten configurar el proceso de inicialización administrada como un todo y que configurar los requisitos de calidad de servicio. Consulte [MPSD sesión plantillas](multiplayer-session-directory.md) para obtener ejemplos de plantillas de sesión para escenarios comunes de título.

| Nota                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------|
| Si no se definen los requisitos de calidad de servicio en la configuración de inicialización de sesión de destino se omite la fase de medición durante la inicialización. |


#### <a name="configuring-managed-initialization-as-a-whole"></a>Configuración de inicialización administrados como un todo.

A continuación se muestran los campos que se configuran para controlar la inicialización administrada general. Forman parte del objeto /constants/system/memberInitialization:
- joinTimeout. Especifica cuánto tiempo espera MPSD para que cada miembro al cual unirse, después de que el episodio de la inicialización ha comenzado. Valor predeterminado es 10 segundos.
-   measurementTimeout. Especifica cuánto tiempo espera MPSD para que cada miembro cargar los resultados de la medida de calidad de servicio, una vez iniciada la fase de medición. Valor predeterminado es 30000 segundos.
-   membersNeededToStart. Especifica el número de miembros que debe realizarse durante la inicialización para el primer episodio de inicialización se realice correctamente. El valor predeterminado es 1.

| Nota                                              |
|----------------------------------------------------------------|
| Si no se alcanza este umbral, todos los miembros producirá un error en la inicialización. |


#### <a name="configuring-qos-requirements"></a>Configurar los requisitos de calidad de servicio

Calidad de servicio solo es necesario durante la inicialización si el título usa una topología punto a punto o del mismo nivel para el host. Cada topología se asigna a una constante específica de topología en /constants/sistema /.
###### <a name="configuring-qos-requirements-for-peer-to-peer-topology"></a>Configurar los requisitos de QoS para topología punto a punto

| Nota                                                                                                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Es raro que los títulos realizar la configuración del requisito de calidad de servicio para la topología punto a punto. Estos valores son muy restrictivos y causar problemas para jugadores con traducciones de direcciones de red estrictas (NAT). |

Requisitos de QOS de la topología punto a punto se establecen en el objeto peerToPeerRequirements. Cada cliente debe ser capaz de conectarse a todos los demás clientes. El objeto tiene los siguientes campos pertinentes:
-   latencyMaximum. Especifica la latencia máxima entre los dos clientes.
-   bandwidthMinimum. Especifica el ancho de banda mínimo entre los dos clientes.


###### <a name="configuring-qos-requirements-for-peer-to-host-topology"></a>Configurar los requisitos de QoS para topología punto a host

Requisitos de QOS de la topología del mismo nivel para el host se establecen en el objeto peerToHostRequirements. Cada cliente debe ser capaz de conectarse a un único host común. Si este objeto está configurado y la inicialización se realiza correctamente, MPSD creará una lista inicial de los clientes que son posibles hosts, conocidos como "candidatos de host". Estos son los campos para establecer:
-   latencyMaximum. Especifica la latencia máxima entre cada elemento del mismo nivel y el host.
-   bandwidthDownMinimum. Especifica el ancho de banda mínimo de nivel inferior entre cada elemento del mismo nivel y el host.
-   bandwidthUpMinimum. Especifica el ancho de banda mínimo de nivel superior entre cada elemento del mismo nivel y el host.
-   hostSelectionMetric. Especifica la métrica utilizada para seleccionar el host.

## <a name="see-also"></a>Consulte también

[Plantillas de sesión MPSD](multiplayer-session-directory.md)

[Operaciones en tiempo de ejecución SmartMatch](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking)

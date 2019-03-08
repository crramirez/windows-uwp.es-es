---
title: Conectado prácticas recomendadas de almacenamiento
description: Recomendaciones sobre cómo obtener el mejor rendimiento y experiencia de almacenamiento conectado
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: dbdb8f95e9afc44a3280d897e74f74d327f94b97
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632930"
---
# <a name="connected-storage-best-practices"></a>Procedimientos recomendados de almacenamiento conectado

Los desarrolladores deben dividirse entre guardar datos en agrupaciones lógicas que son actualizables por separado en lugar de escribir guarda monolítica. Esto permite que los títulos reducir la cantidad de datos que se escriben en varias situaciones, lo que reduce tanto consumo de recursos local y cargar el uso de ancho de banda. La API también permite títulos actualizar más de un elemento de datos en una operación atómica que se garantiza que se realice correctamente por completo o no surten efecto en absoluto (por ejemplo en el caso de guardar intermedio de error grave).

Dado que Xbox One permite a los usuarios cambiar rápidamente entre los títulos, los desarrolladores deben diseñar su puesto para mantener su estado actual listo para guardar en el aviso breve en previsión de recibir un evento de suspensión, lo que puede suceder prácticamente en cualquier momento. Los usos de API de almacenamiento conectado RAM fuera de la reserva de título como el primer punto de almacenamiento con el fin de maximizar la velocidad de escritura de título durante el breve suspensión el período de tiempo. El sistema conserva los datos en un almacenamiento duradero, concilia con cualquier otro dato se escriba desde la última actualización y cargas de datos de programa. Una vez que se almacenan y ponen en cola para la carga, el sistema es sólido a varios errores como error de alimentación o de pérdida de conectividad de red.

## <a name="when-to-load-a-users-connected-storage-space-data"></a>Cuándo se debe cargar los datos de espacio de almacenamiento conectados de un usuario

El tiempo de ejecución para cargar el espacio de datos de almacenamiento con conexión de un usuario puede variar. Aplicaciones realice esta acción durante la ejecución principal, en lugar de en respuesta a partir de un usuario cerrar la sesión o en respuesta a recibir una notificación de suspensión del sistema.
Por lo general, las aplicaciones deben cargar un espacio de almacenamiento conectado como un usuario inicia sesión e indica el deseo de jugar, a menos que el juego en un modo en el que está determinado no funcionalidad de guardar es necesaria. También debe considerar la alineación de la carga de un espacio de almacenamiento conectado con largas secuencias de la carga de datos, para que puedan ejecutar las operaciones en paralelo.
Una vez que la aplicación haya cargado los datos de espacio de almacenamiento con conexión de un usuario, deben conservar este valor para futuras guarda. Conservar un espacio de almacenamiento conectado con el tiempo no tiene efectos negativos en el rendimiento o estabilidad. Dado que al cargar un espacio de almacenamiento conectado provoca una comprobación de sincronización con la nube si el sistema está en línea, liberar y volver a cargar el espacio de almacenamiento con conexión de un usuario durante la lenta o condiciones de red no confiable podrían provocar que el usuario vea una interfaz de usuario de sincronización hasta que el sistema agota el tiempo. Conectado no es necesario liberar explícitamente para que se produzca la sincronización de nubes espacios de almacenamiento. Una vez al controlador de finalización especificado en un `SubmitUpdatesAsync` llamada volvió con `AsyncStatus::Completed`, el sistema se encarga de la sincronización con la nube si la aplicación libera el `ConnectedStorageSpace` objeto.

## <a name="when-to-save"></a>Cuándo se debe guardar

Cada vez que una aplicación recibe una notificación de suspensión, la aplicación al menos debe guardar los datos pertinentes, que permite al sistema volver a un estado de contexto adecuado para el usuario.

Si el diseño de juegos usa periódica, se puede llamar con más frecuencia que al recibir una notificación de suspensión; automática o iniciada por el usuario guarda, almacenamiento conectado Si lo hace por lo que es una buena forma de reducir el riesgo de pérdida de datos debido a la pérdida de alimentación o un bloqueo.
Si va a desarrollar su juego con el XDK, cuando un usuario cierre la sesión, el objeto de usuario para que el usuario siga siendo válido y, en ese momento la aplicación puede realizar final guarde operaciones mediante el uso de almacenamiento conectado.

## <a name="robustness"></a>Potencia

Porque siempre se sincronizan datos guardados en la nube, un error en los datos guardados y código de aplicación que hace que la aplicación se bloquee podría una copia de seguridad en la nube y se distribuyen entre los dispositivos. Para evitar que los usuarios tener una aplicación que se bloquea en cada inicio, diseñar la aplicación para asegurarse de que:

-   Los usuarios pueden alcanzar un punto en la aplicación en el que puede administrar el estado guardado, incluso si algunos de los datos guardados están mal formados.
-   La aplicación puede controlar los datos dañados automáticamente, recuperar tantos datos como sea posible y todo lo demás a un estado seguro reinicializar.

## <a name="use-cases-for-save-game-designs"></a>Casos de uso para los diseños de juego de guardar

El diseño de un juego de guardar el sistema que realiza el mejor uso de contenedores en el espacio de almacenamiento con conexión depende del tipo de aplicación:

### <a name="single-save"></a>Guardar único

Para las aplicaciones que usan una única, estilo de campaña Guardar del sistema, como un primer shooter persona:

-   Poner todos los datos en un solo contenedor y siempre se escriben en el mismo contenedor identificado por su nombre.
-   Considere la posibilidad de exponer una opción para restablecer todos los datos que borrarán todos datos guardados de un usuario, en caso de que desea empezar a reproducir la aplicación desde el principio, con ningún progreso anterior que se conservan.

### <a name="multiple-saves"></a>Guarda varias

Para las aplicaciones que tienen un número fijo de guardar ranuras, vamos a decir que cinco, hay dos maneras de utilizar contenedores para guardar los datos del juego:

-   Store todos los 5 ranuras dentro de un contenedor de nombre fijo con 1 blob para cada espacio de almacenamiento. Con este método 5 ranuras de todos los serán totalmente sincronizada y disponible, o bien, en el caso de que se produce un error de sincronización en cualquier momento, ninguna de las ranuras se sincronizarán y permanecerán en su estado anterior. Si un usuario juega la aplicación sin conexión en dos consolas, guardar el progreso en la ranura 1 en la consola de la primera y en la ranura 2 en la consola de segundo, el usuario debe elegir qué datos se deben conservar sobre la conexión ambas consolas Xbox Live; la lógica de combinación para los contenedores producirá un conflicto.
-   Store cada ranura en un contenedor con su propio nombre. Esto permite que el progreso independiente en cada ranura, incluso en varios equipos que pueden estar sin conexión. Sin embargo, si un usuario cancela la mitad a través de una sincronización, es posible que sólo algunas de las ranuras estará disponible durante la sesión; algunos de los contenedores que no haya realizado descarga. En tal caso, se notifica al usuario que la sincronización esté incompleta y que algunos de los datos en la nube no se encuentra en la consola local.

El enfoque que se usa, la aplicación debe proporcionar al usuario con la interfaz de usuario para eliminar guarda individual de las ranuras.

### <a name="warning"></a>Advertencia

**No almacene datos dependientes en contenedores.** No almacene datos con las dependencias entre más de un contenedor. Los contenedores se pueden separar debido a la sincronización incompleta, pérdida de energía u otras condiciones de error. Datos almacenados en un único contenedor deben ser autosuficientes y coherente.

### <a name="tips"></a>Sugerencias

**Impedir que a los usuarios si se desactiva la consola o al salir.** El título debe impedir que a los usuarios si se desactiva la consola o salir de la aplicación al guardar. En la consola Xbox 360, si un usuario apaga el sistema mientras se está guardando el título, no se guardan los datos del usuario. En Xbox One, su título recibe un evento de suspensión y tiene 1 segundo para usar la API de almacenamiento conectado a guardar el estado. El sistema garantiza que los datos están confirmados correctamente en el disco duro antes de que se cierra completamente o entra en su estado de bajo consumo de energía. Si el usuario expulsa el disco del título para reproducir en otro, se produce el mismo proceso de suspensión.

**Conservar los espacios de almacenamiento conectado.** Conservar objetos ConnectedStorageSpace en lugar de intentar cargar ellos cada vez que se produce un evento de lectura o escritura. No hay ningún efecto negativo en el rendimiento o solidez deber conservar un objeto ConnectedStorageSpace durante un período prolongado.

**Mantener el tamaño de los datos más pequeño.** Mantener pequeño el tamaño de los datos guardados. Cuando la consola está en línea, se carga todos los datos de usuario en el almacenamiento conectado a la nube. Optimice sus formatos de datos para asegurarse de retrasos mínimo y el uso de ancho de banda.

**Compruebe que los usuarios no le importa no va a guardar.** Compruebe si hay errores de OutOfLocalStorage devueltos GetForUserAsync y SubmitUpdatesAsync y consultar a los usuarios para ver si realmente desea reproducir sin guardar los cambios. Si un usuario indica que se desean guardar partidas, vuelva a intentar la operación.

**Comprobar la cuota y el símbolo del sistema para liberar espacio del usuario.** Comprobar si hay un error QuotaExceeded devuelto por SubmitUpdatesAsync. Si la aplicación recibe este mensaje, notificar a los usuarios que no se puede guardar más datos hasta que se haya liberado espacio y presentarlos con interfaz de usuario que les permite hacerlo. Cada usuario obtiene 64 o 256 MB de datos por aplicación y cada título XDK obtiene 64 MB de almacenamiento por máquina local en la consola.

**Guardar el estado de los menús de restauración más adelante.** Guardar estado de menú y otras opciones de la aplicación además de guardar los datos del juego core. Si el usuario desempeña en otra aplicación y, a continuación, vuelve a la suya, restaurar a un estado del menú contextual correspondiente.
Responder a los cambios de usuario con sesión iniciada. Los usuarios pueden cerrar la sesión mientras se suspende la aplicación. Cuando se reanuda la aplicación, debe determinar si ha cambiado el conjunto de usuarios con sesión iniciada. Considere la posibilidad de navegar a una ubicación adecuada dentro de la aplicación, como un menú, cuando esto ocurre.

**Proporciona la interfaz de usuario para administrar los datos guardados.** La aplicación debe proporcionar una interfaz de usuario que permite a los usuarios administrar sus datos guardados dentro de la aplicación. Para las aplicaciones con una automática del sistema de almacenamiento, la aplicación debe ofrecer una opción para restablecer los datos que desea permitir que los usuarios restablecer el estado predeterminado de reproducción.

**Asegúrese de que los usuarios siempre pueden llegar a la interfaz de usuario para administrar las partidas guardadas.** Asegúrese de que la aplicación siempre puede alcanzar su interfaz de usuario de administración para los juegos guardados, incluso en presencia de buggy datos guardados. Si los datos guardados de un usuario se vuelven ilegibles debido a un error o datos dañados de la aplicación, la aplicación debe permitir a los usuarios recuperar a un estado que no bloquear o impedir que se reproduce.
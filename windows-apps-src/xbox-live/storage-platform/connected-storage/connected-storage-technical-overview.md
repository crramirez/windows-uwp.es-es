---
title: Introducción técnica al almacenamiento conectado
description: Un análisis detallado sobre el funcionamiento interno de almacenamiento conectado.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 6eddd11a370b8dcadc5108fe00539c2c6d1d9d1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624060"
---
# <a name="connected-storage"></a>Almacenamiento conectado

> [!NOTE]
> Este documento se escribió originalmente para los desarrolladores administrados asociados Xbox One.  Algunas de la Xbox One contenido específico, como el almacenamiento Local y el título pueden omitirse para UWP en Windows.  El contenido conceptual y la API en este documento siguen siendo pertinentes.  Póngase en contacto con su representante de Microsoft (si procede) tiene alguna pregunta.

El almacenamiento y son muy diferentes de las de Xbox 360; guardar modelos en Xbox One Xbox One presenta un modelo de aplicación más flexible que admite aplicaciones rápida conmutación de varias aplicaciones simultáneas, y rápido suspender y reanudar de aplicaciones. Datos almacenados mediante la API de almacenamiento conectado, automáticamente se desplaza para que los usuarios a través de varias consolas Xbox One y también está disponibles para su uso sin conexión.

En esta sección se tratan los siguientes temas:

-   Mediante la API de almacenamiento conectado a juegos de la tienda guardado y los datos de la aplicación en Xbox One.
-   Procedimientos recomendados para el diseño de la aplicación guarde los sistemas, para que integran correctamente con las ventajas de la experiencia de usuario proporcionadas por el modelo de aplicación de Xbox One.
-   Cómo el sistema de almacenamiento conectado maximiza la velocidad con la que la aplicación puede guardar los datos
-   Cómo el sistema controla los conflictos de datos y sincronización de datos de varias consolas sin necesidad de UI de la aplicación.
-   El modelo de resistencia de almacenamiento conectado, que está diseñado para que la aplicación siempre tiene una vista coherente de los datos almacenados en contenedores individuales, incluso en el caso de pérdida de energía o conectividad de red interrumpida.

> [!NOTE]
> El término *aplicación*, tal y como se usa en este caso, hace referencia a cualquier aplicación que se ejecuta en la consola, incluidos los juegos.

## <a name="overview"></a>Introducción

El modelo de aplicación de Xbox One permite a los usuarios usar varias aplicaciones a la vez, lo que significa que la aplicación no puede pedir al usuario a esperar que guardarse antes de desactivar la consola o al pasar a otra aplicación de datos. Los usuarios de Xbox One también disfrutarán de tener sus datos se mueven automáticamente a través de las consolas para que cada consola Xbox One se siente como si su propia consola. La plataforma Xbox One proporciona la API de almacenamiento conectado para ayudar a su aplicación a cumplir estos requisitos.

El sistema de almacenamiento conectado permite que las aplicaciones almacenar datos como uno o más *blobs* en *contenedores*. Cuando una aplicación guarda los datos, que es rápidamente copiar desde la partición exclusiva en la partición compartida para que se pueden controlar las tareas de almacenar los datos en disco y cargarlo al almacenamiento de Xbox Live título fuera de la duración de la aplicación.

Cuando la aplicación solicita datos de un usuario específico del sistema de almacenamiento conectado, el sistema comprueba con la nube para los datos actualizados automáticamente y se notifica a los usuarios si necesitan esperar que la descarga de datos. El sistema también solicita a los usuarios elegir entre los datos en conflicto en algunos casos, como cuando un usuario ha desempeñado fuera de línea en más de una consola, o cuando se está cargando otra consola guarda los datos para ese usuario.

La aplicación también tiene una cantidad dedicada, pero limitada, de espacio de almacenamiento en la nube para todos los usuarios, por lo que los usuarios no tendrán que tomar decisiones difíciles sobre eliminar permanentemente los datos de una aplicación para dejar espacio para guarda la otra aplicación. Sin embargo, hay una cantidad limitada de espacio de almacenamiento en el disco duro Xbox One para almacenar en caché guarda localmente, por lo que el sistema proporciona una experiencia de usuario para liberar espacio en memoria caché local. Los usuarios están en el control de lo que se almacena en caché localmente; nunca perder el acceso a los datos que les interesen cuando están reproduciendo sin conexión. El sistema de almacenamiento conectado también permite una pequeña cantidad de datos independiente del usuario se almacena localmente para la aplicación. Estos datos por equipo y no se mueven y no se cargan en la nube.

Xbox One sistema de almacenamiento conectados se encarga de la administración de energía del sistema para que pendientes operaciones de escritura en el disco duro y las cargas a la nube se controlan automáticamente, no es necesario para los títulos mostrar la interfaz de usuario para que diga "Guardar en curso, no desactive la consola."

### <a name="the-xbox-one-app-model-and-app-navigation"></a>La navegación de aplicación y el modelo de aplicación Xbox One

Xbox One permite a los usuarios cambiar rápidamente entre varias aplicaciones. Aplicaciones escritas bien permitir que los usuarios retome donde dejó durante su última actividad con contexto pertinente cargar rápidamente, incluso si se cerró la aplicación desde la última vez que el usuario accedió a él.

Las consolas Xbox One pueden ejecutar solo una aplicación en la partición exclusiva a la vez. Presentar una experiencia de usuario rápida conmutación requiere que la aplicación actualmente en ejecución exclusiva para apagar rápidamente cuando el usuario desea ejecutar otra. Cuando un usuario intenta cambiar a otra aplicación, el sistema envía la aplicación activa una notificación de suspensión. Durante este tiempo, la aplicación debe guardar el estado pertinente y devolver resultados de su función de suspensión. El sistema aplica un límite de tiempo máximo de 1 segundo para esta operación. Si la aplicación no ha devuelto menos de 1 segundo, el sistema termina forzosamente la aplicación. Las aplicaciones no pueden impedir que los usuarios al salir, igual que con el modelo de navegación en tabletas y smartphones modernos.

También hay otros casos en los que se suspende una aplicación exclusivo, como el sistema escribir un estado inactivo o bajo de energía cuando el usuario presiona el botón de encendido en la consola. Una vez que se suspende la aplicación, se puede reanudar sin que se descarguen por el sistema. Esto habilita la funcionalidad de reanudación rápida. Lo importante es saber es que las aplicaciones suspendidas también pueden finaliza o reanudar. La aplicación siempre debe guardar el estado en caso de que se termina.

Para que funcione bien con el modelo de aplicación de Xbox One, las aplicaciones deben estar preparadas para serializar el estado en un búfer de memoria rápidamente para que se puede guardar estado pertinente en 1 el segundo período de tiempo de suspensión.

Tenga en cuenta que hay una diferencia entre los datos guardados sobre el juego de usuario y datos sobre el estado de la aplicación, como la ubicación dentro de los menús. Además de guardar el juego cuando se suspende la aplicación, debe considerar conservar el estado de menú si el usuario está en medio de una configuración o personalizar un carácter fuera el principal motor de juego.

Los usuarios pueden dejar un juego en suspenso durante mucho tiempo. Considere la posibilidad de ofrecer una experiencia diferente cuando se reanuda el juego después de una suspensión largo. Quitar un usuario de vuelta en un bombardeo de su campaña podría ser una experiencia discordante e inesperada, si el usuario no ha desempeñado durante dos semanas, mientras que un salto de una hora sería más comunes y garantizar un rápido retorno de juego.

Para obtener más información sobre el modelo de aplicación de Xbox One, consulte los siguientes recursos:

-   [Cambio de aplicación moderna para la sala de estar](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest%202012/Xfest%202012%20-%20Modern%20Application%20Switching%20for%20the%20Living%20Room.pptx), una presentación en Xfest 2012
-   [La experiencia de Shell](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=PROD-D_Experience.pptx&folder=platform/xfest2013), una presentación en Xfest 2013
-   [Proceso de administración de la duración para Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), notas del producto en GDN

> [!NOTE]
> Algunas presentaciones en [Xfest 2012](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2012.aspx) y [Xfest 2013](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2013.aspx) contener información obsoleta debido al anuncio que Xbox One admite la reproducción sin conexión.


### <a name="storage-options-on-xbox-one"></a>Opciones de almacenamiento en Xbox One

Xbox One proporciona varias opciones de almacenamiento, cada uno con sus propias ventajas y las restricciones. Las aplicaciones que deba usar una combinación de opciones en función de los requisitos de las aplicaciones.


<a name="connected-storage"></a>Almacenamiento conectado
-----------------
Almacenamiento conectado está diseñado para ayudar a las aplicaciones a guardar los datos de juego Xbox One y otro que se debe trasladar entre las consolas de aplicación correspondiente Estados-data. La API de almacenamiento conectado, específicas de Xbox One, ayuda a guardar y cargar esos datos. La API funciona en combinación con el modelo de aplicación de Xbox One.

La API de almacenamiento conectado proporciona las siguientes características:

-   Las aplicaciones pueden guardar rápidamente hasta 16 MB de datos a la vez en un búfer de memoria en la partición del sistema, que se almacenan en caché localmente en el disco duro por el sistema y cargado en la nube.
- Para socios administrados y ID@Xbox a los desarrolladores:
  - 256 MB por usuario/aplicación de almacenamiento en la nube.
- Para los desarrolladores del programa de creadores de Xbox Live:
  - 64 MB por usuario/aplicación de almacenamiento en la nube.
-   Respuesta sólida a cortes de alimentación, las aplicaciones no tienen que tratar con datos parciales que se va a guardar.
-   Automáticamente se cargan datos en la nube, incluso cuando no se está ejecutando la aplicación.
-   Datos están disponibles a través de las consolas Xbox One que están conectadas a Xbox Live.
-   Xbox Live controla la administración de sincronización y el conflicto entre dispositivos sin necesidad de intervención por parte de la aplicación.

Para obtener más información acerca de la API de almacenamiento conectado, consulte la sección correspondiente en xblesdk.chm (que documenta las API de SDK de Xbox Live extensión) en el SDK de Xbox Live.


<a name="xbox-live-title-storage"></a>Almacenamiento de información de Xbox Live título
-----------------------
El servicio de almacenamiento de título ofrece una API de REST multiplataforma para el almacenamiento de datos con las siguientes capacidades:

-   Proporciona datos de uso compartido entre varias plataformas, aplicaciones y usuarios
-   Admite binarios, JSON y archivos de configuración
-   Para socios administrados y ID@Xbox a los desarrolladores:
    - 256 MB por usuario/aplicación de almacenamiento en la nube
    - 256 MB de almacenamiento global título por
- Para los desarrolladores del programa de creadores de Xbox Live:
  -   64 MB por usuario/aplicación de almacenamiento en la nube
  -   256 MB de almacenamiento global título por

Requisitos para usar el servicio:

-   La consola Xbox One debe estar en línea con el fin de obtener acceso al servicio
-   Todas las interacciones de servicio deben realizarse mientras se ejecuta la aplicación; transferencia de datos no se completa automáticamente en segundo plano.

Para obtener más información, consulte *almacenamiento de Xbox Live título*, en la documentación de XDK.


<a name="local-temporary-storage"></a>Almacenamiento temporal local
-----------------------
En la consola, una aplicación tiene acceso al almacenamiento local temporal con las siguientes características:

-   2 GB de almacenamiento en disco duro dedicado, puede tener acceso a la ruta de acceso T:\\.
-   Contenido de este almacenamiento puede expulsarse cuando no se está ejecutando la aplicación.

Para obtener más información sobre el almacenamiento local, vea el almacenamiento Local, en la documentación de XDK.


<a name="configuring-your-app-for-connected-storage"></a>Configuración de la aplicación para el almacenamiento conectado
------------------------------------------
Cuando se usa la API de almacenamiento conectado, todos leer y escribir las operaciones están asociadas con una Xbox Live principal servicio configuración de Id. (¿SCID), definido en el archivo de manifiesto de la aplicación, AppXManifest.xml:

```xml
      <Extensions>
        <mx:Extension Category="xbox.live">
        <mx:XboxLive TitleId="<your title ID>" PrimaryServiceConfigId="<your SCID>"
        RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
```

Para obtener más información acerca de la adquisición en el título de Id. y ¿SCID para la aplicación, consulte *configuración de seguridad de espacios aislados para el desarrollo de Xbox Live*, en la documentación de XDK.



## <a name="connected-storage-system-concepts"></a>Almacenamiento conectados: conceptos del sistema

En esta sección se describe los componentes del sistema de almacenamiento conectados, sus relaciones y sus usos adecuados.

### <a name="connected-storage-space"></a>Espacio de almacenamiento conectado

En un nivel alto, todos los datos del sistema de almacenamiento conectado está asociado con un usuario o una máquina (por ejemplo, una consola Xbox One individual). Todos los datos guardados por una aplicación de una máquina o usuario concreto se almacena en un espacio de almacenamiento conectados.

Cada usuario de la aplicación obtiene un espacio de almacenamiento conectado con un límite de almacenamiento total de 256 MB. Es importante tener en cuenta que este almacenamiento está dedicado a la aplicación por sí solo, no se comparte con otras aplicaciones.

La aplicación también tiene 64 MB de espacio en un espacio de almacenamiento conectados local para la máquina. Este espacio de almacenamiento es independiente de los usuarios y puede tener acceso incluso si no hay ningún usuario haya iniciado sesión.

Para adquirir un espacio de almacenamiento conectado, las llamadas de la aplicación la *ConnectedStorageSpace.GetForUserAsync método* o *ConnectedStorageSpace.GetForMachineAsync método*. Esta una operación potencialmente larga, especialmente si el usuario con datos guardados en un dispositivo y reanuda el juego por primera vez en otro dispositivo. Para obtener más información acerca de este proceso y las posibles condiciones de error que se pueden producir mientras la aplicación está esperando adquirir un espacio de almacenamiento conectado, consulte *sincronizar un espacio de almacenamiento conectado*, más adelante en este documento.

Después de que la aplicación adquiere un **ConnectedStorageSpace** objeto, las llamadas a todos los métodos en el *Windows.Storage Namespace* qué usar ese objeto u otros objetos que derivan de ella, no dependen de una respuesta desde los servicios web en completarse. Sin embargo, dado que el acceso a la unidad de disco duro de uno de Xbox no es exclusivo de la aplicación activa, no se puede garantizar estrictos límites superiores en el rendimiento de estos métodos.


### <a name="connected-storage-containers-and-blobs"></a>Almacenamiento conectados: contenedores y blobs

El *contenedor de almacenamiento conectados*, o *contenedor* para abreviar, es la unidad básica de almacenamiento. Cada espacio de almacenamiento conectados puede contener varios contenedores, como se muestra en el diagrama siguiente.

**Figura 1.  Espacio de almacenamiento conectados (por título o equipo o por título o usuario)**

![](../../images/connected_storage/connected_storage_space_containers.png) Datos se almacenan en contenedores como uno o más búferes llamado *blobs*. El siguiente diagrama muestra la representación interna del sistema de contenedores en el disco. Para cada contenedor, hay un archivo contenedor que contiene referencias al archivo de datos para cada blob del contenedor.

**Figura 2.  Diagrama de un contenedor**

![](../../images/connected_storage/container_storage_blobs.png)

Para almacenar datos en un contenedor, llame a la *ConnectedStorageContainer.SubmitUpdatesAsync método*, que proporciona una asignación de nombres y los blobs (objetos de búfer). Todos los cambios que se describen en un **SubmitUpdatesAsync** llamada se aplican de forma atómica, es decir, ya sea todos los blobs se actualizan cuando se le solicite, o toda la operación se termina y el contenedor permanece en su estado anterior a la llamada.

Guardado de las operaciones que usan individuales **SubmitUpdatesAsync** están limitados a 16 MB de datos a la vez.


### <a name="submitupdatesasync-behavior"></a>Comportamiento de SubmitUpdatesAsync

Cuando **SubmitUpdatesAsync** es llamado, a se copian los búferes que se proporcionan a la llamada de la partición de la aplicación rápidamente en un espacio de memoria dedicada en la partición del sistema. Una vez que la memoria se ha copiado correctamente en la partición del sistema, el controlador de finalización proporcionada en el **SubmitUpdatesAsync** llamada se invoca dentro de la aplicación, que indica a la aplicación que es seguro liberar la memoria que asignó localmente para los datos.

El sistema, a continuación, guarda los blobs en la unidad de disco duro de la consola y completa la operación con una actualización de final del contenedor que confirma la operación completa en ese contenedor.

Hay un límite de 16 MB en la memoria en la partición para la recepción compartida **SubmitUpdatesAsync** datos. Si una llamada a **SubmitUpdatesAsync** no puede ser atendidas inmediatamente por el sistema porque no hay suficiente memoria libre en el búfer de 16 MB dedicado, la llamada se pone en cola para el servicio. El sistema continuamente transfiere datos desde el búfer de 16 MB en el disco duro y se atienden las actualizaciones en cola en el orden en que se solicitan como espacio disponible en el búfer de 16 MB.

**Figura 3.  Comportamiento de SubmitUpdatesAsync**

![](../../images/connected_storage/submitupdatesasync_behavior.png) Cargar en la nube se produce de forma similar: Los blobs individuales se cargan en el servicio y se confirma la operación de actualización mediante una actualización final en un archivo contenedor que hace referencia a todos los blobs cargados. En una carga a la nube, esta consolidación en una actualización única y última garantiza que todos los datos al que hace referencia en un **SubmitUpdatesAsync** llamada o se confirma en su totalidad o el contenedor se deja sin modificar. De esta manera, incluso si un sistema se queda sin conexión o apaga durante una operación de carga, un usuario podría ir a otra consola Xbox One, descargar datos desde la nube y continuar jugando con una vista coherente de todos los contenedores.

> [!IMPORTANT]
> Las dependencias de datos entre los contenedores no son seguras.  Los resultados de la persona *SubmitUpdatesAsync* se garantiza que las llamadas se aplique por completo o no en absoluto.

**SubmitUpdatesAsync** llamadas no deben suponer que un futuro **SubmitUpdatesAsync** llamada se completará correctamente con el fin de dejar el contenedor en un estado válido. En otras palabras, las aplicaciones no se basan en más de un **SubmitUpdatesAsync** llamada para guardar todos los datos necesarios en un contenedor. Cada **SubmitUpdatesAsync** llamada debe dejar el contenido del contenedor especificado en un estado válido para la aplicación leer más tarde.

Para ilustrar este problema, considere un escenario donde un contenedor realiza un seguimiento de la cantidad de oro y alimentos mantenidos por un carácter que se denomina Bob. El título puede almacenar dos blobs, denominados *alimentos* y *gold*. Bob comienza con 100 oro y no hay comida en su inventario.

**Figura 4.  Escenario de ejemplo: Bob comienza con 100 gold.**

![](../../images/connected_storage/submitupdatesasync_example_scenario1.png)

Ahora Bob pasa 50 gold. El título se prepara un **SubmitUpdatesAsync** llamar, que actualiza el valor del blob gold en 50.

El sistema captura el blob actualizado y la información sobre la actualización del contenedor en el búfer de actualizaciones. A continuación, el sistema copia el valor del nuevo blob en el disco duro.

**Figura 5.  El sistema captura la información actualizada y copia los valores en el disco duro.**

![](../../images/connected_storage/submitupdatesasync_example_scenario2.png)

Por último, el sistema actualiza el archivo de contenedor en el disco duro para hacer referencia al blob nuevo. Finalmente, el sistema elimina el blob sin referencia en una operación de la colección de elementos no utilizados.

**Figura 6.  El sistema actualiza el archivo de contenedor en el disco duro y elimina el blob sin referencia.**

![](../../images/connected_storage/submitupdatesasync_example_scenario3.png)

Tenga en cuenta que los blobs que más usan por **SubmitUpdatesAsync** llamada, más tiempo se necesita para completar las operaciones atómicas necesarias de las operaciones de sistema de archivos para almacenar los datos con eficacia. La granularidad del almacenamiento de datos en el ejemplo anterior es demasiado pequeña, pero está diseñado para ilustrar claramente el comportamiento de la actualización atómica de varios blobs en un contenedor.


### <a name="updating-multiple-blobs--the-wrong-way"></a>Actualizar varios blobs, la forma equivocada

Considere un escenario en el que Bob quiere comprar algunos alimentos. Por motivos de simplicidad, diremos que adquiere de 1 unidad de oro 1 unidad de alimentos y Bob quiere comprar 25 unidades de comida. La aplicación podría emitir uno **SubmitUpdatesAsync** llamada para agregar 25 unidades de comida y, a continuación, otra para restar 25 unidades de oro de BLOB\_contenedor de inventario. Sin embargo, incluso si los controladores completados para ambos **SubmitUpdatesAsync** se ha llamado a las llamadas, es posible de resultados incorrectos debido a eventos como la pérdida de energía, lo que podría dejar de los datos desde que se escriben en el disco duro, o un sincronización incompleta a la nube. Los diagramas siguientes explican los pasos realizados por el sistema y el resultado de una pérdida de energía en cualquiera de los pasos.

Se asume que los datos de ambos **SubmitUpdatesAsync** llamadas ya está en el búfer de actualización del sistema y controladores de finalización del título para ambas llamadas se han invocado.

El sistema escribe primero los datos para el nuevo valor del objeto binario de comida en el disco.

**Figura 7.  El sistema escribe el valor del objeto binario de comida en el disco.**

![](../../images/connected_storage/update_method_wrong_way_1.png) A continuación, el sistema actualiza el contenedor para hacer referencia al valor recién escrito. Como se muestra en el diagrama siguiente, si se perdieron power después de este paso y antes del siguiente, la Bob Baton Rouge acabaría con una gran cantidad ganando 25 comida sin tener el oro correspondiente deduce de su inventario.

**Figura 8.  El sistema actualiza el contenedor para hacer referencia al valor recién escrito.**

![](../../images/connected_storage/update_method_wrong_way_2.png)

A continuación, el sistema escribe los datos para el nuevo valor del blob gold en el disco. El valor de oro al que hace referencia el Bob\_contenedor inventario todavía no se ha actualizado y Bob tiene 25 gold más de lo que debería, pero estamos un paso más cerca al resultado deseado.

**Figura 9.  El sistema escribe los datos para el nuevo valor del blob gold en el disco.**

![](../../images/connected_storage/update_method_wrong_way_3.png)

Por último, el sistema actualiza el archivo de contenedor para hacer referencia a los blobs recién escritos para gold, el resultado deseado.

**Figura 10.  El sistema actualiza el archivo de contenedor para hacer referencia al blob gold recién escrito.**

![](../../images/connected_storage/update_method_wrong_way_4.png)

### <a name="updating-multiple-blobs--the-right-way"></a>Actualizar varios blobs, la manera correcta

Asegúrese de forma atómica se actualiza la cantidad de comida en el inventario de Bob y de oro, sin posibilidad de estado intermedio incorrecto debido a la pérdida de energía, la manera adecuada consiste en actualizar ambos blobs en una sola **SubmitUpdatesAsync** llamar. El sistema, a continuación, llevará a cabo los pasos siguientes.

El sistema escribe primero los datos para el nuevo valor del objeto binario de comida en el disco.

**Figura 11.  El sistema escribe los datos para el nuevo valor del objeto binario de alimentos.**

![](../../images/connected_storage/update_method_right_way_1.png) A continuación, el sistema escribe los datos para el nuevo valor del blob gold en el disco.

**Figura 12.  El sistema escribe los datos para el nuevo valor del blob gold.**

![](../../images/connected_storage/update_method_right_way_2.png) Por último, el sistema actualiza el archivo de contenedor para hacer referencia tanto de los nuevos blobs.

**Figura 13.  El sistema actualiza el archivo de contenedor para hacer referencia a los nuevos blobs.**

![](../../images/connected_storage/update_method_right_way_3.png) Aunque en este ejemplo es muy simple, ilustra la importancia de realizar todas las modificaciones a los datos en un contenedor que se debe aplicar de forma atómica mediante la emisión de una sola **SubmitUpdatesAsync** llamar con todas las actualizaciones deseadas. De esta forma para el caso de la compra de comida con gold, la aplicación evita una condición de carrera potenciales que podría actualizar solo uno de los valores y deje el carácter con demasiada gold incorrectamente.

### <a name="performance-characteristics-and-considerations"></a>Consideraciones y las características de rendimiento

El búfer de la actualización de 16 MB en la partición compartido permite que un número limitado de operaciones de actualización que se realizará muy rápidamente. La velocidad a la que el sistema puede conservar los datos en el disco depende de la cantidad de datos en el búfer y el número de blobs. Dado que cada blob se escribe en el disco con la resistencia, cuanto mayor sea el número de blobs en el búfer, más tiempo tarda en hacer que persistan en el disco.

Figura 13 se muestra un ejemplo para el tiempo de procesamiento para un **SubmitUpdatesAsync** operación cada 2 segundos con dos 512 KB actualizaciones de blob y otro 1024 KB blob update cuando no hay ninguna otra actividad de la unidad de disco duro en el sistema. El sistema puede funcionar en un estado estable, procesamiento de cada actualización dentro de 14: 18ms.

**Figura 14.  Tiempo de procesamiento de una operación SubmitUpdatesAsync cada 2 segundos con actualizaciones de blob dos 512 KB y una actualización y ninguna otra actividad de la unidad de disco duro de blob de 1024 KB.**

![](../../images/connected_storage/submitupdatesasync_proc_time_mixed_size_fixed_interval.png) Figura 14 se muestra el tiempo de procesamiento para tres blobs 1024 KB distintos intervalos de tiempo.

El sistema puede procesar estas actualizaciones a intervalos de 3 segundos en estado estable de 87ms. Al aumentar la frecuencia de una vez cada 2 segundos, el sistema pueda procesar las actualizaciones de estado estable de 87ms.

Reducir el intervalo de 1 segundo entre las actualizaciones, modifica el comportamiento de estado estable. El sistema puede procesar 60 actualizaciones en 87ms cada actualización, pero todas las actualizaciones más allá tardan mucho, alcanzar un tiempo de procesamiento de estado estable de 500 milisegundos por segundo / actualización con la fluctuación significativo. Esto es porque se llena el búfer de 16 MB de memoria más rápido de lo que puede vaciar los datos en el disco; las actualizaciones se ven obligadas a esperar para que las actualizaciones anteriores se escriben.

El efecto aumenta significativamente al actualizar el intervalo a una actualización cada 0,5 segundos. El sistema puede procesar solo 7 actualizaciones en este intervalo, de nuevo a 87ms cada actualización, antes de llegar a un estado estable en el que cada actualización tiene más de 1 segundo en procesarse con variaciones muy altas.

**Figura 15.  Tiempo de procesamiento de los tres blobs 1024 KB distintos intervalos de tiempo.**

![](../../images/connected_storage/submitupdatesasync_proc_time_fixed_size_various_intervals.png) Estos son solo ejemplos ilustrativos. La aplicación por lo general no debería guardar datos esto a menudo, pero que también no suele estar funcionando en un entorno libre de E/S de disco.

Es importante comprender las características del sistema basándose en estos ejemplos medir la aplicación en diversas condiciones de funcionamiento, asegurándose de que la guardó operaciones pueden completarse en menos de 1 segundo durante la aplicación de la suspensión controlador.


## <a name="synchronizing-a-connected-storage-space"></a>Sincronización de un espacio de almacenamiento conectado

-   Comprobación de conectividad
-   Adquisición de bloqueo
-   Lógica de anuncio, comparación y fusión de contenedor
-   Descarga de contenedor

Cuando la aplicación solicita acceso a un espacio de almacenamiento conectado, el sistema realiza un proceso de sincronización para mantener los datos del usuario guardada en un estado coherente a través de las consolas Xbox One y para que sus datos estén disponibles para la reproducción sin conexión. Dado que la sincronización puede demorar una cantidad de tiempo y puede requerir al usuario tomar decisiones, el sistema podría mostrar la interfaz de usuario para el usuario en distintas fases del proceso.

El usuario puede navegar lejos de la aplicación presionando el botón de Xbox en cualquier momento, incluso si la sincronización de la interfaz de usuario está activo. El sistema oculta la interfaz de usuario y la sincronización continuará en cuanto sea posible sin interacción del usuario. Cuando el usuario navega a la aplicación, la interfaz de usuario aparecerá de nuevo a menos que se ha completado la sincronización. El sistema nunca realiza una suposición acerca de la selección de un usuario cuando se oculta la interfaz de usuario.

Dado que el sistema no muestra ninguna interfaz de usuario de la sincronización cuando el usuario está en la pantalla de inicio y la representación de la aplicación sigue estando visible en el icono de aplicación grande, es importante que la aplicación representar objetos visuales contextualmente adecuados mientras un **GetForUserAsync** se completa la llamada. El procesamiento continuado indica al usuario que la aplicación sigue siendo interactiva y está esperando los datos que se cargará.

El siguiente diagrama describe la secuencia de alto nivel que el sistema sigue cuando una aplicación solicita un espacio de almacenamiento conectados. Si la secuencia completa tarda más de unos segundos, se muestra la interfaz de usuario de sincronización dibujado por el sistema.

**Figura 16.  Secuencia seguido por el sistema cuando una aplicación solicita el espacio de almacenamiento conectados.**

![](../../images/connected_storage/app_requests_connected_storage_space.png) El sistema pasa por las fases siguientes cuando se atiende un **GetForUserAsync** solicitud:

-   Comprobación de conectividad
-   Adquisición de bloqueo
-   Lógica de anuncio, comparación y fusión de contenedor
-   Descarga de contenedor

### <a name="connectivity-check"></a>Comprobación de conectividad

Para empezar a atender un **GetForUserAsync** solicitud, el sistema comprueba la conectividad. Si la consola está sin conexión, se omite el proceso de sincronización completa y el espacio de almacenamiento conectadas para el usuario especificado está marcado como sin conexión para la sesión actual. Los datos modificados se concilia con almacenamiento en la nube la próxima vez que la aplicación accede a espacio de almacenamiento conectados del mismo usuario y el sistema puede alcanzar el servicio de almacenamiento de título. Nunca se muestra ninguna interfaz de usuario para este caso.

Para obtener más información acerca del control sin conexión fuera del contexto de almacenamiento conectados, consulte *servicio interrupción resistencia para títulos de una Xbox*.

### <a name="lock-acquisition"></a>Adquisición de bloqueo

Después de comprobar la conectividad, el sistema intenta adquirir acceso exclusivo al espacio de almacenamiento en la nube asociado con la aplicación y el usuario actual. Esto se consigue mediante la colocación de un archivo de bloqueo en el área de almacenamiento conectados de su almacenamiento de información de título. Si la consola está en línea, puede alcanzar el servicio y es capaz de adquirir el bloqueo en un breve período de tiempo, no se muestra ninguna interfaz de usuario y continúa el proceso de sincronización.

Una vez que el sistema ha adquirido un bloqueo para un espacio de almacenamiento conectado determinado y devuelve una instancia de un espacio de almacenamiento conectado a la aplicación, ninguna de las API de la aplicación llama a operar en datos en que el espacio de almacenamiento conectados se bloqueará en las solicitudes web correcta. El bloqueo proporciona protección suficiente, por lo que incluso si un usuario tuviera que desconecte el cable de red desde el sistema después de la aplicación ha adquirido un espacio de almacenamiento conectado, las llamadas API funcionarán en función de los datos disponibles localmente.

Hay algunos escenarios posibles errores durante el paso de adquisición de bloqueo:

 **Sincronización de la interfaz de usuario** si la consola está en línea, pero no ha adquirido el bloqueo del servicio en un tiempo muy corto, se muestra una interfaz de usuario "sincronización".

 **Interrumpir el bloqueo** si el usuario ha desempeñado la aplicación en otra consola ya que quien desempeñado por última vez en la actual, es posible que la otra consola tiene acceso exclusivo al espacio de almacenamiento y está en medio de carga de datos. También es posible que otra consola de carga de datos ha iniciado pero ha perdido su conexión o la energía antes de finalizar.

Ambos casos se conocen como *contención de bloqueo*, y en cualquier caso, el sistema presenta la interfaz de usuario para explicar que otra consola está cargando datos. El usuario puede esperar a este proceso completar o trabajar con los datos disponibles actualmente en la nube. Si el usuario elige trabajar con datos en la nube, el sistema toma el bloqueo sí mismo (interrumpe el bloqueo), al adquirir acceso exclusivo para el almacenamiento en nube para el usuario y la aplicación. Se ha cancelado la carga desde la consola de otra y continúa el proceso de sincronización.

### <a name="container-listing-comparison-and-merger-logic"></a>Lógica de anuncio, comparación y fusión de contenedor

Después de adquirir un bloqueo, el sistema solicita una lista de todos los contenedores en la nube para la aplicación determinada y el usuario. A continuación, compara el contenido de la unidad de disco duro local con los datos en la nube y continúa según los resultados de la comparación:

 **Datos locales coincide con la nube** si no ha habido ningún cambio desde otros consolas y los datos en la nube y locales duros unidad es idéntica, a continuación, la sincronización es completar, el controlador de finalización de la **GetForUserAsync**llamada se invoca en este momento, y la aplicación puede continuar con la carga y guarda.

 **Ningún dato local** si la nube tiene datos, pero la consola local no tiene ninguna, se descargan los datos desde la nube localmente. Esto puede ocurrir, por ejemplo, cuando se reproduce en casa de un amigo por primera vez.

 **Mismos contenedores, modificados localmente y en la nube** si el usuario modificó los contenedores en la nube mediante la reproducción en otra consola y ha modificado los mismos contenedores cuando se usa la consola actual sin conexión, los datos no se pueden combinar automáticamente. Se pedirá al usuario elegir qué datos se deben conservar. En el caso de conflictos, el usuario puede elegir una directiva de reemplazo: Siempre se mantienen los datos locales o los datos en la nube o el usuario puede seleccionar **cancelar** y aplazar la elección. Si el usuario elige la nube o locales datos como una directiva de reemplazo, los contenedores con el mismo nombre, pero con distinto contenido: se resolverá en consecuencia.

Si el usuario selecciona **cancelar**, el título tendrán acceso a la operación de guardar el sistema en un estado no han resuelto, como si el usuario estaba reproduciendo sin conexión. En este caso, la interfaz de usuario de la resolución de conflictos se presenta la próxima vez que la aplicación solicita acceso al espacio de almacenamiento conectado, si la consola está en línea.

### <a name="container-download"></a>Descarga de contenedor

Después de que se han resuelto los conflictos, el sistema tiene toda la información necesaria identificar qué contenedores deben descargarse desde la nube. Todos los contenedores necesarios, se descargarán el controlador de finalización de la *ConnectedStorageSpace.GetForUserAsync método* se invocará en este momento, y la aplicación puede continuar con la carga y guarda.

Algunos posibles errores durante este paso:

**No hay suficiente almacenamiento local**  
En el caso de espacio de unidad de disco duro local insuficiente para los contenedores necesarios, se presentan a los usuarios con la interfaz de usuario le pide que liberar espacio en disco quitando los datos guardados localmente. Para ayudarles a evitar la eliminación permanente de los datos importantes que no es una copia de seguridad en la nube, la interfaz de usuario indica claramente los datos que es la memoria caché local simplemente y que es único en la consola actual.

Cuando se presenta la interfaz de usuario para el usuario:

-   Si el usuario libera suficiente espacio, la sincronización continúa y se completa.
-   Si el usuario cancela la interfaz de usuario sin liberar espacio suficiente, el controlador de finalización de la **GetForUserAsync** llamada devuelve **OutOfLocalStorage**: vea *ConnectedStorageErrorStatus Enumeración*. La aplicación debe confirmar que el usuario intenta reproducir sin poder guardar los datos. Si el usuario acepta, la aplicación debe continuar sin guardar los datos para ese usuario. Si el usuario indica que desea guardar los datos mientras se está reproduciendo, la aplicación debe repetir el **GetForUserAsync** llamar, a continuación, se mostrará la interfaz de usuario para liberar espacio.

**Usuario cancela la sincronización**  
Si el usuario no desea esperar a que complete la sincronización y selecciona cancela, se informa al usuario que no todos los datos guardados estarán disponibles. El controlador de finalización de la **GetForUserAsync** llamada se invocará en este momento, y la aplicación puede continuar con la carga y guarda.

**Tiempo de espera de red**  
Si se agota el tiempo debido a un problema con la conectividad de red o la disponibilidad del servicio de descarga de los datos, el usuario tiene la opción para volver a intentar la sincronización. Si elige no, se informa al usuario que no todos los datos guardados estarán disponibles. El controlador de finalización de la **GetForUserAsync** llamada se invocará en este momento, y la aplicación puede continuar con la carga y guarda.

## <a name="development-tools"></a>Herramientas de desarrollo

Dos herramientas le ayudarán con el uso de la aplicación de almacenamiento conectados de desarrollo: XbStorage y Fiddler.

### <a name="managing-connected-storage-with-xbstorage"></a>Administración del almacenamiento conectado con XbStorage

XbStorage es una herramienta de desarrollo que permite administrar los datos de almacenamiento conectados local en un kit de desarrollo de Xbox One desde un equipo de desarrollo.

La herramienta permite borrar los espacios de almacenamiento conectados local desde el disco duro, así como importar y exportar espacios de almacenamiento individuales conectados del usuario o del equipo mediante el uso de archivos XML.

Cuando se realiza una operación en un espacio de almacenamiento conectado local, el sistema se comporta como si esa operación se realizó por la propia aplicación. Copiar los datos de un espacio de almacenamiento conectado a un archivo local, hace que la sincronización con la nube antes de copiar.

De forma similar, copiar datos desde un archivo XML en el equipo de desarrollo a un contenedor de almacenamiento conectados en el kit de desarrollo de Xbox One hace que la consola para iniciar la carga de datos a la nube. Hay una excepción: si el kit de desarrollo no puede adquirir el bloqueo, o si hay un conflicto entre los contenedores de la consola y los de la nube. En tal caso, la consola se comporta como si el usuario había decidido no resuelva el conflicto: por ejemplo, si elige una versión del contenedor para mantener- y la consola se comporta como si se reproduce sin conexión hasta la próxima vez que se inicia el título.

Comando de restablecimiento del XbStorage borra el almacenamiento local de los datos guardados todas 'SCIDs y los usuarios, pero no modifica los datos almacenados en la nube. Esto es útil para establecer una consola en el estado sería en si un usuario se itinerancia a una consola y descarga de datos de la nube al reproducir un título.

Para obtener más información sobre XbStorage, consulte *administrar el almacenamiento conectado (xbstorage.exe)*, en la documentación de XDK.

### <a name="monitoring-connected-storage-network-activity-using-fiddler"></a>Supervisión de la actividad de red de almacenamiento conectados mediante Fiddler

Puede ser útil determinar si la consola está interactuando con el servicio cuando se realizan operaciones de almacenamiento en la nube. Fiddler puede ayudar a determinar si la consola está realizando llamadas al servicio correctamente o si están produciendo errores de autorización. Para obtener información acerca de cómo configurar Fiddler en una Xbox One, consulte *cómo usar Fiddler con Xbox One*, en esta documentación XDK.

## <a name="resources"></a>Recursos

Además de los recursos sugeridos por encima, pueden serle de ayuda en el desarrollo de la aplicación o el título:

-   Introducción a almacenamiento conectado, en la documentación de XDK
-   [Procesos como la administración de vigencia](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=ProcessLifetimeManagement_08_2013_qfe5.zip&folder=platform/aug2013xdk_qfe5/samples), un ejemplo disponible en los ejemplos en Game Developer Network (GDN)
-   ["Proceso de administración de vigencia (PLM) para Xbox One"](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), notas del producto disponible en las notas del producto en GDN

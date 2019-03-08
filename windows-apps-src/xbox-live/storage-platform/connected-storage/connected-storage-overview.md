---
title: Introducción al almacenamiento conectado
description: Obtenga información sobre el uso de almacenamiento conectado para guardar y cargar los datos del juego en todos los dispositivos.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 40ad13e46e074154d72d7aad236747c3374110ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639060"
---
# <a name="connected-storage"></a>Almacenamiento conectado
Almacenamiento conectado está diseñado para permitir que el título guardar los datos de juegos y otros datos de estado relevantes que se deben trasladar entre dispositivos. La API de almacenamiento conectado permite títulos en Xbox One y Platform(UWP) Universal de Windows para guardar, cargar y eliminar datos del título que se almacena localmente y también se sincronizan en la nube siempre que el título de Xbox One o UWP está conectado a internet. Guardado datos estarán disponibles en cualquier otro dispositivo que se ejecuta el título de una vez que se produce la sincronización. Los desarrolladores pueden realizar para guardar el estado del título de forma más precisa posible para ofrecer que la mejor lejos de casa reproducir experiencia. Almacenamiento conectado es lo que le permite progresar en un juego en casa, y luego retomar desde su juego donde lo dejó en cualquier otro dispositivo que admite el juego mismo.

## <a name="connected-storage-features"></a>Características de almacenamiento conectado

La API de almacenamiento conectado proporciona las siguientes características:

- Las aplicaciones pueden guardar rápidamente hasta 16 MB de datos a la vez en un búfer de memoria, que se almacenan en caché localmente en el disco duro por el sistema y cargado en la nube.
- Para socios administrados y ID@Xbox a los desarrolladores:
    - 256 MB por usuario/aplicación de almacenamiento en la nube.
- Para los desarrolladores del programa de creadores de Xbox Live:
    - 64 MB por usuario/aplicación de almacenamiento en la nube.
- Respuesta sólida a cortes de alimentación, las aplicaciones no tienen que tratar con datos parciales que se va a guardar.
- Automáticamente se cargan datos en la nube, incluso cuando no se está ejecutando la aplicación.
- Datos están disponibles en todos los dispositivos de Xbox One o UWP que están conectados a Xbox Live.
- Xbox Live controla la administración de sincronización y el conflicto entre dispositivos sin necesidad de intervención por parte de la aplicación.

El sistema de almacenamiento conectado se asegura de que se realiza todos los guarda en su totalidad o no admitirlo. Esto significa que como desarrollador nunca tendrá que preocuparse de datos parcialmente guardados que afecte a su estado del juego en el caso de error de alimentación o el usuario cierra la aplicación de repente, ya sea manualmente o abriendo otra aplicación o juego en la consola. Esto también garantiza un juego más suave reproducir experiencia para los usuarios de su título, como almacenamiento conectado es una parte importante de lo que permite a los usuarios cambiar entre los juegos y aplicaciones rápidamente y libremente, pero todavía se vuelve al juego original en el estado en el que dejó. Con el fin de implementar estas características en su propio título deberá estar familiarizado con las API de almacenamiento conectado.

## <a name="connected-storage-structure"></a>Estructura de almacenamiento conectado

El sistema de almacenamiento conectado permite que las aplicaciones almacenar datos como uno o más blobs en contenedores. En un nivel alto, todos los datos del sistema de almacenamiento conectado está asociado con un usuario o un usuario o equipo en el caso del Kit de desarrollo de Xbox desarrollado títulos. Guardan todos los datos por una aplicación para un usuario determinado o equipo se almacena en un espacio de almacenamiento conectado. Cada usuario de la aplicación obtiene un espacio de almacenamiento conectado con un límite de 64 o 256 MB de almacenamiento total. Es importante tener en cuenta que este almacenamiento está dedicada a la aplicación por sí solo, no se comparte con otras aplicaciones.

### <a name="containers-and-blobs"></a>Contenedores y blobs

El contenedor de almacenamiento conectado, o un contenedor para abreviar, es la unidad básica de almacenamiento. Cada espacio de almacenamiento conectado puede contener varios contenedores, como se muestra en el diagrama siguiente.

Espacio de almacenamiento conectado (por título o equipo o por título o usuario)

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 Datos se almacenan en contenedores como uno o varios búferes llama a los blobs. El siguiente diagrama muestra la representación interna del sistema de contenedores en el disco. Para cada contenedor, hay un archivo contenedor que contiene referencias al archivo de datos para cada blob del contenedor.

Diagrama de un contenedor

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

Para almacenar datos en un contenedor, llame al método apropiado de contenedor de API SubmitUpdatesAsync, que proporciona una asignación de nombres y los blobs (objetos de búfer). Todos los cambios que se describe en una llamada SubmitUpdatesAsync se aplican de forma atómica, es decir, ya sea todos los blobs se actualizan cuando se le solicite, o toda la operación se termina y el contenedor permanece en su estado anterior a la llamada.

Individuo guardar las operaciones que usan SubmitUpdatesAsync están limitados a 16 MB de datos a la vez.

## <a name="connected-storage-api"></a>API de almacenamiento conectadas

Almacenamiento conectado tiene API independientes para las aplicaciones XDK y UWP. Afortunadamente, estas API son similares a entre sí muy de cerca. Las dos API difieren principalmente en sus nombres de clase y espacio de nombres. Las funciones necesarias para [guardar](connected-storage-saving.md), [cargar](connected-storage-loading.md), y [eliminar](connected-storage-deleting.md) datos con la API tienen el mismo nombre.

Diferencias adicionales entre las dos API de almacenamiento conectado se detallan en la sección de almacenamiento conectados de [trasladar Xbox Live código desde el XDK a UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

Puede encontrar que las API de almacenamiento conectado XDK documentados en el archivo .chm XDK bajo la ruta de acceso: **Xbox uno XDK >> referencia de API >> referencia de API de plataforma >> referencia de la API del sistema >> Windows.Xbox.Storage**.
Las API XDK también se documentan en el [developer.microsoft.com sitio](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
El vínculo a las API XDK requiere que tenga un Account(MSA) Microsoft que se ha habilitado para el acceso de Kit(XDK) para desarrolladores de Xbox.
Windows.Xbox.Storage es el nombre del espacio de nombres de almacenamiento conectado para las consolas Xbox One.

Puede encontrar las API de UWP conectado almacenamiento documentados en el archivo .chm de Xbox Live SDK en la ruta de acceso: **Las API de Xbox Live >> Xbox Live referencia de API del SDK de extensiones de plataforma >> Windows.Gaming.XboxLive.Storage**.
Las API de almacenamiento conectados de UWP también están documentadas en línea en el [Windows.Gaming.XboxLive.Storage referencia de espacio de nombres](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Windows.Gaming.XboxLive.Storage es el nombre del espacio de nombres de almacenamiento conectado para aplicaciones UWP.

Para empezar a usar almacenamiento conectado, deberá adquirir un almacenamiento conectado *espacio*. Un espacio de almacenamiento conectado está asociado con un usuario o equipo y contiene todos los datos de almacenamiento conectado asociados con ese usuario o equipo en forma de *contenedores* y *blobs*. Adquisición de un espacio de almacenamiento conectado para una máquina o el usuario tendrá acceso para leer y escribir a esos datos de las entidades almacenadas. Para adquirir un espacio de almacenamiento conectado, títulos XDK y UWP llamará el `GetForUserAsync` títulos XDK de método, también pueden llamar el `GetForMachineAsync` método, los títulos UWP no se pueden llamar a `GetForMachineAsync`. `GetForUserAsync` y `GetForMachineAsync` se encuentran en el `ConnectedStorageSpace` clase en el XDK. `GetForUserAsync` se encuentra en la `GameSaveProvider` clase de la API de UWP. Estos métodos son operaciones potencialmente de larga ejecución, especialmente si el usuario con datos guardados en un dispositivo y reanuda el juego por primera vez en otro dispositivo. `GetForUserAsync` Recupera un espacio de almacenamiento conectado para un usuario que, a continuación, puede usar para crear, acceder y elimine los contenedores.

Para crear un contenedor o acceder a un contenedor creado anteriormente, llame a la `CreateContainer` función de su `ConnectedStorageSpace` o `GameSaveProvider` (clase), esto le dará acceso a un contenedor con nombre para el usuario o equipo asociado con el `ConnectedStorageSpace` o `GameSaveProvider`usado para crear el contenedor. El `ConnectedStorageSpace` y `GameSaveProvider` clases también incluyen el `DeleteContainerAsync` proporciona una función que le permite eliminar un contenedor que proporcionar el nombre del contenedor que se va a eliminar.

Para actualizar los blobs en la llamada de contenedor `SubmitUpdatesAsync` en el `ConnectedStorageContainer` clase en el XDK o en el `GameSaveContainer` clase de la API de UWP. `SubmitUpdatesAsync` permite proporcionar una lista de nombres y los búferes de datos se escriban en el blob, una lista de nombres de blobs que se van a eliminarse y un nombre para mostrar para la llamada a guardan contenedor. Se trata de la función que en última instancia, necesitará llamar al actualizar los datos en el espacio de almacenamiento conectado.

Para ver ejemplos de las API de almacenamiento conectado en uso, consulte los siguientes artículos de almacenamiento conectado: [Guardar datos](connected-storage-saving.md)
[cargar datos](connected-storage-loading.md)
[eliminar datos](connected-storage-deleting.md)

> [!NOTE]
> Una nota sobre seguridad:
>
> Aplicaciones universales de Windows Platform (UWP) que se ejecutan en equipos con guardar datos locales en una ubicación que sea accesible para el usuario local y no es intrínsecamente protegida contra la manipulación del usuario.
>Puede aplicar su propio cifrado y comprueba la validez al juego guardado de los datos con el fin de ayudar a evitar que los usuarios modifiquen los juegos guardados fuera el juego.
>Por esta misma razón debe decidir si desea permitir que las versiones de PC y Xbox de tu juego a compartir guarda o mantenerlos separados.

## <a name="managing-local-storage"></a>Administración del almacenamiento local

Al probar el almacenamiento conectado en el Kit de desarrollo de Xbox o UWP la aplicación, deberá manipular los datos almacenados localmente en el dispositivo de desarrollo.

La herramienta xbstorage se suministra con el XDK y es una herramienta de línea de comandos que permite manipular el almacenamiento local en la consola de desarrollo.
Para los desarrolladores de UWP hay una herramienta idéntica para PC denominados gamesaveutil.exe que se suministra con el SDK de Windows 10 Fall Creators Update(10.0.16299.91) y versiones posteriores.

Ambas herramientas le permiten manipular el almacenamiento local en el dispositivo con estos comandos:

|Comando  |Descripción  |
|---------|---------|
|Restablecer    | Realiza una restablecimiento de fábrica en almacenamiento conectado. |
|importar   | Importa datos desde el archivo XML especificado en un espacio de almacenamiento conectado. |
|Exportar   | Exporta datos de un espacio de almacenamiento conectado en el archivo XML especificado. |
|Eliminar   | Elimina los datos de un espacio de almacenamiento conectado. |
|generar | Genera datos ficticios y guarda en el archivo XML especificado. |
|Simular | Simula las condiciones de espacio de almacenamiento insuficiente. |

Para obtener más información sobre las funciones disponibles en la herramienta de xbstorage y gamesaveutils.exe leer [administración de almacenamiento local conectado](connected-storage-xb-storage.md).

## <a name="technical-overview"></a>Introducción técnica

Para aprovechar más en profundidad de la visión de cómo funciona el almacenamiento conectado en el XDK leer el [Introducción técnica a almacenamiento conectado](connected-storage-technical-overview.md). La información general técnica se escribió específicamente para los desarrolladores XDK pero los conceptos incluidos en se aplican al almacenamiento conectado en general.
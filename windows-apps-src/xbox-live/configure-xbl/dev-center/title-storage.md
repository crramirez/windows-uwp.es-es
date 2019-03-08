---
title: Título de la configuración de almacenamiento en el centro de partners
description: Obtenga información sobre cómo configurar el almacenamiento de título en el centro de partners
ms.date: 04/24/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, juegos, uwp, windows 10, Xbox uno, el almacenamiento de título, el centro de partners
ms.openlocfilehash: d32f9f2f4e003db50ad560acc513511a850d43b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612100"
---
# <a name="configure-storage-for-you-title-in-partner-center"></a>Configurar el almacenamiento para el título, en el centro de partners

Xbox Live le permite guardar datos asociados con su juego en la nube a través del servicio de almacenamiento de título. La página de configuración de almacenamiento de título permite determinar qué tipos de servicios de almacenamiento en la nube su juego se permiten, así como cargar archivos que se usará para el almacenamiento Global.

Puede encontrar la página de configuración de almacenamiento de título de Xbox Live, vaya a [asociado](https://partner.microsoft.com/dashboard), elección de la aplicación desde **Introducción** o **productos**, abra el  **Servicios** colocar hacia abajo y seleccione **Xbox Live**. Los desarrolladores en el programa de creadores tendrán que haga clic en **Mostrar opciones** en el **guarda y almacenamiento en la nube** sección de su página de configuración para ver las opciones de configuración de almacenamiento de título. Con el conjunto completo de características de Xbox Live disponibles, debe encontrar el **título almacenamiento** vínculo para ir a la página de configuración de almacenamiento de título.

Configuración del almacenamiento de título tiene dos secciones principales. La sección de configuración de almacenamiento de título y la sección de administración de archivos de almacenamiento global.

## <a name="section-1-title-storage-settings"></a>Sección 1: Configuración de almacenamiento del título

![Captura de pantalla de configuración de almacenamiento de información de título](../../images/dev-center/title-storage/title-storage-settings.JPG)

En esta sección puede habilitar cualquiera de los cuatro tipos de almacenamiento de información activando la casilla la **Active** columna. Después de elegir el título que 's tipos de almacenamiento puede elegir si la lectura de datos se limitará al Reproductor que posee, haciendo clic en la fila de tipo de almacenamiento **propietario** sólo el botón de radio o compartir públicamente, haciendo clic en el **Todo el mundo** botón de radio. Si selecciona **propietario sólo** para un determinado **tipo de almacenamiento** , a continuación, los datos del almacenamiento de título de ese tipo sólo serán legibles por el jugador que genera esos datos. Si selecciona **todo el mundo** para un determinado **Storagetype** , a continuación, los datos del almacenamiento de título de ese tipo se podrán leer todos los jugadores. Escribir o modificar los datos guardados solo está disponible para el usuario que lo generó en todos los casos.

## <a name="storage-types"></a>Tipos de almacenamiento

Hay cuatro tipos de almacenamiento que se pueden activar en la página de configuración de almacenamiento de título. Puede encontrar una descripción de cada tipo de almacenamiento, mantenga el puntero sobre el icono de información situado junto al nombre de cada tipo de almacenamiento, o mediante la lectura de la tabla siguiente.

|Tipo de almacenamiento |Descripción |Ejemplo de uso  |
|---------|---------|---------|
|Global             |Datos cargados al centro de partners que se puede leer cualquier dispositivo y es accesible a todos los usuarios. Puede solo se escribe en las cargas de desarrollador al centro de partners. | Anunciar las actualizaciones a todos los usuarios a través de la fuente de noticias en el juego.     |
|Almacenamiento conectado  |Permite la sincronización en segundo plano de datos del juego en XboxOne y juegos para Windows 10. Un juego tolerante a errores sólido guardar el servicio. Puede leer cualquier dispositivo, se puede escribir en los dispositivos de Xbox One y Windows 10    | Guardar archivos para un usuario individual permitir que la reproducción en una consola independiente.         |
|Universal          |Almacenamiento de blob accesible que proporciona acceso de lectura/escritura a cualquier dispositivo que no es una consola Xbox 360 o Windows Phone de la red. Se puede leer por dispositivos IOS y Android.      | guardar la reproducción u otras estadísticas para ser accesible desde varios dispositivos de Windows.        |
|confianza            |Almacenamiento de blobs puede tener acceso de red que solo puede escribirse mediante Xbox One, Xbox 360 y Windows Phone. Puede leer cualquier dispositivo. Se puede leer por IOS y Android.     | almacene la clasificación de un jugador en participan varios jugadores.        |

## <a name="section-2-global-storage-file-management"></a>Sección 2: Administración de almacenamiento global

![captura de pantalla de administración de archivos de almacenamiento global](../../images/dev-center/title-storage/global-storage-file-management.JPG)

Para ver opciones de la administración de archivos de almacenamiento Global completa, debe hacer clic en el **Mostrar opciones** lista desplegable. En esta sección puede agregar archivos y carpetas que estarán accesibles si el **Global** tipo de almacenamiento se establece como activo en la configuración de almacenamiento de título. El juego debe publicarse para realizar pruebas con el fin de agregar archivos en esta sección. Puede ver una advertencia en la parte superior de la página de configuración de almacenamiento de título si su juego no está publicado adecuadamente para las pruebas.

## <a name="manage-global-storage-files"></a>Administrar archivos de almacenamiento Global

Administración de archivos de almacenamiento global permite cargar y descargar archivos que se usará para el almacenamiento global. Estos archivos pueden tener el acceso a cualquier persona que posee el título y están diseñados para compartirse entre todos los jugadores que jugar a un juego. Para ver el archivo de almacenamiento global opciones que debe hacer clic en el **Mostrar opciones** lista desplegable situada junto al título de secciones. Para agregar el primer archivo, haga clic en el **+ nuevo...**  vínculo. A continuación, le dará la opción de agregar un nuevo archivo o carpeta a los archivos de almacenamiento global.

### <a name="new-folders"></a>Nuevas carpetas

Al agregar una nueva carpeta en el almacenamiento global para los archivos basta con nombre de la carpeta y haga clic en el **crear** botón. La nueva carpeta aparecerá en la tabla del explorador de archivos.

![Agregar cuadro de diálogo de carpeta](../../images/dev-center/title-storage/add-folder-global-storage-filled.JPG)

Con el fin de agregar archivos a la carpeta debe cargarlos en la carpeta directamente mediante la inserción de las carpetas **acciones** botón: "**...** "y seleccionando **cargar archivos**. No se puede arrastrar y colocar archivos en carpetas dentro de la tabla del explorador de archivos. Mediante el **crear carpeta** acción en el **acciones** menú de una carpeta creará una carpeta secundaria. Elegir el **eliminar** acción en el **acciones** menú de una carpeta eliminará la carpeta y todo su contenido.

### <a name="new-files"></a>Nuevos archivos

Al agregar un nuevo archivo en el almacenamiento global que le pedirá que cargue un archivo desde el Explorador de archivos de su equipo y, a continuación, se le pida que elija uno de los tres tipos de archivo, binario, configuración y JSON. Además de poder cargar un archivo con el **+ nuevo...**  botón también puede arrastrar y colocar archivos de equipo a la tabla del explorador de archivos.

> [!WARNING]
> No se puede arrastrar las carpetas en la tabla del explorador de archivos, intentar esta acción dará como resultado en la carpeta que se tratan como un archivo y no funcionará según lo previsto.

Las acciones de administración de archivos: ![gif de administración de archivos](../../images/dev-center/title-storage/global-storage-management.gif)

#### <a name="file-types"></a>Tipos de archivo

* Binario - se debe usar el tipo binario de imágenes, sonidos y datos personalizados. Estos datos deben ser HTTP descriptivo.
* Configuración: los archivos de configuración contienen información acerca de su juego y pueden tener una consulta dinámica en función de alguna entrada de valores devueltos.
* JSON - .json files. Estos archivos contienen información sobre un aspecto de su juego, similar a un archivo de configuración.

## <a name="further-reading"></a>Obtener más información

Para obtener más información acerca de la plataforma de almacenamiento de información de Xbox Live, lea el [documentación título Storage](../../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) que le facilitará más detalles en Universal, confianza, almacenamiento Global y los tipos de archivo de almacenamiento y el [almacenamiento conectado documentación de](../../storage-platform/connected-storage/connected-storage-overview.md), lo que aprenderá más acerca de cómo guardar el progreso de juegos en la nube para que pueda continuar su juego entre dispositivos.
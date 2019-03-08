---
title: Administración de almacenamiento local conectado
description: Obtenga información sobre cómo administrar datos locales de almacenamiento conectado en un entorno de desarrollo.
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 09fce637c50b0a03230d0808e51a9e5cfadc7179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642060"
---
# <a name="managing-local-connected-storage"></a>Administración de almacenamiento local conectado
Mientras se usa almacenamiento conectado para almacenar los datos de juegos en la nube, también es un componente de almacenamiento local para el servicio de almacenamiento conectado. Si se encuentra en un PC o consola hay una memoria caché local de los datos de almacenamiento conectado que contiene los datos sincronizados en la nube. Si está creando un título XDK o UWP hay una herramienta que le permiten administrar los datos locales de almacenamiento conectado.

Consulte la tabla siguiente para encontrar la herramienta adecuada para administrar los datos de almacenamiento conectado localmente en caché:

|Clasificación de título  |Dispositivo  |Herramienta de almacenamiento local  |
|---------|---------|---------|
|XDK     |Consola Xbox One     |*xbstorage*         |
|UWP     |PC         |*gamesaveutil*         |
|UWP     |Consola Xbox One     |Portal de dispositivo Xbox (XDP |) simple

- *Xbstorage* es una herramienta de línea de comandos, ejecutar desde la línea de comandos XDK para administrar almacenadas en caché localmente de almacenamiento conectado en la consola de una Xbox. El *xbstorage* herramienta se puede encontrar en el XDK una Xbox bajo la ruta de acceso de archivo: **/Program archivos (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* es una herramienta de línea de comandos para administrar para UWP localmente en caché de almacenamiento conectado en el equipo. El *gamesaveutil* herramienta se suministra con el [SDK de Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) Fall Creators Update y versiones posteriores (10.0.16299.15 de compilación y versiones posteriores). Una vez haya instalado la versión adecuada del SDK de Windows 10, *gamesaveutil* pueden encontrarse en la carpeta: **Kits de ProgramFiles(x86)/Windows/10/bin / [SDK Version]/x64/gamesaveutil.exe**.
- *Portal(XDP) de dispositivo Xbox* es un portal en línea que le permite administrar los datos de UWP de almacenamiento conectado localmente en caché en la consola de una Xbox. Este artículo no trata el uso XDP. Para aprender a utilizar XDP lee [Portal de dispositivos para Xbox](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox).

## <a name="xbstorage"></a>Xbstorage

*Xbstorage* permite borrar los datos almacenados en caché localmente datos del almacenamiento conectado desde el disco duro, así como importación y exportación de datos para los usuarios o equipos de espacios de almacenamiento conectados mediante el uso de archivos XML.

Cuando se realiza una operación de datos local con el *xbstorage* herramienta, el sistema se comportará como si esa operación se realizó por la propia aplicación, por lo que hará que el acto de leer los datos de un espacio de almacenamiento conectado a un archivo local sincronización con la nube antes de copiar.

De forma similar, una copia de datos de un archivo XML en el equipo de desarrollo a un contenedor de almacenamiento conectado en el kit de desarrollo de Xbox One hará que la consola para iniciar la carga de datos a la nube. Sin embargo, hay condiciones en que esto no sucederá: si el kit de desarrollo no puede adquirir el bloqueo, o si hay un conflicto de datos entre los contenedores de la consola y los de la nube, la consola se comportará como si el usuario había decidido no resuelva el conflicto por Seleccionar una versión del contenedor para mantener y la consola se comportará como si el usuario está reproduciendo sin conexión hasta la próxima vez que se inicia el título.

Es la única excepción a estos comandos **restablecer** **/force** que borra el almacenamiento local de datos guardados para todos los usuarios y SCIDs, pero no modifica los datos almacenados en la nube. Esto es útil para colocar una consola en el estado sería en si un usuario se itinerancia a una consola y descarga de datos de la nube al reproducir un título.

### <a name="xbstorage-commands"></a>Comandos Xbstorage

Xbstorage tiene las siguientes los desarrolladores de seis comandos pueden usar con la línea de comandos XDK para administrar datos locales en su Kit de desarrollo de Xbox uno:

<a id="xbstorage_reset"></a>

|Comando  |Descripción  |
|---------|---------|
|Restablecer    |Realiza una restablecimiento de fábrica en almacenamiento conectado.         |
|importar   |Importa datos desde el archivo XML especificado en un espacio de almacenamiento conectado.         |
|Exportar   |Exporta datos de un espacio de almacenamiento conectado en el archivo XML especificado.         |
|Eliminar   |Elimina los datos de un espacio de almacenamiento conectado.         |
|generar |Genera datos ficticios y guarda en el archivo XML especificado.         |
|Simular |Simula condiciones de espacio de almacenamiento insuficiente.         |

### <a name="xbstorage-reset"></a>Restablecer Xbstorage

`xbstorage reset [/force]`

Borra todos los datos locales en el almacenamiento conectado desde la consola local, lo restaura en la configuración de fábrica. Datos que se han conservado en la nube no se modificación y se volverán a descargar según sea necesarios.

|Opción  |Descripción  |
|---------|---------|
|/force   |Confirma que se debe restablecer el almacenamiento conectado. Ejecutando el comando restablecer sin especificar **/force** hace que el mensaje siguiente se muestra:   Como el generador de almacenamiento conectado restablecimiento es potencialmente destructivo, este comando no lleva a cabo el restablecimiento a menos que el **/force** marcador está presente. |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Importación de Xbstorage

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

Importa los datos especificados en *filename* a un espacio de almacenamiento conectado.
El archivo es un archivo XML que contiene los datos. Para obtener un ejemplo, vea [xbstorage generar](#xbstorage_generate). Para obtener más información sobre el formato del archivo XML, vea [formato de archivo de importación y exportación](#xbstorage_fileformat), más adelante en este tema.
Hay dos maneras de especificar el espacio de almacenamiento conectado:

- Si el archivo de entrada tiene un **ContextDescription** sección que se rellena correctamente, a continuación, se usará para especificar el espacio de almacenamiento conectado.
- El espacio de almacenamiento también se puede total o parcialmente especificar a través de los parámetros de línea de comandos, que tienen prioridad sobre los elementos correspondientes del espacio de almacenamiento especificada en el archivo de entrada.

Ejemplos de uso:

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> Antes de importar en el espacio de almacenamiento conectado especificado, el sistema intentará sincronizar con la nube con la misma lógica que se ejecuta cuando se adquiere un espacio de almacenamiento conectado por una aplicación en ejecución.
>
> Si se está ejecutando una aplicación con el mismo ¿de SCID principal, esta operación podría provocar una condición de carrera, y podría ser el contenido del espacio de almacenamiento conectado en un estado indeterminado.
>
> Si **/reemplazar** no se especifica, se borrarán los contenedores especificados en el archivo de entrada antes de escribir los blobs especificados en el archivo de entrada. Contenedores en el espacio de almacenamiento conectado no especificada en el archivo de entrada permanecerá intactos.

|Opción  |Descripción  |
|---------|---------|
|*file-name*     |Especifica un archivo XML que contiene los datos que se va a importar.         |
|/scid:*SCID*    |Especifica el identificador de configuración de servicio (¿SCID).         |
|/ Machine        |Especifica un espacio de almacenamiento conectado por equipo.  Esta opción no se puede usar simultáneamente con el **/msa** opción.         |
|/msa:*account*  |Especifica una cuenta que se usará para el almacenamiento con conexión por usuario. El usuario debe haber iniciado sesión en la consola para el espacio que se utilizará.  Esta opción no se puede usar simultáneamente con el **automático** opción.         |
|/replace        |Elimina todos los contenedores en el espacio de almacenamiento conectado especificado antes de importar.         |
|/verbose        |Muestra el estado de la importación.         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Exportación de Xbstorage

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Exporta datos de un espacio de almacenamiento conectado en el archivo especificado por **outfile**.    El archivo es un archivo XML que contiene los datos. Consulte [xbstorage generar](#xbstorage_generate) para ver cómo generar un ejemplo. Para obtener más información sobre el formato del archivo XML, vea [formato de archivo de importación y exportación](#xbstorage_fileformat), más adelante en este tema. Hay dos maneras de especificar el espacio de almacenamiento conectado:

- Si el **/Context** se usa el parámetro y el nombre de archivo especificado por \<infile > tiene un **ContextDescription** sección correctamente rellenado, entonces se usará ese archivo para especificar el Espacio de almacenamiento conectado.
- El espacio de almacenamiento también se puede total o parcialmente especificar a través de los parámetros de línea de comandos, que tienen prioridad sobre los elementos correspondientes del espacio de almacenamiento especificada en el **/Context** archivo.

Ejemplos de uso:

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> Antes de exportar el espacio de almacenamiento conectado especificado, el sistema intentará sincronizar con la nube con la misma lógica que se ejecuta cuando se adquiere un espacio de almacenamiento conectado por una aplicación en ejecución.
>
> Si se está ejecutando una aplicación con el mismo ¿de SCID principal, esta operación podría provocar una condición de carrera, y podría ser el contenido del espacio de almacenamiento conectado en un estado indeterminado.

|Opción  |Descripción  |
|---------|---------|
|*outfile*             |Archivo XML los datos se exportarán a.         |
|/context:*input-file* |Especifica un archivo de entrada desde el que se va a leer la información de espacio.         |
|/scid:*SCID*          |Especifica el identificador de configuración de servicio (¿SCID).         |
|/ Machine              |Especifica un espacio de almacenamiento conectado por equipo.  Esta opción no se puede usar simultáneamente con el **/msa** opción.         |
|/msa:*account*        |Especifica una cuenta que se usará para el almacenamiento con conexión por usuario. El usuario debe haber iniciado sesión en la consola para el espacio que se utilizará.  Esta opción no se puede usar simultáneamente con el **automático** opción.         |
|/verbose              |Muestra el estado de la operación de exportación.         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Xbstorage delete

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Elimina todos los datos de un espacio de almacenamiento conectado.
Hay dos maneras de especificar el espacio de almacenamiento conectado:

- Si el **/Context** se usa el parámetro y el nombre de archivo especificado por \<infile > tiene un **ContextDescription** sección correctamente rellenado, entonces se usará ese archivo para especificar el Espacio de almacenamiento conectado.
- El espacio de almacenamiento también se puede total o parcialmente especificar a través de los parámetros de línea de comandos, que tienen prioridad sobre los elementos correspondientes del espacio de almacenamiento especificada en el **/Context** archivo.

Ejemplos de uso:

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> Antes de eliminar el espacio de almacenamiento conectado especificado, el sistema intentará sincronizar con la nube con la misma lógica que se ejecuta cuando se adquiere un espacio de almacenamiento conectado por una aplicación en ejecución.
>> Si se está ejecutando una aplicación con el mismo ¿de SCID principal, esta operación podría provocar una condición de carrera, y podría ser el contenido del espacio de almacenamiento conectado en un estado indeterminado.

|Opción  |Descripción |
|---------|---------|
|/context:*input-file*     |Especifica un archivo de entrada desde el que se va a leer la información de espacio.         |
|/scid:*SCID*              |Especifica el identificador de configuración de servicio (¿SCID).         |
|/ Machine                  |Especifica un espacio de almacenamiento conectado por equipo.  Esta opción no se puede usar simultáneamente con el **/msa** opción.         |
|/msa:*account*            |Especifica una cuenta que se usará para el almacenamiento con conexión por usuario. El usuario debe haber iniciado sesión en la consola para el espacio que se utilizará.  Esta opción no se puede usar simultáneamente con el **automático** opción.         |
|/verbose                  |Muestra el estado de la operación de eliminación.         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Generar Xbstorage

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

Genera datos ficticios y lo guarda en un archivo especificado por \<filename >. Para obtener más información sobre el formato del archivo XML, vea [formato de archivo de importación y exportación](#xbstorage_fileformat), más adelante en este tema.    El identificador de configuración de servicio (¿SCID) se establecerá en 00000000-0000-0000-0000-000000000000 y la cuenta se establecerá para un espacio de almacenamiento conectado por equipo. Si desea cambiar esos valores, puede editar directamente el archivo después de generarlo.

Ejemplos de uso:

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> Los datos de bytes están una secuencia ascendente simple; Por ejemplo, un blob de cinco bytes tendría los bytes siguientes: 00 01 02 03 04. >> Si desea especificar un espacio de almacenamiento conectado por usuario, cambie el **cuenta** nodo en el archivo XML a algo parecido a lo siguiente:
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|Opción  |Descripción  |
|---------|---------|
|*file-name*     | Archivo XML los datos se escribirán en. |
|/containers:*n* | Especifica el número de *n*, de contenedores para generar. El número predeterminado es 2.  |
|/blobs:*n*      | Especifica el número de *n*, de blobs que se va a generar. El número predeterminado es 3.  |
|/blobsize:*n*   | Especifica el número de *n*, de bytes por blob. El tamaño predeterminado es 1024 bytes.  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Simular Xbstorage

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

Simula fuera de las condiciones de almacenamiento local para el servicio de almacenamiento conectado.

|Opción  |Descripción  |
|---------|---------|
|/reserveremainingspace | Reserva todo el espacio restante en el almacenamiento conectado. Eliminación de algo de ConnectedStorage abrirá espacio que puede usar. |

| / forceoutoflocalstorage | Simula el servicio de almacenamiento conectado no tener ningún espacio disponible restante. Eliminar algo de almacenamiento conectado, no se cambiará el servicio de almacenamiento conectados de los informes no hay memoria suficiente. |

| / detener | Detiene todas las simulaciones. |

| / verbose | Muestra el estado de la operación simulada. |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>Opciones comunes

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|Opción  |Descripción  |
|---------|---------|
| /?                           |  Muestra la Ayuda de xbstorage.exe |
| /X *:address* [*+accesskey*]  | Especifica el nombre de host o la dirección (se muestra como **herramientas IP** en la consola) de una consola de destino, pero no cambia la consola predeterminada. Si no usa esta opción, se usa la consola de forma predeterminada. *Accesskey* es una cadena que puede usar para restringir el acceso a una consola a únicamente las personas que conocen la clave de acceso. Establecer la clave de acceso mediante el comando **xbconfig** **accesskey = *** su clave*; a continuación, reinicie la consola para que surta efecto la clave de acceso. Para obtener acceso a una consola que se configura con una clave de acceso, debe incluir un signo más (+) y la clave de acceso después del nombre de host o dirección IP de la consola.
> [!NOTE]
> Si se proporciona una clave de acceso cuando xbconnect establece la consola de forma predeterminada, la clave de acceso se almacena como parte de la dirección de la consola de forma predeterminada.
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* le permite administrar el almacenamiento conectado localmente en caché para funciones de la aplicación con todas del mismo *xbstorage* proporciona. Al igual que la herramienta xbstorage, *gamesaveutil* ofrece los mismos datos de seis administrar funciones, con algunas diferencias de comportamiento. La importación, exportación, eliminar y restablecer comandos requieren un usuario de Xbox Live, iniciar sesión en. Puede usar el App Xbox en Windows 10 para ver y cambiar el usuario actual.

### <a name="commands"></a>Comandos

|Comando  |Descripción  |
|---------|---------|
|importar   |Importa datos desde el archivo XML especificado         |
|Exportar   |Exporta los datos en el archivo xml especificado         |
|Eliminar   |Elimina los datos de la aplicación especificada        |
|Restablecer    |Elimina los datos locales solo para la aplicación especificada         |
|generar |genera datos ficticios y lo guarda en el archivo xml especificado         |
|Simular |simula las condiciones especiales que son difíciles de probar         |

### <a name="gamesaveutil-import"></a>Importación de Gamesaveutil

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

Importa los datos especificados en \<nombreDeArchivo >

El archivo es un archivo XML que contiene los datos. Tipo `gamesaveutil help generate` para ver cómo generar un ejemplo.

Hay dos maneras de especificar la aplicación PFN y ¿SCID donde se guardan los datos:

Si el archivo de entrada tiene una sección ContextDescription completada correctamente, se usará para especificar la aplicación de destino PFN y ¿SCID.

El PFN ¿SCID pueden total o parcialmente especificarse y a través de los parámetros de línea de comandos, que tienen prioridad sobre los elementos correspondientes de la PFN especificado y ¿SCID desde el archivo de entrada.

|Opción  |Descripción  |
|---------|---------|
|/pfn:\<PFN>       |Especifica la familia de paquete Name(PFN) para la aplicación para llevar a cabo la importación del. Debe estar instalada la aplicación.         |
|/scid:\<SCID>     |Especifica el identificador de configuración de servicio (¿SCID). Se trata de la configuración de Xbox Live.         |
|/replace         |Eliminar todos los contenedores antes de realizar la importación.         |

Usos de ejemplo:

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> La aplicación debe estar instalado y se han sincronizado datos ya con el fin de importar.
> 
> Si no se especifica /replace, contenedores existentes no se modifican a menos que se especifican en el archivo de entrada.

### <a name="gamesaveutil-export"></a>Exportación de Gamesaveutil

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Exporta los datos en el archivo especificado por \<outfile >.

El archivo es un archivo XML que contiene los datos. Escriba help gamesaveutil generar para ver cómo generar un ejemplo.

Hay dos maneras de especificar la ubicación de los datos que se va a exportar:

Si se usa el parámetro Context, y el nombre de archivo especificado por \<infile > tiene una sección de ContextDescription completada correctamente, se usará ese archivo para especificar la ubicación del origen de datos.

También se puede especificar la ubicación a través de los parámetros de línea de comandos, que tienen prioridad sobre los respectivos elementos especificados por el archivo/Context.

|Opción  |Descripción  |
|---------|---------|
|/context:\<infile>     |Use el archivo especificado para especificar la aplicación PFN y ¿SCID.         |
|/pfn:\<PFN>            |Especifica la familia de paquete Name(PFN) para la aplicación para llevar a cabo la exportación desde. Debe estar instalada la aplicación.       |
|/scid:\<SCID>          |Especifica el identificador de configuración de servicio (¿SCID). Se trata de la configuración de Xbox Live.        |

Usos de ejemplo:

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> La aplicación debe estar instalado y se han sincronizado datos ya con el fin de exportar.

### <a name="gamesaveutil-delete"></a>Gamesaveutil delete

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Elimina todos los datos para el PFN y ¿SCID especificados.

Hay dos maneras de especificar la ubicación de los datos que se va a eliminar:

Si se usa el parámetro Context, y el nombre de archivo especificado por \<infile > tiene una sección de ContextDescription completada correctamente, se usará ese archivo para especificar la ubicación del origen de datos.

También se puede especificar la ubicación a través de los parámetros de línea de comandos, que tienen prioridad sobre los respectivos elementos especificados por el archivo/Context.

|Opción  |Descripción  |
|---------|---------|
|/context:\<infile>     |Use el archivo especificado para especificar la aplicación PFN y ¿SCID.         |
|/pfn:\<PFN>            |Especifica la familia de paquete Name(PFN) para la aplicación para eliminar los datos de. Debe estar instalada la aplicación.       |
|/scid:\<SCID>          |Especifica el identificador de configuración de servicio (¿SCID). Se trata de la configuración de Xbox Live.        |

Usos de ejemplo:

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> La aplicación debe estar instalada para eliminar los contenedores.

### <a name="gamesaveutil-reset"></a>Restablecer Gamesaveutil

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Elimina todos los datos locales para el PFN y ¿SCID especificados.

Hay dos maneras de especificar la ubicación de los datos que se va a eliminar:

Si se usa el parámetro Context, y el nombre de archivo especificado por \<infile > tiene una sección de ContextDescription completada correctamente, se usará ese archivo para especificar la ubicación del origen de datos.

También se puede especificar la ubicación a través de los parámetros de línea de comandos, que tienen prioridad sobre los respectivos elementos especificados por el archivo/Context.

|Opción  |Descripción  |
|---------|---------|
|/context:\<infile>     |Use el archivo especificado para especificar la aplicación PFN y ¿SCID.         |
|/pfn:\<PFN>            |Especifica la familia de paquete Name(PFN) para la aplicación para eliminar los datos de. Debe estar instalada la aplicación.       |
|/scid:\<SCID>          |Especifica el identificador de configuración de servicio (¿SCID). Se trata de la configuración de Xbox Live.        |

Usos de ejemplo:

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> La aplicación debe estar instalada para eliminar los datos locales.

### <a name="gamesaveutil-generate"></a>Generar Gamesaveutil

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

Genera datos ficticios y lo guarda en un archivo especificado por \<filename >.
El Comprobador de coherencia de ¿(SCID, servicio de configuración de identificador) se establecerá en 00000000-0000-0000-0000-000000000000. Edite el archivo manualmente después de la generación para cambiar estos valores si así lo desea.

|Opción  |Descripción  |
|---------|---------|
|/Containers:\<n >     |Especifica cuántos contenedores para generar. Valor predeterminado es 2.         |
|/blobs:\<n>          |Especifica cuántos blobs por contenedor para generar. El valor predeterminado es 3.        |
|/blobsize:\<n>       |Especifica el número de bytes por blob. Valor predeterminado es 1024.         |

Usos de ejemplo:

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> Los datos de bytes están una secuencia ascendente simple, es decir, un blob de cinco bytes tendría los bytes 00 01 02 03 04.

### <a name="gamesaveutil-simulate"></a>Simular Gamesaveutil

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

Simula las condiciones especiales en ciertos escenarios de prueba.

|Opción  |Descripción  |
|---------|---------|
|/markcontainerschanged     |Obliga a todos los contenedores para que parezca que se han modificado cuando una aplicación se reanuda desde el estado suspendido y obtiene un nuevo proveedor. Afecta a todas las aplicaciones hasta que se borra con /stop.      |
|nombre-nodo /Stop                      |Detiene todas las simulaciones.         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>Formato de archivo de importación y exportación

El archivo XML que se usa con el **importar**, **exportar**, y **generar** comandos con el *xbstorage* herramienta tiene el formato que se muestra en el ejemplo siguiente.

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

El único cambio necesario para dar formato a este código xml para **importar**, **exportar**, y **generar** en *gamesaveutil* consiste en reemplazar el \<Cuenta > nodo de miembro de la \<ContextDescription > nodo con un \<PackageFamilyName > nodo.
Esta operación cambiará el \<ContextDescription > nodo desde este:

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

A esto:

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> El formato de datos en estos archivos XML no es idéntico a lo que aparece en la plataforma, es para la importación y exportación para fines comerciales. El formato de datos para estos archivos XML potencialmente podría cambiar en el futuro, por lo que se deben tratar como un formato intermedio o utilidad, no un formato de archivo.

El **ContextDescription** nodo es opcional. Si está realizando un espacio de almacenamiento conectado para una máquina, puede usar `<Account machine="true"/>` en lugar de `<Account msa="user@domain.com"/>`. En caso contrario, el contexto puede especificarse en la línea de comandos para la importación.
Los blobs y contenedores deben tener los nombres correspondientes que tienen concedidos por el juego o aplicación para que se genera el archivo.
El contenido de cada blob debe ser una cadena que se encapsulan en un **CDATA** etiqueta, que se genera mediante una llamada a **CryptBinaryToStringW** con la marca **CRYPT_STRING_BASE64** que proporciona los datos para ese blob como una matriz de bytes sin formato. Por el contrario, se pueden convertir los datos blob llamando a **CryptStringToBinary** y proporcionar la cadena cifrada anteriormente. Se muestra un ejemplo del uso de estas dos funciones en [CryptBinaryToString devuelve bytes no válidos](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral) en los foros de MSDN para Visual Studio.

<a id="ID4EWBAE"></a>
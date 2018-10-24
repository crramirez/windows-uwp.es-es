---
author: laurenhughes
title: Crear un paquete de la aplicación con la herramienta MakeAppx.exe
description: MakeAppx.exe crea, cifra, descifra y extrae los archivos de paquetes de aplicaciones y lotes.
ms.author: lahugh
ms.date: 06/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, packaging, empaquetado
ms.assetid: 7c1c3355-8bf7-4c9f-b13b-2b9874b7c63c
ms.localizationpriority: medium
ms.openlocfilehash: dbde8f2f11276ded6ad0994a1cd52f7f12de229e
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5477348"
---
# <a name="create-an-app-package-with-the-makeappxexe-tool"></a>Crear un paquete de la aplicación con la herramienta MakeAppx.exe


**MakeAppx.exe** crea tanto paquetes de aplicaciones como lotes de paquetes de aplicaciones. **MakeAppx.exe** también extrae los archivos de un paquete de aplicaciones o un lote y cifra o descifra los paquetes de aplicaciones y lotes. Esta herramienta se incluye en el SDK de Windows 10 y puede usarse desde un símbolo del sistema o un archivo de script.

> [!IMPORTANT] 
> Si usaste Visual Studio para desarrollar tu aplicación, es recomendable usar el Asistente de Visual Studio para crear el paquete de la aplicación. Para más información, consulta [Empaquetado de aplicaciones para UWP con Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Ten en cuenta que **MakeAppx.exe** no crea ningún archivo .appxupload. El archivo .appxupload, se crea como parte del proceso de empaquetado de Visual Studio y contiene otros dos archivos: .msix o .appx y .appxsym. El archivo .appxsym es un archivo .pdb comprimido que contiene los símbolos públicos de la aplicación usados para el [análisis de bloqueos](https://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) en el Centro de desarrollo de Windows. También se puede enviar un archivo .appx normal, pero no habrá disponible ninguna información de análisis o de depuración de bloqueos. Para obtener más información sobre el envío de paquetes a la Store, consulta [Cargar paquetes de aplicación](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages). 

 Las actualizaciones de esta herramienta en la versión más reciente de Windows 10 no afectan al uso de paquete .appx. Puedes seguir usando esta herramienta con los paquetes .appx, o usar la herramienta de soporte técnico para los paquetes .msix tal como se describe a continuación.

Para crear un archivo .appxupload manualmente:
- Coloca el .msix y .appxsym en una carpeta
- Comprime la carpeta.
- Cambiar el nombre de extensión de la carpeta comprimida de .zip a .appxupload.

## <a name="using-makeappxexe"></a>Uso de MakeAppx.exe

En función de la ruta de acceso de instalación del SDK, aquí es donde está **MakeAppx.exe** en tu equipo Windows 10:
- x86: C:\Archivos de programa (x86)\Windows Kits\10\bin\x86\makeappx.exe
- x64: C:\Archivos de programa (x86)\Windows Kits\10\bin\x64\makeappx.exe

No hay ninguna versión ARM de esta herramienta.

### <a name="makeappxexe-syntax-and-options"></a>Opciones y sintaxis de MakeAppx.exe

Sintaxis general de **MakeAppx.exe**:

``` Usage
MakeAppx <command> [options]      
```

La siguiente tabla describe los comandos de **MakeAppx.exe**.

| **Comando**   | **Descripción**                       |
|---------------|---------------------------------------|
| pack          | Crea un paquete.                    |
| unpack        | Extrae todos los archivos del paquete especificado en el directorio de salida especificado. |
| bundle        | Crea un lote.                     |
| unbundle      | Desempaqueta todos los paquetes a un subdirectorio en la ruta de acceso de salida especificada, cuyo nombre es el nombre completo del paquete. |
| encrypt       | Crea un paquete o un lote de aplicación cifrado desde el paquete o lote de entrada en el paquete o lote de salida especificado. |
| decrypt       | Crea un paquete o un lote de aplicación descifrado desde el paquete o lote de aplicación de entrada en el paquete o lote de salida especificado. |


Esta lista de opciones se aplica a todos los comandos:

| **Opción**    | **Descripción**                       |
|---------------|---------------------------------------|
| /d            | Especifica el directorio de entrada, salida o contenido. |
| /l            | Se usa para los paquetes localizados. La validación predeterminados se desactiva en paquetes localizados. Esta opción deshabilita solo esa validación específica, sin necesidad de deshabilitar toda la validación. |
| /kf           | Cifra o descifra el paquete o lote mediante la clave del archivo de clave especificado. No puede usarse junto con /kt. |
| /kt           | Cifra el o descifra el paquete o lote mediante la clave de prueba global. No puede usarse junto con /kf. |
| /no           | Se impide que se sobrescriba el archivo de salida, si lo hay. Si no se especifica esta opción o la opción /o, se pregunta al usuario si desea sobrescribir el archivo. |
| /nv           | Omite la validación semántica. Si no se especifica esta opción, la herramienta realiza una validación completa del paquete. |
| /o            | Sobrescribe el archivo de salida, si existe. Si no se especifica esta opción o la opción /no, se pregunta al usuario si desea sobrescribir el archivo. |
| /p            | Especifica el paquete de la aplicación o el lote.  |
| /v            | Permite la salida del registro detallado en la consola. |
| /?            | Muestra el texto de ayuda.                   |


La siguiente lista contiene posibles argumentos:

| **Argumento**                          | **Descripción**                       |
|---------------------------------------|---------------------------------------|
| &lt;output package name&gt;           | El nombre del paquete creado. Este es el nombre de archivo con .msix o .appx anexado. |
| &lt;encrypted output package name&gt; | El nombre del paquete cifrado creado. Este es el nombre de archivo con .emsix o .eappx anexado. |
| &lt;input package name&gt;            | El nombre del paquete. Este es el nombre de archivo con .msix o .appx anexado. |
| &lt;encrypted input package name&gt;  | El nombre del paquete cifrado. Este es el nombre de archivo con .emsix o .eappx anexado. |
| &lt;output bundle name&gt;            | El nombre del lote creado. Este es el nombre de archivo con .msixbundle o .appxbundle anexado. |
| &lt;encrypted output bundle name&gt;  | El nombre del lote cifrado creado. Este es el nombre de archivo con .emsixbundle o .eappxbundle anexado. |
| &lt;input bundle name&gt;             | El nombre del lote. Este es el nombre de archivo con .msixbundle o .appxbundle anexado. |
| &lt;encrypted input bundle name&gt;   | El nombre del lote cifrado. Este es el nombre de archivo con .emsixbundle o .eappxbundle anexado. |
| &lt;content directory&gt;             | La ruta de acceso del contenido de paquete de aplicación o el lote. |
| &lt;mapping file&gt;                  | El nombre de archivo que especifica el origen del paquete y el destino. |
| &lt;output directory&gt;              | La ruta de acceso al directorio de los paquetes y lotes de salida. |
| &lt;key file&gt;                      | El nombre del archivo que contiene una clave para el cifrado o el descifrado. |
| &lt;algorithm ID&gt;                  | Los algoritmos usados al crear una asignación de bloques. Los algoritmos válidos son: SHA256 (predeterminado), SHA384 y SHA512. |


### <a name="create-an-app-package"></a>Crear un paquete de la aplicación

Un paquete de aplicación es un conjunto completo de archivos de la aplicación empaquetado en un archivo de paquete .msix o .appx. Para crear un paquete de aplicación mediante el comando **pack**, debes proporcionar un directorio de contenido o un archivo de asignación de la ubicación del paquete. También puedes cifrar un paquete mientras lo creas. Si deseas cifrar el paquete, debes usar /ep y especificar si estás usando un archivo de clave (/kf) o la clave de prueba global (/kt). Para obtener más información sobre cómo crear un paquete cifrado, consulta [Cifrar o descifrar un paquete o lote](#encrypt-or-decrypt-a-package-or-bundle).

Opciones específicas del comando **pack**:

| **Opción**    | **Descripción**                       |
|---------------|---------------------------------------|
| /f            | Especifica el archivo de asignación.           |
| /h            | Especifica el algoritmo hash que usar al crear la asignación de bloques. Solo puede usarse con el comando pack. Los algoritmos válidos son: SHA256 (predeterminado), SHA384 y SHA512. |
| /m            | Especifica la ruta de acceso a un manifiesto de aplicación de entrada que se usará como base para generar el manifiesto del paquete de salida de la aplicación o del paquete de recursos.  Si usas esta opción, también debes emplear /f e incluir una sección [ResourceMetadata] en el archivo de asignación para especificar las dimensiones de los recursos que se incluirán en el manifiesto generado.|
| /nc           | Evita la compresión de los archivos del paquete. De manera predeterminada, los archivos se comprimen en función del tipo de archivo detectado. |
| /r            | Crea un paquete de recursos. Debe usarse junto con /m e implica el uso de la opción /l. |  


Los siguientes ejemplos de uso muestran algunas posibles opciones de sintaxis para el comando **pack**:

``` syntax 
MakeAppx pack [options] /d <content directory> /p <output package name>
MakeAppx pack [options] /f <mapping file> /p <output package name>
MakeAppx pack [options] /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /r /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kf <key file>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kt

```
El siguiente ejemplo muestra ejemplos de la línea de comandos para el comando **pack**:

``` examples
MakeAppx pack /v /h SHA256 /d "C:\My Files" /p MyPackage.msix
MakeAppx pack /v /o /f MyMapping.txt /p MyPackage.msix
MakeAppx pack /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p AppPackage.msix
MakeAppx pack /r /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p ResourcePackage.msix
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kf MyKeyFile.txt
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kt
```

### <a name="create-an-app-bundle"></a>Crear un lote de aplicaciones

Un lote de aplicaciones es similar a un paquete de aplicaciones, pero un lote puede reducir el tamaño de la aplicación que se descargarán los usuarios. Los lotes de aplicaciones resultan ser útiles para recursos específicos por idioma, recursos de escala de imagen variables o recursos que se apliquen a versiones específicas de Microsoft DirectX, por ejemplo. De forma similar a crear un paquete de aplicación cifrado, también puedes cifrar el lote de aplicaciones mientras lo agrupas. Para cifrar el lote de aplicaciones, usa la opción /ep y especifica si vas a usar un archivo de clave (/kf) o la clave de prueba global (/kt). Para obtener más información sobre cómo crear un lote cifrado, consulta [Cifrar o descifrar un paquete o lote](#encrypt-or-decrypt-a-package-or-bundle).

Opciones específicas del comando **bundle**:

| **Opción**    | **Descripción**                       |
|---------------|---------------------------------------|
| /bv           | Especifica el número de versión del lote. El número de versión debe estar en cuatro partes separadas por puntos con la forma: &lt;Principal&gt;.&lt;Secundaria&gt;.&lt;Compilación&gt;.&lt;Revisión&gt;. |
| /f            | Especifica el archivo de asignación.           |

Ten en cuenta que si no se especifica la versión del paquete, o si se establece en "0.0.0.0", el lote se crea con la fecha y la hora actuales.

Los siguientes ejemplos de uso muestran algunas posibles opciones de sintaxis para el comando **bundle**:

``` syntax
MakeAppx bundle [options] /d <content directory> /p <output bundle name>
MakeAppx bundle [options] /f <mapping file> /p <output bundle name>
MakeAppx bundle [options] /d <content directory> /ep <encrypted output bundle name> /kf MyKeyFile.txt
MakeAppx bundle [options] /f <mapping file> /ep <encrypted output bundle name> /kt
```
El siguiente bloque contiene ejemplos para el comando **bundle**:

``` examples
MakeAppx bundle /v /d "C:\My Files" /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kf MyKeyFile.txt
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kt
```

### <a name="extract-files-from-a-package-or-bundle"></a>Extraer archivos de un paquete o lote

Además de empaquetar y crear lotes de aplicaciones, **MakeAppx.exe** también puede desempaquetar o sacar de lotes los paquetes existentes. Debes proporcionar el directorio de contenido como destino para los archivos extraídos. Si estás intentando extraer archivos de un paquete o lote cifrado, puedes descifrar y extraer los archivos al mismo tiempo mediante la opción /ep y especificar si debe deben descifrarse con un archivo de clave (/kf) o con la clave de prueba global (/kt). Para obtener más información acerca de cómo descifrar un paquete o lote, consulta [Cifrar o descifrar un paquete o lote](#encrypt-or-decrypt-a-package-or-bundle).

Opciones específicas de los comandos **upack** y **unbundle**:

| **Opción**    | **Descripción**                       |
|---------------|---------------------------------------|
| /nd           | No realiza ningún descifrado al desempaquetar o separar el paquete o lote. |
| /pfn          | Desempaqueta/separa todos los archivos a un subdirectorio de la ruta de salida especificada, cuyo nombre es el nombre completo del paquete o lote. |

Los siguientes ejemplos de uso muestran algunas opciones de sintaxis posible para los comandos **unpack** y **unbundle**:

``` syntax
MakeAppx unpack [options] /p <input package name> /d <output directory>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kf <key file>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kt

MakeAppx unbundle [options] /p <input bundle name> /d <output directory>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kf <key file>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kt
```

El siguiente bloque contiene ejemplos de uso de los comandos **unpack** y **unbundle**:

``` examples
MakeAppx unpack /v /p MyPackage.msix /d "C:\My Files"
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kt

MakeAppx unbundle /v /p MyBundle.msixbundle /d "C:\My Files"
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kt
```

### <a name="encrypt-or-decrypt-a-package-or-bundle"></a>Cifrar y descifrar un paquete o lote

La herramienta **MakeAppx.exe** también puede cifrar o descifrar un paquete o un lote existentes. Simplemente debes proporcionar el nombre del paquete, el nombre del paquete de salida y si el cifrado o el descifrado deben usar un archivo de clave (/kf) o la clave de prueba global (/kt).

El cifrado y el descifrado no están disponibles a través del asistente de empaquetado de Visual Studio. 

Opciones específicas de los comandos **encrypt** y **decrypt**:

| **Opción**    | **Descripción**                       |
|---------------|---------------------------------------|
| /ep           | Especifica un paquete o lote de aplicaciones cifrado. |

Los siguientes ejemplos de uso muestran algunas opciones de sintaxis posible para los comandos **encrypt** y **decrypt**:

``` syntax
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kf <key file>
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kt

MakeAppx decrypt [options] /ep <package name> /p <output package name> /kf <key file>
MakeAppx decrypt [options] /ep <package name> /p <output package name> /kt
```

El siguiente bloque contiene ejemplos de uso de los comandos **encrypt** y **decrypt**:

``` examples
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt

MakeAppx.exe decrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe decrypt p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt
```

## <a name="key-files"></a>Archivos de clave

Los archivos de clave deben comenzar con una línea que contenga la cadena "[Keys]" seguida de líneas que describan las claves con las que cifrar cada paquete. Cada clave se representa mediante un par de cadenas entre comillas, separadas por espacios o tabuladores. La primera cadena representa el Id. de clave de 32bits codificado en base64, y la segunda representa la clave de cifrado de 32bits codificada en base64. Un archivo de clave debe ser un archivo de texto sencillo.

Ejemplo de un archivo de clave:

``` Example
[Keys]
"OWVwSzliRGY1VWt1ODk4N1Q4R2Vqc04zMzIzNnlUREU="    "MjNFTlFhZGRGZEY2YnVxMTBocjd6THdOdk9pZkpvelc="
```

## <a name="mapping-files"></a>Archivos de asignación
Los archivos de asignación deben comenzar con una línea que contenga la cadena "[Files]" seguida de líneas que describan los archivos que agregar al paquete. Cada archivo se describe mediante un par de rutas de acceso entre comillas, separadas por espacios o pestañas. Cada archivo representa su origen (en el disco) y destino (en el paquete). Un archivo de asignaciones debe ser un archivo de texto sencillo.

Ejemplo de un archivo de asignaciones (sin la opción /m):

``` Example
[Files]
"C:\MyApp\StartPage.html"               "default.html"
"C:\Program Files (x86)\example.txt"    "misc\example.txt"
"\\MyServer\path\icon.png"              "icon.png"
"my app files\readme.txt"               "my app files\readme.txt"
"CustomManifest.xml"                    "AppxManifest.xml"
``` 

Al usar un archivo de asignaciones, puedes elegir si quieres usar la opción /m. La opción /m permite al usuario especificar los metadatos de recursos en el archivo de asignaciones que se incluirán en el manifiesto generado. Si usas la opción /m, el archivo de asignaciones debe contener una sección que comience por la línea "[ResourceMetadata]", seguida de líneas que especifique "ResourceDimensions" y "ResourceId." Es posible que un paquete de aplicación contenga varias "ResourceDimensions", pero solo puede haber un "ResourceId".

Ejemplo de un archivo de asignaciones (con la opción /m):

``` Example
[ResourceMetadata]
"ResourceDimensions"                    "language-en-us"
"ResourceId"                            "English"

[Files]
"images\en-us\logo.png"                 "en-us\logo.png"
"en-us.pri"                             "resources.pri"
```

## <a name="semantic-validation-performed-by-makeappxexe"></a>Validación semántica realizada por MakeAppx.exe

**MakeAppx.exe** realiza una validación semántica limitada que se ha diseñado para capturar los errores de implementación más comunes y ayudar a garantizar que el paquete de la aplicación sea válido. Consulta la opción /nv si quieres omitir la validación mientras usas **MakeAppx.exe**. 

Esta validación garantiza que:
- Todos los archivos a los que se hace referencia en el manifiesto del paquete se incluyen en el paquete de la aplicación.
- Una aplicación no tiene dos claves idénticas.
- Una aplicación no se registra para un protocolo prohibido de esta lista: SMB, FILE, MS-WWA-WEB, MS-WWA. 

No es una validación semántica completa, ya que solo está diseñada para detectar errores comunes. No se garantiza que los paquetes creados por **MakeAppx.exe** puedan instalarse.
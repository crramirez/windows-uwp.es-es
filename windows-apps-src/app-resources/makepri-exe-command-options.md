---
author: stevewhims
Description: MakePri.exe has the set of commands createconfig, dump, new, resourcepack, and versioned. This topic details their use.
title: Opciones de línea de comandos de MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: c0a3892348baff56bbef8d40dd9aade4e612c50d
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4085790"
---
# <a name="makepriexe-command-line-options"></a>Opciones de línea de comandos de MakePri.exe

[MakePri.exe](compile-resources-manually-with-makepri.md) tiene el conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` y `versioned`. En este tema se detallan las opciones de línea de comandos para su uso.

> [!NOTE]
> MakePri.exe se instala al comprobar la opción de **SDK de Windows administra las aplicaciones para UWP** al instalar el Kit de desarrollo de Software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (también en carpetas con el nombre para el resto de arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="makepri-commands"></a>Comandos MakePri

Ejecuta `MakePri.exe help` para ver los comandos que se pueden usar con MakePri.exe.

```
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Comando Createconfig

El comando `createconfig` crea un archivo de configuración PRI nuevo e inicializado que define los valores predeterminados de calificador que especifiques. Ejecuta `MakePri.exe createconfig /?` para ver ayuda detallada para este comando.

```
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Comando Dump (Volcar)

El comando `dump` genera un archivo xml volcado que contiene una lista de todos los recursos de un archivo PRI especificado. Ejecuta `MakePri.exe dump /?` para ver ayuda detallada para este comando.

> [!NOTE]
> Un paquete de recursos sin esquemas es el que se creó con el conmutador *omitSchemaFromResourcePacks* del archivo de configuración PRI. Para volcar un paquete de recursos sin esquema, usa el conmutador `/es <main_package_PRI_file>`. Si no especificas el archivo principal, verás el mensaje de error "*The resources.pri in the package was corrupted so encryption failed (error PRI222: 0xdef0000f - Unspecified error occurred)*".

```
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>Comando New (Nuevo)

El comando `new` crea un nuevo archivo PRI indizando los archivos en el proyecto, según lo indique el archivo de configuración. Ejecuta `MakePri.exe new /?` para ver ayuda detallada para este comando.

```
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>Comando ResourcePack (Paquete de recursos)

El comando `resourcepack` crea un nuevo archivo PRI indizando los archivos del proyecto, según lo indique el archivo de configuración. Un archivo PRI de paquete de recursos contiene solo las variantes adicionales de recursos ya especificados en un archivo PRI existente. Ejecuta `MakePri.exe resourcepack /?` para ver ayuda detallada para este comando.

```
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Comando Versioned (Versionado)

El comando `versioned` crea un nuevo archivo PRI versionado indizando los archivos del proyecto, según lo indique el archivo de configuración. Ejecuta `MakePri.exe versioned /?` para ver ayuda detallada para este comando.

```
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll(ex)

Debes usar la opción de DLL de extensión (/ex) con `createconfig`, `dump`, `new`, `resourcepack`, y `versioned`, para especificar la ubicación de la DLL de extensión de entorno del sistema de administración de recursos.

## <a name="logging47metadata-file"></a>Registro de&#47;archivo de metadatos

MakePri puede incluir información específica de un paquete de recursos en el archivo de metadatos del indizador. Este es un ejemplo de un archivo de registro para `resources.pri` con archivos PRI de recursos `german.pri` y `highresolution.pri`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>Opción &#47;IndexFile(if)

Debes usar la opción de archivo de índice (/if) con `dump`, `resourcepack` y `versioned` para especificar un archivo PRI de entrada.

Para `resourcepack` y `versioned`, en lugar de proporcionar un archivo PRI como parámetro de entrada de /IndexFile(if), en su lugar, puedes proporcionar un archivo de esquema.

```
/IndexFile(if) <FILEPATH>
```

**FILEPATH** es un token que especifica la ubicación del archivo PRI de entrada o el archivo de esquema PRI.

## <a name="47mappingfilemf-option"></a>Opción &#47;MappingFile(mf)

Debes usar la opción de archivo de asignación (/mf) con `new`, `resourcepack` y `versioned` para generar un archivo de asignación. [MakeAppx.exe](../packaging/create-app-package-with-makeappx-tool.md) usa el archivo de asignación para generar paquetes de la aplicación.

```
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** es un token que especifica el formato del archivo de asignación. El único formato válido es `appx`.

```
/mf appx
```

Este es un contenido de ejemplo de un archivo de asignación principal.

```
"ResourceDimensions"                   "language-de-de"
```

Y este es un contenido de ejemplo de un archivo de asignación de paquete de recursos.

```
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>Resumen de salida

Si se crean paquetes de recursos, el resumen de salida de MakePRI.exe tiene una forma más detallada. Aquí tienes un ejemplo.

```
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>Opción &#47;Overwrite(o) (Sobrescribir)

Si no se proporciona la opción de sobrescribir (/o) y los archivos de salida especificados ya existen, MakePri.exe requiere una confirmación antes de sobrescribir.

```
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>Opción &#47;OutputFile(of) (Archivo de salida)

Debes usar la opción de archivo de salida (/of) con `dump`, `new`, `resourcepack` y `versioned` para especificar la ubicación de salida y el nombre del archivo PRI a generar. Si MakePri.exe genera más de un archivo PRI de recursos, los coloca en la carpeta principal del archivo de destino. Por ejemplo, si especificas `/of MyParentFolder\TargetFile.pri`, a continuación, MakePri.exe genera `TargetFile.language-en.pri` y `TargetFile.scale-100.pri`, junto con `TargetFile.pri`en `ParentFolder`.

Aquí te mostramos una condición de error de ejemplo y el mensaje de error correspondiente.

| Condición de error | Mensaje de error |
| --------------- | ------------- |
| El nombre del archivo de salida es igual que el nombre de uno de los paquetes de recursos de la configuración. | Configuración no válida: el nombre del paquete de recursos <resource pack name> no puede ser el mismo que el del archivo de salida <outputfilename.pri>. |

## <a name="reversemaprm-option"></a>Opción /ReverseMap(rm) (Invertir asignación)

Debes usar la opción de asignación inversa (/rm) con `new`, `resourcepack` y `versioned` para generar una sección de asignación inversa en el archivo PRI, que se pueden usar para depurar.

## <a name="47schemafilesf-option"></a>Opción &#47;SchemaFile(sf) (Archivo de esquema)

Debes usar la opción de archivo de esquema (/sf) con `new`, `resourcepack` y `versioned` para escribir un archivo de esquema en la ubicación especificada.

Para `resourcepack` y `versioned`, en lugar de proporcionar un archivo PRI como parámetro de entrada de /IndexFile(if), en su lugar, puedes proporcionar un archivo de esquema.

```
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** es un token que especifica dónde escribir el archivo de esquema.

Este es un ejemplo de un archivo de esquema.

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor(vma) (Versión principal) está en desuso

La opción de versión principal (/vma) (para el comando `new`) está en desuso y utilizarlo provoca este mensaje de advertencia.

```
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

Para proporcionar el número de versión principal usa el atributo [resources@majorVersion](makepri-exe-configuration.md) en el archivo de configuración.

## <a name="related-topics"></a>Artículos relacionados

* [MakePri.exe](compile-resources-manually-with-makepri.md)

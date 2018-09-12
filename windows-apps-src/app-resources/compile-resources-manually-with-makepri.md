---
author: stevewhims
Description: MakePri.exe is a command line tool that you can use to create and dump PRI files. It is integrated as part of MSBuild within Microsoft Visual Studio, but it could be useful to you for creating packages manually or with a custom build system.
title: Compilar recursos manualmente con MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: d065fdffe2fcb32a9d574c90f59eb7115597167a
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3933692"
---
# <a name="compile-resources-manually-with-makepriexe"></a>Compilar recursos manualmente con MakePri.exe

MakePri.exe es una herramienta de línea de comandos que se puede usar para crear y volcar archivos PRI. Viene integrada como parte de MSBuild en Microsoft Visual Studio, pero podría serte útil para crear paquetes manualmente o con un sistema integrado personalizado.

> [!NOTE]
> MakePri.exe se instala al comprobar la opción de **SDK de Windows administra las aplicaciones para UWP** al instalar el Kit de desarrollo de Software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (también en carpetas con el nombre para el resto de arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

El límite de tamaño de un archivo PRI es de 64kilobytes.

## <a name="in-this-section"></a>En esta sección
|Tema|Descripción|
|-|-|
| [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md) | MakePri.exe tiene el conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` y `versioned`. En este tema se detallan las opciones de línea de comandos para su uso. |
| [Archivo de configuración de MakePri.exe](makepri-exe-configuration.md) | En este tema se describe el esquema del archivo de configuración XML de MakePri.exe. |
| [Indizadores específicos de formato de MakePri.exe](makepri-exe-format-specific-indexers.md) | En este tema se describen los indizadores específicos de formato usados por la herramienta MakePri.exe para generar su índice de recursos. |

## <a name="makepriexe-command-line-options"></a>Opciones de línea de comandos de MakePri.exe

MakePri.exe tiene el conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` y `versioned`. Para obtener información detallada sobre su uso, consulta [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md).

## <a name="makepriexe-configuration"></a>Configuración de MakePri.exe

El archivo de configuración PRI XML determina qué recursos se indizan y cómo se indizan. El esquema del XML de configuración se describe en [Configuración de MakePri.exe](makepri-exe-configuration.md).

## <a name="format-specific-indexers"></a>Indizadores específicos del formato

MakePri.exe se suele usar con las opciones `new`, `versioned` y `resourcepack`. En esos casos indexa los archivos de origen para generar un índice de recursos. MakePri.exe usa diversos indizadores individuales para leer distintos archivos de recursos de origen o contenedores de recursos. El indexador más sencillo es el indizador de carpeta, que indiza el contenido de una carpeta en relación con recursos como imágenes `.jpg` or `.png`. Para obtener más información, consulta [Indizadores específicos del formato de MakePri.exe](makepri-exe-format-specific-indexers.md).

## <a name="makepriexe-warnings-and-error-messages"></a>Mensajes de advertencias y error de MakePri.exe

```
Resources found for language(s) '<language(s)>' but no resources found for default language(s): '<language(s)>'. Change the default language or qualify resources with the default language.
```

La advertencia anterior se muestra cuando MakePri.exe o MSBuild detecta archivos o recursos de cadena de un determinado recurso con nombre que parezca estar marcado con calificadores de idioma, pero no se encuentra ningún candidato para un idioma predeterminado. Se describe el proceso para usar calificadores en los nombres de archivos y carpetas en [Adaptar los recursos de idioma, escala y otros calificadores](tailor-resources-lang-scale-contrast.md). Un archivo o una carpeta pueden contener un nombre de idioma, pero no se detectan los recursos que estén calificados para el idioma predeterminado exacto. Por ejemplo, si un proyecto usa "en-US" como idioma predeterminado y tiene un archivo denominado "de/logo.png", pero no tiene ningún archivo marcado con el idioma predeterminado "en-US", aparecerá esta advertencia. Para quitar esta advertencia, los archivos o los recursos de cadena deberían estar calificados con el idioma predeterminado, o bien se debería cambiar el idioma predeterminado. Para cambiar el idioma predeterminado, con la solución abierta en Visual Studio, abre `Package.appxmanifest`. En la pestaña Aplicaciones, confirma que el idioma predeterminado esté establecido correctamente (por ejemplo, "en" o "en-US").

```
No default or neutral resource given for '<resource identifier>'. The application may throw an exception for certain user configurations when retrieving the resources.
```

La advertencia anterior se muestra cuando MakePri.exe o MSBuild detectan archivos o recursos que parecen estar marcados con calificadores de idioma para los que los recursos no están claros. Hay calificadores, pero sin garantía de que un candidato de recursos particular se pueda devolver para ese identificador de recursos en tiempo de ejecución. Si no se puede encontrar un candidato de recurso para un idioma particular, una región u otro calificador que sea un valor predeterminado o que coincida siempre con el contexto de un usuario, se mostrará esta advertencia. En el momento de ejecución, para configuraciones de usuario concretas, como las preferencias de idioma o la ubicación principal del usuario (**Configuración** > **Hora e idioma** > **Región e idioma**), las API usadas para recuperar el recurso pueden generar una excepción inesperada. Para quitar esta advertencia, se deberán proporcionar recursos predeterminados, como un recurso en el idioma predeterminado o la ubicación global del proyecto (homeregion-001).

## <a name="using-makepriexe-in-a-build-system"></a>Uso de MakePri.exe en un sistema de compilación

Los sistemas de compilación deberían usar los comandos de MakePri.exe `new`, `versioned` o `resourcepack`, en función del tipo de proyecto que se esté compilando. Los sistemas de compilación que crean un nuevo archivo PRI deberían usar el comando `new`. Los sistemas de compilación que deben garantizar la compatibilidad de desajustes internos con las iteraciones, pueden usar el comando `versioned`. Los sistemas de compilación que deben crear un archivo PRI que contenga variantes adicionales de recursos, con validación para garantizar que no se agreguen nuevos recursos para dicha variante, deberían usar el comando `resourcepack`.

Los sistemas de compilación que requieren un control explícito de los archivos de origen que se indizan pueden usar el indizador ResFiles en lugar de indizar una carpeta. Los sistemas de compilación también pueden usar varios pasos de índice con diferentes [indizadores específicos del formato](makepri-exe-format-specific-indexers.md) para generar un único archivo PRI.

Los sistemas de compilación también pueden usar el indizador específico del formato de PRI para agregar archivos PRI precompilados en el PRI del paquete desde otros componentes, como bibliotecas de clases, ensamblados, SDK y DLL.

Cuando se compilan los archivos PRI para otros componentes, bibliotecas de clases, ensamblados, DLL y SDK, deberá usarse la configuración **initialPath** para garantizar que los recursos de componentes tienen sus propios mapas de subrecursos que no entran en conflicto con la aplicación en la que están incluidos.

## <a name="related-topics"></a>Temas relacionados
* [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md)
* [Configuración de MakePri.exe](makepri-exe-configuration.md)
* [Indizadores específicos de formato de MakePri.exe](makepri-exe-format-specific-indexers.md)
* [Adaptar los recursos al idioma, escala y otros calificadores](tailor-resources-lang-scale-contrast.md)

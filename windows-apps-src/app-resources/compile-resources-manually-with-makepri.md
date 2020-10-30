---
description: MakePri.exe es una herramienta de línea de comandos que se puede usar para crear y volcar archivos PRI. Viene integrada como parte de MSBuild en Microsoft Visual Studio, pero podría ser útil para crear paquetes manualmente o con un sistema de compilación personalizado.
title: Compilar recursos manualmente con MakePri.exe
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 579aaf5f833f2d3ddb8e4d8c080a6f94ac21f40f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031868"
---
# <a name="compile-resources-manually-with-makepriexe"></a>Compilar recursos manualmente con MakePri.exe

MakePri.exe es una herramienta de línea de comandos que se puede usar para crear y volcar archivos PRI. Viene integrada como parte de MSBuild en Microsoft Visual Studio, pero podría ser útil para crear paquetes manualmente o con un sistema de compilación personalizado.

> [!NOTE]
> MakePri.exe se instala al activar la opción **Windows SDK para aplicaciones administradas para UWP** al instalar el kit de desarrollo de software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (así como en las carpetas denominadas para las otras arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="in-this-section"></a>En esta sección
|Tema|Descripción|
|-|-|
| [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md) | MakePri.exe tiene el conjunto de comandos `createconfig` ,,, `dump` `new` `resourcepack` y `versioned` . En este tema se detallan las opciones de línea de comandos para su uso. |
| [Archivo de configuración de MakePri.exe](makepri-exe-configuration.md) | En este tema se describe el esquema del archivo de configuración XML de MakePri.exe. |
| [Indexadores específicos del formato de MakePri.exe](makepri-exe-format-specific-indexers.md) | En este tema se describen los indizadores específicos de formato utilizados por la herramienta MakePri.exe para generar su índice de recursos. |

## <a name="makepriexe-command-line-options"></a>Opciones de línea de comandos de MakePri.exe

MakePri.exe tiene el conjunto de comandos `createconfig` ,,, `dump` `new` `resourcepack` y `versioned` . Para obtener información detallada sobre su uso, consulte [MakePri.exe opciones de la línea de comandos](makepri-exe-command-options.md).

## <a name="makepriexe-configuration"></a>Configuración de MakePri.exe

El archivo de configuración XML PRI determina cómo y qué recursos se indizan. El esquema del XML de configuración se describe en [MakePri.exe configuración](makepri-exe-configuration.md).

## <a name="format-specific-indexers"></a>Indexadores específicos de formato

Normalmente, MakePri.exe se utiliza con `new` las `versioned` Opciones, y `resourcepack` . En esos casos, indiza los archivos de código fuente para generar un índice de recursos. MakePri.exe usa varios indexadores individuales para leer diferentes archivos de recursos de origen o contenedores para los recursos. El indexador más sencillo es el indizador de carpetas, que indexa el contenido de una carpeta para recursos como `.jpg` imágenes de o `.png` . Para obtener más información, consulte [ indexadores específicos de formato deMakePri.exe](makepri-exe-format-specific-indexers.md).

## <a name="makepriexe-warnings-and-error-messages"></a>MakePri.exe advertencias y mensajes de error

### <a name="resources-found-for-languages-languages-but-no-resources-found-for-default-languages-languages-change-the-default-language-or-qualify-resources-with-the-default-language"></a>Se encontraron recursos en idiomas <> ' pero no se encontraron recursos para los idiomas predeterminados: ' <Language (s) > '. Cambiar el idioma predeterminado o calificar los recursos con el idioma predeterminado.

Esta advertencia se muestra cuando MakePri.exe o MSBuild detecta archivos o recursos de cadena de un recurso con nombre determinado que aparecen marcados con calificadores de idioma, pero no se encuentra ningún candidato para un idioma predeterminado. El proceso para usar calificadores en nombres de archivos y carpetas se describe en [adaptar los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md). Un archivo o carpeta puede tener un nombre de idioma, pero no se detecta ningún recurso que esté calificado para el idioma predeterminado exacto. Por ejemplo, si un proyecto usa "en-US" como idioma predeterminado y tiene un archivo denominado "de/logo.png", pero no tiene archivos marcados con el idioma predeterminado "en-US", aparecerá esta advertencia. Para quitar esta advertencia, los archivos o los recursos de cadena deben estar calificados con el idioma predeterminado, o bien se debe cambiar el idioma predeterminado. Para cambiar el idioma predeterminado, con la solución abierta en Visual Studio, Abra `Package.appxmanifest` . En la pestaña aplicación, confirme que el idioma predeterminado está establecido correctamente (por ejemplo, "en" o "en-US").

### <a name="no-default-or-neutral-resource-given-for-resource-identifier-the-application-may-throw-an-exception-for-certain-user-configurations-when-retrieving-the-resources"></a>No se proporcionó ningún recurso predeterminado o neutro para ' <resource identifier> '. La aplicación puede producir una excepción para ciertas configuraciones de usuario al recuperar los recursos.

Esta advertencia se muestra cuando MakePri.exe o MSBuild detecta archivos o recursos que parecen marcados con calificadores de lenguaje para los que los recursos no están claros. Hay calificadores, pero no hay ninguna garantía de que se pueda devolver un candidato de recurso determinado para ese identificador de recurso en tiempo de ejecución. Si no se puede encontrar ningún candidato de recurso para un idioma determinado, homeregion u otro calificador que sea un valor predeterminado o que siempre coincidirá con el contexto de un usuario, se mostrará esta advertencia. En tiempo de ejecución, para determinadas configuraciones de usuario, como las preferencias de idioma de un usuario o la ubicación de inicio (hora de **configuración**  >  **&**  >  **región** de idioma &), las API que se usan para recuperar el recurso pueden producir una excepción inesperada. Para quitar esta advertencia, se deben proporcionar los recursos predeterminados, como un recurso en el idioma predeterminado del proyecto o en la región de inicio global (homeregion-001).

## <a name="using-makepriexe-in-a-build-system"></a>Usar MakePri.exe en un sistema de compilación

Los sistemas de compilación deben usar `new` el `versioned` comando MakePri.exe, o `resourcepack` , dependiendo del tipo de proyecto que se está compilando. Los sistemas de compilación que crean un archivo PRI nuevo deben usar el `new` comando. Los sistemas de compilación que deben garantizar la compatibilidad de los desplazamientos internos a través de iteraciones pueden utilizar el `versioned` comando. Los sistemas de compilación que deben crear un archivo PRI que contenga variantes adicionales de recursos, con validación para garantizar que no se agreguen nuevos recursos para esa variante, deben usar el `resourcepack` comando.

Los sistemas de compilación que requieren un control explícito sobre los archivos de código fuente que se indexan pueden usar el indexador ResFiles en lugar de indizar una carpeta. Los sistemas de compilación también pueden usar varios pasos de índice con distintos [indexadores específicos del formato](makepri-exe-format-specific-indexers.md) para generar un único archivo PRI.

Los sistemas de compilación también pueden usar el indexador específico del formato PRI para agregar archivos PRI pregenerados al PRI para el paquete desde otros componentes, como bibliotecas de clases, ensamblados, SDK y archivos dll.

Cuando los archivos PRI se compilan para otros componentes, bibliotecas de clases, ensamblados, dll y SDK, se debe usar la configuración de **initialPath** para asegurarse de que los recursos de componente tienen sus propias asignaciones de Subrecursos que no entran en conflicto con la aplicación en la que se incluyen.

## <a name="related-topics"></a>Temas relacionados
* [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md)
* [ Configuración deMakePri.exe](makepri-exe-configuration.md)
* [Indexadores específicos del formato de MakePri.exe](makepri-exe-format-specific-indexers.md)
* [Adapte los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md)

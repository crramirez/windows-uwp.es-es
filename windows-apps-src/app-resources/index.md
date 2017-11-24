---
author: stevewhims
Description: "Esta sección muestra cómo crear, empaquetar y consumir los recursos de cadena de la aplicación, imagen y archivos."
title: "Recursos de aplicaciones y sistema de administración de recursos"
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
localizationpriority: medium
ms.openlocfilehash: 38a131704bacbffdf89636aa70b405aa30861d27
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2017
---
# <a name="app-resources-and-the-resource-management-system"></a>Recursos de aplicaciones y sistema de administración de recursos
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Esta sección muestra cómo crear, empaquetar y consumir los recursos de cadena de la aplicación, imagen y archivos. Por ejemplo, es posible que empaquetes un archivo con tu juego informal que contenga una definición de los niveles del juego y que cargues el archivo en tiempo de ejecución. También te mostramos cómo el mantener tus recursos independientemente de la lógica de la aplicación facilita localizar y personalizar la aplicación para distintas configuraciones regionales, pantallas de dispositivos, configuración de accesibilidad y demás contextos de máquina y usuario. Normalmente, los recursos como cadenas e imágenes deben existir en diversas variantes de idioma, escala y contraste. Para recursos como esos, tienes el apoyo del [Sistema de administración de recursos](resource-management-system.md).

Existen dos tipos de recursos de aplicaciones.
- Un recurso de archivo es un recurso que se almacena como un archivo en el disco. Un recurso de archivo puede contener una imagen de mapa de bits, XAML, XML, HTML o cualquier otro tipo de datos.
- Un recurso incrustado es un recurso que está incrustado en algún archivo de recursos que lo contiene. El ejemplo más común es un recurso de cadena incrustado dentro de un archivo de recursos (.resw o .resjson).

Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../globalizing/globalizing-portal.md).

| Artículo | Description |
|---------|-------------|
| [Sistema de administración de recursos](resource-management-system.md) | Al compilar, el sistema de administración de recursos crea un índice de todas las diferentes variantes de los recursos que se empaquetan con tu aplicación. En tiempo de ejecución, el sistema detecta el usuario y la configuración de la máquina que están en vigor y carga los recursos que son la mejor coincidencia para esa configuración. |
| [Cómo compara y elige recursos el sistema de administración de recursos](how-rms-matches-and-chooses-resources.md) | Cuando se solicita un recurso, puede ser que haya varios candidatos que coincidan en algún grado con el contexto de recurso actual. El sistema de administración de recursos analizará todos los candidatos y determinará cuál es el mejor candidato a devolver. Este tema describe con detalle ese proceso y proporciona ejemplos. |
| [Cómo compara etiquetas de idioma el sistema de administración de recursos](how-rms-matches-lang-tags.md) | El tema anterior (Cómo compara y elige recursos el sistema de administración de recursos[](how-rms-matches-and-chooses-resources.md)) se ocupa de la coincidencia de calificadores en general. Este tema se centra en la coincidencia de la etiqueta de idiomas con más detalle. |
| [Adaptar los recursos al idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md) | Este tema explica el concepto general de calificadores de recursos, cómo usarlos y la finalidad de cada uno de los nombres de calificador. |
| [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md) | Si quieres que tu aplicación admita diferentes idiomas de pantalla y tienes literales de cadena en el código o en el marcado XAML, o bien en el manifiesto de paquete de la aplicación, en ese caso mueve esas cadenas a un archivo de recursos (.resw). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación. |
| [Cargar imágenes y activos adaptados a la escala, el tema, el contraste alto y otros](images-tailored-for-scale-theme-contrast.md) | La aplicación puede cargar archivos de recursos de imagen que contengan imágenes adaptadas para factor de escala de visualización, el tema, el contraste alto y otros contextos de tiempo de ejecución. |
| [Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto.](tile-toast-language-scale-contrast.md) | Las ventanas y notificaciones del sistema pueden cargar cadenas y archivos adaptados al idioma, el factor de escala de visualización, el contraste alto y otros contextos de tiempo de ejecución. |
| [Esquemas de URI](uri-schemes.md) | Existen varios esquemas de URI (identificador uniforme de recursos) que puedes usar para hacer referencia a archivos que provienen del paquete de la aplicación, las carpetas de datos de la aplicación o la nube. También puedes usar un esquema de URI para hacer referencia a cadenas cargadas desde archivos de recursos (.resw) de la aplicación. |
| [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md) | MakePri.exe es una herramienta de línea de comandos que se puede usar para crear y volcar archivos PRI. Viene integrada como parte de MSBuild en Microsoft Visual Studio, pero podría serte útil para crear paquetes manualmente o con un sistema integrado personalizado. |
| [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md) | MakePri.exe tiene el conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` y `versioned`. En este tema se detallan las opciones de línea de comandos para su uso. |
| [Archivo de configuración de MakePri.exe](makepri-exe-configuration.md) | En este tema se describe el esquema del archivo de configuración XML de MakePri.exe. |
| [Indizadores específicos de formato de MakePri.exe](makepri-exe-format-specific-indexers.md) | Este tema describe los indizadores específicos de formato usados por la herramienta MakePri.exe para generar su índice de recursos. |
| [Usar el sistema de administración de recursos de Windows 10 en una aplicación o juego heredado.](using-mrt-for-converted-desktop-apps-and-games.md) | Al empaquetar la aplicación o juego .NET o Win32 como un paquete AppX, puedes aprovechar el sistema de administración de recursos para cargar recursos de una aplicación adaptados al contexto en tiempo de ejecución. Este tema describe las técnicas en profundidad. |

Consulta también la documentación originalmente creada para Windows 8.x, que aún se aplica a las aplicaciones para la Plataforma universal de Windows (UWP) y Windows 10.

-   [Localización y recursos de aplicaciones](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)

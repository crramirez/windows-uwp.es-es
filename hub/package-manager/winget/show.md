---
title: Comando show
description: Muestra los detalles de la aplicación especificada, incluidos los detalles sobre el origen de la aplicación, así como los metadatos asociados a la aplicación.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 9443bb31133ee9643571a4861af7027272b35d94
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334505"
---
# <a name="show-command-winget"></a>Comando show (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

El comando **show** de la herramienta [winget](index.md) muestra los detalles de la aplicación especificada, incluidos los detalles sobre el origen de la aplicación, así como los metadatos asociados a la aplicación.

El comando **show** solo muestra los metadatos que se enviaron con la aplicación. Si la aplicación enviada excluye algunos metadatos, no se mostrarán los datos.

## <a name="usage"></a>Uso

`winget show [[-q] \<query>] [\<options>]`

![Comando show](images\show.png)

## <a name="arguments"></a>Argumentos

Están disponibles los siguientes argumentos.

| Argumento  | Descripción |
|--------------|-------------|
| **-q,--query** |  Consulta usada para buscar una aplicación. |
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="options"></a>Opciones

Están disponibles las opciones siguientes:

| Opción  | Descripción |
|--------------|-------------|
| **-m,--manifest** | Ruta de acceso al manifiesto de la aplicación que se va a instalar. |
| **--id**         |  Filtra los resultados por identificador. |
| **--name**   |      Filtra los resultados por nombre. |
| **--moniker**   |  Filtra los resultados por moniker de la aplicación. |
| **-v,--version** |  Usa la versión especificada. El valor predeterminado es la versión más reciente. |
| **-s,--source** |   Busca la aplicación mediante el [origen](source.md) especificado. |
| **-e,--exact**     | Busca la aplicación mediante una coincidencia exacta. |
| **--versions**    | Muestra las versiones disponibles de la aplicación. |

## <a name="multiple-selections"></a>Selección múltiple

Si la consulta enviada a **winget** no da como resultado una sola aplicación, **winget** mostrará los resultados de la búsqueda. Esto te dará los datos adicionales necesarios para refinar la búsqueda.

## <a name="results-of-show"></a>Resultados de show

Si se detecta una única aplicación, se mostrarán los datos siguientes.

### <a name="metadata"></a>Metadatos

| Value  | Descripción |
|--------------|-------------|
| **Id**   | Identificador de la aplicación. |
| **Nombre**  | Nombre de la aplicación. |
| **Publisher** | Editor de la aplicación. |
| **Versión** | Versión de la aplicación. |
| **Author**  | Creador de la aplicación. |
| **AppMoniker** | AppMoniker de la aplicación. |
| **Descripción** | Descripción de la aplicación. |
| **Licencia**  | Licencia de la aplicación. |
| **LicenseUrl** | La dirección URL del archivo de licencia de la aplicación. |
| **Homepage**  | Página principal de la aplicación. |
| **Tags** | Etiquetas proporcionadas para ayudar en la búsqueda.  |
| **Comando** | Comandos admitidos por la aplicación. |
| **Channel**  | Detalles sobre si la aplicación se encuentra en versión preliminar o versión publicada.  |
| **Minimum OS Version** | Versión mínima del sistema operativo compatible con la aplicación. |

### <a name="installer-details"></a>Detalles del instalador

| Value  | Descripción |
|--------------|-------------|
| **Arch**   | Arquitectura del instalador. |
| **Idioma**  | Idioma del instalador. |
| **Installer Type**  | Tipo de instalador. |
| **Download Url** | Dirección URL del instalador. |
| **Hash** | SHA 256 del instalador.  |
| **Ámbito** | Muestra si el instalador es por equipo o por usuario. |

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar aplicaciones](index.md)

---
title: Comando search
description: Consulta los orígenes en busca de aplicaciones disponibles que se puedan instalar
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 30d519b5038dfa3d13e7c6f7ba3fead439ce9e3c
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636505"
---
# <a name="search-command-winget"></a>Comando search (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

El comando **search** de la herramienta [winget](index.md) consulta los orígenes en busca de aplicaciones disponibles que se puedan instalar.  

El comando **search** puede mostrar todas las aplicaciones disponibles o se puede filtrar a una aplicación específica. El comando **search** se usa normalmente para identificar la cadena que se va a usar para instalar una aplicación específica.

## <a name="usage"></a>Uso

`winget search [[-q] \<query>] [\<options>]`

![Captura de pantalla de la ventana de Windows PowerShell que muestra los resultados de la búsqueda de winget.](images\search.png)

## <a name="arguments"></a>Argumentos

Están disponibles los siguientes argumentos.

| Argumento  | Descripción |
 --------------|-------------|
| **-q,--query** |  Consulta usada para buscar una aplicación. |
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="show-all"></a>Mostrar todo

Si el comando search no incluye filtros ni opciones, mostrará todas las aplicaciones disponibles en el origen predeterminado. También puedes buscar todas las aplicaciones de otro origen si solo pasas la opción **source**.

## <a name="search-strings"></a>Cadenas de búsqueda

Las cadenas de búsqueda se pueden filtrar con las siguientes opciones.

| Opción  | Descripción |
 --------------|-------------|
| **--id**        |   Limita la búsqueda al identificador de la aplicación. El identificador incluye el editor y el nombre de la aplicación. |
| **--name**      |  Limita la búsqueda al nombre de la aplicación. |
| **--moniker**  |    Limita la búsqueda al moniker especificado. |
| **--tag**    |  Limita la búsqueda a las etiquetas que se muestran para la aplicación. |
| **--command**   |   Limita la búsqueda a los comandos que se muestran para la aplicación. |

La cadena se tratará como una subcadena. De forma predeterminada, la búsqueda también distingue entre mayúsculas y minúsculas. Por ejemplo, `winget search micro` podría devolver lo siguiente:

* Microsoft
* microscopio
* MyMicro

## <a name="search-options"></a>Opciones de búsqueda

Los comandos de búsqueda admiten una serie de opciones o filtros para ayudar a limitar los resultados.

| Opción  | Descripción |
 --------------|-------------|
| **-e, --exact**  |     Usa la cadena exacta en la consulta, incluso distingue mayúsculas y minúsculas. No usará el comportamiento predeterminado de una subcadena.  |  
| **-n, --count**      |  Restringe el resultado de visualización al recuento especificado. |
| **-s, --source**     |  Restringe la búsqueda al nombre de [source](source.md) indicado.  |

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar aplicaciones](index.md)

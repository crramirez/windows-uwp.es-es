---
title: Comando install
description: Instala la aplicación especificada.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5daae6dabee1201dd9df0b83dc56f98b06b15487
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334521"
---
# <a name="install-command-winget"></a>Comando install (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

El comando **install** de la herramienta [winget](index.md) instala la aplicación especificada. Usa el comando [**search**](search.md) para identificar la aplicación que quieres instalar.  

El comando **install** requiere que especifiques la cadena exacta que quieres instalar. Si hay alguna ambigüedad, se te pedirá que filtres aún más el comando **install** a una aplicación exacta.

## <a name="usage"></a>Uso

`winget install [[-q] \<query>] [\<options>]`

![Comando search](images\install.png)

## <a name="arguments"></a>Argumentos

Están disponibles los siguientes argumentos.

| Argumento      | Descripción |
|-------------|-------------|  
| **-q,--query**  |  Consulta usada para buscar una aplicación. |
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="options"></a>Opciones

Las opciones te permiten personalizar la experiencia de instalación para satisfacer tus necesidades.

| Opción      | Descripción |
|-------------|-------------|  
| **-m, --manifest** |   Debe ir seguido de la ruta de acceso al archivo de manifiesto (YAML). Puedes usar el manifiesto para ejecutar la experiencia de instalación desde un [archivo YAML local](#local-install). |
| **--id**    |  Limita la instalación al identificador de la aplicación.   |  
| **--name**   |  Limita la búsqueda al nombre de la aplicación. |  
| **--moniker**   | Limita la búsqueda al moniker que se muestra para la aplicación. |  
| **-v, --version**  |  Te permite especificar la versión exacta que se va a instalar. Si no se especifica, se instalará la aplicación con la versión superior. |  
| **-s, --source**   |  Restringe la búsqueda al nombre de origen indicado. Debe ir seguido del nombre del origen. |  
| **-e, --exact**   |   Usa la cadena exacta en la consulta, incluso distingue mayúsculas y minúsculas. No usará el comportamiento predeterminado de una subcadena. |  
| **-i, --interactive** |  Ejecuta el instalador en modo interactivo. La experiencia predeterminada muestra el progreso del instalador. |  
| **-h, --silent** |  Ejecuta el instalador en modo silencioso. Suprime toda la interfaz de usuario. La experiencia predeterminada muestra el progreso del instalador. |  
| **-o, --log**  |  Dirige el registro a un archivo de registro. Tienes que indicar una ruta de acceso a un archivo al que tengas derechos de escritura. |
| **--override** | Cadena que se pasará directamente al instalador.    |
| **-l, --location** |    Ubicación donde se va a instalar (si se admite). |

### <a name="example-queries"></a>Consultas de ejemplo

En el ejemplo siguiente se instala una versión específica de una aplicación.

```CMD
winget install powertoys --version 0.15.2
```

En el ejemplo siguiente se instala una aplicación a partir de su identificador.

```CMD
winget install --id Microsoft.PowerToys
```

En el ejemplo siguiente se instala una aplicación por su versión e identificador.

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>Selección múltiple

Si la consulta enviada a **winget** no da como resultado una sola aplicación, **winget** mostrará los resultados de la búsqueda. Esto te dará los datos adicionales necesarios para refinar la búsqueda para una instalación correcta.

La mejor manera de limitar la selección a solo un archivo es usar el **id.** de la aplicación combinado con la opción de consulta **exacta**.  Por ejemplo:

```CMD
winget install --id Git.Git -e 
```

## <a name="local-install"></a>Instalación local

La opción **manifest** te permite instalar una aplicación al pasar un archivo YAML directamente al cliente. La opción **manifest** tiene el siguiente uso.

Uso: `winget install --manifest \<file>`

| Opción  | Descripción |
|-------------|-------------|  
|  **-m, --manifest** | Ruta de acceso al manifiesto de la aplicación que se va a instalar. |

### <a name="log-files"></a>Archivos de registro

Los archivos de registro de winget, a menos que se redirijan, se ubicarán en la carpeta siguiente: **\%temp%\\AICLI\\*.log**

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar aplicaciones](index.md)

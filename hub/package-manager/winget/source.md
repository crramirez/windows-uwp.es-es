---
title: Comando source
description: Administra los repositorios a los que accede el Administrador de paquetes de Windows.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: cb897f25324ab8a516d18f5defe7cffa3e6a0109
ms.sourcegitcommit: 5a145eda92b5915393e58006867cdd8b98e922f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84166254"
---
# <a name="source-command-winget"></a>Comando source (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

> [!NOTE]
> Actualmente, el comando **source** es solo para uso interno. En este momento no se admiten orígenes adicionales.

El comando **source** de la herramienta [winget](index.md) administra los repositorios a los que accede el Administrador de paquetes de Windows. Con el comando **source** puedes usar **add**, **remove**, **list** y **update** para agregar, quitar, enumerar y actualizar los repositorios, respectivamente.

Un origen proporciona los datos para que puedas detectar e instalar aplicaciones. Únicamente agrega un nuevo origen si confías en él como ubicación segura.

## <a name="usage"></a>Uso

`winget source \<sub command> \<options>`

![Imagen de origen](images\source.png)

## <a name="arguments"></a>Argumentos

Están disponibles los siguientes argumentos.

| Argumento  | Descripción |
|--------------|-------------|
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="sub-commands"></a>Subcomandos

source admite los siguientes subcomandos para manipular los orígenes.

| Subcomando  | Descripción |
|--------------|-------------|
|  **add** |  Agrega un sitio origen. |
|  **list** | Enumera la lista de orígenes habilitados. |
|  **update** | Actualiza un origen. |
|  **remove** | Quita un origen. |
|  **reset** | Restablece **winget** de nuevo a la configuración inicial.  |

## <a name="options"></a>Opciones

El comando **source** admite las opciones siguientes.

| Opción  | Descripción |
|--------------|-------------|
|  **-n,--name** | Nombre por el cual identificar el origen. |
|  **-a,--arg** | Dirección URL o UNC del origen. |
|  **-t,--type** | Tipo de origen. |
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="add"></a>agregar

El subcomando **add** agrega un nuevo origen. Este subcomando requiere la opción **--name** y el argumento **name**.

Uso: `winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

Ejemplo: `winget source add --name Contoso  https://www.contoso.com/cache`

El subcomando **add** también admite el parámetro **type** opcional. El parámetro **type** comunica al cliente a qué tipo de repositorio se está conectando. Se admiten los tipos siguientes.

| Tipo  | Descripción |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | Tipo de origen \<default>. |

## <a name="list"></a>list

El subcomando **list** enumera los orígenes habilitados actualmente. Este subcomando también ofrece detalles sobre un origen específico.

Uso: `winget source list [-n, --name] \<name>`

### <a name="list-all"></a>list all

El subcomando **list** por sí solo revelará la lista completa de orígenes admitidos. Por ejemplo:

```CMD
> C:\winget source list
> Name   Arg
> -----------------------------------------
> winget https://winget.azureedge.net/cache

```

### <a name="list-source-details"></a>list source details

Para obtener detalles completos sobre el origen, pasa el nombre usado para identificar el origen. Por ejemplo:

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name** muestra el nombre por el que se identifica el origen.
**Type** muestra el tipo de repositorio.
**Arg** muestra la dirección URL o la ruta de acceso usada por el origen.
**Data** muestra el nombre del paquete opcional que se usa, si corresponde.
**Updated** muestra la última fecha y hora en que se actualizó el origen.

## <a name="update"></a>actualización

El subcomando **update** fuerza una actualización de un origen individual o de todos.

Uso: `winget source update [-n, --name] \<name>`

### <a name="update-all"></a>update all

El subcomando **update** por sí solo solicitará y actualizará cada repositorio. Por ejemplo: `C:\winget update`

### <a name="update-source"></a>origen de la actualización

El subcomando **update**, combinado con la opción **--name**, puede dirigirse y actualizarse a un origen individual. Por ejemplo: `C:\winget source update --name contoso`

## <a name="remove"></a>quitar

El subcomando **remove** quita un origen. Este subcomando requiere la opción **--name** y el **argumento de name** para identificar el origen.

Uso: `winget source add [-n, --name] \<name>`

Por ejemplo: `winget source remove --name Contoso`

## <a name="reset"></a>reset

El subcomando **reset** restablece el cliente a su configuración original. El subcomando **reset** quita todos los orígenes y establece el origen en el predeterminado. Este subcomando solo debe usarse en contadas ocasiones.

Uso: `winget source reset`

Por ejemplo: `winget source reset`

## <a name="default-repository"></a>Repositorio predeterminado

El Administrador de paquetes de Windows especifica un repositorio predeterminado. Puedes identificar el repositorio mediante el comando **list**. Por ejemplo: `winget source list`

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar aplicaciones](index.md)

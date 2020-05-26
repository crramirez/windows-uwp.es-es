---
title: Comando validate de winget
description: Valida un archivo de manifiesto para enviar software al Repositorio de manifiestos de paquete de la comunidad Microsoft en GitHub.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824936"
---
# <a name="validate-command-winget"></a>Comando validate (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

El comando **validate** de la herramienta [winget](index.md) valida un [archivo de manifiesto](../package/manifest.md) para enviar software al **Repositorio de manifiestos de paquete de la comunidad Microsoft** en GitHub. El manifiesto debe ser un archivo YAML que siga la [especificación](https://github.com/microsoft/winget-pkgs/YamlSpec.md).

## <a name="usage"></a>Uso

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>Argumentos

Están disponibles los siguientes argumentos.

| Argumento  | Descripción |
|--------------|-------------|
| **--manifest** |  Ruta de acceso al manifiesto que se va a validar. |
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar aplicaciones](index.md)
* [Envío de paquetes al Administrador de paquetes de Windows](../package/index.md)

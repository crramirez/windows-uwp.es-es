---
title: Comando validate de winget
description: Valida un archivo de manifiesto para enviar software al Repositorio de manifiestos de paquete de la comunidad Microsoft en GitHub.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec1f15ef9086c1083430c9bbe55ea52827ae4bfb
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334466"
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

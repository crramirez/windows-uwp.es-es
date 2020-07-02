---
title: comando hash de winget
description: Genera el hash SHA256 para un instalador.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4b6599a2b538829c6d9107b20f5f22d22f646542
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334565"
---
# <a name="hash-command-winget"></a>Comando hash (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

El comando **hash** de la herramienta [winget](index.md) genera el hash SHA256 para un instalador. Este comando se usa si tienes que crear un [archivo de manifiesto](../package/manifest.md) para enviar software al **Repositorio de manifiestos de paquete de la comunidad Microsoft** en GitHub. Además, el comando **hash** también admite la generación de un hash de certificado SHA256 para archivos MSIX.

## <a name="usage"></a>Uso

`winget hash [-f] \<file> [\<options>]`

El subcomando **hash** solo se puede ejecutar en un archivo local. Para usar el subcomando **hash**, descarga el instalador en una ubicación conocida. Después, pasa la ruta de acceso del archivo como argumento al subcomando **hash**.

## <a name="arguments"></a>Argumentos

Están disponibles los siguientes argumentos:

| Argumento  | Descripción |
|--------------|-------------|
| **-f,--file** |  Ruta de acceso al archivo al que se va a aplicar hash. |
| **-m,--msix**  | Especifica que el comando hash también creará SignatureSha256 de SHA256 para su uso con los instaladores de MSIX. |
| **-?, --help** |  Obtiene ayuda adicional sobre este comando. |

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar aplicaciones](index.md)
* [Envío de paquetes al Administrador de paquetes de Windows](../package/index.md)

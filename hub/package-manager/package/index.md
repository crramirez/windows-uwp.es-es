---
title: Envío de paquetes al Administrador de paquetes de Windows
description: Puede usar el Administrador de paquetes de Windows como canal de distribución de los paquetes de software que contienen sus aplicaciones.
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: ae9c9039154e2a576a691a01d64abcf8c9029c1c
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334607"
---
# <a name="submit-packages-to-windows-package-manager"></a>Envío de paquetes al Administrador de paquetes de Windows

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Si eres fabricante de software independiente (ISV), puedes usar el Administrador de paquetes de Windows como canal de distribución de los paquetes de software que contienen tus aplicaciones. Actualmente, el Administrador de paquetes de Windows admite instaladores en los siguientes formatos: MSIX, MSI y EXE.

Para enviar paquetes de software al Administrador de paquetes de Windows, sigue estos pasos:

1. [Crea un manifiesto de paquete que proporcione información sobre tu aplicación](manifest.md). Los manifiestos son archivos YAML que siguen el esquema del Administrador de paquetes de Windows.
2. [Envía el manifiesto al repositorio del Administrador de paquetes de Windows](repository.md). Se trata de un repositorio de código abierto en GitHub, que contiene una colección de manifiestos a los que la herramienta **winget** puede acceder.

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget](../winget/index.md)
* [Creación de un manifiesto de paquete](manifest.md)
* [Envío del manifiesto al repositorio](repository.md)
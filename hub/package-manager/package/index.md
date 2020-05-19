---
title: Envío de paquetes al Administrador de paquetes de Windows
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: e83088c5a6b2755a8ce7f08e513d09f877580db8
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580092"
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
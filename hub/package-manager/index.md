---
title: Administrador de paquetes de Windows
description: El Administrador de paquetes de Windows es una completa solución de administración de paquetes que consta de una herramienta de línea de comandos y un conjunto de servicios para la instalación de aplicaciones en Windows 10.
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5b25f2c651e11a5ff97a630bb802b236771f5441
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334629"
---
# <a name="windows-package-manager-preview"></a>Administrador de paquetes de Windows (versión preliminar)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

El Administrador de paquetes de Windows es una completa [solución de administración de paquetes](#understanding-package-managers) que consta de una herramienta de línea de comandos y un conjunto de servicios para la instalación de aplicaciones en Windows 10.

## <a name="windows-package-manager-for-developers"></a>Administrador de paquetes de Windows para desarrolladores

Los desarrolladores usan la herramienta de línea de comandos **winget** para detectar, instalar, actualizar, quitar y configurar un conjunto seleccionado de aplicaciones. Una vez instalado, los desarrolladores pueden acceder a **winget** a través del Terminal Windows, PowerShell o el símbolo del sistema.

Para más información, consulta [Uso de la herramienta winget para instalar y administrar aplicaciones](winget/index.md).

## <a name="windows-package-manager-for-isvs"></a>Administrador de paquetes de Windows para ISV

Los fabricantes de software independiente (ISV) pueden usar el Administrador de paquetes de Windows como canal de distribución de los paquetes de software que contienen sus herramientas y aplicaciones. Para enviar paquetes de software (que contengan instaladores .msix, .msi o .exe) en el Administrador de paquetes de Windows, ofrecemos el **Repositorio de manifiestos de paquete de la comunidad Microsoft** de código abierto en GitHub, donde los ISV pueden cargar [manifiestos de paquete](package/manifest.md) para que sus paquetes de software se analicen para su inclusión en el Administrador de paquetes de Windows. Los manifiestos se validan automáticamente y también pueden revisarse manualmente.

Para más información, consulta [Envío de paquetes al Administrador de paquetes de Windows](package/repository.md).

## <a name="understanding-package-managers"></a>Descripción de los administradores de paquetes

Un administrador de paquetes es un sistema o conjunto de herramientas que se usa para automatizar la instalación, actualización, configuración y uso de software. La mayoría de los administradores de paquetes están diseñados para detectar e instalar las herramientas de desarrollo.

Idealmente, los desarrolladores usan un administrador de paquetes para especificar los requisitos previos para las herramientas que necesitan para desarrollar soluciones para un proyecto dado. Luego, el administrador de paquetes sigue las instrucciones declarativas para instalar y configurar las herramientas. El administrador de paquetes reduce el tiempo invertido en preparar un entorno y ayuda a garantizar que las mismas versiones de los paquetes se instalen en sus equipos.

Los administradores de paquetes de terceros pueden aprovechar el [Repositorio de manifiestos de paquete de la comunidad Microsoft](package/repository.md) para aumentar el tamaño de su catálogo de software.

## <a name="related-topics"></a>Temas relacionados

* [Uso de la herramienta winget para instalar y administrar paquetes de software](winget/index.md)
* [Envío de paquetes al Administrador de paquetes de Windows](package/index.md)
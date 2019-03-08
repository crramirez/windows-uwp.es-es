---
title: Las pruebas en una única consola de Xbox
description: Obtenga información sobre cómo probar servicios de Xbox Live en la consola de Xbox Live
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, juegos, xbox, xbox live, xbox uno
ms.localizationpriority: low
ms.openlocfilehash: aa14071f5ddc9af778fc59cc20891bf83310088b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628020"
---
# <a name="testing-on-the-xbox-one-console"></a>Las pruebas en la consola Xbox One

Al desarrollar el título para la familia de las consolas Xbox One sólo es natural que desearía poder probar el título y las características de Xbox Live en una consola real. Existen algunas opciones para probar su título en el hardware. Puede usar cualquier retail Xbox una única consola para probar una aplicación o el título de la plataforma Universal de Windows (UWP) mediante la activación de modo de desarrollador de la consola. Esta opción es accesible para todos los desarrolladores y es la única opción para los desarrolladores programa de creadores de Xbox Live. ID@Xbox y socios administrados tienen la opción de ordenación y el uso de un Kit de desarrollo de Xbox.

## <a name="retail-console-testing-xbox-live-creators"></a>Venta por menor de las pruebas de la consola: Creadores de Xbox Live

Modo de programador de activación en una consola Xbox One de venta directa, podrá implementar los títulos UWP y aplicaciones en la consola Xbox uno asociando una compilación de Visual Studio. Se trata de la consola de pruebas de la opción para los desarrolladores programa de creadores de Xbox Live. No podrá probar juegos XDK en una consola Xbox One comercial.

* Siga el [instrucciones de activación del modo de desarrollador](../xbox-apps/devkit-activation.md) para permitir el desarrollo de pruebas en la consola de venta directa.  
* Cargar su título en Xbox One siguiendo el [configurar sus instrucciones de Xbox One](../xbox-apps/development-environment-setup.md#setting-up-your-xbox-one).  
* Siga el [instrucciones de desactivación del modo de desarrollador](../xbox-apps/devkit-deactivation.md) vuelven a poner la consola en modo de venta directa o desinstalar el entorno de desarrollo en la consola de venta directa.  
* Mientras que la consola está en modo de programador puede tener acceso remoto a través de su equipo mediante el uso de la [portal de dispositivos de Windows para Xbox](../debug-test-perf/device-portal-xbox.md).  

## <a name="xbox-development-kit-testing-idxbox-and-managed-partners"></a>Pruebas del kit de desarrollo de Xbox: ID@Xbox y socios administrados

Administrados asociados y ID@Xbox los desarrolladores tienen la opción de comprar un kit de desarrollo de Xbox desde el [Store de desarrollo de Xbox](https://gamedevstore.partners.extranet.microsoft.com/), que solo es accesible a los usuarios con cuentas de desarrollador de aplicaciones administradas. Kits de desarrollo de Xbox, podrá cargar XDK juegos en la consola para probar, juegos UWP también se pueden probar mediante el kit de desarrollo. Kits de desarrollo incluyen opciones de hardware y las características de pruebas que permiten administrar más detallada de las pruebas y la consola de rendimiento.

Para comenzar su viaje con el kit de desarrollo de Xbox leer el [artículo de desarrollo de Xbox una introducción a](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-getting-started) en el sitio de documentación de socio administrado. Esta documentación solo es accesible para los desarrolladores autorizados en el ID@Xbox programa y socios administrados.
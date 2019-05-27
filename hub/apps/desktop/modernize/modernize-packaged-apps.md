---
Description: Obtenga información sobre cómo agregar experiencias modernas para Windows 10 usuarios en una aplicación de escritorio empaquetada en un paquete de aplicación de Windows.
title: Modernizar las aplicaciones de escritorio empaquetadas
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 191a8b8a007a866f37780a7c52cd40047dc9817f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215202"
---
# <a name="features-that-require-package-identity"></a>Características que requieren la identidad del paquete

Si desea actualizar su aplicación de escritorio con [experiencias modernas de Windows 10](index.md), muchas características solo están disponibles en las aplicaciones de escritorio que se empaquetan en un paquete MSIX.

MSIX es un formato de paquete de aplicación de Windows moderno que proporciona una experiencia de empaquetado universal para todas las aplicaciones de Windows, WPF, Windows Forms y aplicaciones de Win32. Empaquetar las aplicaciones de escritorio Windows le permite integrar experiencias modernas de Windows 10 como iconos dinámicos y notificaciones en sus aplicaciones. También se obtiene acceso a una instalación sólida y experiencia de actualización, un modelo de seguridad administrada con un sistema de la capacidad flexible, compatibilidad con la Microsoft Store, administración empresarial y muchos modelos de distribución personalizada. Para más información, consulta [Empaquetar aplicaciones de escritorio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) en la documentación de MSIX.

Si empaqueta su aplicación de escritorio, a continuación, puede usar las API de UWP que requieren la identidad del paquete, empaquete extensiones y componentes UWP en la aplicación empaquetada. Para obtener más información, consulte estos artículos.

## <a name="use-uwp-apis-that-require-package-identity"></a>Use las API de UWP que requieren una identidad de paquete

Algunas API de UWP necesitan [identidad del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) para usarse en una aplicación de escritorio. El paquete MSIX (incluido el manifiesto del paquete) proporciona esta identidad.

Para obtener más información, consulte [esta lista de las API](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integración con extensiones de paquete

Si la aplicación necesita para integrarse con el sistema (por ejemplo: establecer las reglas de firewall), se describen estas cosas en el manifiesto del paquete de la aplicación y el sistema encargará del resto. Para la mayoría de estas tareas, no tendrás que escribir nada de código. Con un poco de XML en el manifiesto, puede hacer cosas como iniciar un proceso cuando el usuario inicia sesión, integrar su aplicación en el Explorador de archivos y agregar la aplicación una lista de destinos de impresión que aparecen en otras aplicaciones.

Para obtener más información, consulte [integrar su aplicación de escritorio con extensiones del paquete](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Ampliación con componentes de UWP

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. En general, primero debe determinar si puede agregar su experiencia por [mejora](desktop-to-uwp-enhance.md) su aplicación de escritorio existente con las API de UWP. Si tiene que usar un componente UWP, para lograr la experiencia, puede agregar un proyecto UWP a la solución y utilizar servicios de aplicaciones para la comunicación entre su aplicación de escritorio y el componente UWP.

Para obtener más información, consulte [extender su aplicación de escritorio con componentes UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuir

Puede distribuir la aplicación mediante la publicación de la Microsoft Store o mediante la instalación de prueba en otros sistemas.

Consulte [distribuir su aplicación de escritorio empaquetado](desktop-to-uwp-distribute.md).

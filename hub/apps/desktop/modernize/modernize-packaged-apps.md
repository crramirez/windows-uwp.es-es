---
Description: Obtenga información sobre cómo agregar experiencias modernas para los usuarios de Windows 10 en una aplicación de escritorio que haya empaquetado en un paquete de aplicación de Windows.
title: Modernización de aplicaciones de escritorio empaquetado
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142514"
---
# <a name="features-that-require-package-identity"></a>Características que requieren la identidad del paquete

Si quiere actualizar su aplicación de escritorio con [experiencias modernas de Windows 10](index.md), muchas características solo están disponibles en las aplicaciones de escritorio que tienen la [identidad del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Hay varias maneras de conceder la identidad del paquete a una aplicación de escritorio:

* Empaquete en un [paquete de MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX es un formato de paquete de aplicaciones moderno que ofrece una experiencia de empaquetado universal para todas las aplicaciones de Windows, WPF, Windows Forms y aplicaciones Win32. Proporciona una experiencia de instalación y actualización sólida, un modelo de seguridad administrado con un sistema de capacidades flexible, compatibilidad con el Microsoft Store, la administración empresarial y muchos modelos de distribución personalizados. Para más información, consulta [Empaquetar aplicaciones de escritorio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) en la documentación de MSIX.
* Si no puede adoptar el empaquetado de MSIX para implementar la aplicación de escritorio, a partir de la compilación de Windows 10 Insider Preview 10.0.19000.0 puede conceder la identidad del paquete creando un *paquete MSIX disperso* que contenga solo un manifiesto del paquete. Para obtener más información, consulte [conceder identidad a aplicaciones de escritorio no empaquetadas](grant-identity-to-nonpackaged-apps.md).

Si la aplicación de escritorio tiene una identidad de paquete, puede usar las siguientes características en la aplicación.

## <a name="use-uwp-apis-that-require-package-identity"></a>Usar API de UWP que requieran la identidad del paquete

La siguiente lista de API de UWP requiere la identidad del paquete que se va a usar en una aplicación de escritorio: [lista de API](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integración con extensiones de paquete

Si la aplicación tiene que integrarse con el sistema (por ejemplo, establecer reglas de firewall), describa esas cosas en el manifiesto del paquete de la aplicación y el sistema hará el resto. Para la mayoría de estas tareas, no tendrás que escribir nada de código. Con un poco de XML en el manifiesto, puede hacer cosas como iniciar un proceso cuando el usuario inicia sesión, integrar la aplicación en el explorador de archivos y agregar a la aplicación una lista de destinos de impresión que aparecen en otras aplicaciones.

Para más información, consulte [integración de la aplicación de escritorio con extensiones de paquete](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Ampliación con componentes de UWP

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. En general, primero debe determinar si puede agregar su experiencia [mejorando](desktop-to-uwp-enhance.md) la aplicación de escritorio existente con las API de UWP. Si tiene que usar un componente de UWP, para lograr la experiencia, puede Agregar un proyecto de UWP a la solución y usar App Services para comunicarse entre su aplicación de escritorio y el componente de UWP.

Para obtener más información, consulte [extensión de la aplicación de escritorio con componentes de UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuir

Si empaqueta la aplicación en un paquete MSIX, puede distribuirla mediante su publicación en el Microsoft Store o mediante la instalación de prueba en otros sistemas.

Consulte [distribuir la aplicación de escritorio empaquetada](desktop-to-uwp-distribute.md).

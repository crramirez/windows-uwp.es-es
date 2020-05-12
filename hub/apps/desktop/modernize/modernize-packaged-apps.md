---
Description: Obtén información sobre cómo agregar experiencias modernas para los usuarios de Windows 10 en una aplicación de escritorio que ha empaquetado en un paquete de aplicación de Windows.
title: Modernización de aplicaciones de escritorio empaquetadas
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1930d879177bc9282a3b55d019aa2bef7eb8f120
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730084"
---
# <a name="features-that-require-package-identity"></a>Características que requieren la identidad del paquete

Si quieres actualizar la aplicación de escritorio con [experiencias modernas de Windows 10](index.md), muchas características solo están disponibles en las aplicaciones de escritorio que tienen [identidad de paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Hay varias maneras de conceder identidad de paquete a una aplicación de escritorio:

* Empaquetarla en un [paquete de MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX es un formato moderno de paquete de la aplicación que proporciona una experiencia de empaquetado universal para todas las aplicaciones Windows, WPF, Windows Forms y Win32. Proporciona una sólida experiencia de instalación y actualización, un modelo de seguridad administrado con un sistema de funcionalidades flexible, compatibilidad con Microsoft Store, administración empresarial y muchos modelos de distribución personalizados. Para más información, consulta [Empaquetar aplicaciones de escritorio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) en la documentación de MSIX.
* Si no puedes adoptar el empaquetado de MSIX para implementar la aplicación de escritorio, a partir de la compilación de Windows 10, versión 2004, puedes conceder la identidad de paquete al crear un *paquete disperso de MSIX* que solo contenga un manifiesto de paquete. Para obtener más información, consulta [Concesión de identidad a aplicaciones de escritorio no empaquetadas](grant-identity-to-nonpackaged-apps.md).

Si la aplicación de escritorio tiene una identidad de paquete, puedes usar las siguientes características en la aplicación.

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>Uso de las API de Windows Runtime que requieren la identidad del paquete

La siguiente lista de API de Windows Runtime requieren que se use la identidad de paquete: [lista de API](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integración con extensiones de paquete

Si la aplicación se debe integrar con el sistema (por ejemplo, establecer reglas de firewall), describe estas cuestiones en el manifiesto del paquete de la aplicación y el sistema se encargará del resto. Para la mayoría de estas tareas, no tendrás que escribir nada de código. Con un poco de XML en el manifiesto, puedes hacer varias cosas; por ejemplo, puedes iniciar un proceso cuando el usuario inicie sesión, integrar la aplicación en el Explorador de archivos y agregarla a una lista de los destinos de impresión que aparecen en otras aplicaciones.

Para obtener más información, consulta [Integración de aplicaciones de escritorio con extensiones de paquete](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Ampliación con componentes de UWP

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. En general, primero debes determinar si puedes agregar tu experiencia [mejorando](desktop-to-uwp-enhance.md) la aplicación de escritorio existente con las API de Windows Runtime. Si tienes que usar un componente de UWP para lograr la experiencia, puedes agregar un proyecto de UWP a la solución y usar los servicios de aplicaciones para comunicarse entre tu aplicación de escritorio y el componente de UWP.

Para obtener más información, consulta [Ampliación de la aplicación de escritorio con componentes de UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribución

Si empaquetas la aplicación en un paquete MSIX, puedes distribuirla publicándola en Microsoft Store o transfiriéndola localmente a otros sistemas.

Consulta [Distribución de la aplicación de escritorio empaquetada](desktop-to-uwp-distribute.md).

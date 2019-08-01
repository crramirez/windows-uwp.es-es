---
Description: Vea los detalles relacionados con la identidad única asignada a la aplicación por el Microsoft Store y obtenga un vínculo a la lista de la tienda de la aplicación.
title: Ver detalles de identidad de las aplicaciones
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bd9f61aa32c7e4b78603af37947b39a44e705015
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682639"
---
# <a name="view-app-identity-details"></a>Ver detalles de identidad de las aplicaciones


Puede ver los detalles relacionados con la identidad única asignada a la aplicación por el Microsoft Store en sus páginas de identidad de la **aplicación** . También puede obtener un vínculo a la lista de tiendas de la aplicación en esta página.

Para encontrar esta información, ve a una de las aplicaciones y expande **Administración de aplicaciones** en el menú de navegación izquierdo. Selecciona **Identidad de aplicación** para ver los detalles.


## <a name="values-to-include-in-your-app-package-manifest"></a>Valores que se deben incluir en el manifiesto del paquete de la aplicación

Los valores siguientes se deben incluir en el manifiesto del paquete. Si [usas Microsoft Visual Studio para compilar los paquetes](/windows/msix/package/packaging-uwp-apps) y has iniciado sesión con la misma cuenta de Microsoft asociada a tu cuenta de desarrollador, los detalles se incluyen automáticamente. Si vas a compilar el paquete manualmente, debes agregar estos elementos:

-   **Paquete/identidad/nombre**
-   **Paquete/identidad/publicador**
-   **Paquete/propiedades/PublisherDisplayName**

Para obtener más información, consulta [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) en la [referencia del esquema del manifiesto del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Juntos, estos elementos declaran la identidad de la aplicación y establecen la "familia del paquete" a la que pertenecen todos sus paquetes. Los paquetes individuales tendrán detalles adicionales, como la versión y la arquitectura.


## <a name="additional-values-for-package-family"></a>Valores adicionales para la familia del paquete

Los siguientes valores son valores adicionales que hacen referencia a la familia del paquete de la aplicación, pero que no se incluyen en el manifiesto.

-   **Nombre de familia de paquete (PFN)** : Este valor se usa con ciertas API de Windows.
-   **SID del paquete**: Necesitará este valor para enviar notificaciones WNS a la aplicación. Para más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Vincular a la descripción de la aplicación

El vínculo directo a la página de la aplicación se puede compartir para que los clientes encuentren la aplicación en la Tienda. Este vínculo tiene el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.

El **Id. de la Tienda** de la aplicación también se muestra en esta sección. Este id. de la Tienda se puede usar para [generar distintivos de la Tienda](https://go.microsoft.com/fwlink/p/?LinkId=534236) o para identificar de otro modo la aplicación.

El **vínculo al protocolo de la Tienda** puede usarse para vincular directamente a tu aplicación en la Tienda sin tener que abrir un navegador, como cuando se vincula desde dentro de una aplicación. Para obtener más información, consulta [Vincular a la aplicación](link-to-your-app.md).



 

 





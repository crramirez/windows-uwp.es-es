---
description: Vea los detalles relacionados con la identidad única asignada a la aplicación por el Microsoft Store y obtenga un vínculo a la lista de la tienda de la aplicación.
title: Ver detalles de identidad de las aplicaciones
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11984d1aa5f8da20b529ab485615bd1f760cc2cd
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034908"
---
# <a name="view-app-identity-details"></a>Ver detalles de identidad de las aplicaciones


Puede ver los detalles relacionados con la identidad única asignada a la aplicación por el Microsoft Store en sus páginas de identidad de la **aplicación** . También puede obtener un vínculo a la lista de tiendas de la aplicación en esta página.

Para encontrar esta información, ve a una de las aplicaciones y expande **Administración de aplicaciones** en el menú de navegación izquierdo. Seleccione **identidad** de la aplicación para ver estos detalles.


## <a name="values-to-include-in-your-app-package-manifest"></a>Valores que se van a incluir en el manifiesto del paquete de la aplicación

Los valores siguientes se deben incluir en el manifiesto del paquete. Si [usa Microsoft Visual Studio para compilar los paquetes](/windows/msix/package/packaging-uwp-apps)y ha iniciado sesión con la misma cuenta de Microsoft que ha asociado a su cuenta de desarrollador, estos detalles se incluyen automáticamente. Si va a compilar el paquete manualmente, deberá agregar estos elementos:

-   **Paquete/identidad/nombre**
-   **Paquete/identidad/publicador**
-   **Paquete/propiedades/PublisherDisplayName**

Para obtener más información, consulta [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) en la [referencia del esquema del manifiesto del paquete](/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Juntos, estos elementos declaran la identidad de la aplicación y establecen la "familia del paquete" a la que pertenecen todos sus paquetes. Los paquetes individuales tendrán detalles adicionales, como la versión y la arquitectura.


## <a name="additional-values-for-package-family"></a>Valores adicionales para la familia del paquete

Los siguientes valores son valores adicionales que hacen referencia a la familia del paquete de la aplicación, pero que no se incluyen en el manifiesto.

-   **Nombre de familia de paquete (PFN)** : este valor se usa con ciertas API de Windows.
-   **Package SID** : necesitarás este valor para enviar notificaciones de WNS a la aplicación. Para más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Vincular a la descripción de la aplicación

Se puede compartir el vínculo directo a la página de la aplicación para ayudar a los clientes a encontrar la aplicación en la tienda. Este vínculo está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Cuando un cliente hace clic en este vínculo, se abre la página de lista basada en Web de la aplicación. En los dispositivos Windows, la aplicación de la tienda también iniciará y mostrará la lista de la aplicación.

El **Id. de la Tienda** de la aplicación también se muestra en esta sección. Este id. de la Tienda se puede usar para [generar distintivos de la Tienda](https://developer.microsoft.com/store/badges) o para identificar de otro modo la aplicación.

El **vínculo del Protocolo de almacenamiento** se puede usar para vincular directamente a la aplicación en la tienda sin necesidad de abrir un explorador, como cuando se vincula desde una aplicación. Para obtener más información, consulte [vincular a la aplicación](link-to-your-app.md).



 

 

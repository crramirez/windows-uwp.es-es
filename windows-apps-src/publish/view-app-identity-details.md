---
author: jnHs
Description: "Al trabajar con una aplicación en el panel del Centro de desarrollo de Windows, puedes ver los detalles relacionados con la identidad única que la Tienda Windows asigna a la aplicación y obtener un vínculo a la descripción de la aplicación en la Tienda."
title: Ver detalles de identidad de las aplicaciones
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b48f99d4146bfa5e4d9b2af3184e1ce3c1aea491
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="view-app-identity-details"></a>Ver detalles de identidad de las aplicaciones


Al trabajar con una aplicación en el panel del Centro de desarrollo de Windows, puedes ver los detalles relacionados con la identidad única que la Tienda Windows asigna a la aplicación y obtener un vínculo a la descripción de la aplicación en la Tienda.

Para encontrar esta información, ve a una de las aplicaciones y expande **Administración de aplicaciones** en el menú de navegación izquierdo. Haz clic en **Identidad de aplicación** para ver los detalles.

> **Nota** Debes tener un [nombre reservado](create-your-app-by-reserving-a-name.md) para la aplicación, para ver la mayoría de estos detalles de identidad.

## <a name="values-to-include-in-your-appx-manifest"></a>Valores que se deben incluir en el manifiesto APPX


Los valores siguientes se deben incluir en el manifiesto APPX. Si está usando Microsoft Visual Studio para compilar los paquetes y has iniciado la sesión con la misma cuenta de Microsoft asociada a tu cuenta de desarrollador, los detalles se incluyen automáticamente. Si vas a compilar el paquete manualmente, debes agregar los valores.

-   **Paquete/identidad/nombre**
-   **Paquete/identidad/publicador**

Para obtener más información, consulta [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441) en la [referencia del esquema del manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/br211473).

Juntos, estos elementos declaran la identidad de la aplicación y establecen la "familia del paquete" a la que pertenecen todos sus paquetes. Los paquetes individuales tendrán detalles adicionales, como la versión y la arquitectura.

## <a name="additional-values-for-package-family"></a>Valores adicionales para la familia del paquete


Los siguientes valores son valores adicionales que hacen referencia a la familia del paquete de la aplicación, pero que no se incluyen en el manifiesto.

-   **Nombre de familia de paquete (PFN)**: este valor se usa con ciertas API de Windows.
-   **SID de paquete**: necesitarás este valor para enviar notificaciones de WNS a la aplicación. Para más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

## <a name="link-to-your-apps-listing"></a>Vincular a la descripción de la aplicación

El vínculo a la página de la aplicación se puede compartir para que los clientes encuentren la aplicación en la Tienda. Este vínculo tiene el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

> **Nota** Esta dirección URL funcionará en cualquier versión del sistema operativo que tenga la aplicación disponible. También puedes ver vínculos adicionales para Windows 8.1 y versiones anteriores o para Windows Phone 8.1 y versiones anteriores, aunque solo funcionarán para las versiones de sistema operativo especificadas.

Cuando un cliente hace clic en este vínculo, se abrirá la página de descripción basada en web de la aplicación. Si la aplicación está disponible para el dispositivo Windows del cliente, la aplicación de la Tienda también se inicia y muestra la sinopsis de la aplicación.

El **Id. de la Tienda** de la aplicación también se muestra en esta sección. Este id. de la Tienda se puede usar para [generar distintivos de la Tienda](http://go.microsoft.com/fwlink/p/?LinkId=534236) o para identificar de otro modo la aplicación.

 

 





---
Description: Al trabajar con una aplicación en el panel del Centro de desarrollo de Windows, puedes ver los detalles relacionados con la identidad única que la Tienda Windows asigna a la aplicación y obtener un vínculo a la descripción de la aplicación en la Tienda.
title: Ver detalles de identidad de las aplicaciones
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
---

# Ver detalles de identidad de las aplicaciones


Al trabajar con una aplicación en el panel del Centro de desarrollo de Windows, puedes ver los detalles relacionados con la identidad única que la Tienda Windows asigna a la aplicación y obtener un vínculo a la descripción de la aplicación en la Tienda.

Para encontrar esta información, ve a una de las aplicaciones y expande **Administración de aplicaciones** en el menú de navegación izquierdo. Haz clic en **Identidad de aplicación** para ver los detalles.

> **Note**  Debes tener un [nombre reservado](create-your-app-by-reserving-a-name.md) para la aplicación para ver la mayoría de los detalles de identidad.

## Valores que se deben incluir en el manifiesto APPX


Los valores siguientes se deben incluir en el manifiesto APPX. Si está usando Microsoft Visual Studio para compilar los paquetes y has iniciado la sesión con la misma cuenta de Microsoft asociada a tu cuenta de desarrollador, los detalles se incluyen automáticamente. Si vas a compilar el paquete manualmente, debes agregar los valores.

-   **Paquete/identidad/nombre**
-   **Paquete/identidad/publicador**

Para obtener más información, consulta [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441) en la [referencia del esquema del manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/br211473).

Juntos, estos elementos declaran la identidad de la aplicación y establecen la "familia del paquete" a la que pertenecen todos sus paquetes. Los paquetes individuales tendrán detalles adicionales, como la versión y la arquitectura.

## Valores adicionales para la familia del paquete


Los siguientes valores son valores adicionales que hacen referencia a la familia del paquete de la aplicación, pero que no se incluyen en el manifiesto.

-   **Nombre de familia de paquete (PFN)**: este valor se usa con ciertas API de Windows.
-   **Package SID**: necesitarás este valor para enviar notificaciones de WNS a la aplicación. Para más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

## Vincular a la descripción de la aplicación


El vínculo a la página de la aplicación se puede compartir para que los clientes encuentren la aplicación en la Tienda. Este vínculo tiene el formato **`https://www.microsoft.com/store/apps/<your app's Product ID>`**.

> **Note**  Según las versiones de los sistemas operativos que te interesen, puede que veas más de un vínculo. Todas las aplicaciones muestran la dirección URL para Windows 10 usando el formato indicado arriba, que funciona para cualquier sistema operativo. Puedes ver vínculos adicionales para Windows 8.1 y versiones anteriores o para Windows Phone 8.1 y versiones anteriores, aunque solo funcionarán para las versiones de sistema operativo especificadas.

Cuando un cliente hace clic en este vínculo, se abrirá la página de descripción basada en web de la aplicación. Si la aplicación está disponible para el dispositivo Windows del cliente, la aplicación de la Tienda también se inicia y muestra la sinopsis de la aplicación.

 

 






<!--HONumber=Mar16_HO1-->



---
Description: Descubre cómo se ponen los paquetes de la aplicación a disposición de los clientes y cómo se administran escenarios de paquetes específicos.
title: Orientación para administrar paquetes de la aplicación
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5ecd8cc96196c31615eac032183956de3bee9e4b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171059"
---
# <a name="guidance-for-app-package-management"></a>Orientación para administrar paquetes de la aplicación

Descubre cómo se ponen los paquetes de la aplicación a disposición de los clientes y cómo se administran escenarios de paquetes específicos.

-   [Versiones del sistema operativo y distribución de paquetes](#os-versions-and-package-distribution)
-   [Agregar paquetes para Windows 10 a una aplicación publicada anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Quitar una aplicación de la Tienda](#removing-an-app-from-the-store)
-   [Quitar paquetes de una familia de dispositivos anteriormente compatibles](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versiones del sistema operativo y distribución de paquetes

Distintos sistemas operativos pueden ejecutar distintos tipos de paquetes. Si se puede ejecutar más de un paquete en el dispositivo de un cliente, el Microsoft Store proporcionará la mejor coincidencia disponible.

En general, un sistema operativo puede ejecutar los paquetes destinados a versiones anteriores del sistema operativo dentro de la misma familia de dispositivos. Los dispositivos Windows 10 pueden ejecutar todas las versiones anteriores del sistema operativo admitidas (por familia de dispositivos). Los dispositivos de escritorio de Windows 10 pueden ejecutar aplicaciones creadas para Windows 8.1 o Windows 8; los dispositivos móviles de Windows 10 pueden ejecutar aplicaciones creadas para Windows Phone 8.1, Windows Phone 8 e incluso Windows Phone 7.x. Sin embargo, los clientes de Windows 10 solo obtendrán esos paquetes si la aplicación no incluye los paquetes UWP que se dirigen a la familia de dispositivos aplicable.

> [!IMPORTANT]
> Ya no se pueden cargar nuevos paquetes XAP compilados con los SDK de Windows Phone 8. x. Las aplicaciones que ya se encuentran en el almacén con paquetes XAP seguirán funcionando en dispositivos Windows 10 Mobile. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Quitar una aplicación de la Tienda

En ocasiones, es posible que desee dejar de ofrecer una aplicación a los clientes y "cancelar su publicación". Para ello, haga clic en **crear aplicación no disponible** en la página **información general** de la aplicación. Después de confirmar que desea que la aplicación no esté disponible, en unas horas dejará de estar visible en el almacén y ningún cliente nuevo podrá obtenerla (a menos que tenga un [código promocional](generate-promotional-codes.md) y esté usando un dispositivo de Windows 10).

> [!IMPORTANT]
> Esta opción invalidará la configuración de [visibilidad](choose-visibility-options.md#discoverability) que haya seleccionado en los envíos. 

Esta opción tiene el mismo efecto que si creara un envío y elegir **hacer que este producto esté disponible pero no se puede detectar en el almacén** con la opción **detener adquisición** . Sin embargo, no es necesario que cree un nuevo envío.

Tenga en cuenta que los clientes que ya tienen la aplicación podrán seguir utilizándola y pueden descargarla de nuevo (y incluso obtener actualizaciones si envía nuevos paquetes más adelante).

Después de hacer que la aplicación no esté disponible, la verá en el centro de Partners. Si decides volver a ofrecer la aplicación a los clientes, puedes hacer clic en **Make app available** en la página de información general de la aplicación. Después de confirmar la acción, la aplicación estará disponible para los nuevos clientes (a menos que la configuración de tu último envío lo impida) en cuestión de horas.

> [!NOTE]
> Si quiere mantener la aplicación disponible, pero no desea continuar ofreciéndola a nuevos clientes en una versión de SO determinada, puede crear un nuevo envío y quitar todos los paquetes de la versión del sistema operativo en la que desea evitar nuevas adquisiciones. Por ejemplo, si anteriormente tenía paquetes para Windows Phone 8,1 y Windows 10, y no desea seguir ofreciendo la aplicación a los nuevos clientes en Windows Phone 8,1, quite todos los paquetes de Windows Phone 8,1 del envío. Una vez publicada la actualización, ningún cliente nuevo en Windows Phone 8,1 podrá adquirir la aplicación a través de los clientes que ya la tienen. Sin embargo, la aplicación seguirá estando disponible para los nuevos clientes en Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Quitar paquetes de una familia de dispositivos anteriormente compatibles

Si quita todos los paquetes de una determinada [familia de dispositivos](/uwp/extension-sdks/device-families-overview) que la aplicación admitía anteriormente, se le pedirá que confirme que es su intención antes de guardar los cambios en la página **paquetes** .

Al publicar un envío que quita todos los paquetes que podrían ejecutarse en una familia de dispositivos que la aplicación admitía previamente, los nuevos clientes no podrán adquirir la aplicación en esa familia de dispositivos. Siempre puedes publicar otra actualización más adelante para proporcionar paquetes para esa familia de dispositivos de nuevo.

Ten en cuenta que aunque quites todos los paquetes que admitan una determinada familia de dispositivos, los clientes existentes que ya hayan instalado la aplicación en ese tipo de dispositivo pueden seguir usándola, y obtendrán las actualizaciones que proporciones más adelante.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Agregar paquetes para Windows 10 a una aplicación publicada anteriormente

Si tiene una aplicación en la tienda que solo incluye paquetes para Windows 8. x y/o Windows Phone 8. x y quiere actualizar la aplicación para Windows 10, cree un nuevo envío y agregue los paquetes UWP. msixupload o. appxupload durante el paso [paquetes](upload-app-packages.md) . Una vez que la aplicación pasa por el proceso de certificación, el paquete UWP también estará disponible para las nuevas adquisiciones de los clientes en Windows 10.

> [!NOTE]
> Una vez que un cliente de Windows 10 obtiene el paquete de UWP, no puede volver a poner el cliente en el uso de un paquete para cualquier versión anterior del sistema operativo. 

Tenga en cuenta que el número de versión de los paquetes de Windows 10 debe ser mayor que el de los paquetes de Windows 8, Windows 8.1 o Windows Phone 8,1 que haya usado. Para obtener más información, consulte [numeración](package-version-numbering.md)de la versión del paquete.

Para obtener más información sobre cómo empaquetar aplicaciones para UWP para la tienda, consulte [empaquetar aplicaciones](../packaging/index.md).
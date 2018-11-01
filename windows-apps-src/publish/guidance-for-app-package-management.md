---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: Instrucciones para la administración de paquetes de la aplicación
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e625522b0e9fd03fda49eb28bbedb20c00c15634
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5873163"
---
# <a name="guidance-for-app-package-management"></a>Orientación para administrar paquetes de la aplicación

Descubre cómo se ponen los paquetes de la aplicación a disposición de los clientes y cómo se administran escenarios de paquetes específicos.

-   [Versiones del sistema operativo y distribución de paquetes](#os-versions-and-package-distribution)
-   [Agregar paquetes para Windows 10 a una aplicación publicada anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Mantener la compatibilidad de paquete para Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Quitar una aplicación de la Tienda](#removing-an-app-from-the-store)
-   [Quitar paquetes de una familia de dispositivos anteriormente compatibles](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versiones del sistema operativo y distribución de paquetes

Distintos sistemas operativos pueden ejecutar distintos tipos de paquetes. Si en el dispositivo de un cliente puede ejecutarse más de uno de los paquetes, Microsoft Store proporcionará el más apropiado.

En general, las versiones más posteriores del sistema operativo pueden ejecutar paquetes destinados a versiones anteriores del sistema operativo de la misma familia de dispositivos. Dispositivos de Windows 10 pueden ejecutar todas las versiones anteriores compatibles del sistema operativo (por familia de dispositivos). Dispositivos de escritorio de Windows 10 pueden ejecutar las aplicaciones que se han creado para Windows8.1 o Windows8; Los dispositivos móviles Windows 10 pueden ejecutar las aplicaciones que se han creado para Windows Phone 8.1, WindowsPhone8 e incluso de Windows Phone 7.x. Sin embargo, los clientes de Windows 10 solo obtendrán esos paquetes si la aplicación no incluye paquetes para UWP destinada a la familia de dispositivos aplicables.

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, productos creados recientemente no pueden incluir paquetes destinados a 8.x/Windows de Windows Phone 8.x o versiones anteriores. Para obtener más información, consulta este [blog post](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/).


## <a name="removing-an-app-from-the-store"></a>Quitar una aplicación de Store

En ocasiones, es posible que quieras dejar de ofrecer una aplicación a los clientes, es decir, "dejar de publicarla". Para ello, haz clic en **Make app unavailable** en la página de **Información general de la aplicación**. Después de confirmar que quieres que la aplicación deje de estar disponible, en el plazo de unas horas dejará de estar visible en Microsoft Store y ningún cliente nuevo podrá acceder a ella (a no ser que tengan un [código promocional](generate-promotional-codes.md) y usen un dispositivo Windows 10).

> [!IMPORTANT]
> Esta opción reemplazará cualquier configuración de [visibilidad](choose-visibility-options.md#discoverability) que tengas seleccionada en los envíos. 

Esta opción tiene el mismo efecto que si hubieras creado un envío y hubieras elegido **Hacer este producto disponible, pero no detectable, en Store** con la opción **Detener adquisición**. Sin embargo, no requiere crear un nuevo envío.

Ten en cuenta que los clientes que ya tengan la aplicación podrán seguir usándola y podrás volver a descargarla (y podrían incluso recibir actualizaciones si envías nuevos paquetes más adelante).

Después de que la aplicación está disponible, aún tendrás verla en el centro de partners. Si decides volver a ofrecer la aplicación a los clientes, puedes hacer clic en **Make app available** en la página de información general de la aplicación. Después de confirmar la acción, la aplicación estará disponible para los nuevos clientes (a menos que la configuración de tu último envío lo impida) en cuestión de horas.

> [!NOTE]
> Si quieres que tu aplicación siga estando disponible, pero no quieres seguir ofreciéndola a los nuevos clientes con una versión de sistema operativo determinada, puedes crear un nuevo envío y quitar todos los paquetes de la versión de sistema operativo para la que quieres impedir nuevas compras. Por ejemplo, si anteriormente tenías paquetes para Windows Phone 8.1 y Windows 10 y no quieres seguir ofreciendo la aplicación a los clientes nuevos de WindowsPhone8.1, quita todos los paquetes de WindowsPhone8.1 del envío. Después se publique la actualización, ningún cliente nuevo en WindowsPhone8.1 no podrá comprar la aplicación, aunque los clientes que ya la tengan podrán seguir usándola). Sin embargo, la aplicación seguirá estando disponible para los clientes nuevos de Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Quitar paquetes de una familia de dispositivos anteriormente compatibles

Si quitas todos los paquetes de un determinado [familia de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) que anteriormente compatible con la aplicación, se te pedirá para confirmar que esta es tu intención antes de poder guardar los cambios en la página de **paquetes** .

Al publicar un envío que quite todos los paquetes que podrían ejecutarse en una familia de dispositivos que era compatible con la aplicación, los clientes nuevos no podrán comprar la aplicación en esa familia. Siempre puedes publicar otra actualización más adelante para proporcionar paquetes para esa familia de dispositivos de nuevo.

Ten en cuenta que aunque quites todos los paquetes que admitan una determinada familia de dispositivos, los clientes existentes que ya hayan instalado la aplicación en ese tipo de dispositivo pueden seguir usándola, y obtendrán las actualizaciones que proporciones más adelante.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Agregar paquetes para Windows 10 a una aplicación publicada anteriormente

Si tienes una aplicación de la tienda que solo se incluye paquetes para Windows 8.x o Windows Phone 8.x y quieres actualizarla para Windows 10, crea un nuevo envío y agrega los paquetes de .msixupload o .appxupload para UWP durante el paso de [los paquetes](upload-app-packages.md) . Después de que la aplicación pase por el proceso de certificación, el paquete para UWP también estará disponible para nuevas adquisiciones por los clientes de Windows 10.

> [!NOTE]
> Una vez que un cliente de Windows 10 obtenga el paquete para UWP, no puede revertirlo para que use un paquete para cualquier versión del sistema operativo anterior. 

Ten en cuenta que el número de versión de los paquetes de Windows 10 debe ser mayor que los paquetes Windows8, Windows8.1 o Windows Phone 8.1 que has usado. Para obtener más información, consulta [Numeración de la versión del paquete](package-version-numbering.md).

Para obtener más información sobre cómo empaquetar aplicaciones para UWP para Microsoft Store, consulta [Empaquetado de aplicaciones](../packaging/index.md).

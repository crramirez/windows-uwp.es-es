---
description: Una vez que los paquetes se hayan cargado correctamente, verá una tabla que indica los paquetes que se ofrecerán a familias específicas de dispositivos Windows 10 (y versiones anteriores del sistema operativo, si procede), en orden clasificado.
title: Disponibilidad de familias de dispositivos
ms.date: 03/21/2019
ms.topic: article
keywords: Windows 10, UWP, paquetes, carga, disponibilidad de la familia de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 48cbb0fd9ecf27c9926d55e22abc17d039d3674b
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2020
ms.locfileid: "89411969"
---
# <a name="device-family-availability"></a>Disponibilidad de familias de dispositivos

Una vez que los paquetes se hayan cargado correctamente en la página **paquetes** , la sección disponibilidad de la **familia de dispositivos** mostrará una tabla que indica los paquetes que se ofrecerán a familias de dispositivos específicos de Windows 10 (y versiones anteriores del sistema operativo, si procede), en orden clasificado. Esta sección también le permite elegir si quiere o no ofrecer el envío a los clientes en familias específicas de dispositivos Windows 10.

> [!NOTE]
> Si aún no ha cargado los paquetes, en la sección disponibilidad de la **familia de dispositivos** se mostrarán las familias de dispositivos de Windows 10 con casillas de verificación que permiten indicar si el envío se ofrecerá a los clientes en esas familias de dispositivos. La tabla aparecerá después de cargar uno o más paquetes.

En esta sección también se incluye una casilla donde puede indicar si desea permitir que Microsoft haga que la aplicación esté disponible para las familias de dispositivos de Windows 10 futuras. Se recomienda dejar esta casilla marcada, de modo que la aplicación esté disponible para más clientes potenciales al incorporarse nuevas familias de dispositivos.


## <a name="choosing-which-device-families-to-support"></a>Elegir qué familias de dispositivos admitir

Si carga paquetes destinados a una familia de dispositivos individuales, seleccionaremos la casilla para que esos paquetes estén disponibles para los nuevos clientes en ese tipo de dispositivo. Por ejemplo, si un paquete tiene como destino Windows. Desktop, el cuadro **escritorio de Windows 10** se comprobará para ese paquete (y no podrá activar las casillas de otras familias de dispositivos).

Los paquetes destinados a la familia de dispositivos Windows. universal se pueden ejecutar en cualquier dispositivo de Windows 10 (incluido Xbox One). De forma predeterminada, estos paquetes estarán disponibles para los nuevos clientes en todos los tipos de dispositivos, *excepto* para Xbox.

Puedes desactivar la casilla para cualquier familia de dispositivos Windows 10 si no quieres ofrecer el envío a los clientes de ese tipo de dispositivo. Si se desactiva la casilla de una familia de dispositivos, los clientes nuevos de ese tipo de dispositivo no podrán conseguir la aplicación (aunque los clientes que ya la tengan pueden seguir usándola y obtendrán cualquier actualización que envíes).

Si la aplicación lo admite, se recomienda mantener activadas todas las casillas, a menos que tenga una razón concreta para limitar los tipos de dispositivos de Windows 10 que pueden adquirir la aplicación. Por ejemplo, si sabe que la aplicación no ofrece una buena experiencia en [Surface Hub](https://developer.microsoft.com/windows/surfacehub) o en [Microsoft HoloLens](https://developer.microsoft.com/mixed-reality), puede desactivar el cuadro **Windows 10 Team** y/o **Windows 10 Holographic** . Esto impide que los clientes nuevos adquieran la aplicación en esos dispositivos. Si más adelante decide que está listo para ofrecerlo a los clientes, puede crear un nuevo envío con los cuadros seleccionados.

<span id="xbox" />

La única familia de dispositivos de Windows 10 que no está activada de forma predeterminada para Windows. los paquetes universales es **Windows 10 Xbox**. Si la aplicación no es un juego (o si se trata de un juego y ha habilitado el [programa de creadores de Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) o ha pasado por el proceso de [aprobación de concepto](../gaming/concept-approval.md) ) y el envío incluye paquetes UWP neutros o x64 compilados con el SDK de Windows 10 versión 14393 o posterior, puede activar la casilla **Xbox de Windows 10** para ofrecer la aplicación a los clientes en Xbox One

> [!IMPORTANT]
> Para que la aplicación se inicie en dispositivos Xbox, debe incluir un paquete neutro o x64 compilado con Windows SDK versión 14393 o posterior. Sin embargo, si activa **Xbox de Windows 10**, el paquete con versión más reciente aplicable a Xbox (es decir, un paquete neutro o x64 que tenga como destino la familia de dispositivos de Xbox o universal) siempre se ofrecerá a los clientes de Xbox, incluso si se ha compilado con una versión anterior del SDK. Por este motivo, es fundamental garantizar que la última versión del paquete aplicable a Xbox esté compilada mediante la versión 14393 o posterior de Windows SDK. Si no es así, verás un mensaje de error que indica que los clientes de Xbox no podrán iniciar la aplicación. 
> 
> Para resolver este error, realice una de las siguientes acciones:
> - Reemplaza los paquetes correspondientes con los nuevos que hayas compilado mediante la versión 14393 o posterior de Windows SDK.
> - Si ya tienes un paquete que admita Xbox y que esté compilado con la versión 14393 (o posterior) de Windows SDK, aumenta su número de versión para que sea el paquete más actualizado del envío.
> - Desactiva la casilla **Windows 10 Xbox**.
>   
> Si aun así no se soluciona el problema, ponte en contacto con el servicio de soporte técnico.

Si va a enviar una aplicación para UWP para Windows 10 IoT Core, no debe realizar cambios en las selecciones predeterminadas después de cargar los paquetes. no hay ninguna casilla independiente para Windows 10 IoT. Para obtener más información sobre la publicación de aplicaciones de IoT Core para UWP, consulte [compatibilidad con Microsoft Store para aplicaciones de UWP de IOT Core](/windows/iot-core/commercialize-your-device/installingandservicing).

Si el envío de una aplicación publicada anteriormente incluye paquetes que se pueden ejecutar en **Windows 8/8.1** y **Windows Phone 8. x y versiones anteriores**, esos paquetes estarán disponibles para los clientes en esas versiones del sistema operativo. Para dejar de ofrecer la aplicación a estos clientes, quite los paquetes correspondientes de su envío.

> [!IMPORTANT]
> Para impedir por completo que una familia de dispositivos de Windows 10 específica Envíe su envío, actualice el elemento [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) en el manifiesto para que solo tenga como destino la familia de dispositivos que desea admitir (es decir, Windows. Mobile o Windows. Desktop), en lugar de pasarlo como el valor Windows. universal (para la familia de dispositivos universal) que Microsoft Visual Studio incluye en el manifiesto de forma predeterminada.

Es importante tener en cuenta que las selecciones realizadas en la sección **disponibilidad** de la familia de dispositivos solo se aplican a las nuevas adquisiciones. Cualquier persona que ya tenga su aplicación puede seguir utilizándola y obtendrá las actualizaciones que envíe, aunque Quite aquí su familia de dispositivos. Esto se aplica incluso a los clientes que adquieren la aplicación antes de actualizar a Windows 10. Por ejemplo, si tiene una aplicación publicada con Windows Phone paquetes 8,1 y agrega un paquete de Windows 10 (UWP) destinado a Windows. la familia de dispositivos universales, los clientes de Windows 10 Mobile con el paquete de Windows Phone 8,1 recibirán una actualización de este paquete de Windows 10 (UWP), aunque haya desactivado la casilla para **Windows 10 Mobile**.

Para obtener más información acerca de las familias de dispositivos, consulte [Programming with EXTENSION SDK](/uwp/extension-sdks/device-families-overview).


## <a name="understanding-ranking"></a>Comprender la clasificación

Además de permitirle indicar qué familias de dispositivos de Windows 10 pueden descargar el envío, la sección disponibilidad de la **familia de dispositivos** muestra los paquetes específicos que estarán disponibles para las distintas familias de dispositivos. Si tienes más de un paquete que se pueda ejecutar en una determinada familia de dispositivos, la tabla indicará el orden en el que se ofrecerán los paquetes, en función de los números de versión de los mismos. Para obtener más información acerca de la manera en que la Tienda clasifica los paquetes según sus números de versión, consulta [Numeración de la versión del paquete](package-version-numbering.md). 

Como ejemplo, supongamos que tienes dos paquetes: Package_A.appxupload y Package_B.appxupload. En una familia de dispositivos determinada si Package_A.appxupload está clasificado en el puesto número 1 y Package_B.appxupload está en el puesto número 2, en el momento en que un cliente que usa un dispositivo de esta familia compra la aplicación, la Tienda intentará entregar el paquete Package_A.appxupload. Si el dispositivo del cliente no puede ejecutar el paquete Package_A.appxupload, la Tienda le ofrecerá el paquete Package_B.appxupload. Si el dispositivo del cliente no puede ejecutar ninguno de los paquetes de esa familia de dispositivos (por ejemplo, si el **MinVersion** que admite la aplicación es mayor que la versión del dispositivo del cliente), el cliente no podrá descargar la aplicación en ese dispositivo.

> [!NOTE]
> Los números de versión de los paquetes. xap (para aplicaciones publicadas anteriormente) no se tienen en cuenta a la hora de determinar qué paquete debe proporcionar un cliente determinado. Por este motivo, si tienes más de un paquete .xap con la misma clasificación, verás un asterisco en lugar de un número y los clientes recibirán cualquiera de los paquetes. Para proporcionar a un cliente un paquete .xap más reciente, asegúrate de quitar el antiguo paquete .xap del nuevo envío.
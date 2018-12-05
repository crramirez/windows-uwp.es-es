---
Description: After your packages have been successfully uploaded, you'll see a table that indicates which packages will be offered to specific Windows 10 device families (and earlier OS versions, if applicable), in ranked order.
title: Disponibilidad de familias de dispositivos
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, paquetes, carga, disponibilidad de familia de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 217a6ab9f25ee533a754138db5cf83c2ac81e3e9
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8695585"
---
# <a name="device-family-availability"></a>Disponibilidad de familias de dispositivos

Una vez hayas cargado los paquetes correctamente en la página **Paquetes**, la sección **Disponibilidad de familias de dispositivos** mostrará una tabla que indica qué paquetes se ofrecerán a familias específicas de dispositivos Windows 10 (y a versiones anteriores del sistema operativo, si procede), ordenados según su clasificación. En esta sección también podrás elegir si quieres o no ofrecer el envío a clientes de familias específicas de dispositivos Windows 10.

> [!NOTE]
> Si todavía no has cargado los paquetes, la sección **Disponibilidad de familias de dispositivos** te mostrará las familias de dispositivos Windows10 con casillas que te permiten indicar si el envío se ofrecerá o no a aquellos clientes de las familias de dispositivos indicadas. La tabla aparecerá después de cargar uno o más paquetes.

Esta sección también incluye una casilla para indicar si quieres permitir que Microsoft ponga la aplicación a disposición de futuras familias de dispositivos Windows 10. Se recomienda dejar esta casilla marcada, de modo que la aplicación esté disponible para más clientes potenciales al incorporarse nuevas familias de dispositivos.


## <a name="choosing-which-device-families-to-support"></a>Elegir qué familias de dispositivos admitir

Si cargas paquetes destinados a una familia de dispositivos individual, activaremos la casilla para que esos paquetes estén disponibles para clientes nuevos en ese tipo de dispositivo. Por ejemplo, si un paquete está destinado a Windows.Desktop, la casilla **escritorio de Windows 10** estará activada para ese paquete (y no podrás activar las casillas para otras familias de dispositivos).

Los paquetes destinados a la familia de dispositivos Windows.Universal se pueden ejecutar en cualquier dispositivo de Windows 10 (incluido Xbox One). De manera predeterminada, pondremos esos paquetes a disposición de los nuevos clientes en todos los tipos de dispositivo *excepto* para Xbox.

Puedes desactivar la casilla para cualquier familia de dispositivos Windows 10 si no quieres ofrecer el envío a los clientes de ese tipo de dispositivo. Si se desactiva la casilla de una familia de dispositivos, los clientes nuevos de ese tipo de dispositivo no podrán conseguir la aplicación (aunque los clientes que ya la tengan pueden seguir usándola y obtendrán cualquier actualización que envíes).

Si tu app las admite, se recomienda dejar estas casillas activadas salvo que tengas un motivo específico para limitar los tipos de dispositivos Windows 10 que pueden comprar tu aplicación. Por ejemplo, si sabes que tu aplicación no ofrece una buena experiencia en [Surface Hub](https://developer.microsoft.com/windows/surfacehub) o [Microsoft HoloLens](https://developer.microsoft.com/windows/mixed-reality), puedes desactivar la casilla **Windows 10 Team** o **Windows 10 Holographic**. Esto impide que los clientes nuevos adquieran la aplicación en esos dispositivos. Si más adelante decides que la aplicación está lista para esos clientes, puedes crear un nuevo envío con las casillas activadas.

<span id="xbox" />

La única familia de dispositivos Windows 10 que no está activada de manera predeterminada para los paquetes Windows.Universal es **Windows 10 Xbox**. Si la aplicación no es un juego (o si es un juego y ya has habilitado el [Programa de creadores de XboxLive](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) o pasado por el proceso de [aprobación de concepto](../gaming/concept-approval.md)) y el envío incluye paquetes para UWP x64 y/o neutrales compilados mediante el SDK de Windows 10 (versión 14393 o posterior), puedes activar la casilla **Windows 10 Xbox** para ofrecer la aplicación a los clientes de Xbox One.

> [!IMPORTANT]
> Para que la aplicación funcione en dispositivos Xbox, debes incluir un paquete x64 o neutral que esté compilado con la versión 14393 o superior de Windows SDK. Sin embargo, si marcas la casilla de **Windows 10 Xbox**, la última versión del paquete que puedes aplicar a Xbox (este paquete x64 o neutral está destinado a la familia de dispositivos de Xbox o Universal) siempre se ofrecerá a los clientes en Xbox, incluso si se compila con una versión anterior del SDK. Por este motivo, es fundamental garantizar que la última versión del paquete aplicable a Xbox esté compilada mediante la versión 14393 o posterior de Windows SDK. Si no es así, verás un mensaje de error que indica que los clientes de Xbox no podrán iniciar la aplicación. 
> 
> Para resolver este error, puedes realizar una de las siguientes acciones:
> - Reemplaza los paquetes correspondientes con los nuevos que hayas compilado mediante la versión 14393 o posterior de Windows SDK.
> - Si ya tienes un paquete que admita Xbox y que esté compilado con la versión 14393 (o posterior) de Windows SDK, aumenta su número de versión para que sea el paquete más actualizado del envío.
> - Desactiva la casilla **Windows 10 Xbox**.
>   
> Si aun así no se soluciona el problema, ponte en contacto con el servicio de soporte técnico.

Si vas a enviar una aplicación para UWP para Windows 10 IoT Core, no debes realizar cambios a las selecciones predeterminadas después de cargar tu paquetes: no hay ninguna casilla de verificación independiente para Windows 10 IoT. Para obtener más información sobre la publicación de aplicaciones para UWP de IoT Core, consulta [soporte técnico de Microsoft Store para aplicaciones para UWP de IoT Core](https://docs.microsoft.com/windows/iot-core/commercialize-your-device/installingandservicing).

Si tu envío para una aplicación publicada anteriormente incluye paquetes que se pueden ejecutar en **Windows 8/8.1** y **de Windows Phone 8.x y versiones anteriores**, dichos paquetes estarán disponibles para los clientes con esas versiones de sistema operativo. Si quieres dejar de ofrecer la aplicación a estos clientes, quita los paquetes correspondientes del envío.

> [!IMPORTANT]
> Para impedir totalmente que una determinada familia de dispositivos de Windows 10 obtenga el envío, actualiza el elemento [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) en el manifiesto como destino la familia de dispositivos que quieres admitir (es decir, Windows.Mobile o Windows.Desktop), en su lugar que dejarlo como el valor Windows.Universal (correspondiente a la familia de dispositivos universal) Microsoft Visual Studio incluye en el manifiesto de manera predeterminada.

Es importante tener en cuenta que las selecciones que realices en la sección **Disponibilidad de familia de dispositivos** solo se aplican a las nuevas adquisiciones. Cualquiera que ya tenga la aplicación puede seguir usándola y obtener cualquier actualización que envíes, aunque quites aquí su familia de dispositivos. Esto se aplica incluso a los clientes que adquieren la aplicación antes de actualizar a Windows 10. Por ejemplo, si tienes una aplicación publicada con los paquetes de Windows Phone 8.1 y agregar un paquete de Windows 10 (UWP) destinadas a la familia de dispositivos Windows.Universal, Windows 10 mobile a los clientes que tenían el paquete de Windows Phone 8.1 se ofrecerá una actualización a esta Windows Paquete de 10 (UWP), incluso si has sin marcar la casilla de **Windows 10 Mobile**.

Para obtener más información sobre las familias de dispositivos, consulta [**Información general sobre las familias de dispositivos**](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview).


## <a name="understanding-ranking"></a>Comprender la clasificación

Además de permitirte indicar qué familias de dispositivos de Windows 10 pueden descargar el envío, la sección de la **disponibilidad de familias de dispositivo** muestra los paquetes específicos que estarán disponibles para distintas familias de dispositivo. Si tienes más de un paquete que se pueda ejecutar en una determinada familia de dispositivos, la tabla indicará el orden en el que se ofrecerán los paquetes, en función de los números de versión de los mismos. Para obtener más información acerca de la manera en que la Store clasifica los paquetes según sus números de versión, consulta [Numeración de la versión del paquete](package-version-numbering.md). 

Como ejemplo, supongamos que tienes dos paquetes: Package_A.appxupload y Package_B.appxupload. En una familia de dispositivos determinada si Package_A.appxupload está clasificado en el puesto número 1 y Package_B.appxupload está en el puesto número 2, en el momento en que un cliente que usa un dispositivo de esta familia compra la aplicación, la Store intentará entregar el paquete Package_A.appxupload. Si el dispositivo del cliente no puede ejecutar el paquete Package_A.appxupload, la Tienda le ofrecerá el paquete Package_B.appxupload. Si el dispositivo del cliente no puede ejecutar ninguno de los paquetes para esa familia (por ejemplo, si admite la **MinVersion** la aplicación es mayor que la versión en el dispositivo del cliente), a continuación, el cliente no podrá descargar la aplicación en ese dispositivo.

> [!NOTE]
> Los números de versión en los paquetes .xap (para aplicaciones publicadas anteriormente) no se consideran al determinar qué paquete proporcionar a un cliente determinado. Por este motivo, si tienes más de un paquete .xap con la misma clasificación, verás un asterisco en lugar de un número y los clientes recibirán cualquiera de los paquetes. Para proporcionar a un cliente un paquete .xap más reciente, asegúrate de quitar el antiguo paquete .xap del nuevo envío.


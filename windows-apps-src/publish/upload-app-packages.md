---
author: jnHs
Description: "La página Paquetes es donde se cargan todos los archivos del paquete (xap, AppX, .appxupload o .appxbundle) de la aplicación que vas a enviar. En este paso puedes cargar paquetes para cualquier sistema operativo de destino de la aplicación."
title: "Cargar paquetes de aplicación"
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 10523df0297013573dad62d32901c597ba850175
ms.lasthandoff: 02/07/2017

---

# <a name="upload-app-packages"></a>Cargar paquetes de aplicación


La página **Paquetes** es donde se cargan todos los archivos del paquete (.appx, .appxupload, .appxbundle o .xap) de la aplicación que vas a enviar. En este paso puedes cargar paquetes para cualquier sistema operativo de destino de la aplicación. Cada vez que un cliente descarga la aplicación, la Tienda le proporciona automáticamente el paquete que mejor se adapta a su dispositivo. Después de cargar los paquetes, verás una tabla que indica [qué paquetes que se ofrecerán a familias específicas de dispositivos Windows 10](#device-family-availability) (y a versiones anteriores del sistema operativo, si procede) ordenados según su clasificación.

Consulta [Requisitos del paquete de la aplicación](app-package-requirements.md) para obtener más información sobre lo que incluye un paquete y cómo debe estructurarse. También debes obtener información sobre [cómo los números de versión pueden afectar a los paquetes que se entregan a clientes específicos](package-version-numbering.md) y [cómo se distribuyen los paquetes a diferentes sistemas operativos](guidance-for-app-package-management.md).

## <a name="uploading-packages-to-your-submission"></a>Cargar paquetes en el envío

Para cargar paquetes, arrástralos en el campo de carga o haz clic para examinar los archivos. La página **Paquetes** te permite cargar archivos .xap, .appx, .appxupload y .appxbundle.

Si has creado algún [paquete piloto](package-flights.md) para tu aplicación, verás una lista desplegable con la opción para copiar los paquetes de uno de los paquetes piloto. Selecciona el paquete piloto que tiene los paquetes que quieres extraer. A continuación, podrás seleccionar varios o todos los paquetes, para incluirlos en este envío.

> **Importante** En Windows 10 siempre debes cargar el archivo .appxupload, no .appx ni .appxbundle. Para más información sobre cómo empaquetar aplicaciones para UWP para la Tienda, consulta [Empaquetar aplicaciones universales de Windows para Windows 10](../packaging/packaging-uwp-apps.md).

Si se detectan problemas con los paquetes durante su validación, deberás quitar el paquete, corregir el problema y, a continuación, intenta cargarlo de nuevo. Para obtener más información, consulta [Resolver errores de carga de paquetes](resolve-package-upload-errors.md).

En otros casos se mostrarán advertencias para informarte de posibles problemas, aunque no se te impedirá que continúes con el envío.

## <a name="device-family-availability"></a>Disponibilidad de familias de dispositivos

Una vez hayas cargado los paquetes, la sección **Disponibilidad de familias de dispositivos** mostrará una tabla que indica qué paquetes se ofrecerán a familias específicas de dispositivos Windows 10 (y a versiones anteriores del sistema operativo, si procede), ordenados según su clasificación. En esta sección también podrás elegir si quieres ofrecer el envío a clientes de familias específicas de dispositivos Windows 10.

> **Nota** Si todavía no has cargado los paquetes, la sección **Disponibilidad de familias de dispositivos** te mostrará las familias de dispositivos Windows 10 con casillas que indicarán si el envío se ofrecerá a aquellos clientes de las familias de dispositivos indicadas. La tabla no aparecerá hasta que cargues los paquetes.

Asimismo, también verás una casilla que puedes usar para indicar si quieres permitir que Microsoft ponga la aplicación a disposición de futuras familias de dispositivos Windows 10. Se recomienda dejar esta casilla marcada, de modo que la aplicación esté disponible para más clientes potenciales al incorporarse nuevas familias de dispositivos.

### <a name="choosing-which-device-families-to-support"></a>Elegir qué familias de dispositivos admitir

Puedes desactivar la casilla para cualquier familia de dispositivos Windows 10 si no quieres ofrecer el envío a los clientes de ese tipo de dispositivo. Si se desactiva la casilla de una familia de dispositivos, los clientes nuevos de ese tipo de dispositivo no podrán conseguir la aplicación (aunque los clientes que ya la tengan pueden seguir usándola y obtendrán cualquier actualización que envíes). 

> **Nota** No hay casillas para las versiones de **Windows 8/8.1** y **Windows Phone 8.x y anterior**. Si el envío incluye paquetes que pueden ejecutarse en esas versiones del sistema operativo, estos paquetes también estarán disponibles para los clientes. Si quieres dejar de ofrecer la aplicación a los clientes que tengan versiones anteriores del sistema operativo, debes quitar los paquetes correspondientes del envío.

Si la aplicación admite familias de dispositivos móviles y de escritorio, te recomendamos que dejes las casillas de **Windows 10 Mobile** y **Escritorio de Windows 10** marcadas a no ser que tengas un motivo específico para limitar los tipos de dispositivos Windows 10 que pueden comprar la aplicación. Por ejemplo, puedes haber creado paquetes universales de Windows, pero sabes que todavía tienes que resolver algunos problemas de la aplicación en dispositivos móviles. Para evitar que los nuevos clientes descarguen la aplicación en dispositivos móviles Windows 10, puedes desmarcar la casilla **Windows 10 Mobile**. Si más adelante decides que la aplicación está lista para dispositivos móviles Windows 10, puedes crear un nuevo envío con la casilla **Windows 10 Mobile** marcada.

Si la aplicación no es un juego (o si es un juego y ya has pasado por el proceso de [aprobación de concepto](../gaming/concept-approval.md)) y el envío incluye paquetes para UWP x64 y/o neutrales compilados mediante el SDK de Windows 10 (versión 14393 o posterior), puedes marcar la casilla **Windows 10 Xbox** para ofrecer la aplicación a los clientes de Xbox. 

> **Importante** Para que la aplicación funcione en dispositivos Xbox, debes incluir un paquete x64 o neutral que esté compilado con la versión 14393 o superior de Windows SDK. Sin embargo, si marcas la casilla de Windows 10 Xbox, la última versión del paquete que puedes aplicar a Xbox (este paquete x64 o neutral está destinado a la familia de dispositivos de Xbox o Universal) siempre se ofrecerá a los clientes en Xbox, incluso si se compila con una versión anterior del SDK. Por este motivo, es fundamental garantizar que la última versión del paquete aplicable a Xbox esté compilada mediante la versión 14393 o posterior de Windows SDK. Si no es así, verás un mensaje de error que indica que los clientes de Xbox no podrán iniciar la aplicación. 
> 
> Para resolver este error, puedes realizar una de las siguientes acciones:
> -    Reemplaza los paquetes correspondientes con los nuevos que hayas compilado mediante la versión 14393 o posterior de Windows SDK.
> -    Si ya tienes un paquete que admita Xbox y que esté compilado con la versión 14393 (o posterior) de Windows SDK, aumenta su número de versión para que sea el paquete más actualizado del envío.
> -    Desactiva la casilla **Windows 10 Xbox**.
>     
> Si aun así no se soluciona el problema, ponte en contacto con el servicio de soporte técnico.

Si ya probaste la aplicación para garantizar que se ejecuta correctamente en Microsoft HoloLens, también puedes marcar la casilla **Windows 10 Holographic** para ofrecer la aplicación a los clientes de HoloLens. Para obtener más información sobre cómo compilar, probar y publicar aplicaciones holográficas, consulta [Windows Holographic Development Overview (Introducción al desarrollo de Windows Holographic)](http://dev.windows.com/holographic/development_overview).

> **Importante** Para impedir totalmente que una determinada familia de dispositivos Windows 10 obtenga la aplicación, debes actualizar el elemento [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) del manifiesto APPX para especificar únicamente como destino la familia de dispositivos compatible (es decir, Windows.Mobile o Windows.Desktop), en lugar de dejar el valor Windows.Universal (correspondiente a la familia de dispositivos universal) que Microsoft Visual Studio incluye de manera predeterminada en el manifiesto APPX.

Es importante tener en cuenta que las selecciones hechas aquí se aplican solo a las nuevas adquisiciones. Cualquiera que ya tenga la aplicación puede seguir usándola y obtener cualquier actualización que envíes, aunque quites aquí su familia de dispositivos. Esto se aplica incluso a los clientes que adquieren la aplicación antes de actualizar a Windows 10. Por ejemplo, si tienes una aplicación publicada con paquetes de Windows Phone 8.1 y más adelante agregas un paquete de Windows 10 (UWP) a la misma aplicación que tiene la familia de dispositivos universal como destino, a los clientes de Windows 10 Mobile que tengan el paquete de Windows Phone 8.1 se les ofrecerá actualizarse a este paquete de Windows 10 (UWP), aunque hayas desmarcado la casilla **Windows 10 Mobile** (ya que no se trata de una nueva compra, sino de una actualización). Sin embargo, si no proporcionas ningún paquete de Windows 10 (UWP) que tenga como destino las familias de dispositivos universal o móvil, tus clientes móviles de Windows 10 permanecerán en el paquete de Windows Phone 8.1.

Para obtener más información acerca de las familias de dispositivos, consulta [Guide to Universal Windows Platform (UWP) apps (Guía de aplicaciones para la Plataforma universal de Windows [UWP])](https://msdn.microsoft.com/library/windows/apps/dn894631) y [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

### <a name="understanding-ranking"></a>Comprender la clasificación

Además de permitirte indicar qué familias de dispositivos Windows 10 pueden descargar el envío, esta sección también te muestra qué paquetes estarán disponibles para ciertas familias de dispositivos. Si tienes más de un paquete que se pueda ejecutar en una determinada familia de dispositivos, la tabla indicará el orden en el que se ofrecerán los paquetes, en función de los números de versión de los mismos. Para obtener más información acerca de la manera en que la Tienda clasifica los paquetes según sus números de versión, consulta [Numeración de la versión del paquete](package-version-numbering.md). 

Como ejemplo, supongamos que tienes dos paquetes: Package_A.appxupload y Package_B.appxupload. En una familia de dispositivos determinada si Package_A.appxupload está clasificado en el puesto número 1 y Package_B.appxupload está en el puesto número 2, en el momento en que un cliente que usa un dispositivo de esta familia compra la aplicación, la Tienda intentará entregar el paquete Package_A.appxupload. Si el dispositivo del cliente no puede ejecutar el paquete Package_A.appxupload, la Tienda le ofrecerá el paquete Package_B.appxupload. Por otro lado, si el dispositivo del cliente no puede ejecutar ninguno de los paquetes para esa familia de dispositivos (por ejemplo, si la versión **MinVersion** que admite la aplicación es mayor que la versión del dispositivo del cliente), no se podrá descargar la aplicación en ese dispositivo.

> **Nota** Los números de versión de los paquetes .xap no se tienen en cuenta al decidir qué paquete proporcionar a un cliente determinado. Por este motivo, si tienes más de un paquete .xap con la misma clasificación, verás un asterisco en lugar de un número y los clientes recibirán cualquiera de los paquetes. Para proporcionar a un cliente un paquete .xap más reciente, asegúrate de quitar el antiguo paquete .xap del nuevo envío.



## <a name="package-details"></a>Detalles del paquete

Una vez que los paquetes se hayan cargado correctamente, los incluiremos en una lista donde estarán agrupados por sistema operativo de destino. Se mostrará el nombre, la versión y la arquitectura del paquete. Para obtener más información como, por ejemplo, los idiomas admitidos, las capacidades de la aplicación y el tamaño de archivo de cada paquete, haz clic en **Mostrar detalles**.

Si estás usando la [mediación de anuncios de Windows](../monetize/use-ad-mediation-to-maximize-revenue.md), también verás un vínculo para configurar la mediación de anuncios de cada paquete.

Si necesitas quitar un paquete de tu envío, haz clic en el vínculo **Quitar** en la parte inferior de la sección **Detalles** de cada paquete.

## <a name="removing-redundant-packages"></a>Quitar paquetes redundantes

Si se detecta que uno o varios de los paquetes son redundantes, se mostrará una advertencia para sugerirte que quites los paquetes redundantes de este envío. Esto sucede a menudo cuando has cargado paquetes previamente y ahora estás proporcionando paquetes de una versión posterior que admiten el mismo conjunto de clientes. En este caso, ningún cliente recibirá el paquete redundante porque hay un paquete mejor (de versión más reciente) para ellos.

Cuando detectemos paquetes redundantes, proporcionaremos una opción para quitarlos de este envío automáticamente. Si así lo prefieres, también puedes quitar paquetes del envío de forma individual.

## <a name="gradual-package-rollout"></a>Lanzamiento gradual del paquete

Si el envío es una actualización de una aplicación publicada anteriormente, verás una casilla denominada **Lanzar gradualmente la actualización después de publicar este envío (solo a clientes de Windows 10)**. Esta opción te permitirá seleccionar un porcentaje de clientes a los que enviar los paquetes, y así podrás supervisar sus comentarios y los datos analíticos para asegurarte de que la actualización es correcta antes de realizar una distribución más amplia. Asimismo, puedes incrementar el porcentaje (o detener la actualización) en cualquier momento, sin tener que crear un nuevo envío. 

Para obtener más información, consulta [Gradual package rollout (Lanzamiento gradual del paquete)](gradual-package-rollout.md).

## <a name="mandatory-update"></a>Actualización obligatoria

Si el envío es una actualización de una aplicación publicada anteriormente, verás una casilla denominada **Hacer que esta actualización sea obligatoria**. Esto te permitirá establecer la fecha y hora para realizar una actualización obligatoria, siempre y cuando hayas usado las API de Windows.Services.Store para permitir que la aplicación, mediante programación, busque actualizaciones de paquetes, las descargue y las instale. La aplicación debe estar destinada a la versión 1607 (o posterior) de Windows 10 para poder usar esta opción.

Para obtener más información, consulta [Download and install package updates for your app (Descargar e instalar actualizaciones del paquete de la aplicación](../packaging/self-install-package-updates.md).

 






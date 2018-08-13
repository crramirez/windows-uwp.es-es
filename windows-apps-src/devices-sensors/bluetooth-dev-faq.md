---
author: msatranjr
title: Preguntas más frecuentes de los desarrolladores de Bluetooth
description: Este artículo contiene respuestas a las preguntas más frecuentes relacionadas con las API de Bluetooth para la UWP.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: c0af6a19e17a62ed82c32e68ea1732e1f51d4641
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "301931"
---
# <a name="bluetooth-developer-faq"></a>Preguntas más frecuentes de los desarrolladores de Bluetooth

Este artículo contiene respuestas a las preguntas más frecuentes sobre las API de Bluetooth para la UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>¿Qué API se debe usar? ¿Bluetooth clásico (RFCOMM) o Bluetooth baja energía (GATT)?
Existen diversas discusiones en línea sobre este tema general por lo que vamos a mantener esta respuesta directamente en la diferencia con respecto a Windows. A continuación presentamos algunas instrucciones generales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Use las API de GATT cuando se comunica con un dispositivo compatible con Bluetooth baja energía. Si se va a usar caso es poco frecuente, bajo ancho de banda o requiere bajo consumo de energía, Bluetooth baja energía es la respuesta. El espacio de nombres principal que incluye esta funcionalidad es [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Cuándo no usar LE Bluetooth**
- Ancho de banda alto, escenarios de alta frecuencia. Si necesita mantener constantemente la sincronización con grandes cantidades de datos, considere el uso de Bluetooth clásico u o incluso WiFi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth clásico (Windows.Devices.Bluetooth.Rfcomm)

Las API de RFCOMM ofrecen a los desarrolladores un Sockets para llevar a cabo la comunicación de estilo del puerto serie y bidireccional. Una vez que tenga una Sockets, los métodos sobre la escritura y lectura de él son bastante estándar. Una implementación de este se presenta en el [ejemplo de Rfcomm Chat](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Cuándo no usar Rfcomm de Bluetooth** 
- Notificaciones. El protocolo GATT Bluetooth tiene un comando específico para esto y dará como resultado considerablemente menos consumo de energía y tiempos de respuesta. 
- Comprobación de la detección de proximidad o la presencia. Mejor usar las [API de anuncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) y conectarse a través de Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>¿Por qué mi dispositivo Bluetooth LE deja de responder tras una desconexión?

La razón más habitual para que esto suceda es porque el dispositivo remoto haya perdido la información de emparejamiento. Muchos de los dispositivos Bluetooth más antiguos no requieren autenticación. Para proteger al usuario, todas las ceremonias de emparejamiento realizadas desde la aplicación Configuración requerirán autenticación, y algunos dispositivos no saben cómo hacerlo. 

A partir de Windows10, versión 1511, los desarrolladores tienen el control de la ceremonia de emparejamiento. La [Device enumeration and pairing sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) (Muestra de enumeración y emparejamiento de dispositivos) detalla los diversos aspectos de la asociación de nuevos dispositivos.

En este ejemplo, iniciamos el emparejamiento con un dispositivo sin usar cifrado. Ten en cuenta que esto solo funcionará si el dispositivo remoto no requiere cifrado ni autenticación para funcionar.

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>¿Tengo que emparejar los dispositivos Bluetooth antes de usarlos?

En el caso de los dispositivos Bluetooth RFCOMM (los clásicos), no es necesario. A partir de Windows10, versión 1607, puedes simplemente consultar si hay dispositivos cercanos y conectarte a ellos. La [RFCOMM Chat Sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (Muestra de chat de RFCOMM) actualizada muestra esta funcionalidad. 

**(14393 e inferior)** Esta característica no está disponible para Bluetooth baja energía (GATT cliente), por lo que aún tendrá a par, ya sea a través de la página configuración o uso de la API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) en pedidos acceso a estos dispositivos.

**(15030 y anterior)** Ya no se necesita el apareamiento de dispositivos Bluetooth. Uso de las nuevas APIs Async, como GetGattServicesAsync y GetCharacteristicsAsync con el fin de consultar el estado actual del dispositivo remoto. Consulte los [documentos de cliente](gatt-client.md) para obtener más detalles. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>¿Cuándo debo emparejar con un dispositivo antes de comunicarse con él?
Por lo general, si necesita un bono a largo plazo, confianza con un dispositivo, emparejar con él (remite al usuario a la página de configuración o uso del dispositivo (enumeración) y las API de emparejamiento). Si necesita simplemente leer información desactiva el dispositivo que está públicamente expuestos (sensor de temperatura o baliza), a continuación, conectar o escuchar los anuncios de sin realizar ningún esfuerzo para emparejar con el dispositivo. Esto evitará problemas de interoperabilidad a largo plazo debido a que un host de dispositivos no admiten el emparejamiento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>¿Todos los dispositivos de Windows son compatibles con periféricos rol?

No: esta es una característica dependientes de hardware, pero se proporciona un método (BluetoothAdapter.IsPeripheralRoleSupported) para consultar si se admite o no.  Dispositivos admitidos actualmente son de Windows Phone en 8992 + y RPi3 (IoT de Windows). 

## <a name="can-i-access-these-apis-from-win32"></a>¿Tener acceso a estas API de Win32?

Sí, deberían funcionar todas estas API. Este blog explica la forma de llamar a [Las API de Windows desde aplicaciones de escritorio](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>¿Esta funcionalidad se supone que existe en *- Aquí SKU insertar -*?

**Bluetooth LE**: Sí, toda la funcionalidad se encuentra en OneCore y debe estar disponible en dispositivos más recientes con una pila Bluetooth LE funcione. 
> Advertencia: Rol periférico es depende del hardware y algunas ediciones de Windows Server no son compatibles con Bluetooth. 

**Bluetooth BR/EDR (clásico)**: existen algunas variaciones pero mucho, disponen de soporte técnico de nivel de perfil muy similar. Vea a los documentos en [RFCOMM](send-or-receive-files-with-rfcomm.md) y estos documentos perfiles compatibles para [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) y [teléfono](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)


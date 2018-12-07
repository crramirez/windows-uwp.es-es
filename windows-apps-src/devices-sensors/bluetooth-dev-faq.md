---
title: Preguntas más frecuentes de los desarrolladores de Bluetooth
description: Este artículo contiene respuestas a las preguntas más frecuentes relacionadas con las API de Bluetooth para la UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 4cc1bafb90b20083d55a622873dea7be5efbf5b7
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8789516"
---
# <a name="bluetooth-developer-faq"></a>Preguntas más frecuentes de los desarrolladores de Bluetooth

Este artículo contiene respuestas a las preguntas más frecuentes sobre las API de Bluetooth para la UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>¿Qué API debo usar? ¿Bluetooth clásico (RFCOMM) o Bluetooth Low Energy (GATT)?
Hay varias discusiones en línea alrededor de este tema general así que vamos a mantener esta respuesta perfectamente en la diferencia con respecto a Windows. Estas son algunas directrices generales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Usa las API de GATT cuando te estás comunicando con un dispositivo compatible con Bluetooth de bajo consumo. Si usas estás caso es poco frecuente de ancho de banda bajo o requiere bajo consumo, Bluetooth de bajo consumo es la respuesta. El espacio de nombres principal que incluya esta funcionalidad es [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Cuándo no usar Bluetooth LE**
- Ancho de banda alto, los escenarios de alta frecuencia. Si es necesario mantener constantemente sincronización con grandes cantidades de datos, considera la posibilidad de usar WiFi clásica u o incluso de Bluetooth. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth clásico (Windows.Devices.Bluetooth.Rfcomm)

Las API RFCOMM ofrece a los desarrolladores un socket para realizar la comunicación de estilo de puerto serie de dirección bi. Una vez que tienes un socket, los métodos de escritura en y de lectura de ella son bastante estándar. Una implementación de este se presenta en la [muestra de Rfcomm Chat](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Cuándo no usar Rfcomm de Bluetooth** 
- Notificaciones. El protocolo de Bluetooth GATT tiene un comando específico para ello y se producirá mucho menor consumo de energía y tiempos de respuesta. 
- Comprobación de detección de presencia o de proximidad. Mejor usar las [API de anuncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) y conectarse a través de Bluetooth LE. 


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

**(14393 e inferior)** Esta característica no está disponible para Bluetooth de bajo consumo (cliente GATT), por lo que aún tendrás que par ya sea a través de la página de configuración o mediante las API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) en pedidos acceso a estos dispositivos.

**(15030 y anterior)** Ya no se necesita el emparejamiento de dispositivos Bluetooth. El nuevo APIs Async, como GetGattServicesAsync y GetCharacteristicsAsync para consultar el estado actual del dispositivo remoto. Ver los [documentos de cliente](gatt-client.md) para obtener más detalles. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>¿Cuándo debo emparejar con un dispositivo antes de comunicarse con él?
Por lo general, si se requiere una garantía de confianza, a largo plazo con un dispositivo, emparejarlo (dirigir al usuario a la página de configuración o mediante las API de emparejamiento y la enumeración de dispositivos). Si tienes que leer la información de desactivar el dispositivo que expone públicamente (un sensor de temperatura o balizas), a continuación, conectar o escuchar anuncios sin necesidad de que ningún esfuerzo emparejar el dispositivo. Esto evitará problemas de interoperabilidad a largo plazo como un host de dispositivos no admiten el emparejamiento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>¿Todos los dispositivos de Windows admiten la función periférica?

No: esta es una característica dependientes de hardware, pero se proporciona un método (BluetoothAdapter.IsPeripheralRoleSupported) para consultar si se admite o no.  Dispositivos compatibles incluyen Windows Phone en 8992 + y RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>¿Puedo acceder a estas API de Win32?

Sí, deberían funcionar todas estas API. Este blog detalla la forma de llamar a [Las API de Windows desde aplicaciones de escritorio](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>¿Esta funcionalidad se supone que exista en *- Aquí SKU insertar -*?

**Bluetooth LE**: Sí, todas las funcionalidades de OneCore y deben tener disponible en dispositivos más recientes con una pila de Bluetooth LE funcional. 
> Advertencia: La función periférica es depende del hardware y algunas ediciones de Windows Server no admiten Bluetooth. 

**Bluetooth BR/EDR (los clásicos)**: existen algunas variaciones pero mucho tienen compatibilidad del nivel de perfil muy similar. Consulta a la documentación sobre [RFCOMM](send-or-receive-files-with-rfcomm.md) y estos documentos de perfil admitido para [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) y [teléfono](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)


---
title: Preguntas más frecuentes de los desarrolladores de Bluetooth
description: Este artículo contiene respuestas a las preguntas más frecuentes relacionadas con las API de Bluetooth para la UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 704ce146f95ed64a5891c130fee4d78c90dff034
ms.sourcegitcommit: 58d35b89662d4ad240650933e43fee0b00e9a962
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67344522"
---
# <a name="bluetooth-developer-faq"></a>Preguntas más frecuentes de los desarrolladores de Bluetooth

Este artículo contiene respuestas a las preguntas más frecuentes sobre las API de Bluetooth para la UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>¿Qué API debería usar? ¿Bluetooth Classic (RFCOMM) o Bluetooth Low Energy (GATT)?
Hay varias conversaciones en línea entorno a este tema general. Por lo tanto, esta respuesta se centrará en las diferencias con respecto a Windows. Estas son algunas directrices generales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Usa las API de GATT cuando te comunicas con un dispositivo que admite Bluetooth Low Energy. Si el caso de uso es poco frecuente y de bajo ancho de banda o requiere de bajo consumo de energía, Bluetooth baja energía es la respuesta. El espacio de nombres principal que incluye esta funcionalidad es [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Cuándo no usar Bluetooth LE**
- Escenarios de ancho de banda elevado y alta frecuencia. Si necesitas mantenerte sincronizado constantemente con grandes cantidades de datos, considera usar Bluetooth Classic o incluso Wi-Fi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Las API RFCOMM brindar a los desarrolladores un socket para realizar la comunicación bidireccional en estilo de puerto serie. Cuando esté satisfecho con un socket, los métodos para escribir y leer son bastante estándar. Una implementación de esto se presenta en el [ejemplo de chat de Rfcomm](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Cuándo no usar Rfcomm de Bluetooth** 
- Notificaciones. El protocolo GATT de Bluetooth tiene un comando específico para esto y resultará en un uso de energía y tiempos de respuesta significativamente menores. 
- Comprobar la detección de proximidad o presencia. Es mejor usar las [API de anuncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) y conectarse a través de Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>¿Por qué mi dispositivo Bluetooth LE deja de responder tras una desconexión?

La razón más común que se produce esto es porque el dispositivo remoto ha perdido la información de emparejamiento. Un gran número de dispositivos Bluetooth más antiguos no requiere autenticación. Para proteger al usuario, todas las transacciones de emparejamiento realizadas de la configuración de la aplicación requerirá la autenticación, y algunos dispositivos no se diseñaron con esto en mente. 

A partir de Windows 10 versión 1511, los desarrolladores tienen control sobre el protocolo de enlace de emparejamiento. La [Device enumeration and pairing sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) (Muestra de enumeración y emparejamiento de dispositivos) detalla los diversos aspectos de la asociación de nuevos dispositivos.

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

No tiene dispositivos par antes de usarlos si aprovecha RFCOMM de Bluetooth (clásico). A partir de Windows 10, versión 1607, puedes simplemente consultar si hay dispositivos cercanos y conectarte a ellos. La [RFCOMM Chat Sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (Muestra de chat de RFCOMM) actualizada muestra esta funcionalidad. 

**(14393 y anterior)** Esta característica no está disponible para Bluetooth Low Energy (cliente GATT), por lo que para acceder a estos dispositivos aún tendrás que emparejarlos mediante la página Configuración o mediante las API [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) .

**(15030 y superior)** Ya no se necesita el emparejamiento de dispositivos Bluetooth. Usa las nuevas API de Async, como GetGattServicesAsync y GetCharacteristicsAsync, para consultar el estado actual del dispositivo remoto. Para más información, consulta los [documentos sobre clientes](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>¿Cuándo debería emparejar un dispositivo antes de comunicarme con él?
Por lo general, si necesita un bono de confianza y a largo plazo con un dispositivo, par con él, ya sea al dirigir al usuario a la página de configuración o mediante las API de emparejamiento y enumeración de dispositivos. Si desea leer información desde el dispositivo que expone públicamente (un sensor de temperatura o señalización), a continuación, conectarse o escuchar los anuncios de sin realizar ningún esfuerzo para emparejarse con el dispositivo. Esto evitará problemas de interoperabilidad a largo plazo, dado un gran número de dispositivos no admite el emparejamiento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>¿Todos los dispositivos Windows admiten el rol de periférico?

No. Esta es una característica depende del hardware, pero se proporciona un método, BluetoothAdapter.IsPeripheralRoleSupported, para consultar si se admite o no.  Entre los dispositivos compatibles actualmente se incluye Windows Phone en 8992 + y RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>¿Puedo acceder a estas API de Win32?

Sí, todas estas API deberían funcionar. En este blog se detalla la manera de llamar a las [API de Windows desde aplicaciones de escritorio](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>¿Esta funcionalidad debe existir en *-Insert SKU here-* ?

**Bluetooth LE**: Sí, toda la funcionalidad está en OneCore y debe estar disponible en dispositivos más reciente con una pila Bluetooth LE funcione. 
> Advertencia: Rol periférico depende del hardware y algunas ediciones de Windows Server no son compatibles con Bluetooth. 

**Bluetooth BR/EDR (clásico)** : Algunas variaciones existen pero principalmente, tienen soporte técnico de nivel de perfil muy similares. Consulte los documentos en [RFCOMM](send-or-receive-files-with-rfcomm.md) y estos admiten generar perfiles de documentos para [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) y [teléfono](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

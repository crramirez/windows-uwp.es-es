---
title: Preguntas más frecuentes de los desarrolladores de Bluetooth
description: Este artículo contiene respuestas a las preguntas más frecuentes relacionadas con las API de Bluetooth para la UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 4cc1bafb90b20083d55a622873dea7be5efbf5b7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633490"
---
# <a name="bluetooth-developer-faq"></a>Preguntas más frecuentes de los desarrolladores de Bluetooth

Este artículo contiene respuestas a las preguntas más frecuentes sobre las API de Bluetooth para la UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>¿Qué API debería usar? ¿Bluetooth Classic (RFCOMM) o Bluetooth Low Energy (GATT)?
Hay varias conversaciones en línea entorno a este tema general. Por lo tanto, esta respuesta se centrará en las diferencias con respecto a Windows. Estas son algunas directrices generales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Usa las API de GATT cuando te comunicas con un dispositivo que admite Bluetooth Low Energy. Si tu caso de uso es poco frecuente, usa poco ancho de banda o requiere bajo consumo, Bluetooth Low Energy es la respuesta. El espacio de nombres principal que incluye esta funcionalidad es [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Cuándo no usar Bluetooth LE**
- Escenarios de ancho de banda elevado y alta frecuencia. Si necesitas mantenerte sincronizado constantemente con grandes cantidades de datos, considera usar Bluetooth Classic o incluso Wi-Fi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Las API de RFCOMM ofrecen a los desarrolladores un socket para realizar la comunicación de estilo puerto serie bidireccional. Cuando tengas un socket, los métodos de escribir y leer desde él son bastante estándar. Una implementación de esto se presenta en el [ejemplo de chat de Rfcomm](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Cuándo no usar Rfcomm de Bluetooth** 
- Notificaciones. El protocolo GATT de Bluetooth tiene un comando específico para esto y resultará en un uso de energía y tiempos de respuesta significativamente menores. 
- Comprobar la detección de proximidad o presencia. Es mejor usar las [API de anuncio](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) y conectarse a través de Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>¿Por qué mi dispositivo Bluetooth LE deja de responder tras una desconexión?

La razón más habitual para que esto suceda es porque el dispositivo remoto haya perdido la información de emparejamiento. Muchos de los dispositivos Bluetooth más antiguos no requieren autenticación. Para proteger al usuario, todas las ceremonias de emparejamiento realizadas desde la aplicación Configuración requerirán autenticación, y algunos dispositivos no saben cómo hacerlo. 

A partir de Windows 10, versión 1511, los desarrolladores tienen el control de la ceremonia de emparejamiento. La [Device enumeration and pairing sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) (Muestra de enumeración y emparejamiento de dispositivos) detalla los diversos aspectos de la asociación de nuevos dispositivos.

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

En el caso de los dispositivos Bluetooth RFCOMM (los clásicos), no es necesario. A partir de Windows 10, versión 1607, puedes simplemente consultar si hay dispositivos cercanos y conectarte a ellos. La [RFCOMM Chat Sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (Muestra de chat de RFCOMM) actualizada muestra esta funcionalidad. 

**(14393 y anterior)** Esta característica no está disponible para Bluetooth Low Energy (cliente GATT), por lo que para acceder a estos dispositivos aún tendrás que emparejarlos mediante la página Configuración o mediante las API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) .

**(15030 y superior)** Ya no se necesita el emparejamiento de dispositivos Bluetooth. Usa las nuevas API de Async, como GetGattServicesAsync y GetCharacteristicsAsync, para consultar el estado actual del dispositivo remoto. Para más información, consulta los [documentos sobre clientes](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>¿Cuándo debería emparejar un dispositivo antes de comunicarme con él?
Por lo general, si necesitas un enlace de confianza a largo plazo con un dispositivo, emparéjalo (dirigiendo al usuario a la página de configuración o mediante las API de enumeración y emparejamiento de dispositivos). Si simplemente tiene que leer la información del dispositivo que expone públicamente (un sensor de temperatura o señalización), a continuación, conectarse o escuchar los anuncios de sin realizar ningún esfuerzo para emparejarse con el dispositivo. Esto evitará problemas de interoperabilidad a largo plazo porque un abanico de dispositivos no admite el emparejamiento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>¿Todos los dispositivos Windows admiten el rol de periférico?

No: esta es una característica dependiente de hardware, pero se proporciona un método (BluetoothAdapter.IsPeripheralRoleSupported) para consultar si se admite o no.  Entre los dispositivos compatibles actualmente se incluye Windows Phone en 8992 + y RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>¿Puedo acceder a estas API de Win32?

Sí, todas estas API deberían funcionar. En este blog se detalla la manera de llamar a las [API de Windows desde aplicaciones de escritorio](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>¿Esta funcionalidad debe existir en *-Insert SKU here-*?

**Bluetooth LE**: Sí, toda la funcionalidad está en OneCore y debe estar disponible en dispositivos más reciente con una pila Bluetooth LE funcione. 
> Advertencia: Rol periférico es depende del hardware y algunas ediciones de Windows Server no son compatibles con Bluetooth. 

**Bluetooth BR/EDR (clásico)**: Existen algunas variaciones, pero en general, tienen soporte técnico de nivel de perfil muy similares. Consulta los documentos en [RFCOMM](send-or-receive-files-with-rfcomm.md) y estos documentos de perfil admitidos para [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) y [teléfonos](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles).


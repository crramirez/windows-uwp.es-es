---
title: Preguntas más frecuentes de los desarrolladores de Bluetooth
description: Este artículo contiene respuestas a las preguntas más frecuentes relacionadas con las API de Bluetooth para la UWP.
ms.date: 09/25/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 1a5ff129afcee21b0b1b41212fb900235d5b21b4
ms.sourcegitcommit: 662fcfdc08b050947e289a57520a2f99fad1a620
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91353725"
---
# <a name="bluetooth-developer-faq"></a>Preguntas más frecuentes de los desarrolladores de Bluetooth

Este artículo contiene respuestas a las preguntas más frecuentes sobre las API de Bluetooth para la UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>¿Qué API utilizo? Bluetooth clásico (RFCOMM) o Bluetooth de baja energía (GATT)
Hay varias discusiones en línea en torno a este tema general, así que vamos a mantener esta respuesta de forma cuadrada en la diferencia con respecto a Windows. Estas son algunas instrucciones generales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows. Devices. Bluetooth. GenericAttributeProfile)

Use las API de GATT cuando se comunique con un dispositivo que admita Bluetooth de baja energía. Si el caso de uso es poco frecuente, el ancho de banda bajo o requiere poca energía, Bluetooth de baja energía es la respuesta. El espacio de nombres principal que incluye esta funcionalidad es [Windows. Devices. Bluetooth. GenericAttributeProfile](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Cuándo no usar Bluetooth LE**
- Alto ancho de banda, escenarios de alta frecuencia. Si necesita mantener la sincronización constantemente con grandes cantidades de datos, considere la posibilidad de usar Bluetooth clásico o quizás incluso WiFi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth clásico (Windows. Devices. Bluetooth. RFCOMM)

Las API de RFCOMM proporcionan a los desarrolladores un socket para realizar una comunicación bidireccional de estilo de puerto serie. Una vez que tenga un socket, los métodos para escribir y leer son bastante estándar. Una implementación de esto se presenta en el [ejemplo de chat de RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Cuándo no usar Bluetooth RFCOMM** 
- Notificaciones. El protocolo GATT de Bluetooth tiene un comando específico para esto y dará como resultado un dibujo de energía significativamente menor y tiempos de respuesta más rápidos. 
- Comprobando detección de proximidad o presencia. Mejor usar las [API de anuncios](/uwp/api/windows.devices.bluetooth.advertisement) y conectarse a través de Bluetooth le. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>¿Por qué mi dispositivo Bluetooth LE deja de responder tras una desconexión?

La razón más común es que el dispositivo remoto ha perdido información de emparejamiento. Un gran número de dispositivos Bluetooth antiguos no requiere autenticación. Para proteger al usuario, todas las transacciones de emparejamiento realizadas desde la aplicación de configuración requerirán autenticación y algunos dispositivos no se diseñaron teniendo esto en cuenta. 

A partir de la versión 1511 de Windows 10, los desarrolladores tienen control sobre el protocolo de enlace de emparejamiento. La [Device enumeration and pairing sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) (Muestra de enumeración y emparejamiento de dispositivos) detalla los diversos aspectos de la asociación de nuevos dispositivos.

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

No tiene que emparejar dispositivos antes de usarlos si usa Bluetooth RFCOMM (clásico). A partir de Windows 10, versión 1607, puedes simplemente consultar si hay dispositivos cercanos y conectarte a ellos. La [RFCOMM Chat Sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (Muestra de chat de RFCOMM) actualizada muestra esta funcionalidad. 

**(14393 y versiones anteriores)** Esta característica no está disponible para Bluetooth de baja energía (cliente GATT), por lo que todavía tendrá que emparejar a través de la página Configuración o mediante las API de [Windows. Devices. Enumeration](/uwp/api/windows.devices.enumeration) para tener acceso a estos dispositivos.

**(15030 y versiones posteriores)** Ya no es necesario emparejar dispositivos Bluetooth. Use las nuevas API asincrónicas, como GetGattServicesAsync y GetCharacteristicsAsync, para consultar el estado actual del dispositivo remoto. Vea los [documentos de cliente](gatt-client.md) para obtener más detalles. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>¿Cuándo debo emparejar con un dispositivo antes de comunicarse con él?
Por lo general, si necesita un bono de confianza a largo plazo con un dispositivo, empareje con él. para ello, dirija al usuario a la página de configuración o use la enumeración de dispositivos y las API de emparejamiento. Si solo necesita leer información del dispositivo que se expone públicamente (un sensor de temperatura o señalización), conéctese o escuche anuncios sin hacer ningún esfuerzo por emparejar con el dispositivo. Esto impedirá problemas de interoperabilidad durante la ejecución prolongada, ya que un gran número de dispositivos no admite el emparejamiento. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>¿Todos los dispositivos de Windows admiten el rol periférico?

No. Se trata de una característica dependiente del hardware, pero se proporciona un método, BluetoothAdapter. IsPeripheralRoleSupported, para consultar si se admite o no.  Los dispositivos admitidos actualmente incluyen Windows Phone en 8992 + y RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>¿Puedo obtener acceso a estas API desde Win32?

Sí, todas estas API deberían funcionar. En este blog se detalla la manera de llamar a las [API de Windows desde aplicaciones de escritorio](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/).

## <a name="is-this-functionality-supposed-to-exist-on-a-specific-sku"></a>¿Se supone que esta funcionalidad existe en una SKU específica?

**Bluetooth le**: sí, toda la funcionalidad se encuentra en OneCore y debe estar disponible en los dispositivos más recientes con una pila de Bluetooth le en funcionamiento.

> ADVERTENCIA: el rol periférico depende del hardware y algunas ediciones de Windows Server no admiten Bluetooth.

**Bluetooth br/EDR (clásico)**: existen algunas variaciones, pero principalmente, tienen una compatibilidad de nivel de perfil muy similar. Vea los documentos en [RFCOMM](send-or-receive-files-with-rfcomm.md) y estos documentos de perfil compatibles para [PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles) y [teléfono](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles)
---
author: muhsinking
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: Identificadores de clase de servicio AEP
description: Los servicios de extremo de asociación (AEP) proporcionan un contrato de programación de los servicios que admite un dispositivo a través de un protocolo determinado. Algunos de estos servicios han establecido identificadores que deben usarse al hacer referencia a ellos.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5f103ee3c281ca95abcaee76cdc6f88b74a49eb1
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5563230"
---
# <a name="aep-service-class-ids"></a>Identificadores de clase de servicio AEP



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Los servicios de extremo de asociación (AEP) proporcionan un contrato de programación de los servicios que admite un dispositivo a través de un protocolo determinado. Algunos de estos servicios han establecido identificadores que deben usarse al hacer referencia a ellos. Estos contratos se identifican con la propiedad **System.Devices.AepService.ServiceClassId**. En este tema se muestran varios identificadores de clases de servicios AEP conocidos. El identificador de clase de servicio AEP también se aplica a los protocolos con identificadores de clase personalizada.

Los desarrolladores de aplicaciones deben usar filtros de sintaxis de consulta avanzada (AQS) basados en los identificadores de clases para limitar sus consultas a los servicios AEP que planeen usar. Esto limitará los resultados de la consulta a los servicios relevantes y aumentará considerablemente el rendimiento, la vida útil de la batería y la calidad del servicio para el dispositivo. Por ejemplo, una aplicación puede usar estos identificadores de clases de servicio para usar un dispositivo como una sincronización de Miracast o como representador de medios digitales DLNA (DMR). Para obtener más información sobre la interacción de los dispositivos y servicios entre sí, consulta [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991).

## <a name="bluetooth-and-bluetooth-le-services"></a>Servicios Bluetooth y Bluetooth LE

Los servicios Bluetooth entran dentro de uno de los dos protocolos, el protocolo Bluetooth o el protocolo Bluetooth LE. Los identificadores de estos protocolos son los siguientes:

-   Identificador del protocolo Bluetooth: {e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
-   Identificador del protocolo Bluetooth LE: {bb7bb05e-5972-42b5-94fc-76eaa7084d49}

El protocolo Bluetooth admite varios servicios que siguen el mismo formato básico. Los cuatro primeros dígitos del GUID varían según el servicio; sin embargo, todos los GUID de Bluetooth terminan con **0000-0000-1000-8000-00805F9B34FB**. Por ejemplo, el servicio RFCOMM tiene el precursor 0x0003, por lo que el identificador completo sería **00030000-0000-1000-8000-00805F9B34FB**. En la siguiente tabla se muestran algunos servicios Bluetooth comunes.

| Nombre del servicio                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT: servicio de notificaciones de alertas    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT: E/S de automatización                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT: servicio de batería               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT: presión sanguínea                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT: composición del cuerpo              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT: administración de Bond               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT: supervisión continua de niveles de glucosa | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT: servicio en tiempo real          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT: potencia del pedaleo                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT: velocidad y cadencia del pedaleo     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT: información del dispositivo            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT: detección ambiental         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT: acceso genérico                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT: atributo genérico             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT: glucosa                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT: termómetro de salud            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT: frecuencia cardiaca                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT: dispositivo de interfaz humana        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT: alerta inmediata               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT: posicionamiento interior            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT: compatibilidad con el protocolo de Internet     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT: pérdida de vínculo                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT: ubicación y navegación       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT: servicio de siguiente cambio de horario de verano       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT: servicio de estado de alerta del teléfono    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT: oxímetro de pulso                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT: servicio de actualización de hora de referencia | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT: cadencia y velocidad de carrera     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT: parámetros de análisis               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT: potencia de transmisión                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT: datos de usuario                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT: escala de peso                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

Para obtener una lista más completa de los servicios de Bluetooth disponibles, consulta las páginas de servicios y protocolos Bluetooth [aquí](http://go.microsoft.com/fwlink/p/?LinkID=619586) y [aquí](http://go.microsoft.com/fwlink/p/?LinkID=619587). También puedes usar la API [**GattServiceUuids**](https://msdn.microsoft.com/library/windows/apps/Dn297571) para obtener algunos servicios GATT comunes.

## <a name="custom-bluetooth-le-services"></a>Servicios Bluetooth LE personalizados

Los servicios Bluetooth LE personalizados usan el siguiente identificador de protocolo: {bb7bb05e 5972-42b5 94fc76eaa7084d49}

Los perfiles personalizados se definen con sus propios GUID definidos. Este GUID personalizado se debe usar para **System.Devices.AepService.ServiceClassId**.

## <a name="upnp-services"></a>Servicios UPnP

Los servicios UPnP usan el siguiente identificador de protocolo: {0e261de4-12f0-46e6-91ba-428607ccef64}

En general, todos los servicios UPnP tienen aplicado su nombre con hash en un GUID mediante el algoritmo definido en RFC 4122. En la tabla siguiente se enumeran algunos de los servicios UPnP comunes definidos en Windows.

| Nombre del servicio                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| Administrador de conexiones                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| Transporte AV                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| Control de representaciones                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| Reenvío de capa 3                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| Configuración de la interfaz común WAN | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| Conexión WAP IP                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| Configuración WFA WLAN             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| Impresora mejorada                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| Impresora básica                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| Registrador de receptor de multimedia           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| Directorio de contenidos                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| DIAL                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

 

## <a name="wsd-services"></a>Servicios WSD

Los servicios WSD usan el siguiente identificador de protocolo: {782232aa-a2f9-4993-971b-aedc551346b0}

En general, todos los servicios WSD tienen aplicado su nombre con hash en un GUID mediante el algoritmo definido en RFC 4122. En la tabla siguiente se enumeran algunos de los servicios WSD comunes definidos en Windows.

| Nombre del servicio | GUID                                     |
|--------------|------------------------------------------|
| Impresora      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| Escáner      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

 

## <a name="aqs-sample"></a>Ejemplo de AQS

Esta AQS filtrará todos los objetos **AssociationEndpointService** UPnP compatibles con DIAL. En este caso, [ **DeviceInformationKind** ](https://msdn.microsoft.com/library/windows/apps/Dn948991) se establece en **AsssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```

 

 

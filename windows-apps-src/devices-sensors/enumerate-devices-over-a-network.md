---
author: muhsinking
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: Enumerar dispositivos a través de una red
description: Además de la detección de dispositivos conectados localmente, puedes usar las API Windows.Devices.Enumeration para enumerar dispositivos a través de protocolos de red e inalámbricos.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 00f8d4314d67828fa30007d3b8af4c4e1d06c154
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936021"
---
# <a name="enumerate-devices-over-a-network"></a>Enumerar dispositivos a través de una red



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Además de la detección de dispositivos conectados localmente, puedes usar las API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) para enumerar dispositivos a través de protocolos de red e inalámbricos.

## <a name="enumerating-devices-over-networked-or-wireless-protocols"></a>Enumeración de dispositivos mediante protocolos de red o inalámbricos

A veces es necesario enumerar los dispositivos que no están conectados localmente y que solo pueden detectarse a través de protocolos de red o inalámbricos. Para ello, las API de [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) tienen tres tipos de objetos de dispositivo: **AssociationEndpoint** (AEP), **AssociationEndpointContainer** (contenedor AEP) y **AssociationEndpointService** (servicio AEP). En conjunto, se denominan AEP u objetos AEP.

Algunas API de dispositivos proporcionan una cadena de selector que puedes usar para enumerar a través de los objetos AEP disponibles. Esto puede incluir tanto dispositivos que están emparejados con el sistema como dispositivos que no lo están. Es posible que algunos de los dispositivos no necesiten emparejarse. Estas API de dispositivo pueden intentar emparejar el dispositivo si es necesario emparejarlo antes de interactuar con él. Wi-Fi Direct es un ejemplo de API que siguen este patrón. Si estas API de dispositivo no emparejan automáticamente el dispositivo, puedes emparejarlo mediante el objeto [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396) disponible en [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960).

Sin embargo, puede haber casos en los que quieras detectar dispositivos manualmente sin usar una cadena de selector predefinida. Por ejemplo, es posible que solo tengas que recopilar información acerca de los dispositivos AEP sin interactuar con ellos, o que desees buscar más objetos AEP de los que se detectarán con la cadena de selector predefinida. En este caso, podrás crear tu propia cadena de selector y usarla siguiendo las instrucciones de la sección [Compilar un selector de dispositivos](build-a-device-selector.md).

Cuando crees tu selector, te recomendamos que limites el ámbito de la enumeración a los protocolos que te interesan. Por ejemplo, no es necesario que la radio de Wi-Fi busque dispositivos Wi-Fi Direct si lo que te interesa son los dispositivos UPnP. Windows ha definido una identidad para todos los protocolos que puedes usar para definir el ámbito de la enumeración. En la tabla siguiente se enumeran los tipos y los identificadores de protocolo.

| Tipo de dispositivo de red o protocolo              | Identificador                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP (incluido DIAL y DLNA)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| Web services on devices (WSD)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-Fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| Detección de servicio DNS (DNS-SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| Punto de servicio                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| Impresoras de red (impresoras de Active Directory) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Windows connect now (WNC)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| Bases WiGig                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| Aprovisionamiento Wi-Fi para impresoras HP           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| Bluetooth                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| Bluetooth LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## <a name="aqs-examples"></a>Ejemplos de AQS

Cada tipo AEP tiene una propiedad que puedes usar para limitar la enumeración a un protocolo específico. Ten en cuenta que puedes usar el operador OR en un filtro AQS para combinar varios protocolos. Estos son algunos ejemplos de cadenas de filtro AQS que muestran cómo realizar consultas de dispositivos AEP.

Esta AQS consulta todos los objetos UPnP **AssociationEndpoint** cuando la enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está establecida como **AsssociationEndpoint**.

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Esta AQS consulta todos los objetos UPnP y WSD **AssociationEndpoint** cuando la enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está establecida como **AsssociationEndpoint**.

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Esta AQS consulta todos los objetos UPnP **AssociationEndpointService** si la enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está establecida como **AssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Esta AQS consulta los objetos **AssociationEndpointContainer** cuando la enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) está establecida como **AssociationEndpointContainer**, pero solo los encuentra al enumerar el protocolo UPnP. Por lo general, no sería útil enumerar contenedores que solo provienen de un protocolo. Sin embargo, esto puede ser útil si limitas tu filtro a los protocolos en los que sabes que se puede detectar tu dispositivo.

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 

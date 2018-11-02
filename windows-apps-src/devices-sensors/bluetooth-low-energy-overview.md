---
author: msatranjr
title: Bluetooth Low Energy
description: Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, bajo consumo, gatt, separación, central, periférico, cliente, servidor, monitor, Editor
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5973384"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth de bajo consumo (LE) es una especificación que define protocolos para la detección y la comunicación entre dispositivos eficiente. Detección de dispositivos se realiza a través del protocolo de perfil de acceso genérico (BPA). Después de la detección, la comunicación de dispositivo a otro se realiza a través del protocolo de atributo genérico (GATT). Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP. Para ver más detalles sobre Bluetooth LE, consulta la [Especificación de Bluetooth Core](https://www.bluetooth.com/specifications/bluetooth-core-specification) versión 4.0, donde se introdujo Bluetooth LE. 

![Roles de Bluetooth LE](images/gatt-roles.png)

*Roles GATT y separación se introdujeron en Windows 10 versión 1703*

Protocolos GATT y separación se pueden implementar en tu aplicación para UWP mediante el uso de los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central y periféricos
Las dos funciones principales de detección se denominan Central y periféricos. En general, Windows funciona en el modo Central y se conecta a diversos dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Una sigla comunes que verás en las APIs Bluetooth Windows es de atributo genérico (GATT). El perfil GATT define la estructura de datos y los modos de funcionamiento por el que se comunican dos dispositivos Bluetooth LE. El atributo es el bloque de creación principal de GATT. Los tipos principales de atributos son servicios, características y descriptores. Estos atributos realizan de manera diferente entre clientes y servidores, por lo que es más útil hablar de su interacción en las secciones pertinentes. 

![Típica jerarquía de atributo en un perfil común](images/gatt-service.png)

*El servicio de frecuencia cardiaca se expresa en forma de API de servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Una vez establecida una conexión, el dispositivo que contiene los datos (por lo general, un sensor de IoT pequeño o dispositivo transportable) se conoce como el servidor. El dispositivo que use esos datos para realizar una función se conoce como el cliente. Por ejemplo, un equipo con Windows (cliente) lee datos de un monitor de frecuencia cardiaca (servidor) para realizar un seguimiento de que un usuario está funcionando forma óptima. Para obtener más información, consulta los temas de [Cliente GATT](gatt-client.md) y [Servidor GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Monitores y publicadores (balizas)
Además de los roles Central y periférico, hay funciones observador y emisor. Emisoras normalmente se conoce como balizas, se no comunican mediante GATT, ya que usan el espacio limitado proporcionado en el paquete de anuncio para la comunicación. Del mismo modo, no es necesario establecer una conexión para recibir datos de un observador, busca los anuncios cercanos. Para configurar Windows para observar cercanos anuncios, usa la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Con el fin de difundir cargas de baliza, usa la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obtener más información, consulta el tema de [anuncio](ble-beacon.md) .

## <a name="see-also"></a>Consulta también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificación básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)
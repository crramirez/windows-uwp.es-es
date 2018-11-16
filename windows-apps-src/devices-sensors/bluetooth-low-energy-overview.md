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
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6973293"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth de bajo consumo (LE) es una especificación que define los protocolos de detección y la comunicación entre dispositivos eficiente. Detección de dispositivos se realiza a través del protocolo de perfil de acceso genérico (BPA). Después de la detección, la comunicación de dispositivo a otro se realiza a través del protocolo de atributo genérico (GATT). Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP. Para ver más detalles acerca de Bluetooth LE, consulte la versión de la [Especificación de básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) 4.0, donde se introdujo Bluetooth LE. 

![Roles de Bluetooth LE](images/gatt-roles.png)

*Roles GATT y separación se introdujeron en Windows 10 versión 1703*

Protocolos GATT y separación se pueden implementar en tu aplicación para UWP mediante el uso de los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central y periféricos
Las dos funciones principales de detección se denominan Central y periféricos. En general, Windows funciona en el modo Central y se conecta a diversos dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Una sigla comunes que verás en las APIs Bluetooth Windows es el atributo genérico (GATT). El perfil GATT define la estructura de datos y los modos de funcionamiento por el que se comunican los dos dispositivos Bluetooth LE. El atributo es el bloque de creación principal de GATT. Los tipos principales de atributos son servicios, características y descriptores. Estos atributos realizan de manera diferente entre clientes y servidores, por lo que resulta más útil explicar su interacción en las secciones pertinentes. 

![Típica jerarquía de atributo en un perfil común](images/gatt-service.png)

*El servicio de frecuencia cardiaca se expresa en forma de API de servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Una vez establecida una conexión, el dispositivo que contiene los datos (normalmente un sensor de IoT pequeño o dispositivo transportable) se conoce como el servidor. El dispositivo que use esos datos para realizar una función se conoce como el cliente. Por ejemplo, un equipo de Windows (cliente) lee datos de un monitor de frecuencia cardiaca (servidor) para realizar un seguimiento de que un usuario está funcionando correctamente. Para obtener más información, consulta los temas de [Cliente GATT](gatt-client.md) y [Servidor GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Monitores y publicadores (balizas)
Además de las funciones Central y periféricos, hay funciones observador y emisor. Emisoras normalmente se conoce como balizas, no comunican a través de GATT, ya que usan el espacio limitado proporcionado en el paquete de anuncio para la comunicación. Del mismo modo, no es necesario establecer una conexión para recibir datos de un observador, busca los anuncios cercanos. Para configurar Windows para observar cercanos anuncios, usa la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Con el fin de difusión cargas de baliza, usa la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obtener más información, consulta el tema de [anuncio](ble-beacon.md) .

## <a name="see-also"></a>Consulta también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificación básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)
---
author: msatranjr
title: Bluetooth Low Energy
description: Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones UWP.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, bajo consumo de energía, gatt, brecha, central, periférico, cliente, servidor, monitor, publisher
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919133"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth baja energía (LE) es una especificación que define protocolos para el descubrimiento y la comunicación entre los dispositivos de bajo consumo de energía. Descubrimiento de dispositivos se realiza mediante el protocolo de perfil de acceso genérico (GAP). Después del discovery, dispositivos de comunicación se realiza mediante el protocolo de atributo genérico (GATT). Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones UWP. Para ver más detalles acerca de Bluetooth LE, consulte la [Especificación básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) versión 4.0, donde se introdujo el Bluetooth. 

![Funciones de Bluetooth LE](images/gatt-roles.png)

*GATT y la separación de funciones se introdujeron en Windows 10 versión 1703*

Protocolos del GATT y brecha pueden implementarse en su aplicación UWP utilizando los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Centrales y periféricos
Las dos funciones principales de descubrimiento se denominan Central y periféricos. En general, Windows funciona en modo Central y se conecta a varios dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Un acrónimo común que se verá en las APIs de Bluetooth de Windows es el atributo genérico (GATT). El perfil del GATT define la estructura de datos y modos de operación por la que se comunican dos dispositivos Bluetooth LE. El atributo es el bloque de construcción principal del GATT. Los principales tipos de atributos son servicios, características y descriptores. Estos atributos realizan de forma diferente entre clientes y servidores, por lo que es más útil describir su interacción en las secciones pertinentes. 

![Jerarquía de atributo típico en un perfil común](images/gatt-service.png)

*El servicio de ritmo cardíaco se expresará en forma de API de servidor del GATT*

## <a name="client-and-server"></a>Cliente y servidor
Después de haber establecido una conexión, el dispositivo que contiene los datos (normalmente un sensor IoT pequeño y transportable) se conoce como el servidor. El dispositivo que utiliza esos datos para realizar una función se conoce como el cliente. Por ejemplo, un PC con Windows (cliente) lee datos de un monitor de ritmo cardíaco (servidor) para realizar un seguimiento de que un usuario está funcionando óptimamente. Para obtener más información, vea los temas [Del GATT cliente](gatt-client.md) y [Servidor del GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Monitores y Publishers (Beacons)
Además de las funciones centrales y periféricos, hay funciones de observador y Broadcaster. Los broadcasters se conocen comúnmente como balizas, no comunican a través del GATT porque ocupan el espacio limitado proporcionado en el paquete de anuncio para la comunicación. De forma similar, un observador no tiene que establecer una conexión para recibir datos, busca los anuncios cercanos. Para configurar Windows para observar cercanos anuncios, utilice la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Con el fin de difundir cargas de baliza, utilice la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obtener más información, vea el tema del [anuncio](ble-beacon.md) .

## <a name="see-also"></a>Consulta también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificación de la base de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)
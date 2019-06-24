---
title: Bluetooth Low Energy
description: En este tema se proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP.
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, bluetooth, bluetooth LE, low energy, gatt, gap, central, peripheral, periférico, client, cliente, server, servidor, watcher, monitor, publisher, editor
ms.localizationpriority: medium
ms.openlocfilehash: 3f23bdc658d2a82e3edeefd0a7be471ca9620d33
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321610"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth Low Energy (LE) es una especificación que define los protocolos de detección y comunicación entre dispositivos de consumo eficiente. La detección de dispositivos se hace a través del protocolo de perfil de acceso genérico (GAP). Después de la detección, la comunicación de dispositivo a dispositivo se hace a través del protocolo de atributo genérico (GATT). En este tema se proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP. Para ver más detalles sobre Bluetooth LE, consulta [Bluetooth Core Specification](https://www.bluetooth.com/specifications/bluetooth-core-specification/) versión 4.0, donde se presentó Bluetooth LE. 

![Roles de Bluetooth LE](images/gatt-roles.png)

*GATT y separación de roles se introdujeron en Windows 10 versión 1703*

Los protocolos GATT y GAP se pueden implementar en tu aplicación para UWP usando los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central y periféricos
Los dos roles principales de detección se llaman Central y Periférico. En general, Windows funciona en modo Central y se conecta a distintos dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Un acrónimo común que verás en la API de Windows Bluetooth es GATT (atributo genérico). El perfil GATT define la estructura de datos y modos de funcionamiento por los que se comunican dos dispositivos Bluetooth LE. El atributo es el principal bloque base de GATT. Los tipos principales de atributos son servicios, características y descriptores. Estos atributos tienen un rendimiento diferente entre clientes y servidores, por lo que es más útil hablar sobre su interacción en las secciones pertinentes. 

![Jerarquía de atributo típico de un perfil común](images/gatt-service.png)

*El servicio de ritmo cardíaco se expresa en forma de API de servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Después de que se haya establecido una conexión, el dispositivo que contiene los datos (normalmente un sensor pequeño de IoT o dispositivo transportable) se conoce como el servidor. El dispositivo que usa esos datos para realizar una función se conoce como el cliente. Por ejemplo, un PC Windows (cliente) lee los datos de un monitor de frecuencia cardiaca (servidor) para hacer un seguimiento de que un usuario está haciendo ejercicio de manera óptima. Para más información, consulta los temas [Cliente GATT](gatt-client.md) y [Servidor GATT](gatt-server.md).

## <a name="watchers-and-publishers-beacons"></a>Monitores y editores (balizas)
Además de los roles Central y Periférico, existen los roles Observador y Emisor. Los emisores se conocen como balizas, no se comunica a través de GATT porque usan el espacio limitado que se proporciona en el paquete de anuncio para la comunicación. Del mismo modo, un observador no tiene que establecer una conexión para recibir datos, busca los anuncios cercanos. Para configurar Windows de modo que observe los anuncios cercanos, usa la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher). Para difundir cargas de baliza, usa la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher). Para más información, consulta el tema [Anuncio](ble-beacon.md).

## <a name="see-also"></a>Vea también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificación principal de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification/)
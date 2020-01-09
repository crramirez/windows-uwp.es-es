---
title: Bluetooth de bajo consumo
description: En este tema se proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP.
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, bluetooth, bluetooth LE, low energy, gatt, gap, central, peripheral, periférico, client, cliente, server, servidor, watcher, monitor, publisher, editor
ms.localizationpriority: medium
ms.openlocfilehash: 4859dfb540b252f379a0ec3cbfe52985c0776fd9
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684857"
---
# <a name="bluetooth-low-energy"></a>Bluetooth de bajo consumo
Bluetooth Low Energy (LE) es una especificación que define los protocolos de detección y comunicación entre dispositivos de consumo eficiente. La detección de dispositivos se hace a través del protocolo de perfil de acceso genérico (GAP). Después de la detección, la comunicación de dispositivo a dispositivo se hace a través del protocolo de atributo genérico (GATT). En este tema se proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP. Para ver más detalles sobre Bluetooth LE, consulta [Bluetooth Core Specification](https://www.bluetooth.com/specifications/bluetooth-core-specification/) versión 4.0, donde se presentó Bluetooth LE. 

![Roles de Bluetooth LE](images/gatt-roles.png)

*Los roles GATT y GAP se introdujeron en la versión 1703 de Windows 10*

Los protocolos GATT y GAP se pueden implementar en tu aplicación para UWP usando los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows. Devices. Bluetooth. Advertisement](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>Central y periféricos
Los dos roles principales de detección se llaman Central y Periférico. En general, Windows funciona en modo Central y se conecta a distintos dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Un acrónimo común que verás en la API de Windows Bluetooth es GATT (atributo genérico). El perfil GATT define la estructura de datos y modos de funcionamiento por los que se comunican dos dispositivos Bluetooth LE. El atributo es el principal bloque base de GATT. Los tipos principales de atributos son servicios, características y descriptores. Estos atributos tienen un rendimiento diferente entre clientes y servidores, por lo que es más útil hablar sobre su interacción en las secciones pertinentes. 

![Jerarquía de atributo típica en un perfil común](images/gatt-service.png)

*El servicio de tarifa cardíaca se expresa en el formulario de API del servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Después de que se haya establecido una conexión, el dispositivo que contiene los datos (normalmente un sensor pequeño de IoT o dispositivo transportable) se conoce como el servidor. El dispositivo que usa esos datos para realizar una función se conoce como el cliente. Por ejemplo, un PC Windows (cliente) lee los datos de un monitor de frecuencia cardiaca (servidor) para hacer un seguimiento de que un usuario está haciendo ejercicio de manera óptima. Para más información, consulta los temas [Cliente GATT](gatt-client.md) y [Servidor GATT](gatt-server.md).

## <a name="watchers-and-publishers-beacons"></a>Monitores y editores (balizas)
Además de los roles Central y Periférico, existen los roles Observador y Emisor. Los emisores se conocen como balizas, no se comunica a través de GATT porque usan el espacio limitado que se proporciona en el paquete de anuncio para la comunicación. Del mismo modo, un observador no tiene que establecer una conexión para recibir datos, busca los anuncios cercanos. Para configurar Windows de modo que observe los anuncios cercanos, usa la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher). Para difundir cargas de baliza, usa la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher). Para más información, consulta el tema [Anuncio](ble-beacon.md).

## <a name="see-also"></a>Consulta también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows. Devices. Bluetooth. Advertisement](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)
- [Especificación de núcleos de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)

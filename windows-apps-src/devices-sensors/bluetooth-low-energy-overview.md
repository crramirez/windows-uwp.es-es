---
author: msatranjr
title: Bluetooth Low Energy
description: Este tema proporciona una introducción rápida a Bluetooth LE en aplicaciones UWP.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, bluetooth, bluetooth LE, energía baja, gatt, espacio, central, periférico, cliente, server, monitor, publisher
ms.localizationpriority: medium
ms.openlocfilehash: 0d6cc1fb5a0b3cb96748b99c490b23a9e1df128f
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "305306"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth baja energía (ej) es una especificación que define protocolos para la detección y la comunicación entre los dispositivos de alimentación eficiente. Detección de dispositivos se realiza a través del protocolo de perfil de acceso genérico (espacio). Después de la detección, dispositivos de comunicación se realiza a través del protocolo de atributo genérico (GATT). Este tema proporciona una introducción rápida a Bluetooth LE en aplicaciones UWP. Para ver más detalles acerca de LE Bluetooth, vea la [Especificación de núcleo de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) versión 4.0, donde se introdujo LE Bluetooth. 

![Bluetooth LE Roles](images/gatt-roles.png)

*GATT y la separación de funciones se introdujeron en 10 Windows versión 1703*

Protocolos GATT e intervalos se pueden implementar en su aplicación UWP mediante el uso de los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central y periféricos
Las dos funciones principales de detección se denominan Central y periféricos. En general, Windows funciona en modo Central y se conecta a diversos dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Un acrónimo comunes que verá en el Windows Bluetooth APIs es atributo genérico (GATT). El perfil de GATT define la estructura de datos y modos de funcionamiento por el que se comunican dos dispositivos Bluetooth LE. El atributo es el bloque de creación principal de GATT. Los tipos principales de atributos son los servicios, las características y descriptores de. Estos atributos realizan de forma diferente entre clientes y servidores, por lo que es más útil explicar su interacción en las secciones relevantes. 

![Jerarquía de atributo típica en un perfil común](images/gatt-service.png)

*El servicio de tasa de corazón se expresará en forma de API de servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Después de que se ha establecido una conexión, el dispositivo que contiene los datos (normalmente un detector IoT pequeño o para llevar) se conoce como el servidor. El dispositivo que utiliza esos datos para realizar una función se conoce como el cliente. Por ejemplo, un PC con Windows (cliente) lee datos de un monitor de tasa de corazón (servidor) para realizar un seguimiento de que un usuario está funcionando correctamente. Para obtener más información, vea los temas [GATT cliente](gatt-client.md) y [Servidor GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Observadores y editores (balizas)
Además de las funciones Central y periféricos, hay funciones observador y emisor. Con frecuencia se hace referencia a las empresas de broadcast como balizas, no comunicarán a través del GATT debido a que utilizan el espacio limitado proporcionado en el paquete de anuncio para la comunicación. De forma similar, un observador no tiene que establecer una conexión para recibir datos, busca los anuncios cercanos. Para configurar Windows para observar cercanos anuncios, use la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Con el fin de Difundir presentación baliza cargas, use la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obtener más información, vea el tema de [anuncio](ble-beacon.md) .

## <a name="see-also"></a>Consulte también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificación de núcleo de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)
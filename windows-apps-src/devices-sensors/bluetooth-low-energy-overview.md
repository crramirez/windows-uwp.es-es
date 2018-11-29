---
title: Bluetooth Low Energy
description: Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP.
ms.date: 03/15/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, bajo consumo, gatt, separación, central, periférico, cliente, servidor, monitor, Editor
ms.localizationpriority: medium
ms.openlocfilehash: 3853003e54e58b3949c248fb03cb0a83e2d6d112
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8197710"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth de bajo consumo (LE) es una especificación que define los protocolos de detección y la comunicación entre dispositivos eficiente. Detección de dispositivos se realiza mediante el protocolo de perfil de acceso genérico (BPA). Después de la detección, la comunicación de dispositivo a otro se realiza mediante el protocolo de atributo genérico (GATT). Este tema proporciona una introducción rápida de Bluetooth LE en aplicaciones para UWP. Para ver más detalles sobre Bluetooth LE, consulta la [Especificación de básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) versión 4.0, donde se introdujo Bluetooth LE. 

![Roles de Bluetooth LE](images/gatt-roles.png)

*Roles GATT y separación se introdujeron en Windows 10 versión 1703*

Protocolos GATT y separación se pueden implementar en tu aplicación para UWP mediante el uso de los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central y periféricos
Las dos funciones principales de detección se denominan Central y periférico. En general, Windows funciona en el modo Central y se conecta a diversos dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Una sigla comunes que verás en las APIs Bluetooth Windows es atributo genérico (GATT). El perfil GATT define la estructura de datos y los modos de funcionamiento por el que se comunican los dos dispositivos Bluetooth LE. El atributo es el bloque de creación principal de GATT. Los tipos principales de atributos son servicios, características y descriptores. Estos atributos realizan de manera diferente entre clientes y servidores, por lo que es más útil explicar su interacción en las secciones pertinentes. 

![Típica jerarquía de atributo en un perfil común](images/gatt-service.png)

*El servicio de frecuencia cardiaca se expresa en forma de API de servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Después de que se ha establecido una conexión, el dispositivo que contiene los datos (normalmente un sensor de IoT pequeño o dispositivo transportable) se conoce como el servidor. El dispositivo que use esos datos para realizar una función se conoce como el cliente. Por ejemplo, un equipo de Windows (cliente) lee datos de un monitor de frecuencia cardiaca (Server) para realizar un seguimiento que un usuario está funcionando forma óptima. Para obtener más información, consulta los temas de [Cliente GATT](gatt-client.md) y [Servidor GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Establécelos y publicadores (balizas)
Además de los roles Central y periférico, hay funciones observador y emisor. Emisoras normalmente se conoce como balizas, se no comunican mediante GATT, ya que usan el espacio limitado proporcionado en el paquete de anuncios para la comunicación. Del mismo modo, no es necesario establecer una conexión para recibir datos de un observador, busca los anuncios cercanos. Para configurar Windows para observar cercanos anuncios, usa la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Con el fin de difundir cargas de baliza, usa la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obtener más información, consulta el tema de [anuncio](ble-beacon.md) .

## <a name="see-also"></a>Consulta también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Especificación básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)
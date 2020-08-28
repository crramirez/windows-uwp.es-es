---
title: Bluetooth de bajo consumo
description: Obtenga información sobre la especificación de baja energía de Bluetooth (LE) en aplicaciones UWP que define los protocolos para la detección y la comunicación entre dispositivos de eficacia energética.
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Bluetooth, Bluetooth LE, baja energía, GATT, brecha, centro, periférico, cliente, servidor, monitor y publicador
ms.localizationpriority: medium
ms.openlocfilehash: 56fcb84f5dddc0c49e48c49787a4278a1f10446f
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043397"
---
# <a name="bluetooth-low-energy"></a>Bluetooth de bajo consumo
Bluetooth de baja energía (LE) es una especificación que define los protocolos para la detección y la comunicación entre dispositivos de eficacia energética. La detección de dispositivos se realiza a través del Protocolo de Perfil de acceso genérico (GAP). Después de la detección, la comunicación entre dispositivos se realiza a través del Protocolo de atributo genérico (GATT). En este tema se proporciona información general rápida sobre Bluetooth LE en aplicaciones para UWP. Para ver más detalles acerca de Bluetooth LE, consulte la versión 4,0 de la [especificación básica de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification/) , donde se presentó Bluetooth le. 

![Roles de Bluetooth LE](images/gatt-roles.png)

*Los roles GATT y GAP se introdujeron en la versión 1703 de Windows 10*

Los protocolos GATT y GAP se pueden implementar en la aplicación para UWP mediante el uso de los siguientes espacios de nombres.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>Central y periféricos
Los dos roles principales de detección se denominan central y periféricos. En general, Windows funciona en modo central y se conecta a varios dispositivos periféricos. 

## <a name="attributes"></a>Atributos
Un acrónimo común que verá en las API de Windows Bluetooth es atributo genérico (GATT). El perfil GATT define la estructura de los datos y los modos de funcionamiento por los que se comunican dos dispositivos Bluetooth LE. El atributo es el bloque de creación principal de GATT. Los principales tipos de atributos son los servicios, las características y los descriptores. Estos atributos se realizan de manera diferente entre clientes y servidores, por lo que resulta más útil analizar su interacción en las secciones correspondientes. 

![Jerarquía de atributo típica en un perfil común](images/gatt-service.png)

*El servicio de tarifa cardíaca se expresa en el formulario de API del servidor GATT*

## <a name="client-and-server"></a>Cliente y servidor
Una vez establecida una conexión, el dispositivo que contiene los datos (normalmente un sensor de IoT pequeño o portátil) se conoce como servidor. El dispositivo que usa esos datos para realizar una función se conoce como el cliente. Por ejemplo, un equipo de Windows (cliente) lee los datos de un monitor de frecuencia de corazón (servidor) para hacer un seguimiento de que un usuario está trabajando de forma óptima. Para obtener más información, consulte los temas sobre el [cliente GATT](gatt-client.md) y el [servidor GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Monitores y publicadores (balizas)
Además de los roles central y periféricos, hay roles de observador y de difusión. Los emisores se conocen normalmente como balizas, no se comunican a través de GATT porque usan el espacio limitado proporcionado en el paquete de anuncios para la comunicación. Del mismo modo, un observador no tiene que establecer una conexión para recibir datos, sino que busca anuncios cercanos. Para configurar Windows para observar anuncios cercanos, use la clase [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Para difundir las cargas de señalización, utilice la clase [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Para obtener más información, vea el tema sobre el [anuncio](ble-beacon.md) .

## <a name="see-also"></a>Consulte también
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)
- [Especificación de núcleos de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)

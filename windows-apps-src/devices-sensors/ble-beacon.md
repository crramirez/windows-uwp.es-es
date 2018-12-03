---
title: Anuncios de Bluetooth
description: En esta sección se incluyen artículos acerca de cómo integrar anuncios de Bluetooth de bajo consumo (LE) en aplicaciones para la Plataforma universal de Windows (UWP) a través de las API AdvertisementWatcher y AdvertisementPublisher.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
ms.localizationpriority: medium
ms.openlocfilehash: e9eafde0596ad3156f52a7a2f0a1566444a9836a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8461715"
---
# <a name="bluetooth-le-advertisements"></a>Anuncios de Bluetooth de bajo consumo (LE)


**API importantes**

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

En este artículo se proporciona información general de las balizas de los anuncios de Bluetooth de bajo consumo (LE) para aplicaciones para la Plataforma universal de Windows (UWP).  

## <a name="overview"></a>Introducción

Hay dos funciones principales que un desarrollador puede realizar mediante las API de anuncio de bajo consumo (LE):

-   [Advertisement Watcher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx) (Monitor de anuncios): Escuchar balizas cercanas y filtrarlas en función de la carga o la proximidad.  
-   [Anunciante](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): Definir una carga de Windows para anunciarse en nombre de un desarrollador.  

Hay un completo código de ejemplo en [Bluetooth Advertisement Sample](http://go.microsoft.com/fwlink/p/?LinkId=619990) (Muestra de publicidad de Bluetooth) en Github

## <a name="basic-setup"></a>Configuración básica

Para usar la funcionalidad básica de Bluetooth LE en una aplicación para la Plataforma Universal de Windows, debes comprobar la capacidad de Bluetooth en Package.appxmanifest.

1. Abre Package.appxmanifest.
2. Ve a la pestaña Capacidades.
3. Busca Bluetooth en la lista de la izquierda y activa la casilla situada a su lado.

## <a name="publishing-advertisements"></a>Publicación de anuncios

Los anuncios de Bluetooth LE permiten que tu dispositivo señalice constantemente una carga específica, denominada anuncio. Este anuncio puede verse en cualquier otro dispositivo cercano compatible con Bluetooth LE, si está configurado para escuchar este anuncio específico.

> **Nota**: para la privacidad del usuario, la vida útil de tu anuncio está vinculada a la de la aplicación. Puedes crear un BluetoothLEAdvertisementPublisher y llamar a Inicio en una tarea en segundo plano para un anuncio en segundo plano. Para obtener más información sobre las tareas en segundo plano, consulta [Inicio, reanudación y tareas en segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/index).

### <a name="basic-publishing"></a>Publicación básica

Existen muchas formas distintas de agregar datos a un anuncio. Este ejemplo muestra una forma común de crear un anuncio específico de la empresa. 

Primero, crea el anunciante que controla si el dispositivo señaliza o no un anuncio específico.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

En segundo lugar, crea una sección de datos personalizados. Este ejemplo usa un valor de **CompanyId** sin asignar *0xFFFE* y agrega el texto *Hello World* al anuncio. 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

Ahora que ha creado el anunciante y se ha configurado, puedes llamar a **Inicio** para comenzar la publicidad.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>Supervisión de anuncios

### <a name="basic-watching"></a>Supervisión básica

El siguiente código enseña cómo crear un monitor de anuncios de Bluetooth LE, establecer una devolución de llamada y comenzar a supervisar todos los anuncios de LE.

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
``` 

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>Exploración activa
Para recibir también anuncios de respuesta de exploración, establece los siguiente tras crear el monitor. Ten en cuenta que esto producirá un mayor consumo de energía y no está disponible en los modos en segundo plano.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>Supervisión del patrón de un anuncio específico

A veces puedes querer escuchar un anuncio específico. En este caso, escucha un anuncio que contiene una carga con una empresa inventada (identificada como 0xFFFE) y la cadena *Hello World* en el anuncio. Esto puede combinarse con el ejemplo de publicación básica para contar con un equipo Windows que anuncia y otro que supervisa. Asegúrate de establecer este filtro de anuncios antes de iniciar el monitor.

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>Supervisión de un anuncio cercano

A veces puede que solo te interese desencadenar el monitor cuando la publicidad del dispositivo esté en el alcance. Puedes definir tu propio rango, simplemente ten en cuenta que los valores se recortarán para estar entre 0 y -128. 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>Calibración de la distancia

Cuando se desencadena la devolución de llamada del monitor de Bluetooth LE, las clases eventArgs incluyen un valor RSSI que indica la intensidad de la señal recibida (la intensidad de la señal de Bluetooth).

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

Esto puede traducirse aproximadamente a distancia, pero no se debe usar para medir distancias reales, ya que cada radio individual es diferente. Diferentes factores ambientales pueden dificultar la calibración de la distancia (por ejemplo, las paredes, carcasas alrededor de la radio o incluso la humedad del aire).

Una alternativa a juzgar la distancia pura es definir "depósitos". Las radios tienden a informar de 0 a-50 DBm cuando están muy cerca, de -50 a -90 cuando están a una distancia media y menos de -90 cuando están lejos. El método de ensayo y error es la mejor forma de determinar cómo quieres que sean estos depósitos para tu aplicación.
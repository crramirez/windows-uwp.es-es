---
author: TerryWarwick
title: Enumeración de dispositivos PointOfService
description: Más información acerca de cómo enumerar dispositivos PointOfService
ms.author: jken
ms.date: 06/8/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: be1fdc42295fc03ff86a69e287a4089abe547689
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018443"
---
# <a name="enumerating-point-of-service-devices"></a>Enumeración de dispositivos de punto de servicio
En esta sección aprenderás cómo [**definir un selector de dispositivos**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) que se usa para consultar los dispositivos disponibles para el sistema y cómo usar este selector para enumerar los dispositivos de punto de servicio mediante uno de los métodos siguientes:

**Método 1:** [**Obtener el primer dispositivo disponible**](#Method-1:-get-first-available-device)<br />En esta sección aprenderás a usar **GetDefaultAsync** para obtener acceso al primer dispositivo disponible en una clase de dispositivo PointOfService específica.

**Método 2:** [**Instantánea de dispositivos**](#Method-2:-Snapshot-of-devices)<br />En esta sección aprenderás a enumerar una instantánea de dispositivos PointOfService que están presentes en el sistema en un punto determinado en el tiempo. Esto resulta útil cuando quieres compilar tu propia interfaz de usuario o necesitas enumerar dispositivos sin mostrar una interfaz de usuario al usuario. FindAllAsync retendrá los resultados hasta que se complete toda la enumeración.

**Método 3:** [**Enumerar y ver**](#Method-3:-Enumerate-and-watch)<br />En esta sección conocerás un modelo de enumeración más eficaz y flexible que te permite enumerar dispositivos que están presentes actualmente y también recibir notificaciones cuando se agregan dispositivos o se eliminan del sistema.  Esto es útil cuando quieres mantener una lista actual de dispositivos en segundo plano para mostrarlas en tu interfaz de usuario en lugar de esperar a que se produzca una instantánea.
 

---
## <a name="define-a-device-selector"></a>Definir un selector de dispositivos
Un selector de dispositivos permite limitar los dispositivos en los que se realizan búsquedas al enumerar los dispositivos.  Esto te permitirá obtener únicamente los resultados pertinentes y reducir el tiempo necesario que se tarda en enumerar los dispositivos deseados.  

El uso de [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) te proporcionará un selector para enumerar todas las POSPrinters conectadas al sistema, incluidas las POSPrinters conectadas por USB, red y Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();   

```

Usando el método [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector_Windows_Devices_PointOfService_PosConnectionTypes_) que toma [**PosConnectionTypes**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) como parámetro, puedes restringir tu selector para enumerar las POSPrinters locales, de red o conectadas por Bluetooth reduciendo el tiempo en que tarda la consulta en completarse.  El siguiente ejemplo muestra el uso de este método para definir un selector que admita solo POSPrinters conectadas localmente.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);   

```
> [!TIP]
> Consulta [**Crear un selector de dispositivos**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para crear cadenas de selector más avanzadas.

---

## <a name="method-1-get-first-available-device"></a>Método 1: obtener el primer dispositivo disponible

La forma más sencilla de obtener un dispositivo PointOfService es usar **GetDefaultAsync** para obtener el primer dispositivo disponible dentro de una clase de dispositivo PointOfService. 

El ejemplo siguiente muestra el uso de [**GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para BarcodeScanner. El patrón de codificación es similar para todas las clases de dispositivos PointOfService.

```Csharp

using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();

```

> [!CAUTION]
> GetDefaultAsync se debe usar con cuidado ya que puede devolver un dispositivo diferente de una sesión a la siguiente. Muchos eventos pueden influir en esta enumeración dando como resultado un primer dispositivo disponible diferente, incluyendo lo siguiente: 
> - Cambio en cámaras conectadas al equipo 
> - Cambiar en dispositivos PointOfService conectados al equipo
> - Cambio en dispositivos PointOfService conectados a la red disponibles en la red
> - Cambio en dispositivos PointOfService conectados por Bluetooth dentro del alcance de tu equipo 
> - Cambios a la configuración de PointOfService 
> - Instalación de controladores u objetos de servicio OPOS
> - Instalación de extensiones PointOfService
> - Actualizar al sistema operativo Windows

---

## <a name="method-2-snapshot-of-devices"></a>Método 2: instantánea de dispositivos

En algunos escenarios es posible que quieras compilar tu propia interfaz de usuario o que necesites enumerar dispositivos sin mostrar una interfaz de usuario al usuario.  En estas situaciones, puedes enumerar una instantánea de dispositivos que están actualmente conectados o emparejados con el sistema mediante [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Este método retendrá resultados hasta que se complete toda la enumeración.

> [!TIP]
> Se recomienda usar el método GetDeviceSelector con el parámetro PosConnectionTypes cuando uses FindAllAsync para limitar la consulta para el tipo de conexión deseada.  Las conexiones de red y por Bluetooth pueden retrasar los resultados ya que sus enumeraciones se deben completar antes de que se devuelvan resultados FindAllAsync.

>[!CAUTION] 
>FindAllAsync devuelve una matriz de dispositivos.  El orden de esta matriz puede cambiar de una sesión a otra, por lo tanto, no se recomienda depender de un orden específico usando un índice codificado de forma rígida en la matriz.  Usa las propiedades DeviceInformation para filtrar tus resultados o proporciona una interfaz de usuario desde la que elija el usuario.

En este ejemplo usa el selector definido anteriormente para tomar una instantánea de dispositivos mediante FindAllAsync. A continuación, enumera a través de cada uno de los elementos devueltos por la colección y escribe el nombre del dispositivo y el id. en la salida de depuración. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Cuando se trabaja con las API [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), con frecuencia deberás usar objetos [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obtener información sobre un dispositivo específico. Por ejemplo, la propiedad [**DeviceInformation.ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) se puede usar para recuperar y volver a usar el mismo dispositivo si está disponible en una sesión futura y la propiedad [**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) se puede usar para fines de visualización en la aplicación.  Consulta la página de referencia [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obtener información acerca de las propiedades adicionales disponibles.

---

## <a name="method-3-enumerate-and-watch"></a>Método 3: enumerar y ver

Para enumerar dispositivos mediante un método eficaz y flexible, crea una clase [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Un observador de dispositivo enumera dispositivos de forma dinámica, para que la aplicación reciba notificaciones si se agregan, quitan o cambian dispositivos una vez completada la enumeración inicial.  Un DeviceWatcher te permitirá detectar cuándo un dispositivo conectado a la red está en línea, un dispositivo Bluetooth esté dentro del alcance, así como si hay un dispositivo conectado de forma local para que puedas tomar las medidas adecuadas dentro de la aplicación.

En este ejemplo se usa el selector definido anteriormente para crear un DeviceWatcher y también define controladores de eventos para las notificaciones de agregado, quitado y actualizado. Deberás rellenar los detalles de las acciones que deseas realizar tras cada notificación.

```Csharp

using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> Consulta [**Enumerar y ver dispositivos**]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obtener más detalles sobre el uso de un DeviceWatcher.

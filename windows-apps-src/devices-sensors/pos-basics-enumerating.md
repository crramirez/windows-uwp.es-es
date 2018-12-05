---
title: Enumeración de dispositivos PointOfService
description: Más información acerca de cómo enumerar dispositivos PointOfService
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 7759186d45d3488336a1b793d173d6d1f21aa601
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691839"
---
# <a name="enumerating-point-of-service-devices"></a>Enumeración de dispositivos de punto de servicio
En esta sección aprenderás cómo [definir un selector de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) que se usa para consultar los dispositivos disponibles para el sistema y cómo usar este selector para enumerar los dispositivos de punto de servicio mediante uno de los métodos siguientes:

**Método 1:** [Usa un selector de dispositivos](#method-1:-use-a-device-picker)
<br/>
Mostrar un selector de dispositivos de la interfaz de usuario y que el usuario elige un dispositivo conectado. Este método controla la actualización de la lista cuando se adjunta y se quitan dispositivos y es más sencilla y más segura que otros métodos.

**Método 2:** [Obtener el primer dispositivo disponible](#Method-1:-get-first-available-device)<br />Usar [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) para tener acceso al primer dispositivo disponible en una clase de dispositivo de punto de servicio específico.

**Método 3:** [Instantánea de dispositivos](#Method-2:-Snapshot-of-devices)<br />Enumerar una instantánea de dispositivos de punto de servicio que están presentes en el sistema en un punto determinado en el tiempo. Esto resulta útil cuando quieres compilar tu propia interfaz de usuario o necesitas enumerar dispositivos sin mostrar una interfaz de usuario al usuario. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) retendrá resultados hasta que se complete toda la enumeración.

**Método 4:** [Enumerar y ver](#Method-3:-Enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) es un modelo de enumeración más eficaz y flexible que te permite enumerar los dispositivos que están presentes actualmente y recibir notificaciones cuando los dispositivos se agregan o eliminan del sistema.  Esto es útil cuando quieres mantener una lista actual de dispositivos en segundo plano para mostrarlas en tu interfaz de usuario en lugar de esperar a que se produzca una instantánea.

## <a name="define-a-device-selector"></a>Definir un selector de dispositivos
Un selector de dispositivos permite limitar los dispositivos en los que se realizan búsquedas al enumerar los dispositivos.  Esto te permitirá únicamente a obtener los resultados pertinentes y reducir el tiempo que se tarda en enumerar los dispositivos deseados.

Puedes usar el método **GetDeviceSelector** para el tipo de dispositivo que estás buscando para obtener el selector de dispositivos de ese tipo. Por ejemplo, usando [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) proporcionará con un selector para enumerar todas las [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) conectados al sistema, incluyendo USB, red e impresoras POS de Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Los métodos de **GetDeviceSelector** para los distintos tipos de dispositivos son:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Mediante el método **GetDeviceSelector** que toma un valor de [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) como un parámetro, puedes restringir tu selector para enumerar local, red o dispositivos de POS conectado por Bluetooth, lo que reduce el tiempo que tarda la consulta en completarse.  El siguiente ejemplo muestra un uso de este método para definir un selector que admita solo localmente conectadas impresoras POS.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Consulta [Crear un selector de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para crear cadenas de selector más avanzadas.

## <a name="method-1-use-a-device-picker"></a>Método 1: Usar un selector de dispositivos

La clase [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) te permite mostrar un control flotante del selector que contiene una lista de dispositivos para el usuario puede elegir. Puedes usar la propiedad de [filtro](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) para elegir qué tipos de dispositivos para mostrar en el selector. Esta propiedad es de tipo [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Puedes agregar tipos de dispositivos para el filtro mediante la propiedad [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) o [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) .

Cuando estés listo para mostrar el selector de dispositivo, llamar al método [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) , que se muestra el selector de la interfaz de usuario y devolver el dispositivo seleccionado. Tendrás que especificar [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) que determina dónde aparece el control flotante. Este método devolverá un objeto [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) , por lo tanto, para usarlo con el punto de servicio de API, tendrás que usar el método **FromIdAsync** para la clase de dispositivo en particular que quieras. Puedes pasan la propiedad [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) como el parámetro del método *deviceId* y obtén una instancia de la clase de dispositivo como el valor devuelto.

El siguiente fragmento de código crea un **DevicePicker**, agrega un filtro de escáner de códigos de barras, tiene el usuario elija un dispositivo y, a continuación, crea un objeto **BarcodeScanner** basado en el identificador de dispositivo:

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>Método 2: Obtener el primer dispositivo disponible

La forma más sencilla de obtener un dispositivo de punto de servicio es usar **GetDefaultAsync** para obtener el primer dispositivo disponible dentro de una clase de dispositivo de punto de servicio. 

El siguiente ejemplo muestra el uso de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). El patrón de codificación es similar para todas las clases de dispositivo de punto de servicio.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** se debe usar con cuidado, ya que puede devolver un dispositivo diferente de una sesión a la siguiente. Muchos eventos pueden influir en esta enumeración dando como resultado un primer dispositivo disponible diferente, incluyendo lo siguiente: 
> - Cambio en cámaras conectadas al equipo 
> - Cambiar el punto de entrada de dispositivos de servicio conectados al equipo
> - Cambio en dispositivos de punto de servicio conectado a la red disponibles en la red
> - Cambio en dispositivos de punto de servicio de Bluetooth dentro del alcance del equipo 
> - Cambios en la configuración de punto de servicio 
> - Instalación de controladores u objetos de servicio OPOS
> - Instalación de extensiones de punto de servicio
> - Actualizar al sistema operativo Windows

## <a name="method-3-snapshot-of-devices"></a>Método 3: Instantánea de dispositivos

En algunos escenarios es posible que quieras compilar tu propia interfaz de usuario o que necesites enumerar dispositivos sin mostrar una interfaz de usuario al usuario.  En estas situaciones, puedes enumerar una instantánea de dispositivos que están actualmente conectados o emparejados con el sistema mediante [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Este método retendrá resultados hasta que se complete toda la enumeración.

> [!TIP]
> Se recomienda usar el método **GetDeviceSelector** con el parámetro **PosConnectionTypes** cuando uses **FindAllAsync** para limitar la consulta para el tipo de conexión deseada.  Conexiones de red y Bluetooth pueden retrasar los resultados como sus enumeraciones deben completar antes de que se devuelven resultados **FindAllAsync** .

> [!CAUTION] 
> **FindAllAsync** devuelve una matriz de dispositivos.  El orden de esta matriz puede cambiar de una sesión a otra, por lo tanto, no se recomienda depender de un orden específico usando un índice codificado de forma rígida en la matriz.  Usa las propiedades [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para filtrar los resultados o proporciona una interfaz de usuario para el usuario puede elegir.

En este ejemplo usa el selector definido anteriormente para tomar una instantánea de dispositivos mediante **FindAllAsync** , a continuación, enumera a través de cada uno de los elementos devueltos por la colección y escribe el nombre del dispositivo y el identificador en el resultado de depuración. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Cuando se trabaja con las API [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), con frecuencia deberás usar objetos [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obtener información sobre un dispositivo específico. Por ejemplo, la propiedad [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) puede usarse para recuperar y volver a usar el mismo dispositivo si está disponible en una sesión futura y la propiedad [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) puede usarse para fines de visualización en la aplicación.  Consulta la página de referencia [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obtener información acerca de las propiedades adicionales disponibles.

## <a name="method-4-enumerate-and-watch"></a>Método 4: Enumerar y ver

Para enumerar dispositivos mediante un método eficaz y flexible, crea una clase [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Un observador de dispositivo enumera dispositivos de forma dinámica, para que la aplicación reciba notificaciones si se agregan, quitan o cambian dispositivos una vez completada la enumeración inicial.  Un **DeviceWatcher** te permitirá detectar cuándo un dispositivo conectado a la red en línea, un dispositivo Bluetooth esté dentro del alcance, así como si se desconecta un dispositivo conectado de forma local para que pueda realizar la acción apropiada dentro de la aplicación.

En este ejemplo usa el selector definido anteriormente para crear un **DeviceWatcher** como define controladores de eventos para las notificaciones de [agregado](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [quitado](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)y [actualizado](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) . Deberás rellenar los detalles de las acciones que deseas realizar tras cada notificación.

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
> Consulta [enumerar y ver dispositivos]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obtener más información sobre el uso de un **DeviceWatcher**.

## <a name="see-also"></a>Ver también
* [Tareas iniciales con punto de servicio](pos-basics.md)
* [Clase DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Clase PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [Enumeración PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Clase BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Clase DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]
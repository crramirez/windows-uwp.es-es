---
title: Enumerar dispositivos PointOfService
description: Obtenga información sobre cuatro métodos para usar un selector de dispositivos con el fin de consultar y enumerar los dispositivos PointOfService disponibles para el sistema.
ms.date: 10/08/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8fc869ab16fd745da32572a54354aeeca8abb14c
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970303"
---
# <a name="enumerating-point-of-service-devices"></a>Enumerar dispositivos de punto de servicio
En esta sección, aprenderá a [definir un selector de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) que se utiliza para consultar los dispositivos disponibles en el sistema y usar este selector para enumerar los dispositivos de punto de servicio mediante uno de los métodos siguientes:

**Método 1:** [usar un selector de dispositivos](#method-1-use-a-device-picker)
<br/>
Mostrar una interfaz de usuario del selector de dispositivos y hacer que el usuario elija un dispositivo conectado. Este método controla la actualización de la lista cuando los dispositivos se adjuntan y se quitan, y es más sencillo y más seguro que otros métodos.

**Método 2:** [obtener el primer dispositivo disponible](#method-2-get-first-available-device)<br />Use [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) para tener acceso al primer dispositivo disponible en una clase de dispositivo de punto de servicio específica.

**Método 3:** [instantánea de dispositivos](#method-3-snapshot-of-devices)<br />Enumerar una instantánea de los dispositivos de punto de servicio que están presentes en el sistema en un momento dado. Esto resulta útil si desea crear su propia interfaz de usuario o tener que enumerar los dispositivos sin mostrar una interfaz de usuario para el usuario. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) retendrá los resultados hasta que se complete toda la enumeración.

**Método 4:** [enumeración y inspección](#method-4-enumerate-and-watch)<br />La [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) es un modelo de enumeración más eficaz y flexible que le permite enumerar los dispositivos que están actualmente presentes y recibir también notificaciones cuando se agregan o quitan dispositivos del sistema.  Esto resulta útil si desea mantener una lista actual de dispositivos en segundo plano para mostrarlo en la interfaz de usuario en lugar de esperar a que se produzca una instantánea.

## <a name="define-a-device-selector"></a>Definir un selector de dispositivos
Un selector de dispositivos permite limitar los dispositivos en los que se realiza la búsqueda al enumerar los dispositivos.  Esto le permitirá obtener solo los resultados relevantes y reducir el tiempo necesario para enumerar los dispositivos deseados.

Puede usar el método **GetDeviceSelector** para el tipo de dispositivo que está buscando para obtener el selector de dispositivos para ese tipo. Por ejemplo, el uso de [PosPrinter. GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) le proporcionará un selector para enumerar todos los [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) conectados al sistema, incluidas las impresoras USB, de red y de PDV de Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Los métodos de **GetDeviceSelector** para los diferentes tipos de dispositivo son:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Mediante el uso de un método **GetDeviceSelector** que toma un valor de [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) como parámetro, puede restringir el selector para enumerar los dispositivos de PDV locales, de red o conectados a Bluetooth, lo que reduce el tiempo que tarda en completarse la consulta.  En el ejemplo siguiente se muestra el uso de este método para definir un selector que solo admite impresoras de PDV conectadas localmente.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Consulte [creación de un selector de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para crear cadenas de selector más avanzadas.

## <a name="method-1-use-a-device-picker"></a>Método 1: usar un selector de dispositivos

La clase [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) permite mostrar un control flotante selector que contiene una lista de dispositivos para que el usuario elija. Puede usar la propiedad [Filter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) para elegir los tipos de dispositivos que se van a mostrar en el selector. Esta propiedad es de tipo [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Puede agregar tipos de dispositivos al filtro mediante la propiedad [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) o [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) .

Cuando esté listo para mostrar el selector de dispositivos, puede llamar al método [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) , que mostrará la interfaz de usuario del selector y devolverá el dispositivo seleccionado. Deberá especificar un [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) que determine dónde aparece el control flotante. Este método devolverá un objeto [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) , por lo que para usarlo con las API de punto de servicio, deberá usar el método **FromIdAsync** para la clase de dispositivo particular que desee. La propiedad [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) se pasa como el parámetro *deviceId* del método y se obtiene una instancia de la clase Device como el valor devuelto.

El siguiente fragmento de código crea un objeto **DevicePicker**, agrega un filtro de escáner de código de barras a él, tiene el usuario selecciona un dispositivo y, a continuación, crea un objeto **BarcodeScanner** basado en el identificador de dispositivo:

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

## <a name="method-2-get-first-available-device"></a>Método 2: obtener el primer dispositivo disponible

La manera más sencilla de obtener un dispositivo de punto de servicio es usar **GetDefaultAsync** para obtener el primer dispositivo disponible dentro de una clase de dispositivo de punto de servicio. 

En el ejemplo siguiente se muestra el uso de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). El patrón de codificación es similar para todas las clases de dispositivo de punto de servicio.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** debe usarse con cuidado, ya que puede devolver un dispositivo diferente de una sesión a la siguiente. Muchos eventos pueden influir en esta enumeración, lo que da lugar a un primer dispositivo disponible diferente, incluidos: 
> - Cambio en las cámaras conectadas al equipo 
> - Cambiar en los dispositivos de punto de servicio conectados al equipo
> - Cambio en los dispositivos de punto de servicio conectados a la red disponibles en la red
> - Cambio en los dispositivos de punto de servicio de Bluetooth dentro del alcance del equipo 
> - Cambios en la configuración del punto de servicio 
> - Instalación de controladores o objetos de servicio OPOS
> - Instalación de extensiones de punto de servicio
> - Actualizar al sistema operativo Windows

## <a name="method-3-snapshot-of-devices"></a>Método 3: instantánea de dispositivos

En algunos escenarios, puede que desee crear su propia interfaz de usuario o tener que enumerar los dispositivos sin mostrar una interfaz de usuario para el usuario.  En estas situaciones, puede enumerar una instantánea de los dispositivos que están conectados actualmente o emparejados con el sistema mediante [DeviceInformation. FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Este método retendrá los resultados hasta que se complete toda la enumeración.

> [!TIP]
> Se recomienda usar el método **GetDeviceSelector** con el parámetro **PosConnectionTypes** al usar **FindAllAsync** para limitar la consulta al tipo de conexión deseado.  Las conexiones de red y Bluetooth pueden retrasar los resultados, ya que sus enumeraciones deben completarse antes de que se devuelvan los resultados de **FindAllAsync** .

> [!CAUTION] 
> **FindAllAsync** devuelve una matriz de dispositivos.  El orden de esta matriz puede cambiar de una sesión a otro, por lo que no se recomienda depender de un orden específico mediante un índice codificado en la matriz.  Use las propiedades de [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para filtrar los resultados o proporcionar una interfaz de usuario para que el usuario elija.

En este ejemplo se usa el selector definido anteriormente para tomar una instantánea de los dispositivos que usan **FindAllAsync** y, a continuación, se enumeran todos los elementos devueltos por la colección y se escribe el nombre y el identificador del dispositivo en la salida de depuración. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Al trabajar con las API de [Windows. Devices. Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) , a menudo necesitará usar objetos [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obtener información acerca de un dispositivo específico. Por ejemplo, la propiedad [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) se puede usar para recuperar y volver a usar el mismo dispositivo si está disponible en una sesión futura y la propiedad [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) se puede usar para la presentación en la aplicación.  Vea la página de referencia de [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) para obtener información sobre las propiedades adicionales disponibles.

## <a name="method-4-enumerate-and-watch"></a>Método 4: enumeración y inspección

Para enumerar dispositivos mediante un método eficaz y flexible, crea una clase [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Un monitor de dispositivo enumera los dispositivos de forma dinámica, por lo que la aplicación recibe notificaciones si se agregan, quitan o cambian dispositivos después de que se complete la enumeración inicial.  Una **DeviceWatcher** le permitirá detectar cuándo se conecta un dispositivo conectado a la red, un dispositivo Bluetooth, y también si se desconecta un dispositivo conectado localmente para que pueda llevar a cabo la acción adecuada en la aplicación.

En este ejemplo se usa el selector definido anteriormente para crear una **DeviceWatcher** y se definen los controladores de eventos para las notificaciones [agregadas](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [eliminadas](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)y [actualizadas](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) . Tendrá que rellenar los detalles de las acciones que desea realizar en cada notificación.

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
> Consulte [enumeración y inspección de dispositivos]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obtener más detalles sobre el uso de una **DeviceWatcher**.

## <a name="see-also"></a>Consulte también
* [Introducción al punto de servicio](pos-basics.md)
* [DeviceInformation (clase)](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Clase PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [Enumeración PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Clase BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher (clase)](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]
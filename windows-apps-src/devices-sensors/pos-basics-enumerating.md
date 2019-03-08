---
title: Enumeración de dispositivos PointOfService
description: Más información acerca de cómo enumerar dispositivos PointOfService
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620910"
---
# <a name="enumerating-point-of-service-devices"></a>Enumeración de dispositivos de punto de servicio
En esta sección, aprenderá cómo [definir un selector de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) que se usa para consultar dispositivos disponibles para el sistema y use este selector para enumerar los dispositivos de punto de servicio mediante uno de los métodos siguientes:

**Método 1:** [Usar un selector de dispositivos](#method-1-use-a-device-picker)
<br/>
Mostrar un selector de dispositivos de interfaz de usuario y que el usuario elija un dispositivo conectado. Este método controla la actualización de la lista cuando se adjunta y se quitan los dispositivos y es más sencillo y más seguros que otros métodos.

**Método 2:** [Obtener el primer dispositivo disponible](#method-2-get-first-available-device)<br />Use [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) para tener acceso al primer dispositivo disponible en una clase de dispositivo de punto de servicio específica.

**Método 3:** [Instantánea de dispositivos](#method-3-snapshot-of-devices)<br />Enumerar una instantánea de los dispositivos de punto de servicio que se encuentran en el sistema en un momento dado en el tiempo. Esto resulta útil cuando quieres compilar tu propia interfaz de usuario o necesitas enumerar dispositivos sin mostrar una interfaz de usuario al usuario. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) retendrá los resultados hasta que se complete toda la enumeración.

**Método 4:** [Enumerar y ver](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) es un modelo de enumeración más eficaz y flexible que le permite enumerar los dispositivos que están actualmente presentes y también recibir notificaciones cuando los dispositivos se agregan o quitan del sistema.  Esto es útil cuando quieres mantener una lista actual de dispositivos en segundo plano para mostrarlas en tu interfaz de usuario en lugar de esperar a que se produzca una instantánea.

## <a name="define-a-device-selector"></a>Definir un selector de dispositivos
Un selector de dispositivos permite limitar los dispositivos en los que se realizan búsquedas al enumerar los dispositivos.  Esto le permitirá solo obtener resultados pertinentes y reducir el tiempo necesario para enumerar los dispositivos deseados.

Puede usar el **GetDeviceSelector** método para el tipo de dispositivo que está buscando para obtener el selector de dispositivos para ese tipo. Por ejemplo, mediante [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) le proporcionará un selector para enumerar todos los [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) conectados al sistema, incluidas las impresoras Bluetooth POS, red y USB.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

El **GetDeviceSelector** métodos para los distintos tipos de dispositivos son:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Mediante un **GetDeviceSelector** método que toma un [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) valor como un parámetro, puede restringir el selector para enumerar local, de red o dispositivos de punto de conexión Bluetooth, lo que reduce el tiempo necesario para que la consulta se complete.  El ejemplo siguiente se muestra un uso de este método para definir un selector que admite solo localmente conectado impresoras de PDV.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Consulte [crear un selector de dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) para crear más avanzadas de cadenas de selector.

## <a name="method-1-use-a-device-picker"></a>Método 1: Usar un selector de dispositivos

El [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) clase le permite mostrar un control de selector de flotante que contiene una lista de dispositivos del usuario elegir. Puede usar el [filtro](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) propiedad para elegir qué tipos de dispositivos para mostrar en el selector. Esta propiedad es de tipo [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Puede agregar tipos de dispositivo para el filtro mediante la [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) o [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) propiedad.

Cuando esté listo para mostrar el selector de dispositivos, puede llamar a la [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) método, que se muestra el selector de interfaz de usuario y devuelve el dispositivo seleccionado. Debe especificar un [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) determinará dónde aparece la ventana flotante. Este método devolverá un [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) objeto, por lo que para usarlo con el punto de servicio de API, deberá usar el **FromIdAsync** método para la clase de dispositivo en particular que desee. Pasa el [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) propiedad como el método *deviceId* parámetro y obtener una instancia de la clase de dispositivo como el valor devuelto.

El siguiente fragmento de código crea un **DevicePicker**, agrega un filtro de analizador de código de barras, tiene el usuario selecciona un dispositivo y, a continuación, se crea un **BarcodeScanner** objeto según el identificador de dispositivo:

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

La manera más sencilla de obtener un dispositivo de punto de servicio es usar **GetDefaultAsync** para obtener el primer dispositivo disponible dentro de una clase de dispositivo de punto de servicio. 

El ejemplo siguiente muestra el uso de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) para [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). El patrón de codificación es similar para todas las clases de dispositivos de punto de servicio.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** debe utilizarse con cuidado, como un dispositivo diferente puede devolver desde una sesión a la siguiente. Muchos eventos pueden influir en esta enumeración dando como resultado un primer dispositivo disponible diferente, incluyendo lo siguiente: 
> - Cambio en cámaras conectadas al equipo 
> - Cambiar punto de entrada de servicio de dispositivos conectado al equipo
> - Cambio en los dispositivos de punto de servicio conectado a la red disponibles en la red
> - Cambio en los dispositivos Bluetooth Point of Service dentro del alcance del equipo 
> - Cambios realizados en la configuración del punto de servicio 
> - Instalación de controladores u objetos de servicio OPOS
> - Instalación de extensiones de punto de servicio
> - Actualizar al sistema operativo Windows

## <a name="method-3-snapshot-of-devices"></a>Método 3: Instantánea de dispositivos

En algunos escenarios es posible que quieras compilar tu propia interfaz de usuario o que necesites enumerar dispositivos sin mostrar una interfaz de usuario al usuario.  En estas situaciones, puede enumerar una instantánea de los dispositivos que están actualmente conectadas o emparejado con el sistema mediante [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Este método retendrá resultados hasta que se complete toda la enumeración.

> [!TIP]
> Se recomienda usar la **GetDeviceSelector** método con el **PosConnectionTypes** al usar **FindAllAsync** para limitar la consulta para el tipo de conexión deseado.  Conexiones de red y Bluetooth pueden retrasar los resultados como sus enumeraciones deben completarse antes de **FindAllAsync** los resultados se devuelven.

> [!CAUTION] 
> **FindAllAsync** devuelve una matriz de dispositivos.  El orden de esta matriz puede cambiar de una sesión a otra, por lo tanto, no se recomienda depender de un orden específico usando un índice codificado de forma rígida en la matriz.  Use [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) propiedades para filtrar los resultados o proporcionar una interfaz de usuario para el usuario elegir.

Este ejemplo utiliza el selector definido anteriormente para crear una instantánea de los dispositivos mediante **FindAllAsync** , a continuación, enumera a través de cada uno de los elementos devueltos por la colección y escribe el nombre del dispositivo y el identificador en la salida de depuración. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Cuando se trabaja con el [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API, con frecuencia necesitará usar [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) objetos para obtener información sobre un dispositivo específico. Por ejemplo, el [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) propiedad puede usarse para recuperar y volver a usar el mismo dispositivo si está disponible en una sesión futura y la [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) propiedad puede utilizarse para mostrar con fines de la aplicación.  Consulte la [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) página de referencia para obtener información acerca de las propiedades adicionales disponibles.

## <a name="method-4-enumerate-and-watch"></a>Método 4: Enumerar y ver

Es la creación de un método más eficaz y flexible de enumerar dispositivos un [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Un observador de dispositivo enumera dispositivos de forma dinámica, para que la aplicación reciba notificaciones si se agregan, quitan o cambian dispositivos una vez completada la enumeración inicial.  Un **DeviceWatcher** le permitirá detectar cuándo una red conectada dispositivo está en línea, un dispositivo Bluetooth está en intervalo, así como si se desconecta un dispositivo conectado localmente para que pueda tomar la acción apropiada dentro de su aplicación.

Este ejemplo utiliza el selector definido anteriormente para crear un **DeviceWatcher** , así como define controladores de eventos para el [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [quitado](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed), y [actualizada ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) notificaciones. Deberás rellenar los detalles de las acciones que deseas realizar tras cada notificación.

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
> Consulte [Enumerate y vea dispositivos]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) para obtener más detalles sobre el uso de un **DeviceWatcher**.

## <a name="see-also"></a>Consulte también
* [Introducción a punto de servicio](pos-basics.md)
* [Clase DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Clase PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [Enumeración PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Clase BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Clase DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]
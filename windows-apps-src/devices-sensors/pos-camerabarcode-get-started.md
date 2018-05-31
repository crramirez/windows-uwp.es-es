---
author: TerryWarwick
title: Tareas iniciales con un escáner de código de barras basado en cámara
description: Aprender a utilizar el escáner de código de barras basado en cámara
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 7bbb26fb3c977917732a079f28d274f7f2bfba41
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833363"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Tareas iniciales con un escáner de código de barras basado en cámara
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Paso 1: agregar declaraciones de funcionalidad al manifiesto de la aplicación
1. En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2. Selecciona la pestaña **Funcionalidades**
3. Marca las casillas de **Cámara web** y **PointOfService** 

>[!NOTE] 
> La funcionalidad **Cámara Web** es necesaria para que el descodificador de software reciba los fotogramas a descodificar, así como para proporcionar una vista previa de la aplicación

## <a name="step-2-add-using-directives"></a>Paso 2: agregar directivas de uso

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>Paso 3: definir el selector de dispositivos

### **<a name="option-a-find-all-barcode-scanners"></a>Opción A: Buscar todos los escáneres de códigos de barras**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### **<a name="option-b-scoping-device-selector-to-connection-type"></a>Opción B: Filtrar el selector de dispositivos según el tipo de conexión**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>Paso 4: Enumerar los escáneres de códigos de barras
Si no esperas que la lista de dispositivos cambie durante el ciclo de vida de la aplicación, puedes enumerar una instantánea una sola vez con *FindAllAsync*, pero si crees que la lista de escáneres de códigos de barras podría cambiar durante el ciclo de vida de la aplicación, debes usar un *DeviceWatcher* en su lugar.  

> [!Important] 
> El uso de GetDefaultAsync para enumerar dispositivos PointOfService puede provocar un comportamiento incoherente, ya que solo devuelve el primer dispositivo que se encuentra en la clase y esto cambia de una sesión a otra.

### **<a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>Opción A: enumerar una instantánea de escáneres de códigos de barras**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Consulta [*Enumerar una instantánea de dispositivos*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) para obtener más información sobre el uso de *FindAllAsync*.

### **<a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>Opción B: enumerar y vigilar los cambios en los escáneres de códigos de barras disponibles**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> Consulta [*Enumerar y observar cambios en dispositivo*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) y [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) para obtener más información.

## <a name="step-5-identify-camera-barcode-scanners"></a>Paso 5: identificar escáneres de códigos de barras basados en cámara
Un escáner de código de barras basado en cámara se crea dinámicamente cuando Windows empareja las cámaras conectadas a tu equipo con un descodificador de software.  Cada conjunto descodificador-equipo es un escáner de código de barras totalmente funcional.

Para cada escáner de código de barras de la colección de dispositivos resultante, puedes determinar los que están basados en cámara en comparación con los escáneres físicos, comprobando la propiedad [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId).  Un VideoDeviceID no NULL indicará que el objeto de escáner de código de barras de la colección del dispositivo es un escáner de códigos de barras basado en cámara.  Si tienes más de un escáner de código de barras basado en cámara, puedes crear una colección independiente que excluya los escáneres de código de barras físicos. 

Los escáneres de código de barras basados en cámara que usan el descodificador que viene con Windows aparecerán como: 

> Microsoft BarcodeScanner (*nombre de tu cámara*)

Si la cámara está integrada en el chasis del equipo, el nombre puede diferenciar entre *frontal* y *trasera* si tienes más de una cámara.  En el futuro, es posible que haya descodificadores de software adicionales y que sigan un método de asignación de nombre diferente.

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Paso 6: reclamar el escáner de código de barras basado en cámara 
Usa [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) para obtener el uso exclusivo del escáner de código de barras basado en cámara.

## <a name="step-7-system-provided-preview"></a>Paso 7: vista previa proporcionada por el sistema
Una vista previa de cámara es necesaria para que el usuario apunte correctamente a los códigos de barras con la cámara.  Windows proporciona una sencilla vista previa de cámara que iniciará un cuadro de diálogo para ofrecer un control básico del escáner de código de barras basado en cámara.  Basta con llamar a [ClaimedBarcodeScanner.ShowideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) para abrir el cuadro de diálogo y a [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) para cerrarlo al finalizar.

> [!TIP]
> Consulta [Vista previa de hospedaje](pos-camerabarcode-hosting-preview.md) para hospedar en tu aplicación la vista previa del escáner de código de barras basado en cámara.
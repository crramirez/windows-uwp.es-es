---
author: TerryWarwick
title: Tareas iniciales con el escáner de códigos de barra
description: Obtén información sobre cómo interactuar con un escáner de código de barras desde una aplicación universal de Windows
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0fddfa3aa78c274735634315b1230b0893020805
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874203"
---
# <a name="getting-started-with-barcode-scanners"></a>Tareas iniciales con un escáner de códigos de barra

Obtén información sobre cómo interactuar con un escáner de código de barras desde una aplicación universal de Windows.  Este tema proporciona información sobre las funciones específicas del escáner de código de barras.

## <a name="configuring-your-barcode-scanner"></a>Configuración del escáner de códigos de barra
Los escáneres de código de barras se pueden configurar en varios modos diferentes.  Es importante configurar correctamente el escáner de códigos de barra para la aplicación deseada.  Muchos de los escáneres de códigos de barra se pueden configurar en modo **cuñas de teclado**, lo hace que el escáner aparezca como un teclado de Windows.  Esto te permite digitalizar códigos de barra en aplicaciones que no están preparadas para ello, como el Bloc de notas.  Al digitalizar un código de barras en este modo, los datos descodificados desde el escáner se introducen en el punto de inserción como si hubieras escrito los datos mediante el teclado.  Si quieres tener más control sobre el escáner desde la aplicación para UWP, tendrás que configurarlo en un modo diferente al de cuña de teclado.

### <a name="usb-barcode-scanner"></a>Escáner de código de barras USB
Un escáner de código de barras conectado por USB debe configurarse en modo **HID POS Scanner** para que funcione con el controlador del escáner de código de barras que se incluye en Windows. Este controlador es una implementación de la especificación de **tablas de uso de punto de venta HID** publicada para [**USB HID**](http://www.usb.org/developers/hidpage/).  Consulta la documentación del escáner o ponte en contacto con el fabricante para obtener instrucciones sobre cómo habilitar el modo **HID POS Scanner**.  Una vez configurado como **HID POS Scanner**, el escáner de código de barras aparecerá en el Administrador de dispositivos, bajo el nodo **POS Barcode Scanner** como **escáner de código de barras POS HID**.
El fabricante del escáner de código de barras también puede tener un controlador específico del proveedor que sea compatible con las API de escáner de código de barras de UWP que usen un modo distinto al de **HID POS Scanner**.  Si ya tiene instalado un controlador provisto por el fabricante compatible con las API de escáner de código de barras de UWP, puede ver un dispositivo específico de proveedor en la lista que aparece bajo **POS Barcode Scanner**, en el Administrador de dispositivos.

### <a name="bluetooth-barcode-scanner"></a>Escáner de código de barras con Bluetooth
Un escáner de código de barras conectado por Bluetooth debe configurarse en modo **protocolo de puerto serie - interfaz serie sencilla (SPP-SSI)** para funcionar con las API de escáner de código de barras de UWP.  Consulta la documentación del escáner o ponte en contacto con el fabricante para obtener instrucciones sobre cómo habilitar el modo **modo SPP-SSI**.  
Para poder usar el escáner de código de barras con Bluetooth debes emparejarlo en Configuración - Dispositivos - Bluetooth y otros dispositivos - Añadir Bluetooth u otro dispositivo.  
Puedes iniciar y controlar el emparejamiento usando el espacio de nombres **Windows.Devices.Enumeration**.  Para obtener más información, consulta [**Emparejar dispositivos**](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

## <a name="using-software-trigger-with-barcode-scanners"></a>Uso de desencadenante de software con escáneres de código de barras
### <a name="initiate-scan-from-software"></a>Iniciar digitalización desde software
Puede ser útil para controlar la digitalización desde el software si estás usando un escáner de código de barras en modo de presentación, o si el escáner no tiene un desencadenante físico, como los escáneres de código de barras basados en cámaras. Puedes iniciar el proceso de digitalización llamando a [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).  
Según el valor de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived), el escáner puede digitalizar solo un código de barras y parar, o digitalizar continuamente hasta llamar a [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Establece el valor deseado de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) para controlar el comportamiento del escáner cuando se descodifica un código de barras.

| Valor | Descripción |
| ----- | ----------- |
| Verdadero   | Escanear solo un código de barras y detener |
| Falso  | Escanear códigos de barra continuamente sin detenerse |


> [!Important]
> Confirma que el escáner de código de barras es compatible con el uso de desencadenante de software, primero comprobando la propiedad [**IsSoftwareTriggerSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

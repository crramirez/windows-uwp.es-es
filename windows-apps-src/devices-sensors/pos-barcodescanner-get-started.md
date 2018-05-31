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
ms.openlocfilehash: ed0fa79f5bbdfdaf8ca1f3273fa8d741f17efe1d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833366"
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

## <a name="working-with-symbologies"></a>El trabajo con simbologías
Una [**simbología de código de barras**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) es la asignación de datos a un formato específico de código de barras. Algunas simbologías comunes son UPC, Code 128, códigos QR, etc. Las API del escáner de código de barras de UWP permiten que una aplicación controle la forma en que el escáner procesa estas simbologías sin configurar manualmente el escáner. 

### <a name="determine-which-symbologies-are-supported"></a>Determine qué simbologías son compatibles 
Dado que la aplicación puede usarse con modelos de escáner de código de barras de diferentes fabricantes, quizá necesites consultar al escáner para determinar la lista de simbologías que admite.  Esto puede ser útil si tu aplicación requiere una simbología específica que puede no ser compatible con todos los escáneres, o si necesitas habilitar simbologías que se hayan deshabilitado manualmente o mediante programación en el escáner.
Una vez que tengas un objeto [**BarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) mediante el uso de [**BarcodeScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), llama a [**GetSupportedSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obtener una lista de simbologías admitidas por el dispositivo.

### <a name="determine-if-a-specific-symbology-is-supported"></a>Determina si se admite un simbología concreta
Para determinar si el escáner admite un simbología específica, puedes llamar a [**IsSymbologySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

### <a name="changing-which-symbologies-are-recognized"></a>Cambiar las simbologías reconocidas
En algunos casos, es aconsejable usar un subconjunto de simbologías que el escáner de código de barras admita.  Esto es especialmente útil para bloquear simbologías que no vayas a usar en la aplicación. Por ejemplo, para asegurarse de que un usuario escanea el código de barras correcto, podrías restringir la digitalización a CUP o EAN al adquirir SKU de elemento y a Code 128 al adquirir números de serie.
Cuando sepas qué simbologías admite tu escáner, puedes establecer las simbologías que quieres que reconozca.  Esto puede hacerse después de haber establecido un objeto [**ClaimedBarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) usando [**ClaimScannerAsyc**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Puedes llamar a [**SetActiveSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para permitir un conjunto específico de simbologías mientras están deshabilitadas las que no están en tu lista.

### <a name="restricting-scan-data-by-data-length"></a>Restricción de datos escaneados por su longitud
Algunas simbologías son de longitud variable, como Code 39 o Code 128.  Los códigos de barras de esta simbología pueden estar cerca de los que contengan datos diferentes, a menudo de longitudes específicas. Establecer la longitud específica de los datos que necesitas puede evitar escaneados no válidos.

| Método    | Descripción |
| :-------- | :---------- |
| [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | Te permite configurar un rango de longitudes deseadas de los datos descodificados y cómo maneja el escáner el dígito de control. |
| [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Te permite recuperar la longitud actual y comprobar las configuraciones de dígitos. |

> [!Important] 
> Confirma que el escáner de código de barras admite el uso de atributos de simbología, en primer lugar, comprobando las siguientes propiedades: [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) o [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Te permite recuperar la longitud actual y comprobar las configuraciones de dígitos. :
> - [**IsDecodeLengthSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [**ICheckDigitTransmissionSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [**IsCheckDigitValidationSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

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

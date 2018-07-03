---
author: TerryWarwick
title: Trabajar con simbologías del escáner de código de barras
description: Este artículo contiene información sobre simbologías del escáner de códigos de barras.
ms.author: jken
ms.date: 05/3/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: f6e03d62316a1b842330f39ac958e4471a895815
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874535"
---
# <a name="working-with-symbologies"></a>El trabajo con simbologías
Una [simbología de código de barras](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) es la asignación de datos a un formato específico de código de barras. Algunas simbologías comunes son UPC, Code 128, códigos QR, etc. Las API del escáner de código de barras de UWP permiten que una aplicación controle la forma en que el escáner procesa estas simbologías sin configurar manualmente el escáner. 

## <a name="determine-which-symbologies-are-supported"></a>Determine qué simbologías son compatibles 
Dado que la aplicación puede usarse con modelos de escáner de código de barras de diferentes fabricantes, quizá necesites consultar al escáner para determinar la lista de simbologías que admite.  Esto puede ser útil si tu aplicación requiere una simbología específica que puede no ser compatible con todos los escáneres, o si necesitas habilitar simbologías que se hayan deshabilitado manualmente o mediante programación en el escáner.
Una vez tengas un objeto [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) usando [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), llama a [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obtener una lista de simbologías admitidas por el dispositivo.

## <a name="determine-if-a-specific-symbology-is-supported"></a>Determina si se admite un simbología concreta
Para determinar si el escáner admite un simbología específica, puedes llamar a [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

## <a name="changing-which-symbologies-are-recognized"></a>Cambiar las simbologías reconocidas
En algunos casos, es aconsejable usar un subconjunto de simbologías que el escáner de código de barras admita.  Esto es especialmente útil para bloquear simbologías que no vayas a usar en la aplicación. Por ejemplo, para asegurarse de que un usuario escanea el código de barras correcto, podrías restringir la digitalización a CUP o EAN al adquirir SKU de elemento y a Code 128 al adquirir números de serie.
Cuando sepas qué simbologías admite tu escáner, puedes establecer las simbologías que quieres que reconozca.  Esto puede hacerse después de haber establecido un objeto [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) usando [ClaimScannerAsyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Puedes llamar a [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para permitir un conjunto específico de simbologías mientras están deshabilitadas las que no están en tu lista.

## <a name="restricting-scan-data-by-data-length"></a>Restricción de datos escaneados por su longitud
Algunas simbologías son de longitud variable, como Code 39 o Code 128.  Los códigos de barras de esta simbología pueden estar cerca de los que contengan datos diferentes, a menudo de longitudes específicas. Establecer la longitud específica de los datos que necesitas puede evitar escaneados no válidos.

| Método    | Descripción |
| :-------- | :---------- |
| [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | Te permite configurar un rango de longitudes deseadas de los datos descodificados y cómo maneja el escáner el dígito de control. |
| [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Te permite recuperar la longitud actual y comprobar las configuraciones de dígitos. |

> [!Important] 
> Confirma que el escáner de código de barras es compatible con el uso de atributos de simbologías, comprobando primero las siguientes propiedades. 
> - [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [ICheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

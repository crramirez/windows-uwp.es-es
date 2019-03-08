---
title: Trabajar con simbologías del escáner de código de barras
description: Este artículo contiene información sobre simbologías del escáner de códigos de barras.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 690b6b8ee688f62dcae375ed48e07797c921bf43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637600"
---
# <a name="working-with-symbologies"></a>El trabajo con simbologías
Una [simbología de código de barras](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) es la asignación de datos a un formato específico de código de barras. Algunos Simbologías comunes incluyen UPC, código 128, código QR y así sucesivamente.  El analizador de código de barras API de plataforma Universal de Windows permiten que una aplicación controlar cómo el analizador procesa estos Simbologías sin configurar manualmente el analizador. 

## <a name="determine-which-symbologies-are-supported"></a>Determine qué simbologías son compatibles 
Dado que la aplicación puede usarse con modelos de escáner de código de barras de diferentes fabricantes, quizá necesites consultar al escáner para determinar la lista de simbologías que admite.  Esto puede ser útil si tu aplicación requiere una simbología específica que puede no ser compatible con todos los escáneres, o si necesitas habilitar simbologías que se hayan deshabilitado manualmente o mediante programación en el escáner.

Una vez tengas un objeto [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) usando [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), llama a [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obtener una lista de simbologías admitidas por el dispositivo.

El ejemplo siguiente obtiene una lista de Simbologías admitidas del escáner de código de barras y mostrarlos en un bloque de texto:

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>Determina si se admite un simbología concreta
Para determinar si el escáner admite una simbología específico, puede llamar a [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

El ejemplo siguiente comprueba si el escáner admite la **Code32** simbología:

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Cambiar qué Simbologías se reconocen
En algunos casos, es aconsejable usar un subconjunto de simbologías que el escáner de código de barras admita.  Esto es especialmente útil para bloquear simbologías que no vayas a usar en la aplicación. Por ejemplo, para asegurarse de que un usuario escanea el código de barras correcto, podrías restringir la digitalización a CUP o EAN al adquirir SKU de elemento y a Code 128 al adquirir números de serie.

Cuando sepas qué simbologías admite tu escáner, puedes establecer las simbologías que quieres que reconozca.  Esto puede realizarse después de haber establecido un [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) objeto [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Puedes llamar a [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para permitir un conjunto específico de simbologías mientras están deshabilitadas las que no están en tu lista.

En el ejemplo siguiente se establece Simbologías activos de un escáner solicitadas a [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) y [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Atributos de simbología de código de barras
Código de barras diferentes Simbologías pueden tener atributos diferentes, como varios auxiliares descodificar longitudes, transmitir el dígito de control en el host como parte de los datos sin procesar y comprobación de validación de dígitos. Con el [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) (clase), puede obtener y establecer estos atributos para un determinado [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) y simbología de código de barras.

Puede obtener los atributos de un determinado simbología con [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). El fragmento de código siguiente obtiene los atributos de la simbología Upca para un **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Cuando haya terminado de modificar los atributos y están listos para establecerlos, puede llamar a [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Este método devuelve un **bool**, que es **true** si los atributos se han establecido correctamente.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Restringir la longitud de datos por datos de análisis
Algunas simbologías son de longitud variable, como Code 39 o Code 128.  Códigos de barras de estos Simbologías pueden encontrarse cerca entre sí que contienen datos diferentes a menudo del período especificado. Establecer la longitud específica de los datos que necesitas puede evitar escaneados no válidos.

Antes de establecer la longitud de descodificación, compruebe si la simbología de código de barras admite varias longitudes con [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Una vez que sepa que es compatible, puede establecer el [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), que es de tipo [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Esta propiedad puede ser cualquiera de los siguientes valores:

* **AnyLength**: Descodificar longitudes de cualquier número.
* **Discretos**: Descodificar longitudes de uno de ellos [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) o [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) caracteres de byte único.
* **Range**: Descodificar longitudes entre **DecodeLength1** y **DecodeLength2** caracteres de byte único. El orden de **DecodeLength1** y **DecodeLength2** importa el (puede ser mayor o menor que el otro).

Por último, puede establecer los valores de **DecodeLength1** y **DecodeLength2** para controlar la longitud de los datos que necesita.

El fragmento de código siguiente muestra cómo establecer la longitud de descodificación:

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>Transmisión de dígitos de comprobación

Otro atributo que se puede establecer en una simbología es si el dígito de control se transmitirá el host como parte de los datos sin procesar. Antes de establecer esta opción, asegúrese de que la simbología es compatible con la comprobación de transmisión de dígitos con [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). A continuación, establecer si se habilita la transmisión de dígitos de comprobación con [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

El fragmento de código siguiente muestra la transmisión de dígitos de comprobación de configuración:

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>Comprobación de validación de dígitos

También puede establecer si el dígito de control de código de barras que se va a validar. Antes de establecer esta opción, asegúrese de que la simbología es compatible con la comprobación de validación de un dígito con [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). A continuación, establezca si está habilitada la validación de dígitos de comprobación con [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

El fragmento de código siguiente muestra la validación de dígitos de comprobación de configuración:

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte también

* [Escáner de código de barras](pos-barcodescanner.md)
* [Clase BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [Clase BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Clase ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [Clase BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [Enumeración BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)
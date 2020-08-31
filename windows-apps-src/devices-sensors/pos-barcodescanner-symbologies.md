---
title: Trabajar con simbologías de escáner de código de barras
description: Use las API de escáner de código de barras UWP para procesar simbologías de código de barras sin configurar manualmente el analizador.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96014dfeb0b160d9cb94afef5ba4251b3c8bf8aa
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054385"
---
# <a name="working-with-symbologies"></a>Trabajar con simbologías
Una [simbología de código de barras](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) es la asignación de datos a un formato de código de barras específico. Algunas simbologías comunes incluyen UPC, código 128, código QR, etc.  Las API de escáner de código de barras de Plataforma universal de Windows permiten que una aplicación controle la forma en que el escáner procesa estas simbologías sin necesidad de configurar manualmente el analizador. 

## <a name="determine-which-symbologies-are-supported"></a>Determinar qué simbologías se admiten 
Dado que la aplicación puede usarse con diferentes modelos de escáner de códigos de barras de varios fabricantes, es posible que desee consultar el escáner para determinar la lista de simbologías que admite.  Esto puede ser útil si la aplicación requiere una simbología específica que es posible que no sea compatible con todos los escáneres o que tenga que habilitar las simbologías que se han deshabilitado manualmente o mediante programación en el escáner.

Una vez que tenga un objeto [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) con [BarcodeScanner. FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), llame a [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) para obtener una lista de las simbologías admitidas por el dispositivo.

En el ejemplo siguiente se obtiene una lista de las simbologías admitidas del escáner de códigos de barras y se muestran en un bloque de texto:

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

## <a name="determine-if-a-specific-symbology-is-supported"></a>Determinar si se admite una simbología específica
Para determinar si el escáner admite una simbología específica, puede llamar a [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

En el ejemplo siguiente se comprueba si el escáner de códigos de barras admite la simbología **Code32** :

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Cambiar qué simbologías se reconocen
En algunos casos, puede que desee usar un subconjunto de simbologías que admita el escáner de códigos de barras.  Esto es especialmente útil para bloquear las simbologías que no pretende utilizar en su aplicación. Por ejemplo, para asegurarse de que un usuario examina el código de barras correcto, puede restringir el análisis a UPC o EAN al adquirir las SKU de los productos y restringir el análisis al código 128 al adquirir los números de serie.

Una vez que conozca las simbologías que admite el escáner, puede establecer las simbologías que desee que reconozca.  Esto se puede hacer después de haber establecido un objeto [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) mediante [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Puede llamar a [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) para habilitar un conjunto específico de simbologías mientras se deshabilitan las que se omiten en la lista.

En el ejemplo siguiente se establecen las simbologías activas de un escáner de códigos de barras reclamado en [CODE39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) y [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Atributos de simbología de código de barras
Las simbologías de código de barras diferentes pueden tener atributos diferentes, como la compatibilidad con varias longitudes de descodificación, la transmisión del dígito de comprobación al host como parte de los datos sin procesar y la comprobación de la validación de los dígitos. Con la clase [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) , puede obtener y establecer estos atributos para una simbología determinada de [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) y Barcode.

Puede obtener los atributos de una simbología determinada con [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). El siguiente fragmento de código obtiene los atributos de la simbología UPCA para un **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Cuando haya terminado de modificar los atributos y esté listo para establecerlos, puede llamar a [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Este método devuelve un valor **booleano**, que es **true** si los atributos se establecieron correctamente.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Restringir datos de examen por longitud de datos
Algunas simbologías son de longitud variable como el código 39 o el código 128.  Los códigos de barras de estas simbologías pueden encontrarse cerca de otros con datos diferentes a menudo de longitud específica. La configuración de la longitud específica de los datos que necesita puede impedir exámenes no válidos.

Antes de establecer la longitud de decodificación, compruebe si la simbología de código de barras admite varias longitudes con [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Una vez que sepa que es compatible, puede establecer el valor de [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), que es de tipo [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Esta propiedad puede ser cualquiera de los siguientes valores:

* **AnyLength**: descodificar longitudes de cualquier número.
* **Discreta**: descodificar longitudes de caracteres de un solo byte [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) o [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) .
* **Intervalo**: descodificación de longitudes entre los caracteres de un solo byte **DecodeLength1** y **DecodeLength2** . El orden de **DecodeLength1** y **DecodeLength2** no importa (puede ser mayor o menor que el otro).

Por último, puede establecer los valores de **DecodeLength1** y **DecodeLength2** para controlar la longitud de los datos que necesita.

El siguiente fragmento de código muestra cómo establecer la longitud de decodificación:

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

### <a name="check-digit-transmission"></a>Comprobar transmisión de dígitos

Otro atributo que se puede establecer en una simbología es si el dígito de comprobación se transmitirá al host como parte de los datos sin procesar. Antes de establecer esto, asegúrese de que la simbología admita la transmisión de dígitos de comprobación con [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). A continuación, establezca si la transmisión de dígitos de comprobación está habilitada con [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

El siguiente fragmento de código muestra cómo establecer la transmisión de dígitos de comprobación:

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

### <a name="check-digit-validation"></a>Comprobación de la validación de dígitos

También puede establecer si se validará el dígito de comprobación del código de barras. Antes de establecer esto, asegúrese de que la simbología admita la validación de dígitos de comprobación con [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). A continuación, establezca si la validación de los dígitos de comprobación está habilitada con [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

El siguiente fragmento de código muestra cómo establecer la validación de los dígitos de comprobación:

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

* [Escáner de códigos de barras](pos-barcodescanner.md)
* [Clase BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [Clase BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Clase ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [Clase BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [Enumeración BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)
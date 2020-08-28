---
title: Obtener y conocer los datos de código de barras
description: Obtenga información sobre cómo obtener datos de un escáner de código de barras en un objeto BarcodeScannerReport y comprender su formato y su contenido.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5d2ab873774ce8b48252b82ce6cf7521165554d4
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043447"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtener y conocer los datos de código de barras

Una vez que haya configurado el escáner de códigos de barras, debe tener una forma de comprender los datos que digitaliza. Al digitalizar un código de barras, se genera el evento de [recibido](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) . La [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) debe suscribirse a este evento. El evento **Received** pasa un objeto [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , que puede usar para tener acceso a los datos del código de barras.

## <a name="subscribe-to-the-datareceived-event"></a>Suscribirse al evento de recibido

Una vez que tenga un **ClaimedBarcodeScanner**, haga que se suscriba al evento de **recibido** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

El controlador de eventos pasará el **ClaimedBarcodeScanner** y un objeto **BarcodeScannerDataReceivedEventArgs** . Puede tener acceso a los datos del código de barras a través de la propiedad de [Informe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) de este objeto, que es de tipo [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtener los datos

Una vez que tenga **BarcodeScannerReport**, puede tener acceso a los datos de código de barras y analizarlos. **BarcodeScannerReport** tiene tres propiedades:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): los datos de código de barras completos y sin formato.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): etiqueta del código de barras descodificado, que no incluye el encabezado, la suma de comprobación y otros datos de diversa.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): el tipo de etiqueta del código de barras descodificado. Los valores posibles se definen en la clase [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Si desea tener acceso a **ScanDataLabel** o **ScanDataType**, primero debe establecer [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) en **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtener el tipo de datos de examen

Obtener el tipo de etiqueta del código de barras descodificado es bastante trivial &mdash; simplemente se llama a [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) en **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtener la etiqueta de datos de digitalización

Para obtener la etiqueta del código de barras descodificado, hay algunas cosas que debe tener en cuenta. Solo determinados tipos de datos contienen texto codificado, por lo que debe comprobar primero si la simbología se puede convertir en una cadena y, a continuación, convertir el búfer que obtenemos de **ScanDataLabel** en una cadena UTF-8 codificada.

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>Obtención de los datos de análisis sin procesar

Para obtener los datos completos sin procesar del código de barras, simplemente convierta el búfer que obtenemos de **ScanData** en una cadena.

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

En general, estos datos tienen el formato que se proporciona desde el escáner. Sin embargo, se quita la información del encabezado y del finalizador del mensaje, ya que no contienen información útil para una aplicación y es probable que sean específicas del escáner.

La información de encabezado común es un carácter de prefijo (por ejemplo, un carácter STX). La información de finalizador común es un carácter de terminador (como un carácter ETX o CR) y un carácter de comprobación de bloque si el escáner genera uno.

Esta propiedad debe incluir un carácter de simbología si el escáner devuelve uno **(por ejemplo, un para UPC** -a). También debe incluir dígitos de comprobación si están presentes en la etiqueta y los devuelve el escáner. (Tenga en cuenta que los caracteres de simbología y los dígitos de comprobación pueden o no estar presentes, dependiendo de la configuración del escáner. El escáner los devolverá si están presentes, pero no los generará ni calculará si están ausentes.)

Es posible que algunas mercancías estén marcadas con un código de barras adicional. Este código de barras normalmente se coloca a la derecha del código de barras principal y consta de dos o cinco caracteres de información adicionales. Si el escáner Lee mercancías que contienen códigos de barras principales y complementarios, los caracteres complementarios se anexan a los caracteres principales y el resultado se entrega a la aplicación como una etiqueta. (Tenga en cuenta que un escáner puede admitir una configuración que habilita o deshabilita la lectura de códigos complementarios).

Algunos productos pueden estar marcados con varias etiquetas, a veces denominadas etiquetas de múltiples *símbolos* o *etiquetas en capas*. Normalmente, estos códigos de barras se organizan verticalmente y pueden ser del mismo o de la simbología diferente. Si el escáner lee la mercancía que contiene varias etiquetas, cada código de barras se entrega a la aplicación como una etiqueta independiente. Esto es necesario debido a la falta de normalización actual de estos tipos de código de barras. Una no puede determinar todas las variaciones en función de los datos de código de barras individuales. Por lo tanto, la aplicación tendrá que determinar cuándo se ha leído un código de barras de varias etiquetas en función de los datos devueltos. (Tenga en cuenta que un escáner puede o no admitir la lectura de varias etiquetas).

Este valor se establece antes de que se genere un evento de **recibido** en la aplicación.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte también
* [Escáner de códigos de barras](pos-barcodescanner.md)
* [Clase ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Clase BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Clase BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Clase BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)

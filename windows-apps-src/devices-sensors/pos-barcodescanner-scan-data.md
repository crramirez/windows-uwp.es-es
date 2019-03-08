---
title: Obtener y conocer los datos de código de barras
description: Obtenga información sobre cómo obtener e interpretar los datos de código de barras que examinan.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605970"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtener y conocer los datos de código de barras

Una vez que haya configurado el escáner de código de barras, por supuesto necesita una manera de comprender los datos que examinan. Al escanear un código de barras, el [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) provoca el evento. El [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) debe suscribirse a este evento. El **DataReceived** evento pasa una [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) objeto, que puede usar para tener acceso a los datos de código de barras.

## <a name="subscribe-to-the-datareceived-event"></a>Suscribirse al evento DataReceived

Una vez que tenga un **ClaimedBarcodeScanner**, tiene suscribirse a la **DataReceived** eventos:

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

El controlador de eventos que se pasará el **ClaimedBarcodeScanner** y un **BarcodeScannerDataReceivedEventArgs** objeto. Puede obtener acceso a los datos de código de barras a través de este objeto [informe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) propiedad, que es de tipo [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtener los datos

Una vez que tenga el **BarcodeScannerReport**, puede obtener acceso y analizar los datos de código de barras. **BarcodeScannerReport** tiene tres propiedades:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): Los datos sin procesar, completa el código de barras.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): La etiqueta de código de barras descodificado, que no incluye el encabezado, suma de comprobación y otra información diversa.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): El tipo de etiqueta de código de barras descodificado. Los valores posibles se definen en el [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) clase.

Si desea obtener acceso a los **ScanDataLabel** o **ScanDataType**, primero debe establecer [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) a **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtiene el tipo de datos de análisis

Obtener el tipo de etiqueta del código de barras descodificado es bastante trivial&mdash;simplemente llamamos [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) en **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtiene la etiqueta de datos de análisis

Para obtener la etiqueta del código de barras descodificado, hay algunas cosas que hay que tener en cuenta. Solo determinados tipos de datos contienen texto codificado, por lo que debe comprobar primero si la simbología puede convertirse en una cadena y, a continuación, convertir el búfer se obtienen de **ScanDataLabel** a una cadena codificada en UTF-8.

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

### <a name="get-the-raw-scan-data"></a>Obtener los datos de análisis sin procesar

Para obtener los datos sin procesar completos desde el código de barras, tan sólo convertimos el búfer se obtienen de **ScanData** en una cadena.

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

Estos datos son, en general, en el formato, tal como lo entrega desde el analizador. Información de encabezado y finalizador de mensaje se quitan, sin embargo, ya que no contienen información útil para una aplicación y están probables que sea específico de analizador.

Información de encabezado común es un carácter de prefijo (por ejemplo, un carácter STX). Información común de finalizador es un carácter terminador (por ejemplo, un carácter ETX o CR) y un carácter de comprobación bloque si uno generado por el analizador.

Esta propiedad debe incluir un carácter de simbología si uno devuelto por el analizador (por ejemplo, un **A** para UPC-A). También debe incluir los dígitos de comprobación de si están presentes en la etiqueta y devuelto por el analizador. (Tenga en cuenta que los caracteres de simbología y comprobación de dígitos pueden o pueden no estar presentes, dependiendo de la configuración del analizador. El analizador devolverá ellos si están presentes, pero no generarán o calcule si no aparecen.)

Algunos productos pueden estar marcado con un código de barras complementario. Este código de barras normalmente se coloca a la derecha del código de barras principal y consta de un dos o cinco caracteres adicionales de información. Si el analizador lee mercancía que contiene códigos de barras principal y adicionales, los caracteres complementarios se anexan a los caracteres principales y el resultado se entrega a la aplicación como una etiqueta. (Tenga en cuenta que un analizador puede admitir una configuración que habilita o deshabilita la lectura de códigos complementarios).

Algunos productos pueden marcarse con varias etiquetas, a veces denominadas *multisymbol etiquetas* o *en capas etiquetas*. Estos códigos de barras normalmente se organizan verticalmente y pueden ser de la misma u otra simbología. Si el analizador lee mercancía que contenga varias etiquetas, cada código de barras se entrega a la aplicación como una etiqueta independiente. Esto es necesario debido a la falta de estandarización de estos tipos de código de barras actual. Uno no es capaz de determinar todas las variaciones en función de los datos de código de barras individuales. Por lo tanto, la aplicación deberá determinar cuándo un código de barras múltiples de etiqueta se ha leído en función de los datos devueltos. (Tenga en cuenta que un analizador puede o no puede admitir la lectura de varias etiquetas.)

Este valor se establece antes un **DataReceived** eventos provocados a la aplicación.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte también
* [Escáner de código de barras](pos-barcodescanner.md)
* [Clase ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Clase BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Clase BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Clase BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)

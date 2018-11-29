---
title: Obtener y conocer los datos de código de barras
description: Obtén información sobre cómo obtener e interpretar los datos de código de barras que digitalizar.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8198969"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtener y conocer los datos de código de barras

Una vez que hayas configurado el escáner de códigos de barras, por supuesto que necesitas una manera de comprender los datos que examen. Al digitalizar un código de barras, se genera el evento [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) . El [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) suscribirte a este evento. El evento **DataReceived** pasa un objeto [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , que puedes usar para acceder a los datos de código de barras.

## <a name="subscribe-to-the-datareceived-event"></a>Suscribirse al evento DataReceived

Una vez que tengas un **ClaimedBarcodeScanner**, tienen se suscribe al evento **DataReceived** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

El controlador de eventos se pasarán la **ClaimedBarcodeScanner** y un objeto **BarcodeScannerDataReceivedEventArgs** . Puede acceder a los datos de códigos de barras a través de la propiedad de [informe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) de este objeto, que es de tipo [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtener los datos

Cuando tengas el **BarcodeScannerReport**, puedes acceder y analizar los datos de código de barras. **BarcodeScannerReport** tiene tres propiedades:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): los datos de códigos de barras completo, sin procesar.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): la etiqueta de código de barras descodificada, que no incluye el encabezado, suma de comprobación y otra información.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): el tipo de etiqueta de código de barras descodificado. Los valores posibles se definen en la clase [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Si quieres obtener acceso a **ScanDataLabel** o **ScanDataType**, primero debes establecer [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) en **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtener el tipo de datos de análisis

Obtener el tipo de etiqueta de código de barras descodificada es bastante trivial&mdash;simplemente llamamos [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) en **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtener la etiqueta de datos de análisis

Para obtener la etiqueta de código de barras descodificada, hay algunas cosas que tienes que tener en cuenta. Solo determinados tipos de datos contienen texto codificado, por lo que debe comprobar si la simbología puede convertirse en una cadena y, a continuación, convierte el búfer obtenemos desde **ScanDataLabel** en una cadena codificada de UTF-8.

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

Para obtener completo, los datos sin procesar desde el código de barras, simplemente convertimos el búfer que obtenemos desde **ScanData** en una cadena.

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

Estos datos son, en general, en el formato que se entregan desde el escáner. Información de encabezado y tráiler del mensaje se quitan, sin embargo, ya que no contienen información útil para una aplicación y es probable que sea el escáner específicos.

Información de encabezado común es un carácter de prefijo (por ejemplo, un carácter STX). Información de tráiler común es un carácter de terminador (por ejemplo, un carácter ETX o CR) y un carácter de comprobación de bloque si uno generado por el analizador.

Esta propiedad debe incluir un carácter de simbología si se devuelve una por el analizador (por ejemplo, **una** para UPC-A). También se debe incluir dígitos de verificación si están presentes en la etiqueta y devuelto por el analizador. (Ten en cuenta que los caracteres de simbología y comprobación de dígitos pueden o no esté presentes, según la configuración del escáner. El analizador devolverá ellos si está presente, pero no generarán o calcule si están presentes.)

Algunos productos pueden marcarse con un código de barras adicional. Este código de barras normalmente se coloca a la derecha del código de barras principal y consta de un caracteres de dos o cinco adicionales de información. Si el escáner lee los artículos que contiene los códigos de barras adicionales y principales, los caracteres adicionales se anexan a los caracteres principales y el resultado se entrega a la aplicación como una etiqueta. (Ten en cuenta que un escáner puede admitir una configuración que habilita o deshabilita la lectura de códigos complementarios.)

Algunos productos pueden marcarse con varias etiquetas, a veces se denomina *multisymbol etiquetas* o *etiquetas en niveles*. Estos códigos de barras normalmente se organizan verticalmente y pueden ser de la simbología iguales o diferente. Si el escáner lee los artículos que contienen varias etiquetas, cada código de barras se entrega a la aplicación como una etiqueta independiente. Esto es necesario debido a la falta de normalización de estos tipos de códigos de barras actual. Uno no es capaz de determinar todas las variaciones en función de los datos de códigos de barras individuales. Por lo tanto, la aplicación que determinar cuándo un código de barras de etiqueta varios se ha leído en función de los datos devueltos. (Ten en cuenta que un escáner puede admite o no la lectura de varias etiquetas.)

Este valor se establece antes de que se genere a la aplicación de un evento de **DataReceived** .

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Ver también
* [Escáner de código de barras](pos-barcodescanner.md)
* [Clase ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Clase BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Clase BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Clase BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)

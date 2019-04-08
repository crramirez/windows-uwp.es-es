---
title: Obtener y comprender los datos de bandas magnéticas
description: Obtenga información sobre cómo obtener e interpretar los datos de una banda magnética.
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, uwp, de punto de servicio, punto de venta, lector de banda magnética
ms.localizationpriority: medium
ms.openlocfilehash: 1805213c7c30ccbc67fb96098f11480703589bb4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651610"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Obtener y comprender los datos de bandas magnéticas

Una vez que haya configurado en la aplicación mediante los pasos descritos en el lector de banda magnética [Introducción a punto de servicio](pos-basics.md), está listo para empezar a enviar datos a partir de él.

## <a name="subscribe-to-datareceived-events"></a>Suscríbase a * DataReceived eventos

Cada vez que el lector reconoce una tarjeta deslizada, se producirá uno de los tres eventos:

* [Evento AamvaCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): Se produce al pasar una tarjeta de vehículos.
* [Evento BankCardDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): Se produce cuando una tarjeta bancaria pasar.
* [Evento VendorSpecificDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): Se produce al pasar una tarjeta específica del proveedor.

La aplicación solo necesita suscribirse a los eventos que son compatibles con el lector de la banda magnética. Puede ver qué tipos de tarjetas son compatibles con [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
).

El código siguiente muestra cómo suscribirse a las tres ***DataReceived** eventos:

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

El controlador de eventos que se pasará el [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) y un *args* objeto, cuyo tipo variarán dependiendo del evento:

* **AamvaCardDataReceived** eventos: [Clase MagneticStripeReaderAamvaCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** eventos: [Clase MagneticStripeReaderBankCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** eventos: [Clase MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Obtener los datos

Para el **AamvaCardDataReceived** y **BankCardDataReceived** eventos, puede obtener algunos de los datos directamente desde el *args* objeto. El ejemplo siguiente muestra algunas propiedades de obtención y asignarlas a variables de miembro:

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

Sin embargo, algunos datos, incluidos todos los datos desde el **VendorSpecificDataReceived** eventos, se deben recuperar a través de la **informe** objeto, que es una propiedad de la *args* parámetro. Se trata del tipo [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Puede usar el [CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) propiedad para averiguar qué tipo de tarjeta ha sido Deslizar y, a continuación, úsela para informar a cómo interpretar los datos de [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [ Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3), y [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Los datos de cada una de las pistas se representan como [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) objetos. De esta clase, puede obtener los siguientes tipos de datos:

* [Datos](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): Los datos sin procesar o descodificados.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): Los datos discrecionales. 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): Los datos cifrados.

El fragmento de código siguiente obtiene el informe y los datos de seguimiento y, a continuación, comprueba el tipo de tarjeta:

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Consulte también

* [Lector de la banda magnética](pos-magnetic-stripe-reader.md)
* [Clase ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [Clase MagneticStripeReaderAamvaCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [Clase MagneticStripeReaderBankCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [Clase MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)
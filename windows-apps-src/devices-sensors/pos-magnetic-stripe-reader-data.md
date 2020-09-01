---
title: Obtener y comprender los datos de bandas magnéticas
description: Obtenga información sobre cómo obtener e interpretar los datos de un lector de franjas magnéticas mediante las API de punto de servicio (UWP) de Plataforma universal de Windows (UWP).
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos, lector de stripe magnética
ms.localizationpriority: medium
ms.openlocfilehash: 2a405a66fbc243925c4c9bbd5b1ee499a9a7b9f8
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238280"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Obtener y comprender los datos de bandas magnéticas

Una vez que haya configurado el lector de franjas magnéticas en la aplicación mediante los pasos descritos en [Introducción al punto de servicio](pos-basics.md), está listo para empezar a obtener datos de él.

## <a name="subscribe-to-datareceived-events"></a>Suscribirse a * eventos recibidos

Siempre que el lector reconozca una tarjeta deslizante, generará uno de tres eventos:

* [Evento AamvaCardDataReceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): se produce cuando se pasa el dedo por una tarjeta del vehículo del motor.
* [Evento BankCardDataReceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): tiene lugar cuando se pasa el dedo por una tarjeta bancaria.
* [Evento VendorSpecificDataReceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): se produce cuando se pasa el dedo por una tarjeta específica del proveedor.

La aplicación solo necesita suscribirse a los eventos admitidos por el lector de franjas magnéticas. Puede ver qué tipos de tarjetas se admiten con [MagneticStripeReader. SupportedCardTypes](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes).

En el código siguiente se muestra cómo suscribirse a los tres eventos de los ***recibidos** :

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

El controlador de eventos pasará el [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) y un objeto *args* , cuyo tipo variará en función del evento:

* Evento **AamvaCardDataReceived** : [clase MagneticStripeReaderAamvaCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* Evento **BankCardDataReceived** : [clase MagneticStripeReaderBankCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* Evento **VendorSpecificDataReceived** : [clase MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Obtener los datos

En el caso de los eventos **AamvaCardDataReceived** y **BankCardDataReceived** , puede obtener algunos de los datos directamente desde el objeto *args* . En el ejemplo siguiente se muestra cómo obtener algunas propiedades y asignarlas a variables de miembro:

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

Sin embargo, algunos datos, incluidos todos los datos del evento **VendorSpecificDataReceived** , deben recuperarse a través del objeto de **Informe** , que es una propiedad del parámetro *args* . Es de tipo [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Puede usar la propiedad [CardType](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) para averiguar qué tipo de tarjeta se ha descartado y, a continuación, usarla para informar de cómo interpretar los datos de [Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)y [Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Los datos de cada una de las pistas se representan como objetos [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) . A partir de esta clase, puede obtener los siguientes tipos de datos:

* [Datos](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): los datos sin formato o descodificados.
* [DiscretionaryData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): los datos discrecionales. 
* [EncryptedData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): los datos cifrados.

El siguiente fragmento de código obtiene el informe y los datos de seguimiento y, a continuación, comprueba el tipo de tarjeta:

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

## <a name="see-also"></a>Vea también

* [Lector de bandas magnéticas](pos-magnetic-stripe-reader.md)
* [Clase ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [Clase MagneticStripeReaderAamvaCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [Clase MagneticStripeReaderBankCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [Clase MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)
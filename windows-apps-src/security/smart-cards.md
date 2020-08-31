---
title: Tarjetas inteligentes
description: En este tema se explica la manera en que las aplicaciones para la Plataforma universal de Windows (UWP) pueden usar tarjetas inteligentes para conectar a los usuarios para proteger los servicios de red, incluido cómo obtener acceso a los lectores de tarjetas inteligentes físicas, crear tarjetas inteligentes virtuales, comunicarse con tarjetas inteligentes, autenticar usuarios, restablecer los PIN de usuario y quitar o desconectar tarjetas inteligentes.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, security
ms.localizationpriority: medium
ms.openlocfilehash: 5c792cde951b2bf01585256f51f67a545d96510d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170839"
---
# <a name="smart-cards"></a>Tarjetas inteligentes




En este tema se explica la manera en que las aplicaciones para la Plataforma universal de Windows (UWP) pueden usar tarjetas inteligentes para conectar a los usuarios para proteger los servicios de red, incluido cómo obtener acceso a los lectores de tarjetas inteligentes físicas, crear tarjetas inteligentes virtuales, comunicarse con tarjetas inteligentes, autenticar usuarios, restablecer los PIN de usuario y quitar o desconectar tarjetas inteligentes. 

## <a name="configure-the-app-manifest"></a>Configuración del manifiesto de la aplicación


Para que la aplicación pueda autenticar los usuarios mediante tarjetas inteligentes o tarjetas inteligentes virtuales, debes establecer la funcionalidad **Certificados de usuario compartidos** en el archivo Package.appxmanifest.

## <a name="access-connected-card-readers-and-smart-cards"></a>Acceder a lectores de tarjetas conectados y tarjetas inteligentes


Para consultar lectores y tarjetas inteligentes conectadas deberás pasar el identificador del dispositivo (especificado en [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)) al método [**SmartCardReader.FromIdAsync**](/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync). Para acceder a las tarjetas inteligentes conectadas al dispositivo lector devuelto, llama a [**SmartCardReader.FindAllCardsAsync**](/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync).

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

También deberás habilitar la aplicación para que busque eventos [**CardAdded**](/uwp/api/windows.devices.smartcards.smartcardreader.cardadded) mediante una función que determine el comportamiento de la aplicación cuando se inserte una tarjeta.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

Después, puedes pasar cada objeto [**SmartCard**](/uwp/api/Windows.Devices.SmartCards.SmartCard) devuelto a [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) para acceder a los métodos que permiten que tu aplicación acceda a su configuración y la personalice.

## <a name="create-a-virtual-smart-card"></a>Crear una tarjeta inteligente virtual


Para crear una tarjeta inteligente virtual usando [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning), tu aplicación primero debe proporcionar un nombre descriptivo, una clave de administración y una [**SmartCardPinPolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy). Por lo general, el nombre descriptivo es algo que se proporciona a la aplicación, pero esta debe proporcionar también una clave de administración y generar una instancia de la **SmartCardPinPolicy** actual antes de pasar los tres valores a [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

1.  Crea una nueva instancia de una clase [**SmartCardPinPolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy).
2.  Para generar el valor de clave de administración, llama a [**CryptographicBuffer.GenerateRandom**](/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) mediante el valor de clave de administración proporcionado por el servicio o por la herramienta de administración.
3.  Pasa estos valores junto con la cadena *FriendlyNameText* a [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

Después de que el método [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) haya devuelto el objeto [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) asociado, la tarjeta inteligente virtual está aprovisionada y lista para usar.

>[!NOTE]
>Para crear una tarjeta inteligente virtual mediante una aplicación de UWP, el usuario que ejecuta la aplicación debe ser miembro del grupo administradores. Si el usuario no es miembro del grupo administradores, se producirá un error en la creación de la tarjeta inteligente virtual.

## <a name="handle-authentication-challenges"></a>Administrar los desafíos de autenticación


Para autenticar con tarjetas inteligentes o tarjetas inteligentes virtuales, tu aplicación debe proporcionar el comportamiento para completar los desafíos entre los datos de la clave de administración almacenados en la tarjeta y los datos de la clave de administración que mantiene el servidor de autenticación o la herramienta de administración.

El código siguiente muestra cómo admitir la autenticación con tarjeta inteligente para los servicios o cómo modificar los detalles de una tarjeta física o virtual. Si los datos generados con la clave de administración en la tarjeta ("desafío") son los mismos que los datos de la clave de administración proporcionados por el servidor o por la herramienta de administración ("adminkey"), la autenticación es correcta.

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

Verás que hacemos referencia a este código en el resto del tema; en él revisamos cómo completar una acción de autenticación y cómo aplicar cambios a la información de una tarjeta inteligente o una tarjeta inteligente virtual.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>Comprobar la respuesta de la autenticación con tarjeta inteligente o con tarjeta inteligente virtual


Ahora que tenemos definida la lógica de los desafíos de autenticación, podemos comunicarnos con el lector para acceder a la tarjeta inteligente, o bien, acceder a una tarjeta inteligente virtual para la autenticación.

1.  Para comenzar el desafío, llama a [**GetChallengeContextAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync) desde el objeto [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) asociado con la tarjeta inteligente. Esto generará una instancia de [**SmartCardChallengeContext**](/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext), que contiene el valor de [**Desafío**](/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge) de la tarjeta.

2.  A continuación, pasa el valor del desafío de la tarjeta y la clave de administración proporcionada por el servicio o por la herramienta de administración al **ChallengeResponseAlgorithm** que hemos definido en el ejemplo anterior.

3.  [**VerifyResponseAsync**](/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync) devolverá **true** si la autenticación es correcta.

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>Cambiar o restablecer un PIN de usuario


Para cambiar el PIN asociado a una tarjeta inteligente:

1.  Accede a la tarjeta y genera el objeto [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) asociado.
2.  Llama a [**RequestPinChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) para mostrar al usuario una interfaz de usuario para que complete esta operación.
3.  Si el PIN se cambió correctamente, la llamada devolverá **true**.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

Solicitar el restablecimiento de un PIN:

1.  Llama a [**RequestPinResetAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) para iniciar la operación. Esta llamada incluye un método [**SmartCardPinResetHandler**](/uwp/api/windows.devices.smartcards.smartcardpinresethandler) que representa la tarjeta inteligente y la solicitud de restablecimiento del PIN.
2.  [**SmartCardPinResetHandler**](/uwp/api/windows.devices.smartcards.smartcardpinresethandler) proporciona información que nuestro elemento **ChallengeResponseAlgorithm**, encapsulado en una llamada a [**SmartCardPinResetDeferral**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral) usa para comparar el valor del desafío de la tarjeta con la clave de administración proporcionada por el servicio o por la herramienta de administración con el fin de autenticar la solicitud.

3.  Si el desafío es correcto, la llamada a [**RequestPinResetAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) finaliza y devuelve **true** si el PIN se restableció correctamente.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>Quitar una tarjeta inteligente o una tarjeta inteligente virtual


Cuando se quita una tarjeta inteligente física, se activa un evento [**CardRemoved**](/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved) cuando la tarjeta se elimina.

Asocia la activación de este evento con el lector de tarjetas usando el método que define el comportamiento de tu aplicación al quitar la tarjeta o el lector como un controlador de eventos. Este comportamiento puede ser tan simple como proporcionar una notificación al usuario cuando se retira la tarjeta.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

La retirada de una tarjeta inteligente virtual se controla mediante programación; para ello, se recupera primero la tarjeta y, después, se llama a [**RequestVirtualSmartCardDeletionAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync) desde el objeto [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) devuelto.

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```
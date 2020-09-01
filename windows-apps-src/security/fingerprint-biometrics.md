---
title: Biometría de huellas digitales
description: En este artículo se explica cómo agregar la opción de biometría de huellas digitales en la aplicación para la Plataforma universal de Windows (UWP).
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, security
ms.localizationpriority: medium
ms.openlocfilehash: bcb82280d80467157faa8aa5195ad7b9d9a7336b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167219"
---
# <a name="fingerprint-biometrics"></a>Biometría de huellas digitales




En este artículo se explica cómo agregar la opción de biometría de huellas digitales en la aplicación para la Plataforma universal de Windows (UWP). La inclusión de una solicitud de autenticación con huella digital cuando el usuario deba dar su consentimiento a una acción concreta aumenta la seguridad de la aplicación. Por ejemplo, puedes solicitar la autenticación con huella digital antes de autorizar una compra desde la aplicación o de permitir el acceso a recursos restringidos. Puedes administrar la autenticación con huella digital mediante la clase [**UserConsentVerifier**](/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier) del espacio de nombres [**Windows.Security.Credentials.UI**](/uwp/api/Windows.Security.Credentials.UI).

## <a name="check-the-device-for-a-fingerprint-reader"></a>Comprueba si el dispositivo tiene un lector de huellas digitales


Para saber si el dispositivo tiene un lector de huellas digitales, llama al método [**UserConsentVerifier.CheckAvailabilityAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.checkavailabilityasync). Incluso si un dispositivo admite la autenticación con huellas digitales, es recomendable que la aplicación proporcione a los usuarios la opción para habilitarla o deshabilitarla en la configuración.

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>Solicitar consentimiento y devolver resultados


Para solicitar el consentimiento del usuario mediante un examen de huellas digitales, llama al método [**UserConsentVerifier.RequestVerificationAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync). Para que la autenticación con huella digital funcione, el usuario debe haber agregado anteriormente una "firma" con huella digital a la base de datos de huellas digitales.

Cuando se llama al método [**UserConsentVerifier.RequestVerificationAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync), se le muestra al usuario un diálogo modal que solicita un examen de la huella digital. Puedes incluir un mensaje en el método **UserConsentVerifier.RequestVerificationAsync** que se mostrará al usuario como parte del diálogo modal, tal como se muestra en la siguiente imagen.

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```
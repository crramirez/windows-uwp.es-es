---
author: drewbatgit
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: En este artículo se describe el proceso para agregar streaming adaptable de contenido multimedia con protección de contenido de Microsoft PlayReady a una aplicación para la Plataforma universal de Windows (UWP).
title: Streaming adaptable con PlayReady
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7132de481f22f53269cd9bd69a38819c5b71cb55
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7158045"
---
# <a name="adaptive-streaming-with-playready"></a>Streaming adaptable con PlayReady


En este artículo se describe el proceso para agregar streaming adaptable de contenido multimedia con protección de contenido de Microsoft PlayReady a una aplicación para Plataforma universal de Windows (UWP). 

Actualmente, esta característica admite la reproducción de contenido Dynamic Streaming over HTTP (DASH).

HLS (HTTP Live Streaming de Apple) no es compatible con PlayReady.

Actualmente, tampoco se admite Smooth Streaming de forma nativa. Sin embargo, PlayReady es extensible y, si se usan código o bibliotecas adicionales, se puede admitir Smooth Streaming protegido con PlayReady y, de este modo, se pueden aprovechar las ventajas de DRM (administración de derechos digitales) de software y hardware.

En este artículo solo se tratan los aspectos del streaming adaptable específicos de PlayReady. Para obtener información sobre la implementación del streaming adaptable en general, consulta [Streaming adaptable](adaptive-streaming.md).

En este artículo se usa el código del [Adaptive streaming sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming) (ejemplo de streaming adaptable) del repositorio **Windows-universal-samples** de Microsoft en GitHub. El escenario 4 presenta el uso de streaming adaptable con PlayReady. Para descargar el repositorio en un archivo ZIP, desplázate hasta el nivel raíz del repositorio y selecciona el botón **Descargar ZIP**.

Necesitarás las siguientes instrucciones **using**:

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

El espacio de nombres **LicenseRequest** procede de **CommonLicenseRequest.cs**, un archivo de PlayReady que Microsoft proporciona a los licenciatarios.

Debes declarar algunas variables globales:

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

También tendrás que declarar la constante siguiente:

```csharp
private const uint MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = 0x8004B895;
```

## <a name="setting-up-the-mediaprotectionmanager"></a>Configurar el MediaProtectionManager

Para agregar la protección de contenido de PlayReady a tu aplicación para UWP, tienes que configurar un objeto [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040). Esto se hace al inicializar el objeto [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

El siguiente código configura un objeto [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040):

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

Este código puede copiarse simplemente en la aplicación, dado que es obligatorio para configurar la protección de contenido.

El evento [ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041) se activa cuando no se puede realizar la carga de datos binarios. Debemos agregar un controlador de eventos para controlar este evento, que señale que no se completó la carga:

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

De forma similar, debemos agregar un controlador de eventos para el evento [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045), que se activa cuando se solicita un servicio. Este código comprueba de qué tipo de solicitud se trata y responde en consecuencia:

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## <a name="individualization-service-requests"></a>Solicitudes de servicio de individualización

El siguiente código realiza de forma reactiva una solicitud de servicio de individualización de PlayReady. Pasamos la solicitud como un parámetro a la función. Rodeamos la llamada en un bloque try/catch y, si no hay ninguna excepción, decimos que la solicitud se completó correctamente:

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null && comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

Como alternativa, es posible que debamos realizar de forma proactiva una solicitud de servicio de individualización, en cuyo caso llamamos a la siguiente función en lugar de que el código llame a `ReactiveIndivRequest` en `ProtectionManager_ServiceRequested`:

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## <a name="license-acquisition-service-requests"></a>Solicitudes de servicio de adquisición de licencias

Si, en su lugar, la solicitud era de tipo [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285), llamamos a la siguiente función para solicitar y adquirir la licencia de PlayReady. Indicamos al objeto **MediaProtectionServiceCompletion** que se haya pasado si la solicitud era correcta o no y completamos la solicitud:

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## <a name="initializing-the-adaptivemediasource"></a>Inicializar el objeto AdaptiveMediaSource

Por último, necesitarás una función para inicializar el objeto [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912), que se creó a partir de una clase [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) y una clase [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) concretas. La clase **Uri** debe ser el vínculo al archivo multimedia (HLS o DASH), y la clase **MediaElement** debe estar definida en el código XAML.

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

Puedes llamar a esta función en cualquier evento que controle el inicio del streaming adaptable, por ejemplo, en un evento Click de botón.

## <a name="see-also"></a>Consulta también
- [DRM de PlayReady](playready-client-sdk.md)





---
title: Vista previa de hospedaje del escáner de códigos de barras de la cámara
description: Obtenga información acerca de cómo hospedar una versión preliminar del escáner de código de barras de la cámara en la aplicación
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6657fa52a5e3cac0821def7102b4bc93089b2ffe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159639"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Hospedaje de una vista previa del escáner de barras de códigos de cámara en la aplicación
## <a name="step-1-setup-your-camera-preview"></a>Paso 1: configuración de la versión preliminar de la cámara
El primer paso para agregar una vista previa a la aplicación para el escáner de códigos de barras de la cámara puede realizarse siguiendo las instrucciones del tema [Mostrar la vista previa de la cámara](../audio-video-camera/simple-camera-preview-access.md) .  Una vez completado este paso, vuelva a este tema para las modificaciones específicas del escáner de códigos de barras de la cámara.

## <a name="step-2-update-capability-declarations"></a>Paso 2: actualizar las declaraciones de capacidad
Para evitar que los usuarios reciban la solicitud de consentimiento para el micrófono, puede excluirlo de las funcionalidades enumeradas en el manifiesto de la aplicación.

1. En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2. Seleccione la pestaña **Funcionalidades**.
3. Desactive la casilla de **micrófono**

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Paso 3: agregar una directiva using adicional para la captura multimedia

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Paso 4: configurar las opciones de inicialización de MediaCapture
En el ejemplo siguiente se inicializa [**MediaCaptureInitializationSettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings). 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Paso 5: asociar el objeto MediaCapture al escáner de códigos de barras de la cámara
Reemplace el mediaCapture.Iniexistente tializeAsync () en *StartPreviewAsync ()* por lo siguiente:

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Consulte [visualización de la vista previa de la cámara](../audio-video-camera/simple-camera-preview-access.md#add-capability-declarations-to-the-app-manifest) para ver temas más avanzados sobre cómo hospedar una vista previa de cámara en la aplicación.

## <a name="see-also"></a>Vea también

### <a name="samples"></a>Ejemplos

- [Ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
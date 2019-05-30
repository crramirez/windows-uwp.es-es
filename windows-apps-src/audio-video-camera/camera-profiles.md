---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: En este artículo se describe cómo usar perfiles de cámara para detectar y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo. Esto incluye tareas como la selección de perfiles que admiten resoluciones específicas o velocidades de fotogramas, perfiles que admiten el acceso simultáneo con varias cámaras y perfiles que admiten HDR.
title: Descubrir y seleccionar las funcionalidades de cámara con los perfiles de cámara
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8fb73d917124f776fbb4ca1e47799804e4f635e2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358959"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>Descubrir y seleccionar las funcionalidades de cámara con los perfiles de cámara



En este artículo se describe cómo usar perfiles de cámara para detectar y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo. Esto incluye tareas como la selección de perfiles que admiten resoluciones específicas o velocidades de fotogramas, perfiles que admiten el acceso simultáneo con varias cámaras y perfiles que admiten HDR.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

 

## <a name="about-camera-profiles"></a>Acerca de los perfiles de cámara

Las cámaras de diferentes dispositivos admiten diferentes funcionalidades, incluido el conjunto de resoluciones de captura admitidas, la velocidad de fotogramas para capturas de vídeo y si se admite HDR y las capturas de velocidad de fotogramas variable. El marco de captura multimedia de la Plataforma universal de Windows (UWP) almacena este conjunto de funcionalidades en un [**MediaCaptureVideoProfileMediaDescription**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription). Un perfil de cámara, representado por un objeto [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile), tiene tres conjuntos de descripciones de elementos multimedia; uno para la captura de fotos, uno para la captura de vídeo y otro para la vista previa de vídeo.

Antes de inicializar el objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md), puedes consultar a los dispositivos de captura en el dispositivo actual para ver qué perfiles se admiten. Cuando se selecciona un perfil admitido, sabe que el dispositivo de captura admite todas las funcionalidades en las descripciones de elementos multimedia del perfil. Esto elimina la necesidad de un enfoque de prueba y error para determinar qué combinaciones de funcionalidades se admiten en un dispositivo determinado.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

Los ejemplos de código de este artículo reemplazan esta inicialización mínima con la detección de perfiles de cámara que admiten diferentes funcionalidades, que, a continuación, se usan para inicializar el dispositivo de captura multimedia.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>Buscar un dispositivo de vídeo que admita perfiles de cámara

Antes de buscar perfiles de cámara compatibles, debes buscar un dispositivo de captura que admita el uso de perfiles de cámara. El método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido en el ejemplo siguiente usa el método [**DeviceInformaion.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) para recuperar una lista de todos los dispositivos de captura de vídeo disponibles. Recorre todos los dispositivos en la lista, llamando al método estático, [**IsVideoProfileSupported**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported), para que cada dispositivo vea si admite perfiles de vídeo. Además, la propiedad [**EnclosureLocation.Panel**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.panel) para cada dispositivo, que permite especificar si quieres que la cámara esté en la parte frontal o posterior del dispositivo.

Si se encuentra un dispositivo compatible con los perfiles de cámara en el panel especificado, se devuelve el valor [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id), que contiene la cadena de identificador del dispositivo.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Si el identificador de dispositivo devuelto desde el método auxiliar **GetVideoProfileSupportedDeviceIdAsync** es nulo o una cadena vacía, no hay ningún dispositivo en el panel especificado que admita perfiles de cámara. En este caso, se debe inicializar el dispositivo de captura multimedia sin usar perfiles.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>Seleccionar un perfil basado en la resolución y velocidad de fotogramas compatibles

Para seleccionar un perfil con funcionalidades concretas, como la capacidad de lograr una resolución y una velocidad de fotogramas concretas, se debe llamar al método auxiliar definido anteriormente para obtener el identificador de un dispositivo de captura que admita el uso de perfiles de cámara.

Crea un objeto [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) nuevo, pasando el identificador de dispositivo seleccionado. A continuación, realiza una llamada al método estático [**MediaCapture.FindAllVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) para obtener una lista de todos los perfiles de cámara compatibles con el dispositivo.

Este ejemplo usa un método de consulta Linq, incluido en el espacio de nombres **System.Linq** de "using", para seleccionar un perfil que contenga un objeto [**SupportedRecordMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) donde las propiedades [**Width**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height) y [**FrameRate**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) coinciden con los valores solicitados. Si se encuentra una coincidencia, [**VideoProfile**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) y [**RecordMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) de **MediaCaptureInitializationSettings** se definen con los valores de tipo anónimo devueltos desde la consulta Linq. Si no se encuentra ninguna coincidencia, se usa el perfil predeterminado.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Tras rellenar **MediaCaptureInitializationSettings** con el perfil de cámara deseado, simplemente, realiza una llamada a [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) en el objeto de captura multimedia para configurarlo en el perfil deseado.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="use-media-frame-source-groups-to-get-profiles"></a>Usar grupo de origen de fotogramas multimedia para obtener perfiles

A partir de Windows 10, versión 1803, puedes usar la clase [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup) para obtener perfiles de cámara con las capacidades específicas antes de inicializar el objeto **MediaCapture**. Los grupos de origen de fotogramas permiten a los fabricantes de dispositivos representar grupos de sensores o capturar funcionalidades como un dispositivo virtual único. Esto permite escenarios de fotografía computacional como el uso de cámaras de profundidad y color juntas, pero también se pueden usar para seleccionar perfiles de cámara para escenarios de captura sencillos. Para obtener más información acerca del uso de **MediaFrameSourceGroup**, consulta [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

En el método de ejemplo siguiente se muestra cómo usar objetos **MediaFrameSourceGroup** para encontrar un perfil de cámara que admita un perfil de vídeo conocido, como uno que admita HDR o secuencia fotográfica variable. En primer lugar, llama a [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) para obtener una lista de todos los grupos de origen de fotogramas multimedia disponibles en el dispositivo actual. Recorra cada grupo de origen y llame a [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) para obtener una lista de todos los perfiles de vídeo para el grupo de origen actual que admiten el perfil especificado, en este caso, HDR con fotografía WCG. Si se encuentra un perfil que cumpla con los criterios, crea un nuevo objeto **MediaCaptureInitializationSettings** y establece el **VideoProfile** en el perfil de selección y el **VideoDeviceId** en la propiedad **Id** del grupo de origen de fotogramas multimedia actual. Por tanto, por ejemplo, podrías pasar el valor **KnownVideoProfile.HdrWithWcgVideo** a este método para obtener opciones de configuración de captura multimedia que admiten vídeo HDR. Pasa **KnownVideoProfile.VariablePhotoSequence** para obtener la configuración que admite la secuencia de fotografías variable.

 [!code-cs[FindKnownVideoProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindKnownVideoProfile)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>Usar perfiles conocidos para encontrar un perfil que admita vídeo HDR (técnica heredada)

> [!NOTE] 
> Las API descritas en esta sección están en desuso a partir de Windows 10, versión 1803. Consulta la sección anterior, **Usar grupo de origen de fotogramas multimedia para obtener perfiles**.

La selección de un perfil que admita HDR comienza como los otros escenarios. Crear un **MediaCaptureInitializationSettings** y una cadena que contenga el identificador de dispositivo de captura. Agrega una variable booleana que realizará un seguimiento de si se admite vídeo HDR.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Usa el método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido anteriormente para obtener el identificador de dispositivo para un dispositivo de captura que admita perfiles de cámara.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

El método estático [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) devuelve los perfiles de cámara compatibles con el dispositivo especificado que se clasifica por el valor [**KnownVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.KnownVideoProfile) especificado. Para este escenario, el valor **VideoRecording** se especifica para limitar los perfiles de cámara devueltos a aquellos que admiten la grabación de vídeo.

Recorra la lista de perfiles de cámara devueltos. Para cada perfil de cámara, recorre cada [**VideoProfileMediaDescription**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) en la comprobación de perfiles para determinar si la propiedad [**IsHdrVideoSupported**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) es true. Tras encontrar una descripción de elemento multimedia adecuada, interrumpe el recorrido y asigna el perfil y los objetos de la descripción al objeto **MediaCaptureInitializationSettings**.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>Determinar si un dispositivo admite captura simultánea de fotos y vídeo

Muchos dispositivos admiten la captura de fotos y vídeo simultáneamente. Para determinar si un dispositivo de captura admite esto, realiza una llamada a [**MediaCapture.FindAllVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) para obtener todos los perfiles de cámara admitidos por el dispositivo. Usa una consulta de vínculo para encontrar un perfil que tenga al menos una entrada para [**SupportedPhotoMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) y [**SupportedRecordMediaDescription**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription), lo que significa que el perfil admite la captura simultánea.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Puedes ajustar esta consulta para buscar los perfiles que admiten resoluciones específicas y otras funcionalidades, además de la grabación de vídeo simultánea. También puedes usar el objeto [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) y especificar el valor [**BalancedVideoAndPhoto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.KnownVideoProfile) para recuperar perfiles que admitan la captura simultánea, pero la consulta de todos los perfiles proporcionará resultados más completos.

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Capturar básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 





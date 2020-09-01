---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: En este artículo se describe cómo usar perfiles de cámara para detectar y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo. Esto incluye tareas como la selección de perfiles que admiten resoluciones específicas o velocidades de fotogramas, perfiles que admiten el acceso simultáneo con varias cámaras y perfiles que admiten HDR.
title: Descubrir y seleccionar las funcionalidades de cámara con los perfiles de cámara
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 08b5bda92535bc324589105ce37f4a6458c523be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161049"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>Descubrir y seleccionar las funcionalidades de cámara con los perfiles de cámara



En este artículo se describe cómo usar perfiles de cámara para detectar y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo. Esto incluye tareas como la selección de perfiles que admiten resoluciones específicas o velocidades de fotogramas, perfiles que admiten el acceso simultáneo con varias cámaras y perfiles que admiten HDR.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

 

## <a name="about-camera-profiles"></a>Acerca de los perfiles de cámara

Las cámaras de diferentes dispositivos admiten diferentes funcionalidades, incluido el conjunto de resoluciones de captura admitidas, la velocidad de fotogramas para capturas de vídeo y si se admite HDR y las capturas de velocidad de fotogramas variable. El marco de captura multimedia de la Plataforma universal de Windows (UWP) almacena este conjunto de funcionalidades en un [**MediaCaptureVideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription). Un perfil de cámara, representado por un objeto [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile), tiene tres conjuntos de descripciones de elementos multimedia; uno para la captura de fotos, uno para la captura de vídeo y otro para la vista previa de vídeo.

Antes de inicializar el objeto [MediaCapture](./index.md), puedes consultar a los dispositivos de captura en el dispositivo actual para ver qué perfiles se admiten. Cuando se selecciona un perfil admitido, sabe que el dispositivo de captura admite todas las funcionalidades en las descripciones de elementos multimedia del perfil. Esto elimina la necesidad de un enfoque de prueba y error para determinar qué combinaciones de funcionalidades se admiten en un dispositivo determinado.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

Los ejemplos de código de este artículo reemplazan esta inicialización mínima con la detección de perfiles de cámara que admiten diferentes funcionalidades, que, a continuación, se usan para inicializar el dispositivo de captura multimedia.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>Buscar un dispositivo de vídeo que admita perfiles de cámara

Antes de buscar perfiles de cámara compatibles, debes buscar un dispositivo de captura que admita el uso de perfiles de cámara. El método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido en el ejemplo siguiente usa el método [**DeviceInformaion.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) para recuperar una lista de todos los dispositivos de captura de vídeo disponibles. Recorre todos los dispositivos en la lista, llamando al método estático, [**IsVideoProfileSupported**](/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported), para que cada dispositivo vea si admite perfiles de vídeo. Además, la propiedad [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) para cada dispositivo, que permite especificar si quieres que la cámara esté en la parte frontal o posterior del dispositivo.

Si se encuentra un dispositivo compatible con los perfiles de cámara en el panel especificado, se devuelve el valor [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id), que contiene la cadena de identificador del dispositivo.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Si el identificador de dispositivo devuelto desde el método auxiliar **GetVideoProfileSupportedDeviceIdAsync** es nulo o una cadena vacía, no hay ningún dispositivo en el panel especificado que admita perfiles de cámara. En este caso, se debe inicializar el dispositivo de captura multimedia sin usar perfiles.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>Seleccionar un perfil basado en la resolución y velocidad de fotogramas compatibles

Para seleccionar un perfil con funcionalidades concretas, como la capacidad de lograr una resolución y una velocidad de fotogramas concretas, se debe llamar al método auxiliar definido anteriormente para obtener el identificador de un dispositivo de captura que admita el uso de perfiles de cámara.

Crea un objeto [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) nuevo, pasando el identificador de dispositivo seleccionado. A continuación, realiza una llamada al método estático [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) para obtener una lista de todos los perfiles de cámara compatibles con el dispositivo.

Este ejemplo usa un método de consulta Linq, incluido en el espacio de nombres **System.Linq** de "using", para seleccionar un perfil que contenga un objeto [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) donde las propiedades [**Width**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height) y [**FrameRate**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) coinciden con los valores solicitados. Si se encuentra una coincidencia, [**VideoProfile**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) y [**RecordMediaDescription**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) de **MediaCaptureInitializationSettings** se definen con los valores de tipo anónimo devueltos desde la consulta Linq. Si no se encuentra ninguna coincidencia, se usa el perfil predeterminado.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Tras rellenar **MediaCaptureInitializationSettings** con el perfil de cámara deseado, simplemente, realiza una llamada a [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) en el objeto de captura multimedia para configurarlo en el perfil deseado.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="use-media-frame-source-groups-to-get-profiles"></a>Usar grupos de orígenes de fotogramas multimedia para obtener perfiles

A partir de Windows 10, versión 1803, puede usar la clase [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup) para obtener perfiles de cámara con capacidades específicas antes de inicializar el objeto **MediaCapture** . Los grupos de orígenes de fotogramas permiten a los fabricantes de dispositivos representar grupos de sensores o capacidades de captura como un único dispositivo virtual. Esto permite escenarios de fotografía computacional, como el uso de cámaras de profundidad y color, pero también se puede usar para seleccionar perfiles de cámara para escenarios de captura sencillos. Para obtener más información sobre el uso de **MediaFrameSourceGroup**, consulte [procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

En el método de ejemplo siguiente se muestra cómo usar los objetos **MediaFrameSourceGroup** para buscar un perfil de cámara que admita un perfil de vídeo conocido, como uno que admita la secuencia de fotos HDR o variable. En primer lugar, llame a [**MediaFrameSourceGroup. FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) para obtener una lista de todos los grupos de orígenes de fotogramas multimedia disponibles en el dispositivo actual. Recorra cada grupo de origen y llame a [**MediaCapture. FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) para obtener una lista de todos los perfiles de vídeo del grupo de origen actual que admiten el perfil especificado, en este caso HDR con WCG Photo. Si se encuentra un perfil que cumpla los criterios, cree un nuevo objeto **MediaCaptureInitializationSettings** y establezca el **perfil** de videosource en el perfil SELECT y el **VideoDeviceId** en la propiedad **ID** del grupo de origen del marco multimedia actual. Por lo tanto, por ejemplo, puede pasar el valor **KnownVideoProfile. HdrWithWcgVideo** a este método para obtener la configuración de la captura multimedia que admite vídeo HDR. Pase **KnownVideoProfile. VariablePhotoSequence** para obtener la configuración que admite la secuencia de fotográficas variable.

 [!code-cs[FindKnownVideoProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindKnownVideoProfile)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>Usar perfiles conocidos para buscar un perfil que admita vídeo HDR (técnica heredada)

> [!NOTE] 
> Las API descritas en esta sección están desusadas a partir de Windows 10, versión 1803. Vea la sección anterior, **usar grupos de orígenes de fotogramas multimedia para obtener perfiles**.

La selección de un perfil que admita HDR comienza como los otros escenarios. Cree un **MediaCaptureInitializationSettings** y una cadena que contenga el identificador del dispositivo de captura. Agrega una variable booleana que realizará un seguimiento de si se admite vídeo HDR.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Usa el método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido anteriormente para obtener el identificador de dispositivo para un dispositivo de captura que admita perfiles de cámara.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

El método estático [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) devuelve los perfiles de cámara compatibles con el dispositivo especificado que se clasifica por el valor [**KnownVideoProfile**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) especificado. Para este escenario, el valor **VideoRecording** se especifica para limitar los perfiles de cámara devueltos a aquellos que admiten la grabación de vídeo.

Recorra la lista de perfiles de cámara devueltos. Para cada perfil de cámara, recorre cada [**VideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) en la comprobación de perfiles para determinar si la propiedad [**IsHdrVideoSupported**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) es true. Tras encontrar una descripción de elemento multimedia adecuada, interrumpe el recorrido y asigna el perfil y los objetos de la descripción al objeto **MediaCaptureInitializationSettings**.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>Determinar si un dispositivo admite captura simultánea de fotos y vídeo

Muchos dispositivos admiten la captura de fotos y vídeo simultáneamente. Para determinar si un dispositivo de captura admite esto, realiza una llamada a [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) para obtener todos los perfiles de cámara admitidos por el dispositivo. Usa una consulta de vínculo para encontrar un perfil que tenga al menos una entrada para [**SupportedPhotoMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) y [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription), lo que significa que el perfil admite la captura simultánea.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Puedes ajustar esta consulta para buscar los perfiles que admiten resoluciones específicas y otras funcionalidades, además de la grabación de vídeo simultánea. También puedes usar el objeto [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) y especificar el valor [**BalancedVideoAndPhoto**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) para recuperar perfiles que admitan la captura simultánea, pero la consulta de todos los perfiles proporcionará resultados más completos.

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
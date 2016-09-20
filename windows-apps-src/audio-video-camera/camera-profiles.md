---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: "En este artículo se describe cómo usar perfiles de cámara para detectar y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo."
title: "Perfiles de cámara"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 755b2747b2250c4ad19970095aed220551389471

---

# Perfiles de cámara

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este artículo se describe cómo usar perfiles de cámara para descubrir y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo.

**Nota**  
Este artículo se basa en los conceptos y el código analizados en [Capturar fotos y vídeo con MediaCapture](capture-photos-and-video-with-mediacapture.md), donde se describen los pasos para la implementación de la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código de este artículo supone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

 

## Acerca de los perfiles de cámara

Las cámaras de diferentes dispositivos admiten diferentes funcionalidades, incluido el conjunto de resoluciones de captura admitidas, la velocidad de fotogramas para capturas de vídeo y si se admite HDR y las capturas de velocidad de fotogramas variable. El marco de captura multimedia de la Plataforma universal de Windows (UWP) almacena este conjunto de funcionalidades en un [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695). Un perfil de cámara, representado por un objeto [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694), tiene tres conjuntos de descripciones de elementos multimedia; uno para la captura de fotos, uno para la captura de vídeo y otro para la vista previa de vídeo.

Antes de inicializar el objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md), puedes consultar a los dispositivos de captura en el dispositivo actual para ver qué perfiles se admiten. Cuando se selecciona un perfil admitido, sabe que el dispositivo de captura admite todas las funcionalidades en las descripciones de elementos multimedia del perfil. Esto elimina la necesidad de un enfoque de prueba y error para determinar qué combinaciones de funcionalidades se admiten en un dispositivo determinado.

En el artículo sobre la captura multimedia básica, [Capturar fotos y vídeo con MediaCapture](capture-photos-and-video-with-mediacapture.md), la clase [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) usada para inicializar la captura multimedia se crea solo con la cadena de identificador del dispositivo de captura, la cantidad mínima de datos necesarios para la inicialización.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

Los ejemplos de código de este artículo reemplazan esta inicialización mínima con la detección de perfiles de cámara que admiten diferentes funcionalidades, que, a continuación, se usan para inicializar el dispositivo de captura multimedia.

## Buscar un dispositivo de vídeo que admita perfiles de cámara

Antes de buscar perfiles de cámara compatibles, debes buscar un dispositivo de captura que admita el uso de perfiles de cámara. El método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido en el ejemplo siguiente usa el método [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) para recuperar una lista de todos los dispositivos de captura de vídeo disponibles. Recorre todos los dispositivos en la lista, llamando al método estático, [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714), para que cada dispositivo vea si admite perfiles de vídeo. Además, la propiedad [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) para cada dispositivo, que permite especificar si quieres que la cámara esté en la parte frontal o posterior del dispositivo.

Si se encuentra un dispositivo compatible con los perfiles de cámara en el panel especificado, se devuelve el valor [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437), que contiene la cadena de identificador del dispositivo.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Si el identificador de dispositivo devuelto desde el método auxiliar **GetVideoProfileSupportedDeviceIdAsync** es nulo o una cadena vacía, no hay ningún dispositivo en el panel especificado que admita perfiles de cámara. En este caso, se debe inicializar el dispositivo de captura multimedia sin usar perfiles.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## Seleccionar un perfil basado en la resolución y velocidad de fotogramas compatibles

Para seleccionar un perfil con funcionalidades concretas, como la capacidad de lograr una resolución y una velocidad de fotogramas concretas, se debe llamar al método auxiliar definido anteriormente para obtener el identificador de un dispositivo de captura que admita el uso de perfiles de cámara.

Crea un objeto [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) nuevo, pasando el identificador de dispositivo seleccionado. A continuación, realiza una llamada al método estático [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) para obtener una lista de todos los perfiles de cámara compatibles con el dispositivo.

Este ejemplo usa un método de consulta Linq, incluido en el espacio de nombres **System.Linq** de "using", para seleccionar un perfil que contenga un objeto [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) donde las propiedades [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) y [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) coinciden con los valores solicitados. Si se encuentra una coincidencia, [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) y [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) de **MediaCaptureInitializationSettings** se definen con los valores de tipo anónimo devueltos desde la consulta Linq. Si no se encuentra ninguna coincidencia, se usa el perfil predeterminado.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Una vez que rellenas **MediaCaptureInitializationSettings** con el perfil de cámara deseado, simplemente, realiza una llamada a [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) en el objeto de captura multimedia para configurarlo en el perfil deseado.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## Seleccionar un perfil que admita la simultaneidad

Puedes usar los perfiles de cámara para determinar si un dispositivo admite la captura de vídeo desde varias cámaras al mismo tiempo. Para este escenario, debes crear dos conjuntos de objetos de captura, uno para la cámara frontal y otro para la posterior. Por cada cámara, crea un objeto **MediaCapture**, un objeto **MediaCaptureInitializationSettings** y una cadena que contenga el identificador de dispositivo de captura. Además, agrega una variable booleana que realizará un seguimiento de si se admite la simultaneidad.

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

El método estático [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709) devuelve una lista de los perfiles de cámara que son compatibles con el dispositivo de captura especificado que también admite la simultaneidad. Usa una consulta Linq para encontrar un perfil que admita la simultaneidad y que sea compatible con la cámara frontal y posterior. Si se encuentra un perfil que cumpla estos requisitos, establece el perfil en cada uno de los objetos **MediaCaptureInitializationSettings** y define la variable de seguimiento de simultaneidad booleano como true.

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

Llama a **MediaCapture.InitializeAsync** para la cámara principal del escenario de la aplicación. Si se admite la simultaneidad, inicializa también la segunda cámara.

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## Usa perfiles conocidos para encontrar un perfil que admita vídeo HDR

La selección de un perfil que admita HDR comienza como los otros escenarios. Crea un **MediaCaptureInitializationSettings** y una cadena que contenga el identificador de dispositivo de captura. Agrega una variable booleana que realizará un seguimiento de si se admite vídeo HDR.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Usa el método auxiliar **GetVideoProfileSupportedDeviceIdAsync** definido anteriormente para obtener el identificador de dispositivo para un dispositivo de captura que admita perfiles de cámara.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

El método estático [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) devuelve los perfiles de cámara compatibles con el dispositivo especificado que se clasifica por el valor [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) especificado. Para este escenario, el valor **VideoRecording** se especifica para limitar los perfiles de cámara devueltos a aquellos que admiten la grabación de vídeo.

Recorra la lista de perfiles de cámara devueltos. Para cada perfil de cámara, recorra cada [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) en la comprobación de perfiles para determinar si la propiedad [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) es true. Una vez que se encuentre una descripción de elemento multimedia adecuada, interrumpe el recorrido y asigna el perfil y los objetos de la descripción al objeto **MediaCaptureInitializationSettings**.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## Determinar si un dispositivo admite captura simultánea de fotos y vídeo

Muchos dispositivos admiten la captura de fotos y vídeo simultáneamente. Para determinar si un dispositivo de captura admite esto, realiza una llamada a [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) para obtener todos los perfiles de cámara admitidos por el dispositivo. Usa una consulta de vínculo para encontrar un perfil que tenga al menos una entrada para [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) y [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705), lo que significa que el perfil admite la captura simultánea.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Puedes ajustar esta consulta para buscar los perfiles que admiten resoluciones específicas y otras funcionalidades, además de la grabación de vídeo simultánea. También puedes usar el objeto [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) y especificar el valor [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) para recuperar perfiles que admitan la captura simultánea, pero la consulta de todos los perfiles proporcionará resultados más completos.

## Temas relacionados

* [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->



---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: Este tema muestra cómo aplicar efectos a la vista previa de la cámara y a las secuencias de grabación de vídeo, y muestra cómo usar el efecto de estabilización de vídeo.
title: Efectos para captura de vídeos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffb379110a42579cd5bb2427f9c851637ff191be
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362678"
---
# <a name="effects-for-video-capture"></a>Efectos para captura de vídeos


Este tema muestra cómo aplicar efectos a la vista previa de la cámara y a las secuencias de grabación de vídeo, y muestra cómo usar el efecto de estabilización de vídeo.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>Agregar y quitar efectos de la secuencia de vídeo de la cámara
Para capturar u obtener una vista previa de vídeo desde la cámara del dispositivo, usa el objeto [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) como se describe en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md). Después de inicializar el objeto **MediaCapture**, puedes agregar uno o más efectos de vídeo a la secuencia de vista previa o de captura mediante llamando a [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync), pasando un objeto [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) que representa el efecto que se va a agregar y un miembro de la enumeración [**MediaStreamType**](/uwp/api/Windows.Media.Capture.MediaStreamType) que indica si el efecto debe agregarse a la secuencia de vista previa de la cámara o a la secuencia de grabación.

> [!NOTE]
> En algunos dispositivos, la secuencia de vista previa y la secuencia de captura son la misma, lo que significa que si especificas **MediaStreamType.VideoPreview** o **MediaStreamType.VideoRecord** cuando llames a **AddVideoEffectAsync**, el efecto se aplicará tanto a las secuencias de vista previa como las de grabación. Puedes determinar si las secuencias de vista previa y de grabación son la misma en el dispositivo actual comprobando la propiedad [**VideoDeviceCharacteristic**](/uwp/api/windows.media.capture.mediacapturesettings.videodevicecharacteristic) de [**MediaCaptureSettings**](/uwp/api/windows.media.capture.mediacapture.mediacapturesettings) para el objeto **MediaCapture**. Si el valor de esta propiedad es **VideoDeviceCharacteristic.AllStreamsIdentical** o **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical**, entonces las secuencias son la misma y cualquier efecto que apliques a una afectará a la otra.

El siguiente ejemplo agrega un efecto tanto a la secuencia de vista previa de la cámara como a la secuencia de grabación. Este ejemplo muestra la comprobación para ver si las secuencias de vista previa y de grabación son la misma.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetBasicAddEffect":::

Ten en cuenta que **AddVideoEffectAsync** devuelve un objeto que implementa [**IMediaExtension**](/uwp/api/Windows.Media.IMediaExtension) que representa el efecto de vídeo agregado. Algunos efectos te permiten cambiar la configuración del efecto pasando un [**PropertySet**](/uwp/api/Windows.Foundation.Collections.PropertySet) al método [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties).

A partir de Windows 10, versión 1607, también puedes usar el objeto devuelto por **AddVideoEffectAsync** para quitar el efecto de la canalización de vídeo pasándolo a [**RemoveEffectAsync**](/uwp/api/windows.media.capture.mediacapture.removeeffectasync). **RemoveEffectAsync** determina automáticamente si el parámetro del objeto de efecto se ha agregado a la secuencia de vista previa o a la de grabación, por lo que no es necesario especificar el tipo de secuencia al realizar la llamada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetRemoveOneEffect":::

También puedes quitar todos los efectos de la secuencia de vista previa o de captura llamando a [**ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) y especificando la secuencia para la que se deben quitar todos los efectos.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetClearAllEffects":::

## <a name="video-stabilization-effect"></a>Efecto de estabilización de vídeo

El efecto de estabilización de vídeo manipula los marcos de una secuencia de vídeo para minimizar la sacudida causada por sujetar el dispositivo de captura con la mano. Dado que esta técnica hace que los píxeles de desplazamiento se muevan a derecha, izquierda, arriba o abajo, y como el efecto no puede saber cuál es el contenido que queda justo fuera del marco de vídeo, el vídeo estabilizado se recorta ligeramente en relación al vídeo original. Se proporciona una función de utilidad que te permite ajustar la configuración para administrar de forma óptima el corte realizado por el efecto de codificación de vídeo.

En los dispositivos que lo admiten, la estabilización de imagen óptica (OIS) estabiliza el vídeo manipulando mecánicamente el dispositivo de captura y, por lo tanto, no es necesario recortar los bordes de los fotogramas de vídeos. Para obtener más información, consulta [Controles de dispositivo de captura para captura de vídeos](capture-device-controls-for-video-capture.md).

### <a name="set-up-your-app-to-use-video-stabilization"></a>Configurar tu aplicación para usar la estabilización de vídeo

Además de los espacios de nombres necesarios para la captura básica de elementos multimedia, si usas el efecto de estabilización de vídeo necesitarás el siguiente espacio de nombres.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetVideoStabilizationEffectUsing":::

Declara una variable de miembro para almacenar el objeto [**VideoStabilizationEffect**](/uwp/api/Windows.Media.Core.VideoStabilizationEffect). Como parte de la implementación del efecto, tendrás que modificar las propiedades de codificación que uses para codificar el vídeo capturado. Para ello, declara dos variables para almacenar una copia de seguridad tanto de la entrada inicial como de las propiedades de codificación de salida, para que así puedas restaurarlas más adelante cuando se deshabilite el efecto. Por último, declara una variable de miembro de tipo [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile), ya que se obtendrá acceso a este objeto desde varias ubicaciones dentro del código.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetDeclareVideoStabilizationEffect":::

Para este escenario, debes asignar el objeto de perfil de codificación multimedia a una variable de miembro para poder acceder a él más adelante.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetEncodingProfileMember":::

### <a name="initialize-the-video-stabilization-effect"></a>Inicializar el efecto de estabilización de vídeo

Después de inicializar el objeto **MediaCapture**, puedes crear una nueva instancia del objeto [**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition). Llama a [**MediaCapture.AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) para agregar el efecto a la canalización de vídeo y recuperar una instancia de clase [**VideoStabilizationEffect**](/uwp/api/Windows.Media.Core.VideoStabilizationEffect). Especifica [**MediaStreamType.VideoRecord**](/uwp/api/Windows.Media.Capture.MediaStreamType) para indicar que el efecto debe aplicarse a la secuencia de grabación del vídeo.

Registra un controlador de eventos para el evento [**EnabledChanged**](/uwp/api/windows.media.core.videostabilizationeffect.enabledchanged) y llama al método auxiliar **SetUpVideoStabilizationRecommendationAsync**; describiremos ambos más adelante en este artículo. Por último, establece la propiedad [**Enabled**](/uwp/api/windows.media.core.videostabilizationeffect.enabled) del efecto en "true" para habilitar el efecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetCreateVideoStabilizationEffect":::

### <a name="use-recommended-encoding-properties"></a>Usa las propiedades de codificación recomendadas

Tal como se explicó anteriormente en este artículo, la técnica que el efecto de estabilización del vídeo usa, hace que el vídeo estabilizado se recorte ligeramente en relación al vídeo de origen. Define la siguiente función auxiliar en el código para ajustar las propiedades de codificación de vídeo para controlar esta limitación del efecto de forma óptima. Este paso no es necesario para poder usar el efecto de estabilización de vídeo, pero si lo no realizas, el vídeo resultante se escalará un poco verticalmente y, por lo tanto, la fidelidad visual será un poco más baja.

Llama al método [**GetRecommendedStreamConfiguration**](/uwp/api/windows.media.core.videostabilizationeffect.getrecommendedstreamconfiguration) que se encuentra en la instancia del efecto de estabilización de vídeo y pasa el objeto [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) (el cual informará al efecto sobre las propiedades de codificación de secuencia) y la clase **MediaEncodingProfile** (la cual permite al efecto conocer las propiedades de codificación de salida actuales). Este método devuelve un objeto [**VideoStreamConfiguration**](/uwp/api/Windows.Media.Capture.VideoStreamConfiguration) que contiene nuevas propiedades recomendadas de codificación de la secuencia de entrada y salida.

Una propiedad de codificación de entrada recomendada es, en caso de ser compatible con el dispositivo, una resolución más alta que la configuración inicial que proporcionaste; gracias a ella, habrá una pérdida mínima en la resolución después de aplicar la opción de recorte del efecto.

Llama a [**VideoDeviceController.SetMediaStreamPropertiesAsync**](/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync) para establecer las nuevas propiedades de codificación. Antes de establecer las propiedades nuevas, usa la variable de miembro para almacenar las propiedades iniciales de codificación y que así puedas cambiar la configuración cuando se desactive el efecto.

Si el efecto de estabilización de vídeo debe recortar el vídeo de salida, la codificación de propiedades de salida recomendada será del tamaño del vídeo recortado. Esto significa que la resolución de salida coincidirá con el tamaño del vídeo recortado. Si no usas las propiedades de salida recomendadas, el vídeo se escalará verticalmente para que coincida con el tamaño de salida inicial, lo que provocará una pérdida de fidelidad visual.

Establece la propiedad [**Video**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.video) del objeto **MediaEncodingProfile**. Antes de establecer las propiedades nuevas, usa la variable de miembro para almacenar las propiedades iniciales de codificación y que así puedas cambiar la configuración cuando se desactive el efecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetSetUpVideoStabilizationRecommendationAsync":::

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>Controlar el efecto de estabilización de vídeo que se va a deshabilitar

El sistema puede deshabilitar automáticamente el efecto de estabilización de vídeo si el rendimiento de píxeles es demasiado alto para que el efecto pueda controlarlo, o si detecta que el efecto se está ejecutando lentamente. Si esto ocurre, se genera el evento EnabledChanged. La instancia **VideoStabilizationEffect** del parámetro *sender* indica el nuevo estado del efecto: habilitado o deshabilitado. La clase [**VideoStabilizationEffectEnabledChangedEventArgs**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedEventArgs) tiene un valor [**VideoStabilizationEffectEnabledChangedReason**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedReason) que indica por qué se habilitó o deshabilitó el efecto. Ten en cuenta que este evento también se genera, si mediante programación, habilitas o deshabilitas el efecto; en tal caso, el motivo será **Programmatic**.

Por lo general, deberías usar este evento para ajustar la interfaz de usuario de la aplicación para indicar el estado actual de la estabilización de vídeo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetVideoStabilizationEnabledChanged":::

### <a name="clean-up-the-video-stabilization-effect"></a>Limpiar el efecto de estabilización de vídeo

Para limpiar el efecto de estabilización de vídeo, llama al método [**RemoveEffectAsync**](/uwp/api/windows.media.capture.mediacapture.removeeffectasync) para quitar el efecto de la canalización de vídeo. Si las variables de miembro que contienen las propiedades de codificación iniciales no son nulas, úsalas para restaurar las propiedades de codificación. Por último, quita el controlador de eventos **EnabledChanged** y define el efecto como "null".

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs" id="SnippetCleanUpVisualStabilizationEffect":::

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 

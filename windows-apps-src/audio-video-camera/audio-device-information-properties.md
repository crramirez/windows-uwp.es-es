---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: En este artículo se enumeran las propiedades DeviceInformation relacionadas con dispositivos de audio.
title: Propiedades de información de dispositivo de audio
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 443bcc3c0280aca85de31d8c9f3704302432cb76
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7979439"
---
# <a name="audio-device-information-properties"></a>Propiedades de información de dispositivo de audio

En este artículo se enumeran las propiedades de información de dispositivos relacionadas con dispositivos de audio. En Windows, cada dispositivo de hardware tiene asociadas las propiedades [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) que proporcionan información detallada acerca de un dispositivo y que puedes usar cuando necesitas información específica sobre el dispositivo o al crear un selector de dispositivos. Para obtener información general sobre la enumeración de dispositivos en Windows, consulta [Enumerar dispositivos](../devices-sensors/enumerate-devices.md) y [Propiedades de información de dispositivo](../devices-sensors/device-information-properties.md).


|Nombre|Tipo|Descripción|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Doble|Especifica la sensibilidad del micrófono en decibelios en relación con las unidades de la escala completa (dBFS).|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|Doble|Especifica la relación de señal/ruido (SNR) del de micrófono medida en unidades de decibelio (dB).|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Booleano|Indica si el dispositivo de audio admite el procesamiento de voz.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Booleano|Indica si el dispositivo de audio admite el procesamiento sin formato.|
|**System.Devices.MicrophoneArray.Geometry**|unsigned char[]|Datos de geometría para varios micrófonos.|

## <a name="related-topics"></a>Temas relacionados

* [Enumerar dispositivos](../devices-sensors/enumerate-devices.md)
* [Propiedades de información de dispositivo](../devices-sensors/device-information-properties.md)
* [Crear un selector de dispositivos](../devices-sensors/build-a-device-selector.md)
* [Reproducción de contenido multimedia](media-playback.md)





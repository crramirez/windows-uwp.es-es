---
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: En este artículo se muestra cómo usar la interfaz IMediaEncodingProperties para establecer la resolución y la velocidad de fotogramas de la secuencia de vista previa de la cámara, así como de las fotos y los vídeos capturados.
title: Establecer el formato, la resolución y la velocidad de fotogramas para MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f81ab1ef635bf4cfb20c289d6998c242f7aa47fc
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7970690"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>Establecer el formato, la resolución y la velocidad de fotogramas para MediaCapture



En este artículo se muestra cómo usar la interfaz [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) para establecer la resolución y la velocidad de fotogramas de la secuencia de vista previa de la cámara, así como de las fotos y los vídeos capturados. También se muestra cómo asegurarse de que la relación de aspecto de la secuencia de vista previa coincida con la de la secuencia multimedia capturada.

Los perfiles de cámara ofrecen una forma más avanzada de descubrir y establecer las propiedades de la secuencia de la cámara, pero no se admiten en todos los dispositivos. Para más información, consulta [Perfiles de cámara](camera-profiles.md).

El código de este artículo es una adaptación de la [muestra de CameraResolution](http://go.microsoft.com/fwlink/p/?LinkId=624252&clcid=0x409). Puedes descargar la muestra para ver el código usado en contexto o para usar la muestra como punto de partida para tu propia aplicación.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para la implementación de la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## <a name="a-media-encoding-properties-helper-class"></a>Clase auxiliar de propiedades de codificación multimedia

Crear una clase auxiliar simple para encapsular la funcionalidad de la interfaz [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) facilita la selección de un conjunto de propiedades de codificación que cumplan criterios particulares. Esta clase auxiliar resulta especialmente útil debido al comportamiento de la característica de propiedades de codificación siguiente:

**Advertencia**  el método [**VideoDeviceController.GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) toma un miembro de la enumeración [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640) , como **VideoRecord** o **fotos**y devuelve una lista de cualquier [** ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) o [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) los objetos que transmiten la secuencia de codificación de configuración, como la resolución de la foto o vídeo. Los resultados de la llamada a **GetAvailableMediaStreamProperties** puede incluir **ImageEncodingProperties** o **VideoEncodingProperties**, independientemente de qué valor **MediaStreamType** se especifique. Por este motivo, siempre debe comprobar el tipo de cada valor devuelto y convertirlo al tipo apropiado antes de intentar acceder a cualquiera de los valores de propiedad.

La clase auxiliar que se define a continuación controla la comprobación y la conversión del tipo de [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) o [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) para que el código de la aplicación no tenga que distinguir entre los dos tipos. Además, la clase auxiliar expone las propiedades de la relación de aspecto de las propiedades, la velocidad de fotogramas (solo de las propiedades de codificación de vídeo) y un nombre descriptivo que facilita la visualización de las propiedades de codificación en la interfaz de usuario de la aplicación.

Debes incluir el espacio de nombres [**Windows.Media.MediaProperties**](https://msdn.microsoft.com/library/windows/apps/hh701296) en el archivo de origen de la clase auxiliar.

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>Determinar si las secuencias de vista previa y captura son independientes

En algunos dispositivos, se usa el mismo pin de hardware para las secuencias de vista previa y captura. En estos dispositivos, al establecer las propiedades de codificación de uno también se establecen las del otro. En los dispositivos que usan pines de hardware diferentes para la vista previa y la captura, las propiedades pueden establecerse por separado para cada secuencia. Usa el siguiente código para determinar si las capturas de vista previa y captura son independientes. Debes ajustar la interfaz de usuario para habilitar o deshabilitar la configuración de las secuencias de forma independiente en función del resultado de esta prueba.

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>Obtener una lista de propiedades de secuencia disponibles

Obtén una lista de las propiedades de secuencia disponibles para un dispositivo de captura obteniendo el [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) del objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md) de tu aplicación y, después, llama a [**GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) y pasa uno de los valores de [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640), **VideoPreview**, **VideoRecord** o **Photo**. En este ejemplo, se usa la sintaxis de Linq para crear una lista de objetos **StreamPropertiesHelper**, que se definió anteriormente en este artículo, para cada uno de los valores de [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) devueltos de **GetAvailableMediaStreamProperties**. En primer lugar, en este ejemplo se usan los métodos de extensión Linq para ordenar las propiedades devueltas primero según la resolución y, luego, según la velocidad de fotogramas.

Si la aplicación tiene requisitos de velocidad de fotogramas o de resolución específicos, puedes seleccionar un conjunto de propiedades de codificación multimedia mediante programación. En su lugar, una aplicación de cámara típica expondrá la lista de propiedades disponibles en la interfaz de usuario y permitirá que el usuario seleccione la configuración que desee. Se crea un **ComboBoxItem** para cada elemento de la lista de objetos **StreamPropertiesHelper** de la lista. El contenido se establece en el nombre descriptivo devuelto por la clase auxiliar y la etiqueta se establece en la clase auxiliar en sí, para que se pueda usar más adelante con el fin de recuperar las propiedades de codificación asociadas. Cada objeto **ComboBoxItem** se agrega posteriormente al objeto **ComboBox** que se pasa al método.

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>Establecer las propiedades de secuencia deseadas

Indica al controlador de dispositivo de vídeo que use tus propiedades de codificación deseadas, para lo cual debe llamar a [**SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) y pasar el valor **MediaStreamType** que indica si se deben establecer las propiedades de foto, vídeo o vista previa. En este ejemplo se establecen las propiedades de codificación solicitadas cuando el usuario selecciona un elemento en uno de los objetos **ComboBox** que se rellenan con el método auxiliar **PopulateStreamPropertiesUI**.

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>Coincidir con la relación de aspecto de las secuencias de vista previa y captura

Una aplicación de cámara típica proporcionará la interfaz de usuario para el usuario de modo que pueda seleccionar la resolución de captura de vídeo o foto, pero establecerá mediante programación la resolución de vista previa. Hay algunas estrategias diferentes para seleccionar la mejor resolución de secuencia de vista previa de la aplicación:

-   Selecciona la mayor resolución de vista previa disponible, dejando que el marco de trabajo de la interfaz de usuario realice la escala necesaria para la vista previa.

-   Selecciona la resolución de vista previa más cercana a la resolución de captura para que la vista previa muestre la representación más cercana al contenido multimedia capturado final.

-   Selecciona la resolución de vista previa más cercana al tamaño del objeto [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278), de modo que no se canalicen en la secuencia de vista previa más píxeles que los que se necesiten.

**Importante**  es posible, en algunos dispositivos, para establecer una relación de aspecto diferente para la secuencia de vista previa de la cámara y la secuencia de captura. El recorte de fotogramas que provoca este error de coincidencia puede dar como resultado contenido presente en los archivos multimedia capturados que no eran visibles en la vista previa, lo que puede provocar una experiencia de usuario negativa. Se recomienda encarecidamente usar la misma relación de aspecto, con un período de tolerancia reducido, para las secuencias de vista previa y captura. Se pueden tener resoluciones totalmente diferentes habilitadas para la captura y la vista previa, siempre que la relación de aspecto sea muy aproximada.


Para garantizar que las secuencias de captura de foto o vídeo coincidan con la relación de aspecto de la secuencia de vista previa, este ejemplo llama a [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) y pasa el valor de enumeración **VideoPreview** para solicitar las propiedades de secuencia actuales de la secuencia de vista previa. A continuación, se define un período de tolerancia de relación de aspecto reducido para que se puedan incluir relaciones de aspecto que no sean exactamente iguales que las de la secuencia de vista previa, siempre que sean aproximadas. A continuación, se usa un método de extensión Linq para seleccionar solo los objetos **StreamPropertiesHelper** donde la relación de aspecto se encuentra dentro del intervalo de tolerancia definido de la secuencia de vista previa.

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 





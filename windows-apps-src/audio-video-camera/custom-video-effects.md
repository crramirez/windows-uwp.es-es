---
description: En este artículo se describe cómo crear un componente de Windows Runtime que implemente la interfaz IBasicVideoEffect para permitir la creación de efectos personalizados para las secuencias de vídeo.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Efectos de vídeo personalizados
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 119d444f073c4f668dcdc63fc118ed408eca8e32
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030498"
---
# <a name="custom-video-effects"></a>Efectos de vídeo personalizados




En este artículo se describe cómo crear un componente de Windows Runtime que implemente la interfaz [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) para permitir la creación de efectos personalizados para las secuencias de vídeo. Se pueden usar efectos personalizados con distintas API de Windows Runtime, como [MediaCapture](/uwp/api/Windows.Media.Capture.MediaCapture), que proporciona acceso a la cámara del dispositivo, y [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition), que permite crear composiciones complejas a partir de clips multimedia.

## <a name="add-a-custom-effect-to-your-app"></a>Agregar un efecto personalizado a la aplicación


Un efecto de vídeo personalizado se define en una clase que implementa la interfaz [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect). Esta clase no se puede incluir directamente en el proyecto de la aplicación. En su lugar, debes usar un componente de Windows Runtime para hospedar la clase de efecto de vídeo.

**Agregar un componente de Windows Runtime para el efecto de vídeo**

1.  En Microsoft Visual Studio, una vez abierta la solución, vaya al menú **archivo** y seleccione **agregar &gt; nuevo proyecto** .
2.  Selecciona el tipo de proyecto **Componente de Windows Runtime (Windows Universal)** .
3.  En este ejemplo, asigne al proyecto el nombre *VideoEffectComponent* . Se hará referencia a este nombre en el código más adelante.
4.  Haga clic en **Aceptar** .
5.  La plantilla de proyecto crea una clase denominada Class1.cs. En el **Explorador de soluciones** , haz clic con el botón derecho en el icono de Class1.cs y selecciona **Cambiar nombre** .
6.  Cambie el nombre del archivo a *ExampleVideoEffect.CS* . Visual Studio mostrará un mensaje que pregunta si quieres actualizar todas las referencias con el nuevo nombre. Haga clic en **Sí** .
7.  Abra **ExampleVideoEffect.CS** y actualice la definición de clase para implementar la interfaz [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetImplementIBasicVideoEffect":::


Debes incluir los siguientes espacios de nombres en el archivo de clase del efecto para tener acceso a todos los tipos usados en los ejemplos de este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetEffectUsing":::


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>Implementar la interfaz IBasicVideoEffect con el procesamiento de software


El efecto de vídeo debe implementar todos los métodos y propiedades de la interfaz [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect). En esta sección te orientamos por una implementación sencilla de esta interfaz que usa procesamiento de software.

### <a name="close-method"></a>Método Close

El sistema llamará al método [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) en la clase cuando se deba apagar el efecto. Debes usar este método para eliminar todos los recursos que hayas creado. El argumento para el método es un [**MediaEffectClosedReason**](/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) que le permite saber si el efecto se cerró normalmente, si se produjo un error o si el efecto no admite el formato de codificación necesario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetClose":::


### <a name="discardqueuedframes-method"></a>Método DiscardQueuedFrames

El método [**DiscardQueuedFrames**](/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) se invoca cuando se debe restablecer el efecto. Un escenario típico se produce si el efecto almacena los fotogramas procesados anteriormente para usarlos en el procesamiento del fotograma actual. Cuando se llama a este método, se debe eliminar el conjunto de fotogramas anteriores guardados. Este método puede usarse para restablecer cualquier estado relacionado con fotogramas anteriores, no solo fotogramas de vídeo acumulados.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetDiscardQueuedFrames":::



### <a name="isreadonly-property"></a>IsReadOnly, propiedad

La propiedad [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) permite que el sistema sepa si el efecto se escribe en la salida del efecto. Si la aplicación no modifica los fotogramas de vídeo (por ejemplo, un efecto que solo realiza análisis de fotogramas de vídeo), debes definir esta propiedad como true, lo que hará que el sistema copie eficazmente la entrada de fotograma a la salida de fotograma.

> [!TIP]
> Cuando la propiedad [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) se define como true, el sistema copia el fotograma de entrada al fotograma de salida antes de llamar a [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe). Definir la propiedad **IsReadOnly** como true no te impide escribir en los fotogramas de salida del efecto en **ProcessFrame** .


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetIsReadOnly":::

### <a name="setencodingproperties-method"></a>Método SetEncodingProperties

El sistema llama a [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) en el efecto para comunicarte las propiedades de codificación de la secuencia de vídeo a que se aplica el efecto. Este método también proporciona una referencia al dispositivo Direct3D usado para la representación de hardware. El uso de este dispositivo se muestra en el ejemplo de procesamiento de hardware más adelante en este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetEncodingProperties":::


### <a name="supportedencodingproperties-property"></a>Propiedad SupportedEncodingProperties

El sistema comprueba la propiedad [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) para determinar qué propiedades de codificación son compatibles con el efecto. Ten en cuenta que, si el consumidor del efecto no puede codificar vídeo mediante las propiedades especificadas, llamará a [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) en el efecto y quitará el efecto de la canalización de vídeo.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedEncodingProperties":::


> [!NOTE] 
> Si se devuelve una lista vacía de objetos [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) de **SupportedEncodingProperties** , el sistema tendrá como valor predeterminado la codificación ARGB32.

 

### <a name="supportedmemorytypes-property"></a>Propiedad SupportedMemoryTypes

El sistema comprueba la propiedad [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) para determinar si el efecto tendrá acceso a los fotogramas de vídeo en la memoria de software o hardware (GPU). Si devuelves [**MediaMemoryTypes.Cpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes), se pasarán al efecto fotogramas de entrada y salida que contienen datos de imagen en objetos [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap). Si devuelves **MediaMemoryTypes.Gpu** , se pasarán al efecto fotogramas de entrada y salida que contienen datos de imagen en objetos [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedMemoryTypes":::


> [!NOTE]
> Si especificas [**MediaMemoryTypes.GpuAndCpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes), el sistema usará la memoria del sistema o de la GPU, la que sea más eficiente para la canalización. Cuando uses este valor, debes comprobar el método [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) para ver si el valor de [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) o [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) pasado al método contiene datos y luego procesar el fotograma según corresponda.

 

### <a name="timeindependent-property"></a>Propiedad TimeIndependent

Gracias a la propiedad [**TimeIndependent**](/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent), el sistema puede saber si el efecto no requiere un tiempo uniforme. Si se establece en true, el sistema puede usar optimizaciones que mejoren el rendimiento del efecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetTimeIndependent":::

### <a name="setproperties-method"></a>Método SetProperties

El método [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) permite que la aplicación que está usando el efecto ajuste los parámetros de efecto. Las propiedades se pasan como mapa [**IPropertySet**](/uwp/api/Windows.Foundation.Collections.IPropertySet) de nombres de propiedad y valores.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetProperties":::


En este ejemplo sencillo, se oscurecerán los píxeles de cada fotograma de vídeo según un valor especificado. Se declara una propiedad y TryGetValue se usa para obtener el valor establecido por la aplicación que llama. Si no se ha establecido ningún valor, se usa un valor predeterminado de 0,5.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetFadeValue":::


### <a name="processframe-method"></a>Método ProcessFrame

El método [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) es donde el efecto modifica los datos de imagen del vídeo. El método se llama una vez por fotograma y se pasa un objeto [**ProcessVideoFrameContext**](/uwp/api/Windows.Media.Effects.ProcessVideoFrameContext). Este objeto contiene un objeto de entrada [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) que contiene el fotograma entrante para ser procesado y un resultado **VideoFrame** en el que se escriben los datos de imagen que se pasarán al resto de la canalización de vídeo. Cada uno de estos objetos **VideoFrame** tiene una propiedad [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) y una propiedad [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface), pero cuál de ellos se debe usar se determina a partir del valor devuelto de la propiedad [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes).

Este ejemplo muestra una implementación sencilla del método **ProcessFrame** mediante el procesamiento de software. Para obtener más información sobre cómo trabajar con objetos [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap), consulta [Imágenes](imaging.md). Más adelante se muestra un ejemplo de implementación de **ProcessFrame** mediante procesamiento de hardware.

El acceso al búfer de datos de **SoftwareBitmap** requiere interoperabilidad COM, por lo que debes incluir el espacio de nombres **System.Runtime.InteropServices** en el archivo de clase del efecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMUsing":::


Agrega el siguiente código dentro del espacio de nombres para que el efecto importe la interfaz para tener acceso al búfer de imagen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMImport":::


> [!NOTE]
> Dado que esta técnica obtiene acceso a un búfer de imagen nativo sin administrar, tendrás que configurar el proyecto para permitir un código no seguro.
> 1.  En Explorador de soluciones, haga clic con el botón derecho en el proyecto VideoEffectComponent y seleccione **propiedades** .
> 2.  Seleccione la pestaña **Compilar** .
> 3.  Active la casilla **permitir código no seguro** .

 

Ahora puedes agregar la implementación del método **ProcessFrame** . Primero, este método obtiene un objeto [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) de los mapas de bits de software de entrada y salida. Ten en cuenta que la trama de salida se abre para la escritura y, el de entrada, para la lectura. Luego, se obtiene un valor de [**IMemoryBufferReference**](/uwp/api/Windows.Foundation.IMemoryBufferReference) para cada búfer llamando a [**CreateReference**](/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference). A continuación, se obtiene el búfer de datos reales convirtiendo los objetos **IMemoryBufferReference** como la interfaz de interoperabilidad definida anteriormente, **IMemoryByteAccess** y, a continuación, se llama a **GetBuffer** .

Ahora que ha se han obtenido los búferes de datos, puedes leer del búfer de entrada y escribir en el búfer de salida. El diseño del búfer se obtiene llamando a [**GetPlaneDescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription), que proporciona información sobre el ancho, intervalo y desplazamiento inicial del búfer. Los bits por píxel se determinan a partir de las propiedades de codificación establecidas previamente con el método [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties). La información de formato de búfer se usa para buscar el índice en el búfer para cada píxel. El valor del píxel en el búfer de origen se copia en el búfer de destino con los valores de color multiplicados por la propiedad FadeValue definida para que este efecto los atenúe según la cantidad especificada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetProcessFrameSoftwareBitmap":::


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>Implementar la interfaz IBasicVideoEffect con el procesamiento de hardware


Crear un efecto de vídeo personalizado mediante el procesamiento de hardware (GPU) es casi igual que usar el procesamiento de software como se describió anteriormente. En esta sección se muestran algunas diferencias en un efecto que usa el procesamiento de hardware. En este ejemplo se usa la API Win2D de Windows Runtime. Para obtener más información sobre el uso de Win2D, consulta la [documentación de Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm).

Usa los siguientes pasos para agregar el paquete de NuGet Win2D en el proyecto creado como se describe en la sección **Agregar un efecto personalizado a la aplicación** al principio de este artículo.

**Para agregar el paquete de NuGet Win2D al proyecto de efecto**

1.  En el **Explorador de soluciones** , haz clic con el botón derecho en el proyecto **VideoEffectComponent** y selecciona **Administrar paquetes NuGet** .
2.  En la parte superior de la ventana, selecciona la pestaña **Examinar** .
3.  En el cuadro de búsqueda, escribe **Win2D** .
4.  Selecciona **Win2D.uwp** y luego selecciona **Instalar** en el panel derecho.
5.  En el cuadro de diálogo **Revisar cambios** se muestra el paquete que se instalará. Haga clic en **Aceptar** .
6.  Acepta la licencia del paquete.

Además de los espacios de nombres incluidos en la configuración básica del proyecto, debes incluir los siguientes espacios de nombres proporcionados por Win2D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetUsingWin2D":::


Dado que este efecto usa la memoria de GPU para el funcionamiento de los datos de imagen, debes devolver [**MediaMemoryTypes.Gpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes) de la propiedad [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedMemoryTypesWin2D":::


Establece las propiedades de codificación que admitirá el efecto con la propiedad [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties). Al trabajar con Win2D, debes usar la codificación ARGB32.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedEncodingPropertiesWin2D":::


Usa el método [**SetEncodingProperties**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para crear un nuevo objeto **CanvasDevice** de Win2D desde el valor de [**IDirect3DDevice**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DDevice) pasado al método.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetEncodingPropertiesWin2D":::


La implementación de [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) es idéntica al ejemplo de procesamiento de software anterior. En este ejemplo se usa una propiedad **BlurAmount** para configurar un efecto de desenfoque de Win2D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetPropertiesWin2D":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetBlurAmountWin2D":::


El último paso es implementar el método [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) que realmente procesa los datos de imagen.

Mediante las API de Win2D, se crea **CanvasBitmap** a partir de la propiedad [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) del fotograma de entrada. Se crea **CanvasRenderTarget** a partir del fotograma de salida **Direct3DSurface** , y **CanvasDrawingSession** a partir de este destino de representación. Se inicializa un nuevo efecto Win2D **GaussianBlurEffect** de Win2D mediante la propiedad **BlurAmount** que expone nuestro efecto a través de [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties). Por último, el método **CanvasDrawingSession.DrawImage** se llama para dibujar el mapa de bits de entrada en el destino de representación con el efecto de desenfoque.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetProcessFrameWin2D":::


## <a name="adding-your-custom-effect-to-your-app"></a>Agregar el efecto personalizado a tu aplicación


Para usar el efecto de vídeo desde la aplicación, debes agregar una referencia al proyecto de efecto a la aplicación.

1.  En el Explorador de soluciones, en el proyecto de la aplicación, haz clic con el botón derecho en **Referencias** y selecciona **Agregar referencia** .
2.  Expande la pestaña **Proyectos** , selecciona **Solución** y activa la casilla para el nombre del proyecto de efecto. Para este ejemplo, el nombre es *VideoEffectComponent* .
3.  Haga clic en **Aceptar** .

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>Agregar el efecto personalizado a una secuencia de vídeo de la cámara

Puedes configurar una secuencia simple de vista previa de la cámara siguiendo los pasos descritos en el artículo [Acceso fácil a la vista previa de cámara](simple-camera-preview-access.md). Sigue estos pasos que te proporcionarán un objeto [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) inicializado que se usa para obtener acceso a la secuencia de vídeo de la cámara.

Para agregar el efecto de vídeo personalizado en una secuencia de cámara, primero crea un nuevo objeto [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition), pasando el espacio de nombres y el nombre de clase para el efecto. A continuación, llame al método [**AddVideoEffect**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) del objeto **MediaCapture** para agregar su efecto al flujo especificado. Este ejemplo usa el valor [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) para especificar que se debe agregar el efecto a la secuencia de vista previa. Si la aplicación admite la captura de vídeo, también podrías usar **MediaStreamType.VideoRecord** para agregar el efecto a la secuencia de captura. **AddVideoEffect** devuelve un objeto [**IMediaExtension**](/uwp/api/Windows.Media.IMediaExtension) que representa el efecto personalizado. Puedes usar el método SetProperties para establecer la configuración del efecto.

Una vez que se haya agregado el efecto, se llama a [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync) para iniciar la secuencia de vista previa.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddVideoEffectAsync":::



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Agregar el efecto personalizado a un clip en MediaComposition

Para obtener instrucciones generales sobre la creación de composiciones multimedia a partir de clips de vídeo, consulta [Composiciones y edición multimedia](media-compositions-and-editing.md). En el siguiente fragmento de código se muestra la creación de una composición de contenido multimedia sencilla que usa un efecto de vídeo personalizado. Un objeto [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) se crea llamando a [**CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync), pasando un archivo de vídeo que seleccionó el usuario con [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) y agregando el vídeo a una nueva clase [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition). A continuación, se crea un nuevo objeto [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition) y se pasa el nombre del espacio de nombres y la clase para el efecto al constructor. Por último, se agrega la definición del efecto a la colección [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) del objeto **MediaClip** .


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddEffectToComposition":::


## <a name="related-topics"></a>Temas relacionados
* [Acceso fácil a la vista previa de cámara](simple-camera-preview-access.md)
* [Composiciones y edición multimedia](media-compositions-and-editing.md)
* [Documentación de Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [Reproducción de multimedia](media-playback.md)

---
Description: This article describes how to create a Windows Runtime component that implements the IBasicVideoEffect interface to allow you to create custom effects for video streams.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Efectos de vídeo personalizados
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: a9e796eee76025e7697c08669e6942e0d69206f7
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8808700"
---
# <a name="custom-video-effects"></a>Efectos de vídeo personalizados




En este artículo se describe cómo crear un componente de Windows Runtime que implemente la interfaz [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) para permitir la creación de efectos personalizados para las secuencias de vídeo. Se pueden usar efectos personalizados con distintas API de Windows Runtime, como [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), que proporciona acceso a la cámara del dispositivo, y [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), que permite crear composiciones complejas a partir de clips multimedia.

## <a name="add-a-custom-effect-to-your-app"></a>Agregar un efecto personalizado a la aplicación


Un efecto de vídeo personalizado se define en una clase que implementa la interfaz [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Esta clase no se puede incluir directamente en el proyecto de la aplicación. En su lugar, debes usar un componente de Windows Runtime para hospedar la clase de efecto de vídeo.

**Agregar un componente de Windows Runtime para el efecto de vídeo**

1.  En Microsoft Visual Studio, con la solución abierta, ve al menú **Archivo** y selecciona **Agregar-&gt;Nuevo proyecto**.
2.  Selecciona el tipo de proyecto **Componente de Windows Runtime (Windows Universal)**.
3.  Para este ejemplo, asigna al proyecto el nombre *VideoEffectComponent*. Se hará referencia a este nombre en el código más adelante.
4.  Haz clic en **Aceptar**.
5.  La plantilla de proyecto crea una clase denominada Class1.cs. En el **Explorador de soluciones**, haz clic con el botón derecho en el icono de Class1.cs y selecciona **Cambiar nombre**.
6.  Cambia el nombre del archivo a *ExampleVideoEffect.cs*. Visual Studio mostrará un mensaje que pregunta si quieres actualizar todas las referencias con el nuevo nombre. Haz clic en **Sí**.
7.  Abre **ExampleVideoEffect.cs** y actualiza la definición de clase para implementar la interfaz [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788).

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


Debes incluir los siguientes espacios de nombres en el archivo de clase del efecto para tener acceso a todos los tipos usados en los ejemplos de este artículo.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>Implementar la interfaz IBasicVideoEffect con el procesamiento de software


El efecto de vídeo debe implementar todos los métodos y propiedades de la interfaz [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). En esta sección te orientamos por una implementación sencilla de esta interfaz que usa procesamiento de software.

### <a name="close-method"></a>Método Close

El sistema llamará al método [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) en la clase cuando se deba apagar el efecto. Debes usar este método para eliminar todos los recursos que hayas creado. El argumento del método es un valor [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason) que te permite saber si se cerró el efecto con normalidad, si se produjo un error o si el efecto no admite el formato de codificación necesario.

[!code-cs[Close](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### <a name="discardqueuedframes-method"></a>Método DiscardQueuedFrames

El método [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) se invoca cuando se debe restablecer el efecto. Un escenario típico se produce si el efecto almacena los fotogramas procesados anteriormente para usarlos en el procesamiento del fotograma actual. Cuando se llama a este método, se debe eliminar el conjunto de fotogramas anteriores guardados. Este método puede usarse para restablecer cualquier estado relacionado con fotogramas anteriores, no solo fotogramas de vídeo acumulados.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### <a name="isreadonly-property"></a>Propiedad IsReadOnly

La propiedad [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) permite que el sistema sepa si el efecto se escribe en la salida del efecto. Si la aplicación no modifica los fotogramas de vídeo (por ejemplo, un efecto que solo realiza análisis de fotogramas de vídeo), debes definir esta propiedad como true, lo que hará que el sistema copie eficazmente la entrada de fotograma a la salida de fotograma.

> [!TIP]
> Cuando la propiedad [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) se define como true, el sistema copia el fotograma de entrada al fotograma de salida antes de llamar a [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794). Definir la propiedad **IsReadOnly** como true no te impide escribir en los fotogramas de salida del efecto en **ProcessFrame**.


[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)]

### <a name="setencodingproperties-method"></a>Método SetEncodingProperties

El sistema llama a [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) en el efecto para comunicarte las propiedades de codificación de la secuencia de vídeo a que se aplica el efecto. Este método también proporciona una referencia al dispositivo Direct3D usado para la representación de hardware. El uso de este dispositivo se muestra en el ejemplo de procesamiento de hardware más adelante en este artículo.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### <a name="supportedencodingproperties-property"></a>Propiedad SupportedEncodingProperties

El sistema comprueba la propiedad [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799) para determinar qué propiedades de codificación son compatibles con el efecto. Ten en cuenta que, si el consumidor del efecto no puede codificar vídeo mediante las propiedades especificadas, llamará a [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) en el efecto y quitará el efecto de la canalización de vídeo.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]


> [!NOTE] 
> Si se devuelve una lista vacía de objetos [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) de **SupportedEncodingProperties**, el sistema tendrá como valor predeterminado la codificación ARGB32.

 

### <a name="supportedmemorytypes-property"></a>Propiedad SupportedMemoryTypes

El sistema comprueba la propiedad [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) para determinar si el efecto tendrá acceso a los fotogramas de vídeo en la memoria de software o hardware (GPU). Si devuelves [**MediaMemoryTypes.Cpu**](https://msdn.microsoft.com/library/windows/apps/dn764822), se pasarán al efecto fotogramas de entrada y salida que contienen datos de imagen en objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358). Si devuelves **MediaMemoryTypes.Gpu**, se pasarán al efecto fotogramas de entrada y salida que contienen datos de imagen en objetos [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505).

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]


> [!NOTE]
> Si especificas [**MediaMemoryTypes.GpuAndCpu**](https://msdn.microsoft.com/library/windows/apps/dn764822), el sistema usará la memoria del sistema o de la GPU, la que sea más eficiente para la canalización. Cuando uses este valor, debes comprobar el método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) para ver si el valor de [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) o [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) pasado al método contiene datos y luego procesar el fotograma según corresponda.

 

### <a name="timeindependent-property"></a>Propiedad TimeIndependent

Gracias a la propiedad [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803), el sistema puede saber si el efecto no requiere un tiempo uniforme. Si se establece en true, el sistema puede usar optimizaciones que mejoren el rendimiento del efecto.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### <a name="setproperties-method"></a>Método SetProperties

El método [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) permite que la aplicación que está usando el efecto ajuste los parámetros de efecto. Las propiedades se pasan como mapa [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) de nombres de propiedad y valores.


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


En este ejemplo sencillo, se oscurecerán los píxeles de cada fotograma de vídeo según un valor especificado. Se declara una propiedad y TryGetValue se usa para obtener el valor establecido por la aplicación que llama. Si no se ha establecido ningún valor, se usa un valor predeterminado de 0,5.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### <a name="processframe-method"></a>Método ProcessFrame

El método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) es donde el efecto modifica los datos de imagen del vídeo. El método se llama una vez por fotograma y se pasa un objeto [**ProcessVideoFrameContext**](https://msdn.microsoft.com/library/windows/apps/dn764826). Este objeto contiene un objeto de entrada [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) que contiene el fotograma entrante para ser procesado y un resultado **VideoFrame** en el que se escriben los datos de imagen que se pasarán al resto de la canalización de vídeo. Cada uno de estos objetos **VideoFrame** tiene una propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) y una propiedad [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920), pero cuál de ellos se debe usar se determina a partir del valor devuelto de la propiedad [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801).

Este ejemplo muestra una implementación sencilla del método **ProcessFrame** mediante el procesamiento de software. Para obtener más información sobre cómo trabajar con objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358), consulta [Imágenes](imaging.md). Más adelante se muestra un ejemplo de implementación de **ProcessFrame** mediante procesamiento de hardware.

El acceso al búfer de datos de **SoftwareBitmap** requiere interoperabilidad COM, por lo que debes incluir el espacio de nombres **System.Runtime.InteropServices** en el archivo de clase del efecto.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


Agrega el siguiente código dentro del espacio de nombres para que el efecto importe la interfaz para tener acceso al búfer de imagen.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]


> [!NOTE]
> Dado que esta técnica obtiene acceso a un búfer de imagen nativo sin administrar, tendrás que configurar el proyecto para permitir un código no seguro.
> 1.  En el Explorador de soluciones, haz clic con el botón derecho en el proyecto VideoEffectComponent y selecciona **Propiedades**.
> 2.  Selecciona la pestaña **Compilación**.
> 3.  Activa la casilla **Permitir código no seguro**.

 

Ahora puedes agregar la implementación del método **ProcessFrame**. Primero, este método obtiene un objeto [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) de los mapas de bits de software de entrada y salida. Ten en cuenta que el fotograma de salida se abre para la escritura y, el de entrada, para la lectura. Luego, se obtiene un valor de [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671) para cada búfer llamando a [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046). A continuación, se obtiene el búfer de datos reales convirtiendo los objetos **IMemoryBufferReference** como la interfaz de interoperabilidad definida anteriormente, **IMemoryByteAccess** y, a continuación, se llama a **GetBuffer**.

Ahora que ha se han obtenido los búferes de datos, puedes leer del búfer de entrada y escribir en el búfer de salida. El diseño del búfer se obtiene llamando a [**GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330), que proporciona información sobre el ancho, intervalo y desplazamiento inicial del búfer. Los bits por píxel se determinan a partir de las propiedades de codificación establecidas previamente con el método [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884). La información de formato de búfer se usa para buscar el índice en el búfer para cada píxel. El valor del píxel en el búfer de origen se copia en el búfer de destino con los valores de color multiplicados por la propiedad FadeValue definida para que este efecto los atenúe según la cantidad especificada.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>Implementar la interfaz IBasicVideoEffect con el procesamiento de hardware


Crear un efecto de vídeo personalizado mediante el procesamiento de hardware (GPU) es casi igual que usar el procesamiento de software como se describió anteriormente. En esta sección se muestran algunas diferencias en un efecto que usa el procesamiento de hardware. En este ejemplo se usa la API Win2D de Windows Runtime. Para obtener más información sobre el uso de Win2D, consulta la [documentación de Win2D](http://go.microsoft.com/fwlink/?LinkId=519078).

Usa los siguientes pasos para agregar el paquete de NuGet Win2D en el proyecto creado como se describe en la sección **Agregar un efecto personalizado a la aplicación** al principio de este artículo.

**Para agregar el paquete de NuGet Win2D al proyecto de efecto**

1.  En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto **VideoEffectComponent** y selecciona **Administrar paquetes NuGet**.
2.  En la parte superior de la ventana, selecciona la pestaña **Examinar**.
3.  En el cuadro de búsqueda, escribe **Win2D**.
4.  Selecciona **Win2D.uwp**y luego selecciona **Instalar** en el panel derecho.
5.  En el cuadro de diálogo **Revisar cambios** se muestra el paquete que se instalará. Haz clic en **Aceptar**.
6.  Acepta la licencia del paquete.

Además de los espacios de nombres incluidos en la configuración básica del proyecto, debes incluir los siguientes espacios de nombres proporcionados por Win2D.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


Dado que este efecto usa la memoria de GPU para el funcionamiento de los datos de imagen, debes devolver [**MediaMemoryTypes.Gpu**](https://msdn.microsoft.com/library/windows/apps/dn764822) de la propiedad [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801).

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


Establece las propiedades de codificación que admitirá el efecto con la propiedad [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799). Al trabajar con Win2D, debes usar la codificación ARGB32.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


Usa el método [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) para crear un nuevo objeto **CanvasDevice** de Win2D desde el valor de [**IDirect3DDevice**](https://msdn.microsoft.com/library/windows/apps/dn895092) pasado al método.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


La implementación de [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) es idéntica al ejemplo de procesamiento de software anterior. En este ejemplo se usa una propiedad **BlurAmount** para configurar un efecto de desenfoque de Win2D.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


El último paso es implementar el método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) que realmente procesa los datos de imagen.

Mediante las API de Win2D, se crea **CanvasBitmap** a partir de la propiedad [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) del fotograma de entrada. Se crea **CanvasRenderTarget** a partir del fotograma de salida **Direct3DSurface**, y **CanvasDrawingSession** a partir de este destino de representación. Se inicializa un nuevo efecto Win2D **GaussianBlurEffect** de Win2D mediante la propiedad **BlurAmount** que expone nuestro efecto a través de [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986). Por último, el método **CanvasDrawingSession.DrawImage** se llama para dibujar el mapa de bits de entrada en el destino de representación con el efecto de desenfoque.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## <a name="adding-your-custom-effect-to-your-app"></a>Agregar el efecto personalizado a tu aplicación


Para usar el efecto de vídeo desde la aplicación, debes agregar una referencia al proyecto de efecto a la aplicación.

1.  En el Explorador de soluciones, en el proyecto de la aplicación, haz clic con el botón derecho en **Referencias** y selecciona **Agregar referencia**.
2.  Expande la pestaña **Proyectos**, selecciona **Solución** y activa la casilla para el nombre del proyecto de efecto. Para este ejemplo, el nombre es *VideoEffectComponent*.
3.  Haz clic en **Aceptar**.

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>Agregar el efecto personalizado a una secuencia de vídeo de la cámara

Puedes configurar una secuencia simple de vista previa de la cámara siguiendo los pasos descritos en el artículo [Acceso fácil a la vista previa de cámara](simple-camera-preview-access.md). Sigue estos pasos que te proporcionarán un objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) inicializado que se usa para obtener acceso a la secuencia de vídeo de la cámara.

Para agregar el efecto de vídeo personalizado en una secuencia de cámara, primero crea un nuevo objeto [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055), pasando el espacio de nombres y el nombre de clase para el efecto. A continuación, llama al método [**AddVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn878035) del objeto **MediaCapture** para agregar el efecto a la secuencia especificada. Este ejemplo usa el valor [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) para especificar que se debe agregar el efecto a la secuencia de vista previa. Si la aplicación admite la captura de vídeo, también podrías usar **MediaStreamType.VideoRecord** para agregar el efecto a la secuencia de captura. **AddVideoEffect** devuelve un objeto [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/br240985) que representa el efecto personalizado. Puedes usar el método SetProperties para establecer la configuración del efecto.

Una vez que se haya agregado el efecto, se llama a [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) para iniciar la secuencia de vista previa.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Agregar el efecto personalizado a un clip en MediaComposition

Para obtener instrucciones generales sobre la creación de composiciones multimedia a partir de clips de vídeo, consulta [Composiciones y edición multimedia](media-compositions-and-editing.md). En el siguiente fragmento de código se muestra la creación de una composición de contenido multimedia sencilla que usa un efecto de vídeo personalizado. Un objeto [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) se crea llamando a [**CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607), pasando un archivo de vídeo que seleccionó el usuario con [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y agregando el vídeo a una nueva clase [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646). A continuación, se crea un nuevo objeto [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055) y se pasa el nombre del espacio de nombres y la clase para el efecto al constructor. Por último, se agrega la definición del efecto a la colección [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) del objeto **MediaClip**.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## <a name="related-topics"></a>Temas relacionados
* [Acceso fácil a la vista previa de cámara](simple-camera-preview-access.md)
* [Composiciones y edición multimedia](media-compositions-and-editing.md)
* [Documentación de Win2D](http://go.microsoft.com/fwlink/p/?LinkId=519078)
* [Reproducción de contenido multimedia](media-playback.md)
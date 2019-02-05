---
Description: This article describes how to create a Windows Runtime component that implements the IBasicAudioEffect interface to allow you to create custom effects for audio streams.
title: Efectos de audio personalizados
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
ms.localizationpriority: medium
ms.openlocfilehash: e04b3a764c170fa3e3e0ce1372e72b73795b5b12
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9049332"
---
# <a name="custom-audio-effects"></a>Efectos de audio personalizados

En este artículo se describe cómo crear un componente de Windows Runtime que implemente la interfaz [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) para permitir la creación de efectos personalizados para las secuencias de audio. Se pueden usar efectos personalizados con varias API de Windows Runtime, como [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), que proporciona acceso a la cámara de un dispositivo, [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), que permite crear composiciones complejas a partir de clips multimedia y [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph) que permite ensamblar rápidamente un gráfico de diversos nodos de submezcla, salida y entrada de audio.

## <a name="add-a-custom-effect-to-your-app"></a>Agregar un efecto personalizado a la aplicación


Un efecto de audio personalizado se define en una clase que implementa la interfaz [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect). Esta clase no se puede incluir directamente en el proyecto de la aplicación. En su lugar, debes usar un componente de Windows Runtime para hospedar la clase de efecto de audio.

**Agregar un componente de Windows Runtime para el efecto de audio**

1.  En Microsoft Visual Studio, con la solución abierta, ve al menú **Archivo** y selecciona **Agregar-&gt;Nuevo proyecto**.
2.  Selecciona el tipo de proyecto **Componente de Windows Runtime (Windows Universal)**.
3.  Para este ejemplo, asigna al proyecto el nombre *AudioEffectComponent*. Se hará referencia a este nombre en el código más adelante.
4.  Haz clic en **Aceptar**.
5.  La plantilla de proyecto crea una clase denominada Class1.cs. En el **Explorador de soluciones**, haz clic con el botón derecho en el icono de Class1.cs y selecciona **Cambiar nombre**.
6.  Cambia el nombre del archivo a *ExampleAudioEffect.cs*. Visual Studio mostrará un mensaje que pregunta si quieres actualizar todas las referencias con el nuevo nombre. Haz clic en **Sí**.
7.  Abre **ExampleAudioEffect.cs** y actualiza la definición de clase para implementar la interfaz [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect).


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

Debes incluir los siguientes espacios de nombres en el archivo de clase del efecto para tener acceso a todos los tipos usados en los ejemplos de este artículo.

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>Implementar la interfaz IBasicAudioEffect

El efecto de audio debe implementar todos los métodos y propiedades de la interfaz [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect). Esta sección te guía a través de una implementación sencilla de esta interfaz para crear un efecto de eco básico.

### <a name="supportedencodingproperties-property"></a>Propiedad SupportedEncodingProperties

El sistema comprueba la propiedad [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.SupportedEncodingProperties) para determinar qué propiedades de codificación son compatibles con el efecto. Ten en cuenta que, si el consumidor del efecto no puede codificar audio mediante las propiedades especificadas, el sistema llamará a [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) en el efecto y quitará el efecto de la canalización de audio. En este ejemplo, se crean objetos [**AudioEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.AudioEncodingProperties) y se agregan a la lista recuperada para admitir la codificación mono flotante de 32 bits a 44,1 kHz y 48 kHz.

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>Método SetEncodingProperties

El sistema llama a [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) en el efecto para comunicarte las propiedades de codificación de la secuencia de audio a que se aplica el efecto. Para implementar un efecto de eco, este ejemplo usa un búfer para almacenar un segundo de datos de audio. Este método permite inicializar el tamaño del búfer según el número de muestras en un segundo de audio, en función de la frecuencia de muestreo con la que se codifica el audio. El efecto de retraso también usa un contador entero para realizar un seguimiento de la posición actual en el búfer de retraso. Dado que **SetEncodingProperties** se llama siempre que el efecto se agrega a la canalización de audio, este es un buen momento para inicializar ese valor a 0. Puede que también quieras capturar el objeto **AudioEncodingProperties** que se ha pasado a este método para usarlo en otro lugar del efecto.

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>Método SetProperties

El método [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) permite que la aplicación que está usando el efecto ajuste los parámetros de efecto. Las propiedades se pasan como mapa [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) de nombres de propiedad y valores.

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

En este ejemplo sencillo se mezcla la muestra de audio actual con un valor del búfer de retraso según el valor de la propiedad **Mix**. Se declara una propiedad y TryGetValue se usa para obtener el valor establecido por la aplicación que llama. Si no se ha establecido ningún valor, se usa un valor predeterminado de 0,5. Ten en cuenta que esta propiedad es de solo lectura. El valor de propiedad debe establecerse mediante **SetProperties**.

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>Método ProcessFrame

El método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) es donde el efecto modifica los datos de audio de la secuencia. El método se llama una vez por trama y se pasa un objeto [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext). Este objeto contiene un objeto de entrada [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioFrame) que contiene la trama entrante para ser procesado y un resultado **AudioFrame** en el que se escriben los datos de audio que se pasarán al resto de la canalización de audio. Una trama de audio es un búfer de muestras de audio que equivale a un segmento breve de datos de audio.

Para obtener acceso al búfer de datos de un **AudioFrame** se requiere interoperabilidad COM, por lo que debes incluir el espacio de nombres **System.Runtime.InteropServices** en el archivo de clase del efecto y añadir el código siguiente dentro del espacio de nombres para el efecto importe la interfaz con la que tener acceso al búfer de audio.

[!code-cs[ComImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> Dado que esta técnica accede a un búfer de imagen nativo sin administrar, tendrás que configurar el proyecto para permitir un código no seguro.
> 1.  En el Explorador de soluciones, haz clic con el botón secundario en el proyecto AudioEffectComponent y selecciona **Propiedades**.
> 2.  Selecciona la pestaña **Compilación**.
> 3.  Activa la casilla **Permitir código no seguro**.

 

Ahora puedes agregar la implementación del método **ProcessFrame** a tu efecto. Primero, este método obtiene un objeto [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioBuffer) de las tramas de audio de entrada y salida. Ten en cuenta que la trama de salida se abre para la escritura y, el de entrada, para la lectura. Luego, se obtiene un valor de [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671) para cada búfer llamando a [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046). A continuación, se obtiene el búfer de datos reales convirtiendo los objetos **IMemoryBufferReference** como la interfaz de interoperabilidad definida anteriormente, **IMemoryByteAccess** y, a continuación, se llama a **GetBuffer**.

Ahora que ha se han obtenido los búferes de datos, puedes leer del búfer de entrada y escribir en el búfer de salida. Se obtiene el valor para cada muestra del búfer de entrada, y se multiplica por 1 - **Mix** para establecer el valor de señal seco del efecto. A continuación, se recupera una muestra desde la posición actual del búfer de eco y se multiplica por **Mix** para establecer el valor húmedo del efecto. La muestra de salida se establece en la suma de los valores húmedos y secos. Por último, cada muestra de entrada se almacena en el búfer de eco y se incrementa el índice de la muestra actual.

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Método Close

El sistema llamará al método [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) en la clase cuando haya que apagar el efecto. Debes usar este método para eliminar todos los recursos que hayas creado. El argumento del método es un valor [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason) que te permite saber si se cerró el efecto con normalidad, si se produjo un error o si el efecto no admite el formato de codificación necesario.

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>Método DiscardQueuedFrames

El método [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) se invoca cuando se debe restablecer el efecto. Un escenario típico se produce si el efecto almacena los fotogramas procesados anteriormente para usarlos en el procesamiento del fotograma actual. Cuando se llama a este método, se debe eliminar el conjunto de fotogramas anteriores guardados. Este método puede usarse para restablecer cualquier estado relacionado con fotogramas anteriores, no solo fotogramas de audio acumulados.

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>Propiedad TimeIndependent

Gracias a la propiedad [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803), el sistema puede saber si el efecto no requiere un tiempo uniforme. Si se establece en true, el sistema puede usar optimizaciones que mejoren el rendimiento del efecto.

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>Propiedad UseInputFrameForOutput

Establece la propiedad [**UseInputFrameForOutput**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.UseInputFrameForOutput) en **true** para indicarle al sistema que el efecto escribe su resultado en el búfer de audio de [**InputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.InputFrame) de la propiedad [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) que se ha pasado a [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) en lugar de escribirlo en [**OutputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.OutputFrame). 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>Agregar el efecto personalizado a tu aplicación


Para usar el efecto de audio desde la aplicación, debes agregar una referencia al proyecto de efecto a la aplicación.

1.  En el Explorador de soluciones, en el proyecto de la aplicación, haz clic con el botón derecho en **Referencias** y selecciona **Agregar referencia**.
2.  Expande la pestaña **Proyectos**, selecciona **Solución** y activa la casilla para el nombre del proyecto de efecto. Para este ejemplo, el nombre es *AudioEffectComponent*.
3.  Haz clic en **Aceptar**.

Si se declara que la clase de efecto de audio es un espacio de nombres diferentes, recuerda incluir ese espacio de nombres en el archivo de código.

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>Agregar el efecto personalizado a un nodo de AudioGraph
Para obtener información general sobre el uso de gráficos de audio, consulta [Gráficos de audio](audio-graphs.md). El siguiente fragmento de código muestra cómo agregar el efecto de eco de ejemplo que se muestra en este artículo a un nodo de gráfico de audio. En primer lugar, se crea un [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet) y se define un valor para la propiedad **Mix** definida por el efecto. A continuación, se llama al constructor [**AudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.AudioEffectDefinition) pasando el nombre de clase completo del tipo de efecto personalizado y el conjunto de propiedades. Por último, se agrega la definición del efecto a la propiedad [**EffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioFileInputNode.EffectDefinitions) de un [**FileInputNode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.CreateAudioFileInputNodeResult.FileInputNode) ya presente, lo que hace que el efecto personalizado procese el audio emitido. 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

Después de agregar el efecto personalizado a un nodo, se puede deshabilitar llamando a [**DisableEffectsByDefinition**](https://msdn.microsoft.com/library/windows/apps/dn958480) y pasando el objeto **AudioEffectDefinition**. Para obtener más información sobre el uso de gráficos de audio en tu aplicación, consulta [AudioGraph](audio-graphs.md).

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Agregar el efecto personalizado a un clip en MediaComposition

El siguiente fragmento de código muestra cómo agregar el efecto de audio personalizado a un clip de vídeo y una pista de audio en segundo plano en una composición multimedia. Para obtener instrucciones generales sobre la creación de composiciones multimedia a partir de clips de vídeo y sobre cómo añadir pistas de audio en segundo plano, consulta [Composiciones y edición multimedia](media-compositions-and-editing.md).

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>Artículos relacionados
* [Acceso fácil a la vista previa de cámara](simple-camera-preview-access.md)
* [Composiciones y edición multimedia](media-compositions-and-editing.md)
* [Documentación de Win2D](https://go.microsoft.com/fwlink/p/?LinkId=519078)
* [Reproducción de contenido multimedia](media-playback.md)

 




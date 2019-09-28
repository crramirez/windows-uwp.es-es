---
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: Optimizar las animaciones, multimedia e imágenes.
description: Crea aplicaciones para la Plataforma universal de Windows (UWP) con animaciones suaves, alta velocidad de fotogramas y capturas multimedia y reproducciones de alto rendimiento.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 579772bba55c93de38c3c43538ad14253dbc2572
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339896"
---
# <a name="optimize-animations-media-and-images"></a>Optimizar las animaciones, multimedia e imágenes.


Crea aplicaciones para la Plataforma universal de Windows (UWP) con animaciones suaves, alta velocidad de fotogramas y capturas multimedia y reproducciones de alto rendimiento.

## <a name="make-animations-smooth"></a>Suavizar las animaciones

Un aspecto clave de las aplicaciones para UWP son las interacciones fluidas. Esto incluye manipulaciones táctiles que "obedecen al dedo", animaciones y transiciones suaves, y movimientos cortos que proporcionan información de entrada. En el marco XAML hay un subproceso, llamado subproceso de composición, dedicado a la composición y animación de los elementos visuales de una aplicación. Como el subproceso de composición es independiente del subproceso de interfaz de usuario (que ejecuta el código del marco y del desarrollador), las aplicaciones pueden lograr una velocidad de fotogramas coherente y animaciones suaves prescindiendo de cálculos de diseño complicados u otros cálculos extensos. En esta sección se muestra cómo usar el subproceso de composición para lograr que las animaciones de una aplicación sean completamente suaves. Para más información sobre las animaciones, consulta [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview). Para más información sobre cómo aumentar la capacidad de respuesta de una aplicación mientras se realizan cálculos intensivos, consulta [Mantener la capacidad de respuesta del subproceso de la interfaz de usuario](keep-the-ui-thread-responsive.md).

### <a name="use-independent-instead-of-dependent-animations"></a>Usar animaciones independientes en lugar de dependientes

Las animaciones independientes se pueden calcular de principio a fin en el momento de la creación, porque los cambios en la propiedad que se está animando no afectan al resto de los objetos de una escena. Las animaciones independientes se pueden ejecutar, por tanto, en el subproceso de composición en lugar de en el subproceso de interfaz de usuario. Esto garantiza que sean suaves, ya que el subproceso de composición se actualiza a un ritmo uniforme.

Está garantizado que todos estos tipos de animaciones son independientes:

-   Animaciones de objetos que usan fotogramas clave
-   Animaciones cuya duración es cero
-   Animaciones de las propiedades [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) y [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top)
-   Animaciones de la propiedad [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   Animaciones de las propiedades de tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush), cuando se selecciona como destino la subpropiedad [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   Animaciones de las siguientes propiedades [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) cuando tienen como destino las subpropiedades de esos tipos de valores devueltos:

    -   [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)
    -   [**Objeto Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
    -   [**Estando**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection)
    -   [**Secuencia**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)

Las animaciones dependientes afectan al diseño y, por tanto, no se pueden calcular sin una entrada adicional desde el subproceso de interfaz de usuario. Asimismo, las animaciones dependientes incluyen modificaciones en propiedades como [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) y [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height). De manera predeterminada, las animaciones dependientes no se ejecutan y requieren que el desarrollador de la aplicación decida usarlas. Cuando se habilitan, se ejecutan de manera fluida si el subproceso de interfaz de usuario permanece desbloqueado, pero comienzan a trastabillar si el marco o la aplicación están realizando una gran cantidad de trabajo adicional en el suproceso de interfaz de usuario.

Casi todas las animaciones del marco XAML son independientes de manera predeterminada, pero puedes llevar a cabo algunas acciones para deshabilitar esta optimización. Ten cuidado si se producen estos escenarios en particular:

-   Establecer la propiedad [**EnableDependentAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.pointanimation.enabledependentanimation) para permitir que una animación dependiente se ejecute en el subproceso de interfaz de usuario. Esto convierte esas animaciones a una versión independiente. Por ejemplo, anima [**ScaleTransform.ScaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) y [**ScaleTransform.ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaley) en lugar de las propiedades [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) y [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) de un objeto. No tengas miedo de escalar objetos como imágenes y texto. El marco solo aplica el escalado bilineal mientras se está animando [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform). La imagen y el texto se volverán a rasterizar a su tamaño final para garantizar que siempre sean claros.
-   Realizar actualizaciones por fotograma, que son, de hecho, animaciones dependientes. Un ejemplo de esto es la aplicación de transformaciones en el controlador del evento [**CompositonTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering).
-   Ejecutar cualquier animación que se considere independiente en un elemento con la propiedad [**CacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.cachemode) establecida en **BitmapCache**. Esta acción se considera dependiente porque debe volver a rasterizarse la memoria caché de cada fotograma.

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>No animes los controles WebView ni MediaPlayerElement

El marco XAML no representa directamente el contenido web de un control [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView), el cual necesita que se componga trabajo adicional con el resto de la escena. Este trabajo extra se acumula al animar el control alrededor de la pantalla y puede crear problemas de sincronización (por ejemplo, es posible que el contenido HTML no se mueva en sincronía con el resto del contenido XAML de la página). Cuando tengas que animar un control **WebView**, intercámbialo por un control [**WebViewBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush) mientras dure la animación.

Tampoco es buena idea animar una clase [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement). Además de la disminución del rendimiento, puede causar la desactivación u otras anomalías en las imágenes del vídeo que se reproduzca.

> **Nota**   las recomendaciones de este artículo para **MediaPlayerElement** también se aplican a [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). **MediaPlayerElement** solo está disponible en Windows 10, versión 1607, por lo que si vas a crear una aplicación para una versión anterior de Windows, debes usar **MediaElement**.

### <a name="use-infinite-animations-sparingly"></a>Usar las aplicaciones infinitas moderadamente

La mayoría de las animaciones se ejecutan durante un tiempo especificado, pero si estableces la propiedad [**Timeline.Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) en "Forever", permitirás que la animación se ejecute indefinidamente. Te recomendamos que reduzcas al mínimo el uso de las animaciones infinitas, ya que consumen recursos de la CPU de forma constante y pueden impedir que esta entre en estado inactivo o de baja energía y, de este modo, la energía se agota con mayor rapidez.

La adición de un controlador de [**CompositionTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) es similar a realizar la ejecución de una animación infinita. Normalmente, el subproceso de interfaz de usuario solo está activo cuando tiene trabajo que realizar, pero al agregar un controlador para este evento se fuerza su ejecución en cada fotograma. Quita el controlador cuando no haya trabajo por realizar y regístralo cuando se vuelva a necesitar.

### <a name="use-the-animation-library"></a>Usar la biblioteca de animaciones

El espacio de nombres [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) incluye una biblioteca de alto rendimiento y animaciones suaves que tienen una apariencia coherente con otras animaciones de Windows. Las clases relevantes tienen "Tema" en el nombre, y se describen en [Información general sobre animaciones](https://docs.microsoft.com/windows/uwp/graphics/animations-overview) Esta biblioteca admite muchos escenarios habituales de animación, como la animación de la primera vista de la aplicación y la creación de transiciones de estado y de contenido. Te recomendamos que uses esta biblioteca de animaciones siempre que te sea posible, para mejorar el rendimiento y la coherencia de la interfaz de usuario de las aplicaciones para UWP.

> **Nota**   la biblioteca de animaciones no puede animar todas las propiedades posibles. Para escenarios XAML en los que no se aplica la biblioteca de animaciones, consulta [Animaciones con guion gráfico](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).


### <a name="animate-compositetransform3d-properties-independently"></a>Animar propiedades CompositeTransform3D de forma independiente

Puedes animar cada propiedad de clase [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.CompositeTransform3D) de forma independiente; por lo tanto, aplica solo las animaciones que necesites. Para ver ejemplos y obtener más información, consulta [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d). Para más información sobre la animación de transformaciones, consulta [Animaciones con guion gráfico](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations) y [Animaciones de fotograma clave y animaciones de función de aceleración](https://docs.microsoft.com/windows/uwp/graphics/key-frame-and-easing-function-animations).

## <a name="optimize-media-resources"></a>Optimizar los recursos multimedia

El audio, el vídeo y las imágenes son tipos de contenido atractivos que usan la mayoría de las aplicaciones. A medida que aumenta la velocidad de las capturas de multimedia y el contenido pasa de la definición estándar a una resolución alta, también aumenta la cantidad de recursos necesarios para almacenar, descodificar y reproducir este contenido. El marco XAML se basa en las últimas características incorporadas a los motores multimedia de UWP para que las aplicaciones obtengan estas mejoras de forma gratuita. Aquí explicamos algunos trucos adicionales que te permiten sacar el máximo provecho del multimedia en tu aplicación para UWP.

### <a name="release-media-streams"></a>Liberar los flujos multimedia

Los archivos multimedia son algunos de los recursos más habituales y con mayor uso de memoria que suelen usarse en las aplicaciones. Como los recursos de archivos multimedia pueden aumentar considerablemente el tamaño de la superficie de memoria de la aplicación, no debes olvidar liberar el control de los elementos multimedia cuando la aplicación deje de usarlos.

Por ejemplo, si la aplicación trabaja con un objeto [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) o [**IInputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IInputStream), asegúrate de llamar al método "Close" en el objeto cuando la aplicación haya dejado de usarlo, para liberar el objeto subyacente.

### <a name="display-full-screen-video-playback-when-possible"></a>Mostrar la reproducción de vídeo en pantalla completa siempre que sea posible

En aplicaciones para UWP, usa siempre la propiedad [**IsFullWindow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow) en [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) para habilitar y deshabilitar la representación en ventana completa. Esto garantiza que se usen optimizaciones en el nivel del sistema durante la reproducción multimedia.

El marco XAML puede optimizar la presentación del contenido de vídeo cuando es el único objeto que se representa, lo que da como resultado una experiencia que usa menos energía y produce velocidades de fotogramas más altas. Para optimizar la reproducción multimedia, establece el tamaño de un objeto **MediaPlayerElement** en el ancho y alto de la pantalla, y no muestres otros elementos XAML.

Existen motivos razonables para superponer elementos XAML sobre un objeto **MediaPlayerElement** que ocupe el ancho y alto total de la pantalla; por ejemplo, puedes superponer subtítulos o controles de transporte momentáneos. Asegúrate de ocultar estos elementos (establece `Visibility="Collapsed"`) cuando no sean necesarios para volver a situar la reproducción multimedia en su estado más eficaz.

### <a name="display-deactivation-and-conserving-power"></a>Desactivación de pantalla y ahorro de energía

Para evitar que se desactive la pantalla cuando no se detecte ninguna acción del usuario (por ejemplo, cuando una aplicación reproduce un vídeo), puedes llamar a [**DisplayRequest.RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive).

Para ahorrar energía y alargar la duración de la batería, debes llamar a [**DisplayRequest.RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) para liberar la solicitud de la pantalla cuando ya no sea necesaria.

Estas son algunas situaciones en las que debes liberar la solicitud de pantalla:

-   La reproducción de vídeo se pone en pausa, por ejemplo, por una acción del usuario, por almacenamiento en búfer o por un ajuste a causa de ancho de banda limitado.
-   La reproducción se detiene. Por ejemplo, se terminó de reproducir el vídeo o finalizó la presentación.
-   Error de reproducción. Por ejemplo, problemas de conectividad de red o un archivo dañado.

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>Colocar los demás elementos al costado del vídeo insertado

A menudo, las aplicaciones ofrecen una vista incrustada donde se reproduce vídeo dentro de una página. En este caso, pierdes claramente la optimización de la pantalla completa porque el objeto [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) no es del tamaño de la página y hay dibujados otros objetos XAML. Ten cuidado de no dibujar un borde alrededor de un objeto **MediaPlayerElement**, ya que entrarás en este modo por equivocación.

No dibujes elementos XAML sobre el vídeo cuando se encuentre en modo insertado. Si lo haces, se fuerza al marco a realizar trabajo adicional para componer la escena. Por ejemplo, para optimizar esta situación, puedes situar los controles de transporte debajo de un elemento multimedia incrustado en lugar de colocarlos sobre el vídeo. En esta imagen, la barra roja indica un conjunto de controles de transporte (Reproducir, Pausar, Detener, etc.).

![MediaPlayerElement con elementos superpuestos](images/videowithoverlay.png)

No coloques estos controles sobre elementos multimedia que no aparecen en pantalla completa. En su lugar, sitúa los controles de transporte en algún lugar fuera del área en la que se representan los elementos multimedia. En la siguiente imagen, los controles se encuentran debajo del elemento multimedia.

![MediaPlayerElement con elementos vecinos](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>Retrasar la configuración del origen de un MediaPlayerElement

Los motores multimedia son objetos que consumen muchos recursos, y el marco XAML retrasa el mayor tiempo posible la carga de las dll y la creación de objetos de gran tamaño. Se fuerza a [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) a realizar este trabajo una vez establecido su origen mediante la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source). Al establecerla cuando el usuario está realmente listo para reproducir el elemento multimedia, se retrasa tanto como es posible la mayor parte del uso de recursos asociado con **MediaPlayerElement**.

### <a name="set-mediaplayerelementpostersource"></a>Establecer MediaPlayerElement.PosterSource

Al establecer [**MediaplayerElement.PosterSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource), se permite que XAML libere algunos recursos de GPU que, de otro modo, se habrían usado. Esta API permite a una aplicación usar la menor cantidad de memoria posible.

### <a name="improve-media-scrubbing"></a>Mejorar el arrastre del cabezal de reproducción multimedia

Siempre es difícil que las plataformas multimedia logren que el arrastre del cabezal de reproducción tenga una verdadera capacidad de respuesta. Por lo general, los usuarios lo logran cambiando el valor de un control deslizante. A continuación, te ofrecemos algunos consejos para que realices esta tarea con la mayor eficacia posible:

-   Actualiza el valor de [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) en función de un temporizador que consulte la propiedad [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) en [**MediaPlayerElement.MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer). Asegúrate de usar una frecuencia de actualización razonable para el temporizador. La propiedad **Position** solo se actualiza cada 250 milisegundos durante la reproducción.
-   El tamaño de la frecuencia de los pasos de la clase Slider debe escalarse con la longitud del vídeo.
-   Suscríbete a los eventos [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) y [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) del control deslizante para establecer la propiedad [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate) en 0 cuando el usuario arrastre el control de posición del control deslizante.
-   En el controlador de eventos [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased), establece de forma manual la posición multimedia en el valor de la posición del control deslizante para ajustar el control de posición óptimo al arrastrar el cabezal de reproducción.

### <a name="match-video-resolution-to-device-resolution"></a>Igualar la resolución de vídeo con la resolución del dispositivo

La descodificación de vídeo consume mucha memoria y ciclos de GPU, por lo tanto, elige un formato de vídeo similar a la resolución en la cual se mostrará. No tiene sentido usar los recursos para descodificar un vídeo de 1080 si se va a reducir su tamaño a uno mucho menor. Muchas aplicaciones no tienen el mismo vídeo codificado en diferentes resoluciones, pero, si existe la posibilidad, usa una codificación similar a la resolución en la cual se mostrará.

### <a name="choose-recommended-formats"></a>Elegir los formatos recomendados

La selección del formato multimedia puede ser un tema delicado y, a menudo, se realiza a partir de decisiones comerciales. Desde una perspectiva de rendimiento de UWP, te recomendamos el formato H.264 como formato de vídeo principal, y AAC y MP3 como los formatos de audio preferidos. Para la reproducción de archivos locales, MP4 es el contenedor de archivos que se prefiere para el contenido de vídeo. La descodificación de H.264 se acelera a través del hardware gráfico más reciente. Asimismo, si bien la aceleración de hardware para la descodificación de VC-1 está ampliamente disponible, en un gran conjunto de hardware gráfico en el mercado, la aceleración está muchas veces limitada a un nivel de aceleración parcial (o nivel IDCT), en lugar de un nivel de carga de hardware pleno (es decir, el modo VLD).

Si tienes pleno control del proceso de generación de contenido de vídeo, debes encontrar la forma de mantener un equilibrio entre la eficacia de compresión y la estructura de GOP. Un tamaño de GOP relativamente pequeño con imágenes B puede aumentar el rendimiento en los modos de búsqueda y de avance y retroceso rápidos.

Cuando incluyas efectos de audio breves de latencia baja (por ejemplo, en juegos), usa archivos WAV con datos PCM sin comprimir para reducir la carga de procesamiento que suele producirse con los formatos comprimidos de audio.


## <a name="optimize-image-resources"></a>Optimizar los recursos de imagen

### <a name="scale-images-to-the-appropriate-size"></a>Escalar imágenes al tamaño adecuado

Las imágenes se capturan en resoluciones muy altas, lo que puede provocar un mayor uso de la CPU por parte de las aplicaciones cuando descodifican los datos de imagen y de más memoria tras su descarga del disco. Pero no tiene sentido descodificar y guardar una imagen de alta resolución en la memoria si se va a mostrar únicamente en un tamaño menor que su tamaño nativo. En su lugar, crea una versión de la imagen que tenga el tamaño exacto que se mostrará en la pantalla mediante las propiedades [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) y [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

No realices lo siguiente:

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

En su lugar, haz esto:

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

De manera predeterminada, las unidades [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) y [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) son píxeles físicos. Puedes usar la propiedad [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) para cambiar este comportamiento: al establecer **DecodePixelType** en **Logical** provocarás que se descodifique automáticamente el tamaño, teniendo en cuenta el factor de escala actual del sistema, el cual es similar a otro contenido XAML. Por lo tanto, sería conveniente establecer de forma general **DecodePixelType** en **Logical** si quieres que, por ejemplo, **DecodePixelWidth** y **DecodePixelHeight** coincidan con las propiedades de alto y ancho del control de imagen en el cual se mostrará esta imagen. Con el comportamiento predeterminado de uso de píxeles físicos, debes tener en cuenta el factor de escala actual del sistema y debes considerar las notificaciones de cambios de escala en caso de que el usuario cambie sus preferencias de visualización.

Si estableces DecodePixelWidth/Height para que sea explícitamente mayor que la imagen que se mostrará en pantalla, la aplicación usará innecesariamente más memoria (hasta 4 bytes por píxel), lo cual supondrá tener que usar más recursos para imágenes de gran tamaño. Igualmente, la imagen se reducirá usando el escalado bilineal, lo que podría provocar que, en factores de escala grandes, se vea borrosa.

Si DecodePixelWidth/DecodePixelHeight se establece explícitamente más pequeño que la imagen que se mostrará en pantalla, entonces se aumentará y podría verse pixelada.

En algunos casos donde no se puede determinar con anterioridad el tamaño de descodificación, deberías dejarlo en manos de la descodificación correcta de tamaño automática de XAML, que hará un mejor intento de descodificación de la imagen en el tamaño adecuado si no se especifica un DecodePixelWidth/DecodePixelHeight explícito.

Te recomendamos que establezcas un tamaño de descodificación explícito si conoces con anterioridad el tamaño del contenido de la imagen. Además, también deberías establecer [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) en **Logical** si el tamaño de descodificación proporcionado está relacionado con otros tamaños de los elementos XAML. Por ejemplo, si estableces explícitamente el tamaño del contenido con Image.Width e Image.Height, puedes establecer DecodePixelType en DecodePixelType.Logical para usar las mismas dimensiones de píxeles lógicos como un control de imagen y luego explícitamente usar BitmapImage.DecodePixelWidth o BitmapImage.DecodePixelHeight para controlar el tamaño de la imagen con el fin de lograr un ahorro de cantidades potencialmente grandes de memoria.

Recuerda que hay que tener en cuenta Image.Stretch al determinar el tamaño del contenido descodificado.

### <a name="right-sized-decoding"></a>Descodificación de tamaño adecuado

En caso de que no establezcas un tamaño de descodificación explícito, el lenguaje XAML intentará ahorrar memoria al descodificar una imagen en el tamaño exacto que tendrá en pantalla, según el diseño inicial de la página que la contiene. Te sugerimos que escribas la aplicación de forma que use esta característica cuando sea posible. Esta característica se deshabilitará si se cumple cualquiera de las siguientes condiciones.

-   La clase [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) está conectada al árbol XAML activo después de configurar el contenido con [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) o con [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource).
-   Se descodifica la imagen mediante la descodificación sincrónica, como sucede con [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource).
-   La imagen se oculta al establecer la propiedad [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) en 0 o [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) en **Collapsed** en el elemento de imagen host, en el pincel o en cualquier elemento primario.
-   El control o el pincel de la imagen usan una enumeración [**Stretch**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) establecida en **None**.
-   La imagen se usa como una propiedad [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid).
-   Se establece `CacheMode="BitmapCache"` en el elemento de imagen o en cualquier elemento primario.
-   El pincel de la imagen no es rectangular (como cuando se aplica a una forma o a un texto).

En los escenarios anteriores, la única forma de conseguir ahorrar memoria es establecer un tamaño de descodificación explícito.

Te recomendamos que asocies siempre una clase [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) al árbol activo antes de establecer el origen. Esto sucederá siempre que especifiques un elemento o pincel de imagen en el marcado. A continuación, se proporcionan ejemplos bajo el título "Ejemplos de árbol activo". Evita usar [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) y, en su lugar, usa [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) cuando establezcas un origen de la secuencia. También es una buena idea evitar ocultar el contenido de imagen (ya sea con opacidad cero o con visibilidad contraída) mientras esperas a que se genere el evento [**ImageOpened**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened). Esta es una decisión importante: no podrás aprovecharte de la descodificación automática al tamaño correcto si haces esto. Si la aplicación debe ocultar el contenido de imagen en un principio, entonces procura que establezca el tamaño de descodificación explícitamente si es posible.

**Ejemplos de árbol dinámico**

Ejemplo 1 (bueno): Se especifica el identificador uniforme de recursos (URI) en el marcado.

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Ejemplo 2; marcado: se especifica el URI en el código subyacente.

```xaml
<Image x:Name="myImage"/>
```

Ejemplo 2; código subyacente (bueno): conectar el elemento BitmapImage al árbol antes de establecer su UriSource.

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Ejemplo 2 de código subyacente (malo): establecer UriSource de BitmapImage antes de conectarlo al árbol.

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>Optimizaciones del almacenamiento en caché

Las optimizaciones del almacenamiento en caché están activas para las imágenes que usan la propiedad [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) para cargar el contenido de un paquete de la aplicación o de la web. El URI se usa para identificar unívocamente el contenido subyacente e, internamente, el marco XAML no descargará o descodificará el contenido varias veces. En su lugar, usará los recursos almacenados en caché de software o hardware para mostrar el contenido varias veces.

La excepción a esta optimización es si la imagen se muestra varias veces en diferentes resoluciones (que se pueden especificar explícitamente o a través de la descodificación de tamaño adecuado automática). Cada entrada de caché también almacena la resolución de la imagen y, si el XAML no puede encontrar una imagen con un origen de URI que coincida con la resolución necesaria, entonces descodificará una nueva versión a ese tamaño. No obstante, no volverá a descargar los datos de la imagen codificada.

En consecuencia, deberías usar [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) cuando se carguen imágenes desde un paquete de la aplicación, y evitar el uso de una secuencia de archivos y de [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) cuando no sea necesario.

### <a name="images-in-virtualized-panels-listview-for-instance"></a>Imágenes en paneles virtualizados (ListView, por ejemplo)

Si una imagen se quita del árbol (bien porque la aplicación la quitó explícitamente o bien porque está en un panel virtualizado moderno y se quitó implícitamente cuando quedó fuera de la vista), el lenguaje XAML optimizará el uso de la memoria liberando los recursos de hardware de la imagen, puesto que ya no son necesarios. La memoria no se libera inmediatamente, sino que se libera durante la actualización de fotogramas que se produce un segundo después de que el elemento de imagen no esté en el árbol.

Por lo tanto, deberías tratar de usar paneles virtualizados modernos para hospedar listas de contenido de imagen.

### <a name="software-rasterized-images"></a>Imágenes de rasterización de software

Cuando usas una imagen para un pincel no rectangular o para una propiedad [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid), la imagen usará una ruta de acceso de rasterización de software, que en ningún momento escalará las imágenes. Además, debe almacenar una copia de la imagen tanto en la memoria de software como en la de hardware. Por ejemplo, si se usa una imagen como el pincel de una elipse, entonces la imagen potencialmente grande y al completo, se almacenará internamente dos veces. Al usar **NineGrid** o un pincel no rectangular, la aplicación deberá escalar previamente sus imágenes para alcanzar, aproximadamente, el tamaño en el que se presentarán.

### <a name="background-thread-image-loading"></a>Carga de imágenes de subproceso en segundo plano

El XAML tiene una optimización interna que le permite descodificar el contenido de una imagen de forma asincrónica en una superficie de memoria de hardware sin la necesidad de una superficie en la memoria de software intermedia. Esto reduce el uso máximo de memoria y la latencia de representación. Esta característica se deshabilitará si se cumple cualquiera de las siguientes condiciones.

-   La imagen se usa como una propiedad [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid).
-   Se establece `CacheMode="BitmapCache"` en el elemento de imagen o en cualquier elemento primario.
-   El pincel de la imagen no es rectangular (como cuando se aplica a una forma o a un texto).

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

La clase [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) intercambia imágenes sin comprimir interoperables entre distintos espacios de nombres de WinRT como [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder), API de cámara y XAML. Esta clase omite una copia adicional que normalmente sería necesaria con [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), y que es útil para reducir la latencia de origen a la pantalla y el uso máximo de la memoria.

También puedes configurar la clase [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que suministra información de origen, para usar una interfaz [**IWICBitmap**](https://docs.microsoft.com/windows/desktop/api/wincodec/nn-wincodec-iwicbitmap) personalizada y así poder proporcionar un almacén de respaldo recargable que permita a la aplicación volver a asignar la memoria como considere apropiado. Este es un caso de uso avanzado de C++.

La aplicación debería usar [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) y [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) para interoperar con otras API de WinRT que produzcan y consuman imágenes. Asimismo, también debería usar la clase **SoftwareBitmapSource** al cargar los datos de la imagen sin comprimir, en lugar de usar [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap).

### <a name="use-getthumbnailasync-for-thumbnails"></a>Usar GetThumbnailAsync para las miniaturas

Un caso de uso de escalado de imágenes es la creación de miniaturas. Si bien puedes usar [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) y [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) para proporcionar versiones pequeñas de las imágenes, la plataforma universal de Windows (UWP) ofrece API aún más eficaces para la recuperación de miniaturas. [**GetThumbnailAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getthumbnailasync) proporciona las miniaturas de las imágenes que tienen el sistema de archivos ya almacenado en la memoria caché. Esto proporciona un rendimiento incluso mejor que las API de XAML porque no es necesario abrir o descodificar la imagen.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>Descodificar las imágenes una sola vez

Para evitar que las imágenes se descodifiquen más de una vez, asigna la propiedad [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) desde un URI en lugar de usar secuencias de memoria. El marco XAML puede asociar el mismo URI en varios lugares con una imagen descodificada, pero no puede hacer ese mismo trabajo para distintas secuencias de memoria que contengan los mismos datos; por ello, crea una imagen descodificada diferente para cada secuencia de memoria.

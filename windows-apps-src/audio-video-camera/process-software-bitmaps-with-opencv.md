---
author: laurenhughes
ms.assetid: ''
description: En este artículo se explica cómo usar la clase SoftwareBitmap con la Open Source Computer Vision Library (OpenCV).
title: Procesar mapas de bits con OpenCV
ms.author: lahugh
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: b9f1f2050590267d0a98779eba11bbe0b363da0c
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7560779"
---
# <a name="process-bitmaps-with-opencv"></a>Procesar mapas de bits con OpenCV

En este artículo se explica cómo usar la clase **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)**, que se usa en muchas de las diferentes API de UWP para representar imágenes con la Open Source Computer Vision Library (OpenCV), un código abierto, biblioteca de código nativo que proporciona una amplia variedad de algoritmos de procesamiento de imágenes. 

Los ejemplos de este artículo te guían para crear un componente de Windows Runtime de código nativo que puede usarse desde una aplicación para UWP, incluidas las aplicaciones creadas con C#. Este componente auxiliar expondrá un único método, **Desenfoque**, que usará la función de procesamiento de imagen de desenfoque de OpenCV. El componente implementa métodos privados que obtienen un puntero al búfer de datos de imagen subyacente que se puede utilizar directamente en la biblioteca de OpenCV, lo que facilita ampliar el componente auxiliar para implementar otras características de procesamiento de OpenCV. 

* Para obtener una introducción sobre el uso de **SoftwareBitmap**, consulta [Crear, editar y guardar imágenes de mapa de bits](imaging.md). 
* Para obtener información sobre cómo usar la biblioteca OpenCV, ve a [http://opencv.org](http://opencv.org).
* Para ver cómo usar el componente auxiliar OpenCV que se muestra en este artículo con **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** para implementar el procesamiento de imágenes en tiempo real de fotogramas de una cámara, consulta [Use OpenCV with MediaFrameReader](use-opencv-with-mediaframereader.md).
* Para ver un ejemplo de código completo que implementa algunos efectos diferentes, consulta [Fotogramas de Cámara + Muestra de OpenCV](https://go.microsoft.com/fwlink/?linkid=854003) en el repositorio de GitHub de muestras universales de Windows.

> [!NOTE] 
> La técnica usada por el componente OpenCVHelper, que se describe con detalle en este artículo, requiere que los datos de imagen que se procesarán residen en la memoria de CPU, no en la memoria de GPU. Por tanto, en el caso de las API que te permiten solicitar la ubicación de memoria de imágenes, como la clase **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)**, debes especificar la memoria de CPU.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Crear un componente auxiliar de Windows Runtime para la interoperabilidad de OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Agrega un nuevo proyecto Componente de Windows Runtime de código nativo a la solución

1. Agrega un nuevo proyecto a la solución en Visual Studio haciendo clic con el botón derecho en la solución en Explorador de soluciones y seleccionando **Agregar->Nuevo proyecto**. 
2. En la categoría **Visual C++**, selecciona **Componente de Windows Runtime (Windows universal)**. Para este ejemplo, asigna al proyecto el nombre de "OpenCVBridge" y haz clic en **Aceptar**. 
3. En el cuadro de diálogo **New Windows Universal Project**, selecciona el destino y la versión mínima del SO de la aplicación y haz clic en **Aceptar**.
4. Haz clic con el botón derecho en el archivo generado automáticamente Class1.cpp en el Explorador de soluciones y selecciona **Quitar**, cuando aparezca el cuadro de diálogo de confirmación, elige **Eliminar**. Luego eliminar el archivo de encabezado Class1.h.
5. Haz clic con el botón derecho en el icono del proyecto OpenCVBridge y selecciona **Agregar->Clase...**. En el cuadro de diálogo **Agregar clase**, escribe "OpenCVHelper" en el campo **Nombre de clase** y luego haz clic en **Aceptar**. El código se agregará a los archivos de clase creados en un paso posterior.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. Agrega los paquetes de NuGet OpenCV al proyecto de componente

1. Haz clic con el botón derecho en el icono de proyecto OpenCVBridge en el Explorador de soluciones y selecciona **Administrar paquetes de NuGet...**
2. Cuando aparezca el cuadro de diálogo Administrador de paquetes de NuGet, selecciona la pestaña **Examinar** y escribe "OpenCV.Win" en el cuadro de búsqueda.
3. Selecciona "OpenCV.Win.Core" y haz clic en **Instalar**. En el cuadro de diálogo **Vista previa**, haz clic en **Aceptar**.
4. Usa el mismo procedimiento para instalar el paquete "OpenCV.Win.ImgProc".

> [!NOTE]
> OpenCV.Win.Core y OpenCV.Win.ImgProc no se actualizan periódicamente, pero siguen siendo recomendables para crear un valor de OpenCVHelper tal como se describe en esta página.

### <a name="3-implement-the-opencvhelper-class"></a>3. Implementa la clase OpenCVHelper

Pega el siguiente código en el archivo de encabezado OpenCVHelper.h. Este código incluye archivos de encabezado OpenCV para los paquetes *Core* e *ImgProc* que instalamos e indica tres métodos que se mostrarán en los siguientes pasos.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

Elimina el contenido existente del archivo OpenCVHelper.cpp y luego agrega las siguientes directivas include. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

Después de agregar las directivas include, agrega las siguientes directivas **using**. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

Después agrega el método **GetPointerToPixelData** a OpenCVHelper.cpp. Este método toma un **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** y, a través de una serie de conversiones, obtiene una representación de interfaces COM de los datos de píxeles mediante los cuales podemos obtener un puntero al búfer de datos subyacente como una matriz **char**. 

En primer lugar, se obtiene un **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)** que incluye los datos de píxeles llamando a **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** y solicitando una búfer de escritura/lectura para que la biblioteca de OpenCV pueda modificar los datos de píxeles.  Se llama a **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** para obtener un objeto **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)**. Después, la interfaz **IMemoryBufferByteAccess** se convierte como una **IInspectable**, la interfaz base de todas las clases de Windows Runtime, y se llama a **[QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521(v=vs.85).aspx)** para obtener una interfaz COM **[IMemoryBufferByteAccess](https://msdn.microsoft.com/library/mt297505(v=vs.85).aspx)** que nos permitirá obtener el búfer de datos de píxeles como una matriz **char**. Por último, rellena la matriz **char** llamando a **[IMemoryBufferByteAccess::GetBuffer](https://msdn.microsoft.com/library/mt297506(v=vs.85).aspx)**. Si alguno de los pasos de conversión de este método no funciona, el método devuelve **false**, lo que indica que no se puede continuar con el procesamiento.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

Luego agrega el método **TryConvert** que se muestra a continuación. Este método toma un **SoftwareBitmap** e intenta convertirlo en un objeto **Mat**, que es el objeto matriz que OpenCV usa para representar los búferes de datos de imágenes. Este método llama al método **GetPointerToPixelData** definido anteriormente para obtener una representación de matriz **char** del búfer de datos de píxeles. Si se realiza correctamente, se llama al constructor de la clase **Mat**, pasando el ancho y el alto de los píxeles obtenidos a partir del objeto de origen **SoftwareBitmap**. 

> [!NOTE] 
> En este ejemplo se especifica la constante CV_8UC4 como el formato de píxeles para el objeto creado **Mat**. Esto significa que el **SoftwareBitmap** que se pasó a este método debe tener un valor de propiedad **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** de **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** con un alfa multiplicado previamente, el equivalente de CV_8UC4, para que funcione con este ejemplo.

Una copia superficial del objeto creado **Mat** se devuelve desde el método para que el procesamiento funcione en el mismo búfer de datos de píxeles de datos al que hace referencia el **SoftwareBitmap** y no una copia de este búfer.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

Por último, este ejemplo de clase auxiliar implementa un único método de procesamiento de imágenes, **Desenfoque**, que simplemente usa el método **TryConvert** definido anteriormente para recuperar un objeto **Mat** que representa el mapa de bits de origen y el mapa de bits de destino para la operación de desenfoque y, luego, llama al método **Desenfoque** desde la biblioteca de OpenCV ImgProc. El otro parámetro de **Desenfoque** especifica el tamaño del efecto de desenfoque en las direcciones X e Y.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Un ejemplo sencillo de SoftwareBitmap OpenCV mediante el componente auxiliar
Ahora que el componente OpenCVBridge se ha creado, podemos crear una aplicación sencilla C# que usa el método **desenfoque** de OpenCV para modificar un **SoftwareBitmap**. Para tener acceso al Componente de Windows Runtime desde la aplicación para UWP, primero debes agregar una referencia al componente. En el Explorador de soluciones, haz clic con el botón derecho en el nodo **Referencias** en el proyecto de aplicación para UWP y selecciona **Agregar referencia...**. En el cuadro de diálogo Administrador de referencias, selecciona **Proyectos->Solución**. Activa la casilla situada junto al proyecto OpenCVBridge y haz clic en **Aceptar**.

El siguiente código de ejemplo permite al usuario seleccionar un archivo de imagen y luego usa **[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)** para crear una representación **SoftwareBitmap** de la imagen. Para obtener más información sobre cómo trabajar con **SoftwareBitmap**, consulta [Crear, editar y guardar imágenes de mapa de bits](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging).

Como se explicó anteriormente en este artículo, la clase **OpenCVHelper** requiere que todas las imágenes **SoftwareBitmap** proporcionadas deben codificarse con el formato de píxel de BGRA8 con valores de alfa multiplicados de antemano, de modo que si la imagen ya no está en este formato, el código de ejemplo llama a **[Convert](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** para convertir la imagen en el formato esperado.

Luego se crea un **SoftwareBitmap** para usarse como el destino de la operación de desenfoque. Las propiedades de imagen de entrada se usan como argumentos para que el constructor cree un mapa de bits con el formato que coincida.

Una nueva instancia de **OpenCVHelper** se crea y se llama al método **Desenfoque**, pasando a los mapas de bits de origen y destino. Por último, se crea un **SoftwareBitmapSource** para asignar la imagen de salida a un control de **Imagen** XAML.


[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>Temas relacionados

* [Referencia de opciones de BitmapEncoder](bitmapencoder-options-reference.md)
* [Metadatos de imagen](image-metadata.md)
 

 





---
ms.assetid: ''
description: En este artículo se explica cómo usar la clase SoftwareBitmap con la biblioteca de Computer Vision de código abierto (OpenCV).
title: Procesar mapas de bits con OpenCV
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, OpenCV, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: a917c4efc8da8fbdabbdc753aacf23724ae17055
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363808"
---
# <a name="process-bitmaps-with-opencv"></a>Procesar mapas de bits con OpenCV

En este artículo se explica cómo usar la clase **[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** , que son usadas por muchas api de Windows Runtime diferentes para representar imágenes, con la biblioteca de Computer Vision de código abierto (OpenCV), una biblioteca de código abierto y una biblioteca de código nativo que proporciona una amplia variedad de algoritmos de procesamiento de imágenes. 

Los ejemplos de este artículo le guiarán a través de la creación de un código nativo Windows Runtime componente que se puede usar desde una aplicación de UWP, incluidas las aplicaciones que se crean con C#. Este componente auxiliar expondrá un método único, **Blur**, que usará la función de procesamiento de imágenes de desenfoque de OpenCV. El componente implementa métodos privados que obtienen un puntero al búfer de datos de imagen subyacente que puede usar directamente la biblioteca OpenCV, lo que simplifica la extensión del componente auxiliar para implementar otras características de procesamiento de OpenCV. 

* Para obtener una introducción al uso de **SoftwareBitmap**, vea [crear, editar y guardar imágenes de mapa de bits](imaging.md). 
* Para obtener información sobre cómo usar la biblioteca de OpenCV, vaya a [https://opencv.org](https://opencv.org) .
* Para ver cómo usar el componente auxiliar OpenCV que se muestra en este artículo con **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** para implementar el procesamiento de imágenes en tiempo real de fotogramas de una cámara, consulte [uso de OpenCV con MediaFrameReader](use-opencv-with-mediaframereader.md).
* Para ver un ejemplo de código completo que implementa algunos efectos diferentes, consulte el [ejemplo de fotogramas de cámara + OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) en el repositorio de github de ejemplos de Windows universal.

> [!NOTE] 
> La técnica usada por el componente OpenCVHelper, que se describe en detalle en este artículo, requiere que los datos de la imagen que se van a procesar residan en la memoria de la CPU, no en la memoria GPU. Por lo tanto, para las API que le permiten solicitar la ubicación de la memoria de las imágenes, como la clase **[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** , debe especificar la memoria de la CPU.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Crear un componente de Windows Runtime auxiliar para la interoperabilidad de OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. agregar un nuevo código nativo Windows Runtime proyecto de componente a la solución

1. Agregue un nuevo proyecto a la solución en Visual Studio; para ello, haga clic con el botón derecho en la solución en Explorador de soluciones y seleccione **Agregar->nuevo proyecto**. 
2. En la categoría **Visual C++** , seleccione **componente de Windows Runtime (Windows universal)**. En este ejemplo, asigne al proyecto el nombre "OpenCVBridge" y haga clic en **Aceptar**. 
3. En el cuadro de diálogo **nuevo proyecto de Windows universal** , seleccione el destino y la versión mínima del sistema operativo de la aplicación y haga clic en **Aceptar**.
4. Haga clic con el botón secundario en el archivo autogerenated Class1. cpp en Explorador de soluciones y seleccione **quitar**cuando aparezca el cuadro de diálogo de confirmación, elija **eliminar**. A continuación, elimine el archivo de encabezado Class1. h.
5. Haga clic con el botón derecho en el icono del proyecto OpenCVBridge y seleccione **agregar >clase..**.. En el cuadro de diálogo **Agregar clase** , escriba "OpenCVHelper" en el campo **nombre de clase** y, a continuación, haga clic en **Aceptar**. El código se agregará a los archivos de clase creados en un paso posterior.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. agregar los paquetes NuGet de OpenCV al proyecto de componente

1. Haga clic con el botón derecho en el icono del proyecto OpenCVBridge en Explorador de soluciones y seleccione **administrar paquetes NuGet..** .
2. Cuando se abra el cuadro de diálogo Administrador de paquetes NuGet, seleccione la pestaña **examinar** y escriba "OpenCV. Win" en el cuadro de búsqueda.
3. Seleccione "OpenCV. win. Core" y haga clic en **instalar**. En el cuadro de diálogo **vista previa** , haga clic en **Aceptar**.
4. Use el mismo procedimiento para instalar el paquete "OpenCV. win. ImgProc".

>[!NOTE]
>OpenCV. win. Core y OpenCV. win. ImgProc no se actualizan con regularidad y no superan las comprobaciones de cumplimiento del almacén, por lo que estos paquetes solo están diseñados para la experimentación.

### <a name="3-implement-the-opencvhelper-class"></a>3. implementar la clase OpenCVHelper

Pegue el código siguiente en el archivo de encabezado OpenCVHelper. h. Este código incluye archivos de encabezado de OpenCV para los paquetes *Core* y *ImgProc* que se han instalado y declara tres métodos que se mostrarán en los pasos siguientes.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h" id="SnippetOpenCVHelperHeader":::

Elimine el contenido existente del archivo OpenCVHelper. cpp y, a continuación, agregue las siguientes directivas Include. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperInclude":::

Después de las directivas Include, agregue las siguientes directivas **using** . 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperUsing":::

A continuación, agregue el método **GetPointerToPixelData** a OpenCVHelper. cpp. Este método toma un **[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** y, a través de una serie de conversiones, obtiene una representación de interfaz com de los datos de píxeles a través de la cual se puede obtener un puntero al búfer de datos subyacente como una matriz de **caracteres** . 

En primer lugar, se obtiene un **[BitmapBuffer](/uwp/api/windows.graphics.imaging.bitmapbuffer)** que contiene los datos de píxeles llamando a **[LockBuffer](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)**, solicitando un búfer de lectura/escritura para que la biblioteca OpenCV pueda modificar los datos de los píxeles.  Se llama a **[CreateReference](/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** para obtener un objeto **[IMemoryBufferReference](/uwp/api/windows.foundation.imemorybufferreference)** . A continuación, la interfaz **IMemoryBufferByteAccess** se convierte en un **IInspectable**, la interfaz base de todas las clases de Windows Runtime y se llama a **[QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** para obtener una interfaz com **[IMemoryBufferByteAccess](/previous-versions/mt297505(v=vs.85))** que nos permita obtener el búfer de datos de píxeles como una matriz de **caracteres** . Por último, rellene la matriz de **caracteres** mediante una llamada a **[IMemoryBufferByteAccess:: getBuffer](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)**. Si se produce un error en cualquiera de los pasos de conversión de este método, el método devuelve **false**, lo que indica que no puede continuar el procesamiento.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperGetPointerToPixelData":::

A continuación, agregue el método **TryConvert** que se muestra a continuación. Este método toma un objeto **SoftwareBitmap** e intenta convertirlo en un objeto **paspartú** , que es el objeto de matriz que OpenCV usa para representar los búferes de datos de la imagen. Este método llama al método **GetPointerToPixelData** definido anteriormente para obtener una representación de matriz de **caracteres** del búfer de datos de píxeles. Si esto se realiza correctamente, se llama al constructor de la clase **Mat** , pasando el ancho y el alto de píxel obtenidos del objeto **SoftwareBitmap** de origen. 

> [!NOTE] 
> En este ejemplo se especifica la constante CV_8UC4 como formato de píxel para el objeto **paspartú** creado. Esto significa que el **SoftwareBitmap** que se pasa a este método debe tener un valor de propiedad **[BitmapPixelFormat](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** de  **[BGRA8](/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** con alfa premultiplicado, el equivalente de CV_8UC4, para que funcione con este ejemplo.

El método devuelve una copia superficial del objeto **paspartú** creado para que el procesamiento posterior funcione en el mismo búfer de datos de píxeles de datos al que hace referencia el objeto **SoftwareBitmap** y no una copia de este búfer.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperTryConvert":::

Por último, esta clase auxiliar de ejemplo implementa un método de procesamiento de imagen único, **Blur**, que simplemente usa el método **TryConvert** definido anteriormente para recuperar un objeto de **paspartú** que representa el mapa de bits de origen y el mapa de bits de destino para la operación de desenfoque y, a continuación, llama al método **Blur** desde la biblioteca ImgProc de OpenCV. El otro parámetro para **desenfocar** especifica el tamaño del efecto de desenfoque en las direcciones X e y.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperBlur":::


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Un ejemplo de OpenCV de SoftwareBitmap sencillo con el componente auxiliar
Ahora que se ha creado el componente OpenCVBridge, podemos crear una aplicación sencilla de C# que use el método OpenCV **Blur** para modificar un elemento **SoftwareBitmap**. Para acceder al componente de Windows Runtime desde la aplicación de UWP, primero debe agregar una referencia al componente. En Explorador de soluciones, haga clic con el botón secundario en el nodo **referencias** en el proyecto de aplicación de UWP y seleccione **Agregar referencia..**.. En el cuadro de diálogo Administrador de referencias, seleccione **proyectos->solución**. Active la casilla situada junto al proyecto OpenCVBridge y haga clic en **Aceptar**.

El código de ejemplo siguiente permite al usuario seleccionar un archivo de imagen y, a continuación, usa **[BitmapDecoder](/uwp/api/windows.graphics.imaging.bitmapencoder)** para crear una representación de **SoftwareBitmap** de la imagen. Para obtener más información sobre cómo trabajar con **SoftwareBitmap**, vea [crear, editar y guardar imágenes de mapa de bits](./imaging.md).

Como se explicó anteriormente en este artículo, la clase **OpenCVHelper** requiere que todas las imágenes **SoftwareBitmap** proporcionadas se codifiquen con el formato de píxel BGRA8 con valores alfa premultiplicados, por lo que si la imagen no está ya en este formato, el código de ejemplo llama a **[Convert](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** para convertir la imagen al formato esperado.

A continuación, se crea un **SoftwareBitmap** para usarlo como destino de la operación de desenfoque. Las propiedades de la imagen de entrada se utilizan como argumentos en el constructor para crear un mapa de bits con formato coincidente.

Se crea una nueva instancia de **OpenCVHelper** y se llama al método **Blur** , pasando los mapas de bits de origen y de destino. Por último, se crea un **SoftwareBitmapSource** para asignar la imagen de salida a un control de **imagen** XAML.

Este código de ejemplo usa las API de los siguientes espacios de nombres, además de los espacios de nombres incluidos en la plantilla de proyecto predeterminada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVMainPageUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVBlur":::

## <a name="related-topics"></a>Temas relacionados

* [Referencia de opciones de BitmapEncoder](bitmapencoder-options-reference.md)
* [Metadatos de imagen](image-metadata.md)
 

 

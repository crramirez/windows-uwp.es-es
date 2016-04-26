---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: En este artículo se describe cómo mostrar rápidamente la secuencia de vista previa de la cámara en una página XAML en una aplicación para la Plataforma universal de Windows (UWP).
title: Acceso fácil a la vista previa de cámara
---

# Acceso fácil a la vista previa de cámara

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este artículo se describe cómo mostrar rápidamente la secuencia de vista previa de la cámara en una página XAML en una aplicación para la Plataforma universal de Windows (UWP). La creación de una aplicación que capture fotos y vídeos con la cámara requiere que realices tareas, como controlar la orientación del dispositivo y de la cámara o establecer opciones de codificación para el archivo capturado. En algunos escenarios de la aplicación, es posible que solo quieras mostrar la secuencia de vista previa de la cámara sin preocuparte por estas otras consideraciones. En este artículo se muestra cómo hacerlo con la cantidad de código mínima. Ten en cuenta que siempre debes apagar la secuencia de vista previa correctamente cuando termines mediante el procedimiento siguiente.

Para obtener información sobre cómo escribir una aplicación de cámara que capture fotos o vídeos, consulta [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Agregar declaraciones de funcionalidades al manifiesto de la aplicación

Para que tu aplicación tenga acceso a la cámara de un dispositivo, debes declarar que esta usa las funcionalidades de *webcam* y *microphone* del dispositivo. Si quieres guardar las fotos y vídeos capturados en la biblioteca de imágenes de los usuarios, también debes declarar las funcionalidades *picturesLibrary* y *videosLibrary*.

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona las casillas **Cámara web** y **Micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y el cuadro de **Biblioteca vídeos**.

## Agregar un CaptureElement a la página

Usa un [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) para visualizar el flujo de vista previa en la página XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## Usar MediaCapture para iniciar el flujo de vista previa

El objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) es la interfaz de la aplicación para la cámara del dispositivo. Esta clase es un miembro del espacio de nombres Windows.Media.Capture. En el ejemplo de este artículo también se usan las API de los espacios de nombres [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) y [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx), además de las que se incluyen con la plantilla de proyecto predeterminada.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

Agrega mediante directivas para incluir los siguientes espacios de nombres en el archivo .cs de tu página.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Declara una variable de clase para el objeto **MediaCapture**.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Crea una nueva instancia de la clase **MediaCapture** y llama a [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) para inicializar el dispositivo de captura. Este método puede presentar errores en los dispositivos que no tienen una cámara, por ejemplo, por lo que debe llamarlo desde un bloque **probar**. Se iniciará una **UnauthorizedAccessException** si intenta inicializar la cámara y el usuario deshabilitó el acceso a la cámara en la configuración de privacidad del dispositivo. También verá esta excepción durante el desarrollo si ignoró agregar las funcionalidades adecuadas al manifiesto de la aplicación.

**Importante** En algunas familias de dispositivos, se muestra al usuario una petición de consentimiento del usuario antes de que se conceda acceso a la aplicación para usar la cámara del dispositivo. Por este motivo, solo debes llamar a [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) desde la conversación principal de la interfaz de usuario. Intentar inicializar la cámara desde otra conversación puede producir errores de inicialización.

Conecta el **MediaCapture** al **CaptureElement** estableciendo la propiedad [**Origen**](https://msdn.microsoft.com/library/windows/apps/br209280). Por último, llama a [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) para iniciar la vista previa.

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## Apagar el flujo de vista previa

Cuando termines de usar la secuencia de vista previa, debes apagar siempre la secuencia y deshacerte correctamente de los recursos asociados para garantizar la disponibilidad de la cámara para otras aplicaciones en el dispositivo. Los pasos necesarios para cerrar la secuencia de vista previa son:

-   Llama a [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) para detener el flujo de vista previa.
-   Establece la propiedad [**Origen**](https://msdn.microsoft.com/library/windows/apps/br209280) de **CaptureElement** en null.
-   Llama al método [**Desechar**](https://msdn.microsoft.com/library/windows/apps/dn278858) del objeto **MediaCapture** para liberar el objeto.
-   Establece la variable de miembro **MediaCapture** en null.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Debes apagar el flujo de vista previa cuando el usuario abandone la página. Para ello, reemplaza el método [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

También debes apagar la secuencia de vista previa correctamente cuando la aplicación se suspenda. Para ello, registra un controlador para el evento [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) en el constructor de la página.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

En el controlador de eventos de **Suspensión**, comprueba primero que la página esté mostrando la clase [**Marco**](https://msdn.microsoft.com/library/windows/apps/br242682) de la aplicación. Para ello, compara el tipo de página con la propiedad [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390). Si la página no se muestra actualmente, es posible que el evento **OnNavigatedFrom** ya se haya generado y apagado el flujo de vista previa. Si la página se muestra actualmente, obtén un objeto [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) a partir de los argumentos del evento pasados al controlador para asegurarte de que el sistema no suspenda la aplicación hasta que el flujo de vista previa esté cerrado. Después de apagar el flujo, llama al método [**Completar**](https://msdn.microsoft.com/library/windows/apps/br224685) del aplazamiento para permitir que el sistema continúe suspendiendo tu aplicación.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## Capturar una imagen estática del flujo de vista previa

Es muy sencillo obtener una imagen estática del flujo de vista previa de la captura multimedia en forma de [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358). Para obtener más información, consulta [Obtener un marco de vista previa](get-a-preview-frame.md).

## Temas relacionados

* [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md)
* [Obtener un marco de vista previa](get-a-preview-frame.md)


<!--HONumber=Mar16_HO5-->



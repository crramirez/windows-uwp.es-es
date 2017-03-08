---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: "En este artículo se describe cómo mostrar rápidamente la secuencia de vista previa de la cámara en una página XAML en una aplicación para la Plataforma universal de Windows (UWP)."
title: "Mostrar la vista previa de la cámara"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d65d09349850f580d8bcee2d3875b38b8ed189f1
ms.lasthandoff: 02/07/2017

---

# <a name="display-the-camera-preview"></a>Mostrar la vista previa de la cámara

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este artículo se describe cómo mostrar rápidamente la secuencia de vista previa de la cámara en una página XAML en una aplicación para la Plataforma universal de Windows (UWP). La creación de una aplicación que capture fotos y vídeos con la cámara requiere que realices tareas, como controlar la orientación del dispositivo y de la cámara o establecer opciones de codificación para el archivo capturado. En algunos escenarios de la aplicación, es posible que solo quieras mostrar la secuencia de vista previa de la cámara sin preocuparte por estas otras consideraciones. En este artículo se muestra cómo hacerlo con la cantidad de código mínima. Ten en cuenta que siempre debes apagar la secuencia de vista previa correctamente cuando termines mediante el procedimiento siguiente.

Para obtener información sobre cómo escribir una aplicación de cámara que capture fotos o haga vídeos, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Agregar declaraciones de funcionalidades al manifiesto de la aplicación

Para que tu aplicación tenga acceso a la cámara de un dispositivo, debes declarar que esta usa las funcionalidades *cámara web* y *micrófono* del dispositivo. 

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona las casillas **Cámara web** y **Micrófono**.

## <a name="add-a-captureelement-to-your-page"></a>Agregar una clase CaptureElement a la página

Usa una clase [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) para visualizar la secuencia de vista previa en la página XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Usar MediaCapture para iniciar el flujo de vista previa

El objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) es la interfaz de la aplicación para la cámara del dispositivo. Esta clase es un miembro del espacio de nombres Windows.Media.Capture. En el ejemplo de este artículo también se usan las API de los espacios de nombres [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) y [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx), además de las que se incluyen con la plantilla de proyecto predeterminada.

Agrega con directivas para incluir los siguientes espacios de nombres en el archivo .cs de tu página.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Declara una variable de miembro de clase para el objeto **MediaCapture** y un valor booleano para comprobar si la cámara está actualmente en vista previa. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Declara una variable de tipo [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest) que se usará para asegurarte de que la pantalla no se apaga mientras se ejecuta la vista previa.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Crea una nueva instancia de la clase **MediaCapture** y llama a [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) para inicializar el dispositivo de captura. Este método puede presentar errores en los dispositivos que no tienen una cámara, por ejemplo, por lo que debe llamarlo desde un bloque **probar**. Se iniciará una **UnauthorizedAccessException** si intenta inicializar la cámara y el usuario deshabilitó el acceso a la cámara en la configuración de privacidad del dispositivo. También verá esta excepción durante el desarrollo si ignoró agregar las funcionalidades adecuadas al manifiesto de la aplicación.

**Importante** En algunas familias de dispositivos, se muestra al usuario una petición de consentimiento antes de que se conceda acceso a la aplicación para usar la cámara del dispositivo. Por este motivo, solo debes llamar a [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) desde la conversación principal de la interfaz de usuario. Intentar inicializar la cámara desde otra conversación puede producir errores de inicialización.

Para conectar el control **MediaCapture** a la clase **CaptureElement**, establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280). Llama al método [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) para iniciar la vista previa. Llama al método [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestActive) para asegurarte de que el dispositivo no entra en suspensión mientras se ejecuta la vista previa. Por último, establece la propiedad [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) en [**Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations) para evitar que la interfaz de usuario y el objeto **CaptureElement** giren cuando el usuario cambia la orientación del dispositivo. Para obtener más información sobre cómo controlar los cambios de orientación del dispositivo, consulta [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## <a name="shut-down-the-preview-stream"></a>Apagar la secuencia de vista previa

Cuando termines de usar la secuencia de vista previa, debes apagar siempre esa secuencia y deshacerte correctamente de los recursos asociados para garantizar la disponibilidad de la cámara para otras aplicaciones del dispositivo. Los pasos necesarios para cerrar la secuencia de vista previa son:

-   Si la cámara está en vista previa, llama al método [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) para detener la secuencia de vista previa. Se generará una excepción si llamas al método **StopPreviewAsync** mientras no se está ejecutando la vista previa.
-   Establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) del objeto **CaptureElement** en nulo. Usa [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) para asegurarte de que esta llamada se ejecuta en el subproceso de interfaz de usuario.
-   Llama al método [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) del objeto **MediaCapture** para liberar el objeto. Vuelve a usar [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) para asegurarte de que esta llamada se ejecuta en el subproceso de interfaz de usuario.
-   Establece la variable de miembro **MediaCapture** en nulo.
-   Llama al método [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestRelease) para permitir que la pantalla se apague cuando esté inactiva.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Debes apagar la secuencia de vista previa cuando el usuario abandone la página. Para ello, reemplaza el método [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

También debes apagar el flujo de vista previa correctamente cuando la aplicación se suspenda. Para ello, registra un controlador para el evento [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) en el constructor de la página.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

En el controlador de eventos **Suspending**, comprueba primero que la página esté mostrando la clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) de la aplicación. Para ello, compara el tipo de página con la propiedad [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390). Si la página no se muestra actualmente, es posible que el evento **OnNavigatedFrom** ya se haya generado y apagado el flujo de vista previa. Si la página se muestra actualmente, obtén un objeto [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) a partir de los argumentos del evento pasados al controlador para asegurarte de que el sistema no suspenda la aplicación hasta que el flujo de vista previa esté cerrado. Después de apagar la secuencia, llama al método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) del aplazamiento para permitir que el sistema continúe suspendiendo tu aplicación.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Obtener un fotograma de vista previa](get-a-preview-frame.md)


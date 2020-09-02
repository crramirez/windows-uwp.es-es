---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: En este artículo se describe cómo mostrar rápidamente la secuencia de vista previa de la cámara en una página XAML en una aplicación para la Plataforma universal de Windows (UWP).
title: Mostrar la vista previa de la cámara
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3487e79b689e5c47cc94ffc29a559a333fe66f47
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363778"
---
# <a name="display-the-camera-preview"></a>Mostrar la vista previa de la cámara


En este artículo se describe cómo mostrar rápidamente la secuencia de vista previa de la cámara en una página XAML en una aplicación para la Plataforma universal de Windows (UWP). La creación de una aplicación que capture fotos y vídeos con la cámara requiere que realices tareas, como controlar la orientación del dispositivo y de la cámara o establecer opciones de codificación para el archivo capturado. En algunos escenarios de la aplicación, es posible que solo quieras mostrar la secuencia de vista previa de la cámara sin preocuparte por estas otras consideraciones. En este artículo se muestra cómo hacerlo con la cantidad de código mínima. Ten en cuenta que siempre debes apagar la secuencia de vista previa correctamente cuando termines mediante el procedimiento siguiente.

Para obtener información sobre cómo escribir una aplicación de cámara que capture fotos o haga vídeos, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Agregar declaraciones de funcionalidades al manifiesto de la aplicación

Para que tu aplicación tenga acceso a la cámara de un dispositivo, debes declarar que esta usa las funcionalidades *webcam* y *microphone* del dispositivo. 

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Seleccione la pestaña **Funcionalidades**.
3.  Active la casilla de la **cámara web** y el cuadro de **micrófono**.

## <a name="add-a-captureelement-to-your-page"></a>Agregar una clase CaptureElement a la página

Usa una clase [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) para visualizar la secuencia de vista previa en la página XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml" id="SnippetCaptureElement":::



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Usar MediaCapture para iniciar el flujo de vista previa

El objeto [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) es la interfaz de la aplicación para la cámara del dispositivo. Esta clase es un miembro del espacio de nombres Windows.Media.Capture. En el ejemplo de este artículo también se usan las API de los espacios de nombres [**Windows.ApplicationModel**](/uwp/api/Windows.ApplicationModel) y [System.Threading.Tasks](/dotnet/api/system.threading.tasks), además de las que se incluyen con la plantilla de proyecto predeterminada.

Agrega con directivas para incluir los siguientes espacios de nombres en el archivo .cs de tu página.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSimpleCameraPreviewUsing":::

Declara una variable de miembro de clase para el objeto **MediaCapture** y un valor booleano para comprobar si la cámara está actualmente en vista previa. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

Declara una variable de tipo [**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest) que se usará para asegurarte de que la pantalla no se apaga mientras se ejecuta la vista previa.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareDisplayRequest":::

Cree un método auxiliar para iniciar la vista previa de la cámara, denominada **StartPreviewAsync** en este ejemplo. En función del escenario de la aplicación, es posible que quiera llamarlo desde el controlador de eventos **OnNavigatedTo** al que se llama cuando se carga la página o esperar e iniciar la vista previa en respuesta a los eventos de la interfaz de usuario.

Crea una nueva instancia de la clase **MediaCapture** y llama a [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) para inicializar el dispositivo de captura. Este método puede presentar errores en los dispositivos que no tienen una cámara, por ejemplo, por lo que debe llamarlo desde un bloque **probar**. Se iniciará una **UnauthorizedAccessException** si intenta inicializar la cámara y el usuario deshabilitó el acceso a la cámara en la configuración de privacidad del dispositivo. También verá esta excepción durante el desarrollo si ignoró agregar las funcionalidades adecuadas al manifiesto de la aplicación.

**Importante** En algunas familias de dispositivos, se muestra al usuario una petición de consentimiento antes de que se conceda acceso a la aplicación para usar la cámara del dispositivo. Por este motivo, solo debes llamar a [**MediaCapture.InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) desde la conversación principal de la interfaz de usuario. Intentar inicializar la cámara desde otra conversación puede producir errores de inicialización.

Para conectar el control **MediaCapture** a la clase **CaptureElement**, establece la propiedad [**Source**](/uwp/api/windows.ui.xaml.controls.captureelement.source). Llama al método [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync) para iniciar la vista previa. Este método producirá una **FileLoadException** si otra aplicación tiene el control exclusivo del dispositivo de captura. Vea la siguiente sección para obtener información que escucha los cambios en el control exclusivo.

Llama al método [**RequestActive**](/uwp/api/windows.system.display.displayrequest.requestactive) para asegurarte de que el dispositivo no entra en suspensión mientras se ejecuta la vista previa. Por último, establece la propiedad [**DisplayInformation.AutoRotationPreferences**](/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) en [**Landscape**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) para evitar que la interfaz de usuario y el objeto **CaptureElement** giren cuando el usuario cambia la orientación del dispositivo. Para obtener más información sobre cómo controlar los cambios de orientación del dispositivo, consulta [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartPreviewAsync":::

## <a name="handle-changes-in-exclusive-control"></a>Controlar los cambios en el control exclusivo
Como se indicó en la sección anterior, **StartPreviewAsync** producirá una **FileLoadException** si otra aplicación tiene el control exclusivo del dispositivo de captura. A partir de Windows 10, versión 1703, puede registrar un controlador para el evento [MediaCapture. CaptureDeviceExclusiveControlStatusChanged](/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged) , que se genera cada vez que cambia el estado de control exclusivo del dispositivo. En el controlador de este evento, Compruebe la propiedad [MediaCaptureDeviceExclusiveControlStatusChangedEventArgs. status](/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) para ver cuál es el estado actual. Si el nuevo estado es **SharedReadOnlyAvailable**, sabrá que actualmente no puede iniciar la versión preliminar y puede que desee actualizar la interfaz de usuario para alertar al usuario. Si el nuevo estado es **ExclusiveControlAvailable**, puede intentar volver a iniciar la vista previa de la cámara.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetExclusiveControlStatusChanged":::

## <a name="shut-down-the-preview-stream"></a>Apagar la secuencia de vista previa

Cuando termines de usar la secuencia de vista previa, debes apagar siempre esa secuencia y deshacerte correctamente de los recursos asociados para garantizar la disponibilidad de la cámara para otras aplicaciones del dispositivo. Los pasos necesarios para cerrar la secuencia de vista previa son:

-   Si la cámara está en vista previa, llama al método [**StopPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.stoppreviewasync) para detener la secuencia de vista previa. Se generará una excepción si llamas al método **StopPreviewAsync** mientras no se está ejecutando la vista previa.
-   Establece la propiedad [**Source**](/uwp/api/windows.ui.xaml.controls.captureelement.source) del objeto **CaptureElement** en nulo. Usa [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para asegurarte de que esta llamada se ejecuta en el subproceso de interfaz de usuario.
-   Llama al método [**Dispose**](/uwp/api/windows.media.capture.mediacapture.dispose) del objeto **MediaCapture** para liberar el objeto. Vuelve a usar [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para asegurarte de que esta llamada se ejecuta en el subproceso de interfaz de usuario.
-   Establece la variable de miembro **MediaCapture** en nulo.
-   Llama al método [**RequestRelease**](/uwp/api/windows.system.display.displayrequest.requestrelease) para permitir que la pantalla se apague cuando esté inactiva.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCleanupCameraAsync":::

Debes apagar la secuencia de vista previa cuando el usuario abandone la página. Para ello, reemplaza el método [**OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetOnNavigatedFrom":::

También debes apagar el flujo de vista previa correctamente cuando la aplicación se suspenda. Para ello, registra un controlador para el evento [**Application.Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) en el constructor de la página.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRegisterSuspending":::

En el controlador de eventos **Suspending**, comprueba primero que la página esté mostrando la clase [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) de la aplicación. Para ello, compara el tipo de página con la propiedad [**CurrentSourcePageType**](/uwp/api/windows.ui.xaml.controls.frame.currentsourcepagetype). Si la página no se muestra actualmente, es posible que el evento **OnNavigatedFrom** ya se haya generado y apagado el flujo de vista previa. Si la página se muestra actualmente, obtén un objeto [**SuspendingDeferral**](/uwp/api/Windows.ApplicationModel.SuspendingDeferral) a partir de los argumentos del evento pasados al controlador para asegurarte de que el sistema no suspenda la aplicación hasta que el flujo de vista previa esté cerrado. Después de apagar la secuencia, llama al método [**Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) del aplazamiento para permitir que el sistema continúe suspendiendo tu aplicación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSuspendingHandler":::


## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Obtener un fotograma de vista previa](get-a-preview-frame.md)

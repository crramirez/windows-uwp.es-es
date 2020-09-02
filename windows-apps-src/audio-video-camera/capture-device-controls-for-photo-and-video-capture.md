---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de fotos y vídeo mejorados, como la estabilización de imagen óptica y el zoom suave.
title: Controles manuales de la cámara para la captura de fotos y vídeos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7248b4f3fe515a164410305bac074f44cae53e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362868"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>Controles manuales de la cámara para la captura de fotos y vídeos



Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de fotos y vídeo mejorados, como la estabilización de imagen óptica y el zoom suave.

Los controles mencionados en este artículo se agregan a la aplicación con el mismo patrón. En primer lugar, comprueba si el control es compatible con el dispositivo actual en el que se ejecuta la aplicación. Si el control se admite, establece el modo deseado para él. Por lo general, si el dispositivo actual no admite un control en particular, debes deshabilitar u ocultar el elemento de la interfaz de usuario que permita al usuario habilitar esa característica.

El código de este artículo es una adaptación de la [muestra del SDK de controles manuales de la cámara](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls). Puedes descargar la muestra para ver el código usado en contexto o para emplearla como punto de partida para tu propia aplicación.

> [!NOTE]
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

Todas las API de control de dispositivos mencionadas en este artículo son miembros del espacio de nombres [**Windows.Media.Devices**](/uwp/api/Windows.Media.Devices).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

## <a name="exposure"></a>Exposure

[**ExposureControl**](/uwp/api/Windows.Media.Devices.ExposureControl) te permite definir la velocidad del obturador que se usa durante la captura de fotos o vídeo.

Este ejemplo usa un control [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar el valor actual de la exposición y una casilla para activar o desactivar el ajuste de exposición automática.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetExposureXAML":::

Comprueba si el dispositivo de captura actual admite **ExposureControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.exposurecontrol.supported). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Establece el estado de activación de la casilla para indicar si el ajuste de exposición automática está activo en el valor de la propiedad [**Auto**](/uwp/api/windows.media.devices.exposurecontrol.auto).

El valor de exposición debe estar dentro del intervalo admitido por el dispositivo y debe ser un incremento del tamaño de paso admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](/uwp/api/windows.media.devices.exposurecontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecontrol.max) y [**Step**](/uwp/api/windows.media.devices.exposurecontrol.step), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante en el valor actual de **ExposureControl** después de anular el registro del controlador de eventos [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que el evento no se desencadene cuando se define el valor.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureControl":::

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor de exposición mediante una llamada a [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureSlider":::

En el controlador de eventos **CheckedChanged** de la casilla de exposición automática, activa y desactiva el ajuste de exposición automática llamando a [**SetAutoAsync**](/uwp/api/windows.media.devices.exposurecontrol.setautoasync) y pasando un valor booleano.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureCheckBox":::

> [!IMPORTANT]
> El modo de exposición automática solo se admite mientras se ejecuta la secuencia de vista previa. Comprueba que la secuencia de vista previa se está ejecutando antes de activar la exposición automática.

## <a name="exposure-compensation"></a>Compensación de exposición

El [**ExposureCompensationControl**](/uwp/api/Windows.Media.Devices.ExposureCompensationControl) te permite establecer la compensación de exposición que se usa durante la captura de fotos o vídeo.

Este ejemplo usa un control [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar el valor de compensación de exposición actual.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetEvXAML":::

Comprueba si el dispositivo de captura actual admite **ExposureCompensationControl** comprobando la propiedad [Supported](supported-codecs.md). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función.

El valor de compensación de la exposición debe estar dentro del intervalo que admite el dispositivo y debe ser un incremento del tamaño de los pasos admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](/uwp/api/windows.media.devices.exposurecompensationcontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecompensationcontrol.max) y [**Step**](/uwp/api/windows.media.devices.exposurecompensationcontrol.step), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual de **ExposureCompensationControl** después de anular el registro del controlador de eventos [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que el evento no se desencadene cuando se define el valor.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvControl":::

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor de exposición mediante una llamada a [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecompensationcontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvValueChanged":::

## <a name="flash"></a>Intermitente

[**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) te permite activar o desactivar el flash o activar el flash automático, que permite al sistema determinar dinámicamente si se usa el flash. Este control también te permite habilitar la reducción de ojos rojos automática en los dispositivos que lo admiten. Todas estas opciones de configuración se aplican a la captura de fotos. El [**TorchControl**](/uwp/api/Windows.Media.Devices.TorchControl) es un control independiente para activar o desactivar la linterna para la captura de vídeo.

En este ejemplo se usa un conjunto de botones de radio para permitir al usuario alternar entre activado, desactivado y la configuración de flash automática. También se proporciona una casilla para permitir la alternancia de la función de reducción de ojos rojos y la linterna de vídeo.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFlashXAML":::

Comprueba si el dispositivo de captura actual admite **FlashControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Si se admite **FlashControl**, es posible que la reducción de ojos rojos automática sea o no sea compatible; por lo tanto, comprueba la propiedad [**RedEyeReductionSupported**](/uwp/api/windows.media.devices.flashcontrol.redeyereductionsupported) antes de habilitar la interfaz de usuario. Como **TorchControl** es independiente del control del flash, también debes comprobar la propiedad [**Supported**](/uwp/api/windows.media.devices.torchcontrol.supported) antes de usarlo.

En el controlador de eventos [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) de cada botón de opción del flash, activa o desactiva los ajustes del flash que correspondan. Ten en cuenta que para definir que el flash se use siempre, debes definir la propiedad [**Enabled**](/uwp/api/windows.media.devices.flashcontrol.enabled) como true y la propiedad [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto), como false.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashControl":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashRadioButtons":::

En el controlador de la casilla de reducción de ojos rojos, define la propiedad [**RedEyeReduction**](/uwp/api/windows.media.devices.flashcontrol.redeyereduction) con el valor adecuado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetRedEye":::

Por último, en el controlador de la casilla de la linterna de vídeo, define la propiedad [**Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) con el valor adecuado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTorch":::

> [!NOTE] 
>  En algunos dispositivos, la linterna no emite luz, incluso si el objeto [**TorchControl.Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) se establece en true, a menos que el dispositivo tenga una secuencia de vista previa en ejecución y esté capturando vídeo activamente. El orden de operaciones recomendado es activar la vista previa de vídeo, activar la linterna estableciendo **Enabled** en true y, a continuación, iniciar la captura de vídeo. En algunos dispositivos la linterna se enciende después de iniciar la vista previa. En otros dispositivos, es posible que la linterna no se encienda hasta que se inicie la captura de vídeo.

## <a name="focus"></a>Foco

El objeto [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) admite tres métodos diferentes usados con frecuencia para ajustar el enfoque de la cámara: autoenfoque continuo, pulsar para enfocar y enfoque manual. Una aplicación de cámara puede admitir los tres métodos, pero para fines de simplificación, en este artículo se explican las tres técnicas por separado. En esta sección también se describe cómo habilitar la luz de la ayuda de foco.

### <a name="continuous-autofocus"></a>Autofocus continuo

Si se habilita el autofocus, se indica a la cámara que debe ajustar el foco dinámicamente para intentar mantener enfocado al sujeto de la foto o vídeo. Este ejemplo usa un botón de radio para activar y desactivar el autoenfoque continuo.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetCAFXAML":::

Comprueba si el dispositivo de captura actual admite el objeto **FocusControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported). A continuación, determina si se admite el autoenfoque continuo comprobando la lista [**SupportedFocusModes**](/uwp/api/windows.media.devices.focuscontrol.supportedfocusmodes) para ver si contiene el valor [**FocusMode.Continuous**](/uwp/api/Windows.Media.Devices.FocusMode) y, si es así, muestra el botón de radio de autoenfoque continuo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCAF":::

En el controlador de eventos [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) del botón de radio de autoenfoque continuo, usa la propiedad [**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) para obtener una instancia del control. Llama a [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) para desbloquear el control en caso de que la aplicación haya llamado anteriormente a [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) para habilitar uno de los otros modos de enfoque.

Crea un nuevo objeto [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) y establece la propiedad [**Mode**](/uwp/api/windows.media.devices.focussettings.mode) en **Continuous**. Establece la propiedad [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) en un valor adecuado para el escenario de la aplicación o seleccionado por el usuario en tu interfaz de usuario. Pasa el objeto **FocusSettings** al método [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) y, a continuación, llama a [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) para iniciar el autoenfoque continuo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCafFocusRadioButton":::

> [!IMPORTANT]
> El modo de autoenfoque solo se admite mientras la secuencia de vista previa está en ejecución. Comprueba que la secuencia de vista previa se está ejecutando antes de activar el autoenfoque automático.

### <a name="tap-to-focus"></a>Pulsar para enfocar

La técnica de pulsar para enfocar usa el objeto [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) y el [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) para especificar una subregión del marco de captura donde debe centrarse el dispositivo de captura. La región de enfoque se determina por la pulsación del usuario en la pantalla que muestra la secuencia de vista previa.

En este ejemplo se usa un botón de radio para habilitar y deshabilitar el modo de pulsar para enfocar.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetTapFocusXAML":::

Comprueba si el dispositivo de captura actual admite el objeto **FocusControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported). Para poder usar esta técnica, **RegionsOfInterestControl** debe ser compatible y debe admitir al menos una región. Comprueba las propiedades [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) y [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) para determinar si se debe mostrar u ocultar el botón de radio de pulsar para enfocar.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocus":::

En el controlador de eventos [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) del botón de radio de pulsar para enfocar, usa la propiedad [**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) para obtener una instancia del control. Llama a [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) para bloquear el control en caso de que la aplicación haya llamado anteriormente a [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) para habilitar el autoenfoque continuo y luego espera a que el usuario pulse la pantalla para cambiar el enfoque.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusRadioButton":::

Este ejemplo se centra en una región cuando el usuario pulsa la pantalla y, a continuación, quita el enfoque de esa región cuando el usuario pulsa de nuevo, como un botón de alternancia. Usa una variable booleana para realizar el seguimiento del estado actual de alternancia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsFocused":::

El siguiente paso es escuchar el evento cuando el usuario pulsa la pantalla, controlando el evento [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped) de [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement), que está mostrando actualmente la secuencia de vista previa de captura. Si la cámara no muestra actualmente la vista previa o si el modo de pulsar para enfocar no está habilitado, regresa del controlador sin hacer nada.

Si la variable de seguimiento * \_ isFocused* se cambia a false y la cámara no está actualmente en el proceso de foco (determinado por la propiedad [**FocusState**](/uwp/api/windows.media.devices.focuscontrol.focusstate) del **FocusControl**), comienza el proceso de punteo en el foco. Obtén la posición de la pulsación del usuario a partir de los argumentos del evento pasados al controlador. En este ejemplo también se usa esta oportunidad para elegir el tamaño de la región que se enfocará. En este caso, el tamaño es 1/4 de la dimensión más pequeña del elemento de captura. Pasa la posición de pulsación y el tamaño de la región al método auxiliar **TapToFocus** que se define en la siguiente sección.

Si el control de alternancia * \_ isFocused* está establecido en true, el usuario TAP debe borrar el foco de la región anterior. Esto se realiza en el método auxiliar **TapUnfocus** que se muestra a continuación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusPreviewControl":::

En el método auxiliar **TapToFocus** , establezca primero el valor de * \_ isFocused* en true para que la siguiente pulsación de pantalla libere el foco de la región punteada.

Es la siguiente tarea de este método auxiliar es determinar el rectángulo dentro de la secuencia de vista previa que se asignará al control de foco. Esto requiere dos pasos. El primer paso es determinar el rectángulo que la secuencia de vista previa ocupa dentro del control [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement). Esto depende de las dimensiones de la secuencia de vista previa y de la orientación del dispositivo. El método auxiliar **GetPreviewStreamRectInControl**, que se muestra al final de esta sección, realiza esta tarea y devuelve el rectángulo que contiene la secuencia de vista previa.

La siguiente tarea en **TapToFocus** es convertir la ubicación de pulsación y el tamaño del rectángulo de enfoque deseado, que se determinaron dentro del controlador de eventos **CaptureElement.Tapped** en coordenadas de la secuencia de captura. El método auxiliar **ConvertUiTapToPreviewRect**, que se muestra más adelante en esta sección, realiza esta conversión y devuelve el rectángulo, en coordenadas de la secuencia de captura, donde se solicitará el enfoque.

Ahora que se ha obtenido el rectángulo de destino, crea un nuevo objeto [**RegionOfInterest**](/uwp/api/Windows.Media.Devices.RegionOfInterest) estableciendo la propiedad [**Bounds**](/uwp/api/windows.media.devices.regionofinterest.bounds) en el rectángulo de destino obtenido en los pasos anteriores.

Obtén el objeto [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) del dispositivo de captura. Crea un nuevo objeto [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) y establece el [**Mode**](/uwp/api/windows.media.devices.focuscontrol.mode) y [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) en los valores deseados después de asegurarte de que son compatibles con el **FocusControl**. Llama a [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) en el **FocusControl** para activar la configuración e indicar al dispositivo que empiece a enfocarse en la región especificada.

A continuación, obtén el objeto [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) del dispositivo de captura y llama a [**SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync) para establecer la región activa. Se pueden establecer varias regiones de interés en los dispositivos que lo admiten, pero este ejemplo solo establece una sola región.

Por último, llama a [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) en el objeto **FocusControl** para iniciar el enfoque.

> [!IMPORTANT]
> Al implementar la pulsación para enfocar, el orden de las operaciones es importante. Debes llamar a estas API en el siguiente orden:
>
> 1. [**FocusControl.Configure**](/uwp/api/windows.media.devices.focuscontrol.configure)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync)
> 3. [**FocusControl.FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapToFocus":::

En el método auxiliar **TapUnfocus**, obtén el objeto **RegionsOfInterestControl** y llama a [**ClearRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.clearregionsasync) para borrar la región que se haya registrado con el control dentro del método auxiliar **TapToFocus**. A continuación, obtén el objeto **FocusControl** y llama a [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) para que el dispositivo se vuelva a enfocar sin una región de interés.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapUnfocus":::

El método auxiliar **GetPreviewStreamRectInControl** usa la resolución de la secuencia de vista previa y la orientación del dispositivo para determinar el rectángulo dentro del elemento de vista previa que contiene la secuencia de vista previa, y recorta cualquier espaciado ancho que puede proporcionar el control para mantener la relación de aspecto de la secuencia. Este método usa variables de miembro de clase definidas en el código de ejemplo de captura básica de elementos multimedia que se encuentra en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetGetPreviewStreamRectInControl":::

El método auxiliar **ConvertUiTapToPreviewRect** toma como argumentos la ubicación del evento de pulsación, el tamaño deseado de la región de enfoque y el rectángulo que contiene la secuencia de vista previa obtenida del método auxiliar **GetPreviewStreamRectInControl**. Este método usa estos valores y la orientación actual del dispositivo para calcular el rectángulo dentro de la secuencia de vista previa que contiene la región deseada. Una vez más, este método usa variables de miembro de clase definidas en el código de ejemplo de captura básica de elementos multimedia que se encuentra en [Capturar fotos y vídeo con MediaCapture](./index.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetConvertUiTapToPreviewRect":::

### <a name="manual-focus"></a>Enfoque manual

La técnica de enfoque manual usa un control **Slider** para definir la profundidad de enfoque actual del dispositivo de captura. Se usa un botón de radio para activar y desactivar el enfoque manual.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetManualFocusXAML":::

Comprueba si el dispositivo de captura actual admite el objeto **FocusControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función.

El valor de enfoque debe estar dentro del intervalo que admite el dispositivo y debe ser un incremento del tamaño de los pasos admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](/uwp/api/windows.media.devices.focuscontrol.min), [**Max**](/uwp/api/windows.media.devices.focuscontrol.max) y [**Step**](/uwp/api/windows.media.devices.focuscontrol.step), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual del objeto **FocusControl** después de anular el registro del controlador de eventos [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que el evento no se desencadene cuando se define el valor.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocus":::

En el controlador de eventos **Checked** del botón de radio de enfoque manual, obtén el objeto **FocusControl** y llama a [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) en caso de que la aplicación hubiera desbloqueado el enfoque anteriormente con una llamada a [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetManualFocusChecked":::

En el controlador de eventos **ValueChanged** del control deslizante del enfoque manual, obtén el valor actual del control y define el valor de enfoque mediante una llamada a [**SetValueAsync**](/uwp/api/windows.media.devices.focuscontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusSlider":::

### <a name="enable-the-focus-light"></a>Habilitar la luz de enfoque

En los dispositivos que lo admiten, puedes habilitar una luz de enfoque auxiliar para ayudar al dispositivo en el proceso de enfoque. En este ejemplo, se usa una casilla para habilitar o deshabilitar la luz de enfoque auxiliar.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFocusLightXAML":::

Comprueba si el dispositivo de captura actual admite **FlashControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported). Asimismo, comprueba [**AssistantLightSupported**](/uwp/api/windows.media.devices.flashcontrol.assistantlightsupported) para asegurarte de que también se admite la luz auxiliar. Si se admiten ambos, puedes mostrar y habilitar la interfaz de usuario para esta función.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLight":::

En el controlador de eventos **CheckedChanged**, obtén el objeto [**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) del dispositivo de captura. Define la propiedad [**AssistantLightEnabled**](/uwp/api/windows.media.devices.flashcontrol.assistantlightenabled) para habilitar o deshabilitar la luz de enfoque.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLightCheckBox":::

## <a name="iso-speed"></a>Velocidad ISO

[**IsoSpeedControl**](/uwp/api/Windows.Media.Devices.IsoSpeedControl) te permite definir la velocidad ISO que se usa durante la captura de fotos o vídeo.

En este ejemplo se usa un control [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar el valor de compensación de exposición actual y una casilla para alternar el ajuste de velocidad ISO automática.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetIsoXAML":::

Comprueba si el dispositivo de captura actual admite **IsoSpeedControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.isospeedcontrol.supported). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Establece el estado de activación de la casilla para indicar si el ajuste de velocidad ISO automática está activo en el valor de la propiedad [**Auto**](/uwp/api/windows.media.devices.isospeedcontrol.auto).

El valor de velocidad ISO debe estar dentro del intervalo admitido por el dispositivo y debe ser un incremento del tamaño de paso admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](/uwp/api/windows.media.devices.isospeedcontrol.min), [**Max**](/uwp/api/windows.media.devices.isospeedcontrol.max) y [**Step**](/uwp/api/windows.media.devices.isospeedcontrol.step), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual del **IsoSpeedControl** después de anular el registro del controlador de eventos [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que el evento no se desencadene cuando se define el valor.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoControl":::

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor de velocidad ISO mediante una llamada a [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoSlider":::

En el controlador de eventos **CheckedChanged** de la casilla de velocidad ISO automática, activa el ajuste de velocidad ISO automática llamando a [**SetAutoAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setautoasync). Para desactivar el ajuste velocidad ISO automática, llama a [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) y pasa el valor actual del control deslizante.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoCheckBox":::

## <a name="optical-image-stabilization"></a>Estabilización de imagen óptica

La estabilización de imagen óptica (OIS) estabiliza una secuencia de vídeo capturada manipulando mecánicamente el dispositivo de captura de hardware, lo que puede proporcionar un resultado superior a la estabilización digital. En los dispositivos que no admiten OIS, puedes usar el efecto VideoStabilizationEffect para realizar la estabilización digital de los vídeos capturados. Para más información, consulta [Efectos para captura de vídeo](effects-for-video-capture.md).

Para determinar si el dispositivo actual admite OIS, comprueba la propiedad [**OpticalImageStabilizationControl.Supported**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supported).

El control de estabilización de imagen óptica admite tres modos: activado, desactivado y automático, lo que significa que el dispositivo determina dinámicamente si OIS puede mejorar la captura multimedia y, si es así, habilita OIS. Para determinar si un dispositivo es compatible con un modo determinado, comprueba si la colección [**OpticalImageStabilizationControl.SupportedModes**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supportedmodes) contiene el modo deseado.

Para habilitar o deshabilitar OIS, define [**OpticalImageStabilizationControl.Mode**](/uwp/api/Windows.Media.Devices.OpticalImageStabilizationMode) en el modo deseado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetOpticalImageStabilizationMode":::

## <a name="powerline-frequency"></a>Frecuencia de la línea de alimentación
Algunos dispositivos de cámara admiten el procesamiento de antiparpadeo que depende de conocer la frecuencia de la CA de las líneas de alimentación en el entorno actual. Algunos dispositivos admiten la determinación automática de la frecuencia de la línea de alimentación, mientras que otros requieren que la frecuencia se establezca de forma manual. El siguiente ejemplo de código muestra cómo determinar la compatibilidad de la frecuencia de la línea de alimentación en el dispositivo y, si es necesario, cómo establecer la frecuencia de forma manual. 

En primer lugar, llama al método **VideoDeviceController**[**TryGetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trygetpowerlinefrequency), pasando un parámetro de salida de tipo [**PowerlineFrequency**](/uwp/api/Windows.Media.Capture.PowerlineFrequency); si se produce un error en esta llamada, el control de frecuencia de la línea de alimentación no se admite en el dispositivo actual. Si se admite la característica, puedes determinar si el modo automático está disponible en el dispositivo intentando establecer el modo automático. Para ello, llame a [**TrySetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trysetpowerlinefrequency) y pase el valor **auto**. Si la llamada se realiza correctamente, significa que se admite la frecuencia de la Powerline automática. Si en el dispositivo se admite el controlador de frecuencia de la línea de alimentación, pero no se admite la detección automática de la frecuencia, puedes establecer la frecuencia manualmente mediante el uso de **TrySetPowerlineFrequency**. En este ejemplo, **MyCustomFrequencyLookup** es un método personalizado que se implementa para determinar la frecuencia correcta para la ubicación actual del dispositivo. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetPowerlineFrequency":::

## <a name="white-balance"></a>Balance de blancos

[**WhiteBalanceControl**](/uwp/api/windows.media.devices.videodevicecontroller.whitebalancecontrol) te permite definir el balance de blancos que se usa durante la captura de fotos o vídeo.

En este ejemplo se usa un control [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para seleccionar entre valores predeterminados de temperatura de color integrados y un control [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) para el ajuste manual del balance de blancos.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetWhiteBalanceXAML":::

Comprueba si el dispositivo de captura actual admite **WhiteBalanceControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.whitebalancecontrol.supported). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Establece los elementos del cuadro combinado en los valores de la enumeración [**ColorTemperaturePreset**](/uwp/api/Windows.Media.Devices.ColorTemperaturePreset). También, establece el elemento seleccionado en el valor actual de la propiedad [**Preset**](/uwp/api/windows.media.devices.whitebalancecontrol.preset).

Para el control manual, el valor del balance de blancos debe estar dentro del intervalo admitido por el dispositivo y debe ser un incremento del tamaño de paso admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](/uwp/api/windows.media.devices.whitebalancecontrol.min), [**Max**](/uwp/api/windows.media.devices.whitebalancecontrol.max) y [**Step**](/uwp/api/windows.media.devices.whitebalancecontrol.step), que se usan para establecer las propiedades correspondientes del control deslizante. Antes de habilitar el control manual, comprueba que el intervalo entre los valores mínimos y máximos admitidos es mayor que el tamaño de paso. Si no es así, no se admite el control manual en el dispositivo actual.

Define el valor del control deslizante con el valor actual de **WhiteBalanceControl** después de anular el registro del controlador de eventos [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que el evento no se desencadene cuando se define el valor.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalance":::

En el controlador de eventos [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) del cuadro combinado de valores predeterminados de temperatura de color, obtén el valor predeterminado seleccionado actualmente y define el valor del control llamando a [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync). Si el valor predeterminado seleccionado no es **Manual**, deshabilita el control deslizante del balance de blancos manual.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceComboBox":::

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor del balance de blancos mediante una llamada a [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceSlider":::

> [!IMPORTANT]
> El ajuste del balance de blancos solo se admite mientras se ejecuta la secuencia de vista previa. Comprueba que la secuencia de vista previa se está ejecutando antes de establecer el valor del balance de blancos o el valor predeterminado.

> [!IMPORTANT]
> El valor predeterminado de **ColorTemperaturePreset.Auto** indica al sistema que ajuste automáticamente el nivel del balance de blancos. En algunos escenarios, como capturar una secuencia fotográfica donde los niveles de balance de blancos deben iguales para cada fotograma, te recomendamos que bloquees el control en el valor automático actual. Para ello, llama a [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) y especifica el valor predeterminado **Manual** y no establezcas un valor en el control mediante [**SetValueAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setvalueasync). Esto hará que el dispositivo bloquee el valor actual. No intentes leer el valor actual del control y después pasar el valor obtenido a **SetValueAsync** porque no se garantiza que este valor sea correcto.

## <a name="zoom"></a>Zoom

El [**ZoomControl**](/uwp/api/Windows.Media.Devices.ZoomControl) te permite establecer el nivel de zoom que se usa durante la captura de fotos o vídeo.

Este ejemplo usa un control [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) para ajustar el nivel de zoom actual. En la siguiente sección, se muestra cómo ajustar el zoom con un gesto de reducir en la pantalla.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetZoomXAML":::

Comprueba si el dispositivo de captura actual admite **ZoomControl** comprobando la propiedad [**Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función.

El valor del nivel de zoom debe estar dentro del intervalo que admite el dispositivo y debe ser un incremento del tamaño de los pasos admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](/uwp/api/windows.media.devices.zoomcontrol.min), [**Max**](/uwp/api/windows.media.devices.zoomcontrol.max) y [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual del objeto **ZoomControl** después de anular el registro del controlador de eventos [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) para que el evento no se desencadene cuando se define el valor.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomControl":::

En el controlador de eventos **ValueChanged**, crea una nueva instancia de la clase [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings) y define la propiedad [**Value**](/uwp/api/windows.media.devices.zoomsettings.value) con el valor actual del control deslizante de zoom. Si la propiedad [**SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) del **ZoomControl** contiene [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode), significa que el dispositivo admite transiciones suaves entre niveles de zoom. Dado que este modo proporciona una mejor experiencia del usuario, normalmente querrás usar este valor para la propiedad [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) del objeto **ZoomSettings**.

Por último, cambia la configuración de zoom actual al pasar el objeto **ZoomSettings** al método [**Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) del objeto **ZoomControl**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomSlider":::

### <a name="smooth-zoom-using-pinch-gesture"></a>Zoom suave con el gesto de reducir

Como se analizó en la sección anterior, en los dispositivos que lo admiten, el modo de zoom suave permite que el dispositivo de captura cambie sin inconvenientes entre niveles de zoom digital, lo que permite al usuario ajustar el nivel de zoom dinámicamente durante la operación de captura sin transiciones discretas y un poco molestas. En esta sección se describe cómo ajustar el nivel de zoom en respuesta a un gesto de reducir.

Primero, determine si el control de zoom digital es compatible en el dispositivo actual comprobando la propiedad [**ZoomControl.Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported). A continuación, determina si el modo de zoom suave está disponible mediante la comprobación de [**ZoomControl.SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) para ver si contiene el valor [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsSmoothZoomSupported":::

En un dispositivo habilitado para funcionalidad multitáctil, un escenario típico consiste en ajustar el factor de zoom con un gesto de reducir con dos dedos. Establezca la propiedad [**ManipulationMode**](/uwp/api/windows.ui.xaml.uielement.manipulationmode) del control [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) en [**ManipulationModes.Scale**](/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) para permitir que el gesto de reducir. A continuación, registra el evento [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) que se desencadena cuando el gesto de reducir cambia el tamaño.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPinchGestureHandler":::

En el controlador del evento **ManipulationDelta**, actualiza el factor de zoom en función de los cambios en el gesto de reducir del usuario. El valor [**ManipulationDelta.Scale**](/uwp/api/Windows.UI.Input.ManipulationDelta) representa el cambio en escala del gesto de reducir, de modo que un pequeño incremento en el tamaño del gesto de reducir es un número ligeramente mayor que 1.0 y una pequeña reducción es un número ligeramente menor que 1.0. En este ejemplo, el valor actual del control de zoom se multiplica por la diferencia de la escala.

Antes de establecer el factor de zoom, debes asegurarte de que el valor no es inferior al valor mínimo que admite el dispositivo, como lo indica la propiedad [**ZoomControl.Min**](/uwp/api/windows.media.devices.zoomcontrol.min). Además, asegúrate de que el valor es menor o igual al valor de [**ZoomControl.Max**](/uwp/api/windows.media.devices.zoomcontrol.max). Por último, debe asegurarse de que el factor de zoom es un múltiplo del tamaño del paso de zoom compatible con el dispositivo, como se indica en la propiedad [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step) . Si el factor de zoom no cumple estos requisitos, se generará una excepción cuando intentes establecer el nivel de zoom en el dispositivo de captura.

Para establecer el nivel de zoom en el dispositivo de captura, crea un nuevo objeto [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings). Establece la propiedad [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) en [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) y, a continuación, establece la propiedad [**Value**](/uwp/api/windows.media.devices.zoomsettings.value) en el factor de zoom deseado. Por último, llama a [**ZoomControl.Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) para definir el nuevo valor de zoom en el dispositivo. El dispositivo pasará sin problemas al nuevo valor de zoom.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)

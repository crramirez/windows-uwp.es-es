---
author: drewbatgit
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de fotos y vídeo mejorados, como la estabilización de imagen óptica y el zoom suave.
title: Controles manuales de la cámara para la captura de fotos y vídeos
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: d0d7a429cf702455d969e1ac1c62def6181e8dd0
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.locfileid: "240642"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>Controles manuales de la cámara para la captura de fotos y vídeos

\[ Actualizado para aplicaciones para UWP en Windows10. Para leer más artículos sobre Windows8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de fotos y vídeo mejorados, como la estabilización de imagen óptica y el zoom suave.

Los controles mencionados en este artículo se agregan a la aplicación con el mismo patrón. En primer lugar, comprueba si el control es compatible con el dispositivo actual en el que se ejecuta la aplicación. Si el control se admite, establece el modo deseado para él. Por lo general, si el dispositivo actual no admite un control en particular, debes deshabilitar u ocultar el elemento de la interfaz de usuario que permita al usuario habilitar esa característica.

El código de este artículo es una adaptación de la [muestra del SDK de controles manuales de la cámara](https://go.microsoft.com/fwlink/?linkid=845228). Puedes descargar la muestra para ver el código usado en contexto o para usar la muestra como punto de partida para tu propia aplicación.

> [!NOTE]
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para la implementación de la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código de este artículo supone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

Todas las API de control de dispositivos mencionadas en este artículo son miembros del espacio de nombres [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## <a name="exposure"></a>Exposure

[**ExposureControl**](https://msdn.microsoft.com/library/windows/apps/dn278910) te permite definir la velocidad del obturador que se usa durante la captura de fotos o vídeo.

Este ejemplo usa un control [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar el valor actual de la exposición y una casilla para activar o desactivar el ajuste de exposición automática.

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

Comprueba si el dispositivo de captura actual admite **ExposureControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297710). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Establece el estado de activación de la casilla para indicar si el ajuste de exposición automática está activo en el valor de la propiedad [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn278911).

El valor de exposición debe estar dentro del intervalo admitido por el dispositivo y debe ser un incremento del tamaño de paso admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278919), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278914) y [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278930), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante en el valor actual de **ExposureControl** después de anular el registro del controlador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que el evento no se desencadene cuando se define el valor.

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor de exposición mediante una llamada a [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927).

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

En el controlador de eventos **CheckedChanged** de la casilla de exposición automática, activa y desactiva el ajuste de exposición automática llamando a [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn278920) y pasando un valor booleano.

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> El modo de exposición automática solo se admite mientras se ejecuta la secuencia de vista previa. Comprueba que la secuencia de vista previa se está ejecutando antes de activar la exposición automática.

## <a name="exposure-compensation"></a>Compensación de exposición

El [**ExposureCompensationControl**](https://msdn.microsoft.com/library/windows/apps/dn278897) te permite establecer la compensación de exposición que se usa durante la captura de fotos o vídeo.

Este ejemplo usa un control [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar el valor de compensación de exposición actual.

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

Comprueba si el dispositivo de captura actual admite **ExposureCompensationControl** comprobando la propiedad [Supported](supported-codecs.md). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función.

El valor de compensación de la exposición debe estar dentro del intervalo que admite el dispositivo y debe ser un incremento del tamaño de los pasos admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278901), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278899) y [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278904), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual de **ExposureCompensationControl** después de anular el registro del controlador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que el evento no se desencadene cuando se define el valor.

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor de exposición mediante una llamada a [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278903).

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## <a name="flash"></a>Flash

[**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) te permite activar o desactivar el flash o activar el flash automático, que permite al sistema determinar dinámicamente si se usa el flash. Este control también te permite habilitar la reducción de ojos rojos automática en los dispositivos que lo admiten. Todas estas opciones de configuración se aplican a la captura de fotos. El [**TorchControl**](https://msdn.microsoft.com/library/windows/apps/dn279077) es un control independiente para activar o desactivar la linterna para la captura de vídeo.

En este ejemplo se usa un conjunto de botones de radio para permitir al usuario alternar entre activado, desactivado y la configuración de flash automática. También se proporciona una casilla para permitir la alternancia de la función de reducción de ojos rojos y la linterna de vídeo.

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

Comprueba si el dispositivo de captura actual admite **FlashControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Si se admite **FlashControl**, es posible que la reducción de ojos rojos automática sea o no sea compatible; por lo tanto, comprueba la propiedad [**RedEyeReductionSupported**](https://msdn.microsoft.com/library/windows/apps/dn297766) antes de habilitar la interfaz de usuario. Como **TorchControl** es independiente del control del flash, también debes comprobar la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279081) antes de usarlo.

En el controlador de eventos [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) de cada botón de opción del flash, activa o desactiva los ajustes del flash que correspondan. Ten en cuenta que para definir que el flash se use siempre, debes definir la propiedad [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn297733) como true y la propiedad [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn297728), como false.

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

En el controlador de la casilla de reducción de ojos rojos, define la propiedad [**RedEyeReduction**](https://msdn.microsoft.com/library/windows/apps/dn297758) con el valor adecuado.

[!code-cs[RedEye](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

Por último, en el controlador de la casilla de la linterna de vídeo, define la propiedad [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) con el valor adecuado.

[!code-cs[Linterna](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  En algunos dispositivos, la linterna no emite luz, incluso si el objeto [**TorchControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) se establece en true, a menos que el dispositivo tenga una secuencia de vista previa en ejecución y esté capturando vídeo activamente. El orden de operaciones recomendado es activar la vista previa de vídeo, activar la linterna estableciendo **Enabled** en true y, a continuación, iniciar la captura de vídeo. En algunos dispositivos la linterna se enciende después de iniciar la vista previa. En otros dispositivos, es posible que la linterna no se encienda hasta que se inicie la captura de vídeo.

## <a name="focus"></a>Enfocar

El objeto [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) admite tres métodos diferentes usados con frecuencia para ajustar el enfoque de la cámara: autoenfoque continuo, pulsar para enfocar y enfoque manual. Una aplicación de cámara puede admitir los tres métodos, pero para fines de simplificación, en este artículo se explican las tres técnicas por separado. En esta sección también se describe cómo habilitar la luz de la ayuda de foco.

### <a name="continuous-autofocus"></a>Autofocus continuo

Si se habilita el autofocus, se indica a la cámara que debe ajustar el foco dinámicamente para intentar mantener enfocado al sujeto de la foto o vídeo. Este ejemplo usa un botón de radio para activar y desactivar el autoenfoque continuo.

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

Comprueba si el dispositivo de captura actual admite el objeto **FocusControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785). A continuación, determina si se admite el autoenfoque continuo comprobando la lista [**SupportedFocusModes**](https://msdn.microsoft.com/library/windows/apps/dn608079) para ver si contiene el valor [**FocusMode.Continuous**](https://msdn.microsoft.com/library/windows/apps/dn608084) y, si es así, muestra el botón de radio de autoenfoque continuo.

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

En el controlador de eventos [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) del botón de radio de autoenfoque continuo, usa la propiedad [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) para obtener una instancia del control. Llama a [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) para desbloquear el control en caso de que la aplicación haya llamado anteriormente a [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) para habilitar uno de los otros modos de enfoque.

Crea un nuevo objeto [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) y establece la propiedad [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608090) en **Continuous**. Establece la propiedad [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) en un valor adecuado para el escenario de la aplicación o seleccionado por el usuario en tu interfaz de usuario. Pasa el objeto **FocusSettings** al método [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) y, a continuación, llama a [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) para iniciar el autoenfoque continuo.

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> El modo de autoenfoque solo se admite mientras la secuencia de vista previa está en ejecución. Comprueba que la secuencia de vista previa se está ejecutando antes de activar el autoenfoque automático.

### <a name="tap-to-focus"></a>Pulsar para enfocar

La técnica de pulsar para enfocar usa el objeto [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) y el [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) para especificar una subregión del marco de captura donde debe centrarse el dispositivo de captura. La región de enfoque se determina por la pulsación del usuario en la pantalla que muestra la secuencia de vista previa.

En este ejemplo se usa un botón de radio para habilitar y deshabilitar el modo de pulsar para enfocar.

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

Comprueba si el dispositivo de captura actual admite el objeto **FocusControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785). Para poder usar esta técnica, **RegionsOfInterestControl** debe ser compatible y debe admitir al menos una región. Comprueba las propiedades [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066) y [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069) para determinar si se debe mostrar u ocultar el botón de radio de pulsar para enfocar.

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

En el controlador de eventos [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) del botón de radio de pulsar para enfocar, usa la propiedad [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) para obtener una instancia del control. Llama a [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) para bloquear el control en caso de que la aplicación haya llamado anteriormente a [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) para habilitar el autoenfoque continuo y luego espera a que el usuario pulse la pantalla para cambiar el enfoque.

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

Este ejemplo se centra en una región cuando el usuario pulsa la pantalla y, a continuación, quita el enfoque de esa región cuando el usuario pulsa de nuevo, como un botón de alternancia. Usa una variable booleana para realizar el seguimiento del estado actual de alternancia.

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

El siguiente paso es escuchar el evento cuando el usuario pulsa la pantalla, controlando el evento [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) de [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278), que está mostrando actualmente la secuencia de vista previa de captura. Si la cámara no muestra actualmente la vista previa o si el modo de pulsar para enfocar no está habilitado, regresa del controlador sin hacer nada.

Si la variable de seguimiento *\_isFocused* se pasó a false, y si la cámara no está actualmente en el proceso de enfoque (determinado por la propiedad [**FocusState**](https://msdn.microsoft.com/library/windows/apps/dn608074) de **FocusControl**), inicia el proceso de pulsar para enfocar. Obtén la posición de la pulsación del usuario a partir de los argumentos del evento pasados al controlador. En este ejemplo también se usa esta oportunidad para elegir el tamaño de la región que se enfocará. En este caso, el tamaño es 1/4 de la dimensión más pequeña del elemento de captura. Pasa la posición de pulsación y el tamaño de la región al método auxiliar **TapToFocus** que se define en la siguiente sección.

Si la variable *\_isFocused* está definida como true, la pulsación del usuario debe borrar el enfoque de la región anterior. Esto se realiza en el método auxiliar **TapUnfocus** que se muestra a continuación.

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

En el método auxiliar **TapToFocus**, primero define la variable *\_isFocused* como true para que la siguiente pulsación de pantalla libere el enfoque de la región en que se pulsó.

Es la siguiente tarea de este método auxiliar es determinar el rectángulo dentro de la secuencia de vista previa que se asignará al control de foco. Esto requiere dos pasos. El primer paso es determinar el rectángulo que la secuencia de vista previa ocupa dentro del control [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278). Esto depende de las dimensiones de la secuencia de vista previa y de la orientación del dispositivo. El método auxiliar **GetPreviewStreamRectInControl**, que se muestra al final de esta sección, realiza esta tarea y devuelve el rectángulo que contiene la secuencia de vista previa.

La siguiente tarea en **TapToFocus** es convertir la ubicación de pulsación y el tamaño del rectángulo de enfoque deseado, que se determinaron dentro del controlador de eventos **CaptureElement.Tapped** en coordenadas de la secuencia de captura. El método auxiliar **ConvertUiTapToPreviewRect**, que se muestra más adelante en esta sección, realiza esta conversión y devuelve el rectángulo, en coordenadas de la secuencia de captura, donde se solicitará el enfoque.

Ahora que se ha obtenido el rectángulo de destino, crea un nuevo objeto [**RegionOfInterest**](https://msdn.microsoft.com/library/windows/apps/dn279058) estableciendo la propiedad [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn279062) en el rectángulo de destino obtenido en los pasos anteriores.

Obtén el objeto [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) del dispositivo de captura. Crea un nuevo objeto [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) y establece el [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608076) y [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) en los valores deseados después de asegurarte de que son compatibles con el **FocusControl**. Llama a [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) en el **FocusControl** para activar la configuración e indicar al dispositivo que empiece a enfocarse en la región especificada.

A continuación, obtén el objeto [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) del dispositivo de captura y llama a [**SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070) para establecer la región activa. Se pueden establecer varias regiones de interés en los dispositivos que lo admiten, pero este ejemplo solo establece una sola región.

Por último, llama a [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) en el objeto **FocusControl** para iniciar el enfoque.

> [!IMPORTANT]
> Al implementar la pulsación para enfocar, el orden de las operaciones es importante. Debes llamar a estas API en el siguiente orden:
>
> 1. [**FocusControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)
> 3. [**FocusControl.FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

En el método auxiliar **TapUnfocus**, obtén el objeto **RegionsOfInterestControl** y llama a [**ClearRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279068) para borrar la región que se haya registrado con el control dentro del método auxiliar **TapToFocus**. A continuación, obtén el objeto **FocusControl** y llama a [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) para que el dispositivo se vuelva a enfocar sin una región de interés.

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

El método auxiliar **GetPreviewStreamRectInControl** usa la resolución de la secuencia de vista previa y la orientación del dispositivo para determinar el rectángulo dentro del elemento de vista previa que contiene la secuencia de vista previa, y recorta cualquier espaciado ancho que puede proporcionar el control para mantener la relación de aspecto de la secuencia. Este método usa variables de miembro de clase definidas en el código de ejemplo de captura básica de elementos multimedia que se encuentra en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

El método auxiliar **ConvertUiTapToPreviewRect** toma como argumentos la ubicación del evento de pulsación, el tamaño deseado de la región de enfoque y el rectángulo que contiene la secuencia de vista previa obtenida del método auxiliar **GetPreviewStreamRectInControl**. Este método usa estos valores y la orientación actual del dispositivo para calcular el rectángulo dentro de la secuencia de vista previa que contiene la región deseada. Una vez más, este método usa variables de miembro de clase definidas en el código de ejemplo de captura básica de elementos multimedia que se encuentra en [Capturar fotos y vídeo con MediaCapture](capture-photos-and-video-with-mediacapture.md).

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### <a name="manual-focus"></a>Enfoque manual

La técnica de enfoque manual usa un control **Slider** para definir la profundidad de enfoque actual del dispositivo de captura. Se usa un botón de radio para activar y desactivar el enfoque manual.

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

Comprueba si el dispositivo de captura actual admite el objeto **FocusControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función.

El valor de enfoque debe estar dentro del intervalo que admite el dispositivo y debe ser un incremento del tamaño de los pasos admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn297808), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn297802) y [**Step**](https://msdn.microsoft.com/library/windows/apps/dn297833), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual del objeto **FocusControl** después de anular el registro del controlador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que el evento no se desencadene cuando se define el valor.

[!code-cs[Focus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

En el controlador de eventos **Checked** del botón de radio de enfoque manual, obtén el objeto **FocusControl** y llama a [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) en caso de que la aplicación hubiera desbloqueado el enfoque anteriormente con una llamada a [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081).

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

En el controlador de eventos **ValueChanged** del control deslizante del enfoque manual, obtén el valor actual del control y define el valor de enfoque mediante una llamada a [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn297828).

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### <a name="enable-the-focus-light"></a>Habilitar la luz de enfoque

En los dispositivos que lo admiten, puedes habilitar una luz de enfoque auxiliar para ayudar al dispositivo en el proceso de enfoque. En este ejemplo, se usa una casilla para habilitar o deshabilitar la luz de enfoque auxiliar.

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

Comprueba si el dispositivo de captura actual admite **FlashControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785). Asimismo, comprueba [**AssistantLightSupported**](https://msdn.microsoft.com/library/windows/apps/dn608066) para asegurarte de que también se admite la luz auxiliar. Si se admiten ambos, puedes mostrar y habilitar la interfaz de usuario para esta función.

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

En el controlador de eventos **CheckedChanged**, obtén el objeto [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) del dispositivo de captura. Define la propiedad [**AssistantLightEnabled**](https://msdn.microsoft.com/library/windows/apps/dn608065) para habilitar o deshabilitar la luz de enfoque.

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## <a name="iso-speed"></a>Velocidad ISO

[**IsoSpeedControl**](https://msdn.microsoft.com/library/windows/apps/dn297850) te permite definir la velocidad ISO que se usa durante la captura de fotos o vídeo.

En este ejemplo se usa un control [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar el valor de compensación de exposición actual y una casilla para alternar el ajuste de velocidad ISO automática.

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

Comprueba si el dispositivo de captura actual admite **IsoSpeedControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297869). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Establece el estado de activación de la casilla para indicar si el ajuste de velocidad ISO automática está activo en el valor de la propiedad [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn608093).

El valor de velocidad ISO debe estar dentro del intervalo admitido por el dispositivo y debe ser un incremento del tamaño de paso admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn608095), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608094) y [**Step**](https://msdn.microsoft.com/library/windows/apps/dn608129), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual del **IsoSpeedControl** después de anular el registro del controlador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que el evento no se desencadene cuando se define el valor.

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor de velocidad ISO mediante una llamada a [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128).

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

En el controlador de eventos **CheckedChanged** de la casilla de velocidad ISO automática, activa el ajuste de velocidad ISO automática llamando a [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn608127). Para desactivar el ajuste velocidad ISO automática, llama a [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) y pasa el valor actual del control deslizante.

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## <a name="optical-image-stabilization"></a>Estabilización de imagen óptica

La estabilización de imagen óptica (OIS) estabiliza una secuencia de vídeo capturada manipulando mecánicamente el dispositivo de captura de hardware, lo que puede proporcionar un resultado superior a la estabilización digital. En los dispositivos que no admiten OIS, puedes usar el efecto VideoStabilizationEffect para realizar la estabilización digital de los vídeos capturados. Para más información, consulta [Efectos para captura de vídeo](effects-for-video-capture.md).

Para determinar si el dispositivo actual admite OIS, comprueba la propiedad [**OpticalImageStabilizationControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926689).

El control de estabilización de imagen óptica admite tres modos: activado, desactivado y automático, lo que significa que el dispositivo determina dinámicamente si OIS puede mejorar la captura multimedia y, si es así, habilita OIS. Para determinar si un dispositivo es compatible con un modo determinado, comprueba si la colección [**OpticalImageStabilizationControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926690) contiene el modo deseado.

Para habilitar o deshabilitar OIS, define [**OpticalImageStabilizationControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926691) en el modo deseado.

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## <a name="powerline-frequency"></a>Frecuencia de la línea de alimentación
Algunos dispositivos de cámara admiten el procesamiento de antiparpadeo que depende de conocer la frecuencia de la CA de las líneas de alimentación en el entorno actual. Algunos dispositivos admiten la determinación automática de la frecuencia de la línea de alimentación, mientras que otros requieren que la frecuencia se establezca de forma manual. El siguiente ejemplo de código muestra cómo determinar la compatibilidad de la frecuencia de la línea de alimentación en el dispositivo y, si es necesario, cómo establecer la frecuencia de forma manual. 

En primer lugar, llama al método **VideoDeviceController** [**TryGetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206898), pasando un parámetro de salida de tipo [**PowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.PowerlineFrequency); si se produce un error en esta llamada, el control de frecuencia de la línea de alimentación no se admite en el dispositivo actual. Si se admite la característica, puedes determinar si el modo automático está disponible en el dispositivo intentando establecer el modo automático. Para hacerlo, llama a [**TrySetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206899) y pasa el valor **Auto**. Si la llamada se realiza correctamente, eso significa que se admite la frecuencia de la línea de alimentación automática. Si en el dispositivo se admite el controlador de frecuencia de la línea de alimentación, pero no se admite la detección automática de la frecuencia, puedes establecer la frecuencia manualmente mediante el uso de **TrySetPowerlineFrequency**. En este ejemplo, **MyCustomFrequencyLookup** es un método personalizado que se implementa para determinar la frecuencia correcta para la ubicación actual del dispositivo. 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## <a name="white-balance"></a>Balance de blancos

[**WhiteBalanceControl**](https://msdn.microsoft.com/library/windows/apps/dn279104) te permite definir el balance de blancos que se usa durante la captura de fotos o vídeo.

En este ejemplo se usa un control [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) para seleccionar entre valores predeterminados de temperatura de color integrados y un control [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para el ajuste manual del balance de blancos.

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

Comprueba si el dispositivo de captura actual admite **WhiteBalanceControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279120). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función. Establece los elementos del cuadro combinado en los valores de la enumeración [**ColorTemperaturePreset**](https://msdn.microsoft.com/library/windows/apps/dn278894). También, establece el elemento seleccionado en el valor actual de la propiedad [**Preset**](https://msdn.microsoft.com/library/windows/apps/dn279110).

Para el control manual, el valor del balance de blancos debe estar dentro del intervalo admitido por el dispositivo y debe ser un incremento del tamaño de paso admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn279109), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn279107) y [**Step**](https://msdn.microsoft.com/library/windows/apps/dn279119), que se usan para establecer las propiedades correspondientes del control deslizante. Antes de habilitar el control manual, comprueba que el intervalo entre los valores mínimos y máximos admitidos es mayor que el tamaño de paso. Si no es así, no se admite el control manual en el dispositivo actual.

Define el valor del control deslizante con el valor actual de **WhiteBalanceControl** después de anular el registro del controlador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que el evento no se desencadene cuando se define el valor.

[!code-cs[WhiteBalance](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

En el controlador de eventos [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) del cuadro combinado de valores predeterminados de temperatura de color, obtén el valor predeterminado seleccionado actualmente y define el valor del control llamando a [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113). Si el valor predeterminado seleccionado no es **Manual**, deshabilita el control deslizante del balance de blancos manual.

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

En el controlador de eventos **ValueChanged**, obtén el valor actual del control y define el valor del balance de blancos mediante una llamada a [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927).

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> El ajuste del balance de blancos solo se admite mientras se ejecuta la secuencia de vista previa. Comprueba que la secuencia de vista previa se está ejecutando antes de establecer el valor del balance de blancos o el valor predeterminado.

> [!IMPORTANT]
> El valor predeterminado de **ColorTemperaturePreset.Auto** indica al sistema que ajuste automáticamente el nivel del balance de blancos. En algunos escenarios, como capturar una secuencia fotográfica donde los niveles de balance de blancos deben iguales para cada fotograma, te recomendamos que bloquees el control en el valor automático actual. Para ello, llama a [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) y especifica el valor predeterminado **Manual** y no establezcas un valor en el control mediante [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn279114). Esto hará que el dispositivo bloquee el valor actual. No intentes leer el valor actual del control y después pasar el valor obtenido a **SetValueAsync** porque no se garantiza que este valor sea correcto.

## <a name="zoom"></a>Zoom

El [**ZoomControl**](https://msdn.microsoft.com/library/windows/apps/dn608149) te permite establecer el nivel de zoom que se usa durante la captura de fotos o vídeo.

Este ejemplo usa un control [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) para ajustar el nivel de zoom actual. En la siguiente sección, se muestra cómo ajustar el zoom con un gesto de reducir en la pantalla.

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

Comprueba si el dispositivo de captura actual admite **ZoomControl** comprobando la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819). Si el control se admite, puedes mostrar y habilitar la interfaz de usuario para esta función.

El valor del nivel de zoom debe estar dentro del intervalo que admite el dispositivo y debe ser un incremento del tamaño de los pasos admitido. Para obtener los valores admitidos para el dispositivo actual, comprueba las propiedades [**Min**](https://msdn.microsoft.com/library/windows/apps/dn633817), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) y [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818), que se usan para establecer las propiedades correspondientes del control deslizante.

Define el valor del control deslizante con el valor actual del objeto **ZoomControl** después de anular el registro del controlador de eventos [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) para que el evento no se desencadene cuando se define el valor.

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

En el controlador de eventos **ValueChanged**, crea una nueva instancia de la clase [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) y define la propiedad [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) con el valor actual del control deslizante de zoom. Si la propiedad [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) del **ZoomControl** contiene [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726), significa que el dispositivo admite transiciones suaves entre niveles de zoom. Dado que este modo proporciona una mejor experiencia del usuario, normalmente querrás usar este valor para la propiedad [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) del objeto **ZoomSettings**.

Por último, cambia la configuración de zoom actual al pasar el objeto **ZoomSettings** al método [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) del objeto **ZoomControl**.

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### <a name="smooth-zoom-using-pinch-gesture"></a>Zoom suave con el gesto de reducir

Como se analizó en la sección anterior, en los dispositivos que lo admiten, el modo de zoom suave permite que el dispositivo de captura cambie sin inconvenientes entre niveles de zoom digital, lo que permite al usuario ajustar el nivel de zoom dinámicamente durante la operación de captura sin transiciones discretas y un poco molestas. En esta sección se describe cómo ajustar el nivel de zoom en respuesta a un gesto de reducir.

Primero, determine si el control de zoom digital es compatible en el dispositivo actual comprobando la propiedad [**ZoomControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819). A continuación, determina si el modo de zoom suave está disponible mediante la comprobación de [**ZoomControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) para ver si contiene el valor [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726).

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

En un dispositivo habilitado para funcionalidad multitáctil, un escenario típico consiste en ajustar el factor de zoom con un gesto de reducir con dos dedos. Establezca la propiedad [**ManipulationMode**](https://msdn.microsoft.com/library/windows/apps/br208948) del control [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) en [**ManipulationModes.Scale**](https://msdn.microsoft.com/library/windows/apps/br227934) para permitir que el gesto de reducir. A continuación, registra el evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) que se desencadena cuando el gesto de reducir cambia el tamaño.

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

En el controlador del evento **ManipulationDelta**, actualiza el factor de zoom en función de los cambios en el gesto de reducir del usuario. El valor [**ManipulationDelta.Scale**](https://msdn.microsoft.com/library/windows/apps/br242016) representa el cambio en escala del gesto de reducir, de modo que un pequeño incremento en el tamaño del gesto de reducir es un número ligeramente mayor que 1.0 y una pequeña reducción es un número ligeramente menor que 1.0. En este ejemplo, el valor actual del control de zoom se multiplica por la diferencia de la escala.

Antes de establecer el factor de zoom, debes asegurarte de que el valor no es inferior al valor mínimo que admite el dispositivo, como lo indica la propiedad [**ZoomControl.Min**](https://msdn.microsoft.com/library/windows/apps/dn633817). Además, asegúrate de que el valor es menor o igual al valor de [**ZoomControl.Max**](https://msdn.microsoft.com/library/windows/apps/dn608150). Por último, debes asegurarte de que el factor de zoom es un múltiplo del tamaño de paso de zoom admitido por el dispositivo, según se indica en la propiedad [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818). Si el factor de zoom no cumple estos requisitos, se generará una excepción cuando intentes establecer el nivel de zoom en el dispositivo de captura.

Para establecer el nivel de zoom en el dispositivo de captura, crea un nuevo objeto [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722). Establece la propiedad [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) en [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726) y, a continuación, establece la propiedad [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) en el factor de zoom deseado. Por último, llama a [**ZoomControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) para definir el nuevo valor de zoom en el dispositivo. El dispositivo pasará sin problemas al nuevo valor de zoom.

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)

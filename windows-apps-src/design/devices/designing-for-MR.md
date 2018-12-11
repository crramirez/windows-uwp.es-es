---
Description: Design your app so that it looks good and functions well in Mixed Reality.
title: Diseñar para realidad mixta
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Realidad mixta, HoloLens, realidad aumentada, mirada, voz, controlador
ms.date: 2/5/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: e6aebac45dc32933f55d917c0b1153cba952d819
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874354"
---
# <a name="designing-for-mixed-reality"></a>Diseñar para realidad mixta

Diseña tu aplicación para que se vea bien en realidad mixta y provecha los nuevos métodos de entrada.

## <a name="overview"></a>Introducción

La [realidad mixta](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) es el resultado de fusionar el mundo físico con el mundo digital. El espectro de experiencias con realidad mixta incluye, por un lado, dispositivos como HoloLens (un dispositivo que mezcla contenido generado por equipos con el mundo real) y, por otro lado, una vista completamente envolvente de realidad virtual (tal y como se ve con unos cascos de Windows Mixed Reality). Consulta [Tipos de aplicaciones de realidad mixta](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps) para tener ejemplos sobre cómo varían las experiencias.

Casi todas las actuales aplicaciones para UWP se ejecutarán en el entorno de realidad mixta como aplicaciones 2D sin ningún cambio, aunque la experiencia del usuario podría mejorar si se siguiesen algunas de las directrices recogidas en este tema.

![Vista de realidad mixta](images/MR-01.png)

Tanto HoloLens como los cascos de Windows Mixed Reality son compatibles con aplicaciones que se ejecutan en la plataforma UWP y admiten dos tipos diferentes de experiencia. 

### <a name="2d-vs-immersive-experience"></a>2D frente a la experiencia envolvente

Una aplicación envolvente ocupa toda la pantalla visible para el usuario y la coloca en el centro de una vista creada por la aplicación. Por ejemplo, un juego envolvente podría llevar al usuario a la superficie de un planeta alienígena o una aplicación turística podría llevar al usuario a un pueblo de América del Sur. Crear una aplicación envolvente requiere gráficos 3D o vídeos estereográficos capturados. Las aplicaciones envolventes suelen desarrollarse a partir de un motor de juego de terceros, como Unity o bien con DirectX.

Si vas a crear aplicaciones envolventes, conviene que visites el [Centro de desarrollo de Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) para obtener más información.

Una aplicación 2D se ejecuta como una ventana tradicional plana dentro de la vista del usuario. En HoloLens, esto da lugar a una vista anclada a la pared o a un punto en el espacio en la propia sala de estar u oficina reales del usuario. En unos cascos de Windows Mixed Reality, la aplicación está anclada a una pared en la [casa de realidad mixta](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) (en ocasiones llamada la *casa sobre el acantilado*).

![Varias aplicaciones ejecutándose en realidad mixta](images/MR-multiple.png)


Estas aplicaciones 2D no ocupan toda la vista sino que están dentro de ella. Pueden existir varias aplicaciones 2D a la vez en el entorno.

El resto de este tema describe las consideraciones del diseño en cuanto a la experiencia 2D.

## <a name="launching-2d-apps"></a>Iniciar aplicaciones 2D

![Menú de inicio de realidad mixta](images/MR-start-options.png)

Todas las aplicaciones se inician desde el menú Inicio, pero también es posible crear un objeto 3D para que actúe como un selector de aplicaciones. Consulta [este vídeo](https://www.youtube.com/watch?v=TxIslHsEXno) para obtener más información.

## <a name="the-2d-app-input-overview"></a>Introducción a la entrada de aplicaciones 2D

Las plataformas HoloLens y de realidad mixta admiten teclados y dispositivos de mouse. Puedes emparejar un teclado y un mouse directamente con HoloLens por Bluetooth. Las aplicaciones de realidad mixta son compatibles con el mouse y teclado conectados al equipo host. Ambos pueden ser útiles en situaciones en las que se necesite un nivel preciso de control.

También son compatibles con otros métodos de entrada más naturales y estos pueden ser especialmente útiles cuando el usuario no está sentado en un escritorio con un teclado real frente a él, o cuando se necesita un control preciso.

Sin ninguna codificación ni hardware adicionales, las aplicaciones usarán la función de la mirada -el vector que tu usuario esté viendo- como un puntero del mouse trabajando con aplicaciones 2D. Se implementa como si el puntero del mouse estuviese sobre algo en la escena virtual.

En una interacción típica, tu usuario mirará un control en tu aplicación y hará que este resalte. El usuario activará una acción, ya sea con un gesto (en HoloLens), un controlador o un comando de voz. Si el usuario selecciona un campo de entrada de texto, aparecerá el teclado del software. 


![El teclado emergente en realidad mixta](images/MR-keyboard.png)

Es importante tener en cuenta que todas estas interacciones tendrán lugar automáticamente sin ninguna codificación adicional por tu parte. Todo ello se debe al hecho de llevar a cabo la ejecución en la plataforma UWP. La entrada desde HoloLens y cascos de realidad mixta aparecerá como entrada táctil en la aplicación 2D. Esto significa que muchas aplicaciones para UWP se ejecutarán y usarán en realidad mixta de manera predeterminada. 

Dicho esto, con algo más de trabajo, la experiencia puede mejorarse de forma significativa. Por ejemplo, el [control de voz](https://developer.microsoft.com/windows/mixed-reality/voice_design) puede ser especialmente eficaz. Los entornos de HoloLens y de realidad mixta son compatibles con comandos de voz para iniciar e interactuar con aplicaciones, y el hecho de incluir soporte de voz aparecerá como una extensión natural de este enfoque. Consulta las [interacciones de voz]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions) para obtener más información sobre cómo agregar soporte de voz a tu aplicación para UWP. 


### <a name="selecting-the-right-controller"></a>Seleccionar el controlador adecuado

![Controladores de movimiento de realidad mixta](images/MR-controllers.png)

Se han diseñado novedosos métodos de entrada especialmente para usarlos con realidad mixta, en particular:

* [Gestos con las manos](https://developer.microsoft.com/windows/mixed-reality/gestures) (solo HoloLens, pero solo se usan para iniciar aplicaciones 2D)
* [Soporte de controlador para juegos](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (ambos entornos)
* [Dispositivo de control de presentaciones](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (solo HoloLens)
* [Controladores de movimiento](https://developer.microsoft.com/windows/mixed-reality/motion_controllers) (solo dispositivos de realidad mixta, mostrados anteriormente)

Estos controladores hacen que interactuar con objetos virtuales parezca natural y preciso. Algunas de las interacciones que consigues de forma gratuita. Por ejemplo, HoloLens selecciona el gesto o al hacer clic en la tecla de Windows del controlador de movimiento o un desencadenador, se generará la respuesta de entrada que esperas, nuevamente, sin ninguna codificación de tu parte.

En otras ocasiones querrás agregar código para aprovechar la información adicional y las entradas disponibles. Por ejemplo, los controladores de movimiento pueden usarse para manipular los objetos con un nivel preciso de control, si escribes código que tenga en cuenta su posición y pulsaciones de botones.

> [!NOTE]
> En resumen, los principios fundamentales siempre deben ser proporcionar al usuario un método de entrada lo más natural y en armonía posible.


## <a name="2d-app-design-considerations-functionality"></a>Consideraciones del diseño de aplicaciones 2D: funcionalidad

Deberás tener varias cosas en cuenta en el momento de crear una aplicación para UWP que se usará potencialmente en una plataforma de realidad mixta.

* Puede que las funciones de arrastrar y soltar no funcionen correctamente cuando se usen con los controladores de movimiento, controladores para juegos o gestos. Si tu aplicación depende en gran medida de las funciones de arrastrar y soltar, deberás proporcionar un método alternativo que sea compatible con estas acciones, como presentar un cuadro de diálogo que confirme si los objetos se mueven a una nueva ubicación.

* Ten en cuenta cómo cambia el sonido. Si la aplicación genera efectos de sonido, el origen del sonido aparecerá como ubicación anclada de tu aplicación en el mundo virtual. Cuando el usuario se desplace fuera de la aplicación, el sonido disminuirá. Consulta [Sonido espacial](https://developer.microsoft.com/windows/mixed-reality/spatial_sound) para obtener más información.

* Ten en cuenta el campo de visión y proporciona prestaciones. No todos los dispositivos proporcionarán un campo de visión tan grande como el monitor del ordenador. Consulta [Fotograma holográfico](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) para obtener información detallada. Además, el usuario puede estar a cierta distancia de una aplicación en ejecución. Es decir, puede que la aplicación aparezca anclada a la pared en una ubicación diferente en el mundo (real o virtual). Puede que tu aplicación necesite llamar la atención de los usuarios, o bien ten en cuenta que la vista completa no siempre estará visible. Las notificaciones del sistema están disponibles, pero otra forma de conseguir la atención del usuario podría ser generar un sonido o una alerta [hablada](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs).

* Una aplicación 2D recibe automáticamente una [barra de la aplicación](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box) para dejar que el usuario se mueva y escale por el entorno virtual. Puede cambiarse el tamaño vertical de las vistas o bien cambiar el tamaño mantenimiento la misma relación de aspecto.


## <a name="2d-app-design-considerations-uiux"></a>Consideraciones del diseño de aplicaciones 2D: interfaz del usuario/experiencia del usuario

* Controles de XAML que implementan el [sistema Fluent Design](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/) como la [vista de navegación](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) y los efectos como [Acrylic](https://docs.microsoft.com/windows/uwp/design/style/acrylic) funcionan especialmente bien en aplicaciones 2D de realidad mixta.

* Comprueba el tamaño del texto y de Windows de tu aplicación en un dispositivo de realidad mixta o al menos en el simulador de realidad mixta. Tu aplicación tendrá un tamaño de Windows predeterminado de 480x853 píxeles efectivos. Usa un tamaño de fuente más grande (se recomienda un tamaño de aproximadamente 32) y lee [Actualizar tu aplicación universal existente para HoloLens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). El artículo [Tipografía](https://developer.microsoft.com/windows/mixed-reality/typography) trata este tema en detalle. Cuando trabajes en Visual Studio, dispondrás de una configuración de editor de diseño XAML para una aplicación 2D HoloLens de 57" que te proporcionará una vista con la escala y las dimensiones correctas. 

![El texto que se muestre en las aplicaciones de realidad mixta debe ser grande.](images/MR-text.png)

* [Tu mirada es tu mouse](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Cuando el usuario mire algo, actuará como un evento **activable con entrada táctil**, por lo tanto, con tan solo mirar un objeto, podría activarse un elemento emergente involuntario u otra interacción no deseada. Puede que tengas que detectar si la aplicación se está ejecutando en realidad mixta y cambiar este comportamiento. Consulta **Soporte para tiempo de ejecución** a continuación. 

* Cuando un usuario mire algo o apunte con un controlador de movimiento, tendrá lugar un evento **activable con entrada táctil**. Se trata de un **PointerPoint** donde **PointerType** es **Touch**, pero **IsInContact** es **false**. Cuando tenga lugar algún formulario de confirmación (por ejemplo, cuando se presione un botón controlador para juegos, se presione un control para presentación, se presione un gatillo controlador de movimiento o el reconocimiento de voz se dirija a "Seleccionar"), tendrá lugar una **presión táctil**, donde **PointerPoint** tendrá **IsInContact** convertido en **true**. Consulta [Interacciones táctiles](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions) para obtener más información sobre estos eventos de entrada.

* Recuerda que la mirada no es tan precisa como señalar con el mouse. Los objetivos o botones del mouse más pequeños podrían frustrar a tus usuarios, así que cambia el tamaño de los controles según corresponda. Si se han diseñado para la función táctil, funcionarán en realidad mixta, pero puedes decidir aumentar el tamaño de algunos botones en tiempo de ejecución. Consulta [Actualizar tu aplicación universal existente para Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* HoloLens define el color negro como la ausencia de luz. Simplemente no se representa y permite así que se muestre el "mundo real". Tu aplicación no debería usar el negro si esto causa confusión. En unos cascos de realidad mixta, el negro es negro.

* HoloLens no admite temas de color en las aplicaciones y predetermina el azul para garantizar la mejor experiencia para los usuarios. Para obtener más información sobre la selección de colores, debes consultar [este tema](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials) que trata el uso del color y el material en diseños de realidad mixta.


## <a name="other-points-to-consider"></a>Otros puntos que considerar

* Aunque el [puente de dispositivo de escritorio](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) puede ayudarte a llevar aplicaciones de escritorio existentes (Win32) a Windows 10 y a la Microsoft Store, no puede crear aplicaciones que se ejecuten en HoloLens o en realidad mixta en este momento.




## <a name="runtime-support"></a>Soporte para tiempo de ejecución

Es posible que tu aplicación determine si se está ejecutando en un dispositivo de realidad mixta en tiempo de ejecución y aprovecharlo como una oportunidad para cambiar el tamaño de los controles o, de otro modo, optimizar la aplicación para su uso en unos cascos.

He aquí un breve fragmento de código que cambia de tamaño el texto dentro de un control TextBlock de XAML únicamente si se usa la aplicación en un dispositivo de realidad mixta.

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>Artículos relacionados


* [Limitaciones actuales para aplicaciones que usan API desde el shell](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Crear aplicaciones 2D](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: crear aplicaciones 2D para UWP para Microsoft HoloLens](https://channel9.msdn.com/Events/Build/2016/B854)
* [XAML condicional](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)



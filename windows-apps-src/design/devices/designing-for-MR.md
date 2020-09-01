---
Description: Diseñe la aplicación para que tenga un aspecto correcto y funcione bien en realidad mixta.
title: Diseño para la realidad mixta
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Realidad mixta, Hololens, realidad aumentada, mira fijamente, voz, controlador
ms.date: 02/05/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: c0b1ae069959e74239234ae5c9ba409fe7a65f23
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165979"
---
# <a name="designing-for-mixed-reality"></a>Diseño para la realidad mixta

Diseña tu aplicación para que tenga un buen aspecto en realidad mixta y aprovecha los nuevos métodos de entrada.

## <a name="overview"></a>Información general

La [realidad mixta](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) es el resultado de mezclar el mundo físico con el mundo digital. El espectro de experiencias de realidad mixta incluye en un dispositivo extremo, como HoloLens (un dispositivo que combina contenido generado por el equipo con el mundo real) y, en el otro, una vista totalmente envolvente de la realidad virtual (como se ve con un casco de la realidad mixta de Windows). Vea [tipos de aplicaciones de realidad mixta](https://developer.microsoft.com/windows/mixed-reality/types_of_mixed_reality_apps) para ver ejemplos de cómo variarán las experiencias.

Casi todas las aplicaciones UWP existentes se ejecutarán en el entorno de realidad mixta como aplicaciones 2D sin cambios, aunque la experiencia del usuario se puede mejorar siguiendo algunas de las instrucciones de este tema.

![Vista de realidad mixta](images/MR-01.png)

Los auriculares HoloLens y Windows Mixed Reality admiten aplicaciones que se ejecutan en la plataforma UWP y ambos admiten dos tipos distintos de experiencia. 

### <a name="2d-vs-immersive-experience"></a>Experiencia en 2D frente a la experiencia envolvente

Una aplicación envolvente usa toda la presentación visible para el usuario y la coloca en el centro de una vista creada por la aplicación. Por ejemplo, un juego envolvente podría poner al usuario en la superficie de un planeta extranjero, o una aplicación de guía de paseo podría poner al usuario en un pueblo de Sudamérica. La creación de una aplicación envolvente requiere gráficos 3D o vídeo Stereographic capturado. Las aplicaciones envolventes se suelen desarrollar con un motor de juegos de terceros, como Unity o con DirectX.

Si va a crear aplicaciones envolventes, visite el [centro de desarrollo de la realidad mixta de Windows](https://developer.microsoft.com/mixed-reality) para obtener más información.

Una aplicación 2D se ejecuta como una ventana plana tradicional dentro de la vista del usuario. En HoloLens, es decir, una vista anclada a la pared o un punto en el espacio de los usuarios en la sala de reuniones o la oficina del mundo real. En un casco de realidad mixta de Windows, la aplicación está anclada a una pared en la [Página principal de realidad mixta](/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) (a veces denominada *casa del acantilado*).

![Varias aplicaciones que se ejecutan en realidad mixta](images/MR-multiple.png)


Estas aplicaciones 2D no asumen toda la vista: se colocan en ella. Puede haber varias aplicaciones 2D en el entorno a la vez.

En el resto de este tema se describen las consideraciones de diseño para la experiencia 2D.

## <a name="launching-2d-apps"></a>Inicio de aplicaciones 2D

![Menú Inicio de realidad mixta](images/MR-start-options.png)

Todas las aplicaciones se inician desde el menú Inicio, pero también es posible crear un objeto 3D para que actúe como un iniciador de aplicaciones. Vea [este vídeo](https://www.youtube.com/watch?v=TxIslHsEXno) para obtener más información.

## <a name="the-2d-app-input-overview"></a>Introducción a la entrada de la aplicación 2D

Los teclados y ratones se admiten en las plataformas HoloLens y Mixed Reality. Puede emparejar un teclado y un mouse directamente con HoloLens a través de Bluetooth. Las aplicaciones de realidad mixta admiten el mouse y el teclado conectados al equipo host. Ambos pueden ser útiles en situaciones en las que es necesario un nivel de control preciso.

También se admiten otros métodos de entrada, más naturales, y pueden ser especialmente útiles cuando el usuario no está sentado en un escritorio con un teclado real delante o cuando se necesita un control preciso.

Sin ningún hardware o codificación adicional, las aplicaciones usarán fijamente: el vector que busca el usuario, como puntero del mouse al trabajar con aplicaciones 2D. Se implementa como si el puntero del mouse se desplace sobre algo de la escena virtual.

En una interacción típica, el usuario observará un control en la aplicación, lo que hará que se resalte. El usuario iniciará una acción, mediante un gesto (en HoloLens) o un contratador, o bien proporcionando un comando de voz. Si el usuario selecciona un campo de entrada de texto, aparecerá el teclado del software. 


![Teclado emergente en realidad mixta](images/MR-keyboard.png)

Es importante tener en cuenta que todas estas interacciones se realizarán automáticamente sin codificación adicional por su parte, como consecuencia de la ejecución en la plataforma de UWP. La entrada de los auriculares HoloLens y de realidad mixta aparecerá como entrada táctil en la aplicación 2D. Esto significa que muchas aplicaciones de UWP se ejecutarán y se podrán usar en la realidad mixta de forma predeterminada. 

Dicho esto, con algún trabajo adicional, la experiencia puede mejorar en gran medida. Por ejemplo, el [control de voz](https://developer.microsoft.com/windows/mixed-reality/voice_design) puede ser especialmente eficaz. Tanto HoloLens como entornos de realidad mixta admiten comandos de voz para iniciar e interactuar con aplicaciones, y la compatibilidad con voz aparecerá como una extensión natural de este enfoque. Consulte [interacciones de voz]( ../input/speech-interactions.md) para más información sobre cómo agregar compatibilidad con voz a la aplicación para UWP. 


### <a name="selecting-the-right-controller"></a>Seleccionar el controlador adecuado

![Controladores de movimiento de realidad mixta](images/MR-controllers.png)

Varios métodos de entrada nuevos se han diseñado especialmente para su uso con la realidad mixta, en concreto:

* [Gestos de mano](https://developer.microsoft.com/windows/mixed-reality/gestures) (solo HoloLens, pero solo se usa para iniciar aplicaciones 2D)
* [Compatibilidad con el controlador de juegos](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (ambos entornos)
* [Dispositivo de clic](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (solo HoloLens)
* [Controladores de movimiento](/windows/mixed-reality/motion-controllers) (solo dispositivos de realidad mixta, mostrados anteriormente).

Estos controladores hacen que la interacción con objetos virtuales parezca natural y precisa. Algunas de las interacciones que obtendrá de forma gratuita. Por ejemplo, el gesto de selección de HoloLens o al hacer clic en la tecla o el desencadenador de Windows del controlador de movimiento generará la respuesta de entrada que cabría esperar, de nuevo, sin necesidad de codificación por su parte.

En otras ocasiones, querrá agregar código para aprovechar las ventajas de la información adicional y las entradas que están disponibles. Por ejemplo, los controladores de movimiento se pueden usar para manipular objetos con un nivel de control fino, si escribe código que tenga en cuenta la posición y las pulsaciones de botones.

> [!NOTE]
> En Resumen: la entidad de seguridad de GUID debe ser proporcionar siempre al usuario como un método de entrada natural y sin problemas como sea posible.


## <a name="2d-app-design-considerations-functionality"></a>consideraciones sobre el diseño de aplicaciones en 2D: funcionalidad

Al crear una aplicación para UWP que se usará potencialmente en una plataforma de realidad mixta, hay varios aspectos que hay que tener en cuenta.

* La operación de arrastrar y colocar no funciona bien cuando se usa con controladores de movimiento, controladores de juegos o gestos. Si su aplicación depende en gran medida de arrastrar y colocar, deberá proporcionar un método alternativo para admitir esta acción, como presentar un cuadro de diálogo que confirma si los objetos se van a pasar a una nueva ubicación.

* Tenga en cuenta cómo cambia el sonido. Si la aplicación genera efectos de sonido, el origen del sonido parecerá ser la ubicación anclada de la aplicación en el mundo virtual. Cuando el usuario sale de la aplicación, el sonido disminuirá. Consulte [sonido espacial](/windows/mixed-reality/spatial-sound) para obtener más información.

* Tenga en cuenta el campo de vista y proporcione prestaciones. No todos los dispositivos proporcionarán un campo de vista tan grande como monitor del equipo. Consulte el [marco holográfica](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) para obtener detalles completos. Además, el usuario puede estar lejos de una aplicación en ejecución. Es decir, la aplicación puede aparecer anclada a la pared en una ubicación diferente del mundo (real o virtual). Es posible que la aplicación necesite tener la atención de los usuarios o tener en cuenta que toda la vista no es visible en todo momento. Las notificaciones del sistema están disponibles, pero otra manera de obtener la atención del usuario puede ser generar una alerta de sonido o de [voz](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs) .

* A una aplicación de 2D se le asigna automáticamente una [barra de aplicación](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box)  para que el usuario pueda moverla y escalarla en el entorno virtual. Se puede cambiar el tamaño de las vistas verticalmente o cambiar su tamaño manteniendo la misma relación de aspecto.


## <a name="2d-app-design-considerations-uiux"></a>consideraciones sobre el diseño de aplicaciones en 2D: UI/UX

* Los controles XAML que implementan el [sistema de diseño fluida](/windows/uwp/design/fluent-design-system/) , como la [vista de navegación](../controls-and-patterns/navigationview.md), y efectos como [acrílico](../style/acrylic.md) funcionan especialmente bien en aplicaciones de realidad mixta 2D.

* Pruebe el tamaño de Windows y el texto de la aplicación en un dispositivo de realidad mixta o, al menos, en el simulador de realidad mixta. La aplicación tendrá un tamaño de Windows predeterminado de 853x480 píxeles efectivos. Use tamaños de fuente mayores (se recomienda un tamaño de punto de aproximadamente 32) y lea [actualización de la aplicación universal existente para Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). En la [tipografía](https://developer.microsoft.com/windows/mixed-reality/typography) del artículo se trata en detalle este tema. Al trabajar en Visual Studio, hay una configuración de editor de diseño XAML para una aplicación de 57 "HoloLens 2D que proporciona una vista con la escala y las dimensiones correctas. 

![El texto que se muestra en las aplicaciones de realidad mixta debe ser grande.](images/MR-text.png)

* [Su mirada es el mouse](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Cuando el usuario mira algo, actúa como un evento **Touch Hover** , por lo que simplemente examinar un objeto puede desencadenar una interacción emergente involuntaria u otra interacción no deseada. Es posible que necesite detectar si la aplicación se está ejecutando actualmente en la realidad mixta y cambiar este comportamiento. Vea **compatibilidad con tiempo de ejecución**, a continuación. 

* Cuando un usuario mira algo o apunta con un controlador de movimiento, se producirá un evento de **desplazamiento táctil** . Se compone de un **PointerPoint** donde **PointerType** es **Touch**, pero **IsInContact** es **false**. Cuando se produce alguna forma de confirmación (por ejemplo, se presiona un botón A un botón, se presiona un dispositivo de clic, se presiona un desencadenador de control de movimiento o se "selecciona" la cabeza de reconocimiento de voz), se produce una **pulsación táctil** , con el **PointerPoint** que tiene **IsInContact** de ser **true**. Consulte [interacciones táctiles](../input/touch-interactions.md) para obtener más información sobre estos eventos de entrada.

* Recuerde que la mirada no es tan precisa como el puntero del mouse. Los botones o los destinos del mouse más pequeños pueden causar frustración a los usuarios, por lo que cambiar el tamaño de los controles en consecuencia. Si están diseñados para tocar, funcionarán en una realidad mixta, pero puede que decida ampliar algunos botones en tiempo de ejecución. Consulte [actualización de la aplicación universal existente para Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* HoloLens define el color negro como ausencia de luz. Simplemente no se representa, lo que permite que el mundo real se muestre. La aplicación no debe usar negro Si esto causará confusión. En un casco de realidad mixta, el negro es negro.

* HoloLens no admite temas de color en las aplicaciones y su valor predeterminado es azul para garantizar la mejor experiencia para los usuarios. Para obtener más consejos sobre la selección de colores, debe consultar [este tema](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials) , en el que se explica el uso de color y material en los diseños de realidad mixta.


## <a name="other-points-to-consider"></a>Otros puntos a tener en cuenta

* Aunque el [puente de escritorio](/windows/msix/desktop/source-code-overview) puede ayudar a realizar aplicaciones de escritorio existentes (Win32) en Windows 10 y en el Microsoft Store, no puede crear aplicaciones que se ejecuten en HoloLens o en realidad mixta en este momento.




## <a name="runtime-support"></a>Compatibilidad con el tiempo de ejecución

Es posible que la aplicación determine si se está ejecutando en un dispositivo de realidad mixta en tiempo de ejecución, y usarlo como una oportunidad para cambiar el tamaño de los controles u otras maneras de optimizar la aplicación para su uso en un casco.

A continuación se muestra una breve parte del código que cambia el tamaño del texto dentro de un control TextBlock de XAML solo si la aplicación se usa en un dispositivo de realidad mixta.

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
                textBlock.FontSize = 14;
            }

```





## <a name="related-articles"></a>Artículos relacionados


* [Limitaciones actuales de las aplicaciones que usan las API del shell](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Compilación de aplicaciones 2D](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: creación de aplicaciones para UWP en 2D para Microsoft HoloLens](https://channel9.msdn.com/Events/Build/2016/B854)
* [XAML condicional](../../debug-test-perf/conditional-xaml.md)
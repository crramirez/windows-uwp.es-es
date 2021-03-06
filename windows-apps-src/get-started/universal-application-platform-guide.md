---
title: ¿Qué es una aplicación para la Plataforma universal de Windows (UWP)?
description: Obtén información sobre las aplicaciones para la Plataforma universal de Windows (UWP) que se pueden ejecutar en una amplia variedad de dispositivos que ejecutan Windows 10.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 09/15/2020
ms.topic: article
ms.custom: contperfq1
keywords: Windows 10, UWP, universal
ms.localizationpriority: medium
ms.openlocfilehash: 3c6fd1f2ebee2e4e3a7d3ceb028f5def19e932c6
ms.sourcegitcommit: 733d422fa1e3433cde842e2f200d3ef202767cc1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94333290"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>¿Qué es una aplicación para la Plataforma universal de Windows (UWP)?

UWP es una de las muchas maneras de crear aplicaciones cliente para Windows. Las aplicaciones UWP usan las API de WinRT para proporcionar características de interfaz de usuario avanzadas y asincrónicas eficaces que son ideales para dispositivos conectados a Internet.

Para descargar las herramientas necesarias para comenzar a crear aplicaciones para UWP, consulte [Preparación](/windows/apps/get-set-up.md) y, después, [escriba su primera aplicación](your-first-app.md).


## <a name="where-does-uwp-fit-in-the-microsoft-development-story"></a>¿Qué papel desempeña UWP en la historia de desarrollo de Microsoft?

UWP es una opción para crear aplicaciones que se ejecutan en dispositivos Windows 10 y se puede combinar con otras plataformas. Las aplicaciones para UWP pueden usar las API de Win32 y las clases .NET (consulte los artículos [Conjuntos de API para aplicaciones para UWP](/previous-versions/mt186421(v=vs.85)), [DLL para aplicaciones para UWP](/previous-versions/mt186422(v=vs.85)) y [.NET para aplicaciones para UWP](/dotnet/api/index?view=dotnet-uwp-10.0)).

La historia de desarrollo de Microsoft continúa evolucionando y, junto con iniciativas como [WinUI](/windows/apps/winui/), [MSIX](/windows/msix/) y [Project Reunion](https://github.com/microsoft/ProjectReunion), UWP es una herramienta eficaz para crear aplicaciones cliente.


## <a name="features-of-a-uwp-app"></a>Características de una aplicación para UWP

Una aplicación para UWP es:

- Segura: Las aplicaciones para UWP declaran cuáles son los recursos y datos de dispositivos a los que acceden. El usuario debe autorizar ese acceso.
- Puede usar una API común en todos los dispositivos que ejecutan Windows 10.
- Puede usar funcionalidades específicas del dispositivo y adaptar la interfaz de usuario para los tamaños de pantalla de otro dispositivo, resoluciones y ppp.
- Disponible desde Microsoft Store en todos los dispositivos (o solo aquellos que especifique) que se ejecutan en Windows 10. Microsoft Store proporciona varias maneras de ganar dinero en tu aplicación.
- Puede instalarse y desinstalarse sin riesgo para la máquina ni incurrir en "máquina rot".
- Atractiva: usa iconos dinámicos, notificaciones push y actividades del usuario que interactúan con la línea de tiempo de Windows y la función Continuar donde lo dejé de Cortana para atraer a los usuarios.
- Programable en C#, C++, Visual Basic y Javascript. Para la interfaz de usuario, use WinUI, XAML, HTML o DirectX.

Veamos esto con más detalle.

### <a name="secure"></a>Segura

La aplicaciones para UWP declaran en el manifiesto las funcionalidades del dispositivo que necesitan, como el acceso al micrófono, la ubicación, la cámara web, los dispositivos USB, los archivos, etc. El usuario debe confirmar y autorizar ese acceso antes de que la aplicación obtenga la funcionalidad.

### <a name="a-common-api-surface-across-all-devices"></a>Una superficie de API común para todos los dispositivos

Windows 10 presenta la Plataforma universal de Windows (UWP), la cual proporciona una plataforma común de aplicaciones en todos los dispositivos que ejecutan Windows 10. Las API principales de la UWP son las mismas en todos los dispositivos de Windows. Si su aplicación usa solo las API principales, se ejecutará en cualquier dispositivo de Windows 10, independientemente de si es un equipo de escritorio, una Xbox, un casco de realidad mixta, etc.

Una aplicación para UWP escrita en C++ /WinRT o C++ /CX tiene acceso a las API de Win32 que forman parte de la UWP. Todas los dispositivos de Windows 10 implementan estas API de Win32.

### <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Las SDK de extensión exponen las funcionalidades exclusivas de tipos específicos de dispositivos

Si su destino son las API universales, la aplicación puede ejecutarse en todos los dispositivos con Windows 10. Pero si quiere que su aplicación para UWP aproveche las API específicas del dispositivo, también puede hacerlo.

Las SDK de extensión le permiten llamar a las API especializadas para diferentes dispositivos. Por ejemplo, si su aplicación para UWP está destinada a un dispositivo de IoT, puede agregar la SDK de extensión IoT a su proyecto para dirigirse a características específicas de los dispositivos IoT. Para obtener más información sobre cómo agregar SDK de extensión, consulte la sección **SDK de extensión** en [Programación con los SDK de extensión](/uwp/extension-sdks/device-families-overvieww#extension-sdks).

Puede escribir su aplicación para que se ejecute solo en un determinado tipo de dispositivo y, a continuación, limitar su distribución desde Microsoft Store a solo ese tipo de dispositivo. O bien, puede probar de manera condicional la presencia de una API en tiempo de ejecución y adaptar el comportamiento de la aplicación en consecuencia. Para obtener más información, consulte la sección **Escribir código** en [Programación con los SDK de extensión](/uwp/extension-sdks/device-families-overview#writing-code).<br>

El siguiente vídeo proporciona una breve introducción a las familias de dispositivos y el código adaptable:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

### <a name="adaptive-controls-and-input"></a>Controles y entrada adaptables

Los elementos de la interfaz de usuario responden al tamaño y los ppp de la pantalla sobre la que se ejecuta la aplicación al ajustar su escala y diseño. Las aplicaciones para UWP funcionan bien con varios tipos de entrada como el teclado, el mouse, la funcionalidad táctil, el lápiz táctil y los controladores de Xbox One. Si necesita personalizar aún más la interfaz de usuario a un tamaño de pantalla o un dispositivo específicos, los nuevos paneles y herramientas de diseño le ayudarán a diseñar interfaces de usuario que se puedan adaptar a los diferentes dispositivos y factores de forma sobre los que se ejecute la aplicación.

![Dispositivos de Windows](images/1894834-hig-device-primer-01-500.png)

Windows te ayuda a destinar la interfaz de usuario a varios dispositivos con las siguientes características:

- Los controles universales y paneles de diseño te ayudan a optimizar la interfaz de usuario para la resolución de pantalla del dispositivo. Por ejemplo, los controles como botones y controles deslizantes se adaptan automáticamente al tamaño de pantalla del dispositivo y a la densidad de ppp. Los paneles de diseño ayudan a ajustar el diseño del contenido en función del tamaño de la pantalla. El ajuste de escala adaptable se amolda a la resolución y a las diferencias de ppp de todos los dispositivos.
- El control de entrada común te permite recibir una entrada mediante función táctil, un lápiz, un mouse, un teclado o bien un controlador, como el controlador de Microsoft Xbox.
- Las herramientas que le ayudan a diseñar una interfaz de usuario que se pueda adaptar a diferentes resoluciones de pantalla.

Algunos aspectos de la interfaz de usuario de la aplicación se adaptan automáticamente en todos los dispositivos. Sin embargo, quizá haya que adaptar el diseño de la experiencia del usuario en la aplicación, en función del dispositivo en el que esta se ejecute. Por ejemplo, una aplicación de fotos podría adaptar su interfaz de usuario cuando se ejecute en un dispositivo portátil y pequeño para garantizar que pueda usarse perfectamente con una sola mano. Cuando una aplicación de fotos se ejecuta en un ordenador de escritorio, la interfaz de usuario debe adaptarse para aprovechar el espacio de pantalla adicional.

### <a name="theres-one-store-for-all-devices"></a>Hay una store para todos los dispositivos

Una store de aplicaciones unificada hace que la aplicación esté disponible en dispositivos con Windows 10 como PC, tabletas, Xbox, HoloLens, Surface Hub y dispositivos de Internet de las cosas (IoT). Puede enviar la aplicación a la store y hacer que esté disponible en todos los tipos de dispositivos o solo en los que elija. Puedes enviar y administrar todas tus aplicaciones para los dispositivos Windows en un solo lugar. ¿Tiene una aplicación de escritorio en C++ y quiere modernizarla con características de UWP y venderla en Microsoft Store? Tampoco hay problema.

Las aplicaciones para UWP se integran con [Application Insights](https://azure.microsoft.com/services/application-insights/) para los análisis y la telemetría detallada, una herramienta esencial para entender a los usuarios y mejorar las aplicaciones.

Las aplicaciones para UWP se pueden empaquetar con [MSIX](/windows/msix/) y distribuirse a través de Microsoft Store, o de otras formas. MSIX permite que las aplicaciones se actualicen independientemente de cómo se distribuyan. Para saber más, consulte [Actualización de paquetes de aplicaciones publicadas que no son de Store desde su código](/windows/msix/non-store-developer-updates).

### <a name="monetize-your-app"></a>Monetiza tu aplicación

Puede elegir cómo monetizar la aplicación. Existen muchas maneras de ganar dinero con las aplicaciones. Lo único que necesita hacer es elegir la que mejor se adapte a sus necesidades, por ejemplo:

- Una descarga de pago es la opción más simple. Simplemente ponga el precio.
- Las versiones de prueba permiten a los usuarios probar la aplicación antes de comprarla, ya que ofrecen capacidades de detección y conversión más sencillas que las opciones "freemium" más tradicionales.
- Precios de venta para incentivar a los usuarios.
- Compras desde la aplicación.


### <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Entrega información en tiempo real y relevante a los usuarios para que regresen

Hay varias formas de mantener a los usuarios interesados con la aplicación para UWP:

- Iconos dinámicos e iconos de pantalla de bloqueo que muestran información relevante y oportuna según el contexto, desde la aplicación y con solo un vistazo.
- Notificaciones push que transmiten alertas en tiempo real para llamar la atención del usuario.
- Las actividades del usuario permiten que los usuarios reanuden la actividad en la aplicación donde la dejaron, incluso en otros dispositivos.
- El centro de actividades organiza notificaciones desde la aplicación.
- La ejecución en segundo plano y los desencadenadores ponen la aplicación en acción justo cuando el usuario lo necesita.
- Tu aplicación puede usar la voz y dispositivos Bluetooth de bajo consumo para que los usuarios interactúen con el mundo que los rodea.
- Integre Cortana para agregar la funcionalidad de comando de voz a la aplicación.

##  <a name="use-a-language-you-already-know"></a>Usa un lenguaje que ya conoces

Las aplicaciones para UWP usan Windows Runtime, la API nativa proporcionada por el sistema operativo. Esta API se implementa en C++ y es compatible con C#, Visual Basic, C++ y JavaScript. Algunas de las opciones para escribir aplicaciones para UWP son:

- Interfaz de usuario XAML, C#, VB o C++
- Interfaz de usuario DirectX y C++
- JavaScript y HTML
- WinUI

## <a name="links-to-help-you-get-going"></a>Vínculos que te ayudan a empezar

### <a name="get-set-up"></a>Prepárate

Consulte [Preparación](/windows/apps/get-started/get-set-up.md) para descargar las herramientas que necesita a fin de empezar a crear aplicaciones y [escriba su primera aplicación](your-first-app.md).

### <a name="design-your-app"></a>Diseña tu aplicación

El sistema de diseño de Microsoft se llama Fluent. El sistema Fluent Design es un conjunto de características para UWP combinado con procedimientos recomendados para crear aplicaciones que funcionan a la perfección en todo tipo de dispositivos con Windows. Las experiencias Fluent se adaptan y se usan naturalmente en todos los dispositivos, desde tabletas a portátiles o PC a televisiones, y en dispositivos de realidad virtual. Consulte [El sistema Fluent Design para aplicaciones para UWP](/windows/uwp/design/fluent-design-system) para ver una introducción a Fluent Design.

Un buen [diseño](http://design.windows.com/) consiste en decidir cómo interactuarán los usuarios con la aplicación, además de qué aspecto tendrá y cómo funcionará. La experiencia del usuario tiene un papel clave a la hora de determinar la satisfacción de los usuarios con tu aplicación, así que no ahorres esfuerzos en este paso. [Conceptos básicos de diseño](https://developer.microsoft.com/windows/apps/design) es una introducción al diseño de aplicación universal de Windows. Consulta [Introducción a las aplicaciones de la Plataforma universal de Windows (UWP) para diseñadores](../design/basics/design-and-ui-intro.md) para obtener información sobre cómo diseñar aplicaciones para UWP que encandilen a los usuarios. Antes de empezar a escribir código, consulta la [Información básica de dispositivos](../design/devices/index.md), que te ayudará a reflexionar sobre la experiencia de interacción que ofrecerá la aplicación en los diferentes factores de forma a los que quieras destinarla.

Además de la interacción en diferentes dispositivos, [planea la aplicación](./plan-your-app.md) para incorporar las ventajas de trabajar en varios dispositivos. Por ejemplo:

- Diseña el flujo de trabajo con [Conceptos básicos del diseño de navegación para las aplicaciones para UWP](../design/basics/navigation-basics.md) para integrar dispositivos móviles, de pantalla pequeña y de pantalla grande. [Diseña la interfaz de usuario](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) para responder a diferentes resoluciones y tamaños de pantalla.

- Determine cómo integrar varios tipos de entrada. Consulta las [Directrices sobre interacciones](../design/layout/index.md) para conocer cómo pueden interactuar los usuarios con la aplicación usando [Cortana](/cortana/skills/), [Voz](../design/input/speech-interactions.md), [interacciones táctiles](../design/input/touch-interactions.md), el [teclado táctil](../design/input/keyboard-interactions.md) y mucho más.  O consulte las [Instrucciones de texto y entrada de texto](../design/controls-and-patterns/text-controls.md) para conocer experiencias de interacción más tradicionales.

### <a name="add-services"></a>Agrega servicios

- Usa [servicios en la nube](https://azure.microsoft.com/documentation/services/cloud-services) para realizar la sincronización entre dispositivos.
- Más información sobre cómo [conectarte a servicios web](/previous-versions/windows/apps/hh761504(v=win.10)) para el soporte de la aplicación.
- Incluya [notificaciones push](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md) y [compras desde la aplicación](../monetize/enable-in-app-product-purchases.md) en el planeamiento. Estas características deberían funcionar en todos los dispositivos.

### <a name="submit-your-app-to-the-store"></a>Envía tu aplicación a Store

[Centro de partners](https://partner.microsoft.com/dashboard) le permite administrar y enviar todas las aplicaciones para dispositivos con Windows en un solo lugar. Consulte las [Publicaciones y juegos de aplicaciones de Windows](../publish/index.md) para aprender a enviar las aplicaciones para la publicación en Microsoft Store.

Las nuevas características simplifican los procesos y te dan más control. También encontrarás [informes analíticos](../publish/analytics.md) detallados combinados con [detalles de pago](../publish/payout-summary.md), formas de [promocionar la aplicación y atraer a los clientes](../publish/attract-customers-and-promote-your-apps.md) y mucho más.

Para obtener más material de introducción, consulte [An Introduction to Building Windows Apps for Windows 10 Devices](/archive/msdn-magazine/2015/may/windows-10-an-introduction-to-building-windows-apps-for-windows-10-devices) (Introducción a la compilación de aplicaciones para dispositivos para Windows 10)

### <a name="more-advanced-topics"></a>Temas más avanzados

- Más información sobre cómo usar [Actividades del usuario](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) para que la actividad del usuario en la aplicación aparezca en la línea de tiempo de Windows y en la opción Continuar donde lo dejé de Cortana.
- Más información sobre cómo usar [iconos, distintivos y notificaciones para las aplicaciones para UWP](../design/shell/tiles-and-notifications/index.md).
- Para obtener la lista completa de las API de Win32 disponibles para las aplicaciones para UWP, consulta los artículos sobre [conjuntos de API para aplicaciones para UWP](/previous-versions/mt186421(v=vs.85)) y [DLL para aplicaciones para UWP](/previous-versions/mt186422(v=vs.85)).
- Consulte [aplicaciones universales de Windows en. NET](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/) para una introducción a la escritura de aplicaciones para UWP en .NET.
- Para obtener una lista de tipos de .NET que puede usar en una aplicación para UWP, consulte [.NET para aplicaciones para UWP](/dotnet/api/index?view=dotnet-uwp-10.0)
- [Compilar aplicaciones con .NET Native](/dotnet/framework/net-native/)
- Más información sobre cómo agregar experiencias modernas para los usuarios de Windows 10 a la aplicación de escritorio existente y distribuirla en Microsoft Store con el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Cómo se relaciona con la Plataforma universal de Windows con la API de Windows Runtime
Si va a compilar una aplicación de la Plataforma universal de Windows (UWP), a continuación, puede obtener un lote de kilometraje y comodidad de tratamiento de los términos de la "Plataforma de Windows Universal (UWP)" y de "Windows Runtime (WinRT)" más o menos como sinónimos. Pero *es* posible buscar en la portada de la tecnología y determinar la diferencia entre estas ideas. Si tiene curiosidad acerca de eso, esta última sección es para usted.

Las API de Windows Runtime y WinRT son una evolución de las API de Windows. Originalmente, Windows se programó a través las API planas de Win32 de estilo C. A las que se agregaron API COM ([DirectX](/windows/desktop/directx) es un ejemplo destacado). Windows Forms, WPF, .NET y los lenguajes administrados tienen su propia manera de escribir aplicaciones de Windows y su propio tipo de la tecnología de API. Windows Runtime es, en segundo plano, la siguiente fase de COM. En el nivel actual de interfaz binaria de aplicaciones (ABI), sus raíces en COM se hacen visibles. Pero Windows Runtime se diseñó para que pueda llamar desde una gran gama de diferentes lenguajes de programación. Y para que se puede llamar de manera que sea muy natural a cada uno de esos lenguajes. Con este fin, el acceso a Windows Runtime está disponible a través de lo que se conoce como las proyecciones de lenguaje. Hay una proyección de lenguaje de Windows Runtime en C#, Visual Basic, estándar C++, JavaScript, etc. Además, una vez empaquetada adecuadamente (consulta [Puente de dispositivo de escritorio](/windows/msix/desktop/source-code-overview)), puedes llamar a la API de WinRT desde una aplicación compilada en un modelo de aplicación de una gran gama: Win32, .NET, WinForms, y WPF.

Y, por supuesto, puede llamar a las API de WinRT desde la aplicación para UWP. UWP es un modelo de aplicación compilado en Windows Runtime. Técnicamente, el modelo de aplicación para UWP se basa en [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), aunque ese detalle puede estar oculto según la elección del lenguaje de programación. Tal y como se ha explicado en este tema, desde un punto de vista de la propuesta de valor, la UWP se presta a la escritura de un archivo binario único que puede elegir publicar en Microsoft Store y ejecutar en una gran gama de factores de forma del dispositivo. La cobertura de los dispositivos de la aplicación para UWP depende del subconjunto de las API de Windows Runtime a las que limita que llame la aplicación o que llama condicionalmente.

Esperamos que esta sección haya descrito adecuadamente la diferencia entre la tecnología subyacente de las API de Windows Runtime y el mecanismo y valor empresarial de la Plataforma universal de Windows.

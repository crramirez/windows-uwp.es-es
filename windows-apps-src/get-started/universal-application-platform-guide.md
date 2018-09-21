---
author: TylerMSFT
title: ¿Qué es una aplicación para Plataforma universal de Windows (UWP)?
description: Obtén más información sobre las aplicaciones para la Plataforma universal de Windows (UWP) que se pueden ejecutar en una amplia variedad de dispositivos que ejecuten Windows 10.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.author: twhitney
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, universal
ms.localizationpriority: medium
ms.openlocfilehash: 7f0168f0a1baef5e68bccdf0a33c3ac7eb7683a7
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4092089"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>¿Qué es una aplicación para Plataforma universal de Windows (UWP)?

![Las aplicaciones para la Plataforma universal de Windows se ejecutan en una amplia variedad de dispositivos, son compatibles con interfaces de usuario adaptables y ofrecen una entrada de usuario intuitiva, una sola Tienda, un solo Centro de desarrollo y servicios en la nube.](images/universalapps-overview.png)

Una aplicación para UWP es:

- Segura: Las aplicaciones para UWP declaran cuáles son los recursos y datos de dispositivos a los que acceden. El usuario debe autorizar ese acceso.
- Pueden usar una API común en todos los dispositivos que usan Windows 10.
- Pueden usar funcionalidades específicas de dispositivo y adaptar la interfaz de usuario a las PPP, resoluciones y tamaños de pantalla de diferentes dispositivos.
- Disponible desde Microsoft Store en todos los dispositivos (o solo aquellos que especifiques) que se ejecutan en Windows 10. Microsoft Store proporciona varias maneras de ganar dinero en tu aplicación.
- Puede instalarse y desinstalarse sin riesgo para la máquina ni incurrir en "equipo rot".
- Atractiva: usa iconos dinámicos, notificaciones de inserción y actividades del usuario que interactúan con la escala de tiempo de Windows y la función Continuar donde lo dejé de Cortana para atraer a los usuarios.
- Programable en C#, C++, Visual Basic y Javascript. Para la interfaz de usuario, usa XAML, HTML o DirectX.

Veamos esto con más detalle.

## <a name="secure"></a>Seguras

La aplicaciones para UWP declaran en el manifiesto las capacidades del dispositivo que necesitan, como el acceso al micrófono, la ubicación, la cámara web, los dispositivos USB, archivos, etc. El usuario debe confirmar y autorizar ese acceso antes de que la aplicación obtenga la funcionalidad.

## <a name="a-common-api-surface-across-all-devices"></a>Una superficie de API común para todos los dispositivos

Windows 10 presenta la Plataforma universal de Windows (UWP), que proporciona una plataforma común de aplicaciones en todos los dispositivos en Windows 10. Las API principales de la UWP son las mismas en todos los dispositivos Windows. Si tu aplicación usa solo las API principales, se ejecutará en cualquier dispositivo Windows 10, con independencia de si es un equipo de escritorio, una Xbox, unos auriculares de realidad mixta, etc.

Una aplicación para UWP escrita en C++ /WinRT or C++ /CX tiene acceso a las API de Win32 que forman parte de la UWP. Todas los dispositivos de Windows 10 implementan estas API de Win32.

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Las SDK de extensión exponen las funcionalidades exclusivas de tipos específicos de dispositivo

Si tu objetivo son las API universales, la aplicación puede ejecutarse en todos los dispositivos con Windows 10. Sin embargo, si quieres que tu aplicación para UWP aproveche las API específicas del dispositivo, puedes hacerlo.

Las SDK de extensión te permiten llamar a API especializadas para diferentes dispositivos. Por ejemplo, si tu aplicación para UWP está destinada a un dispositivo de IoT, puedes agregar la SDK de extensión IoT a tu proyecto para dirigirte a características específicas de los dispositivos IoT. Para obtener más información sobre cómo agregar SDK de extensión, consulta la sección **SDK de extensión** en [Información general sobre las familias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks).

Puedes escribir tu aplicación para que se ejecute solo en un determinado tipo de dispositivo y, a continuación, limitar su distribución desde Microsoft Store a solo ese tipo de dispositivo. O bien, puedes probar de manera condicional la presencia de una API en tiempo de ejecución y adaptar el comportamiento de la aplicación en consecuencia. Para obtener más información, consulta la sección **Escribir código** en [Información general sobre las familias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code)<br>

El siguiente vídeo proporciona una breve introducción de las familias de dispositivos y el código adaptable:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>Controles y entrada adaptable

Los elementos de la interfaz de usuario responden al tamaño y los PPP de la pantalla sobre la que se ejecuta la aplicación ajustando su escala y diseño. Las aplicaciones de UWP funcionan bien con varios tipos de entrada, como el teclado, el mouse, la funcionalidad táctil, el lápiz y los controladores de Xbox One. Si necesitas personalizar aún más la interfaz de usuario a un tamaño de pantalla o un dispositivo específicos, los nuevos paneles y herramientas de diseño te ayudarán a diseñar interfaces de usuario que se puedan adaptar a los diferentes dispositivos y factores de forma sobre los que se ejecute tu aplicación.

![Dispositivos de Windows](images/1894834-hig-device-primer-01-500.png)

Windows te ayuda a destinar la interfaz de usuario a varios dispositivos con las siguientes características:

- Los controles universales y los paneles de diseño te ayudan a optimizar la interfaz de usuario para la resolución de pantalla del dispositivo. Por ejemplo, controles como botones y controles deslizantes se adaptan automáticamente al tamaño de pantalla del dispositivo y la densidad de PPP. Los paneles de diseño ayudan a ajustar el diseño del contenido en función del tamaño de la pantalla. El ajuste de escala adaptable se amolda a la resolución y a las diferencias de PPP que haya entre los dispositivos.
- El control de entrada común te permite recibir una entrada mediante funcionalidad táctil, un lápiz, un ratón, un teclado o bien un controlador, como el controlador de Microsoft Xbox.
- Herramientas que te ayudan a diseñar una interfaz de usuario que se pueda adaptar a diferentes resoluciones de pantalla.

Algunos aspectos de la interfaz de usuario de la aplicación se adaptan automáticamente en todos los dispositivos. Sin embargo, quizá haya que adaptar el diseño de la experiencia del usuario en la aplicación, en función del dispositivo en el que esta se ejecute. Por ejemplo, una aplicación de fotos podría adaptar su interfaz de usuario cuando se ejecute en un dispositivo portátil y pequeño para garantizar que pueda usarse perfectamente con una sola mano. Cuando una aplicación de fotos se ejecuta en un ordenador de escritorio, la interfaz de usuario debe adaptarse para aprovechar el espacio de pantalla adicional.

## <a name="theres-one-store-for-all-devices"></a>Hay una tienda para todos los dispositivos

Una tienda de aplicaciones unificada hace que la aplicación esté disponible en dispositivos de Windows 10 como PC, tabletas, Xbox, HoloLens, Surface Hub, y dispositivos de Internet de las cosas (IoT). Puedes enviar la aplicación a la tienda y hacer que esté disponible en todos los tipos de dispositivos, o solo en los que elijas. Puedes enviar y administrar todas tus aplicaciones para los dispositivos Windows desde un solo lugar. ¿Tienes una aplicación de escritorio en C++ y quieres modernizarla con características de UWP y venderla en Microsoft Store? Tampoco hay problema.

Las aplicaciones de UWP se integran con [Application Insights](http://azure.microsoft.com/services/application-insights/) para los análisis y la telemetría detallada, una herramienta esencial para entender a tus usuarios y mejorar las aplicaciones.

### <a name="monetize-your-app"></a>Monetizar la aplicación

Puedes elegir cómo monetizar tu aplicación. Existen muchas maneras de ganar dinero con tus aplicaciones. Lo único que necesitas hacer es elegir la que mejor se adapte a ti, por ejemplo:

- Una descarga de pago es la opción más simple. Simplemente pon el precio.
- Las versiones de prueba permiten a los usuarios probar tu aplicación antes de comprarla, ya que ofrecen capacidades de detección y conversión más sencillas que las opciones "freemium" más tradicionales.
- Precios de venta para incentivar a los usuarios.
- También están disponibles compras desde la aplicación y anuncios.

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>Las aplicaciones de Microsoft Store proporcionan una experiencia de instalación, desinstalación y actualización fluida.

Todas las aplicaciones de UWP se distribuyen mediante un sistema de empaquetado que protege el sistema, el dispositivo y el usuario. Los usuarios nunca tendrán que lamentar haber instalado una aplicación porque las aplicaciones para UWP se pueden desinstalar sin dejar nada atrás, excepto los documentos creados con la aplicación.

Las aplicaciones se pueden implementar y actualizar sin problemas. El empaquetado de la aplicación puede ser modular para poder descargar contenido y extensiones a petición.

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Ofrece información en tiempo real y relevante a los usuarios para que sigan volviendo

Hay varias formas de mantener a los usuarios interesados con tu aplicación para UWP:

- Iconos dinámicos e iconos de pantalla de bloqueo que muestran información relevante y oportuna según el contexto, desde tu aplicación y con solo un vistazo.
- Notificaciones de inserción que transmiten alertas en tiempo real para llamar la atención del usuario.
- Las actividades del usuario permiten que los usuarios reanuden la actividad en tu aplicación allá donde la dejaron, incluso en otros dispositivos.
- El Centro de acciones organiza notificaciones desde tu aplicación.
- La ejecución en segundo plano y los desencadenadores permiten que tu aplicación cobre vida justo cuando el usuario lo necesita.
- Tu aplicación puede usar la voz y dispositivos Bluetooth de bajo consumo para que los usuarios interactúen con el mundo que los rodea.
- Integra Cortana para agregar funcionalidad de comando de voz a tu aplicación.

##  <a name="use-a-language-you-already-know"></a>Usa un lenguaje que ya conozcas

Las aplicaciones para UWP usan Windows Runtime, la API nativa provista por el sistema operativo. Esta API se implementa en C++ y es compatible con C#, Visual Basic, C++ y JavaScript. Algunas de las opciones para escribir aplicaciones para UWP son:

- Interfaz de usuario XAML, C#, VB o C++
- Interfaz de usuario DirectX y C++
- JavaScript y HTML

## <a name="links-to-help-you-get-going"></a>Vínculos que te ayudan a empezar

### <a name="get-set-up"></a>Prepárate

Consulta [Prepárate](get-set-up.md) para descargar las herramientas que necesitas a fin de empezar a crear aplicaciones y [escribe tu primera aplicación](your-first-app.md).

### <a name="design-your-app"></a>Diseña tu aplicación

El sistema de diseño de Microsoft se denomina Fluent. El sistema Fluent Design es un conjunto de características para UWP combinado con procedimientos recomendados para crear aplicaciones que funcionan a la perfección en todo tipo de dispositivos Windows. Las experiencias fluidas se adaptan y se usan con fluidez en todos los dispositivos, desde tabletas a portátiles o equipos a televisores, y en dispositivos de realidad virtual. Consulta [El sistema Fluent Design para las aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/design/fluent-design-system) para ver una introducción a Fluent Design.

Un buen [diseño](http://go.microsoft.com/fwlink/?LinkId=258848) consiste en decidir cómo interactuarán los usuarios con tu aplicación, además de qué aspecto tendrá y cómo funcionará. La experiencia del usuario tiene un papel clave a la hora de determinar la satisfacción de los usuarios con tu aplicación, así que no ahorres esfuerzos en este paso. [Conceptos básicos de diseño](https://dev.windows.com/design) es una introducción al diseño de aplicaciones universales de Windows. Consulta [Introducción a las aplicaciones de la Plataforma universal de Windows (UWP) para diseñadores](https://msdn.microsoft.com/library/windows/apps/dn958439) para obtener información sobre cómo diseñar aplicaciones para UWP que encandilen a los usuarios. Antes de empezar a escribir código, consulta la [Información básica de dispositivos](../design/devices/index.md) , que te ayudará a reflexionar sobre la experiencia de interacción que ofrecerá la aplicación en los diferentes factores de forma a los que quieras destinarla.

Además de la interacción en diferentes dispositivos, [planea la aplicación](https://msdn.microsoft.com/library/windows/apps/hh465427) para incorporar las ventajas de trabajar en varios dispositivos. Por ejemplo:

- Diseña el flujo de trabajo con [Conceptos básicos de diseño de la navegación para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn958438) para integrar dispositivos móviles, de pantalla pequeña y de pantalla grande. [Diseña la interfaz de usuario](https://msdn.microsoft.com/library/windows/apps/dn958435) para responder a diferentes tamaños de pantalla y resoluciones.

- Determina cómo integrar varios tipos de entrada. Consulta las [Directrices sobre interacciones](https://msdn.microsoft.com/library/windows/apps/dn611861) para conocer cómo pueden interactuar los usuarios con la aplicación usando [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), [voz](https://msdn.microsoft.com/library/windows/apps/dn596121), [interacciones táctiles](https://msdn.microsoft.com/library/windows/apps/hh465370), el [teclado táctil](https://msdn.microsoft.com/library/windows/apps/hh972345) y mucho más.  O consulta las [Directrices de texto y entrada de texto](https://msdn.microsoft.com/library/windows/apps/dn611864) para conocer experiencias de interacción más tradicionales.

### <a name="add-services"></a>Agrega servicios

- Usa [servicios en la nube](http://go.microsoft.com/fwlink/?LinkId=526377) para sincronizarla entre dispositivos.
- Aprende a [conectarte a servicios web](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504) para mejorar la experiencia con la aplicación.
- Aprende a [agregar Cortana a tu aplicación](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382) para que tu aplicación pueda responder a comandos de voz.
- Incluye [notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/mt187203) y [compras desde la aplicación](https://msdn.microsoft.com/library/windows/apps/mt219684) en tu planificación. Estas características deberían funcionar en todos los dispositivos.

### <a name="submit-your-app-to-the-store"></a>Envía tu aplicación a la tienda.

El nuevo panel unificado del Centro de desarrollo de Windows te permite administrar y enviar todas las aplicaciones para dispositivos Windows en un solo lugar. Consulta [Usar el panel unificado del Centro de desarrollo de Windows](../publish/using-the-windows-dev-center-dashboard.md) para aprender a enviar tus aplicaciones para publicarlas en Microsoft Store.

Las nuevas funciones simplifican los procesos y te dan más control. También encontrarás [informes analíticos](https://msdn.microsoft.com/library/windows/apps/mt148522) detallados combinados con [detalles de pago](https://msdn.microsoft.com/library/windows/apps/dn986925), formas de [promocionar la aplicación y atraer a los clientes](https://msdn.microsoft.com/library/windows/apps/mt148526) y mucho más.

Para obtener más material de introducción, consulta [An Introduction to Building Windows Apps for Windows 10 Devices (Introducción a la compilación de aplicaciones para dispositivos para Windows 10)](https://msdn.microsoft.com/magazine/dn973012.aspx).

### <a name="more-advanced-topics"></a>Temas más avanzados

- Aprende a usar [Actividades del usuario](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) para que la actividad del usuario en la aplicación aparezca en la línea de tiempo de Windows y en la opción Continuar donde lo dejé de Cortana.
- Aprende a usar [iconos, distintivos y notificaciones para las aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/).
- Para obtener la lista completa de las API de Win32 disponibles para las aplicaciones para UWP, consulta [Conjuntos de API para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/desktop/mt186421) y [DLLs para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/desktop/mt186422).
- Consulta [aplicaciones universales de Windows en. NET](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net) para obtener información general sobre la escritura de aplicaciones para UWP. NET.
- Para obtener una lista de tipos de .NET que puedes usar en una aplicación para UWP, consulta [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/mt185501.aspx)
- [.NET Native - What it means for Universal Windows Platform (UWP) developers (.NET Native: qué supone para los desarrolladores de la Plataforma universal de Windows [UWP])](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
- Aprende a agregar experiencias modernas para los usuarios de Windows 10 a tu aplicación de escritorio existente y distribuirla en Microsoft Store con el [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Cómo se relaciona con la plataforma Universal de Windows a Windows Runtime APIs
Si vas a crear una aplicación de plataforma Universal de Windows (UWP), a continuación, puedes obtener una gran cantidad de kilometraje y comodidad fuera del tratamiento de los términos "universales de Windows (UWP)" y "Windows Runtime (WinRT)" como sinónimos más o menos. Pero *es* posible buscar en el interior de la tecnología y determinar solo cuál es la diferencia entre las ideas. Si tienes curiosidad sobre esto, esta última sección es para TI.

El tiempo de ejecución de Windows y WinRT APIs, son una evolución de las API de Windows. Originalmente, Windows se programan a través de plana, API de Win32 de estilo C. Para los usuarios se agregaron COM APIs ([DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) que se va a un ejemplo destacado). Windows Forms, WPF, .NET y los lenguajes administrados poner su propios forma de escribir aplicaciones de Windows y su propio tipo de tecnología de API. El tiempo de ejecución de Windows es, en realidad, la siguiente etapa de COM. En la capa de interfaz binaria (ABI) de la aplicación real, sus raíces de COM se vuelven visibles. Pero se ha diseñado el tiempo de ejecución de Windows se pueda llamar desde una gran variedad de lenguajes de programación. Y que se puede llamar de manera que es muy natural a cada uno de estos lenguajes. Para ello, acceso al tiempo de ejecución de Windows estará disponible a través de lo que se conoce como proyecciones de lenguaje. Hay una proyección de lenguaje de Windows Runtime en C#, en Visual Basic, en C++ estándar, en JavaScript y así sucesivamente. Además, una vez empaquetada correctamente (consulta el [Puente de escritorio](/windows/uwp/porting/desktop-to-uwp-root)), puedes llamar a WinRT APIs desde una aplicación compilada en uno de un intervalo de modelos de aplicación excelente: Win32,. NET, WinForms y WPF.

Y, por supuesto, puedes llamar WinRT APIs desde tu aplicación para UWP. UWP es un modelo de aplicaciones que se basa en el tiempo de ejecución de Windows. Técnicamente, el modelo de aplicaciones para UWP se basa en [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), aunque ese detalle que se ocultará del usuario, en función de su elección del lenguaje de programación. Como se explica en este tema, desde un punto de vista de la propuesta de valor, el UWP se presta a escribir un único código binario que, debe elegir, se puede publicarse en Microsoft Store y ejecutar en cualquiera de una gran variedad de factores de forma de dispositivo. El alcance del dispositivo de la aplicación para UWP depende el subconjunto de las API de UWP es limitar la aplicación a llamar a o llamar condicionalmente.

Afortunadamente, esta sección ha tenido éxito en la descripción de la diferencia entre la tecnología subyacente de Windows en tiempo de ejecución APIs y el mecanismo y el valor de empresas de la plataforma Universal de Windows.

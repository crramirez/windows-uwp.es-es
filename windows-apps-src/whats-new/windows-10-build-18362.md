---
title: Novedades de Windows 10, compilación 18362
description: Tanto la compilación 18362 de Windows 10 como las nuevas herramientas para desarrolladores le proporcionan las herramientas, características y experiencias que ofrece la tecnología de la Plataforma universal de Windows.
keywords: Windows 10, 18362, 1903
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bdd003445bed4ad59b21e0b744281651c30bf04f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166889"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>Novedades para desarrolladores en Windows 10, compilación 18362

Windows 10 compilación 18362 (también conocido como SDK versión 1903), en combinación con Visual Studio 2019, proporciona las herramientas, características y experiencias para hacer aplicaciones notables de Windows. [Instala las herramientas y el SDK](https://developer.microsoft.com/windows/downloads#_blank) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

Esta es una colección de características e instrucciones nuevas y mejoradas de interés para los desarrolladores de Windows en esta versión. Para obtener una lista completa de los nuevos espacios de nombres agregados a Windows SDK, consulte [Cambios en la API en compilación 18362 de Windows 10](windows-10-build-18362-api-diff.md). Para obtener más información sobre las características más destacadas de Windows 10, consulte [Lo más destacado de Windows 10](https://developer.microsoft.com/windows/windows-10-for-developers).

## <a name="design--ui"></a>Diseño e interfaz de usuario

Característica | Descripción
:------ | :------
AnimatedVisualPlayer | La API de [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) aloja y controla la reproducción de objetos visuales animados en la aplicación. Esta API se usa para controlar y mostrar el contenido como objetos visuales [Lottie](/windows/communitytoolkit/animations/lottie) que permiten representar las animaciones de Adobe AfterEffects de forma nativa en las aplicaciones.
CompactDensity | Habilitar [Modo compacto](../design/style/spacing.md) en la aplicación permite grupos de control denso con mucha de la información. Esto puede ayudar con grandes cantidades de contenido, maximizar el contenido visible en una página de exploración o navegación e interacción cuando el usuario está usando la entrada de puntero.
Repetidor de elementos | Un control [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) puede crear una experiencia personalizada para mostrar colecciones a los usuarios. ItemsRepeater no proporciona una experiencia de usuario completa o una interfaz de usuario predeterminada. En su lugar, es un bloque de compilación puede usar para crear sus propias experiencias únicas basado en recopilación y controles personalizados.
Sugerencia de enseñanza | Una [sugerencia de enseñanza](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md) es un control flotante parcialmente persistente y con mucho contenido que proporciona información contextual. Puede utilizar este control para informar, recordar y enseñar a los usuarios sobre las características nuevas o importantes.
Comandos de la interfaz de usuario | Con [comandos en aplicaciones para UWP](../design/controls-and-patterns/commanding.md), utilice las clases [XamlUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) y [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) (junto con la interfaz ICommand) para compartir y administrar comandos entre diversos tipos de control, independientemente del tipo de dispositivo y de la entrada que se va a usar.
Biblioteca de interfaz de usuario de Windows | La última versión oficial de la biblioteca de interfaz de usuario de Windows, [WinUI 2.1](/uwp/toolkits/winui/release-notes/winui-2.1), proporciona nuevos controles XAML vibrantes para su aplicación de Windows. Las API de la biblioteca WinUI se ejecutan en versiones anteriores de Windows 10, por lo que no tienes que incluir comprobaciones de versión ni XAML condicional para admitir usuarios que no dispongan del último sistema operativo.
Nivel visual en aplicaciones de escritorio | Ahora puede [usar las API de nivel visual de UWP en aplicaciones de escritorio](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps). Estas API ofrecen una API de modo retenido y alto rendimiento para gráficos, efectos y animaciones, y son la base de todas las interfaces de usuario en todos los dispositivos de Windows.
Profundidad Z y sombra | Use [profundidad Z y sombra](../design/layout/depth-shadow.md) para crear la elevación en la aplicación para UWP. Estas nuevas características permiten que la interfaz de usuario de la aplicación sea más fácil de examinar y transmita mejor lo que es importante para los usuarios.

## <a name="develop-windows-apps"></a>Desarrollar aplicaciones de Windows

Característica | Descripción
:------ | :------
Interfaz de examen antimalware (AMSI) | Obtenga información sobre [cómo la interfaz de examen Antimalware (AMSI) le ayuda a defenderse contra malware](/windows/desktop/amsi/how-amsi-helps), a continuación, revise el [código de ejemplo](/windows/desktop/amsi/dev-audience) para obtener información sobre cómo implementarlo en la aplicación de escritorio.
C++/WinRT 2.0 | La versión 2.0 de C++/WinRT se ha liberado. Compruebe las [novedades de C++/WinRT](../cpp-and-winrt-apis/news.md) para una lista completa de todos los nuevos cambios e incorporaciones.
Elección de la plataforma | ¿Está interesado en crear una nueva aplicación de escritorio? Visite nuestra página renovada [elija su plataforma](/windows/desktop/choose-your-technology) para obtener una descripción detallada y las comparaciones de las plataformas de Windows Forms, WPF y UWP, y para obtener más información sobre la API de Win32.
Agente de conversación | El espacio de nombres [Windows.ApplicationModel.ConversationalAgent](/uwp/api/windows.applicationmodel.conversationalagent) le permite agregar cualquier asistencia digital compatible con la activación del tiempo de ejecución del agente de la plataforma de Windows (AAR) a la aplicación de Windows.
API de archivos de la nube | La *API de archivos de la nube* le permite [crear un motor de sincronización en la nube que admite archivos de marcador de posición](/windows/desktop/cfapi/build-a-cloud-file-sync-engine).
Direct 3D 12 | [La representación de Direct3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) puede mejorar el rendimiento de su representador si se basa en la representación de aplazado basados en mosaicos (TBDR), entre otras técnicas. La técnica ayuda a que el representador mejore la eficacia de la GPU habilitando la aplicación para identificar mejor el orden de los requisitos y las dependencias de datos de representación de recursos. Esto reduce el tráfico de memoria de/hacia la memoria fuera del chip.
Direct Machine Learning (DirectML) | [DirectML](/windows/desktop/direct3d12/dml) es una API de bajo nivel y acelerada por hardware para el aprendizaje automático. Tiene una conocida interfaz de programación (nativo C++, nano COM) flujo de trabajo en el estilo de DirectX 12. Puede integrar las cargas de trabajo de inferencia del aprendizaje automático en su juego, motor, middleware, back-end u otra aplicación. DirectML admite todo el hardware compatible con DirectX 12.
DirectX HLSL | El [Modelo de sombreador HLSL 6.4](/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) proporciona nuevas funciones intrínsecas de aprendizaje automático para su uso con DirectML.
Desarrollo de controladores | Se ha agregado nuevo audio, cámara, pantalla, redes,banda ancha móvil, impresión, sensor, almacenamiento y características wifi para desarrolladores de controladores de Windows. Consulte las [Novedades de desarrollo de controladores](/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest) para obtener más detalles.
Operaciones de sistema de archivos | Esta [Guía de procedimientos recomendados](../files/best-practices-for-writing-to-files.md) puede ayudarle a hacer un mejor uso de Windows.Storage.FileIO y Windows.Storage.PathIO clases para realizar operaciones de E/S del sistema de archivos.
Interacciones con controlador para juegos y control remoto | Use [interacciones de controlador para juegos y control remoto](../design/input/gamepad-and-remote-interactions.md) para compilar experiencias de interacción útiles y accesibles. Con estas interacciones, la aplicación puede ser tan intuitiva y fácil de usar desde dos hasta a diez pies de distancia.
Cambio a la era japonesa | Hemos proporcionado [estas instrucciones](../design/globalizing/japanese-era-change.md) para mostrarle cómo asegurarse de que la aplicación de Windows está lista para el conjunto de cambios de la era japonesa que se realizará el 1 de mayo de 2019. Esta página también está disponible en japonés (en la parte inferior del artículo, haz clic en el control de idioma y selecciona Japonés).
Código abierto de WPF, Windows Forms y WinUI | Los marcos de trabajo de WPF, Windows Forms y WinUI UX ahora están disponibles para las contribuciones de código abierto en GitHub. Para obtener más información y vínculos, consulte el [blog compilación de aplicaciones de Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).
Aplicaciones web progresivas para Xbox | Con [las aplicaciones web progresivas para Xbox One](/microsoft-edge/progressive-web-apps/xbox-considerations) puede extender una aplicación web y hacerla disponible como una aplicación de Xbox One a través de Microsoft Store mientras todavía sigue usando los marcos de trabajo existentes, CDN y el servidor back-end. En su mayor parte, puede empaquetar su PWA para Xbox One de la misma manera que lo haría para Windows. Esta guía le llevará a través del proceso y resaltar las diferencias clave.
Project Rome | El SDK del proyecto Roma ahora está disponible para iOS y Android. Obtenga información sobre cómo integrar las notificaciones de grafo con cada plataforma: [Android](/windows/project-rome/notifications/how-to-guide-for-android) e [iOS](/windows/project-rome/notifications/how-to-guide-for-ios).
Cámaras remotas | Utilice la clase DeviceWatcher para [conectarse a cámaras remotas](../audio-video-camera/connect-to-remote-cameras.md) y leer los fotogramas de las cámaras en la aplicación de Windows.
Controles de UWP en aplicaciones de escritorio (XAML aisladas) | Las API del SDK de Windows para hospedar controles UWP en aplicaciones de escritorio Win32 de WPF, Windows Forms y C++ ya no están en vista previa ara desarrolladores. Para obtener más información, consulte [Controles de UWP en aplicaciones de escritorio](/windows/apps/desktop/modernize/xaml-islands).
Visual Studio 2019 | Se ha lanzado Visual Studio 2019 con las últimas herramientas y servicios para cualquier desarrollador, aplicación o plataforma. Consulta las [Novedades de Visual Studio 2019](/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019) para obtener información sobre la versión más reciente y empezar a trabajar.
Win32 WebView | Nuestras [preguntas más frecuentes](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) proporcionan respuestas a preguntas habituales al usar Microsoft Edge WebView en aplicaciones de escritorio, así como vínculos a ejemplos y recursos adicionales.
Línea de comandos de Windows | Las [Nuevas características de consola](https://devblogs.microsoft.com/commandline/new-experimental-console-features/) incluyen la pestaña de terminal experimental con configuración de desplazamiento, forma de cursor y colores de cursor. Obtenga más información en el [blog de herramientas de línea de comandos de Windows para desarrolladores](https://devblogs.microsoft.com/commandline/).
Kit de herramientas de la Comunidad Windows | El kit de herramientas de la Comunidad Windows v5.1 proporciona actualizaciones interesantes para animación, dispositivos remotos, recorte de imágenes y accesibilidad. </br> • La nueva [biblioteca Lottie de Windows](/windows/communitytoolkit/animations/lottie) proporciona compatibilidad de animación de alta calidad en Windows 10 (1809) mediante el uso de las API de Windows.UI.Composition y permite el consumo de archivos JSON [Bodymovin](https://aescripts.com/bodymovin/) o clases de generadores de código optimizado para la reproducción en sus aplicaciones de Windows. Probar la nueva [aplicación de visualización Lottie](https://www.microsoft.com/p/lottie-viewer/9p7x9k692tmw) desde Microsoft Store para probar las animaciones y generar código optimizado para las aplicaciones de Windows. </br> • El nuevo [selector de dispositivo remoto](/windows/communitytoolkit/controls/remotedevicepicker) permite al usuario seleccionar un dispositivo (próximo o accesible en la nube), iniciar una aplicación en ese dispositivo o comunicarse con servicios de aplicación en el dispositivo remoto. </br> • El nuevo [control ImageCropper](/windows/communitytoolkit/controls/imagecropper) integra la funcionalidad de recorte para seleccionar las imágenes de perfil o para el uso de herramientas de edición de fotos. </br> • Además, ha habido mejoras de accesibilidad en los controles, una actualización de paquete de previsualización [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 para WPF y WinForms, y más características que puede leer en las [notas de la versión ](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0).
Windows Machine Learning | Hemos rediseñado los documentos de inteligencia artificial de Windows y los hemos separado en tres áreas: Machine Learning de Windows (WinML), habilidades de Windows Vision y Direct Machine Learning(DirectML). Visite la nueva [página de aterrizaje](/windows/ai/) </br> • La [*experiencia* MLGen](/windows/ai/mlgen) está cambiando en Visual Studio. En Windows 10, versión 1903 y versiones posterior, *mlgen* ya no se incluye en el SDK de Windows 10. Si usa Visual Studio 2017, en su lugar debe descargar e instalar la extensión de Visual Studio [Generador de código VS 2017 de Windows Machine Learning](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen). Si usa Visual Studio 2019 debe instalar la extensión del [generador de código de Windows Machine Learning](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2). </br> • También nos enorgullece anunciar el nuevo soporte para empaquetado de peso. Los desarrolladores ahora pueden reducir la superficie del disco de sus modelos de aprendizaje automático mediante una técnica denominada empaquetado de peso, disponible a través del [convertidor WinMLTools](/windows/ai/convert-model-winmltools).
Referencia de WinRT consolidado | Hemos agregado una descripción completa del [sistema de tipos de WinRT](/uwp/winrt-cref/winrt-type-system) y [archivos WinMD](/uwp/winrt-cref/winmd-files) para proporcionar notas exhaustivas específicas acerca de las definiciones sobre la estructura de las API de WinRT.
Subsistema de Windows para Linux (WSL) | [Actualizaciones recientes de WSL](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/) incluyen la capacidad de tener acceso a archivos de Linux desde Windows con el Explorador de archivos y algunos nuevos comandos para wsl.exe y wslconfig.exe.
Windows Vision Skills | [Aptitudes de Windows Vision](/windows/ai/windows-vision-skills) es un conjunto de API que le permite crear "aptitudes", como el reconocimiento facial y, a continuación, las empaqueta como un paquete de NuGet que otras aplicaciones pueden consumir, sin necesidad de incluir un modelo de aprendizaje automático.

## <a name="publish--monetize-windows-apps"></a>Publicar y monetizar aplicaciones de Windows

Característica | Descripción
 :------ | :------
MSIX | [Las compilaciones de soporte técnico MSIX en Windows 10 1709 y 1803](/windows/msix/msix-1709-and-1803-support) describen las características de MSIX que se admiten en versiones anteriores a Windows 10, versión 1809.
Empaquetado MSIX e implementación | Hemos introducido varias [mejoras relacionadas con los paquetes de modificación](/windows/msix/modification-package-insider-preview-build-18312) para facilitar la personalización de paquetes en un paquete MSIX. Estas mejoras incluyen el nuevo elemento **rescap6:ModificationPackage** en el manifiesto del paquete, la capacidad de reemplazar un archivo del paquete principal con un paquete de modificación y la capacidad para empaquetar complemento basado en un sistema de archivos como un paquete de modificación MSIX.
Herramienta de empaquetado MSIX | • Se ha agregado [soporte técnico para realizar las conversiones en un equipo remoto](/windows/msix/packaging-tool/remote-conversion-setup). También presentamos la [herramienta de empaquetado MSIX programa Insider](/windows/msix/packaging-tool/insider-program) para ofrecer un acceso anticipado a las nuevas características de la herramienta. </br> • El [Soporte técnico de paquete MSIX en versiones 1709 y posteriores](/windows/msix/packaging-tool/support-on-1709-and-later) proporciona instrucciones sobre cómo usar la herramienta de empaquetado MSIX para compilar paquetes específicamente para Windows 10, versión 1709 y 1803. </br> • El [ambiente de empaquetado MSIX en creación rápida de Hyper-V](/windows/msix/packaging-tool/quick-create-vm) muestra cómo crear un entorno virtual para los proyectos de empaquetado de MSIX. </br> • Los [paquetes de agrupación MSIX ](/windows/msix/packaging-tool/bundle-msix-packages) proporciona instrucciones para crear un paquete mediante la herramienta de empaquetado MSIX. </br> • Los [paquetes de modificación en Windows 10 versión 1809](/windows/msix/modification-package-1809-update) contienen instrucciones para crear un paquete de modificación para la versión de Windows 10 1809 y versiones posteriores mediante la herramienta de empaquetado MSIX y MakeApp.exe.
SDK de MSIX | [Use el SDK de MSIX para compilar un paquete para su uso multiplataforma](/windows/msix/msix-sdk/sdk-guidance)y obtenga información sobre cómo especificar las plataformas de destino que desea que los paquetes extraigan.

## <a name="microsoft-learn"></a>Microsoft Learn

El nuevo sitio de Microsoft Learn ofrece nuevos conocimientos prácticos y las oportunidades de aprendizaje para desarrolladores de Microsoft.

* Si está interesado en aprender a desarrollar aplicaciones de Windows, consulte [nuestra nueva ruta de aprendizaje](/learn/paths/develop-windows10-apps/) para una introducción exhaustiva a la plataforma, las herramientas y cómo escribir sus primeras aplicaciones.

* ¿Desea obtener información sobre cómo agregar características de interfaz de usuario a la aplicación de Windows? Obtenga información sobre cómo [crear una interfaz de usuario](/learn/modules/create-ui-for-windows-10-apps/), [agregar navegación y medios a la interfaz de usuario](/learn/modules/enhance-ui-of-windows-10-app/) o [implementar el enlace de datos](/learn/modules/implement-data-binding-in-windows-10-app/).

* Si está interesado en el desarrollo web, consulte [desarrollar aplicaciones web con Visual Studio Code](/learn/modules/develop-web-apps-with-vs-code/) o [compilar un sitio web sencillo](/learn/modules/build-simple-website/).

* Como alternativa, no dude en explorar [todos los módulos de desarrollador de Windows en Microsoft Learn](/learn/browse/?products=windows&resource_type=module).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicaciones web progresivas

Las aplicaciones web progresivas son sitios web que funcionan como aplicaciones nativas en distintos exploradores y una amplia variedad de dispositivos Windows 10. [Mire el vídeo](https://youtu.be/ugAewC3308Y) para conocer más y luego [eche un vistazo a los documentos](https://developer.microsoft.com/windows/pwa) para obtener más información.

### <a name="vs-code-series"></a>Serie de código de VS

Visite nuestra [nueva serie de vídeos sobre Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obtener información sobre VSCode, cómo usarlo y cómo se creó.

### <a name="mixed-reality-services"></a>Servicios de realidad mixta

HoloLens 2 se anunció recientemente. Consulte esta [serie de vídeos en realidad mixta](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST) para obtener la información más reciente y saber cómo puede participar y empezar a desarrollar.

### <a name="one-dev-question"></a>Una pregunta de desarrollador

En la serie de vídeos de una pregunta de desarrollador, los desarrolladores de Microsoft cubren una serie de preguntas acerca del desarrollo de Windows, la cultura del equipo y el historial.

* [Raymond Chen en la historia y el desarrollo de Windows](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman en la historia y el desarrollo de Windows](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson en aplicaciones web progresivas](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann en la herramienta webhint](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
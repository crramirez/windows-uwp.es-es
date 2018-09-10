---
author: QuinnRadich
title: 'Novedades en los documentos de Windows de mayo de 2018: desarrollar aplicaciones para UWP'
description: Se agregaron nuevas características, vídeos y directrices para los desarrolladores a la documentación de desarrolladores de Windows 10 de mayo de 2018 y la conferencia de Microsoft Build.
keywords: Novedades, actualización, características, directrices para los desarrolladores, Windows 10, mayo, compilación
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "3824059"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Novedades de la documentación de desarrolladores de Windows de mayo de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información, vídeos, directrices para los desarrolladores y muestras que se han puesto a disposición en el mes de mayo para que coincida con la conferencia de desarrolladores de [Microsoft de 2018 de compilación](https://www.microsoft.com/build) .

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="motion-in-fluent-design"></a>Movimiento en Fluent Design

El usuario de movimiento en Fluent Design System evoluciona, integrada en los conceptos básicos de temporización, aceleración, direccionalidad y gravedad. Estos conceptos básicos de la aplicación te ayudarán a guiar al usuario a través de la aplicación y se conectan con su experiencia digital, que refleja el mundo natural. Más información en este artículo:

* [Introducción al movimiento the](../design/motion/index.md) se ha actualizado para reflejar estos conceptos básicos.
* [Movimiento en la práctica](../design/motion/motion-in-practice.md) se proporcionan ejemplos de cómo aplicar estos conceptos básicos de dentro de la aplicación.
* [Direccionalidad y gravedad](../design/motion/directionality-and-gravity.md) solidifica su modelo mental del usuario la aplicación.
* [Sincronización y aceleración](../design/motion/timing-and-easing.md) agrega realismo para el movimiento de la aplicación.

![Movimiento en acción](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Actualizaciones de Fluent Design

Se han realizado las actualizaciones visuales y pequeños cambios a las páginas de Fluent Design siguientes:

* [Alineación, margen, los márgenes](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [Conceptos básicos de comandos](../design/basics/commanding-basics.md)
* [Fluent Design para las aplicaciones de Windows](../design/fluent-design-system/index.md)
* [Introducción al diseño de aplicaciones](../design/basics/design-and-ui-intro.md)
* [Conceptos básicos de navegación](../design/basics/navigation-basics.md)
* [Técnicas de diseño con capacidad de respuesta](../design/layout/responsive-design.md)
* [Tamaños de pantalla y puntos de interrupción](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Información general de estilo](../design/style/index.md)
* [Estilo de escritura](../design/style/writing-style.md)

Además, nos hemos vuelto a escribir las páginas siguientes con nueva información acerca de sus áreas de contenido:

* [Iconos](../design/style/icons.md) ahora proporciona recomendaciones prácticas para usar iconos y que sean seleccionables.
* [Tipografía](../design/style/typography.md) consolida la información de artículos similares, poner todo en un solo lugar con instrucciones actualizadas e ilustraciones.

![Imagen de la paleta de colores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Archivos de instalador de aplicación en Visual Studio

Archivos del instalador de aplicación ahora pueden crearse con Visual Studio 2017, Update 15.7. [Aprende a usar Visual Studio para crear un archivo del instalador de aplicación](../packaging/create-appinstallerfile-vs.md) y habilitar las actualizaciones automáticas a tus aplicaciones. Si estás teniendo problemas, consulta la [solución de problemas de instalación con el archivo del instalador de aplicación](../packaging/troubleshoot-appinstaller-issues.md) para ver los problemas y soluciones comunes.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Bordes de control de WebView para aplicaciones de Windows Forms y WPF

Mostrar el contenido web en la aplicación de escritorio mediante el control de WebView, anteriormente solo está disponible para aplicaciones para UWP. Este control usa el motor para insertar una vista que representa el formato enriquecido HTML de representación contenido desde un servidor web remoto, el código generado dinámicamente o archivos de contenido de Microsoft Edge. Busca el control WebView en la versión más reciente de la [Kit de herramientas de comunidad Windows.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Busca otros controles como WebView en futuras versiones del Kit de herramientas de comunidad Windows. Para obtener más información, consulta [Host UWP controla en aplicaciones de WPF y Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Entrada de mirada e interacciones

[Realiza un seguimiento de la mirada, la atención y la presencia del usuario en función de la ubicación y del movimiento de sus ojos.](../design/input/gaze-interactions.md) Este eficaz modo nuevo usar e interactuar con las aplicaciones para UWP es especialmente útil como tecnología de asistencia. La entrada de mirada también ofrece atractivas oportunidades para juegos (incluida adquisición de destino y seguimiento) y otros escenarios interactivos donde dispositivos de entrada tradicionales (teclado, mouse, entrada táctil) no están disponibles.

### <a name="msix-packaging-format"></a>Formato de empaquetado de MSIX

Presentó en la conferencia de Microsoft de 2018 de compilación, MSIX es un nuevo formato de paquete de inclusión en contenedores que se aplica a todas las aplicaciones de Windows como Win32, Windows Forms, WPF y UWP. Este nuevo formato hereda excelentes características de UWP:

* Instalación sólida y la actualización. 
* Administra el modelo de seguridad con un sistema de funcionalidad flexible.
* Compatibilidad con la Microsoft Store, la administración de empresa y muchos de los modelos de distribución personalizado.

Herramientas para crear estos paquetes estarán disponibles en una versión futura de Visual Studio y SDK de Windows.

El formato de empaquetado MSIX es un formato de código abierto que es más fácil para nuestros partners admitir el ecosistema MSIX con sus soluciones y herramientas. Para obtener más información sobre el formato de empaquetado MSIX, consulte [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Imagen de empaquetado de MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Paquetes opcionales con código ejecutable

Paquetes opcionales en la aplicación ahora pueden contener código ejecutable de C#. [Aprende a usar Visual Studio para configurar paquetes de complemento opcional para admitir el paquete de la aplicación principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transiciones de página

[Las transiciones de página](../design/motion/page-transitions.md) navegar a los usuarios entre las páginas de una aplicación. Ayudan a los usuarios comprender dónde se encuentran en la jerarquía de navegación y proporcionar comentarios sobre la relación entre las páginas.

### <a name="project-rome"></a>Proyecto Roma

El equipo de Project Rome ha renovado su iOS y Android SDK, agregando nuevas características como las actividades del usuario y la refactorización de gran parte de su código para proporcionar una experiencia coherente de programación a través de los SDK diferentes. [Todos los nueva referencia de API y documentos de procedimientos](https://docs.microsoft.com/windows/project-rome/) pasarán dinámicos durante la conferencia de desarrolladores de compilación de 2018.

### <a name="sets"></a>Conjuntos

La funcionalidad conjuntos está disponible en las versiones preliminares de Windows Insider. Al usar la característica de conjuntos, tu aplicación se dibuja en una ventana que puede compartirse con otras aplicaciones, donde cada aplicación tiene su propia pestaña en la barra de título. [Diseños para conjuntos](../design/shell/design-for-sets.md) tiene instrucciones sobre cómo optimizar tu aplicación para proporcionar la mejor experiencia en la interfaz de usuario de conjuntos.

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="get-started"></a>Comenzar

Nos hemos revitalizado nuestra Get a contenido con nuevas pistas de aprendizaje. Estos temas nuevos se pretende proporcionar nuevos desarrolladores de Windows 10 con información sobre algunas tareas comunes que es posible que quieren para llevar a cabo. No son tutoriales y no proporciona un tutorial de mano, pero en su lugar señale que exista documentación existente y cómo usarla. Echa un vistazo a la actualización de la herramienta [empezar a codificar](../get-started/create-uwp-apps.md) la página o explorar cada pista de aprendizaje individual:

* [Construir un formulario](../get-started/construct-form-learning-track.md)
* [Mostrar clientes en una lista](../get-started/display-customers-in-list-learning-track.md)
* [Guardar y cargar la configuración](../get-started/settings-learning-track.md)
* [Trabajar con archivos](../get-started/fileio-learning-track.md)

![Obtener imagen iniciada](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Informe de rendimiento de la publicidad

Ahora, el [informe de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del centro de desarrollo proporciona métricas de visualización. También hemos agregado el artículo de [optimizar la visualización de las unidades de anuncios](../monetize/optimize-ad-unit-viewability.md) para proporcionar recomendaciones para optimizar la visualización de tus anuncios.

### <a name="targeted-push-notifications"></a>Notificaciones push dirigidas

Ahora, la página de [notificaciones](../publish/send-push-notifications-to-your-apps-customers.md) en el panel del centro de desarrollo proporciona datos de análisis adicionales para todas las notificaciones en las vistas de mapa gráfico y mapamundi.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C++ / WinRT es una nueva forma de crear y consumir Windows Runtime APIs. Ha implementada única en los archivos de encabezado y diseñada para darte acceso a las características de aplicación moderna. Para obtener información sobre cómo funciona, a continuación, [lee a los documentos de desarrollador](../cpp-and-winrt-apis/index.md) para obtener más información, [vea el vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) .

### <a name="multi-instance-uwp-apps"></a>Aplicaciones para UWP de varias instancias

Windows ahora te permite ejecutar varias instancias de la aplicación para UWP, con cada uno de ellos en su propio proceso independiente. [Ve el vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para obtener información sobre cómo crear una nueva aplicación que es compatible con esta característica, a continuación, [lee a los documentos de desarrollador](../launch-resume/multi-instance-uwp.md) para obtener más información sobre cómo y por qué se debe usar esta característica.

## <a name="samples"></a>Muestras

### <a name="customer-database-tutorial"></a>Tutorial de base de datos de cliente

En este tutorial se crea una aplicación básica de UWP para administrar una lista de clientes y presenta los conceptos y procedimientos recomendados útiles en el desarrollo empresarial. Te guiará a través de la implementación de elementos de la interfaz de usuario y agrega las operaciones en una base de datos SQLite local y se proporcionan instrucciones dinámica para la conexión a una base de datos REST remota si quieres ir más allá. [Echa un vistazo a este tutorial](../enterprise/customer-database-tutorial.md)
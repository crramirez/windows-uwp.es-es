---
title: 'Novedades de Windows Docs en mayo de 2018: desarrollar aplicaciones para UWP'
description: Instrucciones para desarrolladores, vídeos y las nuevas características se agregaron a la documentación para desarrolladores de Windows 10 de mayo de 2018 y el congreso Microsoft Build.
keywords: ¿Qué es nuevo, actualizar, características, instrucciones para desarrolladores, Windows 10, mayo, compilación
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598790"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Novedades en la documentación para desarrolladores de Windows en mayo de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general de características, instrucciones para desarrolladores, vídeos y ejemplos se encuentran disponibles en el mes de mayo para que coincida con el [Microsoft Build 2018](https://www.microsoft.com/build) congreso para desarrolladores.

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="motion-in-fluent-design"></a>Movimiento de diseño Fluent

El usuario de movimiento en el sistema de diseño Fluent evoluciona, basado en los fundamentos del control de tiempo, entradas y salidas lentas, direccionalidad y gravedad. Aplicar estos aspectos básicos le ayudará a guiar al usuario a través de la aplicación y los conecta con su experiencia digital reflejando del mundo natural. Obtenga más información en este artículo:

* [La información general del movimiento](../design/motion/index.md) se ha actualizado para reflejar estos aspectos básicos.
* [Movimiento en la práctica](../design/motion/motion-in-practice.md) se proporcionan ejemplos de cómo aplicar estos aspectos básicos dentro de la aplicación.
* [Direccionalidad y gravedad](../design/motion/directionality-and-gravity.md) solidifica su modelo mental del usuario de la aplicación.
* [Control de tiempo y facilitando la](../design/motion/timing-and-easing.md) agrega realismo y el movimiento en la aplicación.

![Movimiento en acción](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Actualizaciones de diseño Fluent

Se han realizado actualizaciones visuales y cambios menores a las siguientes páginas de diseño Fluent:

* [Alineación, relleno, márgenes](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [Conceptos básicos de comandos](../design/basics/commanding-basics.md)
* [Diseño Fluent para aplicaciones de Windows](../design/fluent-design-system/index.md)
* [Introducción al diseño de aplicaciones](../design/basics/design-and-ui-intro.md)
* [Conceptos básicos de navegación](../design/basics/navigation-basics.md)
* [Técnicas de diseño dinámico](../design/layout/responsive-design.md)
* [Tamaños de pantalla y puntos de interrupción](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Información general de estilo](../design/style/index.md)
* [Estilo de escritura](../design/style/writing-style.md)

Además, hemos reescrito las siguientes páginas con información nueva en sus áreas de contenido:

* [Iconos](../design/style/icons.md) proporciona ahora recomendaciones prácticas para utilizar iconos y lo que puede hacer clic en.
* [Tipografía](../design/style/typography.md) consolida la información de los artículos similares, ponerlo todo en un solo lugar con la guía actualizada e ilustraciones.

![Imagen de la paleta de colores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Archivos de instalador de la aplicación en Visual Studio

Ahora se pueden crear archivos de instalador de aplicaciones con Visual Studio 2017, versión 15.7 de actualización. [Aprenda a usar Visual Studio para crear un archivo de instalador de la aplicación](../packaging/create-appinstallerfile-vs.md) y habilitar las actualizaciones automáticas a las aplicaciones. Si experimenta problemas, consulte [solucionar problemas de instalación con el archivo instalador de la aplicación](../packaging/troubleshoot-appinstaller-issues.md) para ver los problemas y soluciones comunes.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Borde control WebView para aplicaciones de Windows Forms y WPF

Mostrar contenido web en su aplicación de escritorio mediante el control WebView, que anteriormente solo estaba disponible para aplicaciones UWP. Este control usa el motor para insertar una vista que representa el formato HTML enriquecido de procesamiento contenido desde un servidor web remoto, el código generado dinámicamente o los archivos de contenido de Microsoft Edge. Busque el control WebView en la versión más reciente de la [Windows Community Toolkit.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Busque otros controles, como que WebView en futuras versiones del Kit de herramientas de comunidad de Windows. Para obtener más información, consulte [Host UWP se controla en aplicaciones WPF y Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Entrada de mirada e interacciones

[Realizar un seguimiento de un usuario mirada, atención y presencia según la ubicación y el movimiento de sus ojos.](../design/input/gaze-interactions.md) Esta nueva y poderosa manera de usar e interactuar con sus aplicaciones para UWP es especialmente útil como una tecnología de ayuda. Entrada de mirada también proporciona atractivas oportunidades para juegos (incluida la adquisición de destino y de seguimiento) y otros escenarios interactivos donde los dispositivos de entrada tradicionales (teclado, mouse, toque) no están disponibles.

### <a name="msix-packaging-format"></a>Formato de empaquetado MSIX

Se anunció en el congreso Microsoft Build 2018, MSIX es un nuevo formato de paquete de inclusión en contenedores que se aplica a todas las aplicaciones de Windows incluidas Win32, Windows Forms, WPF y UWP. Este nuevo formato hereda características excelentes de UWP:

* Instalación estable y actualización. 
* Modelo de seguridad con un sistema flexible de capacidad de administrar.
* Compatibilidad con la Microsoft Store, administración empresarial y muchos modelos de distribución personalizada.

Herramientas para crear estos paquetes estarán disponibles en una versión futura de Visual Studio y SDK de Windows.

El formato de empaquetado MSIX es un formato de código abierto que resulta más fácil para nuestros asociados admitir el ecosistema MSIX con sus herramientas y soluciones. Para obtener más información sobre el formato de empaquetado MSIX, consulte [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Imagen de empaquetado MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Paquetes opcionales con código ejecutable

Paquetes opcionales en la aplicación pueden contener ahora el archivo ejecutable C# código. [Aprenda a usar Visual Studio para configurar los paquetes de complementos opcional para admitir el paquete de aplicación principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transiciones de página

[Transiciones de página](../design/motion/page-transitions.md) navegar a los usuarios entre las páginas de una aplicación. Ayudan a los usuarios a comprender dónde se encuentra en la jerarquía de navegación y proporcionar comentarios sobre la relación entre las páginas.

### <a name="project-rome"></a>Proyecto Roma

El equipo de proyecto Roma ha mejorado sus iOS y Android SDK, refactorización gran parte de su código para proporcionar una experiencia de programación coherente entre los diferentes SDK y agregar nuevas características, como las actividades del usuario. [Todos los nuevos procedimientos y referencia de los documentos de API](https://docs.microsoft.com/windows/project-rome/) empezarán a funcionar durante la conferencia para desarrolladores Build 2018.

### <a name="sets"></a>Conjuntos

La característica conjuntos está disponible en las compilaciones de versión preliminar de Insider de Windows. Al utilizar la característica de conjuntos, la aplicación se dibuja en una ventana que se puede compartir con otras aplicaciones, con cada aplicación tiene su propia pestaña en la barra de título. 

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="get-started"></a>Comenzar

Nos hemos revitalizado nuestras a contenido con pistas de aprendizaje de nuevo. Estos nuevos temas pretenden ofrecer nuevos desarrolladores de Windows 10 con información sobre algunas tareas comunes que quizás desee realizar. No está tutoriales y no proporciona un tutorial de mano, pero señalar donde existe documentación existente y cómo usarlo. Consulte la renovada [comenzar a codificar](../get-started/create-uwp-apps.md) página o explorar cada pista learning individuales:

* [Construcción de un formulario](../get-started/construct-form-learning-track.md)
* [Mostrar los clientes en una lista](../get-started/display-customers-in-list-learning-track.md)
* [Guardar y cargar la configuración](../get-started/settings-learning-track.md)
* [Trabajar con archivos](../get-started/fileio-learning-track.md)

![Obtener imagen de introducción](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Informe de rendimiento de la publicidad

El [anunciar el informe de rendimiento](../publish/advertising-performance-report.md) en el centro de partners ahora proporciona métricas de capacidad de visualización. También hemos agregado la [optimizar la capacidad de visualización de las unidades de ad](../monetize/optimize-ad-unit-viewability.md) artículo para proporcionar recomendaciones para optimizar la capacidad de visualización de los anuncios.

### <a name="targeted-push-notifications"></a>Notificaciones push dirigidas

El [notificaciones](../publish/send-push-notifications-to-your-apps-customers.md) página en el centro de partners ahora proporciona datos de análisis adicional para todas las notificaciones en las vistas de mapa de gráfico y el mundo.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C++ / c++ / WinRT es una nueva forma de creación y consumo de Windows Runtime APIs. Tiene implementa sole en los archivos de encabezado y se ha diseñado para proporcionar acceso a las características de aplicaciones modernas de primera clase. [Vea el vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) para saber cómo funciona, a continuación, [leer los documentos para desarrolladores](../cpp-and-winrt-apis/index.md) para obtener más información.

### <a name="multi-instance-uwp-apps"></a>Aplicaciones para UWP de varias instancias

Windows ahora le permite ejecutar varias instancias de la aplicación para UWP, con cada uno en su propio proceso independiente. [Vea el vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para obtener información sobre cómo crear una nueva aplicación que admita esta característica, a continuación, [leer los documentos para desarrolladores](../launch-resume/multi-instance-uwp.md) para obtener instrucciones sobre cómo y por qué usar esta característica.

## <a name="samples"></a>Muestras

### <a name="customer-database-tutorial"></a>Tutorial de base de datos de cliente

Este tutorial crea una aplicación básica de UWP para administrar una lista de clientes y presenta los conceptos y procedimientos recomendados útiles en el desarrollo empresarial. Le guía a través de la implementación de elementos de interfaz de usuario y la adición de las operaciones en una base de datos SQLite local y se proporcionan instrucciones flexible para conectarse a una base de datos remota de REST si quiere ir más allá. [Consulte el tutorial aquí](../enterprise/customer-database-tutorial.md)
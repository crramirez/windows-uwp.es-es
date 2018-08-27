---
author: QuinnRadich
title: 'Novedades en documentos de Windows de mayo de 2018: desarrollar aplicaciones UWP'
description: Instrucciones de desarrollo, vídeos y nuevas características se han agregado a la documentación para desarrolladores de Windows 10 de mayo de 2018 y la conferencia de Microsoft Build.
keywords: ¿Qué es nuevo, actualizar, sobre las características, instrucciones de desarrollo, Windows 10, mayo, compilación
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 322bc056411095019dfc027078cbfef7de0883fb
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2856039"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Novedades en los documentos para desarrolladores de Windows de mayo de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general de características, instrucciones de desarrollo, vídeos y ejemplos esté a disposición en el mes de mayo para que coincida con la conferencia de desarrolladores de [Microsoft compilación 2018](https://www.microsoft.com/build) .

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="motion-in-fluent-design"></a>Movimiento de diseño Fluent

El usuario de movimiento en el sistema de diseño Fluent está evolucionando, basadas en los aspectos básicos de control de tiempo, aceleración, direccionalidad y gravedad. Al aplicar estos conceptos básicos de le ayudará a guiar al usuario a través de la aplicación y conecta con su experiencia digital por lo que refleja el mundo natural. Encontrará más información en este artículo:

* [Información general del movimiento](../design/motion/index.md) se actualizó para reflejar estos aspectos básicos.
* [Movimiento en la práctica](../design/motion/motion-in-practice.md) se proporcionan ejemplos de cómo aplicar estos conceptos básicos de dentro de la aplicación.
* [Direccionalidad y gravity](../design/motion/directionality-and-gravity.md) solidifica su modelo mental del usuario de la aplicación.
* [Control de tiempo y aceleración](../design/motion/timing-and-easing.md) agrega realismo al movimiento en la aplicación.

![Movimiento en acción](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Actualizaciones de diseño Fluent

Se han realizado pequeños cambios y actualizaciones Visual a las páginas de diseño Fluent siguientes:

* [Alineación, espaciado, márgenes](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [Conceptos básicos de comandos](../design/basics/commanding-basics.md)
* [Diseño de Fluent para aplicaciones de Windows](../design/fluent-design-system/index.md)
* [Introducción al diseño de la aplicación](../design/basics/design-and-ui-intro.md)
* [Conceptos básicos de navegación](../design/basics/navigation-basics.md)
* [Técnicas de diseño con capacidad de respuesta](../design/layout/responsive-design.md)
* [Tamaños de pantalla y puntos de interrupción](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Información general de estilo](../design/style/index.md)
* [Estilo de escritura](../design/style/writing-style.md)

Además, nos hemos vuelto a escribir las siguientes páginas con información de todas las novedades en sus áreas de contenido:

* Ahora, [los iconos](../design/style/icons.md) proporciona recomendaciones prácticas para el uso de iconos y hacer que se puede hacer clic.
* [Tipografía](../design/style/typography.md) consolida la información de artículos similares, incluirlo todo en un solo lugar con instrucciones actualizadas e ilustraciones.

![Imagen de la paleta de colores](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Archivos del instalador de la aplicación en Visual Studio

Ahora se pueden crear los archivos del instalador de la aplicación con Visual Studio 2017, 15.7 de actualización. [Aprenda a usar Visual Studio para crear un archivo de instalador de la aplicación](../packaging/create-appinstallerfile-vs.md) y habilitar actualizaciones automáticas para sus aplicaciones. Si se está ejecutando en problemas, vea [solución de problemas de instalación con el archivo del instalador de la aplicación](../packaging/troubleshoot-appinstaller-issues.md) para ver los problemas y soluciones comunes.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Borde de control de vista Web para las aplicaciones de Windows Forms y WPF

Mostrar contenido web en su aplicación de escritorio mediante el control de vista Web, anteriormente, sólo está disponible para las aplicaciones de UWP. Este control usa la representación de motor de una vista que representa el formato HTML enriquecido para insertar contenido desde un servidor web remoto, el código generado dinámicamente o archivos de contenido de Microsoft Edge. Busque el control de vista Web en la versión más reciente de la [Kit de herramientas de la Comunidad de Windows.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Busque otros controles como vista Web en futuras versiones del Kit de herramientas de la Comunidad de Windows. Para obtener más información, vea [Host UWP controla en aplicaciones de WPF y Windows Forms.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Entrada y las interacciones que mirar

[Realiza un seguimiento de la mirada, la atención y la presencia del usuario en función de la ubicación y del movimiento de sus ojos.](../design/input/gaze-interactions.md) Esta nueva y poderosa manera de utilizar e interactuar con sus aplicaciones UWP es especialmente útil como una tecnología de ayuda. Entrada que mirar también ofrece oportunidades atractivas para juegos (incluida la adquisición de destino y de seguimiento) y otros escenarios interactivas donde tradicionales dispositivos de entrada (teclado, mouse, táctil) no están disponibles.

### <a name="msix-packaging-format"></a>Formato de empaquetado MSIX

Anunciado en la conferencia de Microsoft compilación 2018, MSIX es un nuevo formato de paquete de containerization que se aplica a todas las aplicaciones de Windows, incluidos Win32, formularios Windows Forms, WPF y UWP. Este nuevo formato hereda excelentes características de UWP:

* Instalación robusta y la actualización. 
* Modelo de seguridad con un sistema de capacidad flexible administrado.
* Compatibilidad con el Microsoft Store, administración empresarial y muchos modelos de distribución personalizados.

Herramientas para crear estos paquetes estará disponibles en una futura versión de Visual Studio y SDK de Windows.

El formato de empaquetado de MSIX es un formato de código abierto que facilita a los socios admitir el ecosistema MSIX con sus herramientas y soluciones. Para obtener más información acerca del formato de empaquetado MSIX, vea el [SDK de MSIX](https://github.com/Microsoft/msix-packaging). 

![Imagen de empaquetado MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Paquetes opcionales con código ejecutable

Paquetes opcionales en la aplicación ahora pueden contener código ejecutable de C#. [Obtenga información sobre cómo usar Visual Studio para configurar los paquetes de complemento opcional para que admita el paquete de la aplicación principal.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Transiciones de página

[Transiciones de página](../design/motion/page-transitions.md) navegue a los usuarios entre las páginas de una aplicación. Ayudan a los usuarios comprender dónde se encuentran en la jerarquía de navegación y proporcionar comentarios sobre la relación entre las páginas.

### <a name="project-rome"></a>Proyecto Roma

El equipo de proyecto Roma ha reacondicionado sus iOS y Android SDK, refactorización gran parte de su código para proporcionar una experiencia coherente de programación a través de los diferentes SDK y agregar nuevas características como las actividades del usuario. [Todos los nueva referencia de API y documentos de procedimientos](https://docs.microsoft.com/windows/project-rome/) se realizarán live durante la conferencia de desarrolladores de compilación 2018.

### <a name="sets"></a>Conjuntos de

La característica conjuntos de está disponible en Windows Vista previa se basa el especialista. Cuando se usa la característica de conjuntos, aplicación se dibuja en una ventana que es posible que se comparten con otras aplicaciones, con cada aplicación tiene su propia ficha en la barra de título. [Diseño para conjuntos](../design/shell/design-for-sets.md) tiene instrucciones acerca de cómo optimizar la aplicación para proporcionar la mejor experiencia en la interfaz de usuario establece.

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="get-started"></a>Introducción

Nos hemos revitalizado nuestro Get a contenido con nuevas pistas de aprendizaje. Estos nuevos temas se pretende proporcionar los nuevos programadores 10 de Windows con información en algunas tareas comunes que es posible que desee realizar. No está tutoriales y no proporciona un tutorial de mano, pero señale donde se encuentra la documentación existente y cómo usarla. Desproteger la actualización de la herramienta [iniciar la codificación](../get-started/create-uwp-apps.md) de página o explore cada pista de aprendizaje individual:

* [Construir un formulario](../get-started/construct-form-learning-track.md)
* [Mostrar clientes en una lista](../get-started/display-customers-in-list-learning-track.md)
* [Guardar y cargar la configuración](../get-started/settings-learning-track.md)
* [Trabajar con archivos](../get-started/fileio-learning-track.md)

![Obtener imagen de introducción](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Informe de rendimiento de la publicidad

Ahora, el [informe de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del centro de desarrollo proporciona métricas de capacidad de visualización. También se agregó el artículo [optimizar la capacidad de visualización de las unidades de ad](../monetize/optimize-ad-unit-viewability.md) para proporcionar recomendaciones para optimizar la capacidad de visualización de los anuncios.

### <a name="targeted-push-notifications"></a>Notificaciones de inserción dirigido

Ahora, la página de [notificaciones](../publish/send-push-notifications-to-your-apps-customers.md) en el panel del centro de desarrollo de proporciona datos de análisis adicionales para todas las notificaciones en las vistas de mapa gráfico y world.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C + + / WinRT es una nueva forma de creación y consumo de Windows Runtime APIs. Tiene implementado único en archivos de encabezado y diseñada para proporcionar acceso a las características de aplicación moderna de primera clase. Para obtener más información cómo funciona, a continuación, [Lea a los documentos para desarrolladores](../cpp-and-winrt-apis/index.md) para obtener más información, [vea el vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) .

### <a name="multi-instance-uwp-apps"></a>Aplicaciones para UWP de varias instancias

Windows ahora le permite ejecutar varias instancias de la aplicación UWP, con cada uno en su propio proceso independiente. [Vea el vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para aprender a crear una nueva aplicación que es compatible con esta característica, a continuación, [Lea a los documentos para desarrolladores](../launch-resume/multi-instance-uwp.md) para obtener más información acerca de cómo y por qué usar esta característica.

## <a name="samples"></a>Muestras

### <a name="customer-database-tutorial"></a>Tutorial de base de datos de cliente

Este tutorial crea una aplicación UWP básica para la administración de una lista de clientes y presenta los conceptos y prácticas útiles en el desarrollo empresarial. Le guiará a través de la implementación de elementos de la interfaz de usuario y la adición de las operaciones en una base de datos de SQLite local y se proporcionan instrucciones separado para conectarse a una base de datos remota de REST si desea ir más allá. [Desproteger el tutorial aquí](../enterprise/customer-database-tutorial.md)
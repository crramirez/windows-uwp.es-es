---
title: 'Novedades en la documentación de Windows de mayo de 2018: desarrollar aplicaciones para UWP'
description: Se han agregado nuevas características, vídeos y directrices para desarrolladores a la documentación para los desarrolladores de Windows 10 de mayo de 2018 y el congreso Microsoft Build.
keywords: novedades, actualización, características, directrices para desarrolladores, Windows 10, mayo, Build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d8b72501c298f3814092ec4567a5fb608c4bb88f
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682764"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Novedades en la documentación para desarrolladores de Windows de mayo de 2018

La documentación para desarrolladores de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general de características, instrucciones para desarrolladores, vídeos y ejemplos se encuentra disponibles en el mes de mayo para que coincida con el congreso para desarrolladores [Microsoft Build 2018](https://www.microsoft.com/build/).

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="motion-in-fluent-design"></a>Movimiento en Fluent Design

El uso de movimiento en el sistema Fluent Design está evolucionando sobre la base de los principios básicos de control de tiempo, aceleración, direccionalidad y gravedad. Aplicar estos principios básicos ayudará a guiar al usuario a través de la aplicación y lo conectará con su experiencia digital al reflejar el mundo natural. Aprenda más en estos artículos:

* [La introducción al movimiento](../design/motion/index.md) se ha actualizado para reflejar estos principios básicos.
* En [Movimiento en la práctica](../design/motion/motion-in-practice.md) se proporcionan ejemplos de cómo aplicar estos principios básicos en la app.
* En [Direccionalidad y gravedad](../design/motion/directionality-and-gravity.md) se afianza el modelo mental del usuario respecto de la aplicación.
* En [Control de tiempo y aceleración](../design/motion/timing-and-easing.md) se agrega realismo al movimiento en la aplicación.

![Movimiento en acción](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Actualizaciones de Fluent Design

Se han realizado actualizaciones de objetos visuales y cambios menores en las siguientes páginas de Fluent Design:

* [Alineación, relleno, márgenes](../design/layout/alignment-margin-padding.md)
* [Color](../design/style/color.md)
* [Conceptos básicos de comandos](../design/basics/commanding-basics.md)
* [Fluent Design para aplicaciones de Windows](../design/fluent-design-system/index.md)
* [Introducción al diseño de aplicaciones](../design/basics/design-and-ui-intro.md)
* [Conceptos básicos de navegación](../design/basics/navigation-basics.md)
* [Técnicas de diseño con capacidad de respuesta](../design/layout/responsive-design.md)
* [Tamaños de pantalla y puntos de interrupción](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Información general sobre estilo](../design/style/index.md)
* [Estilo de escritura](../design/style/writing-style.md)

Además, hemos vuelto a redactar las siguientes páginas con información nueva sobre las siguientes áreas de contenido:

* En [Iconos](../design/style/icons.md) ahora se proporcionan recomendaciones prácticas para usar iconos y que se pueda hacer clic en ellos.
* En [Tipografía](../design/style/typography.md) se consolida la información de artículos similares, lo que permite tener todo en una misma ubicación con instrucciones e ilustraciones actualizadas.

![Imagen de la paleta de colores.](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>Archivos de instalador de aplicación en Visual Studio

Ahora se pueden crear archivos de instalador de aplicaciones con Visual Studio 2017, versión 15.7 de actualización y otras versiones posteriores. [Aprenda a usar Visual Studio para crear un archivo de instalador de la aplicación](../packaging/create-appinstallerfile-vs.md) y habilitar las actualizaciones automáticas a las aplicaciones. Si experimenta problemas, consulte [solucionar problemas de instalación con el archivo instalador de la aplicación](../packaging/troubleshoot-appinstaller-issues.md) para ver los problemas y soluciones comunes.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Control WebView de Edge para aplicaciones de Windows Forms y WPF

Muestre contenido web en su aplicación de escritorio mediante el control WebView, que anteriormente solo estaba disponible para aplicaciones UWP. Este control usa el motor de representación Microsoft Edge para insertar una vista que representa el contenido con formato HTML enriquecido desde un servidor web remoto, el código generado dinámicamente o los archivos de contenido. Encuentre el control WebView en la versión más reciente del [Kit de herramientas de Comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).

Busque otros controles como el WebView en futuras versiones del Kit de herramientas de Comunidad Windows. Para obtener más información, consulte [Hospedar los controles de UWP en aplicaciones de WPF y Windows Forms](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

### <a name="gaze-input-and-interactions"></a>Entrada e interacciones de mirada

[Realice un seguimiento de la mirada, la atención y la presencia del usuario en función de la ubicación y del movimiento de sus ojos.](../design/input/gaze-interactions.md) Esta nueva y poderosa manera de usar e interactuar con sus aplicaciones para UWP es especialmente útil como una tecnología de asistencia. La entrada de mirada también proporciona atractivas oportunidades para juegos (incluida la adquisición de destino y de seguimiento) y otros escenarios interactivos donde los dispositivos de entrada tradicionales (teclado, mouse, entrada táctil) no están disponibles.

### <a name="msix-packaging-format"></a>Formato de empaquetado MSIX

Anunciado en el congreso Microsoft Build 2018, MSIX es un nuevo formato de paquete de inclusión en contenedores que se aplica a todas las aplicaciones de Windows, incluidas Win32, Windows Forms, WPF y UWP. Este nuevo formato hereda características excelentes de UWP:

* Instalación sólida y actualización. 
* Modelo de seguridad administrado con un sistema de funcionalidad flexible.
* Compatibilidad con la Microsoft Store, administración empresarial y muchos modelos de distribución personalizada.

Las herramientas para crear estos paquetes estarán disponibles en una versión futura de Visual Studio y SDK de Windows.

El formato de empaquetado MSIX es un formato de código abierto que resulta más fácil para nuestros asociados admitir el ecosistema MSIX con sus herramientas y soluciones. Para obtener más información sobre el formato de empaquetado MSIX, consulte [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Imagen de empaquetado MSIX](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Paquetes opcionales con código ejecutable

Paquetes opcionales en la aplicación pueden contener ahora el código ejecutable C#. [Aprenda a usar Visual Studio para configurar los paquetes de complementos opcionales para admitir el paquete de aplicación principal.](/windows/msix/package/optional-packages)

### <a name="page-transitions"></a>Transiciones de página

Las [transiciones de página](../design/motion/page-transitions.md) permiten a los usuarios navegar entre las páginas de una aplicación. Ayudan a los usuarios a comprender dónde se encuentran en la jerarquía de navegación, además de que proporcionan comentarios sobre la relación entre las páginas.

### <a name="project-rome"></a>Proyecto Roma

El equipo del Proyecto Roma ha mejorado sus SDK para iOS y Android, agregando nuevas características como Actividades del usuario y refactorizando gran parte de su código para proporcionar una experiencia de programación coherente entre los diferentes SDK. [Todos los nuevos documentos de procedimientos y referencia de API](https://docs.microsoft.com/windows/project-rome/) empezarán a funcionar durante el congreso para desarrolladores Build 2018.

### <a name="sets"></a>Conjuntos

La característica de Conjuntos está disponible en las compilaciones de versión preliminar de Windows Insider. Al utilizar la característica de Conjuntos, la aplicación se dibuja en una ventana que se puede compartir con otras aplicaciones, y cada aplicación tiene su propia pestaña en la barra de título. 

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="get-started"></a>Comenzar

Hemos revitalizado nuestro contenido de Introducción con nuevas pistas de aprendizaje. Estos nuevos temas pretenden ofrecer a los nuevos desarrolladores de Windows 10 información sobre algunas tareas comunes que quizás deseen realizar. No son tutoriales y no proporcionan un tutorial de mano, pero muestran dónde existe documentación y cómo usarla. Consulte la renovada página [Comenzar a codificar](../get-started/create-uwp-apps.md) o explore cada pista de aprendizaje individual:

* [Construir un formulario](../get-started/construct-form-learning-track.md)
* [Mostrar clientes en una lista](../get-started/display-customers-in-list-learning-track.md)
* [Guardar y cargar la configuración](../get-started/settings-learning-track.md)
* [Trabajar con archivos](../get-started/fileio-learning-track.md)

![Imagen de Introducción.](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Informe de rendimiento de la publicidad

El [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md) en el Centro de partners ahora proporciona métricas de visualización. También hemos agregado el artículo [Optimizar la capacidad de visualización de las unidades de anuncios](../monetize/optimize-ad-unit-viewability.md) para proporcionar recomendaciones para optimizar visualización de sus anuncios.

### <a name="targeted-push-notifications"></a>Notificaciones push dirigidas

La página [Notificaciones](../publish/send-push-notifications-to-your-apps-customers.md) en el Centro de partners ahora proporciona datos de análisis adicional para todas las notificaciones en las vistas de gráficos y mapas mundiales.

## <a name="videos"></a>Vídeos

### <a name="cwinrt"></a>C++/WinRT

C++/ WinRT es una nueva forma de crear y consumir distintas API de Windows Runtime. Se ha implementado únicamente en los archivos de encabezado y está diseñada para ofrecerle acceso de primera clase a las características de las aplicaciones modernas. [Mire el vídeo](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) para conocer cómo funciona y, luego, [lea los documentos para desarrolladores](../cpp-and-winrt-apis/index.md) para obtener más información.

### <a name="multi-instance-uwp-apps"></a>Aplicaciones para UWP de varias instancias

Windows ahora le permite ejecutar varias instancias de la aplicación para UWP, cada una en su propio proceso independiente. [Mire el vídeo](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) para aprender cómo crear una nueva aplicación que admita esta característica y, después, [lea los documentos para desarrolladores](../launch-resume/multi-instance-uwp.md) para obtener instrucciones sobre cómo y por qué usar esta característica.

## <a name="samples"></a>Muestras

### <a name="customer-database-tutorial"></a>Tutorial sobre base de datos de cliente

En este tutorial se crea una aplicación para UWP básica para administrar una lista de clientes y se presentan conceptos y procedimientos que resultarán útiles en el desarrollo para empresas. Con este tutorial podrá recorrer los pasos para la implementación de elementos de la interfaz de usuario y la incorporación de operaciones en una base de datos SQLite local. También encontrará instrucciones detalladas para conectarte a una base de datos remota de REST si quiere avanzar más. [Consulte el tutorial aquí](../enterprise/customer-database-tutorial.md)
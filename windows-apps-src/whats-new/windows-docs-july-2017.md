---
title: 'Novedades de la documentación de Windows de julio de 2017: desarrollar aplicaciones para UWP'
description: En la documentación para desarrolladores de Windows 10 de julio de 2017 se han agregado nuevas características, ejemplos y directrices para los desarrolladores
keywords: what's new;update;features;developer guidance;Windows 10; novedades;actualización;características;directrices para desarrolladores
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 65c3c2fb4b7a5a7f0b5f4b3c89773f3e21bd654d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684738"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>Novedades de la documentación para desarrolladores de Windows de julio de 2017

La documentación para desarrolladores de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general, directrices para los desarrolladores y ejemplos de código sobre las características están a tu disposición desde hace poco y contienen información nueva y actualizada para los desarrolladores de Windows.

[Instala las herramientas y el SDK](https://developer.microsoft.com/windows/downloads#_blank) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/your-first-app.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="fluent-design"></a>Fluent Design

Disponible para [usuarios de Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, estos nuevos efectos usan la profundidad, la perspectiva y el movimiento para que los usuarios puedan centrarse en los elementos de interfaz de usuario más importantes.

[Material acrílico](../design/style/acrylic.md) es un tipo de pincel que crea texturas transparentes. 

![Acrílico en un tema claro](../design/style/images/Acrylic_DarkTheme_Base.png)

El [efecto de paralaje](../design/motion/parallax.md) agrega profundidad tridimensional y perspectiva a la aplicación.

![Ejemplo de efecto de paralaje con una lista y una imagen en segundo plano](../design/style/images/_Parallax_v2.gif)

[Mostrar](../design/style/reveal.md) resalta los elementos importantes de la aplicación. 

![Elemento visual Mostrar](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>Controles de la interfaz de usuario

Disponible para [usuarios de Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, estos nuevos controles facilitan la creación rápida de una interfaz de usuario inigualable.

El [control del selector de colores](../design/controls-and-patterns/color-picker.md) permite a los usuarios examinar y seleccionar colores.  

![Selector de colores predeterminado](../design/controls-and-patterns/images/color-picker-default.png)

El [control de la vista de navegación](../design/controls-and-patterns/navigationview.md) facilita el proceso para agregar navegación de nivel superior a la aplicación.

![Secciones de NavigationView](../design/controls-and-patterns/images/navview_sections.png)

El [control de imagen del usuario](../design/controls-and-patterns/person-picture.md) muestra la imagen de avatar de una persona.

![Control de imagen del usuario](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

El [control de clasificación](../design/controls-and-patterns/rating.md) permite a los usuarios ver y establecer clasificaciones que reflejan el grado de satisfacción con el contenido y los servicios.

![Ejemplo de control de clasificaciones](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>Kits de herramientas de diseño

Los [kits de herramientas de diseño y los recursos para aplicaciones para UWP](../design/downloads/index.md) se han ampliado con la incorporación de los kits de herramientas de Sketch y Adobe XD. Los kits de herramientas ya existentes también se han actualizado y renovado, proporcionando controles más sólidos y plantillas de diseño para las aplicaciones para UWP.

### <a name="dashboard-monetization-and-store-services"></a>Panel de información, monetización y servicios de Store

Las siguientes nuevas características ya están disponibles:

* El SDK de Microsoft Advertising ahora te permite mostrar [anuncios nativos](../monetize/native-ads.md) en tus aplicaciones. Un anuncio nativo es un formato de anuncio basado en componentes, donde cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual. Actualmente, los anuncios nativos están disponibles únicamente para los desarrolladores que se unan a un programa piloto, pero queremos que esta característica esté disponible para todos los desarrolladores pronto.

* La [API de análisis de Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md) ahora proporciona un método que puedes usar para [descargar el archivo CAB de un error de la aplicación](../monetize/download-the-cab-file-for-an-error-in-your-app.md).

* Las [ofertas dirigidas](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) te permiten centrarte en segmentos específicos de los clientes con contenido personalizado y atractivo para aumentar la interacción, retención y monetización. 

* La descripción de la aplicación en la Store ahora incluye [tráileres de vídeo](../publish/app-screenshots-and-images.md#trailers).

* Las nuevas opciones de precios y disponibilidad te permiten [programar cambios en los precios](../publish/set-and-schedule-app-pricing.md) y [establecer las fechas de lanzamiento exactas](..//publish/configure-precise-release-scheduling.md).

* Puedes [importar y exportar las descripciones de la Store](../publish/import-and-export-store-listings.md) para realizar las actualizaciones más rápido, especialmente si tienes anuncios en varios idiomas.

### <a name="my-people"></a>Mis allegados

Disponible para [usuarios de Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, la próxima característica Mis allegados permite a los usuarios anclar contactos desde una aplicación directamente a la barra de tareas. [Aprende cómo agregar compatibilidad con Mis allegados a tu aplicación.](../contacts-and-calendar/my-people-support.md)

![Panel de contactos Mis allegados](images/my-people.png)

[Uso compartido de Mis allegados](../contacts-and-calendar/my-people-sharing.md) permite a los usuarios compartir archivos a través de la aplicación, directamente desde la barra de tareas.

![Uso compartido de Mis allegados](images/my-people-sharing.png)

[Las notificaciones de Mis allegados](../contacts-and-calendar/my-people-support.md) son un nuevo tipo de notificación del sistema que los usuarios pueden enviar a sus contactos anclados.

![Notificación de Mis allegados](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>Anclar a la barra de tareas

Disponible para [usuarios de Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, la nueva clase TaskbarManager te permite pedir al usuario que [ancle tu aplicación a la barra de tareas](../design/shell/pin-to-taskbar.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="media-playback"></a>Reproducción de contenido multimedia

Se han agregado nuevas secciones al artículo de reproducción básica de elementos multimedia, [Reproducir audio y vídeo con MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md). La sección [Reproducir vídeo esférico con MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) muestra cómo reproducir un vídeo codificado de forma esférica, incluyendo el ajuste del campo de visión y la orientación de la vista en formatos compatibles. La sección [Usar MediaPlayer en modo de servidor de fotogramas](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) muestra cómo copiar fotogramas desde contenido multimedia reproducido con [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) a una superficie de Direct3D. Esto permite escenarios como la aplicación de efectos en tiempo real con sombreadores de píxeles. El código de ejemplo muestra una implementación rápida de un efecto de desenfoque en una reproducción de vídeo con Win2D.

### <a name="media-capture"></a>Captura multimedia

El artículo [Procesar fotogramas multimedia con MediaFrameReader](../audio-video-camera/process-media-frames-with-mediaframereader.md) se actualizó para mostrar el uso de la nueva clase [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader), que te permite obtener fotogramas correlacionados en tiempo desde varios orígenes multimedia. Esto resulta de utilidad si necesitas procesar fotogramas desde orígenes diferentes, como una cámara de profundidad y una cámara de color, y necesitas asegurarte de que los fotogramas de cada origen se han capturado cercanos entre sí en el tiempo. Para más información, consulta [Usar MultiSourceMediaFrameReader para obtener fotogramas correlacionados de tiempo de varios orígenes](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources).

### <a name="scoped-search"></a>Ámbito de búsqueda

Se ha agregado un ámbito "UWP" a la documentación de [UWP conceptual](../get-started/universal-application-platform-guide.md) y de la [referencia de API](https://docs.microsoft.com/uwp/api/) en docs.microsoft.com. A menos que este ámbito está desactivado, las búsquedas realizadas dentro de estas áreas devolverán solo documentos de UWP.

![Ámbito de búsqueda](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Probar la aplicación de Windows en Windows 10 S

Prueba la aplicación de Windows para garantizar que funciona correctamente en dispositivos que ejecuten Windows S. Usa [esta guía nueva](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-test-windows-s) para obtener información sobre cómo hacerlo.

## <a name="samples"></a>Ejemplos

### <a name="annotated-audio-app-sample"></a>Ejemplo de la aplicación Annotated Audio

[Un ejemplo de la miniaplicación que muestra audio, entrada de lápiz y escenarios de itinerancia de datos de OneDrive](https://github.com/Microsoft/Windows-appsample-annotated-audio). Este ejemplo graba audio a la vez que permite la captura sincronizada de anotaciones con entrada de lápiz para que puedas recordar más tarde de qué se estaba hablando mientras se tomaban notas.

![Captura de pantalla de la muestra de Annotated Audio](images/Playback.png)  

### <a name="shopping-app-sample"></a>Ejemplo de la aplicación de compras

[Una miniaplicación que presenta una experiencia de compra básica donde los usuarios pueden comprar emoji](https://github.com/Microsoft/Windows-appsample-shopping). Esta aplicación muestra cómo usar la [API de solicitud de pago](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) para implementar la experiencia del proceso de confirmación de compra.

![Captura de pantalla de la muestra de una aplicación de compras](images/shoppingcart.png)  

## <a name="videos"></a>Vídeos

### <a name="accessibility"></a>Accesibilidad

Crear accesibilidad en tus aplicaciones hace que estas lleguen a un público más amplio. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility) y obtén más información sobre el [desarrollo de aplicaciones accesibles](https://developer.microsoft.com/windows/accessible-apps).

### <a name="payments-request-api"></a>API de solicitud de pago

La API de solicitud de pago ayuda a los clientes y vendedores a completar el proceso de finalización de la compra en línea de una forma muy sencilla. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API) y, a continuación, consulta la [documentación de solicitud de pago](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API).

### <a name="windows-10-iot-core"></a>Windows 10 IoT Core

Con Windows 10 IoT Core y la Plataforma universal de Windows, puedes realizar rápidamente prototipos y crear proyectos con conexiones visuales y de componentes, como la puerta de reconocimiento de mascotas. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core) y, a continuación, obtén más información sobre la [introducción a Windows 10 IoT Core](https://developer.microsoft.com/windows/iot).
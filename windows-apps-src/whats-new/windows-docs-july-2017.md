---
author: QuinnRadich
title: 'Novedades en los documentos de Windows de julio de 2017: desarrollar aplicaciones para UWP'
description: "En la documentación de desarrollador de Windows 10 de julio de 2017, se han agregado nuevas características, muestras y directrices para los desarrolladores."
keywords: "novedades, actualización, características, directrices para los desarrolladores, Windows 10"
ms.author: quradic
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: ffe88f522e261b37652a4ed339ef3c379e045113
ms.sourcegitcommit: f93e887fbab6c1f824a8f762ba848f64c7f77c49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2017
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>Novedades en los documentos de los desarrolladores de Windows de julio de 2017

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general sobre las características, directrices para los desarrolladores y muestras de códigos están a tu disposición desde hace poco y contienen información nueva y actualizada para los desarrolladores de Windows.

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/your-first-app.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="fluent-design"></a>Fluent Design

Disponible para [Windows Insiders](https://insider.windows.com/) en las versiones preliminares del SDK, estos nuevos efectos usan la profundidad, la perspectiva y el movimiento para que los usuarios puedan centrarse en los elementos de interfaz de usuario más importantes.

[Material acrílico](../style/acrylic.md) es un tipo de pincel que crea texturas transparentes. 

![Tema acrílico en claro](../style/images/Acrylic_DarkTheme_Base.png)

El [efecto Parallax](../style/parallax.md) agrega profundidad tridimensional y perspectiva a la aplicación.

![Ejemplo de efecto parallax con la lista y una imagen en segundo plano](../style/images/_Parallax_v2.gif)

[Mostrar](../style/reveal.md) resalta los elementos importantes de la aplicación. 

![Elemento visual de Mostrar](../style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>Controles de la interfaz de usuario

Disponible para [Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, estos nuevos controles hacen más fácil poder crear rápidamente una interfaz de usuario inigualable.

El [control del selector de colores](../controls-and-patterns/color-picker.md) permite a los usuarios examinarlo y seleccionar colores.  

![Selector de colores predeterminado](../controls-and-patterns/images/color-picker-default.png)

El [control de la vista de navegación](../controls-and-patterns/navigationview.md) facilita el proceso para agregar la navegación de nivel superior a la aplicación.

![Secciones de NavigationView](../controls-and-patterns/images/navview_sections.png)

El [control de la imagen del usuario](../controls-and-patterns/person-picture.md) muestra la imagen de avatar de una persona.

![Control de imagen del usuario](../controls-and-patterns/images/person-picture/person-picture_hero.png)

El [control de clasificación](../controls-and-patterns/rating.md) permite a los usuarios ver y establecer clasificaciones que reflejan el grado de satisfacción con el contenido y los servicios.

![Ejemplo de control de clasificaciones](../controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>Kits de herramientas de diseño

Las [herramientas de diseño y los recursos para aplicaciones para UWP](../design-downloads/index.md) se han expandido con la incorporación de los kits de herramientas de bocetos y de Adobe XD. Los kits de herramientas ya existentes también se han actualizado y renovado, proporcionando controles más sólidos y plantillas de diseño para las aplicaciones para UWP.

### <a name="dashboard-monetization-and-store-services"></a>Panel de información, monetización y servicios de la Tienda

Las siguientes nuevas características están disponibles:

* El SDK de Microsoft Advertising ahora te permite mostrar [anuncios nativos](../monetize/native-ads.md) en tus aplicaciones. Un anuncio nativo es un formato de anuncio basados en componentes, donde cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual. Los anuncios nativos están únicamente disponibles actualmente a los desarrolladores que se unan a un programa piloto, pero queremos que esta característica esté disponible para todos los desarrolladores pronto.

* La [API de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md) ahora proporciona un método que puedes usar para [descargar el archivo CAB para un error de la aplicación](../monetize/download-the-cab-file-for-an-error-in-your-app.md).

* [Ofertas dirigidas](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) te permiten centrarte en segmentos específicos de los clientes con contenido personalizado y atractivo para aumentar la interacción, retención y monetización. 

* La descripción de la aplicación en la Tienda ahora incluye [tráileres de vídeo](../publish/app-screenshots-and-images.md#trailers).

* Las nuevas opciones de precios y disponibilidad te permiten [programar cambios en los precios](../publish/set-and-schedule-app-pricing.md) y [establecer las fechas de lanzamiento precisas](..//publish/configure-precise-release-scheduling.md).

* Puedes [importar y exportar las descripciones de la Tienda](../publish/import-and-export-store-listings.md) para realizar las actualizaciones más rápido, especialmente si tienes anuncios en varios idiomas.

### <a name="my-people"></a>Mis allegados

Disponible para [Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, la próxima característica Mis allegados permite a los usuarios anclar contactos desde una aplicación directamente a la barra de tareas. [Aprende cómo agregar compatibilidad de Mis allegados a tu aplicación.](../contacts-and-calendar/my-people-support.md)

![Panel de contactos Mis allegados](images/my-people.png)

[Uso compartido de Mis allegados](../contacts-and-calendar/my-people-sharing.md) permite a los usuarios compartir archivos a través de la aplicación, directamente desde la barra de tareas.

![Uso compartido de Mis allegados](images/my-people-sharing.png)

[Las notificaciones de Mis allegados](../contacts-and-calendar/my-people-support.md) son un nuevo tipo de notificación del sistema que los usuarios pueden enviar a sus contactos anclados.

![Notificación de Mis allegados](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>Anclar a la barra de tareas

Disponible para [Windows Insider](https://insider.windows.com/) en las versiones preliminares del SDK, con la nueva clase TaskbarManager, puedes pedir al usuario que [ancle tu aplicación a la barra de tareas](../controls-and-patterns/pin-to-taskbar.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="media-playback"></a>Reproducción de contenido multimedia

Se han agregado nuevas secciones al artículo de reproducción básica de elementos multimedia, [Reproducir audio y vídeo con MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md). La sección [Reproducir vídeo esférico con MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) muestra cómo reproducir un vídeo codificado de forma esférica, incluyendo el ajusto del campo de visión y la orientación de la vista en formatos compatibles. La sección [Usar MediaPlayer en modo de servidor de fotogramas](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) muestra cómo copiar fotogramas desde contenido multimedia reproducidos con [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) a una superficie de Direct3D. Esto permite escenarios como la aplicación de efectos en tiempo real con sombreadores de píxeles. El código de ejemplo muestra una implementación rápida de un efecto de desenfoque en una reproducción de vídeo con Win2D.

### <a name="media-capture"></a>Media Capture

El artículo [Procesar fotogramas multimedia con MediaFrameReader](../audio-video-camera/process-media-frames-with-mediaframereader.md) se ha actualizado para mostrar el uso de la nueva clase [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader), que te permite obtener fotogramas correlacionados en tiempo de varios orígenes multimedia. Esto resulta de utilidad si necesitas procesar fotogramas desde orígenes diferentes, como una cámara de profundidad y una cámara de color, y necesitas asegurarte de que los fotogramas de cada origen se han capturado próximos entre sí en el tiempo. Para más información, consulta [Usa MultiSourceMediaFrameReader para obtener fotogramas correlacionados de tiempo de varios orígenes](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources).

### <a name="scoped-search"></a>Ámbito de búsqueda

Se ha agregado un ámbito "UWP" a la documentación de [UWP conceptual](../get-started/whats-a-uwp.md) y de la [referencia de API](https://docs.microsoft.com/en-us/uwp/api/) sobre docs.microsoft.com. A menos que este ámbito está desactivado, las búsquedas realizadas dentro de estas áreas devolverán solo documentos de UWP.

![Ámbito de búsqueda](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Probar la aplicación de Windows en Windows 10 S

Prueba la aplicación de Windows para garantizar que funciona correctamente en dispositivos que ejecuten Windows S. Usa [esta guía nueva](../porting/desktop-to-uwp-test-windows-s.md) para obtener información sobre cómo hacerlo. 

## <a name="samples"></a>Muestras

### <a name="annotated-audio-app-sample"></a>Muestra de la aplicación Annotated Audio

[Un ejemplo de la miniaplicación que muestra audio, entrada de lápiz y escenarios de itinerancia de datos de OneDrive.](https://github.com/Microsoft/Windows-appsample-annotated-audio). Esta muestra graba audio a la vez que permite la captura sincronizada de anotaciones manuscritas para que puedas recordar más tarde de qué se estaba hablando mientras se tomaban notas.

![Captura de pantalla de la muestra de Annotated Audio](images/Playback.png)  

### <a name="shopping-app-sample"></a>Muestra de la aplicación de compras

[Una miniaplicación que presenta una experiencia de compra básica donde los usuarios pueden comprar emoji](https://github.com/Microsoft/Windows-appsample-shopping). Esta aplicación muestra cómo usar la [API de solicitud de pago](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) para implementar la experiencia del proceso de confirmación de compra.

![Captura de pantalla de la muestra de una aplicación de compras](images/shoppingcart.png)  

## <a name="videos"></a>Vídeos

### <a name="accessibility"></a>Accesibilidad

Crear accesibilidad en tus aplicaciones hace que estas lleguen hasta un público más amplio. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility) y obtén más información sobre el [desarrollo de aplicaciones accesibles](https://developer.microsoft.com/en-us/windows/accessible-apps).

### <a name="payments-request-api"></a>API de solicitud de pago

La API de solicitud de pago ayuda a los clientes y vendedores a completar el proceso de finalización de la compra en línea de una forma muy sencilla. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API), a continuación, consulta la [documentación de la solicitud de pago](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API).

### <a name="windows-10-iot-core"></a>Windows10 IoT Core

Con Windows 10 IoT Core y la Plataforma universal de Windows, puedes realizar rápidamente prototipos y crear proyectos con conexiones visuales y de componentes, como la puerta de reconocimiento de mascotas. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core) y, a continuación, consigue más información sobre la [introducción a Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot).
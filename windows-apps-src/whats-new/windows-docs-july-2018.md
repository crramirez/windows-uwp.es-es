---
author: QuinnRadich
title: 'Novedades en los documentos de Windows de julio de 2018: desarrollar aplicaciones para UWP'
description: Se agregaron nuevas características, vídeos, muestras y directrices para los desarrolladores a la documentación de desarrolladores de Windows 10 de julio de 2018.
keywords: Novedades, actualización, características, directrices para los desarrolladores, Windows 10, julio
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f41d25fd6757e5d3f80d00de341168de4f34e946
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2913607"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>Novedades de la documentación de desarrolladores de Windows de julio de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información, vídeos, directrices para los desarrolladores y muestras que se han puesto a disposición en el mes de julio.

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="progressive-web-apps-on-windows"></a>Aplicaciones Web progresivas en Windows

[Aplicaciones Web progresivas (PWA)](https://developer.microsoft.com/windows/pwa) son simplemente aplicaciones web que están [progresivamente mejorada](https://wikipedia.org/wiki/Progressive_enhancement) con las características nativas de similar a la aplicación sobre la compatibilidad de plataformas y motores de navegador, como la instalación de inicio de homescreen, compatibilidad sin conexión e inserción notificaciones. En Windows 10 con el motor de Microsoft Edge (EdgeHTML), PWA disfruta de la ventaja adicional de ejecutar [independientemente de la ventana del explorador que las aplicaciones para UWP.](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Una imagen de PWA en acción](images/progressive-web-apps.jpg)

Echa un vistazo a nuestras guías de PWA para:

* [Crear una aplicación web sencillo como una PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [Mejorar tu PWA con el tiempo de ejecución de Windows](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [Publicar tu PWA en Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>Bloc de notas

Está disponible en Windows 10 Insider Preview Build 17713, [se ha actualizado el Bloc de notas con muchas características nuevas](http://aka.ms/ant-man). Zoom, ajuste automático buscar y reemplazar y la compatibilidad con finales de línea de Unix/Linux (LF) y Mac (CR) ahora están disponibles para [Los usuarios de Insider de Windows](https://insider.windows.com/). 

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="design-landing-page"></a>Página de aterrizaje de diseño

Echa un vistazo a la [actualiza el diseño de página de inicio](https://developer.microsoft.com/windows/apps/design) para obtener información de un vistazo general de las áreas de diseño UWP y obtener información acerca de las adiciones más recientes a Fluent Design.

### <a name="design-toolkits"></a>Kits de herramientas de diseño

Se han actualizado los kits de herramientas de Adobe XD y Adobe Illustrator con las nuevas características. Estos kits de herramientas de diseño proporcionan controles y plantillas de diseño para diseñar aplicaciones para UWP. [Echa un vistazo aquí.](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

Hemos agregado varios temas nuevos a la [documentación de WebVR](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [¿Qué es WebVR?](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) Explica WebVR What ' s, ¿por qué se debe usar y cómo empezar a desarrollar para ella.

* [WebVR en aplicaciones Web progresivas](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas): aprende a agregar WebVR a una aplicación de Web progresivas (PWA).

* [WebVR en WebView](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview): Obtén información sobre cómo agregar WebVR a un control de WebView en una aplicación de Windows 10.

* [Demostraciones WebVR](https://docs.microsoft.com/microsoft-edge/webvr/demos): echa un vistazo a algunas demostraciones WebVR con Microsoft Edge y unos cascos envolventes de realidad mixta de Windows.

Además, que hemos realizado algunas actualizaciones a las páginas existentes:

* La tabla de contenido ahora mejor organizada en cuatro depósitos de nivel superior distintos: **conceptos básicos**, **desarrollo**, **recursos**y **demostraciones de versiones**.

* [Guía del desarrollador de WebVR (página de aterrizaje)](https://docs.microsoft.com/microsoft-edge/webvr/): actualiza un aspecto, con iconos y las imágenes más grandes y nueva demostración.

* [Usar WebVR con Microsoft Edge](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge): actualizado para incluir información acerca de las ventanas de actualización de 10 de abril de 2018.

## <a name="videos"></a>Vídeos

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>Introducción para desarrolladores: crear y personalizar un formulario de Windows 10

Nuestros [documentos de introducción](../get-started/index.md) para desarrolladores de Windows ahora proporcionar experiencia práctica con las tareas de desarrollo de aplicación básica. Este vídeo te guiará a través de uno de los temas y describe los conceptos básicos de creación de un formulario de la interfaz de usuario en la aplicación. [Ve el vídeo](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be) para ver el código en acción, a continuación, [consultar el tema tú mismo.](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>Mejorar tu Bot con chat de personalidad de proyecto

Chat de proyecto personalidad te permite agregar un rol personalizable a sus robots de chat. Mediante la integración con el SDK de Bot de Microsoft, puedes agregar capacidades de pequeño para hablar de una manera más informal interactuar con los clientes. [Ve el vídeo](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be) para obtener información sobre cómo implementar, a continuación, [probar la demostración interactiva](http://aka.ms/PersonalityChat) para obtener experiencia práctica.

### <a name="one-dev-question"></a>Una pregunta de desarrollo

En la serie de vídeos de una pregunta de desarrollo, los desarrolladores de Microsoft siempre cubren una serie de preguntas frecuentes sobre el desarrollo de Windows, referencia cultural de equipo e historial. Aquí es que hemos respondido a las preguntas más recientes.

Raymond Chen:

* [¿Por qué aplican a Microsoft?](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [¿Por qué no se permiten a los desarrolladores cambiar el dispositivo de audio predeterminado?](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [¿Por qué tantas asincrónica de las funciones UWP?](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>Muestras

### <a name="photo-editor-cwinrt"></a>Photo Editor C++ / WinRT

La aplicación de muestra de Photo Editor muestra el desarrollo con la [C++ / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) proyección de lenguaje. La aplicación te permite recuperar fotos desde la biblioteca de **imágenes** y, a continuación, modificar una imagen seleccionada con distintos efectos de fotos asociado. [Clonar o descargar la muestra aquí.](https://github.com/Microsoft/Windows-appsample-photo-editor)

![Un ejemplo de la muestra en acción](images/photo-editor-banner.png)
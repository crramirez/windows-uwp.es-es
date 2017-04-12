---
author: seksenov
title: "Aplicaciones web hospedadas: Convertir tu aplicación web en una aplicación de Windows con Visual Studio"
description: "Usa Visual Studio para convertir tu sitio web en una aplicación para la Plataforma universal de Windows (UWP) para Windows10."
kw: Hosted Web Apps tutorial, Porting to Windows 10 with Visual Studio, How to convert website to Windows, How to add website to Windows Store, Packaging web application for Microsoft Store, Test Windows 10 native features and runtime APIs with CodePen, How to use Windows Cortana Live Tiles Built-in Camera on my Website with remote JavaScript
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Aplicaciones web hospedadas, Hosted Web Apps, portar aplicaciones web a Windows 10, port web apps to Windows 10, convertir el sitio web en Windows, convert website to Windows, empaquetado de aplicaciones web para la Tienda Windows, packaging web apps for Windows Store
ms.assetid: a58d2c67-77f8-4d01-bea3-a6ebce2d73b9
ms.openlocfilehash: e135b63e2ba5f455dbbf6524e5519a33acd822b0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-your-web-application-to-a-universal-windows-platform-uwp-app"></a>Convertir tu aplicación web en una aplicación para la Plataforma universal de Windows (UWP)

Aprende cómo crear rápidamente una aplicación para la Plataforma universal de Windows (UWP) para Windows10 empezando con solo la dirección URL de un sitio web. 

> [!NOTE]
> Las siguientes instrucciones son para su uso con una plataforma de desarrollo Windows. Si eres usuario de Mac, visita [instrucciones sobre el uso de una plataforma de desarrollo de Mac](./hwa-create-mac.md).

## <a name="what-you-need-to-develop-on-windows"></a>Qué necesitas para desarrollar en Windows

- [Visual Studio2015.](https://www.visualstudio.com/) Visual Studio Community2015 incluye las herramientas para desarrolladores de Windows10, plantillas de aplicaciones universales, un editor de código, un depurador de enorme eficacia, emuladores de Windows Moble, un amplio soporte de lenguaje y mucho más; todo ello es gratuito y está dotado de todas las características necesarias para su uso directo en la producción.
- (Opcional) [SDK independiente de Windows para Windows10.](https://dev.windows.com/downloads/windows-10-sdk) Si estás usando un entorno de desarrollo distinto a Visual Studio2015, puedes descargar un instalador de Windows SDK para Windows10 independiente. Ten en cuenta que no es necesario instalar este SDK si usas Visual Studio2015, porque ya está incluido.

## <a name="step-1-pick-a-website-url"></a>Paso 1: Seleccionar la dirección URL de un sitio web
Elige un sitio web existente que funcione perfectamente como una aplicación de una sola página. Es muy recomendable que seas el propietario o el desarrollador del sitio, de este modo, podrás realizar todos los cambios necesarios. Si no se te ocurre ninguna dirección URL, pruebe a usar este [ejemplo de Codepen](http://codepen.io/seksenov/pen/wBbVyb/?editors=101) como sitio web. Copia tu dirección URL o la dirección URL de Codepen para usarla en el tutorial. 

![Paso 1: Seleccionar la dirección URL de un sitio web](images/hwa-to-uwp/windows_step1.png)

## <a name="step-2-create-a-blank-javascript-app"></a>Paso 2: Crea una aplicación de JavaScript en blanco

Inicia VisualStudio.
1. Haz clic en **Archivo**.
2. Haz clic en **Nuevo proyecto**.
3. En **JavaScript**, selecciona **Windows Universal** y luego haz clic en **Aplicación vacía (Windows universal)**.

![Paso 2: Crea una aplicación de JavaScript en blanco](images/hwa-to-uwp/windows_step2.png)

## <a name="step-3-delete-any-packaged-code"></a>Paso 3: Elimina cualquier código empaquetado

Dado que se trata de una aplicación web hospedada en la que el contenido se proporciona desde un servidor remoto, no necesitarás la mayoría de los archivos de aplicación local que se incluyen de forma predeterminada con la plantilla de JavaScript. Elimina los recursos locales de HTML, JavaScript o CSS. Lo único que debe quedar es el archivo `package.appxmanifest` que permite configurar la aplicación y los recursos de imagen.

![Paso 3: Elimina cualquier código empaquetado](images/hwa-to-uwp/windows_step3.png)

## <a name="step-4-set-the-start-page-url"></a>Paso 4: Establece la dirección URL de la página de inicio

1. Abre el archivo `package.appxmanifest`.
2. En la pestaña **Aplicación**, busca el campo de texto **Página de inicio**.
3. Sustituye `default.html` por la dirección URL de tu sitio web.

![Paso 4: Establece la dirección URL de la página de inicio](images/hwa-to-uwp/windows_step4.png)

## <a name="step-5-define-the-boundaries-of-your-web-app"></a>Paso 5: Define los límites de la aplicación web

Las reglas de URI de contenido de la aplicación (las ACUR) especifican qué direcciones URL remotas tienen permitido el acceso a la aplicación y a las API de Windows universales. Como mínimo, deberás agregar una ACUR para tu página de inicio y para todos los recursos web que use esa página. Para obtener más información sobre las ACUR, [haz clic aquí](./hwa-access-features.md).
1. Abre el archivo `package.appxmanifest`.
2. Haz clic en la pestaña **URI de contenido**.
3. Agrega todas las URI necesarias para la página de inicio.

Por ejemplo:
```
1. http://codepen.io/seksenov/pen/wBbVyb/?editors=101
2. http://*.codepen.io/
```
4. Establece el **Acceso a WinRT** en **Todo** para cada URI que hayas agregado.

![Paso 5: Define los límites de la aplicación web](images/hwa-to-uwp/windows_step5.png)

## <a name="step-6-run-your-app"></a>Paso 6: Ejecuta la aplicación

Llegados a este punto, tienes una aplicación de Windows10 totalmente funcional capaz de obtener acceso a las API de Windows universales.

Si estás trabajando con nuestro ejemplo de Codepen, haz clic en el botón **Notificación del sistema** para llamar a una API de Windows desde script hospedado.

![Paso 6: Ejecuta la aplicación](images/hwa-to-uwp/windows_step6.png)

## <a name="bonus-add-camera-capture"></a>Extra: Agrega la captura de cámara

Copia y pega el siguiente código de JavaScript para habilitar la captura de cámara. Si estás trabajando con tu propio sitio web, crea un botón para invocar el método `cameraCapture()`. Si estás trabajando con nuestro ejemplo de Codepen, ya existe un botón en HTML. Haz clic en el botón y haz una foto.

```JavaScript
function cameraCapture() {
  if(typeof Windows != 'undefined') {
   var captureUI = new Windows.Media.Capture.CameraCaptureUI();
   //Set the format of the picture that's going to be captured (.png, .jpg, ...)
   captureUI.photoSettings.format = Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;
   //Pop up the camera UI to take a picture
   captureUI.captureFileAsync(Windows.Media.Capture.CameraCaptureUIMode.photo).then(function (capturedItem) {
      // Do something with the picture
   });
  }
}
```

## <a name="related-topics"></a>Temas relacionados

- [Mejorar la aplicación web mediante el acceso a características de la Plataforma universal de Windows (UWP)](hwa-access-features.md)
- [Guía de aplicaciones para la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Descarga de activos de diseño para aplicaciones de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)

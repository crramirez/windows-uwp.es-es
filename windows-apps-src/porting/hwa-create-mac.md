---
author: seksenov
title: "Aplicaciones web hospedadas: Convertir tu aplicación web en una aplicación de Windows con un Mac"
description: "Usa un Mac para convertir tu sitio web en una aplicación para la Plataforma universal de Windows (UWP) para Windows 10."
kw: Hosted Web Apps with a Mac, Porting to Windows 10 with a Mac, Convert website to Windows with Mac, Packaging web application with ManfoldJS for Windows Store, Add website to Windows Store with App Studio
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: aplicaciones web hospedadas con un Mac, hosted web apps with a Mac, portar a Windows 10 con un Mac, porting to Windows 10 with a Mac, convertir sitio web a Windows con Mac, convert website to Windows with Mac, sitio web a la Tienda Windows, website to Windows Store, Manifold JS para aplicaciones web, Manifold JS for web apps, App Studio para aplicaciones web, App Studio for web apps
ms.assetid: a4cea1e8-4417-4488-b0e7-45c704dc53e9
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 464097cc3f90c2e84e03e936fb354f4220aff91d
ms.lasthandoff: 02/08/2017

---

# <a name="create-your-hosted-web-app-using-a-mac"></a>Crear la aplicación web hospedada con un Mac

Crea rápidamente una aplicación para la Plataforma universal de Windows (UWP) para Windows 10 empezando con solo la dirección URL de un sitio web. 

> [!NOTE]
> Las siguientes instrucciones son para su uso con una plataforma de desarrollo Mac. Si eres usuario de Windows, visita [instrucciones sobre el uso de una plataforma de desarrollo de Windows](./hwa-create-windows.md).

## <a name="what-you-need-to-develop-on-mac"></a>Qué necesitas para desarrollar en Mac

- Un navegador web.
- Un símbolo del sistema.

## <a name="option-1-manifoldjs"></a>Opción 1: ManifoldJS

[ManifoldJS](http://manifoldjs.com/) es una aplicación de Node.js que se instala fácilmente desde NPM. Esta aplicación toma los metadatos sobre el sitio web y genera las aplicaciones nativas hospedadas en Android, iOS y Windows. Si tu sitio no tiene un [manifiesto de aplicación web](https://www.w3.org/TR/appmanifest/), se generará uno automáticamente.

1. Instala [NodeJS](https://nodejs.org/), que incluye NPM (administrador de paquetes de nodo). <br>

2. Abre un símbolo del sistema y NPM e instala ManifoldJS:
```
npm install -g manifoldjs
```

3. Ejecutar el comando `manifoldjs` en la dirección URL del sitio web:
```
manifoldjs http://codepen.io/seksenov/pen/wBbVyb/?editors=101
```

4. Sigue los pasos que se muestran en el vídeo siguiente para completar el empaquetado y publicar tu aplicación web hospedada en la Tienda Windows.

[![Publicación de una aplicación web para UWP en un equipo Mac mediante ManifoldJS](images/hwa-to-uwp/mac_manifoldjs_video.png)](https://sec.ch9.ms/ch9/0a67/9b06e5c7-d7aa-478d-b30d-f99e145a0a67/ManifoldJS_high.mp4 "Publicación de una aplicación web para UWP en un equipo Mac mediante ManifoldJS")

## <a name="option-2-app-studio"></a>Opción 2: App Studio

[App Studio](http://appstudio.windows.com/) es una herramienta gratuita de creación de aplicaciones en línea que permite crear rápidamente aplicaciones de Windows 10.

1. Abre [App Studio](http://appstudio.windows.com/) en el navegador web.

2. Haz clic en **Iniciar ahora**.

3. En **Plantillas de aplicación web**, haz clic en **Aplicación web hospedada**.

4. Sigue las instrucciones en pantalla para generar un paquete listo para publicarse en la Tienda Windows.

## <a name="related-topics"></a>Temas relacionados

- [Mejorar la aplicación web mediante el acceso a características de la Plataforma universal de Windows (UWP)](./hwa-access-features.md)
- [Guía de aplicaciones para la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Descarga de activos de diseño para aplicaciones de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)


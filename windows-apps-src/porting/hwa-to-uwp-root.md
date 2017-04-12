---
author: seksenov
title: "Aplicaciones web hospedadas: Convertir la aplicación web en una aplicación para la Plataforma universal de Windows (UWP) y acceder a las características nativas de Windows10"
description: "Encuentra recursos para convertir tu aplicación web en una aplicación de la Plataforma universal de Windows (UWP) para la Tienda Windows."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Aplicaciones web hospedadas, convertir un sitio web en una aplicación de Windows, convert a website to Windows app, aplicaciones web en la Tienda de Windows, web apps on Windows Store, aplicaciones de Chrome para Windows, Chrome apps for Windows"
ms.openlocfilehash: c9239f3a3c14bf9da99e60cfef8154eefb4305b4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="hosted-web-apps---access-windows-10-features-from-your-web-app"></a>Aplicaciones web hospedadas: Acceso a características de Windows10 desde tu aplicación web

Tu aplicación web puede tener acceso completo a la Plataforma universal de Windows (UWP), lo que incluye llamar a API de Windows Runtime directamente desde un script hospedado en un servidor, aprovechar la integración de Cortana y usar de un proveedor de autenticación en línea. También se admiten aplicaciones híbridas, ya que puedes incluir código local para que sea llamado desde el script hospedado y administrar la navegación en aplicaciones entre páginas locales y remotas.

## <a name="get-started"></a>Introducción

Tanto si trabajas con PC como con Mac, puedes crear una aplicación web hospedada en cuestión de minutos. La mejor manera de comenzar es mediante [Visual Studio Community2015](https://www.visualstudio.com/vs/community/), un programa gratuito y totalmente funcional, especialmente si trabajas con un dispositivo Windows. Si no tienes acceso a Visual Studio, hay varias opciones entre las que puedes elegir. Si conoces las utilidades de interfaz de línea de comandos (CLI), echa un vistazo a [ManifoldJS](http://manifoldjs.com/). También puedes usar [App Studio](http://appstudio.windows.com/), una herramienta de creación en línea gratuita que no requiere código y que te permite crear rápidamente aplicaciones de Windows10.

- [Instrucciones paso a paso para convertir la aplicación web en una aplicación para la Plataforma universal de Windows (UWP) con Windows](hwa-create-windows.md)

- [Instrucciones paso a paso para convertir la aplicación web en una aplicación para la Plataforma universal de Windows (UWP) con Mac](hwa-create-mac.md)

## <a name="enhance-your-app"></a>Mejorar tu aplicación

- Haz que tu aplicación deslumbre con las [características nativas de Windows Runtime](hwa-access-features.md) a las que puedes llamar directamente desde JavaScript.

- Mantén tu aplicación segura y establece reglas de URI de contenido de aplicación (ACUR) con nuestro modelo de Directiva de seguridad de contenido (CSP).

- Ejecuta la aplicación con la eficacia de la voz mediante la integración de los comandos de Cortana.

- Concede acceso mediante programación a recursos de usuario y funcionalidades de dispositivo mediante la declaración de las funcionalidades de la aplicación.

- Crea un flujo de acceso sencillo para los usuarios verificando su identidad con OpenID y OAuth.

- Deja de tener que decidir entre una aplicación empaquetada y una aplicación web hospedada. Ahora puedes tener un poco de cada una si creas una aplicación híbrida.

## <a name="convert-your-existing-chrome-app"></a>Convertir tu aplicación de Chrome existente

Hemos puesto fácil [convertir tu aplicación hospedada de Chrome existente](hwa-chrome-conversion.md) en una aplicación web hospedada de Windows. [ManifoldJS](http://manifoldjs.com/) ahora acepta manifiestos de Chrome como forma de entrada. También hemos desarrollado una [herramienta de CLI](https://github.com/MicrosoftEdge/hwa-cli) que genera un paquete `.appx` a partir de los archivos `.zip` o `.crx` existentes.

## <a name="demos"></a>Demostraciones

- [Aplicación de viajes de Contoso](http://contosotravel.azurewebsites.net/)


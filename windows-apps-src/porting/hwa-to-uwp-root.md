---
title: "Aplicaciones web hospedadas: Convertir la aplicación web en una aplicación para la plataforma de Windows universales (UWP) y acceder a las características nativas de Windows10"
description: "Crea una aplicación para la Plataforma universal de Windows (UWP) a partir de la dirección URL de tu sitio web. Accede a las características de dispositivo nativas de Windows10 desde el código de la aplicación web. Los Puentes de Microsoft Windows para aplicaciones web hospedadas, anteriormente Project Westminster, te permiten incluir de forma rápida y sencilla tu aplicación web en la Tienda Windows."
author: seksenov
translationtype: Human Translation
ms.sourcegitcommit: 7fe6e240e4ef221b49f9b103cf30192449ce4502
ms.openlocfilehash: 95d50aa37f349f494f260ea3af97211a085623a9

---

# Aplicaciones web hospedadas: Acceso a características de Windows10 desde tu aplicación web

Tu aplicación web puede tener acceso completo a la Plataforma universal de Windows (UWP), lo que incluye llamar a API de Windows Runtime directamente desde un script hospedado en un servidor, aprovechar la integración de Cortana y usar de un proveedor de autenticación en línea. También se admiten aplicaciones híbridas, ya que puedes incluir código local para que sea llamado desde el script hospedado y administrar la navegación en aplicaciones entre páginas locales y remotas.

## Introducción

Tanto si trabajas con PC como si lo haces con Mac, puedes crear tu propia aplicación de web hospedada en cuestión de minutos. La mejor manera de comenzar es mediante [Visual Studio Community2015](https://www.visualstudio.com/), un programa gratuito y de características completas, especialmente si trabajas con un dispositivo Windows. Si no tienes acceso a Visual Studio, hay varias opciones entre las que puedes elegir. Si conoces las utilidades de interfaz de línea de comandos (CLI), echa un vistazo a [ManifoldJS](http://manifoldjs.com/). También puedes usar [App Studio](http://appstudio.windows.com/), una herramienta de creación en línea gratuita y sin necesidad de código que te permite crear rápidamente aplicaciones de Windows10.

- [Instrucciones paso a paso para convertir la aplicación web en una aplicación para la Plataforma universal de Windows (UWP) con Windows](hwa-create-windows.md)

- [Instrucciones paso a paso para convertir la aplicación web en una aplicación para la Plataforma universal de Windows (UWP) con Mac](hwa-create-mac.md)

## Mejorar tu aplicación

- Haz que tu aplicación destaque gracias al [acceso nativo a las características de Windows](hwa-access-features.md) en JavaScript desde Windows Runtime.

- Mantén tu aplicación segura y establece reglas de URI de contenido de aplicación (ACUR) con nuestro modelo de Directiva de seguridad de contenido (CSP).
- Ejecuta la aplicación con la eficacia de la voz mediante la integración con los comandos de voz de Cortana.

- Concede acceso mediante programación a recursos de usuario y funcionalidades de dispositivo mediante la declaración de las funcionalidades de la aplicación.

- Crea un flujo de acceso sencillo para los usuarios verificando su identidad con OpenID y OAuth.

- Deja de tener que decidir entre una aplicación empaquetada y una aplicación web hospedada. Ahora puedes tener un poco de cada una si creas una aplicación híbrida.

## Convertir tu aplicación de Chrome existente

Hemos puesto fácil [convertir tu aplicación hospedada de Chrome existente](hwa-chrome-conversion.md) en una aplicación web hospedada de Windows. [ManifoldJS](http://manifoldjs.com/) ahora acepta manifiestos de Chrome como forma de entrada. También hemos desarrollado una [herramienta de CLI](https://github.com/MicrosoftEdge/hwa-cli) que genera un paquete `.appx` a partir de los archivos `.zip` o `.crx` existentes.

## Demostraciones

- [Aplicación de viajes de Contoso](http://contosotravel.azurewebsites.net/)

- [API de Windows Runtime: Muestras de código JavaScript](http://rjs.azurewebsites.net/)



<!--HONumber=Aug16_HO3-->



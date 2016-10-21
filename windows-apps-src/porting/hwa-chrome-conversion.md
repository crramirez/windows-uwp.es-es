---
author: seksenov
title: "Aplicaciones Web hospedadas: Convierte tu aplicación de Chrome en una aplicación para la Plataforma universal de Windows"
description: "Convierte tu aplicación de Chrome o extensión de Chrome en una aplicación para la Plataforma universal de Windows (UWP) para la Tienda Windows."
kw: Package Chrome Extension for Windows Store tutorial, Port Chrome Extension to Windows 10, How to convert Chrome App to Windows, How to add Chrome Extension to Windows Store, hwa-cli, Hosted Web Apps Command Line Interface CLI Tool, Install Chrome Extension on Windows 10 Device, convert .crx to .AppX
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 7847f69c85708cb42b878253839b06929f837708

---

# Convierte tu aplicación de Chrome existente en una aplicación para la Plataforma universal de Windows

Hemos facilitado la conversión de tu aplicación de Chrome existente hospedada en una aplicación que se ejecute en la Plataforma universal de Windows (UWP). Existen dos formas de convertir tu aplicación de Chrome:

- Opción 1: [ManifoldJS](http://manifoldjs.com/) ahora acepta manifiestos de Chrome como una forma de entrada. 

- Opción 2: Hemos desarrollado una [herramienta de CLI](https://github.com/MicrosoftEdge/hwa-cli) que genera un paquete `.appx` desde los archivos `.zip` o `.crx` existentes.

## Convierte tu aplicación de Chrome existente mediante la interfaz de línea de comandos

1. Instala [NodeJS](https://nodejs.org/en/) y su administrador de paquetes, [npm](https://www.npmjs.com/). 


2. Abre una ventana del símbolo del sistema en el directorio de tu elección


3. Instala la interfaz de línea de comandos (CLI) de aplicaciones web hospedadas (HWA) escribiendo lo siguiente en la línea de comandos: `npm i -g hwa-cli`

4. Convierte tu paquete de Chrome (`.crx` y `.zip` son los formatos de paquete admitidos) escribiendo lo siguiente en la línea de comandos: `hwa convert path/to/chrome/app.crx` o `hwa convert path/to/chrome/app.zip`

    **Sustituye `path/to/chrome/app` por la información de ruta de acceso que lleve a tu aplicación de Chrome.*
    
5. El archivo `.appx` generado aparecerá en la misma carpeta que el paquete de Chrome. Ahora ya puedes para cargar la aplicación en la Tienda Windows. 

## Cargar la aplicación en la TiendaWindows

Para subir la aplicación, visita el panel del [Centro de desarrollo de Windows](https://developer.microsoft.com/windows). Haz clic en "[Crear una nueva aplicación](https://developer.microsoft.com/dashboard/Application/New)" y reserva el nombre de tu aplicación.
![Panel Reservar un nombre del Centro de desarrollo de Windows](images/hwa-to-uwp/reserve_a_name.png)


Carga tu paquete `AppX` navegando a la página de "Paquetes" de la sección Envíos.

Completa la información que te solicite la Tienda Windows.

    During the conversion process, you will be prompted for an Identity Name, Publisher Identity, and Publisher Display Name. To retrieve these values, visit the Dashboard in the [Windows Dev Center](https://developer.microsoft.com/windows).
    - Click on "[Create a new app](https://developer.microsoft.com/dashboard/Application/New)" and reserve your app name.
![Panel Reservar un nombre del Centro de desarrollo de Windows](images/hwa-to-uwp/reserve_a_name.png)
    - A continuación, haz clic en "Identidad de aplicación" en el menú de la izquierda, en la sección "Administración de aplicaciones".
    ![Panel identidad de la aplicación del Centro de desarrollo de Windows](images/hwa-to-uwp/app_identity.png)
    - Deberías ver los 3 valores para el que se te piden en la página: 1. Nombre de identidad: `Package/Identity/Name`
        2. Identidad del editor: `Package/Identity/Publisher`
        3. Nombre para mostrar del editor: `Package/Properties/PublisherDisplayName`


## Guía para la migración de la aplicación web hospedada

Después de empaquetar la aplicación web para la Tienda Windows, personalízala para que funcione perfectamente en todos los dispositivos de Windows, como PC, tabletas, teléfonos, HoloLens, Surface Hub, Xbox y Raspberry Pi.

### Reglas de URI de contenido de la aplicación

Las [reglas de URI de contenido de la aplicación (ACUR)](/hwa-access-features.md#keep-your-app-secure-setting-application-content-uri-rules-acurs) o URI de contenido definen el ámbito de la aplicación web hospedada mediante una lista de direcciones URL permitidas en el manifiesto del paquete de la aplicación. Para controlar la comunicación al contenido remoto y desde este, debes definir qué direcciones URL se incluyen o se excluyen de esta lista. Si un usuario hace clic en una dirección URL que esté incluida de forma explícita, Windows abrirá la ruta de destino en el navegador predeterminado. Con las ACUR, también son puedes de conceder acceso a una dirección URL a las [API universales de Windows](https://msdn.microsoft.com/library/windows/apps/br211377.aspx).

Como mínimo, las reglas deberían incluir la página de inicio de la aplicación. La herramienta de conversión creará automáticamente un conjunto de ACUR por ti, en función de la página de inicio y su dominio. Sin embargo, si hay cualquier redirección mediante programación, ya sea en el servidor o en el cliente, será necesario agregar estos los destinos a la lista de direcciones URL permitidas.

*Nota: Las ACUR solamente se aplican a la navegación de página. Las imágenes, las bibliotecas de JavaScript y otros activos similares no se ven afectados por estas restricciones.*

Muchas aplicaciones usan sitios de terceros para sus flujos de inicio de sesión; por ejemplo, Facebook y Google. La herramienta de conversión creará automáticamente un conjunto de ACUR por ti, en función de los sitios más populares. Si tu método de autenticación no se incluye en esa lista y se trata de un flujo de redirección, tendrás que agregar sus rutas como una ACUR. También puedes pensar en usar un [agente de autenticación web](/hwa-access-features.md#web-authentication-broker).

### Flash

Flash no está permitido en las aplicaciones de Windows10. Tendrás que asegurarte de que la experiencia de la aplicación no se vea afectado por su ausencia.

En el caso de los anuncios, deberás asegurarte de que tu proveedor de anuncios tenga una opción de HTML5. Puedes consultar [Bing Ads](https://bingads.microsoft.com/) y [Anuncios en aplicaciones](http://adsinapps.microsoft.com/).

Los vídeos de YouTube deberían seguir funcionando, ya que ahora [son `<video>`HTML5 de forma predeterminada,](http://youtube-eng.blogspot.com/2015/01/youtube-now-defaults-to-html5_27.html) siempre y cuando utilices el [método de incrustación `<iframe>`](https://developers.google.com/youtube/iframe_api_reference). Si la aplicación todavía usa la API de Flash, tendrás que cambiar el estilo de incrustación mencionado anteriormente.

### Activos de imagen

El almacén web de Chrome ya [requiere una imagen para el icono de la aplicación de 128 x 128](https://developer.chrome.com/webstore/images) en el paquete de la aplicación. En el caso de las aplicaciones de Windows10, debes proporcionar imágenes de icono de la aplicación de 44 x 44, 50 x 50, 150 x 150 y 600 x 350, como mínimo. La herramienta de conversión creará automáticamente estas imágenes, basadas en la imagen de 128 x 128. Para que las experiencia de la aplicación sea más rica y más elegante, te recomendamos crear tus propios archivos de imagen. Estos son algunas [directrices para los activos de los iconos y las ventanas](https://msdn.microsoft.com/library/windows/apps/mt412102.aspx).

### Funcionalidades

Las funcionalidades de la aplicación deben [declararse](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations) en el manifiesto del paquete para tener acceso a ciertos recursos y API. La herramienta de conversión habilitará automáticamente 3 funcionalidades de dispositivo populares: la ubicación, el micrófono y la cámara web. Para el primero, el sistema aún solicitará permiso al usuario antes de conceder acceso.

*Nota: A los usuarios se les notifican todas las funcionalidades que una aplicación declara. Recomendamos quitar todas las funcionalidades que la aplicación no necesite.*

### Descargas de archivos

Las descargas de archivos tradicionales, como se puede ver en el navegador, no se admiten actualmente.

### API para la plataforma Chrome

Chrome proporciona aplicaciones con [API de propósito especial](https://developer.chrome.com/apps/api_index) que se pueden ejecutar como script en segundo plano. Estas no se admiten. Puedes encontrar una funcionalidad equivalente, y mucha más, con las [API de Windows Runtime](https://msdn.microsoft.com/library/windows/apps/br211377.aspx).

## Temas relacionados

- [Mejorar la aplicación web mediante el acceso a características de la Plataforma universal de Windows (UWP)](/hwa-access-features.md)
- [Guía de aplicaciones para la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Descarga de activos de diseño para aplicaciones de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->



---
title: Enfoque de PWA para el desarrollo de Android en Windows
description: Introducción al desarrollo de aplicaciones Android con el enfoque de PWA en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android en Windows, PWA, Android, Cordova, iónico, PhoneGap, aplicación web híbrida
ms.date: 04/28/2020
ms.openlocfilehash: 482fd02ed7b5d978d81ec52309006034f70b7e47
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163989"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>Introducción al desarrollo de una aplicación Web de PWA o híbrida para Android

Esta guía le ayudará a empezar a crear una aplicación web híbrida o una aplicación web progresiva (PWA) en Windows con una única base de código HTML/CSS/JavaScript que se puede usar en la web y en plataformas de dispositivos (Android, iOS y Windows).

Mediante el uso de los marcos y componentes correctos, las aplicaciones basadas en Web pueden funcionar en un dispositivo Android de forma que los usuarios tengan una apariencia muy similar a la de una aplicación nativa.

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>Características de una aplicación Web de PWA o híbrida

Hay dos tipos principales de aplicaciones web que se pueden instalar en dispositivos Android. La diferencia principal es si el código de aplicación se inserta en un paquete de aplicación (híbrido) o se hospeda en un servidor Web (PWA).

- **Aplicaciones web híbridas**: el código (HTML, JS, CSS) se empaqueta en un APK y se puede distribuir a través de la Google Play Store. El motor de visualización está aislado del explorador de Internet de los usuarios, sin uso compartido de la sesión o la memoria caché.

- **Web Apps progresivas (PWA)**: el código (HTML, JS, CSS) vive en la web y no es necesario empaquetarlo como apk. Los recursos se descargan y actualizan según sea necesario mediante un trabajo de servicio. El explorador Chrome se representará y mostrará la aplicación, pero tendrá un aspecto nativo y no incluirá la barra de direcciones del explorador normal, etc. Puede compartir el almacenamiento, la memoria caché y las sesiones con el explorador. Básicamente es como instalar un acceso directo al explorador Chrome en un modo especial. PWA también se puede enumerar en el Google Play Store mediante la actividad Web de confianza.

PWA y las aplicaciones web híbridas son muy similares a las aplicaciones de Android nativas en que:

- Se puede instalar a través del App Store (Google Play Store y/o Microsoft Store)
- Tener acceso a características de dispositivos nativos como cámara, GPS, Bluetooth, notificaciones y lista de contactos
- Trabajar sin conexión (sin conexión a Internet)

PWA también tiene algunas características únicas:

- Se puede instalar en la pantalla principal de Android directamente desde la web (sin una tienda de aplicaciones).
- También se puede instalar mediante el Google Play Store [mediante una actividad Web de confianza](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/) .
- Se puede detectar a través de la búsqueda web o compartido a través de un vínculo de dirección URL
- Confíe en un [trabajador de servicio](https://developers.google.com/web/fundamentals/primers/service-workers) para evitar la necesidad de empaquetar código nativo

No necesita un marco de trabajo para crear una aplicación híbrida o PWA, pero hay algunos marcos populares que se tratarán en esta guía, entre los que se incluyen PhoneGap (con Cordova) y iónico (con Cordova o condensador mediante angular o reAct).

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/) es un marco de código abierto que puede simplificar la comunicación entre el código de JavaScript que vive en una [vista previa](https://developer.android.com/reference/android/webkit/WebView) nativa y la plataforma Android nativa mediante [Complementos](https://cordova.apache.org/plugins/?platforms=cordova-android). Estos complementos exponen los puntos de conexión de JavaScript a los que se puede llamar desde el código y que se usan para llamar a las API de dispositivos Android nativos. Algunos ejemplos de complementos de Cordova incluyen el acceso a servicios de dispositivos como el estado de la batería, el acceso a archivos, los tonos de vibración y timbre, etc. Normalmente, estas características no están disponibles para las aplicaciones web o los exploradores.

Hay dos distribuciones populares de Cordova:

- [PhoneGap](https://phonegap.com/)

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

[PhoneGap](https://phonegap.com/): un marco compatible con Adobe que admite Cordova con herramientas adicionales, como una [línea de comandos](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/), una [aplicación de escritorio](https://phonegap.com/products#desktop-app-section)y [PhoneGap Build](https://build.phonegap.com/), un servicio que le permite cargar el código en un servidor de Adobe que creará aplicaciones nativas sin necesidad de instalar SDK nativos en el equipo local. Esto le permite hacer cosas como compilar una aplicación iOS con la máquina Windows.

### <a name="install-phonegap"></a>Instalación de PhoneGap

Para empezar a crear una aplicación Web de PWA o híbrida con PhoneGap, primero debe instalar las siguientes herramientas:

- Node.js para interactuar con el ecosistema iónico. [Descargue NodeJS para Windows](https://nodejs.org/en/) o siga la [Guía de instalación de NodeJS](../nodejs/setup-on-wsl2.md) con el subsistema de Windows para Linux (WSL). Puede que desee considerar la posibilidad de usar [node version Manager (NVM)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) si va a trabajar con varios proyectos y versión de NodeJS.

Instale PhoneGap escribiendo lo siguiente en la línea de comandos:

```bash
npm install -g phonegap
```

Para crear un nuevo proyecto de PhoneGap, siga los pasos para [empezar.](https://phonegap.com/getstarted/) Visite la sección de [características de PWA](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/) de los documentos de PhoneGap para obtener información sobre cómo migrar la aplicación de un entorno híbrido a un PWA.  

## <a name="ionic"></a>Ionic

[Iónico](https://ionicframework.com/) es un marco que ajusta la interfaz de usuario (UI) de la aplicación para que coincida con el lenguaje de diseño de cada plataforma (Android, iOS y Windows). Iónico permite usar [angular](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro) o [reAct](https://ionicframework.com/react).

> [!NOTE]
> Existe una nueva versión de iónico que usa una alternativa a Cordova, denominada [condensador](https://capacitor.ionicframework.com/). Esta alternativa usa contenedores para que la aplicación sea [más fácil de](https://ionicframework.com/blog/announcing-capacitor-1-0/)usar para la Web.

### <a name="get-started-with-ionic-by-installing-required-tools"></a>Comience con el iónico mediante la instalación de las herramientas necesarias

Para empezar a crear una aplicación Web de PWA o híbrida con iónico, primero debe instalar las siguientes herramientas:

- Node.js para interactuar con el ecosistema iónico. [Descargue NodeJS para Windows](https://nodejs.org/en/) o siga la [Guía de instalación de NodeJS](../nodejs/setup-on-wsl2.md) con el subsistema de Windows para Linux (WSL). Puede que desee considerar la posibilidad de usar [node version Manager (NVM)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) si va a trabajar con varios proyectos y versión de NodeJS.

- VS Code para escribir el código. [Descargue vs code para Windows](https://code.visualstudio.com/). También puede instalar la [extensión remota WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) si prefiere compilar la aplicación con una línea de comandos de Linux.

- Windows terminal para trabajar con la interfaz de la línea de comandos (CLI) preferida. [Instale Windows terminal desde Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

- Git para el control de versiones. [Descargue git](https://git-scm.com/downloads).

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>Crear un nuevo proyecto con el Cordova y angular

Instale iónico y Cordova; para ello, escriba lo siguiente en la línea de comandos:

```bash
npm install -g @ionic/cli cordova
```

Cree una aplicación de angular iónico mediante la plantilla de aplicación "pestañas". para ello, escriba el comando:

```bash
ionic start photo-gallery tabs
```

Cambie a la carpeta de la aplicación:

```bash
cd photo-gallery
```

Ejecute la aplicación en el explorador Web:

```bash
ionic serve
```

Para obtener más información, consulte los [documentos iónico de angular de Cordova](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro). Visite la sección [creación de una aplicación de angular de PWA](https://ionicframework.com/docs/angular/pwa) de los documentos iónicos para aprender a hacer que la aplicación no sea híbrida en un PWA.

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>Crear un nuevo proyecto con condensador iónico y angular

Instale iónico y Cordova-res; para ello, escriba lo siguiente en la línea de comandos:

```bash
npm install -g @ionic/cli native-run cordova-res
```

Cree una aplicación de angular iónico con la plantilla de aplicación "pestañas" y agregue el condensador escribiendo el comando:

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

Cambie a la carpeta de la aplicación:

```bash
cd photo-gallery
```

Agregar componentes para convertir la aplicación en una PWA:

```bash
npm install @ionic/pwa-elements
```

Importar @ionic/pwa-elements mediante agregue lo siguiente al `src/main.ts` archivo:

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

Ejecute la aplicación en el explorador Web:

```bash
ionic serve
```

Para obtener más información, consulte los [documentos de angular del condensador iónico](https://ionicframework.com/docs/angular/your-first-app). Visite la sección [creación de una aplicación de angular de PWA](https://ionicframework.com/docs/angular/pwa) de los documentos iónicos para aprender a hacer que la aplicación no sea híbrida en un PWA.  

## <a name="create-a-new-project-with-ionic-and-react"></a>Crear un nuevo proyecto con iónico y reAct

Para instalar la CLI iónico, escriba lo siguiente en la línea de comandos:

```bash
npm install -g @ionic/cli
```

Cree un nuevo proyecto con reAct escribiendo el comando:

```bash
ionic start myApp blank --type=react
```

Cambie a la carpeta de la aplicación:

```bash
cd myApp
```

Ejecute la aplicación en el explorador Web:

```bash
ionic serve
```

Para obtener más información, consulte los [documentos iónico reAct](https://ionicframework.com/docs/react/quickstart). Visite la sección [creación de una aplicación de reAct de PWA](https://ionicframework.com/docs/react/pwa) de los documentos iónicos para obtener información sobre cómo migrar la aplicación de un entorno híbrido a un PWA.

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>Prueba de la aplicación iónico en un dispositivo o emulador

Para probar la aplicación iónico en un dispositivo Android, conecte el dispositivo (Asegúrese de[que está habilitado en primer lugar para el desarrollo](emulator.md#enable-your-device-for-development)) y, a continuación, en la línea de comandos, escriba:

```bash
ionic cordova run android
```

Para probar la aplicación de iónico en un emulador de dispositivos Android, debe:

1. [Instale los componentes necesarios: el kit de desarrollo de Java (JDK), Gradle y el Android SDK](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements).

2. [Cree un dispositivo virtual Android (AVD)](https://developer.android.com/studio/run/managing-avds.html).

3. Escriba el comando para el iónico para compilar e implementar la aplicación en el emulador: `ionic cordova emulate [<platform>] [options]` . En este caso, el comando debe ser:

```bash
ionic cordova emulate android --list
```

Vea el [emulador de Cordova](https://ionicframework.com/docs/cli/commands/cordova-emulate) en los documentos iónicos para obtener más información.

## <a name="additional-resources"></a>Recursos adicionales

- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](/dual-screen/android/)

- [Agregar exclusiones de Windows Defender para mejorar el rendimiento](defender-settings.md)

- [Habilitar la compatibilidad con la virtualización para mejorar el rendimiento del emulador](emulator.md#enable-virtualization-support)
---
title: ReAct Native para el desarrollo de Android en Windows
description: Introducción al desarrollo de aplicaciones Android con Xamarin nativo en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, reAct nativo, emulador, exponer, paquete de metro, terminal
ms.date: 04/28/2020
ms.openlocfilehash: db49e3ed12fee8e7ced7680e305a84afb89f32a2
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255239"
---
# <a name="get-started-developing-for-android-using-react-native"></a>Introducción al desarrollo para Android con reAct Native

Esta guía le ayudará a empezar a usar reAct Native en Windows para crear una aplicación multiplataforma que funcione en dispositivos Android.

## <a name="overview"></a>Información general

ReAct Native es un marco de trabajo de aplicaciones móviles [de código abierto](https://github.com/facebook/react-native) creado por Facebook. Se usa para desarrollar aplicaciones para Android, iOS, Web y UWP (Windows) que proporcionan controles de interfaz de usuario nativos y acceso completo a la plataforma nativa. El uso de reAct Native requiere una comprensión de los fundamentos de JavaScript.

## <a name="get-started-with-react-native-by-installing-required-tools"></a>Introducción a reAct Native mediante la instalación de las herramientas necesarias

1. [Instale Visual Studio Code](https://code.visualstudio.com) (o el editor de código que prefiera).

2. [Instale Android Studio para Windows](https://developer.android.com/studio). Android Studio instala el Android SDK más reciente de forma predeterminada. ReAct Native requiere el SDK de Android 6,0 (Marshmallow) o superior. Se recomienda usar el SDK más reciente.

3. Cree rutas de acceso de variables de entorno para el SDK de Java y Android SDK:
    - En el menú de búsqueda de Windows, escriba: "editar las variables de entorno del sistema", se abrirá la ventana **propiedades del sistema** .
    - Elija **variables de entorno...** y, a continuación, elija **nuevo...** en **variables de usuario**.
    - Escriba el nombre y el valor de la variable (ruta de acceso). Las rutas de acceso predeterminadas para los SDK de Java y Android son las siguientes. Si ha elegido una ubicación específica para instalar los SDK de Java y Android, asegúrese de actualizar las rutas de acceso de la variable en consecuencia.
        - JAVA_HOME: C:\Archivos de Files\Android\Android Studio\jre\jre
        - ANDROID_HOME: C:\Users\username\AppData\Local\Android\Sdk

    ![Captura de pantalla de la adición de la ruta de variable de entorno](../images/add-environmental-variable-path.png)

4. [Instalación de NodeJS para Windows](https://nodejs.org/en/) Puede considerar la posibilidad de usar [node version Manager (NVM) para Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) si va a trabajar con varios proyectos y versión de NodeJS. Se recomienda instalar la versión más reciente de LTS para los proyectos nuevos.

> [!NOTE]
> También puede ser conveniente tener en cuenta la instalación y el uso de [Windows terminal](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) para trabajar con la interfaz de la línea de comandos (CLI) preferida, así como con [git para el control de versiones](https://git-scm.com/downloads). [Java JDK](https://www.oracle.com/java/technologies/javase-downloads.html) se incluye con Android Studio v 2.2 +, pero si necesita actualizar el JDK por separado de Android Studio, use el [instalador de Windows x64](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html).

## <a name="create-a-new-project-with-react-native"></a>Crear un nuevo proyecto con reAct Native

1. Use NPM para instalar la utilidad de línea de comandos de la [CLI de exposición](https://docs.expo.io/versions/latest/) desde el símbolo del sistema de Windows, PowerShell, [Windows terminal](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)o el terminal integrado en vs Code (ver > terminal integrado).

    ```powershell
    npm install -g expo-cli
    ```

2. Use exponer para crear una aplicación nativa reAct que se ejecute en iOS, Android y Web. Tendrá que elegir entre las plantillas de proyecto, que incluyen **en blanco**, **en blanco (typescript)**, **pestañas** (pantallas de ejemplo mediante reAct-Navigation), **mínima**o **mínima (typescript)**.

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > Si se usa para usar `npx create-react-native-app`, eso seguirá funcionando, pero la inicialización de la CLI de exposición tiene [algunas ventajas adicionales](https://github.com/react-native-community/discussions-and-proposals/issues/23).

3. Abra el nuevo directorio "My-New-App":

    ```powershell
    cd my-new-app
    ```

4. Para ejecutar el proyecto, escriba el siguiente comando. Se abrirá una ventana de localhost en el explorador de Internet predeterminado que muestra el paquete metro de nodo. También mostrará un código QR tanto en la línea de comandos como en la ventana del explorador del paquete metro. * Puede usar el comando: `npm start` o `npm run android` también.

     ```powershell
    expo start
    ```

    ![Captura de pantalla del paquete metro en el explorador](../images/metro-bundler.png)

5. Para ver el proyecto que se ejecuta en un dispositivo Android, debe instalar primero [la aplicación cliente de exposición con el Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) en el dispositivo Android. Una vez instalada la aplicación cliente de exposición, ábrala en el dispositivo y seleccione **examinar código QR**. Una vez registrado el código QR, podrá ver el paquete compilar tanto en el dispositivo como en la ventana del paquete metro que se ejecuta en localhost en el explorador.

6. Para ver el proyecto que se ejecuta en un emulador de Android, primero deberá abrir Android Studio y, a continuación, crear e iniciar un dispositivo virtual. **Tools** > **AVD Manager** > **[+ Create Virtual Device.](https://developer.android.com/studio/run/managing-avds#createavd)**... Una vez creado el dispositivo virtual, seleccione el botón Launch ▷ en la columna **Actions** del Device Manager virtual de Android para iniciar la emulación del dispositivo. Una vez que el dispositivo virtual está abierto, vuelva a la ventana de paquete metro que se ejecuta en la ventana del explorador de Internet y seleccione "ejecutar en dispositivo o emulador Android" en la columna izquierda. Debería ver un mensaje emergente que le permite saber que el paquete de metro está "intentando abrir un simulador..." y, después, vea la aplicación cliente de exposición abierta en el dispositivo Android emulado y, una vez que haya terminado de descargar el paquete de JavaScript, verá que se muestra la aplicación de reAct Native. (Si tiene problemas, [Compruebe los documentos de exposición del emulador de Android](https://docs.expo.io/workflow/android-studio-emulator/)).

7. Abra el proyecto de reAct Native para empezar a trabajar en la aplicación. Debería ver los cambios actualizados automáticamente en la aplicación que se ejecuta a través del cliente de exposición en el dispositivo o en el Android Emulator.

8. Intente cambiar el texto de la vista de la página de aterrizaje para decir: "Hola mundo!". Puede hacerlo en el IDE que prefiera. (Se recomienda VS Code o Android Studio). El archivo de la página de aterrizaje variará en función de la plantilla que elija. Puede ser `App.js`, `App.tsx`o. `HomeScreen.js`

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. Intente agregar una imagen. En primer lugar, debe crear una carpeta en el mismo nivel que las carpetas "Android" y "iOS" de la aplicación, vamos a llamarla "images". Coloque una imagen en esa carpeta, `your-image.png` por ejemplo. Use el formato siguiente para hacer referencia a la imagen y darle estilo a un alto y un ancho.

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> Si desea agregar compatibilidad con la aplicación de reAct Native para que se ejecute como una aplicación de Windows 10, consulte la introducción [a reAct Native for Windows](https://microsoft.github.io/react-native-windows/docs/getting-started) docs.

## <a name="additional-resources"></a>Recursos adicionales

- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Agregar exclusiones de Windows Defender para mejorar el rendimiento](defender-settings.md)

- [Habilitar la compatibilidad con la virtualización para mejorar el rendimiento del emulador](emulator.md#enable-virtualization-support)

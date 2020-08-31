---
title: Mejorar la velocidad de rendimiento mediante la actualización de la configuración de defender
description: Obtenga información acerca de cómo mejorar la velocidad de rendimiento y los tiempos de compilación actualizando la configuración de Windows Defender para excluir la comprobación de los tipos de archivo especificados.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Windows Defender, configuración, configuración, exclusiones,% USERPROFILE%, devenv.exe, rendimiento, velocidad, compilación, Gradle
ms.date: 04/28/2020
ms.openlocfilehash: 0437ffc263c618e52c7a3e4dc3256e9fcd502c8e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154869"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>Actualización de la configuración de Windows Defender para mejorar el rendimiento

En esta guía se explica cómo configurar exclusiones en la configuración de seguridad de Windows Defender con el fin de mejorar los tiempos de compilación y la velocidad de rendimiento general de la máquina Windows.

## <a name="windows-defender-overview"></a>Información general de Windows Defender

En Windows 10, versión 1703 y versiones posteriores, la aplicación [antivirus de Windows Defender](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) forma parte de la seguridad de Windows. Windows Defender pretende mantener su equipo seguro con protección en tiempo real integrada frente a virus, ransomware, spyware y otras amenazas de seguridad.

**Sin embargo**, la protección en tiempo real de Windows Defender también reducirá drásticamente el acceso al sistema de archivos y la velocidad de compilación al desarrollar aplicaciones Android.

Durante el proceso de compilación de Android, se crean muchos archivos en el equipo. Con el análisis en tiempo real de antivirus habilitado, el proceso de compilación se detiene cada vez que se crea un nuevo archivo mientras el antivirus lo examina.

Afortunadamente, Windows Defender tiene la capacidad de excluir archivos, directorios de proyecto o tipos de archivo que sabe que son seguros del proceso de detección de antivirus.

> [!WARNING]
> Para asegurarse de que el equipo es seguro contra software malintencionado, no debe deshabilitar completamente el análisis en tiempo real ni el software antivirus de Windows Defender.

## <a name="add-exclusions-to-windows-defender"></a>Agregar exclusiones a Windows Defender

Para mejorar la velocidad de compilación de Android, agregue exclusiones en el [Security Center de Windows Defender](windowsdefender://) mediante:

1. Seleccionar el botón **Inicio** del menú Windows
2. Escriba la **seguridad de Windows**
3. Seleccionar **protección contra amenazas y virus**
4. Seleccione **Administrar configuración** en **configuración de protección contra amenazas de virus &**
5. Desplácese hasta el encabezado **exclusiones** y seleccione **Agregar o quitar exclusiones** .
6. Seleccione **+ Agregar una exclusión**. A continuación, tendrá que elegir si la exclusión que desea agregar es un **archivo**, una **carpeta**, un **tipo de archivo**o un **proceso**.

![Captura de pantalla para agregar exclusión de Windows Defender](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>Exclusiones recomendadas

En la siguiente lista se muestra la ubicación predeterminada de cada directorio de Android Studio que se recomienda agregar como exclusión del análisis en tiempo real de Windows Defender:

- Caché de Gradle: `%USERPROFILE%\.gradle`
- Proyectos de Android Studio: `%USERPROFILE%\AndroidStudioProjects`
- Android SDK: `%USERPROFILE%\AppData\Local\Android\SDK`
- Archivos del sistema de Android Studio: `%USERPROFILE%\.AndroidStudio<version>\system`

Es posible que estas ubicaciones de directorio no se apliquen al proyecto si no ha usado las ubicaciones predeterminadas establecidas por Android Studio o si ha descargado un proyecto de GitHub (por ejemplo). Considere la posibilidad de agregar una exclusión al directorio del proyecto de desarrollo de Android actual, donde pueda encontrarse.

Las exclusiones adicionales que puede tener en cuenta incluyen:

- Proceso del entorno de desarrollo de Visual Studio: `devenv.exe`
- Proceso de compilación de Visual Studio: `msbuild.exe`
- Directorio JetBrains: `%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

Para obtener más información acerca de cómo agregar exclusiones de detección de antivirus, incluido cómo personalizar ubicaciones de directorio para directiva de grupo entornos controlados, consulte la sección [impacto de antivirus](https://developer.android.com/studio/intro/studio-config#antivirus-impact) de la documentación de Android Studio.

> [!Note]
> Daniel Knoodle ha configurado un repositorio de GitHub con scripts recomendados para agregar [exclusiones de Windows Defender para Visual Studio 2017](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10).

## <a name="additional-resources"></a>Recursos adicionales

- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](/dual-screen/android/)

- [Agregar exclusiones de Windows Defender para mejorar el rendimiento](./defender-settings.md)

- [Habilitar la compatibilidad con la virtualización para mejorar el rendimiento del emulador](./emulator.md#enable-virtualization-support)
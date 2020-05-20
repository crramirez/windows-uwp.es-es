---
title: Ejecución del emulador o dispositivo Android desde Windows
description: Pruebe la aplicación en un emulador o dispositivo Android desde Windows y habilite la virtualización con Hyper-v y la plataforma del hipervisor de Windows (WHPX).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, emulador, dispositivo virtual, configuración de dispositivos, habilitar dispositivo, desarrollador, configuración, virtualización, Visual Studio, Hyper-v, Intel, haxm, AMD, plataforma del hipervisor de Windows, WHPX
ms.date: 04/28/2020
ms.openlocfilehash: c651661d573695902368ffa595ce5d3014791a9a
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255159"
---
# <a name="test-on-an-android-device-or-emulator"></a>Prueba en un emulador o dispositivo Android

Hay varias maneras de probar y depurar la aplicación Android con un dispositivo o emulador real en el equipo Windows. En esta guía hemos descrito algunas recomendaciones.

## <a name="run-on-a-real-android-device"></a>Ejecución en un dispositivo Android real

Para ejecutar la aplicación en un dispositivo Android real, primero deberá habilitar el dispositivo Android para el desarrollo. Las opciones del Desarrollador en Android se han ocultado de forma predeterminada, ya que la versión 4,2 y su habilitación pueden variar en función de la versión de Android.

### <a name="enable-your-device-for-development"></a>Habilitar el dispositivo para el desarrollo

Para un dispositivo que ejecuta una versión reciente de Android 9.0 +:

1. Conecte el dispositivo a su equipo de desarrollo de Windows con un cable USB. Es posible que reciba una notificación para instalar un controlador USB.
2. Abra la pantalla de **configuración** en el dispositivo Android.
3. Seleccione **acerca del teléfono**.
4. Desplácese hasta la parte inferior y pulse **número de compilación** siete veces hasta **que ya sea un desarrollador.** es visible.
5. Vuelva a la pantalla anterior, seleccione **sistema**.
6. Seleccione **avanzadas**, desplácese hasta la parte inferior y pulse **Opciones de desarrollador**.
7. En la ventana **Opciones de desarrollador** , desplácese hacia abajo para buscar y habilitar la **depuración USB**.

Para un dispositivo que ejecuta una versión anterior de Android, consulte [configurar el dispositivo para el desarrollo](https://docs.microsoft.com/xamarin/android/get-started/installation/set-up-device-for-development).

### <a name="run-your-app-on-the-device"></a>Ejecutar la aplicación en el dispositivo

1. En la barra de herramientas Android Studio, seleccione la aplicación en el menú desplegable de **configuraciones de ejecución** .

    ![Android Studio menú de configuración de ejecución](../images/android-run-config-menu.png)

2. En el menú desplegable **dispositivo de destino** , seleccione el dispositivo en el que desea ejecutar la aplicación.

    ![Android Studio menú del dispositivo de destino](../images/android-target-device-menu.png)

3. Seleccione ejecutar ▷. Esto iniciará la aplicación en el dispositivo conectado.

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>Ejecutar la aplicación en un dispositivo Android virtual mediante un emulador

Lo primero que hay que saber sobre la ejecución de un emulador de Android en el equipo Windows es que, independientemente del IDE (Android Studio, Visual Studio, etc.), el rendimiento del emulador se mejora considerablemente al habilitar la compatibilidad con la virtualización.

### <a name="enable-virtualization-support"></a>Habilitar la compatibilidad con la virtualización

Antes de crear un dispositivo virtual con el emulador de Android, se recomienda habilitar la virtualización activando las características de Hyper-V y de la plataforma del hipervisor de Windows (WHPX). Esto permitirá que el procesador del equipo Mejore significativamente la velocidad de ejecución del emulador.

> Para ejecutar la plataforma del hipervisor de Windows y Hyper-V, el equipo debe:
>
> * Tener 4 GB de memoria disponible
> * Tiene un procesador Intel de 64 bits o CPU Ryzen AMD con traducción de direcciones de segundo nivel (SLAT)
> * Ejecutar Windows 10 Build 1803 + ([comprobar el número de compilación](ms-settings:about))
> * Se han actualizado los controladores de gráficos (Device Manager > adaptadores de pantalla > Update driver)
>
> Si el equipo no se ajusta a este criterio, es posible que pueda ejecutar el hipervisor [Intel HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) o [AMD](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors). Para obtener más información, vea el artículo: [aceleración de hardware para el rendimiento del emulador](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) o la [documentación del emulador de Android Studio](https://developer.android.com/studio/run/emulator).

1. Compruebe que el hardware y el software del equipo son compatibles con Hyper-V; para ello, abra un símbolo del sistema y escriba el comando:`systeminfo`

    ![Requisitos de Hyper-V de SystemInfo en el símbolo del sistema](../images/systeminfo.png)

2. En el cuadro de búsqueda de Windows (inferior izquierdo), escriba "características de Windows". Seleccione **activar o desactivar las características de Windows** en los resultados de la búsqueda.

3. Cuando aparezca la lista de **características de Windows** , desplácese para encontrar **Hyper-V** (incluye las herramientas de administración y la plataforma) y la **plataforma del hipervisor de Windows**, asegúrese de que la casilla esté activada para habilitar ambas y, a continuación, seleccione **Aceptar**.

4. Reinicia el equipo cuando se te solicite.

### <a name="emulator-for-native-development-with-android-studio"></a>Emulador para el desarrollo nativo con Android Studio

Al compilar y probar una aplicación nativa de Android, se recomienda [usar Android Studio](./native-android.md). Una vez que la aplicación esté lista para realizar pruebas, puede compilar y ejecutar la aplicación de la siguiente manera:

1. En la barra de herramientas Android Studio, seleccione la aplicación en el menú desplegable de **configuraciones de ejecución** .

    ![Android Studio menú de configuración de ejecución](../images/android-run-config-menu.png)

2. En el menú desplegable **dispositivo de destino** , seleccione el dispositivo en el que desea ejecutar la aplicación.

    ![Android Studio menú del dispositivo de destino](../images/android-target-device-menu.png)

3. Seleccione ejecutar ▷. Se iniciará el [Android Emulator](https://developer.android.com/studio/run/emulator).

> [!TIP]
> Una vez que la aplicación está instalada en el dispositivo emulador, puede usar [aplicar cambios](https://developer.android.com/studio/run#apply-changes) para implementar ciertos códigos y cambios de recursos sin generar un nuevo apk.

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>Emulador para el desarrollo multiplataforma con Visual Studio

Hay muchas [Opciones del emulador de Android](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) disponibles para equipos Windows. Se recomienda usar el [emulador de Android](https://developer.android.com/studio/run/emulator)de Google, ya que ofrece acceso a las imágenes del sistema operativo Android más recientes y a los servicios Google Play.

### <a name="install-android-emulator-with-visual-studio"></a>Instalación del emulador de Android con Visual Studio

1. Si aún no lo tiene instalado, descargue [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Use el Instalador de Visual Studio para [modificar las cargas de trabajo](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) y asegurarse de que tiene la **carga de trabajo desarrollo móvil con .net**.

2. Cree un nuevo proyecto. Una vez que haya [configurado el Android Emulator](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/), puede usar el [Android Device Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#requirements) para crear, duplicar, personalizar e iniciar una variedad de dispositivos virtuales Android. Inicie el Android Device Manager desde el menú herramientas con: **herramientas**  >  **Android**  >  **Android Device Manager**.

3. Una vez que se abra el Android Device Manager, seleccione **+ nuevo** para crear un nuevo dispositivo.

4. Tendrá que asignar un nombre al dispositivo, elegir el tipo de dispositivo base en un menú desplegable, elegir un procesador y la versión del sistema operativo, junto con otras variables para el dispositivo virtual. Para obtener más información, [Android Device Manager pantalla principal](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#main-screen).

5. En la barra de herramientas de Visual Studio, elija entre **depurar** (se asocia al proceso de aplicación que se ejecuta en el emulador después de que se inicie la aplicación) o el modo de **versión** (deshabilita el depurador). A continuación, elija un dispositivo virtual en el menú desplegable de dispositivos y seleccione el botón de **reproducción** ▷ para ejecutar la aplicación en el emulador.

    ![Inicio de Visual Studio Android Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>Recursos adicionales

- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Agregar exclusiones de Windows Defender para mejorar el rendimiento](defender-settings.md)

---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Probar aplicaciones de Surface Hub con Visual Studio
description: El simulador de Visual Studio ofrece un entorno para diseñar, desarrollar, depurar y probar aplicaciones para UWP, incluidas las aplicaciones creadas para Surface Hub.
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37c7f9edbaee008b6e16ef2ca202ff5cbcf39ca2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "67317503"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Probar aplicaciones de Surface Hub con Visual Studio
El simulador de Visual Studio ofrece un entorno donde puede diseñar, desarrollar, depurar y probar aplicaciones de la Plataforma universal de Windows (UWP), incluidas las aplicaciones que hayas creado para Microsoft Surface Hub. El simulador no usa la misma interfaz de usuario que Surface Hub, pero resulta útil para probar el aspecto y el comportamiento de la aplicación con la resolución y el tamaño de la pantalla de Surface Hub.

Para más información sobre el simulador, consulta [Ejecutar aplicaciones para UWP en el simulador](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Agregar resoluciones de Surface Hub al simulador
Para agregar resoluciones de Surface Hub al simulador:

1. Crea una configuración para el dispositivo Surface Hub de 55" y guarda el siguiente código XML en un archivo llamado *HardwareConfigurations-SurfaceHub55.xml*.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. Crea una configuración para el dispositivo Surface Hub de 84" y guarda el siguiente código XML en un archivo llamado *HardwareConfigurations-SurfaceHub84.xml*.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. Copia los dos archivos XML en *C:\Archivos de programa (x86)\Archivos comunes\Microsoft Shared\Windows Simulator\\&lt;número de versión&gt;\HardwareConfigurations*.

   > [!NOTE]
   > Se requieren privilegios administrativos para guardar archivos en esta carpeta.

4. Ejecuta la aplicación en el emulador de Visual Studio. Haz clic en el botón **Cambiar resolución** en la paleta y selecciona una configuración de Surface Hub en la lista.

    ![Resoluciones del simulador de Visual Studio](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Activa el modo de tableta](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) para simular mejor la experiencia de un equipo Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Implementar aplicaciones en un dispositivo Surface Hub desde Visual Studio
La implementación manual de una aplicación en un dispositivo Surface Hub es un proceso sencillo.

### <a name="enable-developer-mode"></a>Habilitar el modo de desarrollador
De forma predeterminada, Surface Hub solo instala las aplicaciones de Microsoft Store. Para instalar aplicaciones firmadas por otros orígenes, debes habilitar el modo de desarrollador.

> [!NOTE]
> Una vez habilitado el modo de desarrollador, tendrás que restablecer el dispositivo Surface Hub si quieres volver a deshabilitarlo. Al restablecer el dispositivo se eliminan todas las configuraciones y los archivos de usuario locales y, a continuación, se vuelve a instalar Windows.

1. En el menú **Inicio** de Surface Hub, abre la aplicación Configuración.

   > [!NOTE]
   > Se requieren privilegios administrativos para tener acceso a la aplicación Configuración del dispositivo Surface Hub.

2. Ve a **Actualización y seguridad \> Para desarrolladores**.

3. Elige el **Modo de desarrollador** y acepta la advertencia.

### <a name="deploy-your-app-from-visual-studio"></a>Implementar la aplicación desde Visual Studio
Para más información sobre el proceso de implementación, consulta [Implementar y depurar aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Esta característica requiere Visual Studio 2015 Update 1 o versiones posteriores, pero se recomienda usar la versión más reciente actualizada de Visual Studio. Una instancia de Visual Studio actualizada incluirá todas las actualizaciones de seguridad y desarrollo más recientes.

1. Navega hasta la lista desplegable de destino de depuración junto al botón **Iniciar depuración** y selecciona **Equipo remoto**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Lista desplegable de destinos de depuración de Visual Studio](images/vs-debug-target.png)

2. Escribe la dirección IP del Surface Hub. Asegúrate de que el modo de autenticación **Universal** está seleccionado.

   > [!TIP] 
   > Una vez habilitado el modo de desarrollador, encontrarás la dirección IP del Surface Hub en la pantalla de bienvenida.

3. Selecciona **Iniciar depuración (F5)** para implementar y depurar la aplicación en el Surface Hub o presiona Ctrl+F5 para implementar únicamente la aplicación.

   > [!TIP]
   > Si el dispositivo Surface Hub muestra la pantalla de bienvenida, elige cualquier botón para descartarla.

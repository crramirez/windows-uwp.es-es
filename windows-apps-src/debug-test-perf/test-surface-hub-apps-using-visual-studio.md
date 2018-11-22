---
author: PatrickFarley
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Probar aplicaciones de Surface Hub con Visual Studio
description: El simulador de Visual Studio ofrece un entorno para diseñar, desarrollar, depurar y probar aplicaciones para UWP, incluidas las aplicaciones creadas para Surface Hub.
ms.author: pafarley
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 63214ce47bffc5a0b13f421e5185d06cd810ea34
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2018
ms.locfileid: "7578942"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Probar aplicaciones de Surface Hub con Visual Studio
El simulador de Visual Studio ofrece un entorno donde puede diseñar, desarrollar, depurar y probar aplicaciones de la Plataforma universal de Windows (UWP), incluidas las aplicaciones que hayas creado para Microsoft Surface Hub. El simulador no usa la misma interfaz de usuario que Surface Hub, pero es útil para probar el aspecto y el comportamiento con el tamaño de pantalla de Surface Hub y la resolución de la aplicación.

Para obtener más información sobre la herramienta del simulador en general, vea [ejecutar aplicaciones para UWP en el simulador](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Agregar resoluciones de Surface Hub al simulador
Para agregar resoluciones de Surface Hub al simulador:

1. Crear una configuración para el de 55" Surface Hub guardando el siguiente código XML en un archivo denominado *HardwareConfigurations-SurfaceHub55.xml*.  

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

2. Crea una configuración de 84" Surface Hub guardando el siguiente código XML en un archivo denominado *HardwareConfigurations-SurfaceHub84.xml*.

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
   > [Activar el modo de tableta](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) mejor simular la experiencia de un dispositivo Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Implementar aplicaciones en un dispositivo de Surface Hub desde Visual Studio
Implementar manualmente una aplicación en un Surface Hub es un proceso sencillo.

### <a name="enable-developer-mode"></a>Habilitar el modo de desarrollador
De manera predeterminada, Surface Hub solo instala aplicaciones de Microsoft Store. Para instalar aplicaciones firmadas por otros orígenes, debes habilitar el modo de desarrollador.

> [!NOTE]
> Una vez habilitado el modo de desarrollador, tendrás que restablecer el Surface Hub si deseas volver a deshabilitarlo. Al restablecer el dispositivo se eliminan todas las configuraciones y los archivos de usuario locales y luego se vuelve a instalar Windows.

1. En el menú **Inicio** de Surface Hub, abre la aplicación Configuración.

   > [!NOTE]
   > Se requieren privilegios administrativos para tener acceso a la aplicación configuración en Surface Hub.

2. Ve a **actualización y seguridad \ > para desarrolladores**.

3. Elige el **Modo de desarrollador** y acepta la advertencia.

### <a name="deploy-your-app-from-visual-studio"></a>Implementar la aplicación desde Visual Studio
Para obtener más información sobre el proceso de implementación en general, vea [implementación y depuración de aplicaciones para UWP](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Esta característica requiere Visual Studio 2015 Update 1 o posterior, sin embargo, te recomendamos que uses la última versión actualizada de Visual Studio. Una instancia de Visual Studio al día gibe que el de desarrollo más reciente y las actualizaciones de seguridad.

1. Navega hasta la lista desplegable de destino de depuración junto al botón **Iniciar depuración** y selecciona **Equipo remoto**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Lista desplegable de destinos de depuración de Visual Studio](images/vs-debug-target.png)

2. Escribe la dirección IP del Surface Hub. Asegúrate de que el modo de autenticación **Universal** está seleccionado.

   > [!TIP] 
   > Una vez habilitado el modo de desarrollador, encontrarás dirección IP del Surface Hub en la pantalla de bienvenida.

3. Selecciona **Iniciar depuración (F5)** para implementar y depurar la aplicación en el Surface Hub o presiona CTRL+F5 para implementar únicamente la aplicación.

   > [!TIP]
   > Si el Surface Hub está mostrando la pantalla de bienvenida, descártalo seleccionando cualquier botón.

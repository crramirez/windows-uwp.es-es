---
author: mcleblanc
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Probar aplicaciones de Surface Hub con Visual Studio
description: "El simulador de Visual Studio ofrece un entorno para diseñar, desarrollar, depurar y probar aplicaciones para UWP, incluidas las aplicaciones creadas para Surface Hub."
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: 655dffb5f1948724c894de291e119be1322f6e77

---

# Probar aplicaciones de Surface Hub con Visual Studio
El simulador de Visual Studio ofrece un entorno donde puede diseñar, desarrollar, depurar y probar aplicaciones de la Plataforma universal de Windows (UWP), incluidas las aplicaciones que hayas creado para Microsoft Surface Hub. El simulador no usa la misma interfaz de usuario que Surface Hub, pero es útil para probar el aspecto y el comportamiento de la aplicación con la resolución y el tamaño de la pantalla de Surface Hub.

Para obtener más información, consulta [Ejecutar aplicaciones de la Tienda Windows en el simulador](https://msdn.microsoft.com/library/hh441475.aspx).

## Agregar resoluciones de Surface Hub al simulador
Para agregar resoluciones de Surface Hub al simulador:

1. Crea una configuración para Surface Hub de 55" guardando el siguiente XML en un archivo denominado **HardwareConfigurations-SurfaceHub55.xml**.  

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

2. Crea una configuración para Surface Hub de 84" guardando el siguiente XML en un archivo denominado  **HardwareConfigurations-SurfaceHub84.xml**.

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

3. Copia los dos archivos XML en **C:\Archivos de programa (x86)\Archivos comunes\Microsoft Shared\Windows Simulator\\&lt;número de versión&gt;\HardwareConfigurations**.

   > 
            **Nota**
            &nbsp;&nbsp;Se requieren privilegios administrativos para guardar archivos en esta carpeta.

4. Ejecuta la aplicación en el emulador de Visual Studio. Haz clic en el botón **Cambiar resolución** en la paleta y selecciona una configuración de Surface Hub en la lista.

    ![Resoluciones del simulador de Visual Studio](images/vs-simulator-resolutions.png)

   > 
            **Sugerencia**
            &nbsp;&nbsp;
            [Activa el modo de tableta](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) para simular mejor la experiencia en un Surface Hub.

## Implementar aplicaciones en un Surface Hub desde Visual Studio
La implementación manual de una aplicación es un proceso sencillo.

### Habilitar el modo de desarrollador
De manera predeterminada, Surface Hub solo instala aplicaciones de la Tienda Windows. Para instalar aplicaciones firmadas por otros orígenes, debes habilitar el modo de desarrollador.

> 
            **Nota**
            &nbsp;&nbsp;Una vez habilitado el modo de desarrollador, se deberá restablecer el Surface Hub para volver a deshabilitarlo. Al restablecer el dispositivo se eliminan todas las configuraciones y los archivos de usuario locales y luego se vuelve a instalar Windows.

1. En el menú **Inicio** de Surface Hub, abre la aplicación Configuración.

   >  
            **Nota**
            &nbsp;&nbsp;Se requieren privilegios administrativos para acceder a la aplicación Configuración.

2. Navega hasta **Actualización y seguridad > Para desarrolladores**.

3. Elige el **Modo de desarrollador** y acepta la advertencia.

### Implementar la aplicación desde Visual Studio
Para obtener más información, consulta [Implementación y depuración de aplicaciones para la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > 
            **Nota**
            &nbsp;&nbsp;Esta característica requiere al menos **Visual Studio 2015 Update 1**.

1. Navega hasta la lista desplegable de destino de depuración junto al botón **Iniciar depuración** y selecciona **Equipo remoto**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Lista desplegable de destinos de depuración de Visual Studio](images/vs-debug-target.png)

2. Escribe la dirección IP del Surface Hub. Asegúrate de que el modo de autenticación **Universal** está seleccionado.

   > 
            **Sugerencia**
            &nbsp;&nbsp;Una vez habilitado el modo de desarrollador, encontrarás la dirección IP del Surface Hub en la pantalla de bienvenida.

3. Elige **Iniciar depuración (F5)** para implementar y depurar la aplicación en el Surface Hub o presiona Ctrl+F5 para implementar únicamente la aplicación.

   > 
            **Sugerencia**
            &nbsp;&nbsp;Si el Surface Hub se encuentra en la pantalla de bienvenida, descártalo seleccionando cualquier botón.



<!--HONumber=Jun16_HO4-->



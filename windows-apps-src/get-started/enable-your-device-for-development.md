---
author: martinekuan
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Habilitar el dispositivo para el desarrollo
description: Existe un enfoque diferente para el desarrollo en dispositivos de Windows 10.
keywords: enable device
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4c890d6202a3151e8fc0cf03b3ff33b98cd6a863

---
# Habilitar el dispositivo para el desarrollo

Existe un enfoque diferente para el desarrollo en dispositivos de Windows 10. Ya no se requiere una licencia de desarrollador para cada dispositivo que desees usar para desarrollar, instalar o probar la aplicación. El dispositivo solo se habilita una vez para estas tareas desde la configuración del dispositivo. Eso es todo. Ya no es necesario renovar las licencias de desarrollador cada 30 o 90 días.

Si usas un dispositivo Windows 8.1 para desarrollar o probar las aplicaciones con Microsoft Visual Studio 2013 o Microsoft Visual Studio 2015, aún es necesario [obtener una licencia de desarrollador](https://msdn.microsoft.com/library/windows/apps/Hh974578) o [registrar el Windows Phone](https://msdn.microsoft.com/library/windows/apps/Dn614128).

## Usar las funciones para desarrolladores

### Desarrollar la aplicación con Microsoft Visual Studio

Si usas Microsoft Visual Studio en un dispositivo Windows 10 y abres una solución para una aplicación de Windows 8.1 o Windows 10, se te pedirá que habilites el dispositivo con este cuadro de diálogo. Es necesario habilitar el dispositivo para poder usar los diseñadores y depurar la aplicación.

![Habilitar el cuadro de diálogo de modo de desarrollador que se muestra en Visual Studio](images/latestenabledialog.png)

Cuando veas este cuadro de diálogo, haz clic en la **configuración para desarrolladores** para ir directamente a la página **Actualización y seguridad**, tal como se muestra a continuación. También puedes hacer clic en **Aceptar** y, a continuación, seguir estos pasos para habilitar el dispositivo Windows 10 para el desarrollo.

### Habilitar los dispositivos Windows 10

Para Windows 10, elige las funciones de desarrollador que quieras habilitar en el dispositivo. Se incluyen todos los dispositivos: equipos de escritorio, tabletas y teléfonos con Windows 10. Puedes habilitar un dispositivo para desarrollo o solo para instalación de prueba.

-   Una vez instalada la *instalación de prueba*, esta ejecuta o prueba una aplicación que la Tienda Windows no ha certificado. Por ejemplo, una aplicación interna de tu empresa solamente.
-   El *Modo de desarrollador* te permite realizar la instalación de prueba de aplicaciones y también ejecutar aplicaciones desde Visual Studio en modo de depuración.

**Nota**  Aunque realices la instalación de prueba de aplicaciones, solo debes instalarlas desde orígenes de confianza. Cuando realizas la instalación de prueba de una aplicación que la Tienda Windows no ha certificado, admites que has obtenido todos los derechos necesarios para este fin y que eres el único responsable de los perjuicios que la instalación y la ejecución de esta aplicación puedan causar. Consulta la sección Windows &gt; Tienda Windows de esta [declaración de privacidad](http://go.microsoft.com/fwlink/?LinkId=521839).

**Usar las funciones para desarrollador**

1.  En el dispositivo que desees habilitar, ve a **Configuración**. Elige **Actualización y seguridad** y luego **Para desarrolladores**.
2.  Elige el nivel de acceso que necesitas. Para obtener información detallada sobre las opciones, consulta la sección [¿Qué opciones de configuración debo elegir, instalación de prueba de aplicaciones o modo de desarrollador?](#WhichSettings)
3.  Lee el aviso de declinación de responsabilidades para la configuración elegida y luego haz clic en **Sí** para aceptar el cambio.

Esta es la página de configuración de la familia de dispositivos de escritorio.

![Ve a Configuración, elige Actualización y seguridad y luego elige Para desarrolladores para ver las opciones.](images/devmode-pc-options.png)

Esta es la página de configuración de la familia de dispositivos móviles.

![En la sección Configuración del teléfono, elige Actualización y seguridad.](images/devmode-mob.png)

### ¿Qué opciones de configuración debo elegir, instalación de prueba de aplicaciones o modo de desarrollador?

De forma predeterminada, solo se pueden instalar aplicaciones para plataforma universal de Windows (UWP) desde la Tienda Windows. Si modificas esta configuración para usar las funciones de desarrollador, puede cambiar el nivel de seguridad de tu dispositivo. No debes instalar aplicaciones proveniente de orígenes sin comprobar.

**Instalación de prueba de aplicaciones**

Por lo general, las empresas y las instituciones educativas que necesitan instalar aplicaciones personalizadas en dispositivos administrados sin necesidad de usar la Tienda Windows eligen la opción de instalación de prueba de aplicaciones. En este caso, es habitual que la organización aplique una directiva que deshabilita la opción *Aplicaciones de la Tienda Windows*, tal como se mostró anteriormente en la imagen de la página de configuración del teléfono. La organización también proporciona el certificado necesario y la ubicación de instalación para hacer la instalación de prueba de aplicaciones. Para obtener más información, consulta los artículos de TechNet [Realizar la instalación de prueba de aplicaciones de línea de negocio en Windows 10](https://technet.microsoft.com/library/mt269549.aspx) e [Introducción a la implementación de aplicaciones en Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Información específica de la familia de dispositivos

-   En la familia de dispositivos de escritorio: puedes instalar un paquete de la aplicación (.appx) y cualquier certificado necesario para ejecutar la aplicación, si ejecutas el script de Windows PowerShell que se creó con el paquete ("Add-AppDevPackage.ps1").

-   En la familia de dispositivos móviles: si el certificado necesario ya está instalado, puedes pulsar en el archivo para instalar cualquier .appx que te hayan enviado por correo electrónico o que tengas en una tarjeta SD.

La **instalación de prueba de aplicaciones** es una opción más segura que el modo de desarrollador porque no permite instalar en el dispositivo aplicaciones que no tengan un certificado de confianza.

**Modo de desarrollador**

Además de la instalación de prueba, la opción de modo de desarrollador habilita la depuración y otras opciones de implementación. Reemplaza el requisito de Windows 8.1 que exige una licencia de desarrollador.

Información específica de la familia de dispositivos

-   En la familia de dispositivos de escritorio:

    Habilita el modo de desarrollador para desarrollar y depurar aplicaciones en Visual Studio. Tal como mencionamos anteriormente, si este modo no está habilitado, en Visual Studio se te pedirá que lo habilites.

-   En la familia de dispositivos móviles:

    Habilita el modo de desarrollador para implementar aplicaciones desde Visual Studio y depurarlas en el dispositivo.

    Puedes pulsar en el archivo para instalar cualquier .appx recibido por correo electrónico o que tengas en una tarjeta SD. No instales aplicaciones provenientes de orígenes sin comprobar.

**Sugerencia**  
Hay varias herramientas que puedes usar para implementar una aplicación de un equipo con Windows 10 a un dispositivo móvil con Windows 10. Ambos dispositivos deben estar conectados a la misma subred de la red mediante una conexión con cable o inalámbrica, o bien deben estar conectados mediante USB. Cualquiera de las formas mencionadas solo instala el paquete de la aplicación (.appx); no se instalan los certificados.

-   Usa la herramienta de implementación de aplicaciones de Windows 10 (WinAppDeployCmd). Obtén más información sobre la [herramienta WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   A partir de Windows 10, versión 1511, puedes usar el [Portal de dispositivos](#device_portal) para realizar implementaciones desde el explorador a un dispositivo móvil con Windows 10, versión 1511 o posterior. Usa la página **Aplicaciones** del Portal de dispositivos (&lt;IP&gt;/appmanager.md) para cargar un paquete de aplicación (.appx) e instalarlo en el dispositivo.

 

### Establecer directivas de grupo o claves del Registro

También puedes usar directivas de grupo o claves del Registro como alternativa para habilitar el dispositivo de escritorio con Windows 10 para el desarrollo.

**En la familia de dispositivos de escritorio**

Usa gpedit.msc para establecer las directivas de grupo necesarias para habilitar el dispositivo, a menos que tengas Windows 10 Home. Si tienes Windows 10 Home, tienes que usar regedit o los comandos de PowerShell para establecer directamente las claves del Registro para habilitar el dispositivo.

**Usar gpedit para habilitar el dispositivo**

1.  Ejecuta **Gpedit.msc**.
2.  Ve a Directiva de equipo Local &gt; Configuración del equipo &gt; Plantillas administrativas &gt; Componentes de Windows &gt; Implementación de paquetes de aplicaciones
3.  Para habilitar la instalación de prueba, edita las directivas para habilitar:

    -   **Permitir la instalación de todas las aplicaciones de confianza**

    - O bien

    Para habilitar el modo de desarrollador, edita las directivas para habilitar ambos:

    -   **Permitir la instalación de todas las aplicaciones de confianza**
    -   **Permite el desarrollo de aplicaciones de la Tienda de Windows y su instalación desde un entorno de desarrollo integrado (IDE)**

4.  Reinicia el equipo.

**Usar regedit para habilitar el dispositivo**

1.  Ejecuta **regedit**.
2.  Para habilitar la instalación de prueba, establece el valor de este DWORD en 1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - O bien

    Para habilitar el modo de desarrollador, establece los valores de este DWORD en 1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Usar PowerShell para habilitar el dispositivo**

1.  Ejecuta PowerShell con privilegios de administrador.
2.  Para habilitar la instalación de prueba, ejecuta el siguiente comando:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - O bien

    Para habilitar el modo de desarrollador, ejecuta el siguiente comando:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## Características del modo de desarrollador

Para cada familia de dispositivos, es posible que haya características para desarrolladores adicionales disponibles. Estas características solo están disponibles cuando el **Modo de desarrollador** está habilitado en el dispositivo, y pueden variar según la versión del sistema operativo.

En esta imagen se muestran las características para desarrolladores de la familia de dispositivos móviles de la versión 1511 de Windows 10.

![Opciones del modo de desarrollador para dispositivos móviles](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"></span>Detección de dispositivos y Device Portal

Para obtener más información sobre la detección de dispositivos y Device Portal, consulta [Windows Device Portal overview (Introducción a Windows Device Portal)](../debug-test-perf/device-portal.md).

Para obtener instrucciones específicas sobre la configuración del dispositivo, consulta:
- [Device Portal para HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal para IoT](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [Device Portal para dispositivos móviles](../debug-test-perf/device-portal-mobile.md)
- [Device Portal para Xbox](../debug-test-perf/device-portal-xbox.md)

### Informe de errores

Establece este valor para especificar el número de volcados de memoria que se guardan en el teléfono.

La recopilación de los volcados de memoria en el teléfono te ofrece acceso inmediato a información importante sobre bloqueos apenas se producen. Los volcados de memoria se recopilan únicamente para aplicaciones firmadas por el desarrollador. Puedes encontrar los volcados de memoria en la carpeta Documents\\Debug. Para obtener más información sobre los archivos de volcado de memoria, consulta [Using dump files (Usar archivos de volcado de memoria)](https://msdn.microsoft.com/library/d5zhxt22.aspx).

## Actualizar el dispositivo de Windows 8.1 a Windows 10

Si creas o realizas la instalación de prueba de aplicaciones en un dispositivo con Windows 8.1, tienes que instalar una licencia de desarrollador. Si actualizas tu dispositivo de Windows 8.1 a Windows 10, esta información permanece. Ejecuta el siguiente comando para quitar la información del dispositivo actualizado a Windows 10. Este paso no es necesario si actualizas directamente de Windows 8.1 a Windows 10, versión 1511 o posterior.

**Para anular el registro de una licencia de desarrollador**

1.  Ejecuta PowerShell con privilegios de administrador.
2.  Ejecutar este comando: **unregister-windowsdeveloperlicense**.

Después debes habilitar el dispositivo para el desarrollo, tal como se describe en este tema, para que puedas seguir desarrollando en este dispositivo. Si no lo haces, es posible que obtengas un error al depurar la aplicación o intentar crear un paquete para ella. Este es un ejemplo de este error:

Error : DEP0700 : Error en el registro de la operación.





<!--HONumber=Jun16_HO4-->



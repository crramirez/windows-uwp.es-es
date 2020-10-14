---
title: Características y depuración del modo de programador
description: Obtenga información sobre las características del modo de Desarrollador en Windows 10 y sobre los errores de instalación.
keywords: Introducción a Visual Studio con licencia de desarrollador, dispositivo con licencia de desarrollador habilitada
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8e42b9f35866f07a07122043ced803045bf99d6
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011959"
---
# <a name="developer-mode-features-and-debugging"></a>Características y depuración del modo de programador

Si solo está interesado en los aspectos básicos de la instalación del modo de Desarrollador en la aplicación, puede seguir las instrucciones que se describen en [habilitación del dispositivo para el desarrollo](enable-your-device-for-development.md) para comenzar. en este artículo se tratan las características avanzadas del modo de desarrollador, el modo de desarrollador de versiones anteriores de Windows 10 y los errores de depuración en la instalación en modo de desarrollador.

## <a name="additional-developer-mode-features"></a>Funciones adicionales del modo de desarrollador

Para cada familia de dispositivos, es posible que haya funciones para desarrolladores adicionales disponibles. Estas funciones solo están disponibles cuando el modo de desarrollador está habilitado en el dispositivo y pueden variar según la versión del sistema operativo.

Esta imagen muestra las funciones de desarrollo para Windows 10:

![Opciones del modo de desarrollador](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Portal de dispositivos

Para obtener más información sobre el Portal de dispositivos, consulta [Introducción al Portal de dispositivos Windows](/windows/uwp/debug-test-perf/device-portal.md).

Para obtener instrucciones específicas sobre la configuración del dispositivo, consulta:
- [Portal de dispositivos para dispositivos de escritorio](/windows/uwp/debug-test-perf/device-portal-desktop.md)
- [Portal de dispositivos para HoloLens](/windows/mixed-reality/using-the-windows-device-portal)
- [Portal de dispositivos para IoT](/windows/iot-core/manage-your-device/DevicePortal)
- [Portal de dispositivos para dispositivos móviles](/windows/uwp/debug-test-perf/device-portal-mobile.md)
- [Portal de dispositivos para Xbox](/windows/xbox-apps/device-portal-xbox.md)

Si se producen problemas al habilitar el modo de desarrollador o el portal de dispositivos, consulta el foro [Known Issues](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) (problemas conocidos) para buscar soluciones para estos problemas, o visita [Error al instalar el paquete de modo de desarrollador](#failure-to-install-developer-mode-package) para obtener más detalles y saber cuáles KB de WSUS deberían habilitarse para desbloquear el paquete de modo de desarrollador.

### <a name="sideload-apps"></a>Instalación de prueba de aplicaciones

> [!IMPORTANT]
> A partir de la actualización más reciente de Windows 10, esta configuración no será visible, ya que la instalación de prueba está habilitada de forma predeterminada. Si usa una versión anterior de Windows 10, las configuraciones predeterminadas solo le permitirán ejecutar aplicaciones de Microsoft Store. Deberá habilitar la instalación de prueba para instalar aplicaciones de fuentes distintas a Microsoft.

Por lo general, la opción de aplicaciones transferidas localmente la usan las empresas y centros docentes que necesitan instalar aplicaciones personalizadas en dispositivos administrados sin necesidad de ir Microsoft Store, o cualquier otra persona que necesite ejecutar aplicaciones de fuentes distintas a Microsoft. En este caso, la organización suele aplicar una directiva que deshabilita el ajuste *Aplicaciones para UWP*, como se mostró anteriormente en la imagen de la página de configuración. La organización también proporciona el certificado necesario y la ubicación de instalación para transferir aplicaciones localmente. Para más información, consulta los artículos de TechNet [Instalación de prueba de aplicaciones en Windows 10](/windows/deploy/sideload-apps-in-windows-10) y [Aspectos básicos de Microsoft Intune](/mem/intune/fundamentals/).

Información específica de la familia de dispositivos

-   En la familia de dispositivos de escritorio: puedes instalar un paquete de la aplicación (.appx) y cualquier certificado necesario para ejecutar la aplicación. Para hacerlo, ejecuta el script de Windows PowerShell que se creó con el paquete ("Add-AppDevPackage.ps1"). Para más información, consulta [Empaquetado de aplicaciones para UWP](/windows/msix/package/packaging-uwp-apps).

-   En la familia de dispositivos móviles: si el certificado necesario ya está instalado, puedes pulsar en el archivo para instalar cualquier archivo .appx que te hayan enviado por correo electrónico o que tengas en una tarjeta SD.


**Transferir localmente aplicaciones** es una opción más segura que el modo de desarrollador porque no permite instalar en el dispositivo aplicaciones que no tengan un certificado de confianza.

> [!NOTE]
> Aunque transfieras aplicaciones localmente, solo deberías instalar aplicaciones procedentes de orígenes de confianza. Cuando instalas una aplicación transferida localmente que no tiene el certificado de Microsoft Store, confirmas que tienes todos los derechos necesarios para transferir localmente la aplicación y que eres el único responsable de los daños que la instalación y la ejecución de esta aplicación puedan causar. Consulta la sección Windows &gt; Microsoft Store de esta [declaración de privacidad](https://privacy.microsoft.com/privacystatement).


### <a name="ssh"></a>SSH

Los servicios SSH se habilitan cuando habilitas la detección de dispositivos en el dispositivo.  Se usa cuando el dispositivo es un destino de implementación remota para aplicaciones para UWP.   Los nombres de los servicios son 'SSH Server Broker' y 'SSH Server Proxy'.

> [!NOTE]
> No se trata de la implementación OpenSSH de Microsoft, que puedes encontrar en [GitHub](https://github.com/PowerShell/Win32-OpenSSH).  

Para aprovechar las ventajas de los servicios SSH, puedes habilitar la detección de dispositivos para permitir el emparejamiento de PIN. Si tienes previsto ejecutar otro servicio SSH, puedes configurar esto en un puerto diferente o desactivar los servicios SSH del modo de desarrollador. Para desactivar los servicios SSH, desactiva la detección de dispositivos.  

El inicio de sesión SSH se realiza a través de la cuenta "DevToolsUser", que acepta una contraseña para autenticación.  Esta contraseña es el PIN que se muestra en el dispositivo después de presionar el botón "Emparejar" de detección de dispositivos y solo es válida mientras se muestra el PIN.  También se habilita un subsistema SFTP para administrar manualmente la carpeta DevelopmentFiles, donde se instalan las implementaciones de archivos sueltos desde Visual Studio.

#### <a name="caveats-for-ssh-usage"></a>Advertencias para el uso de SSH
El servidor SSH existente que se usa en Windows aún no cumple con el protocolo, por lo que el uso de un cliente SFTP o SSH puede requerir una configuración especial.  En particular, el subsistema SFTP se ejecuta en la versión 3 o anteriores, por lo que cualquier cliente que se conecte debe configurarse para esperar un servidor antiguo.  El servidor SSH en dispositivos más antiguos usa `ssh-dss` para la autenticación de clave pública, que está en desuso para OpenSSH.  Para conectarse a dichos dispositivos, el cliente SSH debe estar configurado manualmente para aceptar `ssh-dss`.  

### <a name="device-discovery"></a>Detección de dispositivo

Al habilitar la detección de dispositivos, permites que el dispositivo esté visible para otros dispositivos de la red a través de mDNS.  Esta función también te permite obtener el PIN SSH para el emparejamiento con este dispositivo si presionas el botón "Emparejamiento" una vez habilitada la detección de dispositivos.  Este aviso de PIN debe mostrarse en la pantalla para completar la primera implementación de Visual Studio destinada al dispositivo.  

![Emparejamiento de PIN](images/devmode-pc-pinpair.PNG)

Debes habilitar la detección de dispositivos solo si tienes previsto que el dispositivo sea un destino de implementación. Por ejemplo, si usas Device Portal para implementar una aplicación en un teléfono para probarla, debes habilitar la detección de dispositivos en el teléfono, pero no en el equipo de desarrollo.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Optimizaciones para Explorador de Windows, Escritorio remoto y PowerShell (solo equipos de escritorio)

 En la familia de dispositivos de escritorio, la página de configuración **Para desarrolladores** tiene accesos directos a la configuración que puedes usar para optimizar tu PC para realizar tareas de desarrollo. Para cada opción de configuración, puedes activar la casilla y hacer clic en **Aplicar** o hacer clic en el vínculo **Mostrar configuración** para abrir la página de configuración para esta opción.


## <a name="notes"></a>Notas
En versiones anteriores de Windows 10 Mobile, estaba presente una opción de volcados de memoria en el menú Configuración de desarrollador.  Esta opción se movió al [Portal de dispositivos](/windows/uwp/debug-test-perf/device-portal.md) para que se pueda usar de forma remota en lugar de solo a través de USB.  

Hay varias herramientas que puedes usar para implementar una aplicación desde un PC con Windows 10 a un dispositivo con Windows 10. Ambos dispositivos deben estar conectados a la misma subred de la red mediante una conexión con cable o inalámbrica, o bien deben estar conectados mediante USB. Ambas formas solo instalan el paquete de aplicación (.appx/.appxbundle); no instalan certificados.

-   Usa la herramienta de implementación de aplicaciones de Windows 10 (WinAppDeployCmd). Obtén más información sobre la [herramienta WinAppDeployCmd](/previous-versions/windows/apps/mt203806(v=vs.140)).
-   Puedes usar el [portal de dispositivos](/windows/uwp/debug-test-perf/device-portal.md) para realizar implementaciones desde el navegador a un dispositivo móvil con Windows 10, versión 1511 o posterior. Usa la página **[Aplicaciones](/windows/uwp/debug-test-perf/device-portal.md#apps-manager)** de Device Portal para cargar un paquete de la aplicación (.appx) e instalarlo en el dispositivo.

## <a name="failure-to-install-developer-mode-package"></a>Error al instalar el paquete de modo de desarrollador
En ocasiones, debido a problemas de red o administrativos, el modo de desarrollador no se instalará correctamente. Es necesario el paquete de modo de desarrollador para la implementación **remota** en este PC (con el Portal de dispositivos desde un navegador o la detección de dispositivos para habilitar SSH), pero no para el desarrollo local.  Incluso si experimentas estos problemas, puedes seguir implementando la aplicación localmente con Visual Studio, o desde este dispositivo a otro.

Consulta el foro de [problemas conocidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) para buscar soluciones para estos y otros problemas.

> [!NOTE]
> Si el modo de desarrollador no se instala correctamente, te animamos a enviar una solicitud de comentarios. En la aplicación **Centro de opiniones**, selecciona **Agregar nuevos comentarios** y elige la categoría **Plataforma para desarrolladores**  y la subcategoría **Modo de desarrollador**. Si envías comentarios, ayudarás a que Microsoft solucione el problema encontrado.

### <a name="failed-to-locate-the-package"></a>No se pudo ubicar el paquete

"No se pudo ubicar el paquete del modo de desarrollador en Windows Update. Código de error 0x80004005 Más información"   

Este error puede producirse debido a un problema de conectividad de red, configuración de empresa o que falte el paquete.

Corregir este problema:

1. Asegúrate de que el equipo esté conectado a Internet.
2. Si estás en un equipo unido a un dominio, habla con el administrador de red. El paquete de modo de desarrollador, al igual que todas las funciones a petición, está bloqueado de manera predeterminada en WSUS.
2.1. Para desbloquear el paquete de modo de desarrollador en las versiones actuales y anteriores, deben permitirse las siguientes KB en WSUS: 4016509, 3180030, 3197985  
3. Busca actualizaciones de Windows en Configuración > Actualizaciones y seguridad > Windows Update.
4. Comprueba que el paquete de modo de desarrollador de Windows esté presente en Configuración > Sistema > Aplicaciones y características > Administrar funciones opcionales > Agregar una característica. Si no aparece, Windows no puede encontrar el paquete correcto para el equipo.

Después de llevar a cabo cualquiera de los pasos anteriores, deshabilita y vuelve a habilitar el modo de desarrollador para comprobar la corrección.


### <a name="failed-to-install-the-package"></a>No se pudo instalar el paquete

"No se pudo instalar el paquete de modo de desarrollador. Código de error 0x80004005 Más información"

Este error puede producirse debido a incompatibilidades entre la compilación de Windows y el paquete de modo de desarrollador.

Corregir este problema:

1. Busca actualizaciones de Windows en Configuración > Actualizaciones y seguridad > Windows Update.
2. Reinicia el equipo para asegurarte de que se aplican a todas las actualizaciones.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Usar directivas de grupo o claves del Registro para habilitar un dispositivo

Para la mayoría de los desarrolladores, es recomendable usar la aplicación de configuración para habilitar el dispositivo para la depuración. En determinados escenarios, como las pruebas automatizadas, puedes usar otras formas de habilitar el dispositivo de escritorio Windows 10 para el desarrollo.  Ten en cuenta que estos pasos no habilitarán el servidor SSH ni permitirán que el dispositivo se destine a la implementación y depuración remota.

Puedes usar gpedit.msc para definir las directivas de grupo necesarias para habilitar el dispositivo, a menos que tengas Windows 10 Home. Si tienes Windows 10 Home, tienes que usar regedit o los comandos de PowerShell para establecer directamente las claves del Registro para habilitar el dispositivo.

**Usar gpedit para habilitar el dispositivo**

1.  Ejecuta **Gpedit.msc**.
2.  Ve a Directiva de equipo Local &gt; Configuración del equipo &gt; Plantillas administrativas &gt; Componentes de Windows &gt; Implementación de paquetes de aplicaciones
3.  Para habilitar la instalación de prueba, edita las directivas para habilitar:

    -   **Permitir la instalación de todas las aplicaciones de confianza**

    O

    Para habilitar el modo de desarrollador, edita las directivas para habilitar ambos:

    -   **Permitir la instalación de todas las aplicaciones de confianza**
    -   **Permitir el desarrollo de aplicaciones para UWP y su instalación desde un entorno de desarrollo integrado (IDE)**

4.  Reinicia el equipo.

**Usar regedit para habilitar el dispositivo**

1.  Ejecuta **regedit**.
2.  Para habilitar la instalación de prueba, establece el valor de este DWORD en 1:

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowAllTrustedApps`

    O

    Para habilitar el modo de desarrollador, establece los valores de este DWORD en 1:

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowDevelopmentWithoutDevLicense`

**Usar PowerShell para habilitar el dispositivo**

1.  Ejecuta PowerShell con privilegios de administrador.
2.  Para habilitar la instalación de prueba, ejecuta el siguiente comando:

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
    ```

    O

    Para habilitar el modo de desarrollador, ejecuta el siguiente comando:

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
    ```

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Actualizar el dispositivo de Windows 8.1 a Windows 10

Si creas o transfieres localmente aplicaciones en un dispositivo con Windows 8.1, tienes que instalar una licencia de desarrollador. Si actualizas tu dispositivo de Windows 8.1 a Windows 10, esta información permanece. Ejecuta el siguiente comando para quitar la información del dispositivo actualizado a Windows 10. Este paso no es necesario si actualizas directamente de Windows 8.1 a Windows 10, versión 1511 o posterior.

**Para anular el registro de una licencia de desarrollador**

1.  Ejecuta PowerShell con privilegios de administrador.
2.  Ejecuta este comando: `unregister-windowsdeveloperlicense`.

Después debes habilitar el dispositivo para el desarrollo, tal como se describe en este tema, para que puedas seguir desarrollando en este dispositivo. Si no lo haces, es posible que obtengas un error al depurar la aplicación o intentar crear un paquete para ella. Este es un ejemplo de este error:

Error: DEP0700: Error en el registro de la aplicación.
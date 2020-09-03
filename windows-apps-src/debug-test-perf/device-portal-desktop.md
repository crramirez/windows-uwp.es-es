---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Portal de dispositivos para dispositivos de escritorio Windows
description: Aprende cómo Windows Device Portal abre diagnóstico y automatización en el escritorio de Windows.
ms.date: 08/20/2020
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: e485fcb5d6ca6ecf8c19124c482492ddfb2c5233
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173609"
---
# <a name="device-portal-for-windows-desktop"></a>Portal de dispositivos para dispositivos de escritorio Windows

El Portal de dispositivos Windows es una herramienta de depuración que permite ver información de diagnóstico e interactuar con el equipo de escritorio a través de HTTP desde un explorador web. Para depurar otros dispositivos, consulte [Introducción al Portal de dispositivos Windows](device-portal.md).


Puedes usar Device Portal para lo siguiente:
- Ver y manipular una lista de procesos en ejecución
- Instalar, eliminar, iniciar y cerrar aplicaciones
- Cambiar de perfiles Wi-Fi, ver la intensidad de señal y ver la ipconfig
- Ver gráficos de uso de CPU, memoria, E/S, red y GPU en directo
- Recopilar volcados de proceso
- Recopilar seguimiento de ETW 
- Manipular el almacenamiento aislado de las aplicaciones transferidas localmente

## <a name="set-up-device-portal-on-windows-desktop"></a>Configuración del Portal de dispositivos en el escritorio de Windows

### <a name="turn-on-developer-mode"></a>Activar el modo de desarrollador

A partir de Windows 10, versión 1607, algunas de las características más recientes para equipos de escritorio solo están disponibles cuando se habilita el modo de desarrollador. Para obtener información sobre cómo habilitar el modo de desarrollador, consulta [Habilitar el dispositivo para el desarrollo](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> En ocasiones, debido a problemas de compatibilidad o de red, el modo de desarrollador no se instala correctamente en el dispositivo. Consulta la [sección correspondiente de Habilitar el dispositivo para el desarrollo](../get-started/enable-your-device-for-development.md#failure-to-install-developer-mode-package) con el fin de obtener ayuda para solucionar estos problemas.

### <a name="turn-on-device-portal"></a>Activación del Portal de dispositivos

Puedes habilitar el Portal de dispositivos en la sección **Para desarrolladores** de **Configuración**. Al habilitarlo, también debes crear un nombre de usuario y una contraseña correspondientes. No uses tu cuenta de Microsoft u otras credenciales de Windows.

![Sección Portal de dispositivos de la aplicación Configuración](images/device-portal/device-portal-desk-settings.png)

Una vez que el Portal de dispositivos esté habilitado, verás vínculos web en la parte inferior de la sección. Anota el número de puerto añadido al final de las direcciones URL mostradas: este número se genera aleatoriamente cuando se habilita el Portal de dispositivos, pero debe permanecer coherente entre los distintos reinicios del dispositivo de escritorio.

Estos vínculos ofrecen dos formas de conectarte al Portal de dispositivos: a través de la red local (incluida VPN) o a través del host local. Una vez conectado, debería tener un aspecto similar al siguiente:

![Portal de dispositivos](images/device-portal/device-portal-example.png)


### <a name="turn-off-device-portal"></a>Desactivación del Portal de dispositivos

Puede deshabilitar el Portal de dispositivos en la sección **Para desarrolladores** de **Configuración**.

### <a name="connect-to-device-portal"></a>Conectarte al Portal de dispositivos

Para conectarse a través del host local, abra una ventana del explorador y escriba la dirección que se muestra aquí para el tipo de conexión que esté usando.

* Localhost: `http://127.0.0.1:<PORT>` o `http://localhost:<PORT>`
* Red local: `https://<IP address of the desktop>:<PORT>`

Se necesita HTTPS para la autenticación y la comunicación segura.

Si usa el Portal de dispositivos en un entorno protegido, como un laboratorio de pruebas, donde confía en todos los usuarios de la red local, no tiene información personal sobre el dispositivo y tiene requisitos únicos, puede deshabilitar la opción Autenticación. Esto permite la comunicación sin cifrar y que cualquier persona que tenga la dirección IP del equipo se conecte a él y lo controle.

## <a name="device-portal-content-on-windows-desktop"></a>Contenido del Portal de dispositivos en un dispositivo de escritorio Windows

El Portal de dispositivos en un dispositivo de escritorio Windows mostrará el conjunto de páginas que se describen en [Introducción al Portal de dispositivos Windows](device-portal.md).

- Administrador de aplicaciones
- Xbox Live
- Explorador de archivos
- Procesos en ejecución
- Rendimiento
- Depuración
- Registro de ETW (Seguimiento de eventos para Windows)
- Seguimiento del rendimiento
- Administrador de dispositivos
- Bluetooth
- Redes
- Datos de bloqueo
- Características
- Mixed Reality
- Depurador de instalación en streaming
- Location
- Borrador

## <a name="using-device-portal-for-windows-desktop-to-test-and-debug-msix-apps"></a>Uso del Portal de dispositivos para un dispositivo de escritorio Windows con la finalidad de probar y depurar aplicaciones MSIX


> [!VIDEO https://www.youtube.com/embed/PdgXeOMt4hk]


## <a name="more-device-portal-options"></a>Más opciones del Portal de dispositivos

### <a name="registry-based-configuration-for-device-portal"></a>Configuración basada en el Registro para el Portal de dispositivos

Si quieres seleccionar los números de puerto para Device Portal (como 80 y 443), puedes establecer las siguientes claves de registro:

- En `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: DWORD necesario. Establécelo en 0 para conservar los números de puerto que hayas elegido.
    - `HttpPort`: DWORD necesario. Contiene el número de puerto que escuchará Device Portal para las conexiones HTTP.    
    - `HttpsPort`: DWORD necesario. Contiene el número de puerto que escuchará Device Portal para las conexiones HTTPS.
    
En la misma ruta de acceso de regkey, también puedes desactivar el requisito de autenticación:
- `UseDefaultAuthorizer` - `0` para deshabilitado y `1` para habilitado.  
    - Esto controla tanto el requisito de autenticación básica para cada conexión como el redireccionamiento de HTTP a HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Opciones de línea de comandos para el Portal de dispositivos
Desde un símbolo del sistema administrativo, puedes habilitar y configurar componentes del Portal de dispositivos. Para ver el conjunto más reciente de comandos admitidos en tu compilación, puedes ejecutar `webmanagement /?`

- `sc start webmanagement` o `sc stop webmanagement` 
    - Activa o desactiva el servicio. Esto también requiere que el modo de desarrollador esté habilitado. 
- `-Credentials <username> <password>` 
    - Establece un nombre de usuario y una contraseña para el Portal de dispositivos. El nombre de usuario debe cumplir con los estándares de autenticación básica, por lo que no puede contener dos puntos (:) y debe estar compuesto por caracteres ASCII estándar; por ejemplo, [a-z, A-Z, 0-9], ya que los exploradores no analizan el conjunto de caracteres completo de una forma estándar.  
- `-DeleteSSL` 
    - Esto restablece la memoria caché de certificado SSL usada para las conexiones HTTPS. Si se producen errores de conexión de TLS que no se pueden ignorar (en lugar de la advertencia de certificado esperada), esta opción puede solucionar el problema automáticamente. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Consulta [Aprovisionar el Portal de dispositivos con un certificado SSL personalizado](./device-portal-ssl.md) para obtener detalles.  
    - Esto te permite instalar tu propio certificado SSL para corregir la página de advertencia de SSL que se ve normalmente en el Portal de dispositivos. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Ejecuta una versión independiente del Portal de dispositivos con una configuración específica y mensajes de depuración visibles. Esto resulta muy útil para crear un [complemento empaquetado](./device-portal-plugin.md). 
    - Consulta el [artículo de MSDN Magazine](/archive/msdn-magazine/2017/october/windows-device-portal-write-a-windows-device-portal-packaged-plug-in) para obtener más información acerca de cómo ejecutar esto como un sistema con el fin de probar completamente el complemento empaquetado.

## <a name="troubleshooting"></a>Solución de problemas

A continuación se muestran algunos errores comunes que pueden surgir al configurar el Portal de dispositivos.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch devuelve un número de actualizaciones no válido (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Puedes obtener este error al intentar instalar los paquetes para desarrolladores en una versión preliminar de Windows 10. Estos paquetes de características a petición (FoD) se hospedan en Windows Update y su descarga en versiones preliminares requiere tu participación en la distribución de paquetes piloto. Si la instalación no participa en la distribución de paquetes piloto para la combinación correcta de compilación y anillo, la carga no se podrá descargar. Comprueba lo siguiente:

1. Ve a **Configuración > Actualización y seguridad > Programa Windows Insider** y confirma que la información de la sección **Cuenta de Windows Insider** sea correcta. Si no ves esa sección, selecciona **Vincular una cuenta de Windows Insider**, agrega tu cuenta de correo electrónico y confirma que aparece en el encabezado **Cuenta de Windows Insider** (puede que tengas que seleccionar **Vincular una cuenta de Windows Insider** una segunda vez para vincular una cuenta recién agregada).
 
2. En **¿Qué tipo de contenido te gustaría recibir?** , comprueba que la opción **Active development of Windows** (Desarrollo activo de Windows) esté seleccionada.
 
3. En **¿Con qué frecuencia deseas recibir nuevas versiones?** , asegúrate de que la opción **Windows Insider: Rápida** esté seleccionada.
 
4. Ahora podrás las instalar las FoD. Si has confirmado que estás en Windows Insider: Rápida y sigues sin poder instalar FoD, proporciona tus comentarios y adjunta los archivos de registro de **C:\Windows\Logs\CBS**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: Error de OpenService 1060: El servicio especificado no existe como servicio instalado.

Si los paquetes de desarrollo no están instalados, puede que veas este error. Sin los paquetes de desarrollo, no hay servicio de administración web. Vuelve a intentar instalar los paquetes de desarrollo.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>CBS cannot start download because the system is on metered network (CBS_E_METERED_NETWORK) (CBS no puede iniciar la descarga porque el sistema está en una red de uso medido [CBS_E_METERED_NETWORK])

Si usas una conexión a Internet de uso medido, puede que recibas este error. No podrás descargar los paquetes de desarrollo en una conexión de uso medido.

## <a name="see-also"></a>Vea también

* [Introducción al Portal de dispositivos Windows](device-portal.md)
* [Referencia de API principal de Device Portal](./device-portal-api-core.md)
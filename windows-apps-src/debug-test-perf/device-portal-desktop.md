---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Portal de dispositivos para dispositivos de escritorio Windows
description: Aprende cómo Windows Device Portal abre el diagnóstico y la automatización en el escritorio de Windows.
ms.author: pafarley
ms.date: 03/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 146cce82275047c112d70cfb3d022eab723f49e6
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "3665860"
---
# <a name="device-portal-for-windows-desktop"></a>Portal de dispositivos para dispositivos de escritorio Windows



Portal de dispositivos Windows permite ver información de diagnóstico e interactuar con el equipo de escritorio a través de HTTP desde una ventana del navegador. Puedes usar el Portal de dispositivos para lo siguiente:
- Ver y manipular una lista de procesos en ejecución
- Instalar, eliminar, iniciar y cerrar aplicaciones
- Cambiar de perfiles Wi-Fi, ver la intensidad de señal y ver la ipconfig
- Ver gráficos de uso de CPU, memoria, E/S, red y GPU en directo
- Recopilar volcados de proceso
- Recopilar seguimiento de ETW 
- Manipular el almacenamiento aislado de las aplicaciones transferidas localmente

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurar el Portal de dispositivos en un dispositivo de escritorio Windows

### <a name="turn-on-developer-mode"></a>Activar el modo de desarrollador

A partir de Windows 10, versión 1607, algunas de las características más recientes para equipos de escritorio solo están disponibles cuando se habilita el modo de desarrollador. Para obtener información sobre cómo habilitar el modo de desarrollador, consulta [Habilitar el dispositivo para el desarrollo](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> En ocasiones, debido a problemas de compatibilidad o de red, el modo de desarrollador no se instala correctamente en el dispositivo. Consulta la [sección correspondiente de Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package) con el fin de obtener ayuda para solucionar estos problemas.

### <a name="turn-on-device-portal"></a>Activar el Portal de dispositivos

Puedes habilitar el Portal de dispositivos en la sección **Para desarrolladores** de **Configuración**. Al habilitarlo, también debes crear un nombre de usuario y una contraseña para correspondientes. No uses tu cuenta de Microsoft ni otras credenciales de Windows. 

![Sección Portal de dispositivos de la aplicación Configuración](images/device-portal/device-portal-desk-settings.png) 

Una vez que el Portal de dispositivos esté habilitado, verás vínculos web en la parte inferior de la sección. Anota el número de puerto añadido al final de las direcciones URL mostradas: este número se genera aleatoriamente cuando se habilita el Portal de dispositivos, pero debe permanecer coherente entre los distintos reinicios del dispositivo de escritorio. Si deseas establecer los números de puerto manualmente para que sean permanentes, consulta [Configuración de números de puerto](device-portal-desktop.md#setting-port-numbers).

Estos vínculos ofrecen dos formas de conectarte al Portal de dispositivos: a través de la red local (incluida VPN) o a través del host local.

### <a name="connect-to-device-portal"></a>Conectarte al Portal de dispositivos

Para conectarte a través del host local, abre una ventana del navegador y escribe la dirección que se muestra aquí para el tipo de conexión que estés usando.

* Host local: `http://127.0.0.1:<PORT>` o `http://localhost:<PORT>`
* Red local: `https://<IP address of the desktop>:<PORT>`

Se necesita HTTPS para la autenticación y la comunicación segura.

Si usas el Portal de dispositivos en un entorno protegido, como un laboratorio de pruebas, en el que confíes en todos los usuarios de la red local, no tengas información personal en el dispositivo y existan requisitos únicos, puedes deshabilitar la opción Autenticación. Esto permite la comunicación sin cifrar y que cualquier persona que tenga la dirección IP del equipo se conecte a él y lo controle.

## <a name="device-portal-content-on-windows-desktop"></a>Contenido del Portal de dispositivos en un dispositivo de escritorio Windows

El Portal de dispositivos en un dispositivo de escritorio Windows proporciona el conjunto estándar de páginas. Para obtener descripciones detalladas de estas, consulta [Introducción al Portal de dispositivos Windows](device-portal.md).

- Administrador de aplicaciones
- Explorador de archivos
- Procesos en ejecución
- Rendimiento
- Depuración
- Seguimiento de eventos para Windows (ETW)
- Seguimiento de rendimiento
- Administrador de dispositivos
- Redes
- Datos de bloqueos
- Características
- Realidad mixta
- Depurador de instalación en streaming
- Ubicación
- Borrador

## <a name="more-device-portal-options"></a>Más opciones del Portal de dispositivos
### <a name="registry-based-configuration-for-device-portal"></a>Configuración basada en el Registro para el Portal de dispositivos

Si quieres seleccionar los números de puerto para el Portal de dispositivos (como 80 y 443), puedes establecer las siguientes regkeys:

- En `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: Un DWORD necesario. Establécelo en 0 para conservar los números de puerto que hayas elegido.
    - `HttpPort`: Un DWORD necesario. Contiene el número de puerto que escuchará el Portal de dispositivos para las conexiones HTTP activadas.    
    - `HttpsPort`: Un DWORD necesario. Contiene el número de puerto que escuchará el Portal de dispositivos para las conexiones HTTPS activadas.
    
En la misma ruta de acceso de regkey, también puedes desactivar el requisito de autenticación:
- `UseDefaultAuthorizer` - `0` para deshabilitado, `1` para habilitado.  
    - Esto controla tanto el requisito de autenticación básica para cada conexión como el redireccionamiento de HTTP a HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Opciones de línea de comandos para el Portal de dispositivos
Desde un símbolo del sistema administrativo, puede habilitar y configurar componentes del Portal de dispositivos. Para ver el conjunto más reciente de comandos admitidos en tu compilación, puedes ejecutar `webmanagement /?`

- `sc start webmanagement` o bien `sc stop webmanagement` 
    - Activa o desactiva el servicio. Esto también requiere que el modo de desarrollador esté habilitado. 
- `-Credentials <username> <password>` 
    - Establece un nombre de usuario y una contraseña para el Portal de dispositivos. El nombre de usuario deben cumplir con los estándares de autenticación básica, por lo que no puede contener dos puntos (:) debe estar compuesto por caracteres ASCII estándar [a-z, A-Z, 0-9], ya que los exploradores no analizan el conjunto de caracteres completo de una forma estándar.  
- `-DeleteSSL` 
    - Esto restablece la memoria caché de certificado SSL usada para las conexiones HTTPS. Si se producen errores de conexión de TLS que no se pueden ignorar (en lugar de la advertencia de certificado esperada), esta opción puede solucionar el problema automáticamente. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Consulta [Aprovisionar el Portal de dispositivos con un certificado SSL personalizado](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl) para obtener detalles.  
    - Esto te permite instalar tu propio certificado SSL para corregir la página de advertencia de SSL que se ve normalmente en el Portal de dispositivos. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Ejecuta una versión independiente del Portal de dispositivos con una configuración específica y mensajes de depuración visibles. Esto resulta muy útil para crear un [complemento empaquetado](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Consulta el [artículo de MSDN Magazine](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx) para obtener más información acerca de cómo se ejecutar esto como un sistema con el fin de probar completamente el complemento empaquetado.

## <a name="see-also"></a>Consulta también

* [Introducción al Portal de dispositivos Windows](device-portal.md)
* [Referencia de API principales del Portal de dispositivos](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)

---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal para escritorio
description: "Aprende cómo Windows Device Portal abre el diagnóstico y la automatización en el escritorio de Windows."
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32155bfbb676a5f79dd4b1629f0a88368da36828
ms.sourcegitcommit: 0fa9ae00117e8e6b04ed38956e605bb74c1261c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2017
---
# <a name="device-portal-for-desktop"></a>Device Portal para equipos de escritorio

A partir de Windows 10, versión 1607, hay funciones para desarrollador adicionales disponibles para escritorio. Estas funciones están disponibles únicamente si el modo de desarrollador está habilitado.

Para obtener información sobre cómo habilitar el modo de desarrollador, consulta [Enable your device for development (Habilitar el dispositivo para el desarrollo)](../get-started/enable-your-device-for-development.md).

Device Portal permite ver la información de diagnóstico e interactuar con el escritorio a través de HTTP desde el navegador. Puedes usar Device Portal para lo siguiente:
- Ver y manipular una lista de procesos en ejecución
- Instalar, eliminar, iniciar y cerrar aplicaciones
- Cambiar de perfiles Wi-Fi, ver la intensidad de señal y ver la ipconfig
- Ver gráficos de uso de CPU, memoria, E/S, red y GPU en directo
- Recopilar volcados de proceso
- Recopilar seguimiento de ETW 
- Manipular el almacenamiento aislado de las aplicaciones transferidas localmente

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurar Device Portal en escritorio de Windows

### <a name="turn-on-device-portal"></a>Activar Device Portal

En el menú **Configuración del desarrollador**, con el modo de desarrollador habilitado, puedes habilitar Device Portal.  

Al habilitar Device Portal, también debes crear un nombre de usuario y una contraseña para Device Portal. No uses tu cuenta de Microsoft u otras credenciales de Windows.  

Una vez habilitado Device Portal, verás vínculos a este en la parte inferior de la sección **Configuración**. Comprueba el número de puerto aplicado al final de la dirección URL: este número de puerto se genera aleatoriamente cuando Device Portal está habilitado, pero debe permanecer coherente entre los reinicios del escritorio. Si te gustaría establecer los números de puerto manualmente para que sean permanentes, consulta [Establecer números de puerto](device-portal-desktop.md#setting-port-numbers).

Puedes elegir entre dos formas de conectarte a Device Portal: host local y a través de la red local (incluye VPN).

**Para conectarte al Portal de dispositivos**

1. En el explorador, escribe la dirección que se muestra aquí para el tipo de conexión que estás usando.

    - Host local: `http://127.0.0.1:PORT` o `http://localhost:PORT`

    Usa esta dirección para ver Device Portal de forma local.
    
    - Red local: `https://<The IP address of the desktop>:PORT`

    Usa esta dirección para conectarte a través de una red local.

Se necesita HTTPS para la autenticación y la comunicación segura.

Si usas el Portal de dispositivos en un entorno protegido, como un laboratorio de pruebas, en el que confías en todos los usuarios de la red local, no tienes información personal en el dispositivo y existen requisitos únicos, puedes deshabilitar la autenticación. Esto permite la comunicación sin cifrar y que cualquier persona que tenga la dirección IP del equipo lo controle.

## <a name="device-portal-pages"></a>Páginas de Device Portal

Device Portal en el escritorio proporciona el conjunto estándar de páginas. Para obtener descripciones detalladas, consulta [Windows Device Portal overview (Introducción a Windows Device Portal)](device-portal.md).

- Aplicaciones
- Procesos
- Rendimiento
- Depuración
- Seguimiento de eventos para Windows (ETW)
- Seguimiento del rendimiento
- Dispositivos
- Redes
- Explorador de archivos de la aplicación 

## <a name="setting-port-numbers"></a>Configuración de números de puerto

Si quieres seleccionar los números de puerto para Device Portal (como 80 y 443), puedes establecer las siguientes claves de registro:

- En HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - UseDynamicPorts: un DWORD necesario. Establécelo en 0 para conservar los números de puerto que hayas elegido.
    - HttpPort: un DWORD necesario. Contiene el número de puerto que escuchará Device Portal para las conexiones HTTP.  
    - HttpsPort: un DWORD necesario. Contiene el número de puerto que escuchará Device Portal para las conexiones HTTPS.

## <a name="failure-to-install-developer-mode-package"></a>Error al instalar el paquete de modo de desarrollador
En ocasiones, debido a problemas de compatibilidad o de red, el modo de desarrollador no se instalará correctamente. Es necesario el paquete de modo de desarrollador para la implementación **remota** en el PC, con Device Portal desde un navegador o la detección de dispositivos para habilitar SSH, pero no para el desarrollo local.  Aunque puedes encontrar estos problemas, puedes seguir implementando la aplicación localmente con Visual Studio. 

Consulta el foro de [problemas conocidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) y la [página de modo de desarrollador](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package) para buscar soluciones para estos problemas y mucho más. 


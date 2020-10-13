---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal para dispositivos móviles
description: Obtén información sobre cómo Windows Device Portal te permite configurar y administrar el dispositivo móvil de forma remota.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 1f0a91dd0370b8eda8763b63034d7c3ffaa1acd5
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762997"
---
# <a name="device-portal-for-mobile"></a>Device Portal para dispositivos móviles

A partir de Windows 10, versión 1511, hay funciones para desarrollador adicionales disponibles para la familia de dispositivos móviles. Estas funciones están disponibles únicamente si el modo de desarrollador está habilitado en el dispositivo.

Para obtener información sobre cómo habilitar el modo de desarrollador, consulta [Habilitar el dispositivo para el desarrollo](../get-started/enable-your-device-for-development.md).

![Captura de pantalla de las opciones Detección de dispositivo y Portal de dispositivos.](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>Configurar Device Portal en Windows Phone

### <a name="turn-on-device-discovery-and-pairing"></a>Activar la detección de dispositivos y el emparejamiento

Para conectarte al Portal de dispositivos, tienes que habilitar la detección de dispositivos y el Portal de dispositivos en la configuración de tu teléfono. Esto te permite emparejar el teléfono con un equipo u otro dispositivo Windows 10. Ambos dispositivos deben estar conectados a la misma subred de la red mediante una conexión con cable o inalámbrica, o bien deben estar conectados mediante USB.

La primera vez que te conectes al Portal de dispositivos, se te solicitará un código de seguridad de seis caracteres (distingue mayúsculas de minúsculas). Esto garantiza el acceso al teléfono y te mantiene a salvo de los atacantes. Presiona el botón Emparejar del teléfono para generar y mostrar el código y luego escribe los seis caracteres en el cuadro de texto del navegador.

![Configuración de la detección de dispositivos en el modo de desarrollador](images/device-portal/mob-dev-mode-pairing.png)

Puedes elegir entre tres formas de conectarte a Device Portal: USB, host local y a través de la red local (incluye VPN y tethering).

**Para conectarte al Portal de dispositivos**

1. En el explorador, escribe la dirección que se muestra aquí para el tipo de conexión que estás usando.

    - USB: `http://127.0.0.1:10080`

    Usa esta dirección cuando el teléfono esté conectado a un equipo a través de una conexión USB. Ambos dispositivos deben tener Windows 10, versión 1511 o posterior.
    
    - Host local:`http://127.0.0.1`

    Usa esta dirección para ver Device Portal localmente en el teléfono, mediante Microsoft Edge para Windows 10 Mobile.
    
    - Red local: `https://<The IP address or hostname of the phone>`

    Usa esta dirección para conectarte a través de una red local.

    La dirección IP del teléfono se muestra en la configuración de Device Portal en el teléfono. Se necesita HTTPS para la autenticación y la comunicación segura. El nombre de host (que se puede editar en Configuración > Sistema > Acerca de) también puede utilizarse para acceder a Portal de dispositivosl en la red local (por ejemplo, http://Phone360), lo que resulta útil para los dispositivos que pueden cambiar con frecuencia de redes o direcciones IP o que deben compartirse. 

2. Presiona el botón Emparejar en el teléfono para generar y mostrar el código de seguridad necesario.

3. Escribe el código de seguridad de seis caracteres en el cuadro de contraseña del Portal de dispositivos en el navegador.

4. (Opcional) Activa la casilla Recordar mi equipo en el navegador para recordar este emparejamiento en el futuro.

A continuación se muestra la sección de Portal de dispositivos de la página de configuración para desarrollador en Windows Phone.

![Captura de pantalla de la página de configuración de Portal de dispositivos desde un Windows Phone.](images/device-portal/mob-dev-mode-portal.png)

Si usas el Portal de dispositivos en un entorno protegido, como un laboratorio de pruebas, en el que confías en todos los usuarios de la red local, no tienes información personal en el dispositivo y existen requisitos únicos, puedes deshabilitar la autenticación. Esto permite la comunicación sin cifrar y que cualquier persona que tenga la dirección IP del teléfono lo controle.

## <a name="tool-notes"></a>Notas de la herramienta

## <a name="device-portal-pages"></a>Páginas de Device Portal
### <a name="processes"></a>Procesos

La capacidad de terminar procesos arbitrarios no está incluida en Windows Mobile Device Portal. 

Device Portal en dispositivos móviles proporciona el conjunto estándar de páginas. Para obtener descripciones detalladas, consulta [Introducción a Windows Device Portal](device-portal.md).

- Administrador de aplicaciones
- Explorador de archivos de la aplicación (explorador de almacenamiento aislado)
- Procesos
- Gráficos de rendimiento
- Seguimiento de eventos para Windows (ETW)
- Seguimiento del rendimiento (WPR) 
- Dispositivos
- Funciones de red

## <a name="see-also"></a>Consulta también

* [Introducción al Portal de dispositivos Windows](device-portal.md)
* [Referencia de API principal del Portal de dispositivos](./device-portal-api-core.md)
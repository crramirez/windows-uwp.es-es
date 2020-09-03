---
title: Implementar una aplicación mediante el registro de archivos dinámico
description: En esta guía se muestra cómo usar el diseño de archivos dinámico para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlas.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, portal de dispositivos, administrador de aplicaciones, implementación, SDK
ms.localizationpriority: medium
ms.openlocfilehash: 0fd5bf6be691974d956de0c71f4a1d11aa1a229f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166099"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implementar una aplicación mediante el registro de archivos dinámico 

En esta guía se muestra cómo usar el diseño de archivos dinámico para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlas. El registro de diseños de archivo dinámicos permite a los desarrolladores validar rápidamente sus aplicaciones sin necesidad de empaquetar e instalar las aplicaciones. 

## <a name="what-is-a-loose-file-layout"></a>¿Qué es un diseño de archivo dinámico?

El diseño de archivos dinámicos simplemente es el acto de colocar el contenido de la aplicación en una carpeta en lugar de pasar por el proceso de empaquetado. El contenido del paquete está disponible "vagamente" en una carpeta y sin empaquetar. 

> [!WARNING]
> El objetivo del registro de diseño de archivos dinámicos es que los desarrolladores y diseñadores puedan validar rápidamente sus aplicaciones durante el desarrollo activo. Este enfoque no debe usarse para realizar pruebas o distribuir paquetes piloto de la aplicación. Se recomienda realizar la validación final en una aplicación empaquetada que esté firmada con un certificado de confianza. 

## <a name="advantages-of-loose-file-registration"></a>Ventajas del registro de archivos dinámicos

- **Validación rápida**: dado que los archivos de la aplicación ya están desempaquetados, los usuarios pueden registrar rápidamente el diseño de archivos dinámicos e iniciar la aplicación. Al igual que una aplicación normal, el usuario podrá usar la aplicación tal y como estaba diseñada. 
- **Distribución sencilla en la red**: si los archivos dinámicos se encuentran en un recurso compartido de red en lugar de en una unidad local, los desarrolladores pueden enviar la ubicación del recurso compartido de red a otros usuarios que tengan acceso a la red, así como registrar el diseño de archivos dinámicos y ejecutar la aplicación. Esto permite que varios usuarios validen la aplicación simultáneamente. 
- **Colaboración**: el registro de archivos dinámicos permite a los desarrolladores y diseñadores seguir trabajando en recursos visuales mientras se registra la aplicación. Los usuarios verán estos cambios cuando inicien la aplicación. Ten en cuenta que solo puedes cambiar los recursos estáticos de esta manera. Si necesitas modificar código o contenido creado de forma dinámica, debes volver a compilar la aplicación.

## <a name="how-to-register-a-loose-file-layout"></a>Cómo registrar un diseño de archivos dinámicos

Windows proporciona varias herramientas de desarrollo para registrar diseños de archivos dinámicos en dispositivos locales y remotos. Puedes elegir entre `WinDeployAppCmd` (herramienta Windows SDK), Portal de dispositivos Windows, PowerShell y [Visual Studio](./deploying-and-debugging-uwp-apps.md#register-layout-from-network). A continuación, veremos cómo registrar archivos dinámicos con estas herramientas. Pero primero asegúrate de que tienes la siguiente configuración:

- Los dispositivos deben contar con Windows 10 Creators Update, compilación 14965 o posterior.
- Tendrás que habilitar el [modo de desarrollador](../get-started/enable-your-device-for-development.md) y la [detección de dispositivos](../get-started/enable-your-device-for-development.md#device-discovery) en todos los dispositivos.

> [!IMPORTANT]
> El registro de archivos dinámicos solo está disponible en los dispositivos que admiten el protocolo de recurso compartido de red (SMB): escritorio y Xbox. 

### <a name="register-with-windeployappcmd"></a>Registro con WinDeployAppCmd

Si usa las herramientas de SDK correspondientes a Windows 10 Creators Update, compilación 14965 o posterior, puedes usar el comando `WinDeployAppCmd` en un símbolo del sistema.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Ruta de acceso de red**: ruta de acceso a los archivos dinámicos de la aplicación.

**Dirección IP**: dirección IP del equipo de destino.

**PIN de la máquina de destino**: PIN, en caso de que sea necesario, para establecer una conexión con el dispositivo de destino. Se te pedirá que vuelvas a intentarlo con la opción `-pin` si se requiere autenticación. Consulta [Detección de dispositivo](../get-started/enable-your-device-for-development.md#device-discovery) para obtener información sobre cómo obtener un PIN.

### <a name="windows-device-portal"></a>Portal de dispositivos Windows

El Portal de dispositivos Windows está disponible en todos los dispositivos Windows 10 y los desarrolladores lo usan para probar y validar su trabajo. Está destinado a todos los públicos de la comunidad de desarrolladores con sus puntos de conexión de REST y experiencia de usuario del explorador. Para obtener más información sobre el Portal de dispositivos, consulta [Introducción al Portal de dispositivos Windows](device-portal.md).

Para registrar el diseño de archivos dinámicos en el Portal de dispositivos, sigue estos pasos.

1. Para conectarte al portal de dispositivos, sigue los pasos de la sección **Configuración** de [Introducción al Portal de dispositivos Windows](device-portal.md).
1. En la pestaña Administrador de aplicaciones, selecciona **Register from Network Share** (Registrar desde el recurso compartido de red).
1. Escribe la ruta de acceso del recurso compartido de red para el diseño de archivos dinámicos. 
1. Si el dispositivo host no tiene acceso al recurso compartido de red, se te pedirá que escribas las credenciales necesarias.
1. Una vez completado el registro, puedes iniciar la aplicación.

En la página Administrador de aplicaciones del Portal de dispositivos, también puedes registrar diseños de archivos dinámicos opcionales para la aplicación principal. Para ello, selecciona la casilla **I want to specify optional packages** (Deseo especificar paquetes opcionales) y, después, especifica las rutas de acceso del recurso compartido de red de las aplicaciones opcionales. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell también permite registrar diseños de archivos dinámicos, pero solo en el dispositivo local. Si necesitas registrar un diseño en un dispositivo remoto, tendrás que usar uno de los otros métodos. 

Para registrar el diseño de archivos dinámicos, inicia PowerShell y escribe lo siguiente.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Solucionar problemas

### <a name="mapped-network-drives"></a>Unidades de red asignadas
Actualmente, las unidades de red asignadas no son compatibles con los registros de archivos dinámicos. Consulta la unidad asignada con la ruta de acceso del recurso compartido de red completa.

### <a name="registration-failure"></a>Error de registro
El dispositivo en el que se realiza el registro debe tener acceso al diseño del archivo. Si el diseño de archivo está hospedado en un recurso compartido de red, asegúrate de que el dispositivo tiene acceso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Las modificaciones de los recursos visuales no se están cargando en la aplicación 
La aplicación cargará sus recursos visuales en el momento del inicio. Si se realizaron modificaciones en los recursos visuales después de iniciar la aplicación, debes volver a iniciarla para ver los cambios más recientes.
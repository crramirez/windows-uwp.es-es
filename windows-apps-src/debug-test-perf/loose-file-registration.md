---
author: c-don
title: Implementar una aplicación a través del registro de archivos dinámico
description: Esta guía muestra cómo usar el diseño de archivos sueltos para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlos.
ms.author: cdon
ms.date: 6/1/2018
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos, Administrador de aplicaciones, implementación, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 16dc7c3d8182e249134be941d466574cddc36157
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6648528"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implementar una aplicación a través del registro de archivos dinámico 

Esta guía muestra cómo usar el diseño de archivos sueltos para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlos. Registro de diseños de archivos sueltos permite a los desarrolladores validar rápidamente sus aplicaciones sin necesidad de empaquetar e instalar las aplicaciones. 

## <a name="what-is-a-loose-file-layout"></a>¿Qué es un diseño de archivos sueltos?

Diseño de archivos sueltos es simplemente el acto de colocar contenido de la aplicación en una carpeta en lugar de pasar por el proceso de empaquetado. El contenido del paquete es "imprecisa" disponible en una carpeta y no empaquetada. 

> [!WARNING]
> Registro de diseño de archivos sueltos es para que los desarrolladores y diseñadores validar rápidamente sus aplicaciones durante el desarrollo activo. Este enfoque no debe usarse para "dogfood" o la aplicación de piloto. Te recomendamos que la validación final debe realizarse en una aplicación empaquetada que se inicie con un certificado de confianza. 

## <a name="advantages-of-loose-file-registration"></a>Ventajas de registro de archivos dinámico

- **Validación rápida** : porque los archivos de la aplicación ya se descomprimen, los usuarios pueden registrar el diseño de archivos sueltos e iniciar la aplicación rápidamente. Al igual que una aplicación normal, el usuario podrá usar la aplicación, tal como se diseñó. 
- **En la red de facilitar su distribución** - si se encuentran los archivos sueltos en un recurso compartido de red en lugar de una unidad local, los desarrolladores pueden enviar la ubicación de recurso compartido de red a otros usuarios que tienen acceso a la red, y pueden registrar el diseño de archivos sueltos y ejecutar la aplicación. Esto permite que varios usuarios validar la aplicación al mismo tiempo. 
- **Colaboración** - registro de archivos sueltos permite a los desarrolladores y diseñadores y seguir trabajando activos visuales mientras la aplicación esté registrada. Los usuarios verán estos cambios cuando inicia la aplicación. Ten en cuenta que solo se pueden cambiar activos estáticos de esta manera. Si es necesario modificar cualquier código o el contenido creado dinámicamente, debe volver a compilar la aplicación.

## <a name="how-to-register-a-loose-file-layout"></a>Cómo registrar un diseño de archivos sueltos

Windows proporciona varias herramientas de desarrollo para registrar los diseños de archivos sueltos en dispositivos locales y remotos. Puedes elegir entre `WinDeployAppCmd` (herramienta de SDK de Windows), Windows Device Portal, PowerShell y [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). A continuación analizaremos cómo registrar en archivos sueltos con estas herramientas. Pero, en primer lugar, asegúrate de que tienes tras la instalación:

- Los dispositivos deben estar en Windows 10 Creators Update (compilación 14965) o posterior.
- Tendrás que habilitar [el modo de desarrollador](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) y [detección de dispositivos](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) en todos los dispositivos.

> [!IMPORTANT]
> Registro de archivos sueltos solo está disponible en dispositivos que admiten el protocolo de recurso compartido de red (SMB): escritorio y Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrar con WinDeployAppCmd

Si estás usando las herramientas de SDK correspondiente a Windows 10 Creators Update (compilación 14965) o posterior, puedes usar el `WinDeployAppCmd` comando en un símbolo del sistema.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Ruta de acceso de red** : la ruta de acceso de archivos sueltos de la aplicación.

**Dirección IP** : la dirección IP del equipo de destino.

**el equipo de destino PIN** : un PIN, si es necesario, para establecer una conexión con el dispositivo de destino. Se te pedirá que vuelvas a intentarlo con la `-pin` opción si se requiere autenticación. Consulta la [Detección de dispositivos](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para obtener información sobre cómo obtener un PIN.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal está disponible en todos los dispositivos de Windows 10 y los desarrolladores lo usan para probar y validar su trabajo. Encarga a todas las audiencias de la Comunidad de desarrolladores con su experiencia de usuario del explorador y los puntos de conexión REST. Para obtener más información sobre Device Portal, consulta la [Introducción a Windows Device Portal](device-portal.md).

Para registrar el diseño de archivos sueltos en el Portal de dispositivos, siga estos pasos.

1. Conectarte a Device Portal siguiendo los pasos descritos en la sección **configuración** de la [Introducción a Windows Device Portal](device-portal.md).
1. En la pestaña del Administrador de aplicaciones, selecciona **Register del recurso compartido de red**.
1. Escribe la ruta de acceso de recurso compartido de red para el diseño de archivos sueltos. 
1. Si el dispositivo host no tiene acceso al recurso compartido de red, habrá un símbolo del sistema para especificar las credenciales necesarias.
1. Una vez completado el registro, puedes iniciar la aplicación.

En la página de administrador de aplicaciones del Portal de dispositivos, también puede registrar los diseños de archivos sueltos opcional para la aplicación principal seleccionando la casilla de verificación **que quiero especificar los paquetes opcionales** y, a continuación, especificar las rutas de acceso de recurso compartido de red de las aplicaciones opcionales. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell también te permite registrar los diseños de archivos sueltos, pero solo en el dispositivo local. Si es necesario registrar un diseño a un dispositivo remoto, tendrás que usar uno de los otros métodos. 

Para registrar el diseño de archivos sueltos, iniciar PowerShell y escribe lo siguiente.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Solución de problemas

### <a name="mapped-network-drives"></a>Unidades de red asignadas
Actualmente, las unidades de red asignadas no se admiten los registros de archivos sueltos. Hacer referencia a la unidad asignada con completa la ruta de acceso de recurso compartido de red.

### <a name="registration-failure"></a>Error de registro
El dispositivo en el que el registro está llevando a cabo que tienen acceso para el diseño del archivo. Si el diseño del archivo está alojado en un recurso compartido de red, asegúrate de que el dispositivo tiene acceso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Las modificaciones en los activos visuales no se está cargando en la aplicación 
La aplicación cargará sus activos visuales en tiempo de inicio. Si las modificaciones se realizaron en los activos visuales después de iniciar la aplicación, debes volver a iniciar la aplicación para ver los cambios más recientes.
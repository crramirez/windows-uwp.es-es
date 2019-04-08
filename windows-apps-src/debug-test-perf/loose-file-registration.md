---
title: Implementar una aplicación a través del registro de archivos dinámico
description: En esta guía se muestra cómo usar el diseño de archivos dinámico para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlas.
ms.date: 6/1/2018
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos, Administrador de aplicaciones, implementación, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 928c07bd23228f0fefd78be6019a0d116b2e6e4b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635430"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implementar una aplicación a través del registro de archivos dinámico 

En esta guía se muestra cómo usar el diseño de archivos dinámico para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlas. Registrar los diseños de archivo dinámico permite a los desarrolladores validar rápidamente sus aplicaciones sin necesidad de paquete e instalar las aplicaciones. 

## <a name="what-is-a-loose-file-layout"></a>¿Qué es un diseño de archivo dinámico?

Diseño de archivo dinámico es simplemente el acto de colocar el contenido de la aplicación en una carpeta en lugar de pasar a través del proceso de empaquetado. El contenido del paquete está "flexible" disponibles en una carpeta y no empaquetados. 

> [!WARNING]
> Registro de diseño de archivo dinámico es para que los desarrolladores y diseñadores validar rápidamente sus aplicaciones durante el desarrollo activo. Este enfoque no debe usarse a la versión "interna" o la aplicación de vuelo. Se recomienda que se realiza la validación final en una aplicación empaquetada que está firmada con un certificado de confianza. 

## <a name="advantages-of-loose-file-registration"></a>Ventajas del registro de archivo dinámico

- **Validación rápida** -porque ya se desempaquetan los archivos de aplicación, los usuarios pueden registrar el diseño de archivo dinámico e inicie la aplicación rápidamente. Al igual que una aplicación normal, el usuario podrá usar la aplicación tal como se diseñó. 
- **Fácil distribución en red** -si se encuentran los archivos separados en un recurso compartido de red en lugar de una unidad local, los desarrolladores pueden enviar a la ubicación del recurso compartido de red a otros usuarios que tienen acceso a la red y puede registrar el diseño de archivo dinámico y ejecutar el aplicación. Esto permite que varios usuarios validar la aplicación al mismo tiempo. 
- **Colaboración** -registro de archivo dinámico permite a los desarrolladores y diseñadores seguir trabajando en activos visuales, mientras que la aplicación esté registrada. Los usuarios verán estos cambios al iniciar la aplicación. Tenga en cuenta que solo se pueden cambiar recursos estáticos de esta manera. Si necesita modificar cualquier código o el contenido creado dinámicamente, debe volver a compilar la aplicación.

## <a name="how-to-register-a-loose-file-layout"></a>Cómo registrar un diseño de archivo dinámico

Windows proporciona varias herramientas para desarrolladores para registrar los diseños de archivo dinámico en dispositivos locales y remotos. Puede elegir entre `WinDeployAppCmd` (herramienta de SDK de Windows), Windows Device Portal, PowerShell, y [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). A continuación analizaremos cómo registrar archivos separados con estas herramientas. Pero en primer lugar, asegúrese de que tiene la configuración:

- Los dispositivos deben estar en Windows 10 Creators Update (compilación 14965) o una versión posterior.
- Deberá habilitar [modo de programador](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) y [detección de dispositivos](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) en todos los dispositivos.

> [!IMPORTANT]
> Registro de archivo dinámico solo está disponible en los dispositivos que admiten el protocolo de recurso compartido de red (SMB): Escritorio y Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrar con WinDeployAppCmd

Si está utilizando las herramientas del SDK correspondiente a Windows 10 Creators Update (compilación 14965) o posterior, puede usar el `WinDeployAppCmd` comando en un símbolo del sistema.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Ruta de acceso de red** : la ruta de acceso a archivos separados de la aplicación.

**Dirección IP** : la dirección IP del equipo de destino.

**el equipo de destino PIN** : un PIN, si es necesario establecer una conexión con el dispositivo de destino. Se le pedirá que vuelva a intentarlo con el `-pin` opción si se requiere autenticación. Consulte [detección de dispositivos](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para obtener información sobre cómo obtener un PIN.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal está disponible en todos los dispositivos Windows 10 y se utilizan los desarrolladores para probar y validar su trabajo. Encarga a todos los destinatarios de la Comunidad de desarrolladores con su experiencia de usuario del explorador y los puntos de conexión REST. Para obtener más información sobre el Portal de dispositivos, consulte el [información general de Windows Device Portal](device-portal.md).

Para registrar el diseño de archivo dinámico en el Portal de dispositivos, siga estos pasos.

1. Conéctese al Portal del dispositivo siguiendo los pasos descritos en la **instalación** sección de la [información general de Windows Device Portal](device-portal.md).
1. En la pestaña Administrador de aplicaciones, seleccione **Register del recurso compartido de red**.
1. Escriba la ruta de acceso del recurso compartido de red para el diseño de archivo dinámico. 
1. Si el dispositivo de host no tiene acceso al recurso compartido de red, habrá un símbolo del sistema que escriba las credenciales necesarias.
1. Una vez completado el registro, puede iniciar la aplicación.

En la página del Administrador de aplicaciones de dispositivo del Portal, también puede registrar los diseños de archivo dinámico opcional para la aplicación principal seleccionando el **deseo especificar paquetes opcionales** casilla de verificación y, a continuación, especifique la red comparten las rutas de acceso de las aplicaciones opcionales . 

### <a name="powershell"></a>PowerShell 

Windows PowerShell también permite registrar los diseños de archivo dinámico, pero solo en el dispositivo local. Si necesita registrar un diseño a un dispositivo remoto, deberá usar uno de los otros métodos. 

Para registrar el diseño del archivo flexible, inicie PowerShell y escriba lo siguiente.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Solución de problemas

### <a name="mapped-network-drives"></a>Unidades de red asignadas
Actualmente, no se admiten unidades de red asignadas para los registros de archivo dinámico. Hacer referencia a la unidad asignada con completo la ruta de acceso del recurso compartido de red.

### <a name="registration-failure"></a>Error de registro
El dispositivo en el que está realizando el registro debe tener acceso al diseño del archivo. Si el diseño del archivo se hospeda en un recurso compartido de red, asegúrese de que el dispositivo tenga acceso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Modificaciones de activos visuales no se cargan en la aplicación 
La aplicación cargará sus activos visuales en tiempo de inicio. Si se realizaran modificaciones a los activos visuales después de iniciar la aplicación, debe volver a iniciar la aplicación para ver los cambios más recientes.
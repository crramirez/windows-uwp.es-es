---
title: Implementar una aplicación mediante el registro de archivos dinámico
description: En esta guía se muestra cómo usar el diseño de archivos dinámico para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlas.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, portal de dispositivos, administrador de aplicaciones, implementación, SDK
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681936"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implementar una aplicación mediante el registro de archivos dinámico 

En esta guía se muestra cómo usar el diseño de archivos dinámico para validar y compartir aplicaciones de Windows 10 sin necesidad de empaquetarlas. El registro de diseños de archivo sueltos permite a los desarrolladores validar rápidamente sus aplicaciones sin necesidad de empaquetar e instalar las aplicaciones. 

## <a name="what-is-a-loose-file-layout"></a>¿Qué es un diseño de archivo dinámico?

El diseño de archivos dinámicos es simplemente el acto de colocar el contenido de la aplicación en una carpeta en lugar de pasar por el proceso de empaquetado. El contenido del paquete está "débilmente" disponible en una carpeta y no empaquetado. 

> [!WARNING]
> El registro de distribución de archivos sueltos es para que los desarrolladores y diseñadores validen rápidamente sus aplicaciones durante el desarrollo activo. Este enfoque no debe usarse para "software" o el vuelo de la aplicación. Se recomienda realizar la validación final en una aplicación empaquetada que está firmada con un certificado de confianza. 

## <a name="advantages-of-loose-file-registration"></a>Ventajas del registro de archivos dinámicos

- **Validación rápida** : dado que los archivos de la aplicación ya están desempaquetados, los usuarios pueden registrar rápidamente el diseño de archivo dinámico e iniciar la aplicación. Al igual que una aplicación normal, el usuario podrá usar la aplicación tal y como estaba diseñada. 
- **Distribución sencilla en la red** : si los archivos sueltos se encuentran en un recurso compartido de red en lugar de en una unidad local, los desarrolladores pueden enviar la ubicación del recurso compartido de red a otros usuarios que tengan acceso a la red y pueden registrar el diseño de archivos dinámicos y ejecutar la aplicación. Esto permite que varios usuarios validen la aplicación simultáneamente. 
- **Colaboración** : el registro de archivos dinámicos permite a los desarrolladores y diseñadores seguir trabajando en los recursos visuales mientras se registra la aplicación. Los usuarios verán estos cambios cuando inicien la aplicación. Tenga en cuenta que solo puede cambiar los recursos estáticos de esta manera. Si necesita modificar el código o el contenido creado dinámicamente, debe volver a compilar la aplicación.

## <a name="how-to-register-a-loose-file-layout"></a>Cómo registrar un diseño de archivo dinámico

Windows proporciona varias herramientas de desarrollo para registrar diseños de archivo sueltos en dispositivos locales y remotos. Puede elegir entre `WinDeployAppCmd` (herramienta de Windows SDK), Windows Device portal, PowerShell y [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). A continuación, veremos cómo registrar archivos sueltos con estas herramientas. Pero en primer lugar, asegúrese de que tiene la siguiente configuración:

- Los dispositivos deben estar en Windows 10 Creators Update (compilación 14965) o posterior.
- Tendrá que habilitar el [modo de desarrollador](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) y la [detección de dispositivos](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) en todos los dispositivos.

> [!IMPORTANT]
> El registro de archivos dinámicos solo está disponible en los dispositivos que admiten el protocolo de recurso compartido de red (SMB): escritorio y Xbox. 

### <a name="register-with-windeployappcmd"></a>Registro con WinDeployAppCmd

Si usa las herramientas de SDK correspondientes a Windows 10 Creators Update (compilación 14965) o una versión posterior, puede usar el comando `WinDeployAppCmd` en un símbolo del sistema.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Ruta de acceso de red** : la ruta de acceso a los archivos sueltos de la aplicación.

**Dirección IP** : la dirección IP del equipo de destino.

**PIN del equipo de destino** : un PIN, si es necesario, para establecer una conexión con el dispositivo de destino. Se le pedirá que vuelva a intentarlo con la opción de `-pin` si se requiere autenticación. Consulte [detección de dispositivos](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para obtener información sobre cómo obtener un PIN.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device portal está disponible en todos los dispositivos de Windows 10 y los desarrolladores lo usan para probar y validar su trabajo. Se ocupa de todas las audiencias de la comunidad de desarrolladores con sus puntos de conexión de REST y experiencia del explorador. Para más información sobre el portal de dispositivos, consulte la [Introducción a Windows Device portal](device-portal.md).

Para registrar el diseño de archivos dinámicos en el portal de dispositivos, siga estos pasos.

1. Para conectarse al portal de dispositivos, siga los pasos descritos en la sección **configuración** de [información general de Windows Device portal](device-portal.md).
1. En la pestaña administrador de aplicaciones, seleccione **registrar desde recurso compartido de red**.
1. Escriba la ruta de acceso del recurso compartido de red para el diseño de archivo dinámico. 
1. Si el dispositivo host no tiene acceso al recurso compartido de red, se le pedirá que escriba las credenciales necesarias.
1. Una vez completado el registro, puede iniciar la aplicación.

En la página Administrador de aplicaciones del portal de dispositivos, también puede registrar diseños de archivo sueltos opcionales para la aplicación principal. para ello, seleccione la casilla **quiero especificar paquetes opcionales** y luego especifique las rutas de acceso del recurso compartido de red de las aplicaciones opcionales. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell también permite registrar diseños de archivo sueltos, pero solo en el dispositivo local. Si necesita registrar un diseño en un dispositivo remoto, tendrá que usar uno de los otros métodos. 

Para registrar el diseño de archivo dinámico, inicie PowerShell y escriba lo siguiente.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>de solución de problemas

### <a name="mapped-network-drives"></a>Unidades de red asignadas
Actualmente, las unidades de red asignadas no son compatibles con los registros de archivos sueltos. Haga referencia a la unidad asignada con la ruta de acceso del recurso compartido de red completa.

### <a name="registration-failure"></a>Error de registro
El dispositivo en el que se realiza el registro tendrá que tener acceso al diseño del archivo. Si el diseño de archivo está hospedado en un recurso compartido de red, asegúrese de que el dispositivo tiene acceso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>No se están cargando las modificaciones de los recursos visuales en la aplicación 
La aplicación cargará sus activos visuales en el momento del inicio. Si se realizaron modificaciones en los activos visuales después de iniciar la aplicación, debe volver a iniciar la aplicación para ver los cambios más recientes.

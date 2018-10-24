---
author: ridomin
title: Solucionar problemas de instalación con el archivo del Instalador de aplicación
description: Problemas comunes al instalar aplicaciones de prueba con el archivo del Instalador de aplicación.
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, app installer, instalador de aplicaciones, AppInstaller, sideload, realizar instalación de prueba
ms.localizationpriority: medium
ms.openlocfilehash: e94eb0e819796dda456899bb877057e4532f5ce9
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "5442313"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>Solucionar problemas de instalación con el archivo del Instalador de aplicación

Si encuentras algún problema al instalar una aplicación desde el archivo de Instalador de aplicación, en este tema se proporciona una orientación para solucionar problemas que puede servir de ayuda.

## <a name="prerequisites"></a>Requisitos previos

Para poder realizar instalaciones de prueba de aplicaciones en Windows 10, el dispositivo del usuario debe cumplir los requisitos siguientes:

- El dispositivo debe estar habilitado para el modo de desarrollador o instalación de prueba de aplicaciones. Consulta [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) para obtener información.
- El certificado utilizado para firmar el paquete debe ser de confianza para este dispositivo. Consulta la sección **Certificados de confianza** a continuación para obtener más detalles.
- La versión de Windows 10 debe admitir el esquema de archivos `.appinstaller` y el protocolo de distribución.

## <a name="common-issues"></a>Problemas comunes

Hay algunas problemas al instalar una aplicación de prueba por primera vez en el equipo del usuario. Las siguientes secciones describen los problemas más comunes y sus soluciones.

### <a name="windows-version"></a>Versión de Windows

Cada versión de Windows 10 mejora la experiencia de instalación de prueba; en la siguiente tabla encontrarás las funcionalidades que están disponibles en cada versión principal. Si intentas realizar instalaciones de prueba de una aplicación con un método no admitido en tu versión de Windows 10, obtendrás un error de implementación.

| Versión | Notas de instalación de prueba |
|---------|----------------|
| Compilación 17134 (actualización de abril de 2018, versión 1804)    | Al archivo `.appinstaller` se puede acceder en carpetas UNC/recurso compartido. También están disponibles comprobaciones de actualizaciones configurables. |
| Compilación 16299, (Fall Creators Update, versión 1709) | Ha introducido el archivo `.appinstaller` para proporcionar actualizaciones automáticas a tu aplicación. Esta versión solo admite extremos HTTP. Las comprobaciones de actualizaciones no son configurables y se producen cada 24 horas. |
| Compilación 15063, (Creators Update, versión 1703)      | La aplicación de Instalador de aplicación es capaz de descargar dependencias de la aplicación (solo en modo de lanzamiento) desde la Store. |
| Compilación 14393 (Actualización de aniversario, versión 1607)   | Se has introducido la aplicación de Instalador de aplicación para instalar los archivos .appx y .appxbundle; el archivo .appinstaller no es compatible. |
| Compilación 10586 (actualización de noviembre, versión 1511)      | La instalación de prueba solo está disponible a través de PowerShell utilizando el comando [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps). |
| Compilación 10240 (Windows 10, versión 1507)           | La instalación de prueba solo está disponible a través de PowerShell utilizando el comando [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps). |

### <a name="trusted-certificates"></a>Certificados de confianza

El paquete de aplicación debe firmarse con un certificado que sea de confianza para el dispositivo. Los certificados proporcionados por las entidades de certificación comunes son de confianza de manera predeterminada en el sistema operativo de Windows; sin embargo, si el certificado no es de confianza, se debe instalar en el dispositivo **antes** de instalar la aplicación. Para confiar en el certificado, el certificado debe estar presente en uno de los siguientes almacenes de certificados de equipo local en el dispositivo:

- Editores de confianza
- Personas de confianza
- Entidades de raíz de confianza (no recomendado)

 >[!IMPORTANT]
 > Instalar un certificado en el almacén del equipo Local, requiere acceso administrativo.

### <a name="dependencies-not-installed"></a>Las dependencias no se instalan 

Las aplicaciones para UWP pueden tener dependencias de marco en función de la plataforma de aplicaciones utilizada para generar la aplicación. Si estás usando C# o VB, la aplicación requerirá los paquetes de NET Runtime y .NET Framework. Las aplicaciones C++ requieren las VCLibs.

>[!IMPORTANT] 
> Si el paquete de aplicación está integrado en la configuración del modo de lanzamiento, las dependencias de marco se obtendrán desde Microsoft Store. Sin embargo, si la aplicación está integrada en la configuración del modo de depuración, las dependencias se obtendrán desde la ubicación especificada en el archivo `.appinstaller`.

### <a name="files-not-accessible"></a>Archivos no accesibles

Al instalar desde un extremo HTTP, es importante verificar que todos los archivos están accesibles con el tipo MIME correcto. El método más sencillo para verificar estos archivos es mediante los vínculos proporcionados en la página HTML generada por Visual Studio. Debes comprobar estos archivos:

- `.appinstaller` archivo, disponible como un `application/xml`
- `.appx` y archivos `.appxbundle`, disponibles como `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>Aislar problemas de la aplicación del Instalador de aplicación

Si la aplicación del Instalador de aplicación no puede instalar la aplicación, estos pasos te ayudarán a identificar el problema de instalación.

### <a name="verify-app-package-file-installation"></a>Comprobar la instalación del archivo de paquete de aplicación

- Descargar el archivo de paquete de la aplicación en una carpeta local e intenta instalarlo mediante el comando de PowerShell [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) .

- Descarga el archivo `.appinstaller` en una carpeta local e intenta instalarlo mediante el comando `Add-AppxPackage -Appinstaller` de PowerShell.

## <a name="related-logs"></a>Registros relacionados

La infraestructura de implementación de la aplicación proporciona registros de depuración en el Visor de eventos de Windows. Estos registros se encuentran aquí: `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`




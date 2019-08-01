---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empaquetado de aplicaciones
description: Esta sección contiene o menciona artículos sobre los paquetes de aplicaciones para la Plataforma universal de Windows (UWP).
ms.date: 07/22/2019
ms.topic: article
keywords: windows 10, uwp, packaging
ms.localizationpriority: medium
ms.openlocfilehash: d067511e4ceb5aaf072aabf8f74ccf186a7787dc
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682670"
---
# <a name="packaging-apps"></a>Empaquetado de aplicaciones

Esta sección contiene o se vincula a artículos sobre el empaquetado de aplicaciones para la Plataforma universal de Windows (UWP) en paquetes de aplicaciones MSIX y AppX para su implementación e instalación. Algunos de estos vínculos se dirigen a artículos relevantes de la [documentación de MSIX](https://docs.microsoft.com/windows/msix/).

> [!NOTE]
> El formato original del empaquetado de aplicaciones para UWP en Windows 10 se llamaba AppX. A partir de Windows 10, versión 1809, el nombre de este formato de empaquetado cambió a MSIX y se amplió para admitir todos los tipos de aplicaciones de Windows, incluidas las aplicaciones de escritorio de .NET y C++/Win32. También se ha ampliado la compatibilidad con MSIX a las versiones anteriores de Windows. Para más información, consulta la [documentación de MSIX](https://docs.microsoft.com/windows/msix/).

| Tema | Descripción |
|-------|-------------|
| [Empaquetar una aplicación para UWP con Visual Studio](/windows/msix/package/packaging-uwp-apps) | Para distribuir o vender tu aplicación de la Plataforma universal de Windows (UWP), necesitas crear un paquete de aplicación para ella. |
| [Empaquetado manual de aplicaciones](/windows/msix/package/manual-packaging-root) | Si quieres crear y firmar un paquete de aplicación, pero no usaste Visual Studio para desarrollar tu aplicación, tendrás que usar las herramientas de empaquetado manual de aplicaciones. |
| [Arquitecturas de paquetes de aplicaciones](/windows/msix/package/device-architecture) | Obtén más información sobre qué arquitecturas de procesador debes usar al compilar el paquete de la aplicación. |
| [Instalación en streaming de aplicaciones para UWP](/windows/msix/package/streaming-install) | La instalación en streaming de aplicaciones te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano. |
| [Creación de paquetes opcionales y conjuntos relacionados](/windows/msix/package/optional-packages) | Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Son útiles para el contenido descargable (DLC), para dividir una aplicación grande que tenga restricciones de tamaño o para enviar cualquier contenido adicional aparte de la aplicación original. |
| [Paquetes opcionales con código ejecutable](/windows/msix/package/optional-packages-with-executable-code) | Aprende a usar Visual Studio para crear un paquete opcional con código ejecutable. |
| [Instalar aplicaciones para Windows 10 con el Instalador de aplicación](/windows/msix/app-installer/app-installer-root) | El Instalador de aplicación permite instalar las aplicaciones para Windows 10 haciendo doble clic en el paquete de la aplicación. |
| [Instalar aplicaciones con la herramienta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación para UWP desde una máquina con Windows 10 en cualquier dispositivo con Windows 10 Mobile. Puedes usar esta herramienta para implementar un paquete de aplicación si el dispositivo con Windows 10 Mobile está conectado mediante USB o disponible en la misma subred sin necesidad de Microsoft Visual Studio o la solución para dicha aplicación. Este artículo describe cómo instalar aplicaciones para UWP con esta herramienta. |
| [Configurar compilaciones automatizadas para la aplicación para UWP](auto-build-package-uwp-apps.md) | Si quieres empaquetar tu aplicación dentro de un proceso de compilación automatizado, este tema muestra cómo hacerlo con Visual Studio Team Services (VSTS). |
| [Declaraciones de funcionalidades de las aplicaciones](app-capability-declarations.md) | Las funcionalidades deben declararse en el [manifiesto del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) de la aplicación para poder obtener acceso a determinadas API o ciertos recursos, como imágenes o música, o a dispositivos, como la cámara o el micrófono. |
| [Descargar e instalar actualizaciones de paquetes desde Store](self-install-package-updates.md) | La aplicación para UWP puede buscar mediante programación actualizaciones de paquete, así como instalarlas. La aplicación también puede consultar los paquetes que se marcaron como obligatorios en el Centro de partners y deshabilitar la funcionalidad hasta que se instale la actualización obligatoria.  |

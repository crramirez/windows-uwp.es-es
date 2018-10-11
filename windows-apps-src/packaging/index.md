---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empaquetado de aplicaciones
description: En esta sección se incluyen artículos o vínculos a artículos sobre el empaquetado de la aplicación para la Plataforma universal de Windows (UWP).
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, packaging, empaquetado
ms.localizationpriority: medium
ms.openlocfilehash: ce77391fc189ef33aba3002685b0662d7cab1953
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "4529267"
---
# <a name="packaging-apps"></a>Empaquetado de aplicaciones


## <a name="purpose"></a>Propósito

Esta sección contiene o menciona artículos sobre los paquetes de aplicaciones para la Plataforma universal de Windows (UWP).

| Tema | Descripción |
|-------|-------------|
| [Empaquetar una aplicación para UWP con Visual Studio](packaging-uwp-apps.md) | Para distribuir o vender tu aplicación de Plataforma universal de Windows (UWP), necesitas crear un paquete de aplicación para ella. |
| [Empaquetado manual de la aplicación](manual-packaging-root.md) | Si quieres crear y firmar un paquete de aplicación, pero no usaste Visual Studio para desarrollar tu aplicación, tendrás que usar las herramientas de empaquetado de aplicación manual. |
| [Arquitecturas de paquetes de aplicaciones](device-architecture.md) | Obtén más información sobre qué arquitecturas de procesador debes usar al compilar el paquete de aplicación para UWP. |
| [Instalación en streaming de aplicaciones para UWP](streaming-install.md) | La instalación en streaming de la Plataforma universal de Windows (UWP) te permite especificar qué partes de la aplicación deben descargarse primero de Microsoft Store. Al descargar primero los archivos esenciales de la aplicación, el usuario puede iniciarla e interactuar con ella mientras el resto de archivos terminan de descargarse en segundo plano. |
| [Paquetes opcionales y creación de conjuntos relacionados](optional-packages.md) | Los paquetes opcionales tienen contenido que se puede integrar con un paquete principal. Estos son útiles para el contenido descargable (DLC), para dividir una aplicación grande que tenga restricciones de tamaño o para enviar cualquier contenido adicional aparte de la aplicación original. |
| [Paquetes opcionales con código ejecutable](optional-packages-with-executable-code.md) | Aprende a usar Visual Studio para crear un paquete opcional con código ejecutable. |
| [Instalar aplicaciones para UWP con el Instalador de aplicación](appinstaller-root.md) | El Instalador de aplicación permite la instalación de las aplicaciones para UWP haciendo doble clic en el paquete de la aplicación. |
| [Instalar aplicaciones con la herramienta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación para UWP desde un equipo con Windows10 en cualquier dispositivo con Windows10 Mobile. Puedes usar esta herramienta para implementar un paquete de la aplicación cuando el dispositivo de Windows 10 Mobile está conectado mediante USB o disponible en la misma subred sin necesidad de Microsoft Visual Studio o la solución para esa aplicación. Este artículo describe cómo instalar aplicaciones para UWP con esta herramienta. |
| [Configurar compilaciones automatizadas para la aplicación para UWP](auto-build-package-uwp-apps.md) | Si quieres empaquetar tu aplicación dentro de un proceso de compilación automatizado, este tema muestra cómo hacerlo con Visual Studio Team Services (VSTS). |
| [Declaraciones de funcionalidades de las aplicaciones](app-capability-declarations.md) | Las funcionalidades deben declararse en el [manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/BR211474) de la aplicación para UWP para poder obtener acceso a determinadas API o ciertos recursos, como imágenes o música, o a dispositivos, como la cámara o el micrófono. |
| [Descargar e instalar actualizaciones de paquete desde la Store.](self-install-package-updates.md) | La aplicación para UWP puede buscar mediante programación actualizaciones de paquete, así como instalarlas. La aplicación también puede consultar los paquetes que se marcaron como obligatorios en el panel del Centro de desarrollo de Windows y deshabilitar la funcionalidad hasta que se instale la actualización obligatoria.  |

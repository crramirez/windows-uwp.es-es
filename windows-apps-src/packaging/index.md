---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empaquetado de aplicaciones
description: "Esta sección contiene o menciona artículos sobre los paquetes de aplicaciones para la Plataforma universal de Windows (UWP)."
translationtype: Human Translation
ms.sourcegitcommit: 28cd2b2a922a20e0b9ffc4d1ca65f6a55e92aa8f
ms.openlocfilehash: 8aa4bfc8004b4d0739fe09e6e49e66de372569c4

---
# <a name="packaging-apps"></a>Empaquetado de aplicaciones

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="purpose"></a>Finalidad

Esta sección contiene o menciona artículos sobre los paquetes de aplicaciones para la Plataforma universal de Windows (UWP).

| Tema | Descripción |
|-------|-------------|
| [Empaquetado de aplicaciones para UWP](packaging-uwp-apps.md) | Para vender tu aplicación para UWP o distribuirla a otros usuarios, necesitas crear un paquete appxupload para ella. Al crear el paquete .appxupload, se generará otro paquete .appx destinado a pruebas y a la instalación de prueba. Puedes distribuir la aplicación directamente mediante la instalación de prueba del paquete .appx a un dispositivo. Este artículo describe el proceso de configuración, creación y prueba de un paquete de la aplicación para UWP. Para más información sobre la instalación de prueba, consulta [Realizar la instalación de prueba de aplicaciones con DISM](http://go.microsoft.com/fwlink/?LinkID=231020). |
| [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe crea, cifra, descifra y extrae los archivos de paquetes de aplicaciones y lotes. |
| [Instalar aplicaciones con la herramienta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación para UWP desde un equipo con Windows 10 en cualquier dispositivo con Windows 10 Mobile. Puedes usar esta herramienta para implementar un paquete .appx si el dispositivo con Windows 10 Mobile está conectado mediante USB o disponible en la misma subred sin necesidad de Microsoft Visual Studio o la solución para dicha aplicación. Este artículo describe cómo instalar aplicaciones para UWP con esta herramienta. |
| [Configurar compilaciones automatizadas para la aplicación para UWP](auto-build-package-uwp-apps.md) | Si quieres empaquetar tu aplicación dentro de un proceso de compilación automatizado, este tema muestra cómo hacerlo con Visual Studio Team Services (VSTS). |
| [Declaraciones de funcionalidades de las aplicaciones](app-capability-declarations.md) | Las funcionalidades deben declararse en el [manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/BR211474) de la aplicación para UWP para poder obtener acceso a determinadas API o ciertos recursos, como imágenes o música, o a dispositivos, como la cámara o el micrófono. |
| [Descargar e instalar actualizaciones de paquete para tu aplicación](self-install-package-updates.md) | La aplicación para UWP puede buscar mediante programación actualizaciones de paquete, así como instalarlas. La aplicación también puede consultar los paquetes que se marcaron como obligatorios en el panel del Centro de desarrollo de Windows y deshabilitar la funcionalidad hasta que se instale la actualización obligatoria. En este artículo se describe cómo realizar estas tareas. |
 



<!--HONumber=Dec16_HO1-->



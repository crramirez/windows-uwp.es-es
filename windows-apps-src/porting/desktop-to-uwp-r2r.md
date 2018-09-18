---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: Optimizar las aplicaciones de escritorio de .NET con imágenes nativas
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, imagen nativa compilador
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "4017749"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimizar las aplicaciones de escritorio de .NET con imágenes nativas

> [!NOTE]
> Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Puede mejorar el tiempo de inicio de la aplicación de .NET Framework compilando previamente los archivos binarios. Puedes usar esta tecnología en aplicaciones de gran tamaño que empaquetar y distribuir a través de la tienda Windows. En algunos casos, nos hemos observado una mejora de rendimiento del 20%. Para obtener más información sobre esta tecnología en la [información técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Te hemos publicado una versión preliminar del compilador imagen nativa como un [paquete de NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Puedes aplicar este paquete a cualquier aplicación de .NET Framework que está destinada a la versión 4.6.2 .NET Framework o una versión posterior. Este paquete agrega un paso de compilación de post que incluya a todos los archivos binarios de la aplicación usado una carga de nativa. Se cargará esta carga optimizada cuando la aplicación se ejecuta en .NET 4.7.2 y versiones posteriores mientras que las versiones anteriores aún cargarán el código MSIL.

El [marco de .NET 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) se incluye en la [actualización de Windows 10 de abril de 2018](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). También puede instalar esta versión de .NET Framework en el equipo que ejecuta Windows 7 + y Windows Server 2008 R2 +.

> [!IMPORTANT]
> Si quieres generar imágenes nativas de la aplicación empaquetada por el proyecto de empaquetado de aplicaciones de Windows, asegúrate de establecer la versión mínima de la plataforma de destino del proyecto para la actualización de aniversario de Windows.

## <a name="how-to-produce-native-images"></a>Cómo producir imágenes nativas

Sigue estas instrucciones para configurar los proyectos.

1. Configurar el marco de trabajo de destino como 4.6.2 o superior

2. Configurar la plataforma de destino como x86 o x64 

3. Agrega los paquetes de NuGet.

4. Crear una versión de lanzamiento.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar el marco de trabajo de destino como 4.6.2 o superior

Configurar el proyecto al destino de .NET Framework 4.6.2 tendrás que las herramientas de desarrollo de .NET Framework 4.6.2 o posterior. Estas herramientas están disponibles mediante el instalador de Visual Studio como componentes opcionales en la carga de trabajo de desarrollo de escritorio. NET:

![Instalar .NET 4.6.2 herramientas de desarrollo](images/desktop-to-uwp/install-4.6.2-devpack.png)

Como alternativa, puedes obtener los paquetes de desarrollador de .NET desde:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar la plataforma de destino como x86 o x64

El compilador de imagen nativa optimiza el código para una plataforma determinada. Para usarlo, debes configurar la aplicación como destino una plataforma específica, como x86 o x64.

Si tienes varios proyectos en la solución, solo el proyecto de punto de entrada (más probable es que el proyecto que genera un archivo ejecutable) debe compilarse como x86 o x64. Archivos binarios adicionales que haga referencia desde el proyecto principal se procesará con la arquitectura especificada en el proyecto principal, incluso si se compilan como AnyCPU.

Configurar el proyecto:

1. Haz clic en la solución y, a continuación, selecciona **Configuration Manager**.

2. Selecciona **< nuevo.. >** en el menú desplegable de **plataforma** situada junto al nombre del proyecto que genera el archivo ejecutable.

3. En el cuadro de diálogo de la **Nueva plataforma de proyecto** , asegúrate de que la lista desplegable de **Opciones de configuración de copia de** está establecida en **Cualquier CPU**.

![Configurar x86](images/desktop-to-uwp/configure-x86.png)

Repite este paso para `Release/x64` si quieres que producen x64 archivos binarios.

>[!IMPORTANT]
> Configuración de cualquier CPU no se admite el compilador de imagen nativa.

## <a name="add-the-nuget-packages"></a>Agrega los paquetes de NuGet

El compilador de imagen nativa se proporciona como un paquete de NuGet que tengas que agregar al proyecto de Visual Studio que genera el archivo ejecutable. Suele ser el proyecto de Windows Forms o WPF. Usa este comando de PowerShell para hacerlo.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Los paquetes de vista previa se publican en NuGet.org como en la lista. No encontrarlos NuGet.org exploración o mediante el Administrador de paquetes de UI en Visual Studio. Sin embargo, puede instalar desde la consola del Administrador de paquetes y cuándo se restauración desde un equipo diferente. Haremos que los paquetes totalmente accesible cuando se publica la primera versión preliminar no.

## <a name="create-a-release-build"></a>Crear una versión de lanzamiento

El paquete de NuGet configura el proyecto para ejecutar una herramienta adicional para las compilaciones de versiones. Esta herramienta agrega el código nativo a los mismos archivos binarios.
Para comprobar que la herramienta haya procesado los archivos binarios puedes revisar la compilación de salida para asegurarse de que incluye un mensaje como este:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Preguntas frecuentes

**Q. ¿Los archivos binarios de nuevo funcionan en máquinas sin .NET Framework 4.7.2?**

A. Archivos binarios optimizados se beneficiarán de las mejoras de cuando se ejecuta con .NET Framework 4.7.2. Los clientes que ejecutan versiones anteriores de .NET framework, cargará el código MSIL no optimizadas desde el archivo binario.

**Q. ¿Cómo se puede proporcionar comentarios o informar de problemas?**

A. Notificar un problema con la herramienta de comentarios en Visual Studio 2017. [Obtener más información](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. ¿Qué es el impacto de agregar la imagen nativa a los archivos binarios existentes?**

A. Los archivos binarios optimizados contienen el código administrado y nativo, con lo que los archivos finales mayor.

**Q. ¿Publicar archivos binarios con esta tecnología?**

A. Esta versión incluye una licencia Go Live que puedes usar hoy en día.

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
keywords: Windows 10, imagen nativa del compilador
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2788277"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimizar las aplicaciones de escritorio de .NET con imágenes nativas

> [!NOTE]
> Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Puede mejorar el tiempo de inicio de la aplicación de .NET Framework mediante la compilación previa a la de los archivos binarios. Puede usar esta tecnología en aplicaciones de gran tamaño que empaquetar y distribuir a través de la tienda Windows. En algunos casos, se hemos observó una mejora del rendimiento de un 20%. Encontrará más información acerca de esta tecnología en la [información general técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Nos hemos publicado una versión de vista previa del compilador imagen nativa como un [paquete de NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Puede aplicar este paquete en cualquier aplicación de .NET Framework que se dirige a .NET Framework, versión 4.6.2 o posterior. Este paquete agrega un paso de compilación de post que incluye una carga a todos los archivos binarios que usa la aplicación nativa. Se cargará esta carga optimizada cuando la aplicación se ejecuta en .NET 4.7.2 y por encima de mientras que las versiones anteriores aún cargarán el código MSIL.

El [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) se incluye en la [actualización de Windows 2018 10 de abril](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). También puede instalar esta versión de .NET Framework en PC que ejecutan Windows 7 y Windows Server 2008 R2 +.

> [!IMPORTANT]
> Si desea generar imágenes nativas para su aplicación empaquetadas con el proyecto de empaquetado de la aplicación de Windows, asegúrese de establecer la versión mínima de la plataforma de destino del proyecto a la actualización de aniversario de Windows.

## <a name="how-to-produce-native-images"></a>Procedimiento para generar las imágenes nativas

Siga estas instrucciones para configurar los proyectos.

1. Configurar el marco de destino como 4.6.2 o superior

2. Configuración de la plataforma de destino como x86 o x64 

3. Agregue los paquetes de NuGet.

4. Crear una versión de lanzamiento.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar el marco de destino como 4.6.2 o superior

Para configurar el proyecto de destino .NET Framework 4.6.2 necesitará las herramientas de desarrollo de .NET Framework 4.6.2 o más reciente. Estas herramientas están disponibles a través del instalador de Visual Studio como componentes opcionales en la carga de trabajo de desarrollo de escritorio. NET:

![Instale .NET 4.6.2 herramientas de desarrollo](images/desktop-to-uwp/install-4.6.2-devpack.png)

Como alternativa, puede obtener los paquetes de desarrollador de .NET desde:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configuración de la plataforma de destino como x86 o x64

El compilador de imagen nativa optimiza el código para una plataforma determinada. Para usarlo, debe configurar la aplicación para una plataforma específica, como x86 o x64 de destino.

Si tiene varios proyectos de la solución, sólo el proyecto de punto de entrada (más probable es que el proyecto que genera un archivo ejecutable) debe compilarse como x86 o x64. Archivos binarios adicionales que se hace referencia desde el proyecto principal se procesará con la arquitectura especificada en el proyecto principal, incluso si se compilan como cualquier CPU.

Para configurar el proyecto:

1. Haga clic en la solución y, a continuación, seleccione **Administrador de configuración**.

2. Seleccione **< nuevo.. >** en el menú desplegable de **plataforma** junto al nombre del proyecto que genera el archivo ejecutable.

3. En el cuadro de diálogo **Nueva plataforma de proyecto** , asegúrese de que la lista desplegable **Copiar configuración de** se establece en **Cualquier CPU**.

![Configurar x86](images/desktop-to-uwp/configure-x86.png)

Repita este paso para `Release/x64` si desea generar x64 los archivos binarios.

>[!IMPORTANT]
> Configuración de cualquier CPU no es compatible con el compilador de imagen nativa.

## <a name="add-the-nuget-packages"></a>Agregue los paquetes de NuGet

El compilador de imagen nativa se proporciona como un paquete de NuGet que necesita agregar al proyecto de Visual Studio que genera el archivo ejecutable. Suele ser el proyecto de Windows Forms o WPF. Use este comando de PowerShell para hacerlo.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Los paquetes de vista previa se publican en NuGet.org como en la lista. No encontrarlos NuGet.org exploración o mediante la UI de administrador de paquetes en Visual Studio. Sin embargo, puede instalarlas desde la consola de administrador de paquetes y cuándo se restaurar desde un equipo diferente. Asegúrese de los paquetes de totalmente accesible cuando se publica la primera versión sin vista previa.

## <a name="create-a-release-build"></a>Crear una versión de lanzamiento

El paquete de NuGet configura el proyecto para ejecutar una herramienta adicional para las versiones de lanzamiento. Esta herramienta agrega el código nativo a los mismos archivos binarios.
Para comprobar que la herramienta procesa los archivos binarios, puede revisar la compilación de salida para asegurarse de que incluye un mensaje como ésta:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Preguntas más frecuentes

**Q. ¿Los archivos binarios de nuevo funcionan en máquinas sin .NET Framework 4.7.2?**

A. Archivos binarios optimizados se beneficiarán de las mejoras de cuando se ejecuta con .NET Framework 4.7.2. Los clientes que ejecutan versiones anteriores de .NET framework cargará el código MSIL no optimizadas desde el archivo binario.

**Q. ¿Cómo se puede proporcionar comentarios o informe de errores?**

A. Notificar un problema con la herramienta de comentarios en 2017 de Visual Studio. [Obtener más información](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. ¿Cuál es el impacto de la adición de la imagen nativa para los archivos binarios existentes?**

A. Los archivos binarios de optimizada contienen el código administrado y nativo, con lo que los archivos finales mayor.

**Q. ¿Versión binarios mediante esta tecnología?**

A. Esta versión incluye una licencia Go Live que puede usar hoy mismo.

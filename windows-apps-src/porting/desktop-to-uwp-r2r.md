---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: Optimizar sus aplicaciones de escritorio de .NET con imágenes nativas
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Compilador de imágenes de Windows nativo, 10
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3931125"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimizar sus aplicaciones de escritorio de .NET con imágenes nativas

> [!NOTE]
> Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Puede mejorar el tiempo de inicio de la aplicación.NET Framework Precompilando los archivos binarios. Puede utilizar esta tecnología en aplicaciones de gran tamaño que permite empaquetar y distribuir a través del almacén de Windows. En algunos casos, nos hemos observado una mejora del rendimiento del 20%. Puede obtener más información acerca de esta tecnología en la [Introducción técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Hemos publicamos una versión preliminar del compilador imágenes nativas como un [paquete NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Puede aplicar este paquete a cualquier aplicación de.NET Framework que tiene como destino la versión 4.6.2 de.NET Framework o posterior. Este paquete agrega un paso de compilación de post que incluye una carga nativa para todos los binarios utilizados por la aplicación. Esta carga optimizada se cargarán cuando la aplicación se ejecuta en .NET 4.7.2 y superior mientras que las versiones anteriores cargará el código MSIL.

El [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) se incluye en el [10 de abril de 2018 de Windows update](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). También puede instalar esta versión de la de.NET Framework en PC que ejecutan Windows 7 y Windows Server 2008 R2 +.

> [!IMPORTANT]
> Si desea generar imágenes nativas de la aplicación empaquetada por el proyecto del paquete de aplicaciones de Windows, asegúrese de establecer la versión mínima de la plataforma de destino del proyecto en el aniversario de Windows Update.

## <a name="how-to-produce-native-images"></a>¿Cómo producir imágenes nativas

Siga estas instrucciones para configurar los proyectos.

1. Configurar el marco de trabajo de destino como 4.6.2 o superior

2. Configurar la plataforma de destino como x86 o x64 

3. Agregue los paquetes de NuGet.

4. Crear una versión de lanzamiento.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar el marco de trabajo de destino como 4.6.2 o superior

Para configurar el proyecto de destino de.NET Framework 4.6.2 necesitará las herramientas de desarrollo de.NET Framework 4.6.2 o posterior. Estas herramientas están disponibles a través del instalador de Visual Studio como componentes opcionales en la carga de trabajo de desarrollo de escritorio. NET:

![Instalar .NET 4.6.2 herramientas de desarrollo](images/desktop-to-uwp/install-4.6.2-devpack.png)

Como alternativa, puede obtener los paquetes del desarrollador de .NET desde:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar la plataforma de destino como x86 o x64

El compilador de imagen nativa optimiza el código para una plataforma determinada. Para utilizarla, debe configurar la aplicación para una plataforma específica, como x86 o x64.

Si tiene varios proyectos en la solución, sólo el proyecto de punto de entrada (probablemente en el proyecto que genera un archivo ejecutable) debe compilarse como x86 o x64. Binarios hace referenciados desde el proyecto principal se procesará con la arquitectura especificada en el proyecto principal, incluso si se compilan como cualquier CPU.

Para configurar el proyecto:

1. (Ratón) en la solución y, a continuación, seleccione **Administrador de configuración**.

2. Seleccione **< nuevo.. >** en el menú desplegable de **plataforma** junto al nombre del proyecto que genera el archivo ejecutable.

3. En el cuadro de diálogo **Nueva plataforma de proyecto** , asegúrese de que la lista de desplegable **Copiar configuración de** se establece en **Cualquier CPU**.

![Configurar x86](images/desktop-to-uwp/configure-x86.png)

Repita este paso para `Release/x64` si desea producir x64 binarios.

>[!IMPORTANT]
> Configuración cualquier CPU no es compatible con el compilador de imágenes nativas.

## <a name="add-the-nuget-packages"></a>Agregue los paquetes de NuGet

El compilador de imagen nativa se proporciona como un paquete de NuGet que debe agregar al proyecto de Visual Studio que genera el archivo ejecutable. Suele ser el proyecto de formularios Windows Forms o WPF. Utilice este comando PowerShell para hacerlo.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Los paquetes de vista previa se publican en NuGet.org como no listado. No encontrarlos NuGet.org exploración o mediante la UI del Administrador de paquetes de Visual Studio. Sin embargo, se puede instalar desde la consola del Administrador de paquetes y cuando usted restaurar desde un equipo diferente. Cuando se publica la primera versión preliminar no vamos a hacer los paquetes totalmente accesible.

## <a name="create-a-release-build"></a>Crear una versión de lanzamiento

El paquete NuGet configura el proyecto para ejecutar una herramienta adicional para las versiones de lanzamiento. Esta herramienta agrega el código nativo a los mismos archivos binarios.
Compruebe que la herramienta ha procesado los binarios puede revisar la generación de la salida para asegurarse de que incluye un mensaje como éste:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Preguntas más frecuentes

**Q. ¿Los nuevos binarios funcionan en equipos sin.NET Framework 4.7.2?**

A. Archivos binarios optimizados se beneficiarán de las mejoras cuando se ejecuta con.NET Framework 4.7.2. Los clientes que ejecutan versiones anteriores de .NET framework cargará el código MSIL no optimizada del binario.

**Q. ¿Cómo puedo enviar comentarios o informar de problemas?**

A. Informar de un problema con la herramienta de comentarios en 2017 de Visual Studio. [Obtener más información](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. ¿Cuál es el impacto de la adición de la imagen nativa en archivos binarios existentes?**

A. Los archivos binarios optimizados contienen el código nativo y administrado, con lo que los archivos finales mayor.

**Q. ¿Puedo publicar archivos binarios con esta tecnología?**

A. Esta versión incluye una licencia Go Live que puede usar hoy mismo.

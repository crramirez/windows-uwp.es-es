---
Description: Esta guía explica cómo configurar la solución de Visual Studio para optimizar los archivos binarios de aplicación con imágenes nativas.
Search.Product: eADQiWindows 10XVcnh
title: Optimice sus aplicaciones de escritorio de .NET con las imágenes nativas
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, la imagen nativa del compilador
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642880"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimice sus aplicaciones de escritorio de .NET con las imágenes nativas

> [!NOTE]
> Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Puede mejorar el tiempo de inicio de la aplicación de .NET Framework mediante la compilación previa de los archivos binarios. Puede usar esta tecnología en las aplicaciones grandes que empaquetar y distribuir a través de la Microsoft Store. En algunos casos, nos hemos observado una mejora del rendimiento del 20%. Puede aprender más sobre esta tecnología en la [Introducción técnica a](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Hemos lanzado una versión preliminar del compilador imágenes nativas como un [paquete NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Puede aplicar este paquete a cualquier aplicación de .NET Framework que tenga como destino .NET Framework versión 4.6.2 o versiones posteriores. Este paquete agrega un paso de compilación post que incluye una carga nativa para todos los archivos binarios utilizados por la aplicación. Esta carga optimizada se cargarán cuando la aplicación se ejecuta en .NET 4.7.2 y versiones posteriores mientras que las versiones anteriores todavía cargarán el código MSIL.

El [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) se incluye en el [actualización de Windows 10 de abril de 2018](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). También puede instalar esta versión de .NET Framework en los equipos que ejecutan Windows 7 o posterior y Windows Server 2008 R2 y versiones posteriores.

> [!IMPORTANT]
> Si desea generar imágenes nativas para su aplicación empaquetada por el proyecto de empaquetado de aplicaciones de Windows, asegúrese de establecer la versión mínima de la plataforma de destino del proyecto a la actualización de aniversario de Windows.

## <a name="how-to-produce-native-images"></a>Cómo generar las imágenes nativas

Siga estas instrucciones para configurar los proyectos.

1. Configurar la plataforma de destino como 4.6.2 o posterior

2. Configurar la plataforma de destino como x86 o x64 

3. Agregue los paquetes de NuGet.

4. Crear una versión de lanzamiento.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar la plataforma de destino como 4.6.2 o posterior

Para configurar el proyecto de destino es .NET Framework 4.6.2, necesitará las herramientas de desarrollo de .NET Framework 4.6.2 o versiones más recientes. Estas herramientas están disponibles a través del instalador de Visual Studio como componentes opcionales en la carga de trabajo de desarrollo de escritorio. NET:

![Instalar .NET 4.6.2 herramientas de desarrollo](images/desktop-to-uwp/install-4.6.2-devpack.png)

Como alternativa, puede obtener los paquetes de desarrollador de .NET desde: [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar la plataforma de destino como x86 o x64

El compilador de imágenes nativas optimiza el código para una plataforma determinada. Para ello, deberá configurar la aplicación para tener como destino una plataforma específica, como x86 o x64.

Si tiene varios proyectos en la solución, solo el proyecto de punto de entrada (más probable es que el proyecto que genera un archivo ejecutable) debe compilarse como x86 o x64. Archivos binarios adicionales que se hace referencia desde el proyecto principal se procesará con la arquitectura especificada en el proyecto principal, incluso si se compilan como AnyCPU.

Para configurar el proyecto:

1. Haga clic en la solución y, a continuación, seleccione **Configuration Manager**.

2. Seleccione **< nuevo... >** en el **plataforma** menú desplegable junto al nombre del proyecto que genera el archivo ejecutable.

3. En el **nueva plataforma de proyecto** diálogo cuadro, asegúrese de que el **Copiar configuración de** lista desplegable se establece en **cualquier CPU**.

![Configurar x86](images/desktop-to-uwp/configure-x86.png)

Repita este paso para `Release/x64` si desea generar x64 archivos binarios.

>[!IMPORTANT]
> Configuración AnyCPU no es compatible con el compilador de imágenes nativas.

## <a name="add-the-nuget-packages"></a>Agregue los paquetes de NuGet

El compilador de imágenes nativas se proporciona como un paquete de NuGet que necesita para agregar al proyecto de Visual Studio que genera el archivo ejecutable. Esto suele ser el proyecto de Windows Forms o WPF. Utilice este comando de PowerShell para hacerlo.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Los paquetes de versión preliminar están publicados en NuGet.org como no enumerado. No encontrarlos mediante la exploración de NuGet.org o usando la UI de administrador de paquetes en Visual Studio. Sin embargo, puede instalarlos desde la consola de administrador de paquetes y cuando se restaura desde un equipo diferente. Haremos los paquetes totalmente accesible cuando se publica la primera versión sin vista previa.

## <a name="create-a-release-build"></a>Crear una versión de lanzamiento

El paquete de NuGet configura el proyecto para ejecutar una herramienta adicional para las compilaciones de versión. Esta herramienta agrega el código nativo a los mismos binarios.
Para comprobar que la herramienta ha procesado los archivos binarios puede revisar la salida de compilación para asegurarse de que incluye un mensaje como éste:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Preguntas más frecuentes

**Q. ¿Los archivos binarios nuevos funcionan en equipos sin .NET Framework 4.7.2?**

A. Archivos binarios optimizados se beneficiarán de las mejoras, cuando se ejecuta con .NET Framework 4.7.2. Los clientes que ejecutan versiones anteriores de .NET framework cargará el código MSIL no optimizada del archivo binario.

**Q. ¿Cómo puedo proporcionar comentarios o notificar problemas?**

A. Notificar un problema mediante la herramienta de comentarios en Visual Studio 2017. [Obtener más información](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. ¿Cuál es el impacto de agregar la imagen nativa para los archivos binarios existentes?**

A. Los archivos binarios optimizados contienen el código administrado y nativo, con lo que los archivos finales mayor.

**Q. ¿Versión archivos binarios que se utiliza esta tecnología?**

A. Esta versión incluye una licencia Go Live que puede usar hoy mismo.

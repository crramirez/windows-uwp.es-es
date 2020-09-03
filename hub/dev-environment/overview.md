---
title: Configuración del entorno de desarrollo en Windows 10
description: Una guía para ayudarle a configurar el entorno de desarrollo en Windows e instalar las herramientas y los lenguajes de código que prefiera. Independientemente de si prefiere usar Python, NodeJS, VS Code, GIT, Bash, herramientas y comandos de Linux, Android Studio, tenemos una gran cobertura de herramientas nuevas, como Terminal Windows y WSL.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Configuración de Windows, el entorno de desarrollo, las herramientas de desarrollo, las rutas de acceso de desarrollo, Microsoft, Windows, el desarrollador, las recomendaciones, el rendimiento, WSL, el terminal, NodeJS, Python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 1d47161996c2136e8f983c2472b2d3754d8d3f8d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172699"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>Configuración del entorno de desarrollo en Windows 10

Esta guía le ayudará a empezar a instalar y configurar los lenguajes y las herramientas que necesita para el desarrollo en Windows o el Subsistema de Windows para Linux.

## <a name="development-paths"></a>Rutas de acceso de desarrollo

:::row:::
    :::column:::
       [![JavaScript/NodeJS](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[Introducción a NodeJS](../nodejs/index.yml)**<br>
        Instale NodeJS y obtenga la configuración del entorno de desarrollo en Windows o en el Subsistema de Windows para Linux.
    :::column-end:::
    :::column:::
       [![Python](../images/python-logo.png)](../python/index.yml)<br>
        **[Introducción a Python](../python/index.yml)**<br>
        Instale Python y configure el entorno de desarrollo en Windows o en el Subsistema de Windows para Linux.
    :::column-end:::
    :::column:::
       [![Android](../images/android-logo.png)](/windows/android)<br>
        **[Introducción a Android](/windows/android)**<br>
        Instale Android Studio, o elija una solución multiplataforma como Xamarin, React o Cordova y configure el entorno de desarrollo en Windows.
    :::column-end:::
    :::column:::
       [![Escritorio de Windows](../images/windows-logo.png)](../apps/index.yml)<br>
        **[Introducción al Escritorio de Windows](../apps/index.yml)**<br>
        Empiece a crear aplicaciones de escritorio para Windows 10 con UWP, Win32, WPF, Windows Forms, o a actualizar e implementar aplicaciones de escritorio existentes con MSIX y XAML Islands.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C/C++](../images/c-logo.png)](/cpp/)<br>
        **[Introducción a C++ y C](/cpp/)**<br>
        Comience a usar C++, C y el ensamblado para desarrollar aplicaciones, servicios y herramientas.
    :::column-end:::
    :::column:::
       [![C#](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[Introducción a C#](/dotnet/csharp/)**<br>
        Comience a compilar aplicaciones con C# y .Net Core.
    :::column-end:::
    :::column:::
       [![Azure para Java](../images/java-logo.png)](/azure/developer/java/)<br>
        **[Introducción a Java en Azure](/azure/developer/java/)**<br>
        Comience a crear aplicaciones para la nube con estos tutoriales y herramientas para desarrolladores de Java.
    :::column-end:::
    :::column:::
       [![PowerShell](../images/powershell.png)](/powershell/)<br>
        **[Introducción a PowerShell](/powershell/)**<br>
        Comience a usar la administración de la configuración y automatización de tareas entre plataformas mediante PowerShell, un shell de línea de comandos y un lenguaje de scripting.
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>Herramientas y plataformas

:::row:::
    :::column:::
       [![WSL](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[Subsistema de Windows para Linux](/windows/wsl/)**<br>
        Use su distribución de Linux favorita totalmente integrada con Windows (ya no es necesario el arranque dual).<br>
        [Instalación de WSL](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Terminal Windows](../images/terminal.png)](/windows/terminal/)<br>
        **[Terminal Windows](/windows/terminal/)**<br>
        Personalice el entorno de terminal para trabajar con varios shells de línea de comandos.
        <br>
        [Instalación de terminal](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Administrador de paquetes de Windows](../images/winget.png)](../package-manager/index.md)<br>
        **[Administrador de paquetes de Windows](../package-manager/index.md)**<br>
        Use el cliente winget.exe, un administrador de paquetes completo, con la línea de comandos para instalar aplicaciones en Windows 10.<br>
        [Instalación del Administrador de paquetes de Windows (versión preliminar pública)](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        Ajuste y optimice su experiencia con Windows para aumentar la productividad con este conjunto de utilidades de usuario avanzado.<br>
        [Instalación de PowerToys (versión preliminar pública)](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        Un editor de código fuente ligero con compatibilidad integrada para JavaScript, TypeScript, Node.js, un completo ecosistema de extensiones (C++, C#, Java, Python, PHP, Go) y tiempos de ejecución (como .NET y Unity).<br>
        [Instalación de VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        Un entorno de desarrollo integrado que puede usar para editar, depurar, compilar código y publicar aplicaciones, incluidos los compiladores, la finalización del código de IntelliSense y muchas otras características.<br>
        [Instalación de Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        Una plataforma en la nube completa para hospedar las aplicaciones existentes y optimizar nuevo desarrollo. Los servicios de Azure integran todo lo que necesita para desarrollar, probar, implementar y administrar las aplicaciones.<br>
        [Configuración de una cuenta de Azure](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        Una plataforma de desarrollo de código abierto con herramientas y bibliotecas para compilar cualquier tipo de aplicación, como web, móvil, de escritorio, juegos, IoT, de nube y microservicios.<br>
        [Instalación de .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>Ejecución de Windows y Linux

El Subsistema de Windows para Linux (WSL) permite a los desarrolladores ejecutar un sistema operativo Linux junto con Windows. Ambos comparten la misma unidad de disco duro (y pueden tener acceso a los archivos del otro), el Portapapeles admite copiar y pegar entre los dos de forma natural, no es necesario el arranque dual. WSL permite usar BASH y proporcionará el tipo de entorno más conocido para los usuarios de Mac.
- Obtenga más información en los [documentos de WSL](/windows/wsl) o a través de los [vídeos de WSL en Channel 9](https://channel9.msdn.com/Search?term=wsl&lang-en=true).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

También puede usar Terminal Windows para abrir todas las herramientas de línea de comandos favoritas en la misma ventana con varias pestañas o en varios paneles, ya sea PowerShell, el símbolo del sistema de Windows, Ubuntu, Debian, CLI de Azure, Oh-my-Zsh, Git Bash o todo lo anterior.

- Obtenga más información en los [documentos de Terminal Windows](/windows/terminal) o a través de los [vídeos de WT en Channel 9](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>Transición entre Mac y Windows

Consulte nuestra [guía sobre la transición entre un entorno de desarrollo de Mac y Windows](./mac-to-windows.md) (o el Subsistema de Windows para Linux). Puede ayudarle a ver la diferencia entre:

* [Métodos abreviados de teclado](./mac-to-windows.md#keyboard-shortcuts)
* [Métodos abreviados de trackpad](./mac-to-windows.md#trackpad-shortcuts)
* [Herramientas de terminal y shell](./mac-to-windows.md#terminal-and-shell)
* [Aplicaciones y utilidades](./mac-to-windows.md#apps-and-utilities)

![Imagen de Office](../images/flashy-office3.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Recomendaciones para mejorar el flujo de trabajo](./tips.md)
* [Historias de los desarrolladores que han cambiado de Mac a Windows](./dev-stories.md)
* [Tutoriales, cursos y ejemplos de código populares](./tutorials.md)
* [Documentación de la pila de juegos de Microsoft](/gaming/)
---
description: En este artículo se proporciona información general sobre las plantillas de proyecto y de elementos de Visual Studio para aplicaciones de Windows.
title: Plantillas de proyecto Visual Studio para aplicaciones de Windows
ms.date: 07/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 7e8500aab3c6eeaa1552dc61a95ea7404bf4d5d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174209"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>Plantillas de proyecto Visual Studio para aplicaciones de Windows

Visual Studio 2019 proporciona muchas plantillas de proyecto y de elementos que ayudan a compilar aplicaciones para dispositivos de Windows 10 mediante el uso de C\# o C++. En este tema se describen las plantillas y le ayuda a elegir una para su escenario.

* Las plantillas de proyecto incluyen archivos de proyecto, archivos de código y otros recursos que están configurados para compilar una aplicación o un componente que una aplicación puede cargar y usar.
* Las plantillas de elementos son archivos de proyecto que contienen código de uso frecuente y XAML que se pueden agregar a un proyecto para reducir el tiempo de desarrollo. Por ejemplo, puede usar una plantilla de elemento para agregar una nueva ventana, página o control a la aplicación.

## <a name="winui-templates"></a>Plantillas de WinUI

La [Biblioteca de interfaz de usuario de Windows (WinUI)](../winui/index.md) es la plataforma moderna de la interfaz de usuario (UI) nativa para aplicaciones de Windows en plataformas de escritorio (.NET y Win32 nativo) y de aplicaciones para UWP. [WinUI 3](../winui/winui3/index.md) (actualmente disponible en versión preliminar para desarrolladores) es la versión importante más reciente de WinUI y transforma WinUI en un marco completo de experiencia de usuario para aplicaciones de escritorio de Windows.

WinUI 3 incluye un paquete VSIX para Visual Studio 2019 que proporciona plantillas de proyecto y de elementos que le ayudarán a empezar a crear aplicaciones con una interfaz basada en WinUI. Para obtener más información sobre el paquete VSIX de WinUI 3 y las plantillas de proyecto que proporciona, consulte [esta sección](../winui/winui3/index.md#install-winui-3-preview-2).

> [!IMPORTANT]
> WinUI 3, incluidas las plantillas relacionadas de Visual Studio, están disponibles actualmente como versión preliminar para desarrolladores pensada para la evaluación temprana y para recopilar comentarios de la comunidad de desarrolladores. No debe usarse para aplicaciones de producción en este momento.

## <a name="uwp-templates"></a>Plantillas de UWP

Visual Studio proporciona una variedad de plantillas de proyecto para compilar aplicaciones para UWP con C# o C++. Para usar estas plantillas de proyecto, debe incluir la carga de trabajo **Desarrollo con la Plataforma universal de Windows** al instalar Visual Studio. En el caso de las plantillas de proyecto en C++, también debe incluir el componente opcional **Herramientas de Plataformas universales de Windows de C++ (v142)** para la carga de trabajo **Desarrollo con la Plataforma universal de Windows**.

### <a name="project-templates-for-c-and-uwp"></a>Plantillas de proyecto para C# y UWP

Para tener acceso a las plantillas de proyecto en C# de UWP al crear un nuevo proyecto en Visual Studio, filtre el lenguaje en **C#** , la plataforma en **Windows** y el tipo de proyecto en **UWP**.

![Plantillas de proyecto en C# de UWP](images/uwp-projects-csharp.png)

Puede usar estas plantillas de proyecto para crear aplicaciones para UWP en C#.

| Plantilla | Descripción |
|----------|-------------|
| Aplicación vacía (Windows universal) | Crea una aplicación para UWP. El proyecto generado incluye una página básica que se deriva de la clase [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) y que puede usar para empezar a crear la interfaz de usuario. |
| Aplicación de pruebas unitarias (Windows universal) | Crea un proyecto de prueba unitaria en C# de una aplicación para UWP. Para más información, consulte [Código de C# de prueba unitaria](/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app). |

Puede usar estas plantillas de proyecto para crear fragmentos de una aplicación para UWP en C#.

| Plantilla | Descripción |
|----------|-------------|
| Biblioteca de clases (Windows universal) | Crea una biblioteca de clases administradas (DLL) en C# que pueden usar otras aplicaciones para UWP escritas en código administrado. |
| Componente de Windows Runtime (Windows universal) | Crea un [componente de Windows Runtime](/windows/uwp/winrt-components/) en C# que cualquier aplicación para UWP puede consumir, independientemente del lenguaje de programación en el que esté escrita la aplicación. |
| Paquete de código opcional (Windows universal) | Crea un paquete opcional con código C# ejecutable que una aplicación puede cargar. Para más información, consulte [Paquetes opcionales con código ejecutable](/windows/msix/package/optional-packages-with-executable-code).  |

### <a name="project-templates-for-c-and-uwp"></a>Plantillas de proyecto para C++ y UWP

Hay dos tecnologías diferentes que puede usar para crear aplicaciones para UWP en C++:

* La tecnología recomendada es [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/). Se trata de una proyección del lenguaje C++ que se implementa únicamente en los archivos de encabezado y está diseñada para ofrecer acceso de primera clase a la moderna API WinRT.
* Como alternativa, puede usar el conjunto de extensiones de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) anterior. C++/CX todavía es compatible, pero se recomienda usar C++/WinRT en su lugar.

Para tener acceso a las plantillas de proyecto en C++ de UWP al crear un nuevo proyecto en Visual Studio, filtre el lenguaje en **C++** , la plataforma en **Windows** y el tipo de proyecto en **UWP**. 

> [!NOTE]
> De forma predeterminada, la carga de trabajo **Desarrollo con la Plataforma universal de Windows** en Visual Studio solo proporciona acceso a las plantillas de proyecto en C++/CX. Para tener acceso a las plantillas de proyecto en C++/WinRT, debe instalar el paquete [VSIX de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

![Plantillas de proyecto en C++ de UWP](images/uwp-projects-cpp.png)

Puede usar estas plantillas de proyecto para crear aplicaciones para UWP en C++.

| Plantilla | Descripción |
|----------|-------------|
| Aplicación vacía (C++/WinRT) | Crea una aplicación para UWP en C++/WinRT con una interfaz de usuario XAML. El proyecto generado incluye una página básica que se deriva de la clase [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) y que puede usar para empezar a crear la interfaz de usuario. |
| Aplicación principal (C++/WinRT) | Crea una aplicación para UWP en C++/WinRT que usa [CoreApplication](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) para integrarse con una variedad de marcos de interfaz de usuario en lugar de con una interfaz de usuario XAML. Para un tutorial que muestra cómo usar esta plantilla de proyecto para crear un juego sencillo que usa DirectX, consulte [Crear un juego de Plataforma universal de Windows (UWP) simple con DirectX](/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game). |
| Aplicación vacía (Windows universal - C++/CX) | Crea una aplicación para UWP en C++/WinRT con una interfaz de usuario XAML. El proyecto generado incluye una página básica que se deriva de la clase [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) de la biblioteca de WinUI y que puede usar para empezar a crear la interfaz de usuario. |
| Aplicación XAML y DirectX 11 (Windows universal - C++/CX) | Crea una aplicación para UWP que usa DirectX 11 y un elemento **SwapChainPanel** para que pueda usar controles de interfaz de usuario XAML. Para más información, vea [Plantillas de proyectos de juegos DirectX](/windows/uwp/gaming/user-interface). |
| Aplicación DirectX 11 (Windows universal - C++/CX) | Crea una aplicación para UWP que usa DirectX 11. Para más información, vea [Plantillas de proyectos de juegos DirectX](/windows/uwp/gaming/user-interface). |
| Aplicación DirectX 12 (Windows universal - C++/CX) | Crea una aplicación para UWP que usa DirectX 12. Para más información, vea [Plantillas de proyectos de juegos DirectX](/windows/uwp/gaming/user-interface). |
| Aplicación de pruebas unitarias (Windows universal - C++/CX) | Crea un proyecto de prueba unitaria en C++/CX de una aplicación para UWP. Para más información, consulte [Prueba de un archivo DLL de C++ en UWP](/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps). |

Puede usar estas plantillas de proyecto para crear fragmentos de una aplicación para UWP en C++.

| Plantilla | Descripción |
|----------|-------------|
| Componente de Windows Runtime (C++/WinRT) | Crea un [componente de Windows Runtime](/windows/uwp/winrt-components/) en C++/WinRT que cualquier aplicación para UWP puede consumir, independientemente del lenguaje de programación en el que esté escrita la aplicación. |
| Componente de Windows Runtime (Windows universal) | Crea un [componente de Windows Runtime](/windows/uwp/winrt-components/) en C++/CX que cualquier aplicación para UWP puede consumir, independientemente del lenguaje de programación en el que esté escrita la aplicación. |
| DLL (Windows universal) | Un proyecto para crear una biblioteca de vínculos dinámicos (DLL) en C++/CX que se puede usar en una aplicación para UWP. Para más información, consulte [DLL (C++/CX)](/cpp/cppcx/dlls-c-cx). |
| Biblioteca estática (Windows universal) | Un proyecto para crear una biblioteca estática (LIB) en C++/CX que se puede usar en una aplicación para UWP. Para más información, vea [Bibliotecas estáticas (C++/CX)](/cpp/cppcx/static-libraries-c-cx). |

## <a name="cwin32-templates"></a>Plantillas en C++/Win32

Visual Studio proporciona una variedad de plantillas de proyecto para crear aplicaciones de escritorio de Windows con C++ nativo y acceso directo a la API de Win32. Para usar estas plantillas de proyecto, debe incluir la carga de trabajo **Desarrollo de escritorio con C++** al instalar Visual Studio. Esta carga de trabajo incluye plantillas de proyecto para crear aplicaciones de escritorio, aplicaciones de consola y bibliotecas.

### <a name="project-templates-for-desktop-apps"></a>Plantillas de proyecto para las aplicaciones de escritorio

Para tener acceso a las plantillas de proyecto en C++ al crear un nuevo proyecto en Visual Studio, filtre el lenguaje en **C++** , la plataforma en **Windows** y el tipo de proyecto en **UWP**.

![Plantillas de proyecto de aplicación en C++ nativo](images/desktop-app-projects-cpp.png)

| Plantilla | Descripción |
|----------|----------|
| Aplicación de escritorio de Windows | Crea una aplicación de escritorio clásico de Windows con C++. Para más información, vea [Tutorial: Creación de una aplicación de escritorio tradicional de Windows](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp). |
| Asistente para escritorio de Windows | Proporciona un asistente paso a paso que puede usar para crear uno de los siguientes tipos de proyectos: una aplicación de escritorio de Windows clásica, una aplicación de consola, una biblioteca de vínculos dinámicos (DLL) o una biblioteca estática. Para obtener más información, vea [Asistente para escritorio de Windows](/cpp/windows/windows-desktop-wizard) y [Tutorial: Creación de una aplicación de escritorio tradicional de Windows](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp).         |
| Proyecto de paquete de aplicación de Windows | Crea un proyecto que puede usar para compilar una aplicación de escritorio en un [paquete MSIX](/windows/msix/overview). Con ello se proporciona una experiencia de implementación moderna, la capacidad de integrarse con las características de Windows 10 a través de extensiones de paquete y mucho más. Para más información, consulte [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).  |

### <a name="project-templates-for-console-apps"></a>Plantillas de proyecto para aplicaciones de consola

Para tener acceso a plantillas de proyecto en C++ para aplicaciones de consola, filtre el lenguaje en **C++** , la plataforma en **Windows** y el tipo de proyecto en **WinUI**.

![Plantillas de proyecto de consola en C++ nativo](images/desktop-console-projects-cpp.png)

| Plantilla | Descripción |
|----------|----------|
| Aplicación de consola de Windows (C++/WinRT) | Crea una aplicación de consola en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) sin una interfaz de usuario. Para más información, consulte [Inicio rápido de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start). Esta plantilla de proyecto requiere [VSIX en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).  |
| Aplicación de consola | Crea una aplicación de consola sin una interfaz de usuario. Para más información, vea [Tutorial: Creación de un programa de C++ estándar](/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp). |
| Proyecto vacío | Un proyecto vacío para crear una aplicación, biblioteca o DLL. Debe agregar el código o los recursos necesarios. |

### <a name="project-templates-for-libraries"></a>Plantillas de proyecto para bibliotecas

Para tener acceso a plantillas de proyecto en C++ para bibliotecas, filtre el lenguaje en **C++** , la plataforma en **Windows** y el tipo de proyecto en **Biblioteca**.

![Plantillas de proyecto de biblioteca en C++ nativo](images/desktop-library-projects-cpp.png)

| Plantilla | Descripción |
|----------|----------|
| Biblioteca de vínculos dinámicos (DLL) | Un proyecto para crear una biblioteca de vínculos dinámicos (DLL). Para más información, vea [Tutorial: Creación y uso de una biblioteca de vínculos dinámicos](/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp). |
| Biblioteca estática | Un proyecto para crear una biblioteca estática (LIB). Para más información, vea [Tutorial: Creación y uso de una biblioteca estática](/cpp/build/walkthrough-creating-and-using-a-static-library-cpp). |

### <a name="item-templates-for-native-c-and-win32"></a>Plantillas de elementos para C++ nativo y Win32

Las plantillas de proyecto en C++ incluyen muchas plantillas de elementos que puede usar para realizar tareas como agregar nuevos archivos y recursos al proyecto. Para obtener una lista completa, vea [Utilizar las plantillas Agregar nuevo elemento de Visual C++](/cpp/build/reference/using-visual-cpp-add-new-item-templates).

## <a name="net-templates"></a>Plantillas .NET

Visual Studio proporciona una variedad de plantillas de proyecto para crear aplicaciones de escritorio de Windows que usan .NET y C#. Para usar estas plantillas de proyecto, debe incluir la carga de trabajo **Desarrollo de escritorio de .NET** al instalar Visual Studio.

Para tener acceso a las plantillas de proyecto en C# de .NET al crear un nuevo proyecto en Visual Studio, filtre el lenguaje en **C#** , la plataforma en **Windows** y el tipo de proyecto en **Escritorio**.

![Plantillas de proyecto en C# de .NET](images/dotnet-projects-csharp.png)

Puede usar estas plantillas de proyecto para crear aplicaciones mediante C# y .NET.

| Plantilla | Descripción |
|----------|----------|
| Aplicación de WPF (.NET Core) | Crea una aplicación de [WPF](/dotnet/framework/wpf/) que tiene como destino [.NET Core](/dotnet/core/). Para ver un tutorial de esta plantilla de proyecto, consulte [Crear una aplicación de WPF](/visualstudio/get-started/csharp/tutorial-wpf). |
| Aplicación de WPF (.NET Framework) | Crea una aplicación de [WPF](/dotnet/framework/wpf/) que tiene como destino [.NET Framework](/dotnet/framework/). Para ver un tutorial de esta plantilla de proyecto, consulte [Tutorial: Crear su primera aplicación de WPF](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application). |
| Aplicación de Windows Forms (.NET Core) | Crea una aplicación de [Windows Forms](/dotnet/framework/winforms/) que tiene como destino [.NET Core](/dotnet/core/).  |
| Aplicación de Windows Forms (.NET Framework) | Crea una aplicación de [Windows Forms](/dotnet/framework/winforms/) que tiene como destino [.NET Framework](/dotnet/framework/). Para ver un tutorial de esta plantilla de proyecto, consulte [Creación de una aplicación de Windows Forms en Visual Studio con C#](/visualstudio/ide/create-csharp-winform-visual-studio). |
| Proyecto de paquete de aplicación de Windows | Crea un proyecto que puede usar para compilar una aplicación de WPF o Windows Forms en un [paquete MSIX](/windows/msix/overview). Con ello se proporciona una experiencia de implementación moderna, la capacidad de integrarse con las características de Windows 10 a través de extensiones de paquete y mucho más. Para más información, consulte [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). |
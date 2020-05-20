---
description: En esta guía se muestra cómo empezar a crear aplicaciones de escritorio de .NET y C++ o Win32 con una interfaz de usuario de WinUI 3.
title: Introducción a WinUI 3.0 para aplicaciones de escritorio
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580772"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>Introducción a WinUI 3.0 para aplicaciones de escritorio

La versión preliminar 1 de WinUI 3.0 presenta nuevas plantillas de proyecto que te permiten crear aplicaciones de escritorio administradas de C# y .NET y de escritorio de C++ o Win32 nativas con una interfaz de usuario totalmente basada en WinUI. Cuando se crean aplicaciones utilizando estas plantillas de proyecto, toda la interfaz de usuario de la aplicación se implementa mediante ventanas, controles y otros tipos de interfaz de usuario proporcionados por WinUI 3.0. 

La versión preliminar 1 WinUI 3.0 agrega las siguientes plantillas de proyecto **WinUI in Desktop** (WinUI en escritorio) en Visual Studio 2019:

* Aplicaciones y bibliotecas de C# que tienen como destino .NET 5:
  * Blank App, Packaged (WinUI in Desktop) (Aplicación vacía, empaquetada [WinUI en el escritorio])
  * Class Library (WinUI in Desktop) (Biblioteca de clases [WinUI en el escritorio])

* Aplicaciones Win32 o C++:
  * Blank App, Packaged (WinUI in Desktop) (Aplicación vacía, empaquetada [WinUI en el escritorio])

Las plantillas de proyecto de aplicación generan un proyecto de aplicación WinUI y un [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) que está configurado para compilar la aplicación en un [paquete MSIX](https://docs.microsoft.com/windows/msix/overview) para su implementación.

## <a name="prerequisites"></a>Requisitos previos

Para usar WinUI 3 para las plantillas de proyecto de escritorio descritas en este artículo, configura el equipo de desarrollo siguiendo estas instrucciones:

1. Asegúrate de que el equipo de desarrollo tenga instalado Windows 10, versión 1803 (compilación 17134) o una versión posterior. WinUI 3 para aplicaciones de escritorio requiere la versión 1803 del sistema operativo o una posterior.

2. Instala Visual Studio 2019, versión 16.7 versión preliminar 1. Para más información, consulta [estas instrucciones](index.md#configure-your-dev-environment).

3. Instala las versiones x64 y x86 de .NET 5 versión preliminar 4:
    * x64: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. Instala la extensión VSIX que incluye las plantillas de proyecto de la versión preliminar 1 de WinUI 3.0 para Visual Studio 2019. Para más información, consulta [estas instrucciones](index.md#visual-studio-project-templates).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Creación de una aplicación de escritorio de WinUI 3 para C# y .NET 5

1. En Visual Studio 2019, selecciona **Archivo** -> **Nuevo** -> **Proyecto**.

2. En los filtros desplegables del proyecto, selecciona **C#** , **Windows** y **WinUI**, respectivamente.

3. Selecciona el tipo de proyecto **Blank App, Packaged (WinUI in Desktop)** (Aplicación vacía, empaquetada [WinUI en el escritorio]) y haz clic en **Next** (Siguiente).

    ![Plantilla de proyecto de aplicación vacía](images/WinUI-csharp-newproject.png)

4. Escribe un nombre de proyecto, elige todas las opciones que quieras y haz clic en **Crear**.

5. En el siguiente cuadro de diálogo, establece la **versión de destino** en Windows 10, versión 1903 (compilación 18362) y la **versión mínima** en Windows 10, versión 1803 (compilación 17134) y, a continuación, haz clic en **Aceptar**.

    ![Versión de destino y mínima](images/WinUI-min-target-version.png)

6. En este momento, Visual Studio genera dos proyectos:

    * ***Nombre de proyecto* (Desktop)** : Este proyecto contiene el código de la aplicación. El archivo de código **App.xaml.cs** define una clase `Application` que representa la instancia de la aplicación y el archivo de código **MainWindow.xaml.cs** define una clase `MainWindow` que representa la ventana principal que muestra la aplicación. Estas clases se derivan de los tipos del espacio de nombres **Microsoft.UI.Xaml** proporcionados por WinUI.

        ![Proyecto de aplicación](images/WinUI-csharp-appproject.png)

    * ***Nombre de proyecto* (Package)** : Es un [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) que está configurado para compilar la aplicación en un paquete MSIX para la implementación. Este proyecto contiene el [manifiesto de paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) para la aplicación y es el proyecto de inicio de la solución de forma predeterminada.

        ![Proyecto de aplicación](images/WinUI-csharp-packageproject.png)

7. Para agregar un nuevo elemento a tu proyecto de aplicación, haz clic con el botón derecho en el nodo del proyecto ***nombre del proyecto* (Desktop)** en el **Explorador de soluciones** y selecciona **Agregar** -> **Nuevo elemento**. En el cuadro de diálogo **agregar nuevo elemento**, selecciona la pestaña **WinUI**, elige el elemento que deseas agregar y, a continuación, haz clic en **Agregar**. Puedes elegir entre los siguientes tipos de elementos:

    * **Blank Page** (Página en blanco)
    * **Blank Window** (Ventana en blanco)
    * **Custom Control** (Control personalizado)
    * **Diccionario de recursos**
    * **Resources File** (Archivo de recursos)
    * **User Control** (Control de usuario)

    ![Nuevo elemento](images/WinUI-csharp-newitem.png)

8. Compila y ejecuta la solución para confirmar que la aplicación se ejecuta sin errores.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Creación de una aplicación de escritorio WinUI 3 para C++ o Win32

1. En Visual Studio 2019, selecciona **Archivo** -> **Nuevo** -> **Proyecto**.

2. En los filtros desplegables del proyecto, selecciona **C++** , **Windows** y **WinUI**.

3. Selecciona el tipo de proyecto **Blank App, Packaged (WinUI in Desktop)** (Aplicación vacía, empaquetada [WinUI en el escritorio]) y haz clic en **Siguiente**.

    ![Plantilla de proyecto de aplicación vacía](images/WinUI-cpp-newproject.png)

4. Escribe un nombre de proyecto, elige todas las opciones que quieras y haz clic en **Crear**.

5. En el siguiente cuadro de diálogo, establece la **versión de destino** en Windows 10, versión 1903 (compilación 18362) y la **versión mínima** en Windows 10, versión 1803 (compilación 17134) y, a continuación, haz clic en **Aceptar**.

    ![Versión de destino y mínima](images/WinUI-min-target-version.png)

6. En este momento, Visual Studio genera dos proyectos:

    * ***Nombre de proyecto* (Desktop)** : Este proyecto contiene el código de la aplicación. El archivo **App.xaml** y los diversos archivos de código **App** definen una clase `Application` que representa la instancia de la aplicación y el archivo **MainWindow.xaml** y diversos archivos de código **MainWindow** definen una clase `MainWindow` que representa la ventana principal que muestra la aplicación. Estas clases se derivan de los tipos del espacio de nombres **Microsoft.UI.Xaml** proporcionados por WinUI.

        ![Proyecto de aplicación](images/WinUI-cpp-appproject.png)

    * ***Nombre de proyecto* (Package)** : Es un [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) que está configurado para compilar la aplicación en un paquete MSIX para la implementación. Este proyecto contiene el [manifiesto de paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) para la aplicación y es el proyecto de inicio de la solución de forma predeterminada.

        ![Proyecto de paquete](images/WinUI-cpp-packageproject.png)

7. Para agregar un nuevo elemento a tu proyecto de aplicación, haz clic con el botón derecho en el nodo del proyecto ***nombre del proyecto* (Desktop)** en el **Explorador de soluciones** y selecciona **Agregar** -> **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona la pestaña **WinUI**, elige el elemento que deseas agregar y, a continuación, haz clic en **Agregar**. Puedes elegir entre los siguientes tipos de elementos:

    * **Blank Page** (Página en blanco)
    * **Blank Window** (Ventana en blanco)
    * **Custom Control** (Control personalizado)
    * **Diccionario de recursos**
    * **Resources File** (Archivo de recursos)
    * **User Control** (Control de usuario)

    ![Nuevo elemento](images/WinUI-cpp-newitem.png)

8. Compila y ejecuta la solución para confirmar que la aplicación se ejecuta sin errores.

## <a name="known-issues-and-limitations"></a>Problemas y limitaciones conocidos

Para ver una lista de problemas conocidos y limitaciones de la versión preliminar 1, consulta [esta sección](index.md#preview-1-limitations-and-known-issues).

## <a name="related-topics"></a>Temas relacionados

* [WinUI 3.0](index.md)
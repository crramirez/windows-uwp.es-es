---
description: En esta guía se muestra cómo empezar a crear aplicaciones de escritorio de .NET y C++ o Win32 con una interfaz de usuario de WinUI 3.
title: Introducción a WinUI 3.0 para aplicaciones de escritorio
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 067e6a6798fbfc2633c3e356be64ecae0403cc6d
ms.sourcegitcommit: b69edc6d73370923f31df61c7e42b53de6c928ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870928"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>Introducción a WinUI 3.0 para aplicaciones de escritorio

La versión preliminar 3 de WinUI 3 incluye plantillas de proyecto que le permiten crear aplicaciones de escritorio administradas de C# o .NET Core y de escritorio de C++ o Win32 nativas con una interfaz de usuario totalmente basada en WinUI. Cuando se crean aplicaciones utilizando estas plantillas de proyecto, toda la interfaz de usuario de la aplicación se implementa mediante ventanas, controles y otros tipos de interfaz de usuario proporcionados por WinUI 3. Para obtener una lista completa de las plantillas de proyecto, vea [esta sección](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Requisitos previos

Para usar WinUI 3 para las plantillas de proyecto de escritorio descritas en este artículo, configure el equipo de desarrollo e instale la versión preliminar 3 de WinUI 3 siguiendo las instrucciones que encontrará [aquí](index.md#install-winui-3-preview-3).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Creación de una aplicación de escritorio de WinUI 3 para C# y .NET 5

1. En Visual Studio 2019, selecciona **Archivo** -> **Nuevo** -> **Proyecto**.

2. En los filtros desplegables del proyecto, selecciona **C#** , **Windows** y **WinUI**, respectivamente.

3. Selecciona el tipo de proyecto **Blank App, Packaged (WinUI in Desktop)** (Aplicación vacía, empaquetada [WinUI en el escritorio]) y haz clic en **Next** (Siguiente).

    ![Captura de pantalla del asistente para crear un nuevo proyecto con la opción Blank App Packaged (Win UI in Desktop) (Aplicación vacía empaquetada [WinUI en el escritorio]) resaltada.](images/WinUI-csharp-newproject.png)

4. Escribe un nombre de proyecto, elige todas las opciones que quieras y haz clic en **Crear**.

5. En el siguiente cuadro de diálogo, establece la **versión de destino** en Windows 10, versión 1903 (compilación 18362) y la **versión mínima** en Windows 10, versión 1803 (compilación 17134) y, a continuación, haz clic en **Aceptar**.

    ![Versión de destino y mínima](images/WinUI-min-target-version.png)

6. En este momento, Visual Studio genera dos proyectos:

    * **_Nombre de proyecto_ (Desktop)** : Este proyecto contiene el código de la aplicación. El archivo **App.xaml** y el archivo de código subyacente **App.xaml.cs** definen una clase `Application` que representa la instancia de la aplicación. El archivo **MainWindow.xaml** y el archivo de código subyacente **MainWindow.xaml.cs** definen una clase `MainWindow` que representa la ventana principal que muestra la aplicación. Estas clases se derivan de los tipos del espacio de nombres **Microsoft.UI.Xaml** proporcionados por WinUI.

        ![Captura de pantalla de Visual Studio en la que se muestra el panel Explorador de soluciones y el contenido del archivo X A M L punto C S de las ventanas principales.](images/WinUI-csharp-appproject.png)

    * **_Nombre de proyecto_ (Package)** : Es un [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) que está configurado para compilar la aplicación en un [paquete MSIX](/windows/msix/overview). Con ello se proporciona una experiencia de implementación moderna, la capacidad de integrarse con las características de Windows 10 a través de extensiones de paquete y mucho más. Este proyecto contiene el [manifiesto de paquete](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) para la aplicación y es el proyecto de inicio de la solución de forma predeterminada.

        ![Captura de pantalla de Visual Studio en la que se muestra el panel Explorador de soluciones y el contenido del archivo Package app x manifest.](images/WinUI-csharp-packageproject.png)

7. Para agregar un nuevo elemento a tu proyecto de aplicación, haz clic con el botón derecho en el nodo del proyecto **_nombre del proyecto_ (Desktop)** en el **Explorador de soluciones** y selecciona **Agregar** -> **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona la pestaña **WinUI**, elige el elemento que deseas agregar y, a continuación, haz clic en **Agregar**. Para más detalles sobre los elementos disponibles, consulte [esta sección](index.md#item-templates-for-winui-3).

    ![Captura de pantalla del cuadro de diálogo Agregar nuevo elemento con las opciones Instalado > Elementos de Visual C# > WinUI seleccionadas y la opción Página en blanco resaltada.](images/WinUI-csharp-newitem.png)

8. Compila y ejecuta la solución para confirmar que la aplicación se ejecuta sin errores.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Creación de una aplicación de escritorio WinUI 3 para C++ o Win32

1. En Visual Studio 2019, selecciona **Archivo** -> **Nuevo** -> **Proyecto**.

2. En los filtros desplegables del proyecto, selecciona **C++** , **Windows** y **WinUI**.

3. Selecciona el tipo de proyecto **Blank App, Packaged (WinUI in Desktop)** (Aplicación vacía, empaquetada [WinUI en el escritorio]) y haz clic en **Siguiente**.

    ![Otra captura de pantalla del asistente para crear un nuevo proyecto con la opción Blank App Packaged (WinUI in Desktop) (Aplicación vacía empaquetada [WinUI en el escritorio]) resaltada.](images/WinUI-cpp-newproject.png)

4. Escribe un nombre de proyecto, elige todas las opciones que quieras y haz clic en **Crear**.

5. En el siguiente cuadro de diálogo, establece la **versión de destino** en Windows 10, versión 1903 (compilación 18362) y la **versión mínima** en Windows 10, versión 1803 (compilación 17134) y, a continuación, haz clic en **Aceptar**.

    ![Versión de destino y mínima](images/WinUI-min-target-version.png)

6. En este momento, Visual Studio genera dos proyectos:

    * **_Nombre de proyecto_ (Desktop)** : Este proyecto contiene el código de la aplicación. El archivo **App.xaml** y los diversos archivos de código **App** definen una clase `Application` que representa la instancia de la aplicación y el archivo **MainWindow.xaml** y diversos archivos de código **MainWindow** definen una clase `MainWindow` que representa la ventana principal que muestra la aplicación. Estas clases se derivan de los tipos del espacio de nombres **Microsoft.UI.Xaml** proporcionados por WinUI.

        ![Captura de pantalla de Visual Studio que muestra el panel Explorador de soluciones y el contenido del archivo X A M L de las ventanas principales.](images/WinUI-cpp-appproject.png)

    * **_Nombre de proyecto_ (Package)** : Es un [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) que está configurado para compilar la aplicación en un [paquete MSIX](/windows/msix/overview). Con ello se proporciona una experiencia de implementación moderna, la capacidad de integrarse con las características de Windows 10 a través de extensiones de paquete y mucho más. Este proyecto contiene el [manifiesto de paquete](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) para la aplicación y es el proyecto de inicio de la solución de forma predeterminada.

        ![Otra captura de pantalla de Visual Studio que muestra el panel Explorador de soluciones y el contenido del archivo Package app x manifest.](images/WinUI-cpp-packageproject.png)

7. Para agregar un nuevo elemento a tu proyecto de aplicación, haz clic con el botón derecho en el nodo del proyecto **_nombre del proyecto_ (Desktop)** en el **Explorador de soluciones** y selecciona **Agregar** -> **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona la pestaña **WinUI**, elige el elemento que deseas agregar y, a continuación, haz clic en **Agregar**. Para más detalles sobre los elementos disponibles, consulte [esta sección](index.md#item-templates-for-winui-3).

    ![Nuevo elemento](images/WinUI-cpp-newitem.png)

8. Compila y ejecuta la solución para confirmar que la aplicación se ejecuta sin errores.

   > [!NOTE]
   > Solo se iniciará el proyecto empaquetado, de modo que asegúrese de que se establece uno como proyecto de inicio.

## <a name="localizing-your-winui-desktop-app"></a>Localización de la aplicación de escritorio de WinUI

A fin de admitir varios idiomas en una aplicación de escritorio de WinUI y garantizar la localización correcta del proyecto empaquetado, agregue los recursos adecuados al proyecto (vea [Recursos de la aplicación y el sistema de administración de recursos](/windows/uwp/app-resources/)) y declare cada idioma compatible en el archivo `package.appxmanifest` del proyecto. Al compilar el proyecto, los idiomas especificados se agregan al manifiesto de la aplicación generado (`AppxManifest.xml`) y se usan los recursos correspondientes.

1. Abra el archivo `package.appxmanifest` de .wapproj en un editor de texto y busque la sección siguiente:

    ```xml
    <Resources>
        <Resource Language="x-generate"/>
    </Resources>
    ```

2. Reemplace `<Resource Language="x-generate">` por `<Resource />` elementos de cada uno de los idiomas admitidos. Por ejemplo, el marcado siguiente especifica que los recursos localizados "en-US" y "es-ES" están disponibles:

    ```xml
    <Resources>
        <Resource Language="en-US"/>
        <Resource Language="es-ES"/>
    </Resources>
    ```

## <a name="known-issues-and-limitations"></a>Problemas y limitaciones conocidos

Para ver una lista de problemas conocidos y limitaciones, consulte [esta sección](index.md#preview-3-limitations-and-known-issues).

## <a name="related-topics"></a>Temas relacionados

* [Biblioteca de interfaz de usuario de Windows 3](index.md)
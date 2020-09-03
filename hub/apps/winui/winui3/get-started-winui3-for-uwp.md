---
description: En esta guía se muestra cómo empezar a crear aplicaciones para UWP con una interfaz de usuario de WinUI 3.
title: Introducción a WinUI 3 para aplicaciones para UWP
ms.date: 07/13/2020
ms.topic: article
keywords: windows 10, uwp, winui
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 88b17500527b5f52d7e020e1c37a72e932ec225b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157739"
---
# <a name="get-started-with-winui-3-for-uwp-apps"></a>Introducción a WinUI 3 para aplicaciones para UWP

WinUI 3, versión preliminar 2 presenta nuevas plantillas de proyecto que le permiten crear una aplicación para Plataforma universal de Windows (UWP) con una interfaz de usuario creada completamente en WinUI. Cuando se crean aplicaciones utilizando estas plantillas de proyecto, toda la interfaz de usuario de la aplicación se implementa mediante ventanas, controles y otros estilos proporcionados por WinUI 3. Para obtener una lista completa de las plantillas de proyecto de WinUI 3 admitidas, vea [Plantillas de proyecto para WinUI 3](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Requisitos previos

Para usar WinUI 3 para las plantillas de proyecto de UWP descritas en este artículo, configure el equipo de desarrollo e [instale WinUI 3, versión preliminar 2](index.md#install-winui-3-preview-2).

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>Creación de una "aplicación WinUI 3 en UWP" para C#

1. Cree un nuevo proyecto con Visual Studio 2019.
   - Si Visual Studio ya se está ejecutando, seleccione **Archivo** -> **Nuevo** -> **Proyecto**.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019: menú Archivo -> Nuevo -> Proyecto":::

   - En caso contrario, inicie Visual Studio y seleccione **Crear un proyecto nuevo**.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019: Crear un proyecto nuevo":::

2. En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **C#** , **Windows** y **WinUI**, respectivamente en los filtros desplegables del proyecto.

3. Seleccione el tipo de proyecto **Aplicación vacía (WinUI en UWP)** y haga clic en **Siguiente**.

:::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019: cuadro de diálogo Crear un proyecto nuevo":::

4. Escribe un nombre de proyecto, elige todas las opciones que quieras y haz clic en **Crear**.

:::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Visual Studio 2019: cuadro de diálogo Configurar el nuevo proyecto":::

5. En el siguiente cuadro de diálogo, establece la **versión de destino** en Windows 10, versión 1903 (compilación 18362) y la **versión mínima** en Windows 10, versión 1803 (compilación 17134) y, a continuación, haz clic en **Aceptar**.

:::image type="content" source="images/WinUI-min-target-version.png" alt-text="Cuadro de diálogo de las versiones de destino y mínima":::

6. Visual Studio genera el proyecto de **WinUI en UWP** con los objetos siguientes:

    - ***Nombre de proyecto* (Windows universal)** : contiene el código de aplicación. Este es el proyecto de inicio predeterminado de la solución de proyecto.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Visual Studio 2019: cuadro de diálogo Configurar el nuevo proyecto":::

    - **Package.appxmanifest**: contiene información que el sistema necesita para implementar, mostrar o actualizar la aplicación. Para más detalles, consulte [Manifiesto del paquete de la aplicación](/uwp/schemas/appxpackage/appx-package-manifest).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019: manifiesto del paquete de la aplicación":::

    - **App.xaml/App.xaml.cs**: archivos de código que definen la clase `Application`, que representa la instancia de la aplicación.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019: archivo App.xaml":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019: archivo App.xaml.cs":::

    - **MainPage.xaml/MainPage.xaml.cs**: Archivos de código que representan las ventanas principales que se muestra en la aplicación. Estas clases se derivan de los tipos del espacio de nombres **Microsoft.UI.Xaml** proporcionados por WinUI.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019: archivo MainPage.xaml":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019: archivo MainPage.xaml.cs":::

7. Para agregar un nuevo elemento al proyecto de aplicación, haga clic con el botón derecho en el nodo del proyecto ***Nombre del proyecto* (Windows universal)** en el **Explorador de soluciones** y seleccione **Agregar** -> **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona la pestaña **WinUI**, elige el elemento que deseas agregar y, a continuación, haz clic en **Agregar**. Para más detalles sobre los elementos disponibles, vea [Plantillas de elementos para WinUI 3](index.md#item-templates-for-winui-3).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019: cuadro de diálogo Agregar un nuevo elemento":::

8. Cree, implemente e inicie su aplicación para ver su aspecto.

    1. Puedes depurar la aplicación en el equipo local, en un simulador, un emulador o en un dispositivo remoto. Seleccione el dispositivo de destino en la lista desplegable.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Visual Studio 2019: menú Dispositivo de destino":::

    1. Presione F5, haga clic en el botón **Compilar** o seleccione **Depurar-> Iniciar depuración** para compilar y ejecutar la solución y confirmar que la aplicación se ejecuta sin errores.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Visual Studio 2019: menú Dispositivo de destino":::

## <a name="known-issues-and-limitations"></a>Problemas y limitaciones conocidos

Para ver una lista de problemas conocidos y limitaciones, consulte [esta sección](index.md#preview-2-limitations-and-known-issues).

## <a name="related-topics"></a>Temas relacionados

- [WinUI 3](index.md)
- [Crea tu primera aplicación.](/windows/uwp/get-started/your-first-app)
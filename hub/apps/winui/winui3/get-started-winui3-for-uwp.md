---
description: En esta guía se muestra cómo empezar a crear aplicaciones para UWP con una interfaz de usuario de WinUI 3.
title: Introducción a WinUI 3 para aplicaciones para UWP
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10, uwp, winui
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 5da4944b38fc764eb11bdc2f6daed0cab54ea445
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691623"
---
# <a name="get-started-with-winui-3-for-uwp-apps"></a>Introducción a WinUI 3 para aplicaciones para UWP

La versión preliminar 3 de WinUI 3 incluye nuevas plantillas de proyecto que le permiten crear una aplicación para la Plataforma universal de Windows (UWP) con una interfaz de usuario creada completamente en WinUI. Cuando se crean aplicaciones utilizando estas plantillas de proyecto, toda la interfaz de usuario de la aplicación se implementa mediante ventanas, controles y otros estilos proporcionados por WinUI 3. Para obtener una lista completa de las plantillas de proyecto de WinUI 3 admitidas, vea [Plantillas de proyecto para WinUI 3](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Requisitos previos

Para usar WinUI 3 para las plantillas de proyecto de UWP descritas en este artículo, configure el equipo de desarrollo e [instale WinUI 3, versión preliminar 3](index.md#install-winui-3-preview-3).

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

:::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Captura de pantalla del cuadro de diálogo Configurar el nuevo proyecto con el cuadro de texto Ubicación y la opción Crear resaltada.":::

5. En el siguiente cuadro de diálogo, establece la **versión de destino** en Windows 10, versión 1903 (compilación 18362) y la **versión mínima** en Windows 10, versión 1803 (compilación 17134) y, a continuación, haz clic en **Aceptar**.

:::image type="content" source="images/WinUI-min-target-version.png" alt-text="Cuadro de diálogo de las versiones de destino y mínima":::

6. Visual Studio genera el proyecto de **WinUI en UWP** con los objetos siguientes:

    - **_Nombre de proyecto_ (Windows universal)** : contiene el código de aplicación. Este es el proyecto de inicio predeterminado de la solución de proyecto.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Captura de pantalla del panel Explorador de soluciones con la solución Windows Universal resaltada.":::

    - **Package.appxmanifest**: contiene información que el sistema necesita para implementar, mostrar o actualizar la aplicación. Para más detalles, consulte [Manifiesto del paquete de la aplicación](/uwp/schemas/appxpackage/appx-package-manifest).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019: manifiesto del paquete de la aplicación":::

    - **App.xaml/App.xaml.cs**: archivos de código que definen la clase `Application`, que representa la instancia de la aplicación.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019: archivo App.xaml":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019: archivo App.xaml.cs":::

    - **MainPage.xaml/MainPage.xaml.cs**: Archivos de código que representan las ventanas principales que se muestra en la aplicación. Estas clases se derivan de los tipos del espacio de nombres **Microsoft.UI.Xaml** proporcionados por WinUI.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019: archivo MainPage.xaml":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019: archivo MainPage.xaml.cs":::

7. Para agregar un nuevo elemento al proyecto de aplicación, haga clic con el botón derecho en el nodo del proyecto **_Nombre del proyecto_ (Windows universal)** en el **Explorador de soluciones** y seleccione **Agregar** -> **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona la pestaña **WinUI**, elige el elemento que deseas agregar y, a continuación, haz clic en **Agregar**. Para más detalles sobre los elementos disponibles, vea [Plantillas de elementos para WinUI 3](index.md#item-templates-for-winui-3).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019: cuadro de diálogo Agregar un nuevo elemento":::

8. Cree, implemente e inicie su aplicación para ver su aspecto.

    1. Puedes depurar la aplicación en el equipo local, en un simulador, un emulador o en un dispositivo remoto. Seleccione el dispositivo de destino en la lista desplegable.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Captura de pantalla de la lista desplegable Máquina local.":::

    1. Presione F5, haga clic en el botón **Compilar** o seleccione **Depurar-> Iniciar depuración** para compilar y ejecutar la solución y confirmar que la aplicación se ejecuta sin errores.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Captura de pantalla de la aplicación en ejecución que muestra el botón Click Me.":::

## <a name="known-issues-and-limitations"></a>Problemas y limitaciones conocidos

Para ver una lista de problemas conocidos y limitaciones, consulte [esta sección](index.md#preview-3-limitations-and-known-issues).

## <a name="related-topics"></a>Temas relacionados

- [WinUI 3](index.md)
- [Crea tu primera aplicación.](/windows/uwp/get-started/your-first-app)
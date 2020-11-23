---
title: Introducción a la biblioteca de interfaz de usuario de Windows
description: Cómo instalar la biblioteca de interfaz de usuario de Windows.
ms.topic: reference
ms.date: 07/15/2020
keywords: windows 10, uwp, sdk del kit de herramientas
ms.openlocfilehash: e3b0daae3f053daabe153f8a0058953ff76e3b90
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691573"
---
# <a name="getting-started-with-the-windows-ui-2x-library"></a>Introducción a la biblioteca de interfaz de usuario de Windows 2.x

[WinUI 2.4](release-notes/winui-2.4.md) es la versión estable más reciente de WinUI y debe usarse para las aplicaciones en producción.

La biblioteca está disponibles como un paquete NuGet que se puede agregar a cualquier proyecto de Visual Studio nuevo o existente.

> [!NOTE]
> Para obtener información sobre la prueba de versiones preliminares anticipadas de WinUI 3, consulte [Biblioteca de interfaz de usuario de Windows 3, versión preliminar 3 (noviembre de 2020)](../winui3/index.md).

## <a name="download-and-install-the-windows-ui-library"></a>Descarga e instalación de la biblioteca de interfaz de usuario de Windows

1. Descargue [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) y asegúrese de elegir la carga de trabajo de **desarrollo de la Plataforma universal de Windows** en el instalador de Visual Studio.

2. Abra un proyecto existente, o cree uno nuevo mediante la plantilla Aplicación vacía en Visual C# -> Windows -> Universal o la plantilla adecuada para su proyección de idioma.  

    > [!IMPORTANT]
    > Para usar WinUI 2.4, debes establecer TargetPlatformVersion > = 10.0.18362.0 y TargetPlatformMinVersion > = 10.0.15063.0 en las propiedades del proyecto.

3. En el panel Explorador de soluciones, haga clic con el botón derecho en el nombre del proyecto y seleccione **Administrar paquetes NuGet**. Seleccione la pestaña **Examinar** y busque **Microsoft.UI.Xaml** o **WinUI**. A continuación, elija los [paquetes NuGet de la biblioteca de interfaz de usuario de Windows](nuget-packages.md) que quiere usar.
El paquete **Microsoft.UI.Xaml** contiene características y controles de Fluent adecuados para todas las aplicaciones.  
De manera opcional, puede activar "Incluir versión preliminar" para ver las versiones preliminares más recientes que incluyen nuevas características experimentales.

    ![Captura de pantalla del panel Explorador de soluciones con el proyecto en el que se hizo clic con el botón derecho y la opción Administrar paquetes NuGet resaltada.](images/ManageNugetPackages.png "Imagen de administración de paquetes NuGet")

    ![Captura de pantalla del cuadro de diálogo Administrador de paquetes NuGet que muestra la pestaña Examinar con WinUI en el campo de búsqueda.](images/NugetPackages.png)

4. Agregue los recursos de tema de la interfaz de usuario de Windows UI (WinUI) a sus recursos de App.xaml. Tiene dos maneras de hacerlo, según si tiene recursos de aplicaciones adicionales.

    a. Si no tiene otros recursos de aplicaciones, agregue `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` a Application.Resources:

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. En cambio, si tiene más de un conjunto de recursos de aplicaciones, agregue `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` a Application.Resources.MergedDictionaries:

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > El orden de los recursos agregados a ResourceDictionary afecta al orden en que se aplican. El diccionario `XamlControlsResources` invalida muchas claves de recursos predeterminadas y, por tanto, se debe agregar primero a `Application.Resources` para que no invalide ningún otro estilo o recurso personalizado en su aplicación. Para obtener más información sobre la carga de recursos, consulte [Referencias a ResourceDictionary y a los recursos XAML](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

5. Agregue una referencia al kit de herramientas a las páginas XAML y a sus páginas de código subyacente.

    * Agregue una referencia en la parte superior de su página XAML.

        ```xaml
        xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
        ```

    * En su código (si quiere usar los nombres de tipos sin calificarlos), puede agregar una directiva using.

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>Pasos adicionales para un proyecto de C++/WinRT

Al agregar un paquete NuGet a un proyecto de C++/WinRT, las herramientas generan un conjunto de encabezados de proyección en la carpeta `\Generated Files\winrt` del proyecto. Para incorporar esos archivos de encabezados al proyecto, de modo que se resuelvan las referencias a esos nuevos tipos, puede dirigirse al archivo de encabezado precompilado (por lo general, `pch.h`) e incluirlos. A continuación se muestra un ejemplo que incluye los archivos de encabezado generados para el paquete **Microsoft.UI.Xaml**.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

Para obtener un tutorial paso a paso completo sobre cómo agregar compatibilidad sencilla a la biblioteca de interfaz de usuario C++de Windows en un proyecto de/WinRT C++, vea [un ejemplo sencillo de la biblioteca de interfaz de usuario de Windows/WinRT](/windows/uwp/cpp-and-winrt-apis/simple-winui-example).

## <a name="contributing-to-the-windows-ui-library"></a>Contribución a la biblioteca de interfaz de usuario de Windows

WinUI es un proyecto de código abierto que se hospeda en GitHub.

Agradecemos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui).

## <a name="other-resources"></a>Otros recursos

Si no está familiarizado con UWP, le recomendamos que visite las páginas de [Introducción al desarrollo para UWP](https://developer.microsoft.com/windows/getstarted) en el portal para desarrolladores.
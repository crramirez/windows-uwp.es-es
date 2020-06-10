---
description: Este tema te ayudará durante el proceso de adición de compatibilidad simple con WinUI en un proyecto de C++/WinRT.
title: Un ejemplo sencillo de biblioteca de interfaz de usuario de Windows para C++/WinRT
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, biblioteca de interfaz de usuario de Windows, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 8242055e3c448e2720226859f2ea10e1ae54794f
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437139"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Un ejemplo sencillo de biblioteca de interfaz de usuario de Windows para C++/WinRT

Este tema te ayudará durante el proceso de adición de compatibilidad simple para la [biblioteca de la interfaz de usuario de Windows (WinUI)](https://github.com/Microsoft/microsoft-ui-xaml) a tu proyecto de C++/WinRT. Casualmente, la biblioteca de la interfaz de usuario de Windows está escrita en C++/WinRT.

> [!NOTE]
> El kit de herramientas de la biblioteca de UI de Windows (WinUI) está disponible como paquetes NuGet que puedes agregar a cualquier proyecto nuevo o existente mediante Visual Studio, como veremos en este tema. Para más información sobre el contexto, la configuración y la compatibilidad, consulta [Getting started with the Windows UI Library](/uwp/toolkits/winui/getting-started) (Introducción a la biblioteca de interfaz de usuario de Windows).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Creación de una aplicación en blanco (HelloWinUICppWinRT)

En Visual Studio, crea un nuevo proyecto con la plantilla de proyecto **Aplicación en blanco (C++/WinRT)** . Asegúrate de que usas la plantilla de **(C++/WinRT)** , y no la de **(Windows universal)** .

Establece el nombre del nuevo proyecto en *HelloWinUICppWinRT* y, para que la estructura de carpetas coincida con el tutorial, desactiva **Colocar la solución y el proyecto en el mismo directorio**.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Instala el paquete NuGet Microsoft.UI.Xaml

Haz clic en **Proyecto** \> **Administrar paquetes NuGet...** \> **Examinar**, escribe o pega **Microsoft.UI.Xaml** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete en el proyecto (también verás una notificación del contrato de licencia). Ten cuidado de instalar solo el paquete **Microsoft.UI.Xaml** y no **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Declarar recursos de la aplicación de WinUI

Abre `App.xaml` y pega el marcado siguiente entre las etiquetas de apertura y cierre existentes de **Application**.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Agregar un control de WinUI a MainPage

A continuación, abre `MainPage.xaml`. En la etiqueta de apertura existente de **Página** hay algunas declaraciones de espacio de nombres XML. Agrega la declaración de espacio de nombres XML `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`. Luego, pega el marcado siguiente entre las etiquetas de apertura y cierre existentes de **Page**, sobrescribiendo el elemento **StackPanel** existente.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-pchh-as-necessary"></a>Editar el archivo pch.h, según sea necesario

Al agregar un paquete NuGet a un proyecto de C++/WinRT (como el paquete **Microsoft.UI.Xaml** que agregaste antes) y compilar el proyecto, las herramientas generan un conjunto de encabezados de proyección en la carpeta `\Generated Files\winrt` del proyecto. Si has seguido el tutorial, ahora tendrás una carpeta `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. Para incorporar esos archivos de encabezados al proyecto, de modo que se resuelvan las referencias a esos nuevos tipos, puede dirigirse al archivo de encabezado precompilado (por lo general, `pch.h`) e incluirlos.

Solo tiene que incluir los encabezados que corresponden a los tipos que usa. A continuación se muestra un ejemplo que incluye todos los archivos de encabezado generados para el paquete **Microsoft.UI.Xaml**.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>Editar MainPage.cpp

En `MainPage.cpp`, elimina el código dentro de la implementación de **MainPage::ClickHandler**, ya que *myButton* ya no está en el marcado XAML.

Ahora puedes compilar y ejecutar el proyecto.

![Captura de pantalla sencilla de la biblioteca de interfaz de usuario de Windows para C++/WinRT](images/winui.png)

## <a name="related-topics"></a>Temas relacionados
* [Getting started with the Windows UI Library](/uwp/toolkits/winui/getting-started) (Introducción a la biblioteca de UI de Windows)
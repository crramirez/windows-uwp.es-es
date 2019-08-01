---
description: Este tema te ayudará durante el proceso de adición de compatibilidad simple con WinUI en un proyecto de C++/WinRT.
title: Un ejemplo sencillo de biblioteca de interfaz de usuario de Windows para C++/WinRT
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, biblioteca de interfaz de usuario de Windows, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 5d0066abb2a6eb15f1d31aaf930ed2c0f0faf81a
ms.sourcegitcommit: 4e74c920f1fef507c5cdf874975003702d37bcbb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372711"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Un ejemplo sencillo de biblioteca de interfaz de usuario de Windows para C++/WinRT

Este tema te ayudará durante el proceso de adición de compatibilidad simple para la [biblioteca de la interfaz de usuario de Windows (WinUI)](https://github.com/Microsoft/microsoft-ui-xaml) a tu proyecto de C++/WinRT. Casualmente, la biblioteca de la interfaz de usuario de Windows está escrita en C++/WinRT.

> [!NOTE]
> El kit de herramientas de la biblioteca de UI de Windows (WinUI) está disponible como paquetes NuGet que puedes agregar a cualquier proyecto nuevo o existente mediante Visual Studio, como veremos en este tema. Para más información sobre el contexto, la configuración y la compatibilidad, consulta [Getting started with the Windows UI Library](/uwp/toolkits/winui/getting-started) (Introducción a la biblioteca de interfaz de usuario de Windows).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Creación de una aplicación en blanco (HelloWinUICppWinRT)

En Visual Studio, crea un nuevo proyecto con la plantilla de proyecto **Blank App (C++/WinRT)** (Aplicación en blanco [C++/WinRT]) y asígnale el nombre *HelloWinUICppWinRT*.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Instala el paquete NuGet Microsoft.UI.Xaml

Haz clic en **Proyecto** \> **Administrar paquetes NuGet...** \> **Examinar**, escribe o pega **Microsoft.UI.Xaml** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete del proyecto (también verás una notificación de acuerdo de licencia). Ten cuidado de instalar solo el paquete **Microsoft.UI.Xaml** y no **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Declarar recursos de la aplicación de WinUI

Abre `App.xaml` y pega el marcado siguiente entre las etiquetas de apertura y cierre existentes de **Application**.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Agregar un control de WinUI a MainPage

A continuación, abre `MainPage.xaml`. En la etiqueta de apertura existente de **Application** hay algunas declaraciones de espacio de nombres XML. Agrega la declaración de espacio de nombres XML `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`. Luego, pega el marcado siguiente entre las etiquetas de apertura y cierre existentes de **Page**, sobrescribiendo el elemento **StackPanel** existente.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>Editar MainPage.h y .cpp según sea necesario

Al agregar un paquete NuGet a un proyecto de C++/WinRT (como el paquete **Microsoft.UI.Xaml** que agregaste antes), las herramientas generan un conjunto de encabezados de proyección en la carpeta `\Generated Files\winrt` del proyecto. Para incorporar esos archivos de encabezados al proyecto, de modo que se resuelvan las referencias a esos nuevos tipos, deberás incluirlos.

Por lo tanto, en `MainPage.h`, edita los elementos que has incluido para que tengan el mismo aspecto que los que aparecen en la lista siguiente. Si quisieras usar WinUI desde más de una página XAML, podrías ir al archivo de encabezado precompilado (normalmente, `pch.h`) e incluir los elementos allí.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Y, por último, en `MainPage.cpp`, elimina el código dentro de la implementación de **MainPage::ClickHandler**, ya que *myButton* ya no está en el marcado XAML.

Ahora puedes compilar y ejecutar el proyecto.

![Captura de pantalla sencilla de la biblioteca de interfaz de usuario de Windows para C++/WinRT](images/winui.png)

## <a name="related-topics"></a>Temas relacionados
* [Getting started with the Windows UI Library](/uwp/toolkits/winui/getting-started) (Introducción a la biblioteca de UI de Windows)
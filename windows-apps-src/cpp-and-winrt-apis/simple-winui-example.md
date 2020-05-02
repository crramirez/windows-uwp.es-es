---
description: Este tema te ayudará durante el proceso de adición de compatibilidad simple con WinUI en un proyecto de C++/WinRT.
title: Un ejemplo sencillo de biblioteca de interfaz de usuario de Windows para C++/WinRT
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, biblioteca de interfaz de usuario de Windows, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 0dce8e7ea08b18921f228b3da2e679a9edb02228
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "79200983"
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

## <a name="edit-mainpagecpp-and-h-as-necessary"></a>Editar MainPage.cpp y .h según sea necesario

En `MainPage.cpp`, elimina el código dentro de la implementación de **MainPage::ClickHandler**, ya que *myButton* ya no está en el marcado XAML.

En `MainPage.h`, edita los elementos que has incluido para que tengan el mismo aspecto que los que aparecen en la lista siguiente.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Ahora, compila el proyecto.

Al agregar un paquete NuGet a un proyecto de C++/WinRT (como el paquete **Microsoft.UI.Xaml** que agregaste antes) y compilar el proyecto, las herramientas generan un conjunto de encabezados de proyección en la carpeta `\Generated Files\winrt` del proyecto. Si has seguido el tutorial, ahora tendrás una carpeta `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. La edición que has realizado en `MainPage.h` anterior hace que esos archivos de encabezado de proyección de WinUI se vuelvan visibles para **MainPage**. Y eso es necesario para que se resuelva la referencia en **MainPage** en el tipo **Microsoft::UI::Xaml::Controls::NavigationView**.

> [!IMPORTANT]
> En una aplicación real, querrás que los archivos de encabezado de proyección de WinUI estén visibles para *todas* las páginas XAML del proyecto, no solo para **MainPage**. En ese caso, moverías las inclusiones de los dos archivos de encabezado de proyección de WinUI al archivo de encabezado precompilado (normalmente, `pch.h`). Luego, se resolverán las referencias en cualquier lugar del proyecto en los tipos del paquete NuGet. En el caso de una aplicación mínima de una página, como la que se está compilando en este tutorial, no es necesario usar `pch.h`, y es adecuado incluir los encabezados en `MainPage.h`.

Ahora puedes ejecutar el proyecto.

![Captura de pantalla sencilla de la biblioteca de interfaz de usuario de Windows para C++/WinRT](images/winui.png)

## <a name="related-topics"></a>Temas relacionados
* [Getting started with the Windows UI Library](/uwp/toolkits/winui/getting-started) (Introducción a la biblioteca de UI de Windows)
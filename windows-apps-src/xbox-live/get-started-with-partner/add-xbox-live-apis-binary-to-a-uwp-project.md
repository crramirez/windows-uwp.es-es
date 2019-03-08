---
title: Agregar Xbox Live las API de binario a un proyecto UWP
description: Aprenda a usar NuGet para agregar el paquete binario de la API de Xbox Live a su proyecto UWP.
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593950"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>Agregar paquete binario de API de Xbox Live a su proyecto UWP

## <a name="requirements"></a>Requisitos

2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**. Con Visual Studio 2015 Update 3 o posterior, se pueden crear aplicaciones UWP. Se recomienda que utilice la versión más reciente de Visual Studio para desarrolladores y actualizaciones de seguridad.
4. **[SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** o una versión posterior.

## <a name="add-the-binary-package-via-nuget"></a>Agregue el paquete binario a través de NuGet

Para usar la API de Xbox Live desde el proyecto, puede agregar referencias a los archivos binarios mediante paquetes de NuGet o agregando el origen de la API. Agregar paquetes de NuGet agiliza la compilación mientras que agregar el origen hace que la depuración sea más sencilla. En este artículo le guiará a través del uso de paquetes de NuGet. Si desea usar el código fuente, consulte [compilar la Xbox Live las API de origen en su proyecto de UWP](add-xbox-live-apis-source-to-a-uwp-project.md).

La API de servicios de Xbox incluye versiones para UWP y XDK y de C++ y WinRT y ha su espacio de nombres estructurados como **Microsoft.Xbox.Live.SDK.*. UWP** y **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** está dirigido a desarrolladores que crean un juego para UWP, que puede ejecutarse en PC, la Xbox One o de Windows Phone.
2. **XboxOneXDK** es para ID@Xbox y administra los desarrolladores que usan el XDK una Xbox.
3. El SDK de C++ puede usarse para motores de juegos de C++, mientras que el SDK de WinRT es para los motores de juegos escritos en C++, C#, o JavaScript.
4. Al usar WinRT con un motor de C++, debe usar C++ / c++ / CX que utiliza sombreros (^). C++ es la API que se recomienda que se usará para los motores de juegos de C++.  

> [!TIP]
> Puede leer más sobre la ejecución de UWP en Xbox One en [Introducción al desarrollo de aplicaciones para UWP en Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Puede agregar el paquete de NuGet del SDK de Xbox Live:

1. En Visual Studio, vaya a **herramientas** > **Administrador de paquetes de NuGet** > **administrar paquetes de NuGet para la solución...** .
2. En el Administrador de paquetes de NuGet, haga clic en **examinar** y escriba **Xbox.Live.SDK** en el cuadro de búsqueda.
3. Seleccione la versión de Xbox Live SDK que desea usar en la lista de la izquierda.
3. En el lado derecho de la ventana, active la casilla situada junto a su proyecto y haga clic en **instalar**.

> [!NOTE]
> Los desarrolladores de programas de creadores de Xbox Live deben usar una de las versiones UWP de Xbox Live SDK ya no se admite el XDK.

![Agregar XBL a través de NuGet](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> Para `Microsoft.Xbox.Live.SDK.Cpp.*` en función de los proyectos, asegúrese de incluir el encabezado `#include <xsapi\services.h>` en código fuente del proyecto.

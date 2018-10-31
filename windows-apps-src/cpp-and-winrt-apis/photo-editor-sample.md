---
author: JoshuaPartlow
description: Photo Editor es una aplicación de ejemplo para UWP que muestra el desarrollo con proyección de lenguaje de C++/WinRT. La aplicación de ejemplo permite recuperar fotos desde la biblioteca Imágenes y, a continuación, modificar la imagen seleccionada con distintos efectos fotográficos.
title: Aplicación de ejemplo de C++/WinRT de Photo Editor
ms.author: wdg-dev-content
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, muestra, aplicación, foto, editor
ms.localizationpriority: medium
ms.openlocfilehash: 60bfcd79ed2d659aff8d435bd397df05eb45af72
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5860547"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Aplicación de ejemplo de C++/WinRT de Photo Editor
Puedes clonar o descargar la aplicación de ejemplo desde el repositorio de GitHub [aplicación de ejemplo C++/WinRT Photo Editor](https://github.com/Microsoft/Windows-appsample-photo-editor).

La aplicación Photo Editor es una aplicación de ejemplo para la Plataforma universal de Windows (UWP) que muestra el desarrollo con proyección de lenguaje de [C++/WinRT](intro-to-using-cpp-with-winrt.md). La aplicación de ejemplo permite recuperar fotos desde la biblioteca **Imágenes** y, a continuación, modificar la imagen seleccionada con distintos efectos fotográficos. En el código fuente del ejemplo, verás varios procedimientos comunes, como el [enlace de datos](binding-property.md) y [acciones asincrónicas y operaciones](concurrency.md) llevadas a cabo con la proyección de C++/WinRT. Estas son algunas de las funciones específicas que se muestran en el ejemplo.
    
- Uso de las bibliotecas y la sintaxis de C++17 estándar con las API de Windows Runtime (WinRT).
- Uso de las corrutinas, incluido el uso de co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) y [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_).
- Creación y uso de los tipos de implementación y los tipos proyectados de la clase de Windows Runtime (clase en tiempo de ejecución). Para obtener más información acerca de estos términos, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT](author-apis.md).
- [Control de eventos](handle-events.md), incluido el uso de tokens de eventos de revocación automática.
- Uso del paquete de NuGet Win2D externo y [Windows::UI::Composition](/uwp/api/windows.ui.composition), para efectos de imagen.
- Enlace de datos XAML, incluida la [extensión de marcado {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension).
- Personalización de la interfaz de usuario y estilo de XAML, incluidas las [animaciones conectadas](../design/motion/connected-animation.md).

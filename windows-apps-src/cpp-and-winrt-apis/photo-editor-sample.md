---
description: Photo Editor es una aplicación de ejemplo para UWP que muestra el desarrollo con la proyección de lenguaje de C++/WinRT. La aplicación de ejemplo permite recuperar fotos de la biblioteca Imágenes y, después, editar la imagen seleccionada con distintos efectos fotográficos.
title: Aplicación de ejemplo de C++/WinRT de Photo Editor
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, ejemplo, aplicación, foto, editor
ms.localizationpriority: medium
ms.openlocfilehash: 535ae3ce60eab5f5cef4654c7a5142e01ce5cf3e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219998"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Aplicación de ejemplo de C++/WinRT de Photo Editor

> [!NOTE]
> El ejemplo está dirigido y probado para Windows 10, versión 1903 (10.0; Compilación 18362) y Visual Studio 2019. Si lo prefieres, puedes usar las propiedades del proyecto para redestinar los proyectos para Windows 10, versión 1809 (10.0; Compilación 17763), y/o abrir el ejemplo con Visual Studio 2017.

Para clonar o descargar la aplicación de ejemplo, consulta [Aplicación de ejemplo de C++/WinRT de Photo Editor](/samples/microsoft/windows-appsample-photo-editor/photo-editor-cwinrt-sample-application/) en la galería de ejemplos de código.

La aplicación Photo Editor es una aplicación de ejemplo para la Plataforma universal de Windows (UWP) que muestra el desarrollo con proyección de lenguaje de [C++/WinRT](intro-to-using-cpp-with-winrt.md). La aplicación de ejemplo permite recuperar fotos de la biblioteca **Imágenes** y, a continuación, modificar la imagen seleccionada con distintos efectos fotográficos. En el código fuente del ejemplo, verás varios procedimientos comunes &mdash;como el [enlace de datos](binding-property.md) y [acciones asincrónicas y operaciones](concurrency.md)&mdash; llevadas a cabo con la proyección de C++/WinRT. Estas son algunas de las características específicas que se muestran en el ejemplo.

- Uso de las bibliotecas y la sintaxis de C++17 estándar con las API de Windows Runtime (WinRT).
- Uso de las corrutinas, incluido el uso de co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) y [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1).
- Creación y uso de los tipos de implementación y los tipos proyectados de la clase de Windows Runtime (clase de tiempo de ejecución). Para obtener más información acerca de estos términos, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT](author-apis.md).
- [Control de eventos](handle-events.md), incluido el uso de tokens de eventos de revocación automática.
- Uso del paquete de NuGet Win2D externo y [Windows::UI::Composition](/uwp/api/windows.ui.composition), para efectos de imagen.
- Enlace de datos XAML, incluida la [extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md).
- Personalización de la interfaz de usuario y estilo de XAML, incluidas las [animaciones conectadas](../design/motion/connected-animation.md).

Consulte también [¿Dónde puedo encontrar aplicaciones de ejemplo de C++/WinRT?](./faq.md#where-can-i-find-cwinrt-sample-apps).
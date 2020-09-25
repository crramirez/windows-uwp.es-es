---
description: Aprende a compilar aplicaciones de escritorio para PC Windows, incluido cómo elegir la plataforma de aplicaciones adecuada para las nuevas aplicaciones y cómo modernizar las aplicaciones existentes para Windows 10.
title: Compilación de aplicaciones de escritorio para PC Windows
ms.topic: article
ms.date: 9/10/2020
keywords: windows win32, desarrollo de escritorio
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 34b860916b90d1adc8ae9e7ae8c8ac7950125ce7
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216748"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Compilación de aplicaciones de escritorio para PC Windows

En este artículo se proporciona la información necesaria para empezar a compilar aplicaciones de escritorio para Windows o actualizar las aplicaciones de escritorio existentes para adoptar las experiencias más recientes en Windows 10.

## <a name="platforms-for-desktop-apps"></a>Plataformas para aplicaciones de escritorio

Hay cuatro plataformas principales para compilar aplicaciones de escritorio para equipos Windows. Cada plataforma proporciona un modelo de aplicación que define el ciclo de vida de la aplicación, un marco de trabajo completo de la interfaz de usuario y un conjunto de controles de interfaz de usuario que permiten crear aplicaciones de escritorio como Word, Excel y Photoshop, y el acceso a un conjunto completo de API nativas o administradas para usar las características de Windows. Para obtener una comparación detallada de estas plataformas junto con los recursos adicionales de cada una, consulta [Elección de la plataforma de aplicaciones](choose-your-platform.md).

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>Plataforma</th>
<th>Descripción</th>
<th>Documentos y recursos</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="/windows/uwp/">Plataforma universal de Windows (UWP)</a></td>
<td><p>La plataforma de vanguardia para juegos y aplicaciones de Windows 10. Puedes compilar aplicaciones para UWP que usen exclusivamente los controles y las API de UWP, o bien usar los controles y las API de UWP en aplicaciones de escritorio compiladas con una de las otras plataformas.</p></td>
<td><a href="/windows/uwp/get-started/">Introducción</a><br/><a href="/uwp/">Referencia de las API</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">Ejemplos</a></td>
</tr>
<tr class="even">
<td><a href="/windows/win32/">C++/Win32</a></td>
<td><p>Plataforma preferida para aplicaciones Windows nativas que requieren acceso directo a Windows y al hardware.</p></td>
<td><a href="/windows/win32/desktop-programming/">Introducción</a><br/><a href="/windows/win32/apiindex/windows-api-list/">Referencia de las API</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">Ejemplos</a></td>
</tr>
<tr class="odd">
<td><a href="/dotnet/framework/wpf/">WPF</a></td>
<td><p>Plataforma basada en .NET establecida para aplicaciones Windows administradas con gráficos abundantes que cuenta con un modelo de interfaz de usuario XAML. Estas aplicaciones pueden estar dirigidas a <a href="/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> o a la versión completa de .NET Framework.</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">Introducción</a><br/><a href="/dotnet/api/index">Referencia de API (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">Ejemplos</a></td>
</tr>
<tr class="even">
<td><a href="/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>Plataforma basada en .NET que está diseñada para las aplicaciones de línea de negocio administradas con un modelo de interfaz de usuario ligera. Estas aplicaciones pueden estar dirigidas a <a href="/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> o a la versión completa de .NET Framework.</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">Introducción</a><br/><a href="/dotnet/api/index">Referencia de API (.NET)</a></td>
</tr>
</tbody>
</table>

> [!NOTE]
> En Windows 10, todas estas plataformas también admiten el uso de la biblioteca de interfaz de usuario de Windows (WinUI) para crear interfaces de usuario. Para más información sobre WinUI para aplicaciones de escritorio, consulta [esta sección](choose-your-platform.md#windows-ui-library).

## <a name="update-existing-desktop-apps-for-windows-10"></a>Actualización de aplicaciones de escritorio existentes para Windows 10

Si tienes una aplicación de escritorio de Win32 nativa, WPF o Windows Forms, Windows 10 y la Plataforma universal de Windows (UWP) ofrecen muchas características que puedes usar para ofrecer una experiencia moderna en tu aplicación. La mayoría de estas características están disponibles como componentes modulares que puedes adoptar en tu aplicación a tu propio ritmo y sin tener que volver a escribir la aplicación para una plataforma diferente.

Estas son solo algunas de las características disponibles para mejorar tus aplicaciones de escritorio existentes:

* Usa [MSIX](/windows/msix/) para empaquetar e implementar tus aplicaciones de escritorio. MSIX es un formato moderno de paquete de la aplicación de Windows que proporciona una experiencia de empaquetado universal para todas las aplicaciones de Windows. MSIX reúne los mejores aspectos de las tecnologías de instalación de MSI, .appx, App-V y ClickOnce, y está compilado para ser seguro y confiable.
* Integra tu aplicación de escritorio con experiencias de Windows 10 mediante las [extensiones de paquete](./modernize/desktop-to-uwp-extensions.md). Por ejemplo, haz que los iconos de Inicio apunten a tu aplicación, convierte tu aplicación en un destino de recurso compartido o envía notificaciones del sistema desde la aplicación.
* Usa [islas XAML](./modernize/xaml-islands.md) para hospedar controles XAML de UWP en tu aplicación de escritorio. Muchas de las características más recientes de la interfaz de usuario de Windows 10 solo están disponibles para los controles XAML de UWP.

Para obtener más información, consulta los artículos siguientes.

<br/>

| Artículo | Descripción |
|---------|-------------|
| [Modernización de las aplicaciones de escritorio](./modernize/index.md) | Describe las características de desarrollo de Windows 10 y UWP más recientes que puedes usar en cualquier aplicación de escritorio, incluidas las aplicaciones de escritorio de WPF, Windows Forms y C++ Win32. |
| [Tutorial: Modernización de una aplicación WPF](./modernize/modernize-wpf-tutorial.md) | Sigue las instrucciones paso a paso para modernizar una aplicación de ejemplo de línea de negocio de WPF existente. Para ello, agrega los controles del calendario y el Lápiz de UWP a la aplicación e inclúyela en un paquete MSIX.  |

## <a name="create-new-desktop-apps"></a>Creación de nuevas aplicaciones de escritorio

Si estás creando una nueva aplicación de escritorio para Windows, aquí tienes algunos recursos útiles para empezar.

<br/>

| Artículo | Descripción |
|---------|-------------|
| [Elección de la plataforma de aplicaciones](choose-your-platform.md) | Proporciona una comparación detallada de las principales plataformas de aplicaciones de escritorio y puede ayudarte a elegir la plataforma adecuada para tus necesidades. En este artículo también se proporcionan vínculos útiles a documentos de cada plataforma. |
| [Plantillas de proyecto Visual Studio para aplicaciones de Windows](visual-studio-templates.md) | Describe las plantillas de proyecto y elementos que Visual Studio proporciona para ayudarle a compilar aplicaciones para dispositivos Windows 10 mediante el uso de C\# o C++. |
| [Modernización de las aplicaciones de escritorio](./modernize/index.md) | Describe las características de desarrollo de Windows 10 y UWP más recientes que puedes usar en cualquier aplicación de escritorio, incluidas las aplicaciones de escritorio de WPF, Windows Forms y C++ Win32. |
| [Características y tecnologías](../features-and-technologies.md) | Proporciona información general sobre las características de Windows a las que se puede acceder a través de cada una de las plataformas de aplicaciones de escritorio principales, además de vínculos a los documentos relacionados. |

## <a name="related-documentation-and-technologies"></a>Documentación y tecnologías relacionadas

| Recurso | Descripción |
|---------|-------------|
| [.NET Core 3.1](/dotnet/core/whats-new/dotnet-core-3-1) | Obtenga información sobre las características más recientes de .NET Core 3.1, incluidas las mejoras para las aplicaciones de WPF y Windows Forms. |
| [.NET 5](/dotnet/core/dotnet-five) | En este artículo se detalla lo que se incluye en .NET 5, que es la siguiente versión de .NET Core 3.1. |
| [Guía de escritorio para WPF y .NET Core](/dotnet/desktop-wpf/overview/index) | Desarrolla aplicaciones de WPF para .NET Core en lugar de desarrollarlas para la versión completa de .NET Framework.  |
| [Azure](/azure/) | Amplía el alcance de tus aplicaciones con los servicios en la nube de Azure. |
| [Visual Studio](/visualstudio/) | Aprende a usar Visual Studio para desarrollar aplicaciones y servicios. |
| [MSIX](/windows/msix/) | Empaqueta e implementa cualquier aplicación de Windows con un formato de empaquetado moderno y universal. |
| [Inteligencia artificial de Windows](/windows/ai/) | Usa la IA de Windows para compilar soluciones inteligentes para problemas complejos en tus aplicaciones. |
| [Contenedores de Windows](/virtualization/windowscontainers/) | Empaqueta tus aplicaciones con sus dependencias en entornos de Windows rápidos y completamente aislados. |
| [Aplicaciones web progresivas](/microsoft-edge/progressive-web-apps) | Convierte tus aplicaciones web en Web Apps progresivas que se puedan distribuir y ejecutar como aplicaciones para UWP en Windows 10. |
| [Xamarin](/xamarin/) | Crea aplicaciones multiplataforma para Windows, Android, iOS y macOS con código .NET e interfaces de usuario específicas de la plataforma. |
| [Archivo de documentos para Windows 8.x y versiones anteriores](/previous-versions/windows/) | Accede a la documentación archivada sobre la creación de aplicaciones para Windows 8. x y versiones anteriores. |
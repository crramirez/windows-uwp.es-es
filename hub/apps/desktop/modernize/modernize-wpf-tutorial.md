---
description: En este tutorial se muestra cómo agregar interfaces de usuario de XAML en UWP, crear paquetes MSIX e incorporar otros componentes actuales en la aplicación de WPF.
title: 'Tutorial: Modernización de una aplicación WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: aa8991e7fd0bbb825ff5280f01693f092125f573
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161379"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: Modernización de una aplicación WPF 

Hay muchas maneras de [modernizar](index.md) las aplicaciones de escritorio existentes mediante la integración de las características de Windows más recientes en el código fuente existente en lugar de volver a escribir las aplicaciones desde el principio. En este tutorial exploraremos varias maneras de modernizar una aplicación de línea de negocio de WPF existente con estas características:

* .NET Core 3
* Controles XAML de UWP con islas XAML
* Tarjetas adaptables y notificaciones de Windows 10
* Implementación de MSIX

Para este tutorial se requieren las aptitudes de desarrollo siguientes:

* Experiencia en el desarrollo de aplicaciones de escritorio de Windows con WPF.
* Conocimientos básicos de C# y XAML.
* Conocimientos básicos de UWP.

## <a name="overview"></a>Introducción

En este tutorial se proporciona el código para una aplicación de línea de negocio de WPF sencilla denominada Gastos de Contoso. En el escenario ficticio del tutorial, Gastos de Contoso es una aplicación interna que usan los administradores de Contoso Corporation para realizar un seguimiento de los gastos enviados por sus subordinados. Ahora los administradores están equipados con dispositivos táctiles y les gustaría usar la aplicación Gastos de Contoso sin un mouse o un teclado. Lamentablemente, la versión actual de la aplicación no es táctil.

Contoso desea modernizar esta aplicación con nuevas las características de Windows para permitir que los empleados creen informes de gastos de forma más eficaz. Muchas de las características se pueden implementar fácilmente mediante la creación de una nueva aplicación para UWP. Sin embargo, la aplicación existente es compleja y es el resultado de muchos años de desarrollo en diferentes equipos. Por lo tanto, volver a escribirlo desde cero con una nueva tecnología no es una opción. El equipo busca el mejor enfoque para agregar nuevas características al código base existente.

Al principio del tutorial, Gastos de Contoso tiene como destino .NET Framework 4.7.2 y usa las siguientes bibliotecas de terceros:

* MVVM Light, una implementación básica del patrón MVVM.
* Unity, un contenedor de inserción de dependencias.
* LiteDb, una solución NoSQL insertada para almacenar los datos.
* Bogus, una herramienta para generar datos falsos.

En el tutorial, realizarás las siguientes acciones para mejorar la aplicación Gastos de Contoso con las nuevas características de Windows:

* Migrar una aplicación de WPF existente a .NET Core 3.0. En el futuro, esta acción abrirá escenarios nuevos y importantes.
* Usar islas XAML para hospedar los controles encapsulados **InkCanvas** y **MapControl** que proporciona el kit de herramientas de la comunidad de Windows.
* Usar islas XAML para hospedar cualquier control XAML de UWP estándar (en este caso, **CalendarView**).
* Integrar tarjetas adaptables y notificaciones de Windows 10 en la aplicación.
* Empaquetar la aplicación con MSIX y configurar una canalización de CI/CD en Azure DevOps para poder enviar automáticamente nuevas versiones de la aplicación a evaluadores y usuarios en cuanto estén disponibles.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, el equipo de desarrollo debe tener instalados estos requisitos previos:

* Windows 10, versión 1903 (compilación 18362) o una versión posterior.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK de .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (instala la versión más reciente).

Asegúrate de instalar las características opcionales y las cargas de trabajo siguientes con Visual Studio 2019:

* Desarrollo de escritorio de .NET
* Desarrollo con la Plataforma universal de Windows
* SDK de Windows 10 (10.0.18362.0 o posterior)

## <a name="get-the-contoso-expenses-sample-app"></a>Obtención de la aplicación de ejemplo Gastos de Contoso

Antes de iniciar el tutorial, descarga el código fuente de la aplicación Gastos de Contoso y asegúrate de que puedes compilar el código en Visual Studio.

1. Descarga el código fuente de la aplicación desde la pestaña **Versiones** del [repositorio AppConsult-WinAppsModernizationWorkshop](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). El vínculo directo es [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases).
2. Abre el archivo .zip y extrae todo el contenido en la raíz de la unidad **C:\\** . Se creará una carpeta denominada **C:\WinAppsModernizationWorkshop**.
3. Abre Visual Studio 2019 y haz doble clic en el archivo **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** para abrir la solución.
4. Compruebe que puedes compilar, ejecutar y depurar el proyecto de WPF Gastos de Contoso. Para ello, presiona el botón **Inicio** o CTRL + F5.

## <a name="get-started"></a>Introducción

Cuando tengas el código fuente de la aplicación de ejemplo Gastos de Contoso y puedas comprobar que se puede compilar en Visual Studio, estarás listo para iniciar el tutorial:

* [Parte 1: Migración de la aplicación Gastos de Contoso a .NET Core 3](modernize-wpf-tutorial-1.md)
* [Parte 2: Incorporación de un control InkCanvas en UWP mediante islas XAML](modernize-wpf-tutorial-2.md)
* [Parte 3: Incorporación de un control CalendarView en UWP mediante islas XAML](modernize-wpf-tutorial-3.md)
* [Parte 4: Incorporación de actividades y notificaciones del usuario de Windows 10](modernize-wpf-tutorial-4.md)
* [Parte 5: Empaquetar e implementar con MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Conceptos importantes

En las secciones siguientes se proporciona información general sobre algunos de los conceptos clave que se tratan en este tutorial. Si ya estás familiarizado con estos conceptos, puedes ignorar esta sección.

### <a name="universal-windows-platform-uwp"></a>Plataforma universal de Windows (UWP)

En Windows 8, Microsoft presentó un nuevo marco denominado Windows Runtime (WinRT). A diferencia de .NET Framework, WinRT es una capa nativa de API que se exponen directamente en las aplicaciones. WinRT también incorporó proyecciones de lenguaje, que son capas agregadas encima del tiempo de ejecución para que los desarrolladores puedan interactuar con este igual que con C# y JavaScript, además de C++. Las proyecciones permiten a los desarrolladores compilar aplicaciones basadas en WinRT que aprovechen el mismo conocimiento de C# y XAML que adquirieron al compilar aplicaciones con .NET Framework. 

En Windows 10, Microsoft introdujo la [Plataforma universal de Windows (UWP)](/windows/uwp/get-started/universal-application-platform-guide), que se basa en WinRT. La característica más importante de UWP es que ofrece un conjunto común de API en cada plataforma de dispositivos: independientemente de si la aplicación se ejecuta en un equipo de escritorio, una consola Xbox One o un dispositivo HoloLens, podrá usar las mismas API.

En adelante, la mayoría de las nuevas características de Windows 10 se exponen a través de las API de WinRT, incluidas las características como Línea de tiempo, Proyecto Roma y Windows Hello.

### <a name="msix-packaging"></a>Empaquetado MSIX

[MSIX](/windows/msix/) es el modelo de empaquetado moderno para las aplicaciones de Windows. MSIX admite aplicaciones para UWP, así como aplicaciones de escritorio que se compilan mediante tecnologías como Win32, WPF, Windows Forms, Java, Electron, etc. Al empaquetar una aplicación de escritorio en un paquete MSIX, puedes publicar la aplicación en Microsoft Store. La aplicación de escritorio también obtiene la identidad del paquete cuando se instala, lo que permite que la aplicación de escritorio use un conjunto más amplio de API de WinRT.

Para obtener más información, consulta los artículos siguientes:

* [Empaquetar aplicaciones de escritorio](/windows/uwp/porting/desktop-to-uwp-root)
* [Segundo plano de la aplicación de escritorio empaquetada](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Islas XAML

A partir de Windows 10, versión 1903, puedes hospedar controles de UWP en aplicaciones de escritorio que no son para UWP mediante una característica denominada *islas XAML*. Esta característica te permite mejorar el aspecto y la funcionalidad de las aplicaciones de escritorio existentes, con las características más recientes de la UI de Windows 10 que solo están disponibles a través de controles de UWP. Esto significa que puedes usar características de UWP, como Windows Ink, y controles que admiten el Sistema Fluent Design en las aplicaciones de WPF, Windows Forms y Win32 de C++ existentes.

Para obtener más información, consulta [Controles de UWP en aplicaciones de escritorio (islas XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Este tutorial te guía en el uso de dos tipos diferentes de controles de islas XAML:

* Los controles [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) y [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) del kit de herramientas de la comunidad de Windows. Estos controles de WPF encapsulan la interfaz y la funcionalidad de los controles de UWP correspondientes y se pueden usar como cualquier otro control de WPF en el diseñador de Visual Studio.

* El control [CalendarView](/windows/uwp/design/controls-and-patterns/calendar-view) de UWP. Se trata de un control de UWP estándar que hospedarás mediante el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows.

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](/dotnet/core/) es un marco de código abierto que implementa una versión multiplataforma, ligera y fácilmente extensible de la versión de .NET Framework completa. En comparación con la versión completa de .NET Framework, el tiempo de inicio de .NET Core es mucho más rápido y muchas de las API se han optimizado.

En sus primeras versiones, el enfoque de .NET Core era ofrecer compatibilidad con aplicaciones web o de back-end. Con .NET Core, puedes crear fácilmente aplicaciones web escalables o API que se puedan hospedar en Windows, Linux o arquitecturas de microservicios como contenedores de Docker.

.NET Core 3 es la versión más reciente de .NET Core. Lo más destacado de esta versión es la compatibilidad con aplicaciones de escritorio de Windows, incluidas las aplicaciones de Windows Forms y WPF. Puedes ejecutar aplicaciones de escritorio de Windows nuevas y existentes en .NET Core 3 y disfrutar de todas las ventajas que ofrece .NET Core. Los controles de UWP que se hospedan en [islas XAML](xaml-islands.md) también se pueden usar en las aplicaciones de Windows Forms y WPF que tienen como destino .NET Core 3.

> [!NOTE]
> WPF y Windows Forms no están convirtiéndose en multiplataforma, y no se puede ejecutar ninguna instancia de WPF o Windows Forms en Linux y MacOS. Los componentes de la interfaz de usuario de WPF y Windows Forms siguen teniendo una dependencia en el sistema de representación de Windows.

Para obtener más información, consulta el tema sobre [novedades de .NET Core 3.0](/dotnet/core/whats-new/dotnet-core-3-0).
---
description: En este tutorial se muestra cómo agregar interfaces de usuario XAML de UWP, crear paquetes de MSIX e incorporar otros componentes modernos en la aplicación WPF.
title: 'Tutorial: modernizar una aplicación de WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521295"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: modernizar una aplicación de WPF 

Hay muchas maneras de [modernizar](index.md) las aplicaciones de escritorio existentes mediante la integración de las características de Windows más recientes en el código fuente existente en lugar de volver a escribir las aplicaciones desde el principio. En este tutorial exploraremos varias maneras de modernizar una aplicación de línea de negocio de WPF existente con estas características:

* .NET Core 3
* Controles XAML de UWP con islas XAML
* Tarjetas adaptables y notificaciones de Windows 10
* Implementación de MSIX

En este tutorial se requieren los siguientes conocimientos de desarrollo:

* Experiencia en el desarrollo de aplicaciones de escritorio de Windows con WPF.
* Conocimientos básicos de C# y XAML.
* Conocimientos básicos de UWP.

## <a name="overview"></a>Información general

En este tutorial se proporciona el código para una sencilla aplicación de línea de negocio de WPF denominada gastos de contoso. En el escenario ficticio del tutorial, contoso Reports es una aplicación interna que usan los administradores de Contoso Corporation para realizar un seguimiento de los gastos enviados por sus informes. Ahora los administradores están equipados con dispositivos táctiles y les gustaría usar la aplicación de gastos de Contoso sin un mouse o un teclado. Desafortunadamente, la versión actual de la aplicación no es táctil.

Contoso desea modernizar esta aplicación con nuevas características de Windows para permitir que los empleados creen informes de gastos de forma más eficaz. Muchas de las características se pueden implementar fácilmente mediante la creación de una nueva aplicación de UWP. Sin embargo, la aplicación existente es compleja y es el resultado de muchos años de desarrollo en diferentes equipos. Por lo tanto, volver a escribirlo desde cero con una nueva tecnología no es una opción. El equipo busca el mejor enfoque para agregar nuevas características en el código base existente.

Al principio del tutorial, contoso Expenses tiene como destino el .NET Framework 4.7.2 y usa las siguientes bibliotecas de terceros:

* MVVM Light, una implementación básica del patrón MVVM.
* Unity, un contenedor de inserción de dependencias.
* LiteDb, una solución NoSQL insertada para almacenar los datos.
* Fantasma, una herramienta para generar datos falsos.

En el tutorial, mejorará los gastos de Contoso con nuevas características de Windows:

* Migre una aplicación de WPF existente a .NET Core 3,0. Esto abrirá escenarios nuevos y importantes en el futuro.
* Use las islas XAML para hospedar los controles encapsulados **InkCanvas** y **MapControl** proporcionados por el kit de herramientas de la comunidad de Windows.
* Use las islas XAML para hospedar cualquier control XAML de UWP estándar (en este caso, un **CalendardView**).
* Integre tarjetas adaptables y notificaciones de Windows 10 en la aplicación.
* Empaquete la aplicación con MSIX y configure una canalización de CI/CD en Azure DevOps para que pueda enviar automáticamente nuevas versiones de la aplicación a los evaluadores y usuarios en cuanto estén disponibles.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, el equipo de desarrollo debe tener instalados estos requisitos previos:

* Windows 10, versión 1903 (compilación 18362) o una versión posterior.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK de .net Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (Instale la versión más reciente).

Asegúrese de instalar las siguientes cargas de trabajo y características opcionales con Visual Studio 2019:

* Desarrollo de escritorio de .NET
* Desarrollo con la Plataforma universal de Windows
* SDK de Windows 10 (10.0.18362.0 o posterior)

## <a name="get-the-contoso-expenses-sample-app"></a>Obtención de la aplicación de ejemplo de gastos de Contoso

Antes de comenzar el tutorial, descargue el código fuente de la aplicación de gastos de Contoso y asegúrese de que puede compilar el código en Visual Studio.

1. Descargue el código fuente de la aplicación en la pestaña **releases** del [repositorio de AppConsult WinAppsModernization Workshop](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). El vínculo directo es [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases).
2. Abra el archivo zip y extraiga todo el contenido en la raíz de la unidad **C:\\** . Se creará una carpeta denominada **C:\WinAppsModernizationWorkshop**.
3. Abra Visual Studio 2019 y haga doble clic en el archivo **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** para abrir la solución.
4. Compruebe que puede compilar, ejecutar y depurar el proyecto de WPF de gastos de Contoso presionando el botón **Inicio** o Ctrl + F5.

## <a name="get-started"></a>Introducción

Una vez que tenga el código fuente de la aplicación de ejemplo de gastos de Contoso y pueda confirmar que puede compilarlo en Visual Studio, está listo para iniciar el tutorial:

* [Parte 1: migración de la aplicación de gastos de Contoso a .NET Core 3](modernize-wpf-tutorial-1.md)
* [Parte 2: adición de un control InkCanvas de UWP con islas XAML](modernize-wpf-tutorial-2.md)
* [Parte 3: adición de un control CalendarView de UWP con islas XAML](modernize-wpf-tutorial-3.md)
* [Parte 4: incorporación de notificaciones y actividades de usuario de Windows 10](modernize-wpf-tutorial-4.md)
* [Parte 5: empaquetar e implementar con MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Conceptos importantes

En las secciones siguientes se proporciona información general sobre algunos de los conceptos clave que se describen en este tutorial. Si ya está familiarizado con estos conceptos, puede omitir esta sección.

### <a name="universal-windows-platform-uwp"></a>Plataforma universal de Windows (UWP)

En Windows 8, Microsoft presentó un nuevo marco de trabajo llamado Windows Runtime (WinRT). A diferencia del .NET Framework, WinRT es una capa nativa de API que se exponen directamente en las aplicaciones. WinRT también incorporó proyecciones de lenguaje, que son capas agregadas en la parte superior del tiempo de ejecución para que los desarrolladores puedan interactuar con C# ella mediante lenguajes como C++y JavaScript, además de. Las proyecciones permiten a los desarrolladores compilar aplicaciones sobre WinRT que aprovechan C# el mismo conocimiento y XAML que adquirieron en la creación de aplicaciones con el .NET Framework. 

En Windows 10, Microsoft presentó el [plataforma universal de Windows (UWP)](/windows/uwp/get-started/universal-application-platform-guide), que se basa en WinRT. La característica más importante de UWP es que ofrece un conjunto común de API en cada plataforma de dispositivo: independientemente de si la aplicación se ejecuta en un escritorio, en una Xbox One o en HoloLens, podrá usar las mismas API.

En adelante, la mayoría de las nuevas características de Windows 10 se exponen a través de las API de WinRT, incluidas características como la escala de tiempo, el proyecto Roma y Windows Hello.

### <a name="msix-packaging"></a>Empaquetado de MSIX

[MSIX](/windows/msix/) es el modelo de empaquetado moderno para aplicaciones de Windows. MSIX admite aplicaciones para UWP, así como aplicaciones de escritorio que se compilan mediante tecnologías como Win32, WPF, Windows Forms, Java, electrones, etc. Al empaquetar una aplicación de escritorio en un paquete de MSIX, puede publicar la aplicación en el Microsoft Store. La aplicación de escritorio también obtiene la identidad del paquete cuando se instala, lo que permite a la aplicación de escritorio usar un conjunto más amplio de API de WinRT.

Para obtener más información, consulte estos artículos:

* [Empaquetar aplicaciones de escritorio](/windows/uwp/porting/desktop-to-uwp-root)
* [En segundo plano de la aplicación de escritorio empaquetada](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Islas XAML

A partir de Windows 10, versión 1903, puede hospedar controles de UWP en aplicaciones de escritorio que no son de UWP con una característica denominada *islas XAML*. Esta característica permite mejorar la apariencia, el funcionamiento y la funcionalidad de las aplicaciones de escritorio existentes con las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Esto significa que puede usar características de UWP como Windows Ink y los controles que admiten el sistema de diseño de Fluent en sus aplicaciones WPF, C++ Windows Forms y Win32 existentes.

Para obtener más información, vea [controles de UWP en aplicaciones de escritorio (Islas XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Este tutorial le guía a través del proceso de uso de dos tipos diferentes de controles de isla XAML:

* El [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) y el [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) en el kit de herramientas de la comunidad de Windows. Estos controles de WPF encapsulan la interfaz y la funcionalidad de los controles de UWP correspondientes y se pueden usar como cualquier otro control WPF en el diseñador de Visual Studio.

* Control de [vista de calendario](/windows/uwp/design/controls-and-patterns/calendar-view) de UWP. Se trata de un control estándar de UWP que se hospedará con el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows.

### <a name="net-core-3"></a>.NET Core 3

[.Net Core](https://docs.microsoft.com/dotnet/core/) es un marco de código abierto que implementa una versión multiplataforma, ligera y fácilmente extensible de la .NET Framework completa. En comparación con el .NET Framework completo, el tiempo de inicio de .NET Core es mucho más rápido y muchas de las API se han optimizado.

A través de sus primeras versiones, el enfoque de .NET Core era para admitir aplicaciones web o de back-end. Con .NET Core, puede crear fácilmente aplicaciones web escalables o API que se pueden hospedar en Windows, Linux o en arquitecturas de microservicios como contenedores de Docker.

.NET Core 3 es la versión más reciente de .NET Core. Lo más destacado de esta versión es la compatibilidad con aplicaciones de escritorio de Windows, incluidas las aplicaciones de Windows Forms y WPF. Puedes ejecutar aplicaciones de escritorio de Windows nuevas y existentes en .NET Core 3 y disfrutar de todas las ventajas que ofrece .NET Core. Los controles de UWP que se hospedan en [islas XAML](xaml-islands.md) también se pueden usar en las aplicaciones de Windows Forms y WPF que tienen como destino .NET Core 3.

> [!NOTE]
> WPF y Windows Forms no están convirtiéndose en multiplataforma y no se puede ejecutar WPF o Windows Forms en Linux y MacOS. Los componentes de interfaz de usuario de WPF y Windows Forms siguen teniendo una dependencia en el sistema de representación de Windows.

Para obtener más información, consulta el tema sobre [novedades de .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

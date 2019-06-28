---
description: Este tutorial muestra cómo agregar interfaces de usuario de UWP XAML y crear paquetes MSIX para incorporar otros componentes modernos en su aplicación WPF.
title: 'Tutorial: Modernizar una aplicación de WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420079"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: Modernizar una aplicación de WPF 

Hay muchas maneras de [modernizar](index.md) código en lugar de volver a escribir las aplicaciones desde el principio del origen de las aplicaciones de escritorio existentes mediante la integración de las características más recientes de Windows existentes. En este tutorial exploraremos varias maneras para modernizar una aplicación de línea de negocio WPF existente mediante el uso de estas características:

* .NET Core 3
* Controles de UWP XAML con islas de XAML
* Las tarjetas adaptables y Windows 10 notificaciones
* Implementación de MSIX

Este tutorial requiere los conocimientos de desarrollo siguientes:

* Experiencia en desarrollo de aplicaciones de escritorio de Windows con WPF.
* Conocimientos básicos de C# y XAML.
* Conocimientos básicos de UWP.

## <a name="overview"></a>Información general

En este tutorial se proporciona el código para una aplicación de línea de negocio WPF sencilla denominada Contoso gastos. En el escenario ficticio del tutorial, los gastos de Contoso es una aplicación interna que los administradores de Contoso Corporation usada para realizar un seguimiento de los gastos enviados por sus informes. Los administradores ahora están equipados con dispositivos táctiles y les gustaría usar la aplicación de gastos de Contoso sin un mouse o teclado. Por desgracia, la versión actual de la aplicación no toque descriptivo.

Contoso desea modernizar esta aplicación con las nuevas características de Windows para permitir que los empleados crear informes de gastos de forma más eficaz. Muchas de las características se pueden implementar fácilmente mediante la creación de una nueva aplicación UWP. Sin embargo, la aplicación existente es compleja y es el resultado de muchos años de desarrollo diferentes equipos. Por lo tanto, escribirla de nuevo desde cero con una nueva tecnología, no es una opción. El equipo está buscando el mejor enfoque agregar nuevas características en el código base existente.

Al principio del tutorial, los gastos de Contoso tiene como destino .NET Framework 4.7.2 y utiliza las siguientes bibliotecas de 3ª parte:

* MVVM Light, una implementación básica para el patrón MVVM.
* Un contenedor de inserción de dependencias de Unity.
* LiteDb, una solución NoSQL insertada para almacenar los datos.
* Falsas, una herramienta para generar datos falsos.

En el tutorial, mejorará los gastos de Contoso con nuevas características de Windows:

* Migrar una aplicación WPF existente a .NET Core 3.0. Esto abrirá escenarios nuevos e importantes en el futuro.
* Islas de XAML de uso para hospedar el **InkCanvas** y **MapControl** ajustado controles proporcionados por el Kit de herramientas de la Comunidad de Windows.
* Uso de islas de XAML para hospedar cualquier control UWP XAML estándar (en este caso, un **CalendardView**).
* Integre notificaciones de las tarjetas adaptables y Windows 10 en la aplicación.
* Paquete de la aplicación con MSIX y un conjunto de un CI/CD de canalización de DevOps de Azure para que pueda entregar automáticamente nuevas versiones de la aplicación a los evaluadores y los usuarios en cuanto está disponible.

## <a name="prerequisites"></a>Requisitos previos

Para llevar a cabo este tutorial, el equipo de desarrollo debe tener estos requisitos previos instalados:

* Windows 10, versión 1903 (compilación 18362) o una versión posterior.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK de versión preliminar de .NET core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (instale la última versión de vista previa disponible).

Asegúrese de que instalar las siguientes cargas de trabajo y las características opcionales con 2019 de Visual Studio:

* Desarrollo de escritorio de .NET
* Desarrollo con la Plataforma universal de Windows
* SDK de Windows 10 (10.0.18362.0 o posterior)

## <a name="get-the-contoso-expenses-sample-app"></a>Obtener la aplicación de ejemplo Contoso gastos

Antes de comenzar el tutorial, descargue el código fuente de la aplicación de gastos de Contoso y asegúrese de que puede compilar el código en Visual Studio.

1. Descargar el código fuente de aplicación desde el **versiones** pestaña de la [repositorio taller de AppConsult WinAppsModernization](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Es el vínculo directo [ https://aka.ms/wamwc ](https://aka.ms/wamwc).
2. Abra el archivo zip y extraiga todo el contenido a la raíz de su **C:\\**  unidad. Creará una carpeta denominada **C:\WinAppsModernizationWorkshop**.
3. Abra Visual Studio de 2019 y haga doble clic en el **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** archivo para abrir la solución.
4. Compruebe que se puede compilar, ejecutar y depurar el proyecto de WPF de gastos de Contoso presionando el **iniciar** botón o CTRL + F5.

## <a name="get-started"></a>Comenzar

Después de que tiene el código fuente de la aplicación de ejemplo Contoso gastos y puede confirmar que puede compilar en Visual Studio, está listo para comenzar el tutorial:

* [Parte 1: Migrar el Contoso app de gastos a .NET Core 3](modernize-wpf-tutorial-1.md)
* [Parte 2: Agregar un control de UWP InkCanvas mediante islas de XAML](modernize-wpf-tutorial-2.md)
* [Parte 3: Agregar un control de UWP CalendarView mediante islas de XAML](modernize-wpf-tutorial-3.md)
* [Parte 4: Agregar notificaciones y las actividades de usuario de Windows 10](modernize-wpf-tutorial-4.md)
* [Parte 5: Empaquetar e implementar con MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Conceptos importantes

Las secciones siguientes proporcionan en segundo plano para algunos de los conceptos claves tratados en este tutorial. Si ya está familiarizado con estos conceptos, puede omitir esta sección.

### <a name="universal-windows-platform-uwp"></a>Plataforma universal de Windows (UWP)

En Windows 8, Microsoft presentó un nuevo marco denominado Windows Runtime (WinRT). A diferencia de .NET Framework, WinRT es una capa de API que se exponen directamente a las aplicaciones nativa. WinRT también introdujo las proyecciones de lenguaje, que son capas agregadas sobre el tiempo de ejecución para permitir a los programadores interactuar con él mediante lenguajes como C# y JavaScript además C++. Las proyecciones que los desarrolladores creen aplicaciones encima de WinRT que aprovechan el mismo C# y conocimientos XAML que adquieren en la creación de aplicaciones con .NET Framework. 

En Windows 10, Microsoft presentó la [plataforma Universal de Windows (UWP)](/windows/uwp/get-started/universal-application-platform-guide), que se basa en WinRT. La característica más importante de UWP es que ofrece un conjunto común de API en cada plataforma de dispositivo: sin importar si la aplicación se ejecuta en un equipo de escritorio, en Xbox One o en un HoloLens, que puede usar las mismas API.

A partir de ahora, más recientes de Windows 10 se exponen las características mediante APIs de WinRT, incluidas características como la escala de tiempo, proyecto Roma y Windows Hello.

### <a name="msix-packaging"></a>Empaquetado de MSIX

[MSIX](http://aka.ms/msix) (conocido anteriormente como AppX) es el modelo de empaquetado modernos para aplicaciones de Windows. MSIX es compatible con aplicaciones para UWP, así como aplicaciones de escritorio de compilar mediante tecnologías como Win32, WPF, Windows Forms, Java, Electron y mucho más. Al empaquetar una aplicación de escritorio en un paquete MSIX, puede publicar la aplicación en la Microsoft Store. Su aplicación de escritorio también obtener la identidad del paquete cuando se instala, lo que permite la aplicación de escritorio para utilizar un conjunto más amplio de WinRT APIs.

Para obtener más información, consulte estos artículos:

* [Empaquetar aplicaciones de escritorio](/windows/uwp/porting/desktop-to-uwp-root)
* [En segundo plano de la aplicación de escritorio empaquetada](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Islas de XAML

A partir de Windows 10, versión 1903, puede hospedar controles UWP no de UWP en aplicaciones de escritorio mediante una característica denominada *XAML Islas*. Esta característica permite mejorar el aspecto y la funcionalidad de las aplicaciones de escritorio existentes con las últimas características de interfaz de usuario de Windows 10 que solo están disponibles a través de controles UWP. Esto significa que puede usar las características UWP como tinta de Windows y los controles que admiten el sistema de diseño Fluent en su existente WPF, Windows Forms, y C++ aplicaciones de Win32.

Para obtener más información, consulte [controles UWP en aplicaciones de escritorio (Islas de XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Este tutorial le guiará por el proceso de usar dos tipos diferentes de controles de la isla de XAML:

* El [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) y [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) en el Kit de herramientas de la Comunidad de Windows. Estos controles WPF encapsular la interfaz y funcionalidad de los controles UWP correspondientes y se pueden usar como cualquier otro control WPF en el Diseñador de Visual Studio.

* UWP [vista de calendario](/windows/uwp/design/controls-and-patterns/calendar-view) control. Se trata de un control estándar de UWP que va a hospedar mediante el uso de la [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control en el Kit de herramientas de la Comunidad de Windows.

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/) es un marco de código abierto que implementa una versión multiplataforma, ligera y extensible fácilmente de la versión completa de .NET Framework. En comparación con la versión completa de .NET Framework, el tiempo de inicio de .NET Core es mucho más rápido y muchas de las API se han optimizado.

A través de sus primera varias versiones, el enfoque de .NET Core era para la compatibilidad con web o aplicaciones de back-end. Con .NET Core, puede crear fácilmente aplicaciones web escalables o las API que pueden hospedarse en Windows, Linux, o en arquitecturas de microservicio como contenedores de Docker.

.NET Core 3 es la siguiente versión principal de .NET Core. Lo más destacado de la próxima versión es la compatibilidad con aplicaciones de escritorio de Windows, incluidas las aplicaciones de Windows Forms y WPF. Puede ejecutar aplicaciones de escritorio de Windows nuevas y existentes en .NET Core 3 y disfrutar de todas las ventajas que ofrece .NET Core. Los controles de UWP que se hospedan en [islas XAML](xaml-islands.md) también se pueden usar en las aplicaciones de Windows Forms y WPF que tienen como destino .NET Core 3.

> [!NOTE]
> WPF y Windows Forms no están convirtiendo en multiplataforma, y no se puede ejecutar un WPF o Windows Forms en Linux y MacOS. Los componentes de interfaz de usuario de WPF y Windows Forms todavía tienen una dependencia en el sistema de representación de Windows.

Para obtener más información, consulta los artículos siguientes:

* [Anuncio de la versión preliminar 1 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [Anuncio de la versión preliminar 2 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [Anuncio de la versión preliminar 3 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [Anuncio de la versión preliminar 4 de .NET Core 3.0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [Novedades en .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).
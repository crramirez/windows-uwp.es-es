---
title: Biblioteca de interfaz de usuario de Windows
description: Proporciona información para el desarrollo de aplicaciones de WinUI 2. x y Windows.
ms.topic: article
ms.date: 04/15/2020
keywords: Windows 10, UWP, SDK del kit de herramientas, WinUI, biblioteca de interfaz de usuario de Windows
ms.custom: RS5
ms.openlocfilehash: c1828405c424ca54dcb70e587479fd5307b1046d
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775858"
---
# <a name="windows-ui-library-2x"></a>Biblioteca de interfaz de usuario de Windows 2.x

![Controles WinUI](images/winUI-library-767.png)

La biblioteca de interfaz de usuario de Windows proporciona controles de interfaz de usuario nativos de Windows y otros elementos de la interfaz de usuario para aplicaciones de Windows.

Mantiene la compatibilidad de nivel inferior con versiones anteriores de Windows 10, de modo que la aplicación funciona aunque los usuarios no tengan el sistema operativo más reciente.

> [!NOTE]
> Consulta [WinUI 3.0, versión preliminar 1](../winui3/index.md), una actualización importante de la plataforma de interfaz de usuario de Windows 10 planeada para su lanzamiento en 2020.

## <a name="features"></a>Características

* **Nuevos controles**: la biblioteca de la interfaz de usuario de Windows contiene nuevos controles que no se incluyen como parte de la plataforma de Windows predeterminada.

* **Versiones actualizadas de los controles existentes**: la biblioteca también contiene las versiones actualizadas de los controles de la plataforma de Windows existentes que puede usar con versiones anteriores de Windows 10.

* **Compatibilidad con versiones anteriores de Windows 10**: las API de la biblioteca de interfaz de usuario de Windows se ejecutan en versiones anteriores de Windows 10, por lo que no tiene que incluir comprobaciones de versión ni XAML condicional para admitir usuarios que no dispongan del último sistema operativo.

* **Compatibilidad con XamlDirect**: Las API de XAML directo, diseñadas para desarrolladores de software intermedio, proporcionan acceso a las características XAML de nivel inferior, lo que proporciona un mejor rendimiento de la CPU y del espacio de trabajo. XamlDirect le permite usar las API de XamlDirect en versiones anteriores de Windows 10 sin necesidad de escribir código especial para controlar varias versiones de destino de Windows 10.

## <a name="examples"></a>Ejemplos

La aplicación de ejemplo XAML Controls Gallery incluye demostraciones interactivas y código de ejemplo para usar controles WinUI.

* Instalación de la aplicación XAML Controls Gallery desde [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* La aplicación XAML Controls Gallery también está disponible en [código abierto en GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentación

Se incluyen artículos sobre procedimientos para los controles de la biblioteca de interfaz de usuario de Windows con la [documentación sobre controles de la Plataforma universal de Windows](/windows/uwp/design/controls-and-patterns/).

Los documentos de referencia de API se encuentran aquí: [API de la biblioteca de interfaz de usuario de Windows](/uwp/api/overview/winui/).

## <a name="install-and-use-the-windows-ui-library"></a>Instalación y uso de la biblioteca de interfaz de usuario de Windows

Para obtener instrucciones, consulte [Introducción a la biblioteca de interfaz de usuario de Windows](getting-started.md).

## <a name="open-source-and-developer-roadmap"></a>Hoja de ruta de código abierto y para desarrolladores

WinUI es un proyecto de código abierto que se hospeda en GitHub. Agradecemos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui).

Seguimos desarrollando y haciendo evolucionar WinUI para admitir más escenarios de desarrollo. Para obtener los detalles más recientes de nuestros planes para WinUI, consulte nuestra [hoja de ruta](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) en el repositorio de la biblioteca de interfaz de usuario de Windows.

## <a name="nuget-package-list"></a>Lista de paquetes NuGet

La biblioteca de interfaz de usuario de Windows contiene varios paquetes NuGet: [Lista de paquetes NuGet de la biblioteca de interfaz de usuario de Windows](nuget-packages.md).

## <a name="see-also"></a>Consulta también

[Notas de la versión de la biblioteca de interfaz de usuario de Windows UI 2.x](release-notes/index.md)

---
title: Notas de la versión de WinUI 2.3
description: Notas de la versión de WinUI 2.3, incluidas las nuevas características y correcciones de errores.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 63091f927f63a708c5a5e4d41e9d81fd9f528cb1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154789"
---
# <a name="windows-ui-library-23"></a>Biblioteca de interfaz de usuario de Windows 2.3

WinUI 2.3 es la última versión oficial de la biblioteca de interfaz de usuario de Windows (WinUI).

WinUI es un proyecto de código abierto que se hospeda en GitHub en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui). Registre todos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en este repositorio.

Versiones de WinUI: [Página de versiones de GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

Los paquetes de WinUI se pueden agregar a los proyectos de Visual Studio a través del administrador de paquetes NuGet. Para obtener más información, consulte [Introducción a la biblioteca de interfaz de usuario de Windows](../getting-started.md).

Descarga de paquetes NuGet: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Nuevas características

### <a name="progress-bar-visual-refresh"></a>Actualización visual de la barra de progreso

El control **ProgressBar** tiene dos representaciones visuales diferentes.

#### <a name="indeterminate-progress-bar"></a>Barra de progreso indeterminado

Muestra que una tarea está en curso, pero no bloquea la interacción del usuario.

![Barra de progreso indeterminado](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>Barra de progreso determinada

Muestra el progreso realizado en una cantidad de trabajo conocida. 

![Barra de progreso determinada](../images/DeterminateProgressBar.gif)

[Vínculo de documento](/windows/uwp/design/controls-and-patterns/progress-controls)

[Vínculo de ejemplo](/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

**NumberBox** representa un control que se puede usar para mostrar y editar números. Admite la validación, los incrementos paso a paso y el procesamiento de cálculos en línea de ecuaciones básicas, como la multiplicación, la división, la suma y la resta.

![NumberBox](../images/NumberBoxGif.gif)

[Vínculo de documento y ejemplo](/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons** es un nuevo control de contenedor que permite crear fácilmente grupos relacionados de elementos RadioButton, al mismo tiempo que admite correctamente la funcionalidad de teclado y del lector de pantalla o el narrador.

![RadioButtons](../images/RadioButtons.png)

[Vínculo de documento y ejemplo](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>Ejemplos

La aplicación de ejemplo XAML Controls Gallery incluye demostraciones interactivas y código de ejemplo para usar controles WinUI.

* Instalación de la aplicación XAML Controls Gallery desde [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* La aplicación XAML Controls Gallery también está disponible en [código abierto en GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentación

Se incluyen artículos sobre procedimientos para los controles de la biblioteca de interfaz de usuario de Windows con la [documentación sobre controles de la Plataforma universal de Windows](/windows/uwp/design/controls-and-patterns/).

Los documentos de referencia de API se encuentran aquí: [API de la biblioteca de interfaz de usuario de Windows](/uwp/api/overview/winui/).
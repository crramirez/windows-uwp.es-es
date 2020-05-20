---
title: Lista de paquetes NuGet para la biblioteca de interfaz de usuario de Windows
description: Enumera los paquetes NuGet de la biblioteca de interfaz de usuario de Windows.
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, sdk del kit de herramientas
ms.openlocfilehash: 2bda405977733a6191c4434fd8bd2c63b2ce10ce
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580712"
---
# <a name="windows-ui-library-nuget-packages"></a>Paquetes NuGet de la biblioteca de interfaz de usuario de Windows

NuGet es un administrador de paquetes estándar para aplicaciones .NET, que está integrado en Visual Studio. Desde la solución abierta, seleccione el menú *Herramientas*, *Administrador de paquetes NuGet*, *Administrar paquetes NuGet para la solución* para abrir la interfaz de usuario.  Introduzca el nombre de uno de los paquetes a continuación para buscarlo en línea.

| Nombre del paquete NuGet | Descripción |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | Controles de aplicaciones para UWP. Incluye las API de estos espacios de nombres: [Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml), [Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers), [Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls), [Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives), [Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes), [Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media), [Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | Le permite usar las API de [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) en versiones anteriores de Windows 10 sin necesidad de escribir código especial para controlar varias versiones de destino de Windows 10. |


## <a name="search-in-visual-studio"></a>Búsqueda en Visual Studio

Al buscar en el administrador de paquetes de Visual Studio, debería ver una lista similar a esta (los números de versión pueden ser diferentes, pero los nombres deben ser los mismos).

![](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>Actualización de paquetes NuGet

Actualizamos periódicamente la biblioteca de interfaz de usuario de Windows con nuevos controles, servicios, API y, lo que es más importante, correcciones de errores. Para asegurarse de que tiene la versión más reciente, abra el proyecto en Visual Studio, elija el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** -> **Administrar paquetes NuGet para la solución** y, a continuación, la pestaña *Actualizaciones*. Seleccione el paquete que quiere actualizar y haga clic en Instalar para actualizarlo a la última versión.

## <a name="getting-started"></a>Introducción

Lea [Introducción a la biblioteca de interfaz de usuario de Windows](getting-started.md) para obtener más instrucciones sobre el uso de estos paquetes NuGet en sus propios proyectos.

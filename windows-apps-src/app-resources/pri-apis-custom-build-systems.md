---
description: Con las API de indexación de recursos de paquetes (PRI), puedes desarrollar un sistema de compilación personalizado para los recursos de la aplicación para UWP. El sistema de compilación podrá crear, versionar y volcar archivos PRI para cualquier nivel de complejidad que necesite tu aplicación para UWP.
title: Indexación de recursos de paquetes y sistemas de compilación personalizados
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 84b6a05927e2106df136741a262c3af5ef08a575
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031668"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados
Con las [API de indexación de recursos de paquetes (PRI)](/windows/desktop/menurc/pri-indexing-reference), puedes desarrollar un sistema de compilación personalizado para los recursos de la aplicación para UWP. El sistema de compilación podrá crear y volcar (como XML) archivos de índice de recursos del paquete (PRI), así como controlar sus versiones, en cualquier nivel de complejidad que necesite la aplicación para UWP. Si tiene un sistema de compilación personalizado que actualmente usa la herramienta de línea de comandos MakePri.exe (consulte [compilar recursos manualmente con MakePri.exe](makepri-exe-command-options.md)), para aumentar el rendimiento y el control, se recomienda cambiar a llamar a las API de PRI en lugar de llamar a MakePri.exe.

Las API de PRI se introdujeron en el Windows SDK para Windows 10, versión 1803. Las API adoptan la forma de las API de Windows de Win32, lo que significa que tiene algunas opciones para llamarlas. Puede llamarlos directamente desde una aplicación de Win32 o puede llamarlos a través de la [invocación de plataforma](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) desde una aplicación de .net o incluso desde una aplicación de UWP.

En los escenarios de este tema se muestra cómo llamar a las API de PRI desde un proyecto de aplicación de consola de Windows de Win32 Visual C++. Para obtener información general, consulte [sistema de administración de recursos](resource-management-system.md).

> [!NOTE]
> No es probable que esta advertencia sea un problema, ya que es probable que no desee enviar la aplicación del sistema de compilación personalizada al Microsoft Store. Sin embargo, si eliges la opción de desarrollar el sistema de compilación personalizado en forma de aplicación de UWP, será una aplicación de UWP inusual en la que no podrás enviarlo al Microsoft Store. Esto se debe a que una aplicación para UWP que usa la invocación de plataforma produce un error Microsoft Store certificación. Tenga en cuenta que, en este caso, las llamadas de invocación *de plataforma solo existirán en el sistema de compilación personalizado* ; *no* en la aplicación para UWP de envío (en la que va a crear archivos PRI).

## <a name="scenario-walkthroughs"></a>Tutoriales de escenarios
|Tema|Descripción|
|-|-|
|[Escenario 1: generar un archivo PRI a partir de recursos de cadena y archivos de recursos](pri-apis-scenario-1.md)|En este escenario, crearemos una nueva aplicación para representar nuestro sistema de compilación personalizado. Vamos a crear un indexador de recursos y agregarle cadenas y otros tipos de recursos. A continuación, generaremos y volcaremos un archivo PRI.|

## <a name="important-apis"></a>API importantes
* [Referencia de indexación de recursos de paquetes (PRI)](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Temas relacionados
* [Compilar recursos manualmente con MakePri.exe](makepri-exe-command-options.md)
* [Consumir funciones DLL no administradas](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Sistema de administración de recursos](resource-management-system.md)

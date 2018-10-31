---
author: stevewhims
Description: With the package resource indexing (PRI) APIs, you can develop a custom build system for your UWP app's resources. The build system will be able to create, version, and dump PRI files to whatever level of complexity your UWP app needs.
title: API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 9b85f40fc391df764515d21ba3b334bfe068725c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5837265"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados
Con las [API de indexación de recursos de paquetes (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690), puedes desarrollar un sistema de compilación personalizado para los recursos de la aplicación para UWP. El sistema de compilación podrá crear, versionar y volcar (como XML) archivos de índice de recursos del paquete (PRI) en cualquier nivel de complejidad que necesite tu aplicación para UWP. Si tienes un sistema de compilación personalizado que actualmente utiliza la herramienta de línea de comandos MakePri.exe (consulta [Compile resources manually with MakePri.exe](makepri-exe-command-options.md)), para un aumento del control y del rendimiento, es recomendable llamar a las API de PRI en su lugar en lugar de a MakePri.exe.

Las API de PRI se introdujeron en Windows SDK para Windows 10, versión 1803. Las API adoptan la forma de las API de Windows de Win32, lo que significa que tienes algunas opciones para llamarlas. Puedes llamarlas directamente desde una aplicación de Win32 o puedes llamarlas a través de [invocación de plataforma](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) desde una aplicación .NET o incluso desde una aplicación para UWP.

Los escenarios de este tema muestran la llamada a las API de PRI de un proyecto de aplicación de consola de Windows de Visual C++ de Win32. Para obtener información general, consulta [Sistema de administración de recursos](resource-management-system.md).

El límite de tamaño de un archivo PRI es de 64kilobytes.

> [!NOTE]
> No es probable que esta advertencia sea un problema, ya que probablemente no desearás enviar tu aplicación de sistema de compilación personalizado a la Microsoft Store. Pero, si eliges la opción de desarrollar tu sistema de compilación personalizado en forma de una aplicación para UWP, será una aplicación para UWP inusual en el sentido de que no podrás enviarla a la Microsoft Store. Eso se debe a que una aplicación para UWP que usa la invocación de plataforma produce un error en la certificación de Microsoft Store. Ten en cuenta que, en este caso, las llamadas de invocación de plataforma *solo existirán en el sistema de compilación personalizado*; *no* en tu envío de aplicación para UWP (para el que vas a compilar archivos PRI).

## <a name="scenario-walkthroughs"></a>Tutoriales de escenarios
|Tema|Descripción|
|-|-|
|[Escenario 1: Generar un archivo PRI de los recursos de cadena y los archivos de activos](pri-apis-scenario-1.md)|En este escenario, crearemos una nueva aplicación para representar nuestro sistema de compilación personalizado. Crearemos un indizador de recursos y le agregaremos cadenas y otros tipos de recursos. A continuación, generaremos y volcaremos un archivo PRI.|

## <a name="important-apis"></a>API importantes
* [Referencia de la indexación de recursos de paquetes (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Temas relacionados
* [Compilar recursos manualmente con MakePri.exe](makepri-exe-command-options.md)
* [Consumir funciones DLL no administradas](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Sistema de administración de recursos](resource-management-system.md)

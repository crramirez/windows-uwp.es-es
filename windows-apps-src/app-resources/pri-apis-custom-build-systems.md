---
Description: Con las API de indexación de recursos de paquetes (PRI), puede desarrollar un sistema de compilación personalizado para los recursos de la aplicación para UWP. El sistema de compilación podrá crear, versionar y volcar archivos PRI para cualquier nivel de complejidad que necesite tu aplicación para UWP.
title: Indexación de recursos de paquetes y sistemas de compilación personalizados
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: ebbec3a89d795dde5c47d3dce76f77be06feebe2
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734930"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados
Con las [API de indexación de recursos de paquetes (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference), puedes desarrollar un sistema de compilación personalizado para los recursos de la aplicación para UWP. El sistema de compilación podrá crear y volcar (como XML) archivos de índice de recursos del paquete (PRI), así como controlar sus versiones, en cualquier nivel de complejidad que necesite la aplicación para UWP. Si tienes un sistema de compilación personalizado que actualmente utiliza la herramienta de línea de comandos MakePri.exe (consulta [Compile resources manually with MakePri.exe](makepri-exe-command-options.md)), para un aumento del control y del rendimiento, es recomendable llamar a las API de PRI en su lugar en lugar de a MakePri.exe.

Las API de PRI se introdujeron en Windows SDK para Windows 10, versión 1803. Las API adoptan la forma de las API de Windows de Win32, lo que significa que tienes algunas opciones para llamarlas. Puedes llamarlas directamente desde una aplicación de Win32 o puedes llamarlas a través de [invocación de plataforma](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) desde una aplicación .NET o incluso desde una aplicación para UWP.

Los escenarios de este tema muestran la llamada a las API de PRI de un proyecto de aplicación de consola de Windows de Visual C++ de Win32. Para obtener información general, consulta [Sistema de administración de recursos](resource-management-system.md).

El límite de tamaño de un archivo PRI es de 64 kilobytes.

> [!NOTE]
> No es probable que esta advertencia sea un problema, ya que probablemente no desearás enviar tu aplicación de sistema de compilación personalizado a la Microsoft Store. Pero, si eliges la opción de desarrollar tu sistema de compilación personalizado en forma de una aplicación para UWP, será una aplicación para UWP inusual en el sentido de que no podrás enviarla a la Microsoft Store. Eso se debe a que una aplicación para UWP que usa la invocación de plataforma produce un error en la certificación de Microsoft Store. Ten en cuenta que, en este caso, las llamadas de invocación de plataforma *solo existirán en el sistema de compilación personalizado*; *no* en tu envío de aplicación para UWP (para el que vas a compilar archivos PRI).

## <a name="scenario-walkthroughs"></a>Tutoriales de escenarios
|Tema|Descripción|
|-|-|
|[Escenario 1: generar un archivo PRI a partir de recursos de cadena y archivos de recursos](pri-apis-scenario-1.md)|En este escenario, crearemos una nueva aplicación para representar nuestro sistema de compilación personalizado. Crearemos un indizador de recursos y le agregaremos cadenas y otros tipos de recursos. A continuación, generaremos y volcaremos un archivo PRI.|

## <a name="important-apis"></a>API importantes
* [Referencia de indexación de recursos de paquetes (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Temas relacionados
* [Compilar recursos manualmente con MakePri.exe](makepri-exe-command-options.md)
* [Consumir funciones DLL no administradas](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Sistema de administración de recursos](resource-management-system.md)

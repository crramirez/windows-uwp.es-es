---
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: UWP en recursos del sistema de Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: a78d669902876cdb05a24cee421b0f95a71de31a
ms.sourcegitcommit: bac5574a1f47a5b38c984a5482272c9e49a9c91e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71100842"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP que se ejecutan en Xbox One comparten recursos con el sistema y otras aplicaciones. Los recursos disponibles para una aplicación para UWP en Xbox One dependen de si envías como una aplicación o como un juego de Programa de creadores de Xbox Live.

* Memoria máxima disponible mientras se ejecuta en primer plano:
    * Instala 1 GB
    * Deportivos 5 GB

La memoria máxima disponible para una aplicación que se ejecute en segundo plano es de 128 MB. El modo en segundo plano solo se aplica a aplicaciones simultáneas, como reproductores de música en segundo plano.  Los juegos se suspenderán y terminarán en segundo plano.


Superar estas limitaciones provocará fallos en la asignación de memoria. Para obtener más información acerca de la supervisión de memoria usa, consulta la referencia de la clase [MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Al ejecutar la aplicación o el juego desde el depurador de Visual Studio, estas restricciones de memoria no se aplican. Este límite solo es aplicable si no se ejecuta en modo de depuración.

* CPU
    * Aplicaciones: comparte de 2 a 4 núcleos de CPU según el número de aplicaciones y juegos que se ejecutan en el sistema.
    * Deportivos 4 núcleos de CPU exclusivos y 2 compartidos.

* GPU
    * Aplicaciones: comparte el 45% de GPU según el número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: acceso total a ciclos de GPU disponibles.

* Compatibilidad con DirectX
    * Instala Nivel 10 de la característica DirectX 11.
    * Deportivos Nivel 10 de la característica DirectX 12 y DirectX 11.

* Todas las aplicaciones y juegos deben tener como destino la arquitectura x64 para su desarrollo o su envío a la Store para Xbox.  

Para **desarrollo de aplicaciones**, los recursos disponibles pueden ser limitados en comparación con un PC estándar y puede variar según el número de aplicaciones y juegos que se ejecuten en el sistema.

Para el **desarrollo de juegos**, Xbox One, al igual que otras videoconsolas, es una parte especializada de hardware que requiere un kit de desarrollo específico basado en hardware para tener acceso al potencial completo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware Xbox One, considere registrar con el programa [ID@Xbox](https://www.xbox.com/Developers/id) para acceder a los kits de desarrollo de Xbox One.

## <a name="see-also"></a>Vea también
- [UWP en Xbox One](index.md)
- [Introducción al programa de creadores de Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX y UWP en Xbox One](https://walbourn.github.io/)
---
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: UWP en recursos del sistema de Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8cc6ca24453be83f5c10cc6c86c508a5a3f99c4c
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131920"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP que se ejecutan en Xbox One comparten recursos con el sistema y otras aplicaciones. Los recursos disponibles para una aplicación para UWP en Xbox One dependen de si envías como una aplicación o como un juego de Programa de creadores de Xbox Live.

* Memoria máxima disponible mientras se ejecuta en primer plano:
    * Aplicaciones: 1 GB
    * Juegos: 5 GB

La memoria máxima disponible para una aplicación que se ejecute en segundo plano es de 128 MB. El modo en segundo plano solo se aplica a aplicaciones simultáneas, como reproductores de música en segundo plano.  Los juegos se suspenderán y terminarán en segundo plano.

Superar estas limitaciones provocará fallos en la asignación de memoria. Para obtener más información acerca de la supervisión de memoria usa, consulta la referencia de la clase [MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Cuando se ejecuta la aplicación o juego desde el depurador de Visual Studio, no se aplican estas restricciones de memoria. Este límite solo es aplicable si no se ejecuta en modo de depuración.

* CPU
    * Aplicaciones: comparte de 2 a 4 núcleos de CPU según el número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: 4 2 y exclusivo compartido núcleos de CPU.

* GPU
    * Aplicaciones: comparte el 45% de GPU según el número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: acceso total a ciclos de GPU disponibles.

* Compatibilidad con DirectX
    * Aplicaciones: DirectX 11 Feature Level 10.
    * Juegos: DirectX 12 y el nivel de características de DirectX 11 10.

* Todas las aplicaciones y juegos deben tener como destino la arquitectura x64 para su desarrollo o su envío a la Store para Xbox.  

Para **desarrollo de aplicaciones**, los recursos disponibles pueden ser limitados en comparación con un PC estándar y puede variar según el número de aplicaciones y juegos que se ejecuten en el sistema.

Para el **desarrollo de juegos**, Xbox One, al igual que otras videoconsolas, es una parte especializada de hardware que requiere un kit de desarrollo específico basado en hardware para tener acceso al potencial completo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware Xbox One, considere registrar con el programa [ID@Xbox](https://www.xbox.com/Developers/id) para acceder a los kits de desarrollo de Xbox One.

## <a name="see-also"></a>Vea también
- [UWP en Xbox One](index.md)
- [Empezar a trabajar con el programa de creadores de Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX y UWP en Xbox One](https://walbourn.github.io/)
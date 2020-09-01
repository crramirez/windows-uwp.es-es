---
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: Obtenga información sobre cómo las aplicaciones de UWP que se ejecutan en la consola Xbox One comparten recursos con el sistema y otras aplicaciones, y sobre los requisitos de recursos para aplicaciones o juegos UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: dbc6af56867508e3178ddd8b731af49270faf3d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174689"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP que se ejecutan en la Xbox One comparten recursos con el sistema y otras aplicaciones. Los recursos disponibles para una aplicación de UWP en Xbox One dependen de si se envían como una aplicación o como un juego de programas de Xbox Live Creators.

* Memoria máxima disponible mientras se ejecuta en primer plano:
    * Aplicaciones: 1 GB
    * Juegos: 5 GB

La memoria máxima disponible para una aplicación que se ejecute en segundo plano es de 128 MB. El modo en segundo plano solo se aplica a las aplicaciones simultáneas, como reproductores de música en segundo plano.  Los juegos se suspenderán y finalizarán en segundo plano.


Si se superan estas limitaciones, se producirán errores de asignación de memoria. Para obtener más información acerca de la supervisión de memoria usa, consulta la referencia de la clase [MemoryManager](/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Al ejecutar la aplicación o el juego desde el depurador de Visual Studio, estas restricciones de memoria no se aplican. Este límite solo es aplicable si no se ejecuta en modo de depuración.

* CPU
    * Aplicaciones: recursos compartidos de núcleos de CPU de 2-4 en función del número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: 4 núcleos de CPU exclusivos y 2 compartidos.

* GPU
    * Aplicaciones: recurso compartido del 45% de la GPU en función del número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: acceso completo a los ciclos de GPU disponibles.

* Compatibilidad con DirectX
    * Aplicaciones: nivel 10 de la característica DirectX 11.
    * Juegos: nivel de característica 10 de DirectX 12 y DirectX 11.

* Todas las aplicaciones y juegos deben tener como destino la arquitectura x64 para poder desarrollarse o enviarse a la tienda para Xbox.  

Para el **desarrollo de aplicaciones**, los recursos disponibles pueden estar limitados en comparación con un equipo estándar y pueden variar en función del número de aplicaciones y juegos que se ejecutan en el sistema.

En el **desarrollo de juegos**, Xbox One, al igual que otras consolas de juegos, es un elemento especializado de hardware que requiere un kit de desarrollo basado en hardware específico para tener acceso a todo su potencial. Si está trabajando en un juego que requiere acceso al máximo potencial del hardware de Xbox One, considere la posibilidad de registrarse con el [ID@Xbox](https://www.xbox.com/Developers/id) programa para obtener acceso a un kit de desarrollo de Xbox One.

## <a name="see-also"></a>Vea también
- [UWP en Xbox One](index.md)
- [Introducción al programa de creadores de Xbox Live](/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX y UWP en Xbox One](https://walbourn.github.io/)
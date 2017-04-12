---
author: Mtoepke
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: Recursos del sistema de UWP en Xbox
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.openlocfilehash: 81a437807332ff60a1401c00abdcff78e41a7496
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP y los juegos que se ejecutan en Xbox One comparten recursos con el sistema y otras aplicaciones. Por lo tanto, los juegos y las aplicaciones para UWP tendrán acceso a los siguientes recursos:

* La memoria máxima disponible para una aplicación que se ejecute en primer plano es de 1GB.
    * La memoria máxima disponible para una aplicación que se ejecute en segundo plano es de 128MB.
    * Las aplicaciones que superen estos requisitos de memoria se encontrarán con errores de asignación de memoria. Para obtener más información acerca de la supervisión de memoria usa, consulta la referencia de la clase [MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx).
    
    > [!NOTE]
    > Cuando se ejecuta la aplicación o el juego desde el depurador de Visual Studio, no se aplican las restricciones de memoria. Este límite solo es aplicable si no se ejecuta en modo de depuración.

* Comparte de 2 a 4 núcleos de CPU según el número de aplicaciones y juegos que se ejecutan en el sistema.

* Comparte el 45% del GPU según el número de aplicaciones y juegos que se ejecutan en el sistema.

* UWP en Xbox One admite el nivel de característica 10 de DirectX 11. De momento no se admite DirectX 12.

* Todas las aplicaciones deben tener como destino la arquitectura x64 para su desarrollo o su envío a la Tienda para Xbox.  

Para el **desarrollo de aplicaciones**, es importante tener en cuenta que los recursos disponibles pueden ser limitados en comparación con un equipo estándar.

Para el **desarrollo de juegos**, es importante tener en cuenta que Xbox One, al igual que otras videoconsolas, es una parte especializada de hardware que requiere un kit de desarrollo específico basado en hardware para tener acceso al potencial completo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware Xbox One, puedes registrarte con el programa [ID@Xbox](http://www.xbox.com/Developers/id) para acceder a los kits de desarrollo de Xbox One, que incluye compatibilidad con DirectX 12.

## <a name="see-also"></a>Consulta también
- [UWP en Xbox One](index.md)

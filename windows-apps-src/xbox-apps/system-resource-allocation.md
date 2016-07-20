---
author: Mtoepke
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: UWP en recursos del sistema de Xbox
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 5c5947239e16d883511e56c62f5267568c3d5feb

---

# Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP y los juegos que se ejecutan en Xbox One comparten recursos con el sistema y otras aplicaciones. Por lo tanto, los juegos y las aplicaciones para UWP tendrán acceso a los siguientes recursos:

* En esta versión preliminar, la memoria máxima disponible para una aplicación que se ejecute en primer plano es de 1GB.
    * La memoria máxima disponible para una aplicación que se ejecute en segundo plano es de 128MB.
    * Las aplicaciones que superen estos requisitos de memoria se encontrarán con errores de asignación de memoria. Para obtener más información acerca de la supervisión de memoria usa, consulta la referencia de la clase [MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) referencia.
    * 
              **Nota**&nbsp;&nbsp;Si la aplicación o el juego se ejecutan desde el depurador de Visual Studio, estas restricciones de memoria no se aplican. Este límite solo es aplicable si no se ejecuta en modo de depuración.

* Comparte de 2 a 4 núcleos de CPU según el número de aplicaciones y juegos que se ejecutan en el sistema.

* Comparte el 45% del GPU según el número de aplicaciones y juegos que se ejecutan en el sistema.

* UWP en Xbox One admite el nivel de característica 10 de DirectX 11. De momento no se admite DirectX 12. 

Para el **desarrollo de aplicaciones**, es importante tener en cuenta que los recursos disponibles pueden ser limitados en comparación con un equipo estándar.

Para el **desarrollo de juegos**, es importante tener en cuenta que Xbox One, al igual que otras videoconsolas, es una parte especializada de hardware que requiere un kit de desarrollo específico basado en hardware para tener acceso al potencial completo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware Xbox One, puedes registrarte con el programa [ID@XBOX](http://www.xbox.com/Developers/id) para obtener acceso a los kits de desarrollo de Xbox One, que incluye compatibilidad con DirectX 12.



<!--HONumber=Jul16_HO2-->



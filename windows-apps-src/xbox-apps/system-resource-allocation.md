---
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: UWP en recursos del sistema de Xbox
area: Xbox
---

# Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP y los juegos que se ejecutan en Xbox One comparten recursos con el sistema y otras aplicaciones. 
Por lo tanto, los juegos y aplicaciones para UWP tendrán acceso a los siguientes recursos:

* En esta vista previa, la memoria máxima disponible es de 448 MB.
    * En versiones futuras, la memoria máxima disponible será de 1 GB.
    * Cuando se ejecuta la aplicación o el juego desde el depurador de Visual Studio, no se aplican las restricciones de memoria. Este límite solo es aplicable si no se ejecuta en modo de depuración.

* Comparte de 2 a 4 núcleos de CPU según el número de aplicaciones y juegos que se ejecutan en el sistema.

* Comparte el 45% del GPU según el número de aplicaciones y juegos que se ejecutan en el sistema.

* UWP en Xbox One admite el nivel de característica 10 de DirectX 11. De momento no se admite DirectX 12. 

Para el **desarrollo de aplicaciones**, es importante tener en cuenta que los recursos disponibles pueden ser limitados en comparación con un equipo estándar.

Para el **desarrollo de juegos**, es importante tener en cuenta que Xbox One, al igual que otras videoconsolas, 
es una parte especializada de hardware que requiere un kit de desarrollo específico basado en hardware para tener acceso al potencial completo. 
Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware de Xbox One, 
puedes registrarte con el programa [ID@Xbox](http://www.xbox.com/en-us/Developers/id) para obtener acceso a kits de desarrollo de Xbox One, que incluyen compatibilidad con DirectX 12.


<!--HONumber=Mar16_HO5-->



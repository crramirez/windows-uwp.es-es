---
title: Llevar el juego de Unity a Xbox One
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# Llevar el juego de Unity a Xbox One

En este tutorial paso a paso, damos por hecho que ya tienes un juego en Unity listo para compilar e implementar.

[Versión en vídeo de este tutorial.](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## Paso 0: Asegurarse de que Unity esté correctamente instalado

Al instalar Unity, deben seleccionarse estos componentes:

![Componentes de instalación de Unity](images/unity-install-components.png)

## Paso 1: Compilar la solución para UWP

En el proyecto de juego de Unity, abre las ventanas Configuración de compilación ubicadas en `File -> Build Settings...` y ve al menú de opciones de la Tienda Windows que se muestra a continuación.

![Ventana Configuración de compilación](images/build-settings.png)

Asegúrate de que la opción `SDK` esté establecida en `Universal 10`. A continuación pulsa el botón Compilar de la parte inferior del menú y se abrirá una ventana del explorador que te pedirá que especifiques una carpeta de destino. Crea una carpeta denominada `UWP` en el directorio `Assets` del proyecto y elige esta carpeta como carpeta de destino de la compilación.

![Carpeta de destino de la compilación](images/build-destination.png)

Unity ha creado una nueva solución de Visual Studio que se usará para implementar el juego para UWP a partir del próximo paso.

![Solución de VS para UWP](images/uwp-vs-solution.png)

## Paso 2: Implementar el juego

Abre la solución que se acaba de generar en la carpeta `Assets/UWP`.  Una vez abierta, cambia la plataforma de destino a x64.

![Plataforma de compilación x64](images/x64-build-platform.png)

Ahora que tienes una solución de Visual Studio para UWP para tu juego, [si sigues estos pasos](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started) podrás implementar correctamente el juego en tu Xbox One comercial.

## Notas del desarrollador

- Se recomienda ignorar la carpeta UWP en el control de versiones. Si quieres incluir elementos XAML adicionales en el proyecto y necesitas crear versiones de algunos activos dentro de la carpeta UWP, consulta [estas muestras para hacerlo](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

- Si realizas cambios en Unity en cualquier contenido (excepto los scripts) incluido en la compilación del juego, tendrás que volver a compilar la solución para UWP para ver los cambios la próxima vez que la implementes. Esto ocurre porque, durante el paso de compilación de Unity, todos los activos del proyecto se compilan en un archivo de recursos. La solución para UWP hace referencia a ese archivo de recursos generado cuando implementa el juego.




<!--HONumber=Jun16_HO5-->



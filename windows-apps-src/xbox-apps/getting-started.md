---
author: Mtoepke
title: "Introducción al desarrollo de aplicaciones para UWP en Xbox One"
description: "Cómo configurar el equipo y Xbox One para el desarrollo para UWP."
translationtype: Human Translation
ms.sourcegitcommit: d4ef0da606c98c5eb024f349720fe9641e4a541e
ms.openlocfilehash: 33b8369be9cc0fd54ee044ec2aa6ea38b352c908

---

#Introducción al desarrollo de aplicaciones para UWP en Xbox One

Sigue estos pasos **atentamente** para configurar correctamente el equipo y Xbox One para el desarrollo para la Plataforma universal de Windows (UWP). Una vez lo hayas configurado todo, podrás obtener más información sobre el modo de desarrollador en Xbox One y la compilación de aplicaciones para UWP en la página [UWP para Xbox One](index.md). 

## Antes de empezar
Antes de empezar, debes hacer lo siguiente:
-   Configurar un equipo con Windows 10.
-   Instalar Microsoft Visual Studio 2015 Update 3.
- Disponer, como mínimo, de cinco gigabytes de espacio libre en la consola Xbox One.

## Configurar el equipo de desarrollo
1.  Instala Visual Studio 2015 Update. Asegúrate de que eliges la instalación **Personalizada** y de que seleccionas la casilla **Herramientas de desarrollo de aplicaciones universales de Windows**: no forma parte de la instalación predeterminada. Si eres un desarrollador de C++, asegúrate de elegir **Instalación personalizada** y seleccionar **C++**. Para obtener más información, consulta [Configuración del entorno de desarrollo](development-environment-setup.md). 

2.  Instala el SDK más reciente de Windows 10. Puedes hacerlo desde [https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk).

3.  Habilita el modo de desarrollador para el equipo de desarrollo (Configuración/Actualización y seguridad/Para desarrolladores/Modo de desarrollador).

## Configurar la consola Xbox One
1.  Activa el modo de desarrollador en tu Xbox One. Descarga la aplicación, obtén el código de activación y, después, escríbelo en la página xboxactivate de tu cuenta del Centro de desarrollo. Para obtener más información, consulta [Activación del modo de desarrollador de Xbox One](devkit-activation.md). 

2.  Ve a la aplicación Dev Mode Activation y selecciona **Switch and restart**. Enhorabuena, ahora tienes una consola Xbox One en modo de desarrollador.
  
  > [!NOTE]
  > Tus aplicaciones y juegos de versión comercial no se ejecutarán en modo de desarrollador, pero sí que lo harán las aplicaciones o juegos que crees. Vuelve al Modo comercial para ejecutar tus aplicaciones y juegos favoritos.
    
  > [!NOTE]
  > Para poder implementar una aplicación en tu Xbox One en el modo de desarrollador, es necesario contar con un usuario con la sesión iniciada en la consola. Puedes usar tu cuenta de Xbox Live existente o crear una cuenta nueva para la consola en el modo de desarrollador. 

## Crear tu primer proyecto en Visual Studio 2015

Para obtener información más detallada, consulta [Configuración del entorno de desarrollo](development-environment-setup.md).

1.  **Para C#**: crea un nuevo proyecto universal de Windows, accede a las propiedades del proyecto y selecciona la pestaña **Depurar**. Después, cambia el **Dispositivo de destino** a **Máquina remota**, escribe la dirección IP o el nombre de host de la consola Xbox One en el campo **Máquina remota** y selecciona **Universal (protocolo sin cifrar)** en la lista desplegable **Modo de autenticación**.   

    Para buscar la dirección IP de Xbox One, inicia la Página principal para desarrolladores en la consola (el icono grande del lado derecho de la página principal) y mira a la esquina superior izquierda. Para obtener más información sobre la página principal para desarrolladores, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).  

2.  **Para proyectos de C++ y HTML o Javascript**: sigue una ruta similar, pero en las propiedades del proyecto, ve a la pestaña **Depuración**, selecciona **Máquina remota** en el Depurador para abrir la lista desplegable, escribe la dirección IP o el nombre de host de la consola en el campo **Nombre de la máquina** y, después, selecciona **Universal (protocolo sin cifrar)** en el campo **Tipo de autenticación**.
   
3.  Al presionar F5, la aplicación se compila y se empieza a implementar en tu Xbox One.
  
4.  La primera vez que hagas esto, Visual Studio te pedirá un PIN para Xbox One. Para obtener un PIN, inicia la Página principal para desarrolladores en Xbox One y selecciona el botón **Pair with Visual Studio**.
  
5.  Una vez hayas realizado el emparejamiento, la aplicación empezará a implementarse. La primera vez que lo hagas, es posible que el proceso sea poco lento (tenemos que copiar todas las herramientas en la consola Xbox), pero si el proceso tarda más de unos pocos minutos, probablemente significa que se ha producido un error. Asegúrate de haber seguido los pasos anteriores (especialmente de haber configurado el **Modo de autenticación** en **Universal**) y de usar una conexión de red de cable en tu Xbox One.  

6. Siéntate y relájate. Disfruta de la primera aplicación que se ejecuta en la consola.  

## Eso es todo.

![Hello World](images/getting-started-hello-world.png)

## Consulta también  
- [Preguntas más frecuentes](frequently-asked-questions.md)  
- [Problemas conocidos](known-issues.md)
- [UWP en Xbox One](index.md) 



<!--HONumber=Aug16_HO4-->



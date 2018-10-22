---
author: Mtoepke
title: Introducción al desarrollo de aplicaciones para UWP en Xbox One
description: Cómo configurar el equipo y Xbox One para el desarrollo para UWP.
ms.author: scotmi
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da260b4f9f5f50d97d39af883217dfbae91a566e
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5403148"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Introducción al desarrollo de aplicaciones para UWP en Xbox One

Sigue estos pasos **atentamente** para configurar correctamente el equipo y Xbox One para el desarrollo para la Plataforma universal de Windows (UWP). Una vez lo hayas configurado todo, podrás obtener más información sobre el modo de desarrollador en Xbox One y la compilación de aplicaciones para UWP en la página [UWP para Xbox One](index.md). 

## <a name="before-you-start"></a>Antes de empezar

Antes de empezar, debes hacer lo siguiente:
-   Configurar un equipo con la versión más reciente de Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2017.

    > [!NOTE]
    > Visual Studio 2017 is required if you are using the Windows 10, build 15063 SDK. -->

- Disponer, como mínimo, de cinco gigabytes de espacio libre en la consola Xbox One.

## <a name="setting-up-your-development-pc"></a>Configurar el equipo de desarrollo

1.  Instalar Visual Studio 2015 Update 3 o Visual Studio 2017.

    Si vas a instalar Visual Studio 2015 Update 3, asegúrate de que elija la instalación **personalizada** y selecciona la casilla de verificación de **Herramientas de desarrollo de aplicaciones universales de Windows** , no es parte de la instalación predeterminada. Si eres un desarrollador de C++, asegúrate de elegir **Instalación personalizada** y seleccionar **C++**.

    Si estás instalando Visual Studio 2017, debes asegurarte de que eliges la carga de trabajo **Desarrollo de la Plataforma universal de Windows**. Si eres un desarrollador de C++, en el panel de **Resumen** de la derecha, en **desarrollo de la plataforma Universal de Windows**, asegúrate de que seleccionas la casilla de verificación de **Herramientas de la plataforma Universal de Windows de C++** . No es parte de la instalación predeterminada.

    Para obtener más información, consulta [Configurar el UWP en el entorno de desarrollo de Xbox](development-environment-setup.md).

2.  Instalar el [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)de más reciente.

3.  Habilitar el modo de desarrollador para el equipo de desarrollo (**configuración / actualización y seguridad / para desarrolladores o usar las funciones para desarrolladores / modo de desarrollador**).

Ahora que el PC de desarrollo está listo, ve este vídeo o sigue leyendo para ver cómo configurar tu Xbox One para desarrollar y crear e implementar una aplicación para UWP para ella.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configurar la consola Xbox One

1.  Activa el modo de desarrollador en tu Xbox One. Descargar la aplicación, Obtén el código de activación y, a continuación, se escribe en la página [administrar consolas Xbox One](https://partner.microsoft.com/xboxactivate) en tu cuenta del centro de desarrollo. Para obtener más información, consulta [Activación del modo de desarrollador de Xbox One](devkit-activation.md). 

2.  Abrir la aplicación **Dev Mode Activation** y selecciona el **conmutador y el reinicio**. Enhorabuena, ahora tienes una consola Xbox One en modo de desarrollador.
  
  > [!NOTE]
  > Tus aplicaciones y juegos de versión comercial no se ejecutarán en modo de desarrollador, pero sí que lo harán las aplicaciones o juegos que crees. Vuelve al Modo comercial para ejecutar tus aplicaciones y juegos favoritos.
    
  > [!NOTE]
  > Para poder implementar una aplicación en tu Xbox One en el modo de desarrollador, es necesario contar con un usuario con la sesión iniciada en la consola. Puedes usar tu cuenta de Xbox Live existente o crear una cuenta nueva para la consola en el modo de desarrollador. 

## <a name="creating-your-first-project-in-visual-studio"></a>Crear tu primer proyecto en Visual Studio

Para obtener más información, consulta [Configurar el UWP en el entorno de desarrollo de Xbox](development-environment-setup.md).

1.  **Para C#**: crear un nuevo proyecto Universal de Windows y en el **Explorador de soluciones**, haz clic en el proyecto y selecciona **Propiedades**. Selecciona la pestaña **Depurar** , cambiar el **dispositivo de destino** en el **Equipo remoto**, escribe la dirección IP o el nombre de host de la consola Xbox One en el campo de la **máquina remota** y selecciona **Universal (protocolo sin cifrar)** en la ** Modo de autenticación** lista desplegable.   

    Para buscar la dirección IP de Xbox One, inicia la Página principal para desarrolladores en la consola (el icono grande del lado derecho de la página principal) y mira a la esquina superior izquierda. Para obtener más información sobre la página principal para desarrolladores, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).  

2.  **Para C++ y HTML/Javascript proyectos**: sigue una ruta similar a proyectos de C#, pero en las propiedades del proyecto, ve a la pestaña **Depurar** , selecciona **Máquina remota** en el depurador para abrir la lista desplegable, escribe la dirección IP o el nombre de host de la consola en el campo de **Nombre de la máquina** y selecciona **Universal (protocolo sin cifrar)** en el campo de **Tipo de autenticación** .

3. Seleccione **x64** en la lista desplegable a la izquierda del botón verde de reproducción de la barra de menús superior.
   
4.  Al presionar F5, la aplicación se compila y se empieza a implementar en tu Xbox One.
  
5.  La primera vez que hagas esto, Visual Studio te pedirá un PIN para Xbox One. Puedes obtener un PIN, inicia Dev Home en tu Xbox One y selecciona el botón de **pin de mostrar Visual Studio** .
  
6.  Una vez hayas realizado el emparejamiento, la aplicación empezará a implementarse. La primera vez que lo hagas, es posible que el proceso sea poco lento (tenemos que copiar todas las herramientas en la consola Xbox), pero si el proceso tarda más de unos pocos minutos, probablemente significa que se ha producido un error. Asegúrate de haber seguido los pasos anteriores (especialmente de haber configurado el **Modo de autenticación** en **Universal**) y de usar una conexión de red de cable en tu Xbox One.  

7. Siéntate y relájate. Disfruta de la primera aplicación que se ejecuta en la consola.  

## <a name="thats-it"></a>Eso es todo.

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Consulta también  
- [Preguntas más frecuentes](frequently-asked-questions.md)  
- [Problemas conocidos de UWP en el Programa para desarrolladores de Xbox](known-issues.md)
- [UWP en Xbox One](index.md) 

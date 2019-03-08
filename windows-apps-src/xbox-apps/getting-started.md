---
title: Introducción al desarrollo de aplicaciones para UWP en Xbox One
description: Cómo configurar el equipo y Xbox One para el desarrollo para UWP.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c94d27e87853b570268e3a39fe941c817b3eda6a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590980"
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

1.  Instale Visual Studio 2015 Update 3 o Visual Studio 2017.

    Si va a instalar Visual Studio 2015 Update 3, asegúrese de que elige **personalizado** instalar y seleccione el **herramientas de desarrollo de aplicaciones Windows universales** casilla de verificación: no es parte del valor predeterminado de instalación. Si eres un desarrollador de C++, asegúrate de elegir **Instalación personalizada** y seleccionar **C++**.

    Si estás instalando Visual Studio 2017, debes asegurarte de que eliges la carga de trabajo **Desarrollo de la Plataforma universal de Windows**. Si es desarrollador de C++, en el **resumen** panel de la derecha, bajo **desarrollo de plataforma Universal de Windows**, asegúrese de que selecciona el **herramientas de C++ Universal Windows Platform** casilla de verificación. No es parte de la instalación predeterminada.

    Para obtener más información, consulte [configurar su UWP en el entorno de desarrollo de Xbox](development-environment-setup.md).

2.  Instale la última versión [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Habilitar el modo de programador para el equipo de desarrollo (**configuración / actualización y seguridad / para los desarrolladores o usar características del desarrollador / modo de programador**).

Ahora que el PC de desarrollo está listo, ve este vídeo o sigue leyendo para ver cómo configurar tu Xbox One para desarrollar y crear e implementar una aplicación para UWP para ella.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configurar la consola Xbox One

1.  Activa el modo de desarrollador en tu Xbox One. Descargar la aplicación, obtener el código de activación y, a continuación, escríbala en el **consolas Xbox One administrar** página en su cuenta de desarrollador de la aplicación de centro de partners. Para obtener más información, consulta [Activación del modo de desarrollador de Xbox One](devkit-activation.md). 

2.  Abra el **activación del modo de desarrollo** aplicación y seleccione **conmutador y reinicie**. Enhorabuena, ahora tienes una consola Xbox One en modo de desarrollador.
  
  > [!NOTE]
  > Tus aplicaciones y juegos de versión comercial no se ejecutarán en modo de desarrollador, pero sí que lo harán las aplicaciones o juegos que crees. Vuelve al Modo comercial para ejecutar tus aplicaciones y juegos favoritos.
    
  > [!NOTE]
  > Para poder implementar una aplicación en tu Xbox One en el modo de desarrollador, es necesario contar con un usuario con la sesión iniciada en la consola. Puedes usar tu cuenta de Xbox Live existente o crear una cuenta nueva para la consola en el modo de desarrollador. 

## <a name="creating-your-first-project-in-visual-studio"></a>Crear su primer proyecto en Visual Studio

Para obtener más información, consulte [configurar su UWP en el entorno de desarrollo de Xbox](development-environment-setup.md).

1.  **Para C#** : Cree un nuevo proyecto de Windows Universal y en el **el Explorador de soluciones**, haga clic en el proyecto y seleccione **propiedades**. Seleccione el **depurar** , modifique **dispositivo de destino** a **máquina remota**, escriba la dirección IP o nombre de host de la consola Xbox One en el **máquina remota**  campo y, a continuación, seleccione **Universal (protocolo sin cifrar)** en el **modo de autenticación** lista desplegable.   

    Para buscar la dirección IP de Xbox One, inicia la Página principal para desarrolladores en la consola (el icono grande del lado derecho de la página principal) y mira a la esquina superior izquierda. Para obtener más información sobre la página principal para desarrolladores, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).  

2.  **Para proyectos de C++ y HTML/Javascript**: Siga una ruta de acceso similar a C# proyectos, pero en las propiedades del proyecto que se vaya a la **depuración** ficha, seleccione **máquina remota** en el depurador para abrir la lista desplegable, escriba la dirección IP o nombre de host de la consola en el **nombre de la máquina** campo y, a continuación, seleccione **Universal (protocolo sin cifrar)** en el **tipo de autenticación** campo.

3. Seleccione **x64** en la lista desplegable a la izquierda del botón verde de reproducción en la barra de menús superior.
   
4.  Al presionar F5, la aplicación se compila y se empieza a implementar en tu Xbox One.
  
5.  La primera vez que hagas esto, Visual Studio te pedirá un PIN para Xbox One. Puede obtener un PIN iniciando principal de desarrollo en Xbox One y seleccionando el **pin se muestra Visual Studio** botón.
  
6.  Una vez hayas realizado el emparejamiento, la aplicación empezará a implementarse. La primera vez que lo hagas, es posible que el proceso sea poco lento (tenemos que copiar todas las herramientas en la consola Xbox), pero si el proceso tarda más de unos pocos minutos, probablemente significa que se ha producido un error. Asegúrate de haber seguido los pasos anteriores (especialmente de haber configurado el **Modo de autenticación** en **Universal**) y de usar una conexión de red de cable en tu Xbox One.  

7. Siéntate y relájate. Disfruta de la primera aplicación que se ejecuta en la consola.  

## <a name="thats-it"></a>Eso es todo.

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Consulte también  
- [Preguntas más frecuentes](frequently-asked-questions.md)  
- [Problemas conocidos de UWP en el programa para desarrolladores de Xbox](known-issues.md)
- [UWP en Xbox One](index.md) 

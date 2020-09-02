---
title: Introducción al desarrollo de aplicaciones para UWP en Xbox One
description: Obtenga información sobre cómo configurar el equipo de desarrollo y la consola Xbox One para empezar a trabajar con el desarrollo de aplicaciones Plataforma universal de Windows (UWP) en Xbox One.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 243f8972f1ea5879ee77f036d97c05e29d446b12
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304707"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Introducción al desarrollo de aplicaciones para UWP en Xbox One

Sigue estos pasos **atentamente** para configurar correctamente el equipo y Xbox One para el desarrollo para la Plataforma universal de Windows (UWP). Una vez lo hayas configurado todo, podrás obtener más información sobre el modo de desarrollador en Xbox One y la compilación de aplicaciones para UWP en la página [UWP para Xbox One](index.md). 

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, debes hacer lo siguiente:
-   Configure un equipo con la versión más reciente de Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- Disponer, como mínimo, de cinco gigabytes de espacio libre en la consola Xbox One.

## <a name="setting-up-your-development-pc"></a>Configurar el equipo de desarrollo

1.  Instale Visual Studio 2015 Update 3, Visual Studio 2017 o Visual Studio 2019.

    Si va a instalar Visual Studio 2015 Update 3, asegúrese de elegir instalación **personalizada** y activar la casilla **herramientas de desarrollo de aplicaciones universales de Windows** : no forma parte de la instalación predeterminada. Si eres un desarrollador de C++, asegúrate de elegir **Instalación personalizada** y seleccionar **C++**.

    Si va a instalar Visual Studio 2017 o Visual Studio 2019, asegúrese de elegir la carga de trabajo de **desarrollo de plataforma universal de Windows** . Si es desarrollador de C++, en el panel **Resumen** de la derecha, en **plataforma universal de Windows desarrollo**, asegúrese de activar la casilla herramientas de **c++ plataforma universal de Windows** . No forma parte de la instalación predeterminada.

    Para obtener más información, consulte [configuración de la UWP en el entorno de desarrollo de Xbox](development-environment-setup.md).

2.  Instale el [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)más reciente.

3.  Habilite el modo de desarrollador para el equipo de desarrollo (**configuración/actualizar & seguridad/para desarrolladores/usar características para desarrolladores/modo de desarrollador**).

Ahora que el equipo de desarrollo está listo, puede ver este vídeo o seguir leyendo para ver cómo configurar la consola Xbox One para el desarrollo y crear e implementar una aplicación para UWP en ella.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configurar la consola Xbox One

1.  Activa el modo de desarrollador en tu Xbox One. Descargue la aplicación, obtenga el código de activación y, después, escríbalo en la página **administrar consolas de Xbox One** de la cuenta de desarrollador de la aplicación del centro de Partners. Para obtener más información, consulte [activación del modo de desarrollador de Xbox One](devkit-activation.md). 

2.  Abra la aplicación de **activación en modo de desarrollo** y seleccione **cambiar y reiniciar**. Enhorabuena, ahora tienes una consola Xbox One en modo de desarrollador.
  
  > [!NOTE]
  > Tus aplicaciones y juegos de versión comercial no se ejecutarán en modo de desarrollador, pero sí que lo harán las aplicaciones o juegos que crees. Vuelve al Modo comercial para ejecutar tus aplicaciones y juegos favoritos.
    
  > [!NOTE]
  > Para poder implementar una aplicación en tu Xbox One en el modo de desarrollador, es necesario contar con un usuario con la sesión iniciada en la consola. Puedes usar tu cuenta de Xbox Live existente o crear una cuenta nueva para la consola en el modo de desarrollador. 

## <a name="creating-your-first-project-in-visual-studio"></a>Crear su primer proyecto en Visual Studio

Para obtener información más detallada, consulte [configurar el entorno de desarrollo de la Xbox en la consola de UWP](development-environment-setup.md).

1.  **Para C#**: cree un nuevo proyecto universal de Windows y, en el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **propiedades**. Seleccione la **pestaña depurar** , cambiar **dispositivo de destino** a **equipo remoto**, escriba la dirección IP o el nombre de host de la consola Xbox One en el campo **equipo remoto** y seleccione **universal (protocolo sin cifrar)** en la lista desplegable **modo de autenticación** .   

    Para buscar la dirección IP de Xbox One, inicia la Página principal para desarrolladores en la consola (el icono grande del lado derecho de la página principal) y mira a la esquina superior izquierda. Para obtener más información sobre la página principal para desarrolladores, consulta [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).  

2.  En el **caso de los proyectos de C++ y HTML/JavaScript**: debe seguir una ruta de acceso similar a la de los proyectos de C#, pero en las propiedades del proyecto, vaya a la pestaña **depuración** , seleccione **equipo remoto** en el depurador para abrir la lista desplegable, escriba la dirección IP o el nombre de host de la consola en el campo **nombre** **de la** máquina y seleccione **universal**

3. Seleccione **x64** en la lista desplegable situada a la izquierda del botón verde de reproducción en la barra de menús superior.
   
4.  Al presionar F5, la aplicación se compila y se empieza a implementar en tu Xbox One.
  
5.  La primera vez que hagas esto, Visual Studio te pedirá un PIN para Xbox One. Puede obtener un PIN si inicia dev Home en la Xbox One y selecciona el botón **Mostrar PIN de Visual Studio** .
  
6.  Una vez hayas realizado el emparejamiento, la aplicación empezará a implementarse. La primera vez que lo hagas, es posible que el proceso sea poco lento (tenemos que copiar todas las herramientas en la consola Xbox), pero si el proceso tarda más de unos pocos minutos, probablemente significa que se ha producido un error. Asegúrate de haber seguido los pasos anteriores (especialmente de haber configurado el **Modo de autenticación** en **Universal**) y de usar una conexión de red de cable en tu Xbox One.  

7. Siéntate y relájate. Disfruta de la primera aplicación que se ejecuta en la consola.  

## <a name="thats-it"></a>Eso es todo.

![Hola mundo](images/getting-started-hello-world.png)

## <a name="see-also"></a>Consulte también  
- [Preguntas más frecuentes](frequently-asked-questions.md)  
- [Problemas conocidos de UWP en el Programa para desarrolladores de Xbox](known-issues.md)
- [UWP en Xbox One](index.md) 

---
author: Mtoepke
title: "Introducción al desarrollo de aplicaciones para UWP en Xbox One"
description: "Cómo configurar el equipo y Xbox One para el desarrollo para UWP."
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: b070b6bec350c3c893f934e212b1de743b349861

---

#Introducción al desarrollo de aplicaciones para UWP en Xbox One

Sigue estos pasos **atentamente** para configurar correctamente el equipo y Xbox One para el desarrollo para UWP. Una vez lo hayas configurado todo, podrás obtener más información sobre el modo de desarrollador en Xbox One y la compilación de aplicaciones para UWP en la página [UWP para Xbox One](index.md). 

## Antes de empezar
Antes de empezar, debes hacer lo siguiente:
-   Crear una cuenta del [Centro de desarrollo de Windows](https://dev.windows.com).
-   Unirte al [Programa Windows Insider](https://insider.windows.com/). Lo necesitarás para obtener la versión preliminar de Windows SDK.
-   Configurar un equipo con Windows 10 (cualquier versión será válida, incluida la versión preliminar de Windows 10 Insider más actualizada); para esta versión, nuestras herramientas de desarrollo requieren que se ejecute Windows 10. 
-   Conectar la consola Xbox One a una red. Para lograr el mejor rendimiento, usa una conexión con cable.
- Disponer como mínimo de 5 GB de espacio libre en la consola Xbox One.

## Configurar el equipo de desarrollo
1.  Instala Visual Studio 2015 Update 2. Asegúrate de que eliges la instalación **Personalizada** y de que seleccionas la casilla **Herramientas de desarrollo de aplicaciones universales de Windows**: no forma parte de la instalación predeterminada. Consulta [Configuración del entorno de desarrollo](development-environment-setup.md) para obtener más información (además, si eres desarrollador de C++, asegúrate de que eliges la instalación personalizada y de que también seleccionas C++).

2.  Instala la versión preliminar del SDK de Windows 10 más reciente. Puedes obtenerla desde el [Programa Windows Insider](http://go.microsoft.com/fwlink/p/?LinkId=780552).
  
  > **Importante**
            &nbsp;&nbsp;Si instalas este SDK de versión preliminar en el equipo, no podrás enviar aplicaciones a la tienda integrada en este equipo, por lo tanto, no lo hagas en el equipo de desarrollo de producción. 

## Configurar la consola Xbox One
1.  Activa el modo de desarrollador en tu Xbox One. Descarga la aplicación, obtén el código de activación y escríbelo en la página xboxactivate de tu cuenta del Centro de desarrollo. Consulta [Enabling developer mode on Xbox One (Habilitar el modo de desarrollador en Xbox One)](devkit-activation.md) para obtener más información. 

2.  Espera a que tu Xbox One realice una actualización del sistema para que se ejecute en Developer Preview. Este proceso puede tardar hasta 4 horas. Si no quieres esperar, reinicia en frío la consola manteniendo presionado el botón de encendido durante 10 segundos y, a continuación, vuelve a encenderla. De esta forma, se activará la actualización.  

3.  Ve a la aplicación Dev Mode Activation y selecciona **Switch and restart**. Enhorabuena, ahora tienes una consola Xbox One en modo de desarrollador.
  
  > **Nota**
            &nbsp;&nbsp;Tus aplicaciones y juegos de versión comercial no se ejecutarán en modo de desarrollador, pero sí que lo harán las aplicaciones o juegos que crees. Vuelve al Modo comercial para ejecutar tus aplicaciones y juegos favoritos.
  
  > **Nota**
            &nbsp;&nbsp;Para poder implementar una aplicación en tu Xbox One en el modo de desarrollador, es necesario contar con un usuario que haya iniciado sesión en la consola. Puedes usar tu cuenta de Xbox Live existente o crear una nueva cuenta para la consola en el modo de desarrollador. 

## Crear tu primer proyecto en Visual Studio 2015

Consulta [Configuración del entorno de desarrollo](development-environment-setup.md) para obtener información más detallada.

1.  Para C#: crea un nuevo proyecto universal de Windows, ve a las propiedades del proyecto y selecciona la pestaña **Depurar** , cambia el **Dispositivo de destino** por **Máquina remota**, escribe la dirección IP o el nombre de host de la Xbox One en el campo **Máquina remota** y, a continuación, selecciona **Universal (protocolo sin cifrar)** en la lista desplegable **Modo de autenticación**.   

    Para buscar la dirección IP de Xbox One, inicia la Página principal para desarrolladores en la consola (el icono grande del lado derecho de la página principal) y mira a la esquina superior izquierda. Consulta [Introduction to Xbox One tools (Introducción a las herramientas de Xbox One)](introduction-to-xbox-tools.md) para obtener más información sobre la página principal para desarrolladores.  

2.  En el caso de los proyectos de C++ y HTML o Javascript: sigue una ruta similar, pero en las propiedades del proyecto, vea a la pestaña **Depuración**, selecciona **Máquina remota** en el depurador para abrir la lista desplegable, escribe la dirección IP o el nombre de host de la consola en el campo **Nombre de la máquina** y, a continuación, selecciona **Universal (protocolo sin cifrar)** en el campo **Tipo de autenticación**.
   
3.  Al presionar F5, la aplicación se compila y se empieza a implementar en tu Xbox One.
  
4.  La primera vez que hagas esto, Visual Studio te pedirá un PIN para Xbox One. Para obtener un PIN, inicia la Página principal para desarrolladores en Xbox One y presiona el botón **Pair with Visual Studio**.
  
5.  Una vez hayas realizado el emparejamiento, la aplicación empezará a implementarse. La primera vez que lo hagas, es posible que el proceso sea poco lento (tenemos que copiar todas las herramientas en la consola Xbox), pero si el proceso tarda más de unos pocos minutos, probablemente significa que se ha producido un error. Asegúrate de haber seguido los pasos anteriores (especialmente de haber configurado el **Modo de autenticación** en **Universal**) y de que estés usando una conexión de red cableada en tu Xbox One.  

6. Siéntate y relájate. Disfruta de la primera aplicación que se ejecuta en la consola.  
   ![Hello World](images/getting-started-hello-world.png)
   

## Consulta también  
- [P+F](frequently-asked-questions.md)  
- [Problemas conocidos](known-issues.md)
- [UWP on Xbox One (UWP en Xbox One)](index.md)



<!--HONumber=Jun16_HO4-->



---
author: Mtoepke
title: Problemas conocidos de UWP en el Programa para desarrolladores de Xbox One
description: 
translationtype: Human Translation
ms.sourcegitcommit: 5774ada049e5f300e9cb990f5a079c8c21796f8b
ms.openlocfilehash: 5892e00f4da74af5aa4e24fdd12b0df0e8a4a7d9

---

# Problemas conocidos de UWP en el Programa para desarrolladores de Xbox

En este tema se describen los problemas conocidos de UWP en el Programa para desarrolladores de Xbox One. Para obtener más información acerca de este programa, consulta [UWP en Xbox](index.md). 

\[Si llegaste aquí desde un vínculo en un tema de referencia de API y buscas información de la API de familia de dispositivos universales, consulta [UWP features that aren't yet supported on Xbox](http://go.microsoft.com/fwlink/?LinkID=760755) (Características de UWP que aún no se admiten en Xbox).\]

La siguiente lista destaca algunos problemas conocidos que pueden aparecer en esta versión, aunque esta no es una lista exhaustiva. 

**Queremos recibir tus comentarios**, así que notifica todos los problemas que encuentres en el foro de [desarrollo de aplicaciones para la Plataforma universal de Windows](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Si sigues teniendo problemas, lee la información de este tema, consulta las [preguntas más frecuentes](frequently-asked-questions.md) y usa los foros para pedir ayuda.


<!--## Developing games-->
 
## Los límites de memoria para las aplicaciones en segundo plano se han aplicado parcialmente
 
La superficie de memoria máxima para las aplicaciones que se ejecutan en segundo plano es de 128 megabytes. En la versión actual de UWP en Xbox One, tu aplicación se suspenderá si supera este límite cuando se mueva a segundo plano. Este límite no es exigible actualmente si la aplicación supera el límite mientras se está ejecutando en segundo plano; esto significa que si tu aplicación supera los 128 MB mientras se ejecuta en segundo plano, aún podrá asignar memoria.
 
Actualmente no hay ninguna solución para este problema. Las aplicaciones deben regir su uso de memoria en consecuencia y seguir manteniéndose por debajo del límite de 128 MB mientras se ejecutan en segundo plano.
 
## La implementación desde VS sufre un error si el control parental está activado

Se producirá un error al iniciar la aplicación desde VS si la consola tiene activado el control parental en la configuración.

Para solucionar este problema, deshabilita temporalmente el control parental o:
1. Implementa tu aplicación en la consola con el control parental desactivado.
2. Activa el control parental.
3. Inicia la aplicación desde la consola.
4. Escribe un PIN o una contraseña para permitir que la aplicación se inicie.
5. La aplicación se iniciará.
6. Cierra la aplicación.
7. Inicia desde VS con F5 y la aplicación se iniciará sin pedir confirmación.

En este punto, el permiso es _permanente_ hasta que se cierra la sesión del usuario, aunque se desinstale y vuelva a instalar la aplicación.
 
Existe otro tipo de excepción que solo está disponible para cuentas de menores. Una cuenta de un menor requiere que uno de los progenitores inicie sesión para conceder permiso, pero, cuando lo hace, el progenitor tiene la opción de elegir **Siempre** y permitir que el menor inicie la aplicación. Esa excepción se almacena en la nube y se conservará, incluso si el menor cierra la sesión y vuelve a iniciarla.   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## Compatibilidad con DirectX 12

UWP en Xbox One admite el nivel de característica 10 de DirectX 11. De momento no se admite DirectX 12. 

Xbox One, al igual que todas las consolas de juegos tradicionales, es una herramienta de hardware especializada que requiere un SDK específico para alcanzar su potencial máximo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware de Xbox One, puedes inscribirte en el programa [ID@XBOX](http://www.xbox.com/Developers/id) para obtener acceso a ese SDK, que incluye la compatibilidad con DirectX 12.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 

## Puertos de red bloqueados en Xbox One

Las aplicaciones para la Plataforma universal de Windows (UWP) en dispositivos Xbox One tienen restringido el enlace a los puertos del intervalo [49152, 65535]. Aunque el enlace a estos puertos puede parecer correcto en tiempo de ejecución, el tráfico de red puede eliminarse de forma silenciosa antes de llegar a la aplicación. La aplicación se debe enlazar al puerto 0 siempre que sea posible, lo que permite al sistema seleccionar el puerto local. Si necesitas usar un puerto específico, el número de puerto debe estar en el intervalo [1025, 49151], y debes comprobar y evitar conflictos con el registro IANA. Para obtener más información, consulta [Service Name and Transport Protocol Port Number Registry](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) (Nombre de servicio y registro de número de puerto del protocolo de transporte).

## Cobertura de las APIs de UWP

En Xbox no se admiten todas las API de UWP. Para ver la lista de las API que sabemos que no funcionan, consulta [UWP features that aren't yet supported on Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755) (Características UWP que aún no se admiten en Xbox). Si encuentras problemas con otras API, notifícalo en los foros. 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


<!--## Windows Device Portal (WDP) preview-->

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

## Desplazarse a WDP genera una advertencia de certificado

Recibirás una advertencia acerca del certificado proporcionado, similar a la siguiente captura de pantalla, porque el certificado de seguridad firmado por la consola Xbox One no se considera un editor de confianza conocido. Para acceder a Windows Device Portal, haz clic en **Continuar a este sitio web**.

![Advertencia de certificado de seguridad de sitio web](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## Consulta también
- [Preguntas más frecuentes](frequently-asked-questions.md)
- [UWP en Xbox One](index.md)



<!--HONumber=Aug16_HO4-->



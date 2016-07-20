---
author: Mtoepke
title: Problemas conocidos de UWP en Xbox One Developer Preview
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e016be20af9a0d7a67fa383cbdc93083d12a1113

---

# Problemas conocidos de UWP en Xbox Developer Preview

En este tema se describen los problemas conocidos con la UWP en Xbox Developer Preview. Para obtener más información acerca de esta versión preliminar para desarrolladores, consulta [UWP en Xbox](index.md). 

\[Si llegaste aquí desde un vínculo en un tema de referencia de API y buscas información de la API de familia de dispositivos universales, consulta [UWP features that aren't yet supported on Xbox](http://go.microsoft.com/fwlink/?LinkID=760755) (Características de UWP que aún no se admiten en Xbox).\]

La actualización del sistema de Xbox Developer Preview incluye software experimental y de versión preliminar. Esto significa que algunos juegos y aplicaciones conocidos no funcionan según lo esperado y es posible que experimentes bloqueos y pérdidas de datos ocasionales. Si sales de la versión preliminar para desarrolladores, la consola restablecerá la configuración de fábrica y tendrás que volver a instalar todos los juegos, las aplicaciones y el contenido.

Para los desarrolladores, esto significa que no todas las API y las herramientas de desarrollo funcionarán según lo esperado. No se incluyen todas las características destinadas a la versión final o no tienen la calidad de la versión final. 
**En concreto, el rendimiento del sistema de esta versión preliminar no refleja el rendimiento del sistema de la versión final.**

La siguiente lista destaca algunos problemas conocidos que pueden aparecer en esta versión, aunque esta no es una lista exhaustiva. 


              **Queremos recibir tus comentarios**, así que notifica todos los problemas que encuentres en el foro [Desarrollo de aplicaciones universales de Windows](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Si sigues teniendo problemas, lee la información de este tema, consulta las [preguntas más frecuentes](frequently-asked-questions.md) y usa los foros para pedir ayuda.


<!--## Developing games-->

## Compatibilidad con el modo de mouse

A partir de esta versión preliminar,el _modo de mouse_ está habilitado de manera predeterminada para XAML y para las aplicaciones web hospedadas. Todas las aplicaciones que no lo hayan deshabilitado voluntariamente recibirán un puntero de mouse, similar al que se muestra en el navegador Edge de Xbox.

**Recomendamos encarecidamente que los desarrolladores desactiven el modo de mouse y optimicen para la navegación con el mando de dirección (X-Y).**

Para desactivar el modo de mouse en XAML, sigue este ejemplo:

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Para desactivar el modo de mouse en una aplicación HTML/JavaScript, sigue este ejemplo:

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

Para obtener más información, incluido cómo activar la navegación direccional en una aplicación HTML/JavaScript, consulta [Cómo deshabilitar el modo de mouse](how-to-disable-mouse-mode.md#html).

> 
              **Nota**&nbsp;&nbsp;En esta versión preliminar para desarrolladores, si el modo de mouse está activado, el desplazamiento con el stick derecho del mando puede provocar que la consola se bloquee. Si surge este problema, es necesario reiniciar la consola.

Para obtener información sobre la compatibilidad con el modo de mouse, consulta el tema [Diseño para Xbox y televisión](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode). Este tema incluye información acerca de cómo habilitar y deshabilitar el modo de mouse, de forma que puedas elegir el comportamiento adecuado para la aplicación.

## Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión (error 0x87e10008).

Ahora, las aplicaciones requieren que un usuario haya iniciado sesión para poder iniciarlas (debes contar con un usuario con sesión iniciada para poder iniciar la depuración (F5) desde VS 2015). El mensaje de error actual que se recibe de Visual Studio no es intuitivo:
 
![No se puede activar la aplicación de la tienda de Windows.](images/windows-store-app-activation-error.jpg)
 
Para solucionar este problema, inicia sesión con un usuario desde el Shell de Xbox o desde DevHome antes de implementar la aplicación.
 
## Aún no se han aplicado los límites de memoria para las aplicaciones en segundo plano
 
El límite de 128MB para las aplicaciones que se ejecutan en segundo plano no se aplica en esta versión preliminar. Esto significa aunque la aplicación supere los 128MB cuando se ejecute en segundo plano, podrá asignar memoria.
 
Actualmente no hay ninguna solución para este problema. El uso de memoria debería regularse de acuerdo con esto; en una futura versión preliminar, la aplicación recibirá errores de asignación de memoria si supera el límite de 128MB.
 
## La implementación desde VS sufre un error si el control parental está activado

Se producirá un error al iniciar la aplicación desde VS si la consola tiene activado el control parental en la configuración.

Para solucionar este problema, deshabilita temporalmente el control parental o:
1. Implementar la aplicación en la consola con el control parental desactivado.
2. Activa el control parental.
3. Inicia la aplicación desde la consola.
4. Escribe un PIN o una contraseña para permitir que la aplicación se inicie.
5. La aplicación se iniciará.
6. Cierra la aplicación.
7. Inicia desde VS con F5 y la aplicación se iniciará sin pedir confirmación.

En este punto, el permiso es _permanente_ hasta que se cierra la sesión del usuario, incluso si se desinstala la aplicación y vuelve a instalar.
 
Existe otro tipo de excepción que solo está disponible para cuentas de menores. Una cuenta de un menor requiere que uno de los padres inicie sesión para conceder permiso, pero cuando lo hace, el padre tiene la opción de elegir **Siempre** y permitir que el menor inicie la aplicación siempre. Esa excepción se almacena en la nube y se conservará, incluso si el menor cierra la sesión y vuelve a iniciarla.   

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

UWP en Xbox One admite el nivel de característica 10 de DirectX 11. De momento no se admite DirectX 12. Xbox One, al igual que todas las consolas de juegos tradicionales, es una herramienta de hardware especializada que requiere un SDK específico para alcanzar su potencial máximo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware de Xbox One, puedes inscribirte en el programa [ID@XBOX](http://www.xbox.com/Developers/id) para obtener acceso a ese SDK, que incluye la compatibilidad con DirectX 12.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## Problema conocido con la zona segura del televisor

De manera predeterminada, el área de visualización de las aplicaciones para UWP en Xbox debe estar encuadrada por la zona segura del televisor. Sin embargo, Xbox One Developer Preview contiene un error conocido que hace que la zona segura del televisor empiece en [0, 0] en lugar de en [_offset_, _offset_].

> 
              **Nota**&nbsp;&nbsp;Esto solo se aplica a las aplicaciones para UWP que usan JavaScript.

La forma más sencilla de solucionar este problema es deshabilitar la zona segura del televisor, como se muestra en el siguiente ejemplo de JavaScript.

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

Para obtener más información acerca de la zona segura del televisor, consulta [Diseño para Xbox y televisión](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv).

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 


## Cobertura de las API de la UWP

En Xbox no se admiten todas las API de la UWP. Consulta [Características de la UWP que aún no se admiten en Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755) para ver la lista de las API que sabemos que no funcionan. Si encuentras problemas con otras API, notifícalo en los foros. 

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


## Vista previa de Windows Device Portal (WDP)

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

### La interfaz de usuario de WDP no se muestra correctamente en Internet Explorer 7 

De manera predeterminada, la interfaz de usuario de WDP no se muestren correctamente en el navegador si se usa Internet Explorer 7. Para solucionar este problema, desactiva la Vista de compatibilidad de Internet Explorer 7 para WDP.

### Desplazarse a WDP genera una advertencia de certificado

Recibirás una advertencia acerca del certificado proporcionado, similar a la siguiente captura de pantalla, porque el certificado de seguridad firmado por la consola Xbox One no se considera un editor de confianza conocido. Haz clic en "Continuar a este sitio web" para acceder a Windows Device Portal.

![Advertencia de certificado de seguridad de sitio web](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## Consulta también
- [Preguntas más frecuentes](frequently-asked-questions.md)
- [UWP en Xbox One](index.md)



<!--HONumber=Jul16_HO2-->



---
author: Mtoepke
title: "Preguntas más frecuentes"
description: "Preguntas más frecuentes sobre UWP en Xbox."
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 01173aeabb74d6d703597a9bd7a48c179c688c07

---

# Preguntas más frecuentes

¿Las cosas no funcionan de la forma esperada? Consulta esta página de preguntas más frecuentes. Asimismo, consulta el tema [Known issues (Problemas conocidos)](known-issues.md) y el foro [Developing Universal Windows apps (Desarrollo de aplicaciones universales de Windows)](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### ¿Por qué mis juegos y aplicaciones no funcionan?

Si tus juegos y aplicaciones no funcionan, o si no tienes acceso a la tienda o a los servicios Live, probablemente estás ejecutando el modo de desarrollador. Puedes saber que estás ejecutando el modo de desarrollador, si, al seleccionar Página principal, ves un icono grande de la Página principal para desarrolladores en la parte derecha de la pantalla, en lugar del contenido habitual de Gold o Live. Si quieres jugar a juegos, puedes abrir la Página principal para desarrolladores y volver al modo comercial utilizando el botón **Leave developer mode**.

### ¿Por qué no puedo conectarme a mi Xbox One con Visual Studio?

Para empezar, asegúrate de que estés ejecutando el modo de desarrollador y no el modo comercial. No puedes conectarte a Xbox One cuando está en modo comercial. Para comprobar esto de forma simple, pulsa el botón **Página principal** y busca el icono Página principal para desarrolladores en el lado derecho de la pantalla. Si el icono no está allí, pero en su lugar ves contenido Gold o Live, significa que estás en el modo comercial. Para cambiar al modo de desarrollador, es necesario ejecutar la aplicación Dev Mode Activation.

> 
              **Nota**&nbsp;&nbsp;Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión.

Para obtener más información, consulta [Solucionar errores de implementación](frequently-asked-questions.md#fixing-deployment-failures) más adelante en esta página.

### ¿Cómo puedo alternar entre el modo comercial y el modo de desarrollador?

Sigue las instrucciones de [Xbox One Developer Mode Activation (Activación del modo de desarrollador de Xbox)](devkit-activation.md) para obtener más información sobre estos estados.

### ¿Cómo sé si estoy en modo comercial o desarrollador?

Sigue las instrucciones de [Xbox One Developer Mode Activation (Activación del modo de desarrollador de Xbox)](devkit-activation.md) para obtener más información sobre estos estados. 

Para comprobarlo de forma simple, presiona el botón **Página principal** y mira el lado derecho de la pantalla. Si estás en el modo de desarrollador, verás el icono Página principal para desarrolladores en el lado derecho. Si estás en el modo comercial, verás el contenido Gold o Live habitual.

### ¿Mis juegos y aplicaciones aún funcionarán si activo el modo de desarrollador?

Sí, puedes alternar entre el modo de desarrollador y el modo comercial, en el que puedes jugar a juegos. Para obtener más información, consulta la página [Activación del modo de desarrollador de Xbox One](devkit-activation.md). 

<!-- > **CAUTION**&nbsp;&nbsp;The Xbox Developer Preview System Update includes experimental and early pre-release software. 
This means that some popular games and apps will not work as expected and you may experience occasional crashes and data loss. -->

### ¿Perderé mis juegos y aplicaciones o los cambios guardados?

Si decides salir del programa Developer Preview, es posible que tengas que realizar un restablecimiento de fábrica, lo que borrará todo el contenido de la consola. Si esto sucede, tendrás que volver a instalar todos los juegos y aplicaciones. Siempre y cuando estés conectado mientras juegas, las partidas guardadas se almacenan en tu perfil en la nube de la cuenta de Live, por lo que no las perderás.

### ¿Cómo puedo salir de Developer Preview?

Consulta el tema [Xbox One Developer Mode Deactivation (Desactivación del modo de desarrollador de Xbox One](devkit-deactivation.md) para obtener más información acerca de cómo salir de Developer Preview.

### He vendido mi consola Xbox One y la he dejado en el modo de desarrollador. ¿Cómo puedo desactivar el modo de desarrollador?

Si ya no tienes acceso a tu Xbox One, puedes desactivarla en el Centro de desarrollo de Windows. Para obtener más información, consulta la sección **Desactivar la consola con el Centro de desarrollo de Windows** en el tema [Xbox One Developer Mode Deactivation (Desactivación del modo de desarrollador de Xbox One)](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center).

### Salí de Developer Preview con el Centro de desarrollo de Windows, pero aún estoy en el modo de desarrollador. ¿Qué puedo hacer?

Inicia la Página principal para desarrolladores y selecciona el botón **Leave developer mode**. La consola se reiniciará en modo comercial. 

### ¿Puedo publicar mi aplicación?

La publicación de aplicaciones estará disponible a través del Centro de desarrollo más adelante este año. Las aplicaciones para UWP creadas y probadas en una consola Xbox One comercial pasarán por el mismo proceso de ingesta, revisión y publicación que Windows realiza hoy en día, con revisiones adicionales para satisfacer los estándares actuales de Xbox One.

### ¿Puedo publicar mi juego?

Puedes usar UWP y Xbox One en el modo de desarrollador para crear y probar los juegos de Xbox One. Para publicar juegos para UWP, debes registrarte en [ID@XBOX](http://www.xbox.com/Developers/id). 

              [ID@Xbox](http://www.xbox.com/Developers/id) proporciona a los desarrolladores acceso completo a las API de Xbox Live para sus juegos, incluida la puntuación de jugador y los logros, así como la capacidad de aprovechar el modo multijugador entre dispositivos, almacenamientos en la nube y todas las características de Xbox Live en Xbox One. 

              [ID@XBOX](http://www.xbox.com/Developers/id) también puede proporcionar acceso a kits de desarrollo de Xbox One para juegos que requieran tener acceso al máximo potencial del hardware de Xbox One.

### ¿Los motores de juego estándar funcionarán?

Echa un vistazo a la página [Known issues (Problemas conocidos)](known-issues.md) de esta versión preliminar.

### ¿Qué capacidades y recursos del sistema hay disponibles para los juegos para UWP en Xbox One? 

Para obtener más información, consulta [System resources for UWP apps and games on Xbox One (Recursos del sistema de aplicaciones para UWP y juegos en Xbox One)](system-resource-allocation.md).

### ¿Si creo un juego para UWP de DirectX 12, se ejecutará en mi Xbox One en el modo de desarrollador?

Para obtener más información, consulta [System resources for UWP apps and games on Xbox One (Recursos del sistema de aplicaciones para UWP y juegos en Xbox One)](system-resource-allocation.md).

### ¿Toda la superficie de la API de UWP estará disponible en Xbox?

Echa un vistazo a la página [Known issues (Problemas conocidos)](known-issues.md) de esta versión preliminar.

### Solucionar errores de implementación

Si no puedes implementar la aplicación desde Visual Studio, estos pasos pueden ayudarte a solucionar el problema. Si sigues teniendo problemas, pide ayuda en el foro.

> 
              **Nota**&nbsp;&nbsp;Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión. Si recibes un mensaje de error 0x87e10008, asegúrate de que haya un usuario que haya iniciado sesión y vuelve a intentarlo.

Si Visual Studio no puede conectarse a tu Xbox One:

1. Asegúrate de que te encuentras en el modo de desarrollador (se describe anteriormente en esta página).
2. Asegúrate de que hayas configurado correctamente el equipo de desarrollo. ¿Has seguido *todas* las instrucciones de [Getting started with UWP app development on Xbox One (Introducción al desarrollo de aplicaciones para UWP en Xbox One)](getting-started.md)? 

3. Si aún no lo has hecho, lee el tema [Configuración del entorno de desarrollo](development-environment-setup.md) y el tema [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md).

4. Asegúrate de que puedes "hacer ping" a la dirección IP de la consola desde el equipo de desarrollo.
> 
              **Nota**&nbsp;&nbsp;Para obtener el mejor rendimiento de la implementación, te recomendamos que uses una conexión con cable a la consola.

5. Asegúrate de usar la opción Universal (protocolo sin cifrar) de la lista desplegable de Autenticación de la pestaña **Depurar**. Consulta [Configuración del entorno de desarrollo](development-environment-setup.md) para obtener más detalles.

<!--6. Make sure you are not hitting a PIN pairing issue; see "Visual Studio/Xbox PIN pairing failures" in the [Known Issues](known-issues.md) topic.-->

<!--
If Visual Studio can connect, but deployment is failing (for example you get this error message: "DEP0700 : Registration of the app failed.(0x80073cf9)"):

1. Make sure that your app is not installed by uninstalling it from the Collections app in the Xbox One shell. 

> **Note**&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

2. If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode, and then switch back to Developer Mode. 
This will clear Dev Storage.

3. If your issues persist, follow the steps above and then use **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.
-->

### Si voy a crear una aplicación con HTML o JavaScript, ¿cómo habilito la navegación con el mando para juegos?

TVHelpers es un conjunto de muestras y bibliotecas de JavaScript y XAML/C# para ayudarte a crear fantásticas experiencias de Xbox One y televisión en JavaScript y C#. TVJS es una biblioteca que te ayuda a compilar aplicaciones para UWP de alta gama para Xbox One. TVJS incluye compatibilidad para la navegación de controlador automática, reproducción de elementos multimedia enriquecidos, búsqueda y más. Puedes usar TVJS con tu aplicación web hospedada tan fácilmente como con una aplicación para UWP web empaquetada con acceso total a API de Windows Runtime.

Para obtener más información, consulta el proyecto [TVHelpers](https://github.com/Microsoft/TVHelpers) y la [wiki](https://github.com/Microsoft/TVHelpers/wiki) del proyecto.

## Consulta también
- [Problemas conocidos de UWP en Xbox One Developer Preview.](known-issues.md)
- [UWP on Xbox One (UWP en Xbox One)](index.md)



<!--HONumber=Jul16_HO2-->



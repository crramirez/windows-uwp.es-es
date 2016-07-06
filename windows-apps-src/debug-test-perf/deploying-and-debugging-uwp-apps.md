---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Este artículo te guiará en el procedimiento para dirigirte a distintos destinos de implementación y de depuración."
title: "Implementación y depuración de aplicaciones para la Plataforma universal de Windows (UWP)"
translationtype: Human Translation
ms.sourcegitcommit: 14f6684541716034735fbff7896348073fa55f85
ms.openlocfilehash: e2209e90080c7346bb363304b1a28f6446300332

---

# Implementación y depuración de aplicaciones para la Plataforma universal de Windows (UWP)

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artículo te guiará en el procedimiento para dirigirte a distintos destinos de implementación y de depuración.

Microsoft Visual Studio permite implementar y depurar aplicaciones para la Plataforma universal de Windows (UWP) en una variedad de dispositivos de Windows 10. Visual Studio puede controlar el proceso de creación y registro de la aplicación en el dispositivo de destino.

## Elección de un destino de implementación

Para elegir un destino, navega hasta la lista desplegable de destino de depuración junto al botón **Iniciar depuración** y selecciona el destino en el que quieres implementar la aplicación. Después de seleccionar el destino, elige **Iniciar depuración (F5)** para realizar la implementación y la depuración en ese destino o presiona **CTRL+F5** para realizar solo la implementación en ese destino.

![](images/debug-device-target-list.png)

-   **Equipo local** implementa la aplicación en el equipo de desarrollo actual. Esta opción solo está disponible si la **Versión mínima de la plataforma de destino** de la aplicación es menor o igual que el sistema operativo del equipo de desarrollo.
-   **Simulador** va a implementar la aplicación en un entorno simulado en el equipo de desarrollo actual. Esta opción solo está disponible si la **Versión mínima de la plataforma de destino** de la aplicación es menor o igual que el sistema operativo del equipo de desarrollo.
-   **Dispositivo** implementará la aplicación en un dispositivo conectado mediante USB. El dispositivo debe haberlo desbloqueado el desarrollador y debe tener la pantalla desbloqueada.
-   Un destino **Emulador** arrancará e implementará la aplicación en un emulador con la configuración especificada en el nombre. Los emuladores solo están disponibles en equipos compatibles con Hyper-V que ejecutan Windows 8.1 o posterior.
-   **Equipo remoto** te permitirá especificar un destino remoto para implementar la aplicación. Puedes encontrar más información acerca de la implementación en un equipo remoto en [Especificación de un dispositivo remoto](#specifying-a-remote-device).

## Depuración de aplicaciones implementadas
Visual Studio también se puede asociar a cualquier proceso de aplicación para UWP en ejecución seleccionando **Depurar** y, después, **Asociar al proceso**. Para la asociación a un proceso en ejecución no se requiere el proyecto de Visual Studio original, pero la carga de los [símbolos](#symbols) del proceso será de gran ayuda al depurar un proceso del que no tienes el código original.  
  
Además, cualquier paquete de la aplicación instalado se puede asociar y depurar seleccionando **Depurar**, **Otros** y, después, **Depurar paquete de aplicaciones instalado**.   
 
![Cuadro de diálogo Depurar paquete de aplicaciones instalado](images/gs-debug-uwp-apps-002.png)  

Al seleccionar **No iniciar, pero depurar mi código al empezar**, el depurador de Visual Studio se asociará a la aplicación para UWP cuando se inicie a la hora personalizada. Esta es una forma eficaz de depurar las rutas de acceso de control a partir de [distintos métodos de inicio](../xbox-apps/automate-launching-uwp-apps.md), como la activación de protocolos con parámetros personalizados.  

Las aplicaciones para UWP se pueden desarrollar y compilar en Windows 8.1 o posterior, pero requieren Windows 10 para ejecutarse. Si estás desarrollando una aplicación para UWP en un equipo con Windows 8.1, puedes depurar de forma remota una aplicación para UWP que se ejecute en otro dispositivo de Windows 10, siempre que el equipo host y el equipo de destino se encuentren en la misma LAN. Para hacerlo, descarga e instala [Herramientas remotas para Visual Studio](http://aka.ms/remotedebugger) en ambos equipos. La versión instalada debe coincidir con la versión existente de Visual Studio que hayas instalado y la arquitectura que selecciones (x86, x 64) también debe coincidir con la de aplicación de destino.   
  

## Especificación de un dispositivo remoto

### C# y Microsoft Visual Basic

Para especificar un equipo remoto para aplicaciones de C# o Microsoft Visual Basic, selecciona **Equipo remoto** en la lista desplegable de destino de depuración. El cuadro de diálogo **Conexiones remotas** aparecerá y te permitirá especificar una dirección IP o seleccionar un dispositivo detectado. De manera predeterminada, el modo de autenticación **Universal** está seleccionado. Para determinar el modo de autenticación que se usará, consulta [Modos de autenticación](#authentication-modes).

![](images/debug-remote-connections.png)

Para volver a este cuadro de diálogo, puedes abrir las propiedades del proyecto y navegar hasta la pestaña **Depurar**. Desde esta, selecciona **Buscar...** junto a **Equipo remoto:**

![](images/debug-remote-machine-config.png)

Para implementar una aplicación en un equipo remoto, también deberás descargar e instalar Herramientas remotas para Visual Studio en el equipo de destino. Consulta [Instrucciones del equipo remoto](#remote-pc-instructions) para obtener instrucciones completas.

### C++ y JavaScript

Para especificar el destino de un equipo remoto de una aplicación para UWP de C++ o JavaScript, ve a las propiedades del proyecto. Para ello, haz clic con el botón secundario en el **Explorador de soluciones** y haz clic en **Propiedades**. Navegar hasta la configuración de **Depuración** y cambia el valor de **Depurador para iniciar** a **Equipo remoto**. A continuación, rellena **Nombre del equipo** (o haz clic en **Buscar...** para encontrar uno) y establece la propiedad **Tipo de autenticación** .

![](images/debug-property-pages.png)
Después de especificar el equipo, puedes seleccionar **Equipo remoto** en la lista desplegable de destino de depuración para volver a ese equipo especificado. Solo se puede seleccionar un equipo remoto a la vez.

### Instrucciones del equipo remoto

Para implementar en un equipo remoto, el equipo de destino debe tener instalado Visual Studio Remote Tools. El equipo remoto también deben ejecutar una versión de Windows que sea mayor o igual a la propiedad **Versión mínima de la plataforma de destino** de tus aplicaciones. Una vez que has instalado las herramientas remotas, debes iniciar al depurador remoto en el equipo de destino. Para ello, busca **Depurador remoto** en el menú **Inicio** para iniciarlo, y si te lo pide, permite que el depurador configure las opciones del firewall. De manera predeterminada, el depurador se inicia con la autenticación de Windows. Esto requiere credenciales de usuario si el usuario que inició sesión no es el mismo en ambos equipos. Para cambiar a **Sin autenticación**, ve a **Herramientas** -&gt;**Opciones** en el **Depurador remoto** y establécelo en **Sin autenticación**. Una vez que el depurador remoto esté configurado, puedes implementar desde el equipo de desarrollo.

Para más información, consulta la página de descarga de [Herramientas remotas para Visual Studio]( http://go.microsoft.com/fwlink/?LinkId=717039).

## Modos de autenticación

Existen tres modos de autenticación para la implementación del equipo remoto:

- **Universal (protocolo sin cifrar)**: usa este modo de autenticación siempre que realices una implementación en un dispositivo remoto que no sea un equipo Windows (de escritorio o portátil). Actualmente, solo son los dispositivos IoT. Universal (protocolo sin cifrar) solo se debería usar en redes de confianza. La conexión de depuración es vulnerable para los usuarios malintencionados que podrían interceptar y cambiar los datos que se pasan entre el desarrollo y el equipo remoto.
- **Windows**: este modo de autenticación solo está destinado al uso en la implementación del equipo remoto (de escritorio o portátil). Usa este modo de autenticación si tienes acceso a las credenciales del usuario que inició la sesión del equipo de destino. Este es el canal más seguro para la implementación remota.
- **Ninguno**: este modo de autenticación solo está destinado al uso en la implementación del equipo remoto (de escritorio o portátil). Usa este modo de autenticación si tienes una instalación de equipo de prueba en un entorno en el que se inició sesión con una cuenta de prueba y no se pueden escribir credenciales. Asegúrate de que la configuración del depurador remoto esté establecida para no aceptar ninguna autenticación.

## Opciones de depuración

Windows 10 presenta una mejora del rendimiento de inicio de las aplicaciones para UWP gracias al inicio y la posterior suspensión de aplicaciones de forma proactiva mediante una técnica denominada [inicio previo](https://msdn.microsoft.com/library/windows/apps/Mt593297). Muchas aplicaciones no tendrán que hacer nada especial para funcionar en este modo, pero algunas aplicaciones pueden necesitar ajustar su comportamiento. Para facilitar la depuración de problemas en estas rutas de código puedes comenzar depurando la aplicación desde Visual Studio en el modo de inicio previo. La depuración se admite tanto desde un proyecto de Visual Studio (**Depurar** -&gt;**Otros destinos de depuración** -&gt;**Depurar el inicio previo de la Aplicación Windows universal**) como para las aplicaciones ya instaladas en el equipo (**Depurar** -&gt;**Otros destinos de depuración** -&gt;**Depurar paquete de la aplicación instalado** y activa la casilla **Activar aplicación con inicio previo**). Para obtener más información, lee la entrada de blog sobre el [inicio previo de depuración de UWP]( http://go.microsoft.com/fwlink/?LinkId=717245).

Puedes establecer las siguientes opciones de implementación en la página de propiedades de **Depurar** del proyecto de inicio.

**Permitir bucle invertido de red**

Por motivos de seguridad, no se permite que una aplicación para UWP instalada del modo estándar realice llamadas de red al dispositivo en el que está instalada. De manera predeterminada, la implementación de Visual Studio crea una exención de esta regla para la aplicación implementada. Esta exención te permite probar los procedimientos de comunicación en un solo equipo. Antes de enviar la aplicación a la Tienda Windows, debes probarla sin la exención.

Para quitar la exención de bucle invertido de red de la aplicación:

-   En la página de propiedades de **Depurar** de C# y Visual Basic, desactiva la casilla **Permitir bucle invertido de red**.
-   En la página de propiedades de **Depuración** de JavaScript y C++, establece el valor de **Permitir bucle invertido de red** en **No**.

**No iniciar, pero depurar mi código al empezar (C# y Visual Basic)/Iniciar aplicación (JavaScript y C++)**

Para configurar la implementación para iniciar automáticamente una sesión de depuración cuando se inicie la aplicación:

-   En la página de propiedades de **Depurar** de C# y Visual Basic, activa la casilla **No iniciar, pero depurar mi código al empezar**.
-   En la página de propiedades de **Depuración** de JavaScript y C++, establece el valor de **Iniciar aplicación** en **Sí**.

## Símbolos

Los archivos de símbolos contienen una variedad de datos muy útiles al depurar el código, como variables, nombres de función y direcciones de punto de entrada, lo que te permite comprender mejor las excepciones y el orden de ejecución de la pila de llamadas. Los símbolos de la mayoría de variantes de Windows están disponibles a través del [Servidor de símbolos de Microsoft](http://msdl.microsoft.com/download/symbols) o se pueden descargar para búsquedas más rápidas y sin conexión en [Descargar paquetes de símbolos de Windows](http://aka.ms/winsymbols).

Para establecer opciones de símbolos de Visual Studio, selecciona **Herramientas > Opciones** y, después, ve a **Depurar > Símbolos** en la ventana del cuadro de diálogo.

**Figura 4. Cuadro de diálogo Opciones.**
            
![Cuadro de diálogo Opciones](images/gs-debug-uwp-apps-004.png)

Para cargar símbolos en una sesión de depuración con [WinDbg](#windbg), establece la variable **sympath** en la ubicación del paquete de símbolos. Por ejemplo, al ejecutar el comando siguiente se cargarán los símbolos del servidor de símbolos de Microsoft y, después, se almacenarán en caché en el directorio C:\Symbols:

```
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

Puedes agregar más rutas de acceso mediante el delimitador ';' o usar el comando `.sympath+`. Para obtener información sobre operaciones de símbolos más avanzadas que usan WinDbg, consulta [Public and Private Symbols (Símbolos públicos y privados)](https://msdn.microsoft.com/library/windows/hardware/ff553493).

## WinDbg

WinDbg es un depurador eficaz que se suministra como parte del conjunto de herramientas de depuración para Windows, que se incluye con [Windows SDK](http://go.microsoft.com/fwlink/p?LinkID=271979). La instalación de Windows SDK te permite instalar las herramientas de depuración para Windows como un producto independiente. Aunque es muy útil para depurar código nativo, WinDbg no se recomienda para aplicaciones escritas en código administrado o HTML5. 

Para usar WinDbg con aplicaciones para UWP, tendrás que deshabilitar primero PLM para el paquete de la aplicación mediante PLMDebug, como se describe en la sección anterior. 

```
plmdebug /enableDebug [PackageFullName] "\"C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

A diferencia de Visual Studio, la mayoría de la funcionalidad principal de WinDbg se basa en proporcionar comandos a la ventana de comando. Los comandos proporcionados permiten ver el estado de ejecución, investigar los volcados de memoria de modo usuario y realizar la depuración de distintos modos. 

Uno de los comandos más populares de WinDBG es `!analyze -v`, que se usa para recuperar una cantidad de información detallada acerca de la excepción actual, como:

- FAULTING_IP: puntero de instrucción en el momento del error
- EXCEPTION_RECORD: dirección, código e indicadores de la excepción actual
- STACK_TEXT: seguimiento de la pila antes de la excepción

Para obtener una lista completa de todos los comandos de WinDbg, consulta [Debugger Commands (Comandos del depurador)](https://msdn.microsoft.com/library/ff540507).

## Temas relacionados
- [Herramientas de pruebas y depuración de Administración del ciclo de vida de los procesos (PLM)](testing-debugging-plm.md)
- [Depuración, pruebas y rendimiento](index.md)



<!--HONumber=Jun16_HO4-->



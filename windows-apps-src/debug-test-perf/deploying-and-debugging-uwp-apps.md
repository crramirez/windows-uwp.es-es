---
author: PatrickFarley
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Este artículo te guiará en el procedimiento para dirigirte a distintos destinos de implementación y de depuración."
title: "Implementación y depuración de aplicaciones para la Plataforma universal de Windows (UWP)"
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, test, rendimiento, performance, depuración, debug, pruebas"
ms.openlocfilehash: 2d4f49b0b9756162a22adf5c52910102d4a37281
ms.sourcegitcommit: e8cc657d85566768a6efb7cd972ebf64c25e0628
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2017
---
# <a name="deploying-and-debugging-uwp-apps"></a>Implementación y depuración de aplicaciones para UWP

\[ Actualizado para aplicaciones para UWP en Windows10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artículo te guiará en el procedimiento para dirigirte a distintos destinos de implementación y de depuración.

Microsoft Visual Studio permite implementar y depurar aplicaciones para la Plataforma universal de Windows (UWP) en una variedad de dispositivos de Windows10. Visual Studio puede controlar el proceso de creación y registro de la aplicación en el dispositivo de destino.

## <a name="picking-a-deployment-target"></a>Elección de un destino de implementación

Para elegir un destino, ve a la lista desplegable de destino de depuración junto al botón **Iniciar depuración** y elige el destino en el que quieres implementar la aplicación. Después de seleccionar el destino, elige **Iniciar depuración (F5)** para realizar la implementación y la depuración en ese destino o selecciona **CTRL+F5** para realizar solo la implementación en ese destino.

![Lista de destinos de dispositivo para depuración](images/debug-device-target-list.png)

-   **Simulador** va a implementar la aplicación en un entorno simulado en la máquina de desarrollo actual. Esta opción solo está disponible si la **Versión mínima de la plataforma de destino** de la aplicación es inferior o igual al sistema operativo de la máquina de desarrollo.
-   **Máquina local** implementa la aplicación en la máquina de desarrollo actual. Esta opción solo está disponible si la **Versión mínima de la plataforma de destino** de la aplicación es inferior o igual al sistema operativo de la máquina de desarrollo.
-   **Máquina remota** te permitirá especificar un destino remoto para implementar la aplicación. Puedes encontrar más información acerca de la implementación en una máquina remota en [Especificación de un dispositivo remoto](#specifying-a-remote-device).
-   **Dispositivo** implementará la aplicación en un dispositivo conectado mediante USB. El dispositivo no debe estar bloqueado por el desarrollador y debe tener la pantalla desbloqueada.
-   Un destino de tipo **Emulador** arrancará e implementará la aplicación en un emulador con la configuración especificada en el nombre. Los emuladores solo están disponibles en máquinas compatibles con Hyper-V que ejecutan Windows8.1 o posterior.


## <a name="debugging-deployed-apps"></a>Depuración de aplicaciones implementadas
Visual Studio también se puede asociar a cualquier proceso de aplicación para UWP en ejecución seleccionando **Depurar** y, después, **Asociar al proceso**. Para la asociación a un proceso en ejecución no se requiere el proyecto de Visual Studio original, pero la carga de los [símbolos](#symbols) del proceso será de gran ayuda al depurar un proceso del que no tienes el código original.  

Además, cualquier paquete de la aplicación instalado se puede asociar y depurar seleccionando **Depurar**, **Otros** y, después, **Depurar paquete de aplicaciones instalado**.   

![Cuadro de diálogo Depurar paquete de aplicaciones instalado](images/gs-debug-uwp-apps-002.png)   

Al seleccionar **No iniciar, pero depurar mi código al empezar**, el depurador de Visual Studio se asociará a la aplicación para UWP cuando se inicie a la hora personalizada. Esta es una forma eficaz de depurar las rutas de acceso de control a partir de [distintos métodos de inicio](../xbox-apps/automate-launching-uwp-apps.md), como la activación de protocolos con parámetros personalizados.  

Las aplicaciones para UWP se pueden desarrollar y compilar en Windows 8.1 o posterior, pero requieren Windows 10 para ejecutarse. Si estás desarrollando una aplicación para UWP en un equipo con Windows 8.1, puedes depurar de forma remota una aplicación para UWP que se ejecute en otro dispositivo de Windows 10, siempre que el equipo host y el equipo de destino se encuentren en la misma LAN. Para hacerlo, descarga e instala [Herramientas remotas para Visual Studio](https://www.visualstudio.com/downloads/) en ambos equipos. La versión instalada debe coincidir con la versión existente de Visual Studio que hayas instalado y la arquitectura que selecciones (x86, x64) también debe coincidir con la de aplicación de destino.   

## <a name="package-layout"></a>Diseño del paquete
Con Visual Studio 2015 Update 3, hemos agregado la opción para que los desarrolladores especifiquen la ruta de acceso de diseño de sus aplicaciones para UWP. Esto determina en qué lugar del disco se copia diseño del paquete cuando se compila la aplicación. De manera predeterminada, esta propiedad se establece en relación con el directorio del proyecto raíz. Si no modificas esta propiedad, el comportamiento permanecerá igual al que tiene para las versiones anteriores de Visual Studio.

Esta propiedad se puede modificar en las propiedades de **Depurar** del proyecto.

Si quieres incluir todos los archivos de diseño en el paquete cuando crees un paquete para la aplicación, debes agregar la propiedad de proyecto `<IncludeLayoutFilesInPackage>true</IncludeLayoutFilesInPackage>`.

Para agregar esta propiedad, sigue estos pasos:

1. Haz clic con el botón derecho en el proyecto y luego selecciona **Descargar el proyecto**.
2. Haz clic con el botón derecho en el proyecto y selecciona **Editar [nombreDeProyecto].xxproj** (.xxproj cambiará según el idioma del proyecto).
3. Agrega la propiedad y luego vuelve a cargar el proyecto.

## <a name="specifying-a-remote-device"></a>Especificación de un dispositivo remoto

### <a name="c-and-microsoft-visual-basic"></a>C# y Microsoft Visual Basic

Para especificar una máquina remota para aplicaciones de C# o Microsoft Visual Basic, selecciona **Máquina remota** en la lista desplegable de destinos de depuración. Se mostrará el cuadro de diálogo **Conexiones remotas** y allí podrás especificar una dirección IP o seleccionar un dispositivo detectado. De manera predeterminada, está seleccionado el modo de autenticación **Universal**. Para determinar el modo de autenticación que se usará, consulta [Modos de autenticación](#authentication-modes).

![Cuadro de diálogo Conexiones remotas](images/debug-remote-connections.png)

Para volver a este cuadro de diálogo, puedes abrir las propiedades del proyecto e ir a la pestaña **Depurar**. Allí, selecciona **Buscar** junto a **Máquina remota:**

![Pestaña Depurar](images/debug-remote-machine-config.png)

Para implementar una aplicación en un equipo remoto de Creators Update, también debes descargar e instalar Herramientas remotas para Visual Studio en el equipo de destino. Para obtener instrucciones completas, consulta [Instrucciones del equipo remoto](#remote-pc-instructions).  Sin embargo, a partir del PC de Creators Update también es compatible con la implementación remota.  

### <a name="c-and-javascript"></a>C++ y JavaScript

Para especificar el destino de una máquina remota para una aplicación para UWP en C++ o JavaScript:

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nombre del proyecto y elige **Propiedades**.
2. Ve a la configuración de **Depuración** y cambia el valor de **Depurador para iniciar** a **Máquina remota**.
3. Escribe el **Nombre de máquina** (o haz clic en **Buscar** para encontrar uno) y establece la propiedad **Tipo de autenticación**.

![Páginas de propiedades de depuración](images/debug-property-pages.png)

Después de especificar la máquina, puedes seleccionar **Máquina remota** en la lista desplegable de destinos de depuración para volver a esa máquina especificada. Solo se puede seleccionar una máquina remota a la vez.

### <a name="remote-pc-instructions"></a>Instrucciones del equipo remoto

> [!NOTE]
> Estas instrucciones solo son necesarias para versiones anteriores a Windows 10.  A partir de la actualización de los creadores, un equipo puede tratarse como una consola Xbox.  Es decir, al habilitar la Detección de dispositivos en del equipo con Modo de desarrollador y mediante la Autenticación universal para emparejarlo mediante PIN y conectar con el equipo. 

Para implementar en un equipo remoto con Creators Update preinstalado, el equipo de destino debe tener instalado Visual Studio Remote Tools. La máquina remota también deben ejecutar una versión de Windows que sea superior o igual a la propiedad **Versión mínima de la plataforma de destino** de tus aplicaciones. Una vez que hayas instalado las herramientas remotas, debes iniciar el depurador remoto en el equipo de destino.

Para ello, busca **Depurador remoto** en el menú **Inicio**, ábrelo y, si te lo pide, permite que el depurador configure las opciones del firewall. De manera predeterminada, el depurador se inicia con la autenticación de Windows. Esto requiere credenciales de usuario si el usuario que inició sesión no es el mismo en ambos equipos.

Para cambiar a **Sin autenticación**, en el **Depurador remoto**, ve a **Herramientas** -&gt; **Opciones** y establécelo en **Sin autenticación**. Tras configurar el depurador remoto, también debes asegurarte de que has configurado el dispositivo host en [Modo de desarrollador](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development). Después de esto, puedes implementar desde la máquina de desarrollo.

Para obtener más información, consulta la página [Centro de descarga para Visual Studio](https://www.visualstudio.com/downloads/).

## <a name="passing-command-line-debug-arguments"></a>Pasar argumentos de depuración de la línea de comandos 
En Visual Studio 2017, puedes pasar argumentos de depuración de la línea de comandos al iniciar la depuración de las aplicaciones para UWP. Puedes acceder a los argumentos de depuración de la línea de comandos desde el parámetro *args* del método **OnLaunched** de la clase [**Application**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.application). Para especificar los argumentos de depuración de la línea de comandos, abre las propiedades del proyecto y navega a la pestaña **Depurar**. 

> [!NOTE]
> Esta característica está disponible en Visual Studio 2017 (versión 15.1) para C#, VB y C++. JavaScript está disponible en versiones posteriores de Visual Studio 2017. Los argumentos de depuración de la línea de comandos están disponibles para todos los tipos de implementación, excepto el simulador.

En el caso de proyectos C# y VB para la UWP, verás un campo **Argumentos de la línea de comandos:** en **Opciones de inicio**. 

![Argumentos de la línea de comandos](images/command-line-arguments.png)

En el caso de proyectos C++ y JS para UWP, verás **Argumentos de la línea de comandos** como un campo en **Propiedades de depuración**.

![Argumentos de la línea de comandos C++ y JS](images/command-line-arguments-cpp.png)

Después de especificar los argumentos de la línea de comandos, puedes acceder al valor del argumento en el método **OnLaunched** de la aplicación. El elemento *args* del objeto [**LaunchActivatedEventArgs**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) tendrá una **Argumentos** con el valor establecido en el texto del campo **Argumentos de la línea de comandos** campo. 

![Argumentos de la línea de comandos C++ y JS](images/command-line-arguments-debugging.png)

## <a name="authentication-modes"></a>Modos de autenticación

Existen tres modos de autenticación para la implementación del equipo remoto:

- **Universal (protocolo sin cifrar)**: usa este modo de autenticación siempre que realices una implementación en un dispositivo remoto. Actualmente, esto es para dispositivos de IoT, dispositivos de Xbox y dispositivos HoloLens, así como para dispositivos con Creators Update o equipos más nuevos. Universal (protocolo sin cifrar) solo se debe usar en redes de confianza. La conexión de depuración es vulnerable para los usuarios malintencionados que podrían interceptar y cambiar los datos que se pasan entre el desarrollo y el equipo remoto.
- **Windows**: este modo de autenticación solo está destinado para su uso en la implementación del equipo remoto (de escritorio o portátil) que ejecuten Herramientas remotas para Visual Studio. Usa este modo de autenticación si tienes acceso a las credenciales del usuario que inició la sesión de la máquina de destino. Este es el canal más seguro para la implementación remota.
- **Ninguno**: este modo de autenticación solo está destinado para su uso en la implementación del equipo remoto (de escritorio o portátil) que ejecuten Herramientas remotas para Visual Studio. Usa este modo de autenticación si tienes una instalación de máquina de prueba en un entorno en el que se inició sesión con una cuenta de prueba y no se pueden escribir credenciales. Asegúrate de que la configuración del depurador remoto esté establecida para aceptar que no haya autenticación.

## <a name="advanced-remote-deployment-options"></a>Opciones avanzadas de implementación remota
Con el lanzamiento de Visual Studio 2015 Update 3 y la Actualización de aniversario de Windows 10, hay nuevas opciones avanzadas de implementación remota para determinados dispositivos Windows10. Las opciones avanzadas de implementación remota pueden encontrarse en el menú **Depurar** de las propiedades del proyecto.

Las nuevas propiedades incluyen lo siguiente:
* Tipo de implementación
* Ruta de acceso de registro del paquete
* Mantener todos los archivos en el dispositivo, incluso los que ya no forman parte del diseño

### <a name="requirements"></a>Requisitos
Para usar las opciones avanzadas de implementación remota, debes cumplir los siguientes requisitos:
* Tener instalado Visual Studio 2015 Update 3 con Windows 10 Tools 1.4.1 (que incluye el SDK de Actualización de aniversario de Windows 10)
* Elegir como destino un dispositivo remoto de Xbox con la Actualización de aniversario de Windows 10 o un equipo con Windows 10 Creators Update. 
* Usar el modo Autenticación universal

### <a name="properties-pages"></a>Páginas de propiedades
En el caso de una aplicación para UWP de C# o Visual Basic, la página de propiedades tendrá un aspecto similar al siguiente.

![Propiedades de CS o VB](images/advanced-remote-deploy-cs.png)

En el caso de una aplicación para UWP de C++, la página de propiedades tendrá un aspecto similar al siguiente.

![Propiedades de Cpp](images/advanced-remote-deploy-cpp.png)

### <a name="copy-files-to-device"></a>Copiar archivos en el dispositivo
La opción **Copiar archivos en el dispositivo** transferirá físicamente los archivos a través de la red al dispositivo remoto. Copiará y registrará el diseño de paquete que está integrado en la **Ruta de la carpeta de diseño**. Visual Studio mantendrá sincronizados los archivos que se copian en el dispositivo con los archivos del proyecto de Visual Studio. Sin embargo, hay una opción para **mantener todos los archivos en el dispositivo, incluso los que ya no forman parte del diseño**. Si seleccionas esta opción significa que todos los archivos que se copiaron anteriormente en el dispositivo remoto, pero que ya no forman parte de tu proyecto, permanecerán en el dispositivo remoto.

La **ruta de acceso de registro del paquete** especificada cuando **copias archivos en el dispositivo** es la ubicación física del dispositivo remoto en la que se copian los archivos. Esta ruta de acceso se puede especificar como cualquier ruta de acceso relativa. La ubicación en la que se implementen los archivos será relativa a una raíz de archivos de desarrollo que variará según el dispositivo de destino. Especificar esta ruta de acceso resulta útil en el caso de varios desarrolladores que comparten el mismo dispositivo y trabajan en paquetes con alguna variación en cuanto a compilación.

> [!NOTE]
> **Copiar archivos en el dispositivo** actualmente se admite en Xbox con Actualización de aniversario de Windows 10 y con equipos con Windows 10 Creators Update.

En el dispositivo remoto, el diseño se copiará en la siguiente ubicación predeterminada según la familia de dispositivos:
  `\\MY-DEVKIT\DevelopmentFiles\PACKAGE-REGISTRATION-PATH`

### <a name="register-layout-from-network"></a>Registrar el diseño desde la red
Si eliges registrar el diseño desde la red, puedes compilar el diseño de tu paquete en un recurso compartido de red y luego registrar el diseño en el dispositivo remoto directamente desde la red. Esto requiere que especifiques una ruta de acceso de la carpeta del diseño (un recurso compartido de red) accesible desde el dispositivo remoto. La propiedad **Ruta de acceso de la carpeta del diseño** es la ruta de acceso que se establece en relación con el equipo que ejecuta Visual Studio, mientras que la propiedad **Ruta de acceso de registro del paquete** es la misma ruta de acceso, pero especificada en relación con el dispositivo remoto.

Para registrar correctamente el diseño desde la red, primero debes hacer que **Ruta de acceso de la carpeta del diseño** sea una carpeta de red compartida. Para ello, haz clic con el botón derecho en la carpeta del Explorador de archivos, selecciona **Compartir con > Usuarios específicos**y luego elige los usuarios con los que te gustaría compartir la carpeta. Cuando intentes registrar el diseño desde la red, se te pedirán las credenciales para asegurarte de que te registres como un usuario con acceso al recurso compartido.

Para obtener ayuda con este tema, consulta los siguientes ejemplos:

- Ejemplo 1 (carpeta local de diseño, accesible como un recurso compartido de red):
  * **Ruta de acceso de la carpeta del diseño** = `D:\Layouts\App1`
  * **Ruta de acceso de registro del paquete** = `\\NETWORK-SHARE\Layouts\App1`

- Ejemplo 2 (carpeta de diseño de la red):
  * **Ruta de acceso de la carpeta del diseño** = `\\NETWORK-SHARE\Layouts\App1`
  * **Ruta de acceso de registro del paquete** = `\\NETWORK-SHARE\Layouts\App1`

Cuando registres el diseño desde la red por primera vez, las credenciales se almacenarán en caché en el dispositivo de destino para que no sea necesario iniciar sesión una y otra vez. Para quitar las credenciales almacenadas en caché, puedes usar la [herramienta WinAppDeployCmd.exe](https://msdn.microsoft.com/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool) desde el SDK de Windows 10 con el comando **deletecreds**.

No puedes seleccionar **Mantener todos los archivos en el dispositivo** al registrar el diseño desde la red porque no hay archivos que se copien físicamente al dispositivo remoto.

> [!NOTE]
> **Registrar el diseño desde la red** actualmente se admite en Xbox con Actualización de aniversario de Windows 10 y con equipos con Windows 10 Creators Update.

En el dispositivo remoto, el diseño se registrará en la siguiente ubicación predeterminada según la familia de dispositivos:   `Xbox: \\MY-DEVKIT\DevelopmentFiles\XrfsFiles`, este es un vínculo simbólico de la **ruta de acceso de registro del paquete**
  El equipo no usa un vínculo simbólico y en su lugar, registra directamente la **ruta de acceso de registro del paquete**


## <a name="debugging-options"></a>Opciones de depuración

Windows10 presenta una mejora del rendimiento de inicio de las aplicaciones para UWP gracias al inicio y la posterior suspensión de aplicaciones de forma proactiva mediante una técnica denominada [inicio previo](https://msdn.microsoft.com/library/windows/apps/Mt593297). Muchas aplicaciones no tendrán que hacer nada especial para funcionar en este modo, pero es posible que algunas necesiten ajustar su comportamiento. Para facilitar la depuración de problemas en estas rutas de acceso de código, puedes comenzar depurando la aplicación desde Visual Studio en el modo de inicio previo.

La depuración se admite tanto desde un proyecto de Visual Studio (**Depurar** -&gt; **Otros destinos de depuración** -&gt; **Depurar el inicio previo de la Aplicación Windows universal**) como para las aplicaciones ya instaladas en la máquina (**Depurar** -&gt; **Otros destinos de depuración** -&gt; **Depurar paquete de aplicaciones instalado** y activa la casilla **Activar aplicación con inicio previo**). Para obtener más información, consulta [Debug UWP Prelaunch (Depurar inicio previo de UWP)](http://go.microsoft.com/fwlink/p/?LinkId=717245).

Puedes establecer las siguientes opciones de implementación en la página de propiedades de **Depurar** del proyecto de inicio:

- **Permitir bucle invertido de la red local**

  Por cuestiones de seguridad, no se permite que una aplicación para UWP instalada de modo estándar realice llamadas de red al dispositivo en el que está instalada. De manera predeterminada, la implementación de Visual Studio crea una exención de esta regla para la aplicación implementada. Esta exención te permite probar los procedimientos de comunicación en un solo equipo. Antes de enviar la aplicación a la Tienda Windows, debes probarla sin la exención.

  Para quitar la exención de bucle invertido de red de la aplicación:

  -   En la página de propiedades de **Depurar** de C# y Visual Basic, desactiva la casilla **Permitir bucle invertido de la red local**.
  -   En la página de propiedades de **Depuración** de JavaScript y C++, establece el valor de **Permitir bucle invertido de la red local** en **No**.

- **No iniciar, pero depurar mi código al empezar / Iniciar aplicación**

  Para configurar la implementación para iniciar automáticamente una sesión de depuración cuando se inicie la aplicación:

  -   En la página de propiedades de **Depurar** de C# y Visual Basic, activa la casilla **No iniciar, pero depurar mi código al empezar**.
  -   En la página de propiedades de **Depuración** de JavaScript y C++, establece el valor de **Iniciar aplicación** en **Sí**.

## <a name="symbols"></a>Símbolos

Los archivos de símbolos contienen una variedad de datos muy útiles al depurar el código, como variables, nombres de función y direcciones de punto de entrada, lo que te permite comprender mejor las excepciones y el orden de ejecución de la pila de llamadas. Los símbolos de la mayoría de variantes de Windows están disponibles a través del [Servidor de símbolos de Microsoft](http://msdl.microsoft.com/download/symbols) o se pueden descargar para búsquedas más rápidas y sin conexión en [Descargar paquetes de símbolos de Windows](http://aka.ms/winsymbols).

Para establecer opciones de símbolo de Visual Studio, selecciona **Herramientas > Opciones** y después ve a **Depuración > Símbolos** en la ventana del cuadro de diálogo.

![Cuadro de diálogo Opciones](images/gs-debug-uwp-apps-004.png)

Para cargar símbolos en una sesión de depuración con [WinDbg](#windbg), establece la variable **sympath** en la ubicación del paquete de símbolos. Por ejemplo, al ejecutar el comando siguiente, se cargarán los símbolos del servidor de símbolos de Microsoft y después se almacenarán en caché en el directorio C:\Symbols:

```
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

Puedes agregar más rutas de acceso mediante el delimitador `‘;’` o puedes usar el comando `.sympath+`. Para obtener información sobre operaciones de símbolos más avanzadas que usan WinDbg, consulta [Public and Private Symbols (Símbolos públicos y privados)](https://msdn.microsoft.com/library/windows/hardware/ff553493).

## <a name="windbg"></a>WinDbg

WinDbg es un depurador eficaz que se suministra como parte del conjunto de herramientas de depuración para Windows, que se incluye con [Windows SDK](http://go.microsoft.com/fwlink/p/?LinkID=271979). La instalación de Windows SDK te permite instalar las herramientas de depuración para Windows como un producto independiente. Aunque es muy útil para depurar código nativo, WinDbg no se recomienda para aplicaciones escritas en código administrado o HTML5.

Para usar WinDbg con aplicaciones para UWP, tendrás que deshabilitar primero la administración del ciclo de vida de los procesos (PLM) para el paquete de la aplicación mediante PLMDebug, tal como se describe en [Herramientas de pruebas y depuración de Administración del ciclo de vida de los procesos (PLM)](testing-debugging-plm.md).

```
plmdebug /enableDebug [PackageFullName] "\"C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

A diferencia de Visual Studio, la mayoría de la funcionalidad principal de WinDbg se basa en proporcionar comandos a la ventana Comandos. Los comandos proporcionados permiten ver el estado de ejecución, investigar los volcados de memoria de modo usuario y realizar la depuración de distintos modos.

Uno de los comandos más populares de WinDBG es `!analyze -v`, que se usa para recuperar una cantidad de información detallada acerca de la excepción actual, como:

- FAULTING_IP: puntero de instrucción en el momento del error
- EXCEPTION_RECORD: dirección, código e indicadores de la excepción actual
- STACK_TEXT: seguimiento de la pila antes de la excepción

Para obtener una lista completa de todos los comandos de WinDbg, consulta [Debugger Commands (Comandos del depurador)](https://msdn.microsoft.com/library/ff540507).

## <a name="related-topics"></a>Temas relacionados
- [Herramientas de pruebas y depuración de Administración del ciclo de vida de los procesos (PLM)](testing-debugging-plm.md)
- [Depuración, pruebas y rendimiento](index.md)

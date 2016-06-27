---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Este artículo te guiará en el procedimiento para dirigirte a distintos destinos de implementación y de depuración."
title: "Implementación y depuración de aplicaciones para la Plataforma universal de Windows (UWP)"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: eb639e78bf144572dfbfd2d65514bb4eff7c7be1

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





<!--HONumber=Jun16_HO3-->



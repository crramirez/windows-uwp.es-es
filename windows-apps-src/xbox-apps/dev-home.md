---
author: v-angraf
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Inicio del desarrollador en la consola (Dev Home)
description: Proporciona información sobre la aplicación Dev Home para Xbox One.
ms.author: v-angraf@microsoft.com
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 232770ab4b746663a105982605d1cedcb92adbe3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5981395"
---
# <a name="developer-home-on-the-console-dev-home"></a>Inicio del desarrollador en la consola (Dev Home)
   
  
Dev Home es una experiencia de herramientas en el kit de desarrollo de Xbox One diseñada para ayudar a la productividad del desarrollador. Ofrece funcionalidad para administrar y configurar el kit de desarrollo, administrar usuarios, iniciar títulos instalados y realizar la captura y realiza un seguimiento. En futuras versiones que seguiremos para ampliar la funcionalidad para habilitar características adicionales en función de tus comentarios y también para habilitar la extensibilidad y la adición de sus propias herramientas.   
   
  
Estamos muy interesados en tus comentarios sobre Dev Home y los escenarios que te interesa más lo vean admitir. Proporcione sus comentarios a través de los métodos descritos en **Enviar comentarios** en el menú principal de la aplicación o a través de su administrador de cuenta de desarrollador (DAM).   
   
  
Para iniciar la página principal para desarrolladores en el de noviembre de 2015 o posterior recuperación:  
 
   1. Abre a la guía moviendo izquierda en Home o hacer doble clic en el botón de nexo  
   1. Desplazar hacia abajo para la **configuración** (el icono del engranaje)   
   1. Seleccionar **todas las configuraciones**  
   1. En la página de **desarrollador** de forma predeterminada, selecciona **Inicio del desarrollador** (el icono de inicio)   

 ![](images/dev_home_icons.png)   
  
En versiones anteriores recuperaciones selecciona la ventana de Dev Home en el lado derecho de la pantalla principal de **contenido destacado** o ver la lista de aplicaciones en el Administrador de Xbox One e inicie **Dev Home**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interfaz de usuario  
   
  
El encabezado de la interfaz de usuario de Dev Home contiene las siguientes importantes "un vistazo" información acerca de la consola de desarrollo:   
 
   *  **IP de consola:** La dirección IP actual de la consola.   
   *  **Nombre de la consola:** El nombre de host actual de la consola.  
   *  **Espacio aislado:** El nombre del espacio aislado que se encuentra en la consola.  
   *  **Versión del sistema operativo:** La versión actual de recuperación que se ejecuta en la consola.
   *  Hora actual del sistema.   

   
  
El resto de la interfaz de Dev Home se divide en las páginas siguientes. Para obtener más información acerca de las herramientas de estas páginas, consulta los temas individuales.   
 
   *  [Casa](devhome-home.md)  
   *  [XboxLive](devhome-live.md)  
   *  [Configuración](devhome-settings.md)  
   *  [Captura multimedia](devhome-capture.md)  
   *  [Redes](devhome-networking.md)  
   *  [Rendimiento](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menú principal  
   
  
Al presionar el botón de **menú** en el controlador, puedes acceder al menú principal que permite la configuración de área de trabajo de aplicación, la capacidad para administrar las credenciales para acceder a ubicaciones de red y obtener información acerca de proporcionar comentarios sobre la aplicación.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Experiencia del usuario de modo acoplado  
   
  
Varias herramientas existentes y futuras en Dev Home, por ejemplo, redes y de varios jugadores, están diseñadas para usarse acoplarse a un lado mientras ejecutas el título, por lo que puede tener acceso fácil a las herramientas durante la prueba.   
   
  
Para acceder a modo de complemento, resaltar el título de la herramienta adecuada, presiona el botón de **vista** en el controlador y selecciona **de acoplamiento** en el menú contextual:  
 ![](images/dev_home_4.png)   
  
Dev Home se acoplará a la derecha. Puede cambiar el contexto si presionas dos veces el botón Nexo de la forma habitual.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizar Dev Home  
   
  
Dev Home se diseñó para ser personalizable y cercana. Puedes configurar la aplicación para adaptar el flujo de trabajo y, a continuación, guarda como un área de trabajo. Esta área de trabajo se puede exportar e importado, lo que permite copiar el diseño a otras consolas como sea necesario. Estas opciones se encuentran en el menú principal en el **área de trabajo**. El archivo exportado se encontrarán en la unidad temporal del sistema en el `Dev Home\Workspaces` directorio.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Cambiar el tamaño y orden de las herramientas  
   
  
Para cambiar el tamaño o la posición de una herramienta, usa el botón de menú contextual (botón de vista en el controlador) mientras el título tiene el foco. En el menú contextual selecciona **mover** o **cambiar el tamaño**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Cambiar el color del tema y la imagen de fondo  
   
  
En el menú principal, puedes seleccionar **área de trabajo** y, a continuación, **cambiar el color de tema**. Selecciona un color nuevo y selecciona **Guardar** para actualizar el color del tema usado para resaltar el foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configuración de la aplicación predeterminada para un paquete  
   
  
Si un paquete contiene varias aplicaciones, Dev Home te permitirá establecer la aplicación predeterminada se inicie. Resaltar el paquete en el selector y presiona **el botón para abrir la lista de aplicaciones disponibles** . Resaltar aquel en el que deseas establecer como predeterminado y presionar el botón de **vista** y, a continuación, elige **establecer como predeterminado** en el menú contextual.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Usar Dev Home para registrar e iniciar títulos desde un recurso compartido de red  
   
  
Desde el selector, en la parte inferior de las aplicaciones instaladas y la lista de juegos, puedes seleccionar la opción de **registrar un juego desde un recurso compartido de red** para ejecutar una versión de archivos sueltos de un título de forma remota.   
 ![](images/dev_home_8.png)   
  
A continuación, puedes escribir la ruta de acceso de red en el archivo appxmanifest.xml del título que quieres registrar. Dev Home intentará registrar el título con todas las credenciales existentes para ese recurso compartido y si necesita pedirá las credenciales de red de nuevo. Si es necesario acceder a recursos compartidos adicionales (por ejemplo a los recursos de acceso vinculado medio de símbolos en un servidor independiente), a continuación, tendrás que agregar aquellos a través de la opción a continuación.   
   
  
Puedes administrar estas credenciales almacenadas (y agregar otras adicionales) en la consola mediante la opción de **administrar las credenciales de red** del menú principal.   
 ![](images/dev_home_9.png)   
  
Puede ver las credenciales actualmente en la consola, editar credenciales seleccionando la ruta de acceso de la credencial y hacer clic en **un** botón y quitar una credencial seleccionando el vínculo quitar y hacer clic en **un** botón.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>En esta sección  
  
[Página principal (Dev Home)](devhome-home.md)  


&nbsp;&nbsp;Proporciona acceso rápido a las tareas que se realizan de forma rutinaria en una consola de desarrollo. 
  
  
[Xbox Live página (Dev Home)](devhome-live.md)  


&nbsp;&nbsp;Captura información multijugador y muestra el estado actual del servicio Xbox Live. 
  
  
[Página de configuración (Dev Home)](devhome-settings.md)  


&nbsp;&nbsp;Proporciona acceso a diversas opciones de configuración para la consola de desarrollo. 
  
  
[Página (Dev Home) de captura multimedia](devhome-capture.md)  


&nbsp;&nbsp;La página **de captura multimedia** de Dev Home captura de vídeo del título que se está ejecutando actualmente en la consola. 
  
  
[Página de red (Dev Home)](devhome-networking.md)  


&nbsp;&nbsp;Simula distintas condiciones de redes para solucionar problemas. 
  
  
[Página rendimiento (Dev Home)](devhome-performance.md)  


&nbsp;&nbsp;Simula varias actividad del disco y condiciones de uso de CPU para solucionar problemas. 
 
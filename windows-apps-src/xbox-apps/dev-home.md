---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Inicio del desarrollador en la consola (Dev Home)
description: Proporciona información sobre la aplicación Inicio del desarrollador para Xbox One.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620850"
---
# <a name="developer-home-on-the-console-dev-home"></a>Inicio del desarrollador en la consola (Dev Home)
   
  
Dev Home es una experiencia de herramientas del kit de desarrollo de Xbox One diseñada para ayudar en la productividad de los desarrolladores. Dev Home ofrece funcionalidad para administrar y configurar el kit de desarrollo, administrar usuarios, iniciar títulos instalados y realizar capturas y seguimientos. En futuras versiones continuaremos ampliando la funcionalidad para habilitar características adicionales en función de tus comentarios y también para habilitar la extensibilidad y la incorporación de tus propias herramientas.   
   
  
Estamos muy interesados en tus comentarios sobre Dev Home y en los escenarios que más te interesa que admita. Envía tus comentarios a través de los métodos descritos en **Enviar comentarios** en el menú principal de la aplicación o a través de tu administrador de cuenta de desarrollador (DAM).   
   
  
Para iniciar Dev Home en la recuperación de noviembre de 2015 o posterior:  
 
   1. Abre la guía yendo a la izquierda en Inicio o haciendo doble clic en el botón Nexo.  
   1. Desplázate hacia abajo hasta **Configuración** (el icono del engranaje).   
   1. Selecciona **Toda la configuración**.  
   1. En la página predeterminada **Desarrollador**, selecciona **Inicio del desarrollador** (el icono de la casa)   

 ![](images/dev_home_icons.png)   
  
En recuperaciones anteriores, selecciona el icono de Dev Home a la derecha de la pantalla de inicio en **contenido destacado** o ve la lista de aplicaciones en el Administrador de Xbox One e inicia **Dev Home**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interfaz de usuario  
   
  
El encabezado de la interfaz de usuario de Dev Home contiene la siguiente información importante resumida sobre la consola de desarrollo:   
 
   *  **IP de la consola:** La dirección IP actual de la consola.   
   *  **Nombre de la consola:** El nombre de host actual de la consola.  
   *  **Espacio aislado:** El nombre del espacio aislado que se encuentra en la consola.  
   *  **Versión del sistema operativo:** La versión actual de recuperación que se ejecuta en la consola.
   *  Hora actual del sistema.   

   
  
El resto de la interfaz de usuario de Dev Home se divide en las siguientes páginas. Para obtener más información sobre las herramientas en estas páginas, consulta los temas individuales.   
 
   *  [Página principal](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Configuración](devhome-settings.md)  
   *  [Captura de medios](devhome-capture.md)  
   *  [Funciones de red](devhome-networking.md)  
   *  [Rendimiento](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menú principal  
   
  
Al presionar el botón **menú** en el mando, puedes acceder al menú principal que permite la configuración del área de trabajo de la aplicación, la capacidad de administrar credenciales para acceder a ubicaciones de red e información sobre cómo proporcionar comentarios en la aplicación.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Experiencia de usuario en el modo acoplado  
   
  
Varias herramientas existentes y futuras en Dev Home, por ejemplo, Redes y Multijugador, están diseñadas para usarse acoplarse a un lado mientras ejecutas el título para que puedas tener un acceso fácil a las herramientas durante la prueba.   
   
  
Para acceder al modo acoplado, selecciona el título de la herramienta adecuada, presiona el botón **View** en el mando y selecciona **snap** en el menú contextual:  
 ![](images/dev_home_4.png)   
  
Dev Home se acoplará a la derecha. Puede cambiar el contexto si presionas dos veces el botón Nexo de la forma habitual.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizar Dev Home  
   
  
Dev Home se diseñó para ser personalizable y cercana. Puedes configurar la aplicación según el flujo de trabajo y guardarlo como un área de trabajo. Esta área de trabajo puede exportarse e importarse, lo que permite copiar el diseño en otras consolas, según sea necesario. Estas opciones se encuentran en el menú principal, en **área de trabajo**. El archivo exportado se ubicará en la unidad temporal del sistema en el directorio `Dev Home\Workspaces`.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Cambio de tamaño y orden de las herramientas  
   
  
Para cambiar el tamaño o la posición de una herramienta, usa el botón de menú contextual (botón View del mando) mientras el foco se encuentre en el título. En el menú contextual, selecciona **Mover** o **Cambiar tamaño**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Cambiar el color del tema y la imagen de fondo  
   
  
En el menú principal, puedes seleccionar **Área de trabajo** y luego **Change theme color**. Selecciona un nuevo color y selecciona **Guardar** para actualizar el color del tema usado para resaltar el foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configurar la aplicación predeterminada para un paquete  
   
  
Si un paquete contiene varias aplicaciones, Dev Home te permitirá establecer la aplicación predeterminada que se iniciará. Resalta el paquete en el iniciador y presiona el botón **A** para abrir la lista de aplicaciones disponibles. Resalta la que quieras establecer como predeterminada y presiona el botón **Ver**, luego elige **Establecer como predeterminada** en el menú contextual.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Usar Dev Home para registrar e iniciar títulos desde un recurso compartido de red  
   
  
Desde el iniciador, en la parte inferior de la lista de juegos y aplicaciones instalados, puedes seleccionar la opción **Register a game from a network share** para ejecutar una versión de archivos sueltos de un título de forma remota.   
 ![](images/dev_home_8.png)   
  
A continuación, puedes escribir la ruta de acceso de red en el archivo appxmanifest.xml para el título que quieres registrar. Dev Home intentará registrar el título con cualquiera de las credenciales existentes para ese recurso compartido y, si es necesario, pedirá nuevas credenciales de red. Si necesitas acceso a recursos compartidos adicionales (por ejemplo, para acceder a recursos vinculados de forma simbólica en un servidor independiente), deberás agregarlas a través de la opción siguiente.   
   
  
Puedes administrar estas credenciales almacenadas (y agregar nuevas) en la consola a través de la opción **Manage network credentials** del menú principal.   
 ![](images/dev_home_9.png)   
  
Puedes ver las credenciales que hay actualmente en la consola, editar credenciales seleccionando la ruta de acceso de la credencial y haciendo clic en el botón **A** y quitar una credencial seleccionando el vínculo para quitar y haciendo clic en el botón **A**.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>En esta sección  
  
[Página de inicio (página principal de desarrollo)](devhome-home.md)  


&nbsp;&nbsp;Proporciona acceso rápido a las tareas que se realizan habitualmente en una consola de desarrollo. 
  
  
[Xbox Live página (página principal de desarrollo)](devhome-live.md)  


&nbsp;&nbsp;Captura información de varios jugadores y muestra el estado actual del servicio de Xbox Live. 
  
  
[Configuración de página (página principal de desarrollo)](devhome-settings.md)  


&nbsp;&nbsp;Proporciona acceso a diversas configuraciones para la consola de desarrollo. 
  
  
[Medios de captura de página (página principal de desarrollo)](devhome-capture.md)  


&nbsp;&nbsp;El **záznam Média** página de inicio del desarrollo captura vídeo del título que se está ejecutando actualmente en la consola. 
  
  
[Página de redes (página principal de desarrollo)](devhome-networking.md)  


&nbsp;&nbsp;Simula diversas condiciones de red para solucionar problemas. 
  
  
[Rendimiento de página (página principal de desarrollo)](devhome-performance.md)  


&nbsp;&nbsp;Simula distintos de la actividad del disco y las condiciones de uso de CPU para solucionar problemas. 
 
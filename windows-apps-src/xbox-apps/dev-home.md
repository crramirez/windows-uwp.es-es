---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Inicio del Desarrollador en la consola (dev Home)
description: Obtenga información sobre cómo la experiencia de las herramientas de desarrollo doméstico en el kit de desarrollo de consola Xbox One ayuda a la productividad del desarrollador.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 40100adb1bd9337d933b8ebd155847bde71e341a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172819"
---
# <a name="developer-home-on-the-console-dev-home"></a>Inicio del Desarrollador en la consola (dev Home)
   
  
Dev Home es una experiencia de herramientas en el kit de desarrollo de Xbox One diseñada para ayudar a la productividad del desarrollador. Dev Home ofrece funcionalidad para administrar y configurar el kit de desarrollo, administrar usuarios, iniciar títulos instalados y realizar capturas y seguimientos. En versiones futuras, seguiremos expandiendo la funcionalidad para habilitar características adicionales en función de sus comentarios y también para habilitar la extensibilidad y la incorporación de sus propias herramientas.   
   
  
Estamos muy interesados en sus comentarios sobre dev Home y los escenarios en los que más le interesa ver soporte técnico. Proporcione sus comentarios a través de los métodos que se describen en **Enviar comentarios** en el menú principal de la aplicación o a través del administrador de cuentas de desarrollador (DAM).   
   
  
Para iniciar dev Home en la recuperación de noviembre de 2015 o una versión posterior:  
 
   1. Abra la guía desplazándose a la izquierda en casa o haciendo doble clic en el botón de nexo  
   1. Bajar a **configuración** (icono de engranaje)   
   1. Seleccionar **toda la configuración**  
   1. En la página del **desarrollador** predeterminada, seleccione **desarrollador Inicio** (el icono Inicio).   

 ![](images/dev_home_icons.png)   
  
En las recuperaciones anteriores, seleccione el icono de inicio de desarrollo en el lado derecho de la pantalla principal en **contenido destacado** o vea la lista de aplicaciones en el administrador de Xbox One e inicie **dev Home**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interfaz de usuario  
   
  
El encabezado de la interfaz de usuario de dev Home contiene la siguiente información importante acerca de la consola de desarrollo:   
 
   *  **IP de la consola:** La dirección IP actual de la consola.   
   *  **Nombre de la consola:** El nombre de host actual de la consola.  
   *  **Espacio aislado:** Nombre del espacio aislado en el que se encuentra la consola.  
   *  **Versión del sistema operativo:** La versión de recuperación actual que se está ejecutando en la consola.
   *  Hora actual del sistema.   

   
  
El resto de la interfaz de usuario de dev Home está dividida en las páginas siguientes. Para obtener más información acerca de las herramientas de estas páginas, vea sus temas individuales.   
 
   *  [Página principal](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Configuración](devhome-settings.md)  
   *  [Captura multimedia](devhome-capture.md)  
   *  [Redes](devhome-networking.md)  
   *  [Rendimiento](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menú principal  
   
  
Al presionar el botón de **menú** del controlador, puede acceder al menú principal que permite configurar el área de trabajo de la aplicación, la capacidad de administrar las credenciales para acceder a ubicaciones de red e información sobre cómo proporcionar comentarios sobre la aplicación.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Experiencia de usuario en modo de ajuste  
   
  
Varias herramientas existentes y futuras de dev Home, como networking y multijugador, están diseñadas para usarse ajustadas al lado mientras se está ejecutando el título, de modo que pueda tener acceso fácilmente a las herramientas mientras está probando.   
   
  
Para acceder al modo de ajuste, resalte el título de la herramienta correspondiente, presione el botón **Ver** en el controlador y seleccione **ajustar** en el menú contextual:  
 ![](images/dev_home_4.png)   
  
Dev Home se acoplará a la derecha. Puede cambiar el contexto si presionas dos veces el botón Nexo de la forma habitual.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizar Dev Home  
   
  
Dev Home se diseñó para ser personalizable y cercana. Puede configurar la aplicación para que se adapte a su flujo de trabajo y, a continuación, guardarla como un área de trabajo. Esta área de trabajo se puede exportar e importar, lo que permite copiar el diseño en otras consolas según sea necesario. Estas opciones se encuentran en el menú principal, en **área de trabajo**. El archivo exportado se ubicará en la unidad temporal del sistema en el `Dev Home\Workspaces` directorio.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Cambiar el tamaño y reordenar las herramientas  
   
  
Para cambiar el tamaño o la posición de una herramienta, use el botón de menú contextual (botón ver del controlador) mientras el título tenga el foco. En el menú contextual, seleccione **movimiento o cambio** de **tamaño**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Cambiar el color del tema y la imagen de fondo  
   
  
En el menú principal, puede seleccionar **área de trabajo** y, a continuación, cambiar el **color del tema**. Seleccione un color nuevo y seleccione **Guardar** para actualizar el color del tema que se usa para resaltar el foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configuración de la aplicación predeterminada para un paquete  
   
  
Si un paquete contiene varias aplicaciones, dev Home le permitirá establecer la aplicación predeterminada que se va a iniciar. Resalte el paquete en el iniciador y presione **el botón a para abrir la lista** de aplicaciones disponibles. Resalte el que desee establecer como predeterminado y haga clic en el botón **Ver** y, a continuación, elija **establecer como predeterminado** en el menú contextual.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Uso de dev Home para registrar y iniciar títulos desde un recurso compartido de red  
   
  
En el selector, en la parte inferior de la lista de aplicaciones y juegos instalados, puede seleccionar la opción **registrar un juego desde un recurso compartido de red** para ejecutar una versión de archivo flexible de un título de forma remota.   
 ![](images/dev_home_8.png)   
  
Después, puede escribir la ruta de acceso de red al archivo appxmanifest.xml para el título que desea registrar. Dev Home intentará registrar el título con las credenciales existentes para ese recurso compartido y, si es necesario, solicitará nuevas credenciales de red. Si necesita tener acceso a recursos compartidos adicionales (por ejemplo, para tener acceso a recursos vinculados simbólicamente en un servidor independiente), deberá agregarlos a través de la opción siguiente.   
   
  
Puede administrar estas credenciales almacenadas (y agregar otras) en la consola a través de la opción **administrar credenciales de red** del menú principal.   
 ![](images/dev_home_9.png)   
  
Para ver las credenciales que se encuentran actualmente en la consola de, edite las credenciales; para ello, seleccione la ruta de acceso de la credencial y haga clic en **un** botón para quitar una credencial. para ello, seleccione el vínculo quitar y haga clic en **un** botón.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>En esta sección  
  
[Página principal (dev Home)](devhome-home.md)  


&nbsp;&nbsp;Proporciona acceso rápido a las tareas que se realizan habitualmente en una consola de desarrollo. 
  
  
[Página de Xbox Live (dev Home)](devhome-live.md)  


&nbsp;&nbsp;Captura la información de varios jugadores y muestra el estado actual del servicio Xbox Live. 
  
  
[Página de configuración (dev Home)](devhome-settings.md)  


&nbsp;&nbsp;Proporciona acceso a varias opciones de configuración de la consola de desarrollo. 
  
  
[Página captura de medios (dev Home)](devhome-capture.md)  


&nbsp;&nbsp;La página **captura multimedia** de dev Home captura el vídeo del título que se está ejecutando actualmente en la consola. 
  
  
[Página redes (dev Home)](devhome-networking.md)  


&nbsp;&nbsp;Simula varias condiciones de red para la solución de problemas. 
  
  
[Página rendimiento (dev Home)](devhome-performance.md)  


&nbsp;&nbsp;Simula diversas condiciones de uso de CPU y actividad de disco para la solución de problemas. 
 
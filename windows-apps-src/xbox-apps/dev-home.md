---
author: v-angraf
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Página principal de desarrollador en la consola (desarrollo principal)
description: Proporciona información acerca de la aplicación principal de desarrollo para Xbox uno.
ms.author: v-angraf@microsoft.com
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 3b802b9b53811e03e11ee3afd78f69db4bfd9986
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1015364"
---
# <a name="developer-home-on-the-console-dev-home"></a>Página principal de desarrollador en la consola (desarrollo principal)
   
  
Página principal de desarrollo es una experiencia de herramientas en el kit de desarrollo de Xbox uno diseñada para ayudar en la productividad del desarrollador. Desarrollo principal ofrece funcionalidad para administrar y configurar el kit de desarrollo, administración de usuarios, inicio títulos instalados y realizar captura y realiza un seguimiento. En futuras versiones que seguiremos para expandir la funcionalidad para habilitar las características adicionales en función de sus comentarios y también para habilitar la extensibilidad y la adición de sus propias herramientas.   
   
  
Estamos muy interesados en sus comentarios en el desarrollo principal y los escenarios que más les interesa ver lo admite. Proporcione sus comentarios a través de los métodos descritos en **Enviar comentarios** en el menú principal de la aplicación o a través de su administrador de cuenta para desarrolladores (DAM).   
   
  
Para iniciar el desarrollo principal en el de noviembre de 2015 o posterior recuperación:  
 
   1. Abrir a la Guía de moviendo izquierdo en principal o hacer doble clic en el botón nexo  
   1. Mover hacia abajo a la **configuración** (el icono del engranaje)   
   1. Seleccione **todas las opciones**  
   1. Desde la página de **desarrollador** de forma predeterminada, seleccione **Página principal para desarrolladores** (el icono de inicio)   

 ![](images/dev_home_icons.png)   
  
En anteriores recuperaciones seleccione el icono de desarrollo principal en el lado derecho de la pantalla principal en **contenido destacado** o ver la lista de aplicaciones en el Administrador de una Xbox e inicie **Principal de desarrollo**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interfaz de usuario  
   
  
El encabezado de la interfaz de usuario principal de desarrollo contiene las siguientes importantes "de un vistazo" información acerca de la consola de desarrollo:   
 
   *  **De consola de IP:** La dirección IP actual de la consola.   
   *  **Nombre de la consola:** El nombre de host actual de la consola.  
   *  **Espacio aislado:** El nombre del espacio aislado que se encuentra en la consola.  
   *  **Versión del sistema operativo:** La versión actual de recuperación que se está ejecutando en la consola.
   *  Hora actual del sistema.   

   
  
El resto de la UI de desarrollo principal se divide en las siguientes páginas. Para obtener más información acerca de las herramientas en estas páginas, vea sus temas individuales.   
 
   *  [Casa](devhome-home.md)  
   *  [XboxLive](devhome-live.md)  
   *  [Settings](devhome-settings.md)  
   *  [Captura multimedia](devhome-capture.md)  
   *  [Redes](devhome-networking.md)  
   *  [Rendimiento](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menú principal  
   
  
Al presionar el botón de **menú** en el controlador, puede tener acceso el menú principal que permite la configuración de la aplicación del área de trabajo, la capacidad para administrar las credenciales para obtener acceso a las ubicaciones de red y obtener información acerca de proporcionar comentarios sobre la aplicación.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Se ajusta a modo de experiencia de usuario  
   
  
Varias herramientas existentes y futuras en la página principal de desarrollo, como redes y Multiplayer, están diseñadas para usarse ajustados en el lado mientras se ejecuta su título, por lo que puede tener acceso fácil a herramientas mientras se está probando.   
   
  
Para obtener acceso a modo de complemento, resalte el título de la herramienta apropiada, presione el botón de **vista** en el controlador y seleccione **se ajusta** en el menú contextual:  
 ![](images/dev_home_4.png)   
  
Dev Home se acoplará a la derecha. Puede cambiar el contexto si presionas dos veces el botón Nexo de la forma habitual.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizar Dev Home  
   
  
Dev Home se diseñó para ser personalizable y cercana. Puede configurar la aplicación para adaptarse a su flujo de trabajo y, a continuación, guarde como un área de trabajo. Esta área de trabajo se puede exportar e importadas, lo que permite copiar el diseño a otras consolas como sea necesario. Estas opciones se encuentran en el menú principal en el **área de trabajo**. El archivo exportado se encuentran en la unidad del sistema borrador en el `Dev Home\Workspaces` Active directory.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Cambiar el tamaño y la reordenación de las herramientas  
   
  
Para cambiar el tamaño o la posición de una herramienta, use el botón de menú contextual (botón de vista en el controlador) mientras el título tiene el foco. En el menú contextual, seleccione **mover** o **cambiar su tamaño**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Cambiar el color del tema y la imagen de fondo  
   
  
En el menú principal, puede seleccionar el **área de trabajo** y, a continuación, **cambiar el color de tema**. Seleccione un nuevo color y seleccione **Guardar** para actualizar el color del tema usado para resaltar el foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configuración de la aplicación predeterminada para un paquete  
   
  
Si un paquete contiene varias aplicaciones, desarrollo principal le permitirá establecer la aplicación predeterminada que se iniciará. Resalte el paquete en el selector y presione **el botón para abrir la lista de aplicaciones disponibles** . Resaltar aquel en el que desea establecer como predeterminado y presione el botón **vista** y, a continuación, elija **establecer como predeterminado** en el menú contextual.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Uso de desarrollo principal para registrar e iniciar los títulos de un recurso compartido de red  
   
  
Desde el iniciador, en la parte inferior de las aplicaciones instaladas y la lista de juegos, puede seleccionar la opción **registrar un juego desde un recurso compartido de red** para ejecutar una versión de archivo separado de un título de forma remota.   
 ![](images/dev_home_8.png)   
  
A continuación, puede escribir la ruta de acceso de red en el archivo appxmanifest.xml para el título que desea registrar. Desarrollo principal se intentará registrar el título con todas las credenciales existentes para ese recurso compartido y si necesita solicitará nuevas credenciales de red. Si necesita tener acceso a recursos compartidos adicionales (por ejemplo a los recursos de acceso simbólicamente vinculado en un servidor independiente) necesitará agregar aquellas a través de la opción que aparece a continuación.   
   
  
Puede administrar estas credenciales almacenadas (y agregar otros nuevos) en la consola a través de la opción de **administrar las credenciales de red** del menú principal.   
 ![](images/dev_home_9.png)   
  
Puede ver las credenciales actualmente en la consola, editar las credenciales, seleccione la ruta de acceso de la credencial y hacer clic en **un** botón y quitar una credencial, seleccione el vínculo quitar y hacer clic en **un** botón.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>En esta sección  
  
[Página principal (desarrollo principal)](devhome-home.md)  


&nbsp;&nbsp;Proporciona acceso rápido a las tareas que se realizan habitualmente en una consola de desarrollo. 
  
  
[Xbox Live página (página principal de desarrollo)](devhome-live.md)  


&nbsp;&nbsp;Captura información con varios jugadores y muestra el estado actual del servicio Xbox Live. 
  
  
[Página de configuración (desarrollo principal)](devhome-settings.md)  


&nbsp;&nbsp;Proporciona acceso a diversas opciones para la consola de desarrollo. 
  
  
[Medios capturan página (página principal de desarrollo)](devhome-capture.md)  


&nbsp;&nbsp;La página de **capturan de medios** de desarrollo principal captura vídeo del título que se está ejecutando actualmente en la consola. 
  
  
[Página de red (desarrollo principal)](devhome-networking.md)  


&nbsp;&nbsp;Simula diversas condiciones de redes para solucionar problemas. 
  
  
[Página de rendimiento (desarrollo principal)](devhome-performance.md)  


&nbsp;&nbsp;Simula diversas condiciones de uso de CPU para fines de solución de problemas y la actividad de disco. 
 
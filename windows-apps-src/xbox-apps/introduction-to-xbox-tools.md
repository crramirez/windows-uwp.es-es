---
author: Mtoepke
title: "Introducción a las herramientas de Xbox One"
description: "La herramienta específica de Xbox One, Dev Home, con Windows Device Portal."
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 4414677e942818506020888fa15e7e16ecaf4733

---

# Introducción a las herramientas de Xbox One

Esta sección describe la herramienta específica de Xbox One, _Dev Home_, con Windows Device Portal.

## Dev Home


              _Dev Home_ es una experiencia de herramientas del kit de desarrollo de Xbox One diseñada para ayudar en la productividad del desarrollador. Ofrece funcionalidad para administrar y configurar el kit de desarrollo.

Para abrir Dev Home, selecciona la ventana **Dev Home** en la pantalla principal. Si no ves ninguna ventana, la consola no está en modo de desarrollador.

  ![Windows Device Portal](images/windowsdeviceportal_1.png)

### Interfaz de usuario
La interfaz de usuario de Dev Home se divide en las áreas que se describe en las siguientes secciones. Ten en cuenta que aquí se muestran la dirección IP de la consola y el nombre descriptivo.

  ![Interfaz de usuario de DevHome](images/devhome_ui.png)

#### Encabezado
El encabezado contiene un resumen de la información importante sobre el kit de desarrollo. Esto incluye el nombre de la consola, su dirección IP, el espacio aislado de Xbox Live en que se encuentra y la versión del sistema operativo que se está ejecutando. A la derecha del encabezado, se muestra la hora actual del sistema y la fecha, para mayor comodidad.

#### Ventanas de herramientas
Bajo el encabezado se encuentra el área principal de la aplicación, que contiene un conjunto de ventanas de herramientas configurable. Estos están pensados para permitir que los desarrolladores personalicen la aplicación para proporcionar acceso a diversas herramientas y conjuntos de información. Para obtener más información acerca de las herramientas, consulta las siguientes descripciones de herramienta individuales. Para obtener información sobre cómo configurar el diseño y la apariencia de las ventanas de herramientas, consulta la sección [Personalizar Dev Home](#customizing-dev-home) más adelante en esta página.

##### Menú principal
Si presionas el botón **Menú** en el controlador o vas al botón de menú (la "hamburguesa") en la parte superior izquierda de la pantalla, puedes acceder al menú principal que te permite configurar el color del tema y la imagen de fondo del área de trabajo de la aplicación, además de enviar comentarios sobre la aplicación.

  ![Menú principal](images/devhome_mainmenu.png)

#### Modo acoplado
Las herramientas de Dev Home pueden acoplarse a un lado mientras ejecutas el título para que puedas tener acceso fácil a las herramientas durante la prueba.

Para acceder al modo **Acoplar**, selecciona el título de la herramienta adecuada, presiona el botón **Vista** en el controlador y, finalmente, selecciona **Acoplar** en el menú contextual.

  ![Modo acoplado](images/devhome_snapmode.png)

Dev Home se acoplará a la derecha. Puede cambiar el contexto si presionas dos veces el botón **Nexo** de la forma habitual.

  ![Nexo](images/devhome_nexus.png)

##### Descripciones de herramientas
| Herramienta  | Funcionalidades |
|-------|--------------|
| Juegos y aplicaciones  | Enumera los títulos y las aplicaciones instaladas en el kit de desarrollo y la capacidad para abrirlos rápidamente. También puedes ver el estado de la administración del ciclo de vida de los procesos (PLM) de juegos y aplicaciones, así como cambiar los estados PLM desde un menú contextual. |
| Usuarios | Enumera los usuarios registrados actualmente en la consola. Habilita el inicio y cierre de sesión de usuario de un solo clic, la opción de agregar usuarios e invitados y la visualización de los detalles de dichos usuarios e invitados. |
| [Configuración de la consola](#console-settings) | Ofrece una vista resumida y opciones de configuración de la consola y de la información. |
| VisualStudio | Te permite emparejar la consola con una instancia de Visual Studio para permitir la implementación. Si es necesario, desactiva las instancias existentes de VS emparejadas para evitar la implementación de la aplicación para UWP en un kit. |
| [Windows Device Portal](#windows-device-portal) | Habilita WDP (una herramienta de administración de dispositivos basada en explorador) en el kit. |
| Estado de Xbox Live | Proporciona el estado actual del servicio Xbox Live. |

### Administrar el tamaño de la asignación de almacenamiento de desarrollador

Para aumentar o disminuir la cantidad de espacio en disco que se usa para el almacenamiento de desarrollador, selecciona **Administrar el almacenamiento de desarrollador** en el menú principal. Cambia el valor de la barra **Almacenamiento de desarrollador** y luego selecciona **Guardar y reiniciar** para reiniciar la consola.
  ![Administrar la asignación de almacenamiento de desarrollador](images/devhome_storage.png)

### Personalizar Dev Home

Dev Home se diseñó para ser personalizable y cercana. Puedes elegir una imagen de fondo y un color de tema para personalizar tu experiencia de Dev Home. Estas son las opciones que encontrarás en el menú principal.

#### Cambio de tamaño y orden de las herramientas
Para cambiar el tamaño o la posición de una herramienta, usa el botón de menú contextual (botón **Vista** del controlador) mientras el foco se encuentre en el título. En el menú contextual, selecciona **Mover** o **Cambiar tamaño**.

  ![Mover o cambiar tamaño](images/devhome_move.png)

#### Cambiar el color del tema y la imagen de fondo
En el menú principal, puedes seleccionar **Cambiar color del tema**. Para actualizar el color del tema usado para resaltar el foco, selecciona un color nuevo y, a continuación, haz clic en **Guardar**.

  ![Cambiar color del tema](images/devhome_colors.png)

### Enviar comentarios
Para enviar comentarios sobre Dev Home o cualquiera de los procesos de herramientas, selecciona la opción **Enviar comentarios** en el menú principal.

  ![Enviar comentarios](images/devhome_feedback.png)

## Configuración de la consola
La herramienta de configuración de la consola proporciona acceso rápido a la configuración del kit de desarrollo.

### Establecer un nombre de host para la consola
Cuando se comuniques con la consola desde el equipo de desarrollo, puedes establecer un nombre descriptivo (denominado _nombre de host_) para el kit de desarrollo de Xbox One como alternativa a la dirección IP de consola. El equipo de desarrollo y el kit de desarrollo deben estar en la misma subred para que funcione la conectividad de nombre de host.  

Para definir un nombre de host para un kit de desarrollo, ve a la herramienta de configuración de la consola y escribe el nombre de host en el cuadro __Nombre de host__.  

  > 
              **Nota**&nbsp;&nbsp;No se requiere un nombre exclusivo para la creación del nombre de host. Por lo tanto, intenta evitar la duplicación de nombres. Una manera de hacerlo es derivar el nombre de host del nombre de tu equipo de desarrollo, que normalmente es único dentro de una organización.

## Windows Device Portal
Windows Device Portal (WDP) es una herramienta de administración de dispositivos OneCore que permite una experiencia de administración de dispositivo basada en explorador.

Para habilitar WDP en la consola Xbox One:

1. Selecciona la ventana de Dev Home en la pantalla principal.

  ![Seleccionar la ventana de Dev Home](images/windowsdeviceportal_1.png)

2. En Dev Home, desplázate a la herramienta **Administración remota**.

  ![Herramienta de administración remota](images/windowsdeviceportal_2.png)

3. Selecciona __Administrar Windows Device Portal__y, a continuación, presiona __A__.
4. Selecciona la casilla __Habilitar Windows Device Portal__.
5. Escribe un __Nombre de usuario__ y __Contraseña__y guárdalos. Estos se usan para autenticar el acceso a tu kit de desarrollo desde un explorador.
6. Cierra la página de __Configuración__ y ten en cuenta la dirección URL que aparece en la herramienta _Administración remota_ para poder conectarte
7. Escribe la dirección URL en el explorador y, a continuación, inicia sesión con las credenciales que has configurado.
8. Recibirás una advertencia acerca del certificado proporcionado, similar a la siguiente captura de pantalla, porque el certificado de seguridad firmado por la consola Xbox One no se considera un editor de confianza conocido. Haz clic en **Continuar a este sitio web** para acceder a Windows Device Portal.

  ![Advertencia de certificado de seguridad](images/security_cert_warning.jpg)

## Consulta también
- [Cómo usar Fiddler con Xbox One al desarrollar para la UWP](uwp-fiddler.md)
- [Tecnologías de Microsoft Developer: Windows Device Portal](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox)
- [UWP en Xbox One](index.md)



----



<!--HONumber=Jul16_HO2-->



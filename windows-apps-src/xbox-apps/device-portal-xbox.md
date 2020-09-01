---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal para Xbox
description: Obtenga información sobre cómo habilitar el portal de dispositivos de Xbox para Xbox One, que le proporciona acceso remoto a la consola de desarrollo de Xbox.
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: ed490b0474b919d4439e5b74b676d5974a3c6a30
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174789"
---
# <a name="device-portal-for-xbox"></a>Device Portal para Xbox

## <a name="set-up-device-portal-on-xbox"></a>Configurar Device Portal en Xbox

En los pasos siguientes se muestra cómo habilitar el portal de dispositivos de Xbox, que le proporciona acceso remoto a la consola de desarrollo de Xbox.

1. Abre Dev Home. Esto debería abrirse de forma predeterminada al arrancar la consola de desarrollo de Xbox, pero también puede abrirla desde la pantalla principal.

    ![Página principal para desarrolladores de Device Portal](images/device-portal-xbox-1.png)

2. En dev Home, en la pestaña **Inicio** , en **acceso remoto**, seleccione **configuración de acceso remoto**.

    ![Herramienta Administración remota de Device Portal](images/device-portal-xbox-15.png)

3. Active la opción **Habilitar el portal de dispositivos Xbox** .

4. En **autenticación**, seleccione **establecer nombre de usuario y contraseña**. Escriba un **nombre de usuario** y una **contraseña** que se usarán para autenticar el acceso al kit de desarrollo desde un explorador y **guardarlos** .

5. **Cierre** la página **acceso remoto** y anote la dirección URL que aparece en **acceso remoto** en la pestaña **Inicio** .

6. Escribe la dirección URL en el explorador y, a continuación, inicia sesión con las credenciales que has configurado.

7. Recibirá una advertencia sobre el certificado proporcionado, similar a la imagen siguiente. En Edge, haga clic en **detalles** y, luego, **vaya a la página web** para acceder al portal de dispositivos de Xbox. En el cuadro de diálogo que aparece, escriba el nombre de usuario y la contraseña que especificó anteriormente en la consola Xbox.

    ![Error de certificado de Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Páginas de Device Portal

El portal de dispositivos de Xbox proporciona un conjunto de páginas estándar similar a lo que está disponible en el portal de dispositivos de Windows, así como varias páginas que son únicas. Para obtener descripciones detalladas de la primera, consulte [información general del portal de dispositivos de Windows](../debug-test-perf/device-portal.md). En las secciones siguientes se describen las páginas que son exclusivas del portal de dispositivos de Xbox.

### <a name="home"></a>Inicio

De forma similar a la página de **aplicaciones** del portal de dispositivos de Windows, la página **principal** del portal de dispositivos de Xbox muestra una lista de aplicaciones y juegos instalados en **mis juegos & aplicaciones**. Puede hacer clic en el nombre de un juego o una aplicación para ver más detalles sobre él, como el **nombre de familia del paquete**. En el menú desplegable **acciones** , puede realizar una acción en el juego o la aplicación, como **iniciarlo** .

En **cuentas de prueba de Xbox Live**, puede administrar las cuentas asociadas a su Xbox. Puede Agregar usuarios y cuentas de invitado, crear nuevos usuarios, iniciar y cerrar la sesión de los usuarios y quitar cuentas.

![Inicio](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (juego guardado)

Tanto el portal de dispositivos de Windows como el portal de dispositivos de Xbox tienen una página de **Xbox Live** . Sin embargo, el portal de dispositivos de Xbox tiene una sección única, **Xbox Live Game**Save, donde puede guardar los datos de los juegos instalados en la consola Xbox. Escriba el **ID. de configuración de servicio (SCID)** (consulte [configuración del servicio Xbox Live](/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids) para obtener más información), **memberName (MSA)** y **nombre de familia de paquete (PFN)** asociados al guardado del título y el juego, busque el archivo de **entrada (. JSON o. xml)** y seleccione uno de los botones (**restablecer**, **importar**, **exportar**y **eliminar**) para manipular los datos guardados

En la sección **generar** , puede generar datos ficticios y guardarlos en el archivo de entrada especificado. Simplemente escriba los **contenedores (el valor predeterminado es 2)**, los **BLOBs (valor predeterminado 3)** y **el tamaño del BLOB (el valor predeterminado es 1024)** y seleccione **generar**.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>Monitor HTTP

El monitor HTTP le permite ver el tráfico HTTP y HTTPS descifrado desde la aplicación o el juego cuando se ejecuta en la Xbox One.

![Monitor HTTP](images/device-portal-xbox-18.png)

Para habilitarlo, abra dev Home en la Xbox One, vaya a la pestaña **configuración** y, en el cuadro **configuración del monitor http** , Active **Habilitar monitor http**.

![Dev Home: redes](images/device-portal-xbox-14.png)

Una vez habilitado, en el portal de dispositivos de Xbox, puede **detener**, **Borrar**y guardar el tráfico HTTP y https del **archivo** seleccionando los botones correspondientes.

### <a name="network-fiddler-tracing"></a>Red (seguimiento de Fiddler)

La página **red** del portal de dispositivos Xbox es casi idéntica a la página **redes** del portal de dispositivos de Windows, con la excepción del **seguimiento de Fiddler**, que es único en el portal de dispositivos de Xbox. Esto le permite ejecutar Fiddler en su equipo para registrar e inspeccionar el tráfico HTTP y HTTPS entre su Xbox One e Internet. Consulte [Cómo usar Fiddler con Xbox One al desarrollar para UWP](../xbox-apps/uwp-fiddler.md) para obtener más información.

![Red](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Captura multimedia

En la página **captura multimedia** , puede seleccionar captura de **pantalla** para realizar una captura de pantalla de la Xbox One. Una vez hecho esto, el explorador le pedirá que descargue el archivo. Puede activar **preferir HDR** si desea realizar la captura de pantalla en HDR (si la consola lo admite).

![Captura multimedia](images/device-portal-xbox-12.png)

### <a name="settings"></a>Configuración

En la página **configuración** , puede ver y editar varios valores de configuración de la consola Xbox One. En la parte superior, puede seleccionar **importar** para importar la configuración de un archivo y **exportar** para exportar la configuración actual a un archivo. txt. La importación de la configuración puede facilitar la edición masiva, especialmente cuando se configuran varias consolas. Para crear un archivo de configuración que se va a importar, cambie la configuración a la que desee y, a continuación, exporte la configuración. A continuación, puede usar este archivo para importar la configuración de forma rápida y sencilla para otras consolas.

Hay varias secciones con configuraciones diferentes para ver o editar, que se explican a continuación.

![Configuración](images/device-portal-xbox-20.png)

![Configuración](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Información del dispositivo

* **Nombre del dispositivo**: el nombre del dispositivo. Para editarlo, cambie el nombre en el cuadro y seleccione **Guardar**.

* **Versión del sistema operativo**: solo lectura. Número de versión del sistema operativo.

* **Os Edition**: solo lectura. El nombre de la versión principal del sistema operativo.

* **ID. de dispositivo de Xbox Live**: solo lectura.

* **ID**. de consola: solo lectura.

* **Número de serie**: solo lectura.

* **Tipo de consola**: solo lectura. El tipo de dispositivo Xbox One (Xbox One, Xbox One S o Xbox One X).

* **Modo de desarrollo**: solo lectura. El modo de programador en el que se encuentra el dispositivo.

#### <a name="audio-settings"></a>Configuración de audio

* **Formato de audio fragmentada**: el formato de los datos de audio.

* **Audio HDMI**: el tipo de audio a través del puerto HDMI.

* **Formato de auriculares**: el formato del audio que se suministra a través de los auriculares.

* **Audio óptico**: el tipo de audio a través del puerto óptico.

* **Use HDMI o auriculares de audio óptico**: Active esta casilla si usa un auricular conectado a través de HDMI o óptico.

#### <a name="display-settings"></a>Configuración de pantalla

* **Profundidad de color**: el número de bits usados para cada componente de color de un solo píxel.

* **Espacio de colores**: la gama de colores disponible para la pantalla.

* **Resolución de pantalla**: resolución de la pantalla.

* **Mostrar conexión**: el tipo de conexión a la pantalla.

* **Permitir intervalo dinámico alto (HDR)**: habilita HDR en la pantalla. Solo disponible para pantallas compatibles.

* **Allow 4k**: habilita la resolución de 4k en la pantalla. Solo disponible para pantallas compatibles.

* **Permitir frecuencia de actualización de variables (VRR)**: habilite VRR en la pantalla. Solo disponible para pantallas compatibles.

#### <a name="kinect-settings"></a>Configuración de Kinect

Un sensor de Kinect debe estar conectado a la consola para poder cambiar esta configuración.

* **Habilitar Kinect**: habilita el sensor de Kinect conectado.

* **Forzar recarga de Kinect al cambiar la aplicación**: vuelve a cargar el sensor de Kinect asociado cada vez que se ejecuta una aplicación o un juego diferente.

#### <a name="localization-settings"></a>Configuración de localización

* **Región geográfica**: la región geográfica en la que se establece el dispositivo. Debe ser el código de país específico de 2 caracteres (por ejemplo, **US** para Estados Unidos).

* **Idiomas preferidos**: el idioma en el que está establecido el dispositivo.

* **Zona horaria**: zona horaria en la que se establece el dispositivo.

#### <a name="network-settings"></a>Configuración de red

* **Configuración de la radio inalámbrica**: la configuración inalámbrica del dispositivo (si determinados aspectos, como la LAN inalámbrica están activados o desactivados).

#### <a name="power-settings"></a>Configuración de energía

* **Cuando está inactivo, pantalla atenuar después de (minutos)**: la pantalla se atenúa después de que el dispositivo haya estado inactivo durante este período de tiempo. Establézcalo en **0** para no atenuar nunca la pantalla.

* **Cuando esté inactivo, desactive después**de: el dispositivo se apagará después de que haya estado inactivo durante este período de tiempo.

* **Modo de energía**: el modo de alimentación del dispositivo. Para obtener más información, consulte Acerca de los [modos de ahorro de energía y de energía instantánea](https://support.xbox.com/xbox-one/console/learn-about-power-modes) .

* **Iniciar automáticamente la consola cuando esté conectado a la energía**: el dispositivo se activará automáticamente cuando se conecte a una fuente de alimentación.

#### <a name="preference-settings"></a>Configuración de preferencias

* **Experiencia de inicio predeterminada**: establece la pantalla principal que aparece cuando el dispositivo está encendido.

* **Permitir conexiones desde la aplicación Xbox**: la aplicación Xbox en otro dispositivo (por ejemplo, un equipo con Windows 10) puede conectarse a esta consola.

* **Tratar aplicaciones para UWP como juegos de forma predeterminada**: los juegos y las aplicaciones obtienen distintos recursos asignados en Xbox. Si activa esta casilla, todos los paquetes UWP se identificarán como juegos y, por tanto, obtendrá más recursos.

#### <a name="user-settings"></a>Configuración del usuario

* **Usuario de inicio de sesión automático**: inicia sesión automáticamente en el usuario seleccionado cuando el dispositivo está activado.

* **Controlador de usuario de inicio de sesión automático**: asocia automáticamente un tipo de controlador determinado a un usuario determinado.

#### <a name="xbox-live-sandbox"></a>Espacio aislado de Xbox Live

Aquí puede cambiar el espacio aislado de Xbox Live en el que se encuentra el dispositivo. Escriba el nombre del espacio aislado en el cuadro y seleccione **cambiar**.

### <a name="scratch"></a>Borrador

Se trata de un área de trabajo en blanco que puede personalizar para su gusto. Puede usar el menú (haga clic en el botón de menú de la parte superior izquierda) para agregar herramientas (seleccione **Agregar herramientas al área de trabajo**, luego las herramientas que desea agregar y, a continuación, **Agregar**). Tenga en cuenta que puede usar este menú para agregar herramientas a cualquier área de trabajo, así como administrar las propias áreas de trabajo.

![Agregar herramientas al área de trabajo](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Datos de eventos de juego

En la página **datos de eventos del juego** , puede ver un gráfico en tiempo real que transmite por secuencias el número de eventos de juego de seguimiento de eventos para Windows (ETW) registrados actualmente en la consola Xbox One. Si hay eventos de juego registrados en el sistema, también puede ver los detalles (nombre del evento, aparición del evento y título del juego) que describen cada evento de una tabla de datos debajo del gráfico de datos. La tabla solo está disponible si hay eventos registrados.

![Datos de eventos de juego](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Vea también

* [Introducción al Portal de dispositivos Windows](../debug-test-perf/device-portal.md)
* [Referencia de API principal de Device Portal](../debug-test-perf/device-portal-api-core.md)
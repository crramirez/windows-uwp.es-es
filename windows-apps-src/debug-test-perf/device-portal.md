---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Introducción a Windows Device Portal
description: Obtén información sobre cómo Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB.
ms.date: 4/9/2019
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 39715dc3f4b88a2e9a91a7cb659208f8370f16f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362415"
---
# <a name="windows-device-portal-overview"></a>Introducción a Windows Device Portal

Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB. También proporciona herramientas avanzadas de diagnóstico para ayudarle a solucionar problemas y ver el rendimiento del dispositivo de Windows en tiempo real.

Windows Device Portal es un servidor web en el dispositivo que se puede conectar a desde un explorador web en un equipo. Si el dispositivo tiene un explorador web, también puede conectarse localmente con el explorador en ese dispositivo.

Windows Device Portal está disponible en cada familia de dispositivos, pero las características y el programa de instalación varían en función de los requisitos de cada dispositivo. En este artículo se proporciona una descripción general de Device Portal y vínculos a artículos con información más específica para cada familia de dispositivos.

La funcionalidad de la Windows Device Portal se implementa con [las API de REST](device-portal-api-core.md) que puede usar directamente para tener acceso a datos y controlar el dispositivo mediante programación.

## <a name="setup"></a>Programa de instalación

Cada dispositivo tiene instrucciones específicas para la conexión a Device Portal, pero todos requieren estos pasos generales:

1. Habilitar modo de desarrollador y Portal de dispositivos en el dispositivo (configurado en la aplicación de configuración).

2. Conecte su dispositivo y su equipo a través de una red local o con USB.

3. Navega a la página Device Portal en tu explorador. Esta tabla muestran los puertos y protocolos utilizados por cada familia de dispositivos.

Familia de dispositivos | ¿De forma predeterminada? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sí, en el modo de desarrollador | 80 (predeterminado) | 443 (predeterminado) | http://127.0.0.1:10080
IoT | Sí, en el modo de desarrollador | 8080 | Habilitar a través de la clave del Registro | N/D
Xbox | Habilitar dentro del modo de desarrollador | Deshabilitada | 11443 | N/D
Escritorio| Habilitar dentro del modo de desarrollador | 50080\* | 50043\* | N/D
Phone | Habilitar dentro del modo de desarrollador | 80| 443 | http://127.0.0.1:10080

\* Esto no siempre es el caso, como los puertos del intervalo efímero de notificaciones de Portal de dispositivos en el escritorio (> 50 000) para evitar conflictos con las notificaciones de puerto existentes en el dispositivo. Para obtener más información, consulta la sección de [Configuración de puertos](device-portal-desktop.md#registry-based-configuration-for-device-portal) referente a los equipos de escritorio.  

Para obtener instrucciones específicas sobre la configuración del dispositivo, consulta:

- [Portal de dispositivos para HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portal de dispositivo de IoT](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Portal de dispositivos para dispositivos móviles](device-portal-mobile.md)
- [Portal de dispositivos para Xbox](../xbox-apps/device-portal-xbox.md)
- [Portal de dispositivo de escritorio](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Características

### <a name="toolbar-and-navigation"></a>Barra de herramientas y navegación

En la parte superior de la página de la barra de herramientas proporciona acceso a características más usadas.

- **Power**: Opciones de energía de acceso.
  - **Shutdown**: Desactiva el dispositivo.
  - **Restart**: Los ciclos de energía en el dispositivo.
- **Ayudar a**: Se abre la página de ayuda.

Usa los vínculos del panel de navegación en el lado izquierdo de la página para navegar a las herramientas de administración y supervisión disponibles del dispositivo.

Aquí se describen las herramientas que son comunes a las familias de dispositivos. Puede que haya otras opciones disponibles en función del dispositivo. Para obtener más información, consulte la página específica de su tipo de dispositivo.

### <a name="apps-manager"></a>Administrador de aplicaciones

El Administrador de aplicaciones proporciona la instalación o desinstalación y funcionalidad de administración para la aplicación se empaqueta y se empaqueta en el dispositivo host.

![Página Administrador de aplicaciones del Portal de dispositivo](images/device-portal/WDP_AppsManager2.png)

* **Implementar aplicaciones**: Implemente aplicaciones empaquetadas en local, red o hosts de web y registre archivos separados en recursos compartidos de red.
* **Las aplicaciones instaladas**: Use el menú desplegable para quitar o iniciar las aplicaciones que están instaladas en el dispositivo.
* **Ejecutar aplicaciones**: Obtenga información acerca de las aplicaciones que se están ejecutando actualmente y cerrarlos según sea necesario.

#### <a name="install-sideload-an-app"></a>Instalar una aplicación (instalación de prueba)

Puede agregar o quitar aplicaciones durante el desarrollo con Windows Device Portal:

1. Cuando haya creado un paquete de aplicación, puede instalar remotamente en el dispositivo. Después de compilarlo en Visual Studio, se genera una carpeta de salida.

    ![Instalación de aplicaciones](images/device-portal/iot-installapp0.png)

2. En Windows Device Portal, vaya a la **Administrador de aplicaciones** página.

3. En el **implementar aplicaciones** sección, seleccione **almacenamiento Local**.

4. En **seleccione el paquete de aplicación**, seleccione **Elegir archivo** y busque el paquete de aplicación que desea transferir localmente.

5. En **Seleccione archivo de certificado (.cer) usado para firmar el paquete de aplicación**, seleccione **Elegir archivo** y busque el certificado asociado con ese paquete de aplicación.

6. Active las casillas correspondientes si desea instalar opcional o paquetes de framework, junto con la instalación de la aplicación y seleccione **siguiente** al elegirlos.

7. Seleccione **instalar** para iniciar la instalación.

8. Si el dispositivo se está ejecutando Windows 10 en el modo de S, y es la primera vez que se ha instalado el certificado proporcionado en el dispositivo, reinicie el dispositivo.

#### <a name="install-a-certificate"></a>Instalar un certificado

Como alternativa, puede instalar el certificado a través de Windows Device Portal e instalar la aplicación a través de otros medios:

1. En Windows Device Portal, vaya a la **Administrador de aplicaciones** página.

2. En el **implementar aplicaciones** sección, seleccione **instalar certificado**.

3. En **Seleccione archivo de certificado (.cer) usado para firmar un paquete de aplicación**, seleccione **Elegir archivo** y busque el certificado asociado con el paquete de aplicación que desea transferir localmente.

4. Seleccione **instalar** para iniciar la instalación.

5. Si el dispositivo se está ejecutando Windows 10 en el modo de S, y es la primera vez que se ha instalado el certificado proporcionado en el dispositivo, reinicie el dispositivo.

#### <a name="uninstall-an-app"></a>Desinstalar una aplicación

1. Asegúrate de que la aplicación no se esté ejecutando.
2. Si es así, vaya a **ejecutar aplicaciones** y ciérrelo. Si intenta desinstalar mientras se ejecuta la aplicación, provocará problemas cuando se intenta volver a instalar la aplicación.
3. Seleccione la aplicación en la lista desplegable y haga clic en **quitar**.

### <a name="running-processes"></a>Procesos en ejecución

Esta página muestra detalles acerca de los procesos que se están ejecutando actualmente en el dispositivo host. Esto incluye aplicaciones y procesos del sistema. En algunas plataformas (escritorio, IoT y HoloLens), pueden finalizar procesos.

![Dispositivo Portal ejecutando procesa la página](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorador de archivos

Esta página permite ver y manipular los archivos almacenados por las aplicaciones con instalación de prueba. Consulte la [mediante el Explorador de archivos de aplicación](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) del blog para obtener más información sobre el Explorador de archivos y cómo usarlo.

![Página del explorador de archivos del Portal de dispositivo](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Rendimiento

La página de rendimiento muestra gráficos en tiempo real de la información de diagnóstico del sistema, como el uso de energía, la velocidad de fotogramas, y carga de CPU.

Estas son las métricas disponibles:

- **CPU**: Porcentaje de utilización de CPU total disponible
- **Memoria**: Total, en uso, disponibilidad y confirmado, paginada y no paginada
- **I/O**: Cantidades de datos de lectura y escritura
- **Red**: Datos recibidos y enviados
- **GPU**: Porcentaje de utilización de motor GPU total disponible

![Página de rendimiento del Portal del dispositivo](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Registro de eventos de seguimiento para Windows (ETW)

La página de registro de ETW administra información de seguimiento de eventos para Windows (ETW) en tiempo real en el dispositivo.

![Página de registro de ETW del Portal del dispositivo](images/device-portal/mob-device-portal-etw.png)

Activa **Ocultar proveedores** para mostrar solamente la lista de eventos.

- **Proveedores registrados**: Seleccione el proveedor de eventos y el nivel de seguimiento. El nivel de seguimiento es uno de estos valores:
  1. Terminación o salida anómala
  2. Errores graves
  3. Warnings
  4. Advertencias sin errores
  5. Seguimiento detallado

  Haz clic o pulsa en **Activar** para iniciar el seguimiento. El proveedor se agrega a la lista desplegable de **Proveedores habilitados**.
- **Proveedores personalizados**: Seleccione un proveedor ETW personalizado y el nivel de seguimiento. Identifica el proveedor por su GUID. No incluya los corchetes en el GUID.
- **Los proveedores habilitados**: Enumera los proveedores habilitados. Selecciona un proveedor de la lista desplegable y haz clic o pulsa en **Desactivar** para detener el seguimiento. Haz clic o pulsa en **Detener todo** para suspender todos los seguimientos.
- **Historial de los proveedores**: Esto muestra los proveedores de ETW que estaban habilitados durante la sesión actual. Haz clic o pulsa en **Activar** para activar un proveedor deshabilitado. Haz clic o pulsa en **Borrar** para borrar el historial.
- **Filtros / eventos**: El **eventos** sección enumeran los eventos ETW de los proveedores seleccionados en formato de tabla. La tabla se actualiza en tiempo real. Use la **filtros** menú para configurar filtros personalizados para el que se mostrarán los eventos. Haga clic en el **clara** botón para eliminar todos los eventos ETW de la tabla. Esta acción no deshabilita ningún proveedor. Puede hacer clic en **guardar en el archivo** para exportar los eventos ETW actualmente recopilados en un archivo CSV local.

Para obtener más detalles sobre el uso de registro de ETW, consulte el [uso dispositivo del Portal para ver los registros de depuración](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) entrada de blog.

### <a name="performance-tracing"></a>Seguimiento del rendimiento

La página de seguimiento de rendimiento permite ver el [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) seguimientos desde el dispositivo host.

![Página de seguimiento de rendimiento de Portal de dispositivo](images/device-portal/mob-device-portal-perf-tracing.png)

- **Perfiles disponibles**: Seleccione el perfil WPR en la lista desplegable y haga clic o pulse **iniciar** para comenzar el seguimiento.
- **Perfiles personalizados**: Haga clic o pulse **examinar** para elegir un perfil WPR desde su PC. Haz clic o pulsa en **Cargar e iniciar** para iniciar el seguimiento.

Para detener el seguimiento, haz clic en **Detener**. Permanezca en esta página hasta que el archivo de seguimiento (. ETL) ha terminado de descargar.

Captura. Se pueden abrir los archivos ETL para su análisis en el [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Administrador de dispositivos

La página del Administrador de dispositivos enumera todos los periféricos conectados al dispositivo. Puede hacer clic en los iconos de configuración para ver las propiedades de cada uno.

![Página Administrador de dispositivo del Portal de dispositivo](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Funciones de red

La página red administra las conexiones de red en el dispositivo. A menos que se conecte al Portal de dispositivo a través de USB, cambiar la configuración se probablemente desconectará de Portal de dispositivos.

- **Las redes disponibles**: Muestra las redes Wi-Fi disponibles para el dispositivo. Al pulsar o hacer clic en una red, podrás conectarte a ella y proporcionar una clave de paso si es necesario. Portal de dispositivo no admite aún la autenticación empresarial. También puede usar el **perfiles** lista desplegable para intentar conectarse a cualquiera de los perfiles de Wi-Fi que se sabe que el dispositivo.
- **Configuración de IP**: Muestra información sobre cada uno de los puertos de red del dispositivo de host de dirección.

![Página de Portal de red del dispositivo](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Notas y las características del servicio

### <a name="dns-sd"></a>DNS-SD

Device Portal anuncia su presencia en la red local mediante DNS-SD. Todas las instancias de Device Portal, independientemente del tipo de dispositivo, se anuncian en "WDP._wdp._tcp.local". Los registros TXT de la instancia del servicio proporcionan lo siguiente:

Key | Tipo | Descripción
----|------|-------------
S | entero | Puerto seguro para Device Portal. Si es 0 (cero), Device Portal no escucha las conexiones HTTPS.
D | string | Tipo de dispositivo. Tendrá el formato "Windows.*"; por ejemplo, Windows.Xbox o Windows.Desktop.
A | string | Arquitectura del dispositivo. Será ARM, x86 o AMD64.  
T | lista de cadenas delineada con carácter nulo | Etiquetas aplicadas por el usuario para el dispositivo. Consulta como usarlas en la API de REST de etiquetas. La lista finaliza con carácter nulo doble.  

Se sugiere la conexión en el puerto HTTPS, ya que no todos los dispositivos escuchan en el puerto HTTP anunciado por el registro de DNS-SD.

### <a name="csrf-protection-and-scripting"></a>Protección CSRF y scripting

A fin de ofrecer protección frente a [ataques CSRF](https://wikipedia.org/wiki/Cross-site_request_forgery), se requiere un token único en todas las solicitudes no GET. Este token, el encabezado de la solicitud X-CSRF-Token, se deriva de una cookie de sesión, CSRF-Token. En la interfaz de usuario web de Device Portal, la cookie CSRF-Token se copia en el encabezado X-CSRF-Token en cada solicitud.

> [!IMPORTANT]
> Esta protección impide que los usos de las API de REST desde un cliente independiente (por ejemplo, las utilidades de línea de comandos). Esto puede resolverse de 3 maneras:
> - Use un nombre de usuario "auto-". Los clientes que antepongan "auto-" a su nombre de usuario omitirán la protección CSRF. Es importante que este nombre de usuario no se use para iniciar sesión en Device Portal a través del explorador, ya que abrirá el servicio a los ataques CSRF. Por ejemplo: Si el nombre de usuario del Portal de dispositivo es "admin", ```curl -u auto-admin:password <args>``` debe usarse para omitir la protección de CSRF.
> - Implementa el esquema de cookie a encabezado en el cliente. Se requiere una solicitud GET para establecer la cookie de sesión y, después, la inclusión del encabezado y la cookie en todas las solicitudes posteriores.
> - Deshabilita la autenticación y usa HTTP. La protección CSRF solo se aplica a los extremos HTTPS, para que las conexiones en extremos HTTP no tengan que realizar las acciones anteriores.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protección contra Cross-Site WebSocket Hijacking (CSWSH)

Para protegerse de los [ataques de CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos los clientes que abran una conexión WebSocket al Device Portal también deben proporcionar un encabezado Origin que coincida con el encabezado Host. Esto demuestra a Device Portal que la solicitud proviene de la interfaz de usuario de Device Portal o de una aplicación cliente válida. Sin el encabezado Origin, la solicitud se rechazará.

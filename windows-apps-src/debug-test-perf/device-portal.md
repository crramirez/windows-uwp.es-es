---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Introducción a Windows Device Portal
description: Obtén información sobre cómo Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB.
ms.date: 2/19/2019
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 372524b5a5f002e68c02a8f298703cb7ef99bfdf
ms.sourcegitcommit: a32f656253acc1e8b150919e73c886a59e65756a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "9083723"
---
# <a name="windows-device-portal-overview"></a>Introducción a Windows Device Portal

Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB. También proporciona herramientas de diagnóstico para ayudar a solucionar problemas y ver el rendimiento en tiempo real de tu dispositivo Windows.

Windows Device Portal es un servidor web en el dispositivo que se puede conectar a desde un explorador web en un equipo. Si el dispositivo tiene un explorador web, también puedes conectarte localmente con el explorador en ese dispositivo.

Windows Device Portal está disponible en cada familia de dispositivos, pero las características y la configuración varían en función de los requisitos de cada dispositivo. En este artículo se proporciona una descripción general de Device Portal y vínculos a artículos con información más específica para cada familia de dispositivos.

La funcionalidad de Windows Device Portal se implementa con [Las API de REST](device-portal-api-core.md) que puedes usar directamente para acceder a datos y controlar el dispositivo mediante programación.

## <a name="setup"></a>Programa de instalación

Cada dispositivo tiene instrucciones específicas para la conexión a Device Portal, pero todos requieren estos pasos generales:
1. Habilitar el modo de desarrollador y Device Portal en el dispositivo (configurado en la aplicación configuración).
2. Conecta el dispositivo y el equipo a través de una red local o con USB.
3. Navega a la página Device Portal en tu explorador. Esta tabla muestra los puertos y protocolos utilizados por cada familia de dispositivos.

Familia de dispositivos | ¿De forma predeterminada? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sí, en el modo de desarrollador | 80 (predeterminado) | 443 (predeterminado) | http://127.0.0.1:10080
IoT | Sí, en el modo de desarrollador | 8080 | Habilitar a través de la clave del Registro | N/D
Xbox | Habilitar dentro del modo de desarrollador | Deshabilitado | 11443 | N/D
Escritorio| Habilitar dentro del modo de desarrollador | 50080\* | 50043\* | N/D
Phone | Habilitar dentro del modo de desarrollador | 80| 443 | http://127.0.0.1:10080

\ * No siempre es el caso, ya que Device Portal para escritorio reclama puertos del rango efímero (> 50000) para evitar conflictos con reclamaciones de puertos existentes en el dispositivo. Para obtener más información, consulta la sección de [Configuración de puertos](device-portal-desktop.md#registry-based-configuration-for-device-portal) referente a los equipos de escritorio.  

Para obtener instrucciones específicas sobre la configuración del dispositivo, consulta:
- [Device Portal para HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Device Portal para IoT](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Device Portal para dispositivos móviles](device-portal-mobile.md)
- [Device Portal para Xbox](device-portal-xbox.md)
- [Device Portal para escritorio](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Funciones

### <a name="toolbar-and-navigation"></a>Barra de herramientas y navegación

La barra de herramientas en la parte superior de la página proporciona acceso a características de uso común.
- **Inicio/apagado**: obtener acceso a opciones de energía.
  - **Apagar**: apaga el dispositivo.
  - **Reiniciar**: vuelve a iniciar el dispositivo.
- **Ayuda**: abre la página de ayuda.

Usa los vínculos del panel de navegación en el lado izquierdo de la página para navegar a las herramientas de administración y supervisión disponibles del dispositivo.

Aquí se describen las herramientas que son comunes a las familias de dispositivos. Puede que haya otras opciones disponibles en función del dispositivo. Para obtener más información, consulta la página específica para el tipo de dispositivo.

### <a name="apps-manager"></a>Administrador de aplicaciones

El Administrador de aplicaciones proporciona la instalación o desinstalación y funcionalidad de administración de aplicación paquetes y lotes en el dispositivo host.

![Página Administrador de aplicaciones de Portal de dispositivos](images/device-portal/WDP_AppsManager2.png)

* **Implementar aplicaciones**: implementar las aplicaciones empaquetadas desde la red local, o web hosts y registrar en archivos sueltos de recursos compartidos de red.
* **Aplicaciones instaladas**: usar el menú desplegable para eliminar o iniciar las aplicaciones que están instaladas en el dispositivo.
* **Aplicaciones en ejecución**: obtener información sobre las aplicaciones que se están ejecutando actualmente y ciérralos según sea necesario.

#### <a name="install-sideload-an-app"></a>Instalar una aplicación (instalación de prueba)

Puedes transferir localmente aplicaciones durante el desarrollo con Windows Device Portal:

1.  Cuando hayas creado un paquete de la aplicación, podrás instalarlo remotamente en el dispositivo. Después de compilarlo en Visual Studio, se genera una carpeta de salida.

  ![Instalación de aplicaciones](images/device-portal/iot-installapp0.png)

2. En Windows Device Portal, ve a la página **Administrador de aplicaciones** .

3. En la sección de **implementar aplicaciones** , selecciona **Almacenamiento Local**.

4. En **Seleccionar el paquete de aplicación**, selecciona **Elegir un archivo** y busca el paquete de la aplicación que quieras realizar instalaciones de prueba.

5. En **Seleccionar el archivo de certificado (.cer) utilizado para firmar el paquete de la aplicación**, selecciona **Elegir un archivo** y busca el certificado asociado a ese paquete de la aplicación.

6. Casillas de verificación los respectivos si quieres instalar opcional o los paquetes de marcos junto con la instalación de la aplicación y selecciona **siguiente** para elegirlos.

7. Selecciona **instalar** para iniciar la instalación.

8. Si el dispositivo se está ejecutando Windows 10 en modo S y es la primera vez que el certificado se ha instalado en el dispositivo, reiniciar el dispositivo.

#### <a name="install-a-certificate"></a>Instalar un certificado

Como alternativa, puede instalar el certificado a través de Windows Device Portal e instalar la aplicación a través de otros medios:

1. En Windows Device Portal, ve a la página **Administrador de aplicaciones** .

2. En la sección de **implementar aplicaciones** , selecciona **Instalar certificado**.

3. En **Seleccionar el archivo de certificado (.cer) utilizado para firmar un paquete de aplicación**, selecciona **Elegir un archivo** y busca el certificado asociado con el paquete de la aplicación que quieras realizar instalaciones de prueba.

4. Selecciona **instalar** para iniciar la instalación.

5. Si el dispositivo se está ejecutando Windows 10 en modo S y es la primera vez que el certificado se ha instalado en el dispositivo, reiniciar el dispositivo.

#### <a name="uninstall-an-app"></a>Desinstalar una aplicación
1.  Asegúrate de que la aplicación no se esté ejecutando. 
2.  Si es así, ve a **aplicaciones en ejecución** y ciérralo. Si intentas desinstalar mientras se ejecuta la aplicación, provocará problemas cuando se intenta reinstalar la aplicación. 
3.  Selecciona la aplicación en la lista desplegable y haz clic en **Eliminar**.

### <a name="running-processes"></a>Procesos en ejecución

Esta página muestra detalles acerca de los procesos que se está ejecutando actualmente en el dispositivo host. Esto incluye aplicaciones y procesos del sistema. En algunas plataformas (escritorio, IoT y HoloLens), puedes finalizar los procesos.

![Portal de dispositivos que ejecutan procesos de página](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorador de archivos

Esta página te permite ver y manipular los archivos almacenados por las aplicaciones transferidas localmente. Consulta la entrada de blog [usando el Explorador de archivos de la aplicación](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) para obtener más información sobre el Explorador de archivos y cómo usarla. 

![Página del explorador de archivo del Portal de dispositivo](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Rendimiento

Gráficos en tiempo real de la información de diagnóstico del sistema como el uso de energía, la velocidad de fotogramas, muestra la página de rendimiento y la carga de CPU.

Estas son las métricas disponibles:
- **CPU**: porcentaje del total disponible la CPU
- **Memoria**: Total, en uso, disponible, confirmada, paginada y no paginada
- **E/S**: cantidades de datos de lectura y escritura
- **Red**: envíos y recepciones datos
- **GPU**: uso del motor de porcentaje del total GPU disponible


![Página de rendimiento de Portal de dispositivos](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Registro de seguimiento para Windows (ETW) del evento

La página de registro de ETW administra la información de seguimiento de eventos para Windows (ETW) en tiempo real en el dispositivo.

![Página de registro de ETW del Portal de dispositivos](images/device-portal/mob-device-portal-etw.png)

Activa **Ocultar proveedores** para mostrar solamente la lista de eventos.
- **Los proveedores registrados**: selecciona el proveedor de eventos y el nivel de seguimiento. El nivel de seguimiento es uno de estos valores:
  1. Terminación o salida anómala
  2. Errores graves
  3. Advertencias
  4. Advertencias sin errores
  5. Seguimiento detallado

  Haz clic o pulsa en **Activar** para iniciar el seguimiento. El proveedor se agrega a la lista desplegable de **Proveedores habilitados**.
- **Proveedores personalizados**: selecciona un proveedor ETW personalizado y el nivel de seguimiento. Identifica el proveedor por su GUID. No incluyas corchetes en el GUID.
- **Proveedores de Enabled**: enumera los proveedores habilitados. Selecciona un proveedor de la lista desplegable y haz clic o pulsa en **Desactivar** para detener el seguimiento. Haz clic o pulsa en **Detener todo** para suspender todos los seguimientos.
- **Providers history**: muestra los proveedores de ETW que estaban habilitados durante la sesión actual. Haz clic o pulsa en **Activar** para activar un proveedor deshabilitado. Haz clic o pulsa en **Borrar** para borrar el historial.
- **Filtros / eventos**: la sección de **eventos** enumeran los eventos ETW de los proveedores seleccionados en formato de tabla. La tabla se actualiza en tiempo real. Usa el menú de **filtros** para configurar los filtros personalizados para el que se mostrarán los eventos. Haz clic en el botón **Borrar** para eliminar todos los eventos ETW de la tabla. Esta acción no deshabilita ningún proveedor. Puedes hacer clic en **Guardar en archivo** para exportar los eventos ETW recopilados actualmente en un archivo CSV local.

Para obtener más información sobre el uso de registro de ETW, consulta el [Portal de dispositivos de uso para ver los registros de depuración de](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) entrada de blog. 

### <a name="performance-tracing"></a>Seguimiento de rendimiento

La página de seguimiento del rendimiento permite ver los seguimientos de [Windows Performance Recorder (WPR)](https://msdn.microsoft.com/library/hh448205.aspx) desde el dispositivo host.

![Página de seguimiento de rendimiento de Device Portal](images/device-portal/mob-device-portal-perf-tracing.png)

- **Available profiles**: selecciona el perfil de WPR en la lista desplegable y pulsa o haz clic en **Inicio** para iniciar el seguimiento.
- **Custom profiles**: haz clic o pulsa en **Examinar** para elegir un perfil de WPR desde tu equipo. Haz clic o pulsa en **Cargar e iniciar** para iniciar el seguimiento.

Para detener el seguimiento, haz clic en **Detener**. Permanece en esta página hasta que el archivo de seguimiento (. ETL) ha terminado de descargar.

Captura. Para realizar análisis en el [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/desktop/hh448170.aspx), se pueden abrir archivos ETL.

### <a name="device-manager"></a>Administrador de dispositivos

La página del Administrador de dispositivo enumera todos los periféricos conectados al dispositivo. Puedes hacer clic en los iconos de configuración para ver las propiedades de cada uno.

![Página de administrador de Device Portal de dispositivos](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Redes

La página red administra las conexiones de red en el dispositivo. A menos que estás conectado a Device Portal a través de USB, si modificas esta configuración es probable que le desconectará de Device Portal.
- **Redes disponibles**: muestra las redes Wi-Fi disponibles para el dispositivo. Al pulsar o hacer clic en una red, podrás conectarte a ella y proporcionar una clave de paso si es necesario. Device Portal aún no admite la autenticación de empresa. También puedes usar la lista desplegable de **perfiles** para intentar conectarse a cualquiera de los perfiles de Wi-Fi que se sabe que el dispositivo.
- **Configuración IP**: muestra información de dirección sobre cada uno de lo host de puertos de red del dispositivo.

![Página de redes de Portal de dispositivos](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Notas y características del servicio

### <a name="dns-sd"></a>DNS-SD

Device Portal anuncia su presencia en la red local mediante DNS-SD. Todas las instancias de Device Portal, independientemente del tipo de dispositivo, se anuncian en "WDP._wdp._tcp.local". Los registros TXT de la instancia del servicio proporcionan lo siguiente:

Tecla | Tipo | Descripción 
----|------|-------------
S | entero | Puerto seguro para Device Portal. Si es 0 (cero), Device Portal no escucha las conexiones HTTPS. 
D | cadena | Tipo de dispositivo. Tendrá el formato "Windows.*"; por ejemplo, Windows.Xbox o Windows.Desktop.
A | cadena | Arquitectura del dispositivo. Será ARM, x86 o AMD64.  
T | lista de cadenas delineada con carácter nulo | Etiquetas aplicadas por el usuario para el dispositivo. Consulta como usarlas en la API de REST de etiquetas. La lista finaliza con carácter nulo doble.  

Se sugiere la conexión en el puerto HTTPS, ya que no todos los dispositivos escuchan en el puerto HTTP anunciado por el registro de DNS-SD. 

### <a name="csrf-protection-and-scripting"></a>Protección CSRF y scripting

A fin de ofrecer protección frente a [ataques CSRF](https://wikipedia.org/wiki/Cross-site_request_forgery), se requiere un token único en todas las solicitudes no GET. Este token, el encabezado de la solicitud X-CSRF-Token, se deriva de una cookie de sesión, CSRF-Token. En la interfaz de usuario web de Device Portal, la cookie CSRF-Token se copia en el encabezado X-CSRF-Token en cada solicitud.

> [!IMPORTANT]
> Esta protección impide usar las API de REST desde un cliente independiente (por ejemplo, las herramientas de línea de comandos). Esto puede resolverse de 3 maneras: 
> - Usa un nombre de usuario "auto-". Los clientes que antepongan "auto-" a su nombre de usuario omitirán la protección CSRF. Es importante que este nombre de usuario no se use para iniciar sesión en Device Portal a través del explorador, ya que abrirá el servicio a los ataques CSRF. Ejemplo: Si el nombre de usuario de Device Portal es "admin", debe usarse ```curl -u auto-admin:password <args>``` para omitir la protección CSRF. 
> - Implementa el esquema de cookie a encabezado en el cliente. Se requiere una solicitud GET para establecer la cookie de sesión y, después, la inclusión del encabezado y la cookie en todas las solicitudes posteriores. 
> - Deshabilita la autenticación y usa HTTP. La protección CSRF solo se aplica a los extremos HTTPS, para que las conexiones en extremos HTTP no tengan que realizar las acciones anteriores. 

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protección contra Cross-Site WebSocket Hijacking (CSWSH)

Para protegerse de los [ataques de CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos los clientes que abran una conexión WebSocket al Device Portal también deben proporcionar un encabezado Origin que coincida con el encabezado Host. Esto demuestra a Device Portal que la solicitud proviene de la interfaz de usuario de Device Portal o de una aplicación cliente válida. Sin el encabezado Origin, la solicitud se rechazará. 

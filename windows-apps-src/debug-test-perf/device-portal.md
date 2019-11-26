---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Introducción a Windows Device Portal
description: Obtén información sobre cómo Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB.
ms.date: 04/09/2019
ms.topic: article
keywords: Windows 10, UWP, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254762"
---
# <a name="windows-device-portal-overview"></a>Introducción a Windows Device Portal

Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB. También proporciona herramientas de diagnóstico avanzadas para ayudarle a solucionar problemas y ver el rendimiento en tiempo real de su dispositivo Windows.

Windows Device portal es un servidor Web del dispositivo al que se puede conectar desde un explorador Web en un equipo. Si el dispositivo tiene un explorador Web, también puede conectarse localmente con el explorador en ese dispositivo.

Windows Device portal está disponible en cada familia de dispositivos, pero las características y el programa de instalación varían en función de los requisitos de cada dispositivo. En este artículo se proporciona una descripción general de Device Portal y vínculos a artículos con información más específica para cada familia de dispositivos.

La funcionalidad del portal de dispositivos de Windows se implementa con las [API de REST](device-portal-api-core.md) que puede usar directamente para acceder a los datos y controlar el dispositivo mediante programación.

## <a name="setup"></a>Instalación

Cada dispositivo tiene instrucciones específicas para la conexión a Device Portal, pero todos requieren estos pasos generales:

1. Habilite el modo de desarrollador y el portal de dispositivos en el dispositivo (configurado en la aplicación de configuración).

2. Conecte el dispositivo y el equipo a través de una red local o con USB.

3. Navega a la página Device Portal en tu explorador. En esta tabla se muestran los puertos y protocolos usados por cada familia de dispositivos.

Familia de dispositivos | ¿De forma predeterminada? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sí, en el modo de desarrollador | 80 (predeterminado) | 443 (predeterminado) | http://127.0.0.1:10080
IoT | Sí, en el modo de desarrollador | 8080 | Habilitar a través de la clave del Registro | N/D
Xbox | Habilitar dentro del modo de desarrollador | Deshabilitada | 11443 | N/D
Escritorio| Habilitar dentro del modo de desarrollador | 50080\* | 50043\* | N/D
Teléfono | Habilitar dentro del modo de desarrollador | 80| 443 | http://127.0.0.1:10080

\* este no es siempre el caso, ya que el portal de dispositivos en los puertos de notificaciones de escritorio del intervalo efímero (> 50000) para evitar colisiones con las notificaciones de Puerto existentes en el dispositivo. Para obtener más información, consulta la sección de [Configuración de puertos](device-portal-desktop.md#registry-based-configuration-for-device-portal) referente a los equipos de escritorio.  

Para obtener instrucciones específicas sobre la configuración del dispositivo, consulta:

- [Portal de dispositivos para HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portal de dispositivos para IoT](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [Portal de dispositivos para dispositivos móviles](device-portal-mobile.md)
- [Portal de dispositivos para Xbox](../xbox-apps/device-portal-xbox.md)
- [Portal de dispositivos para dispositivos de escritorio](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Características

### <a name="toolbar-and-navigation"></a>Barra de herramientas y navegación

La barra de herramientas de la parte superior de la página proporciona acceso a las características de uso frecuente.

- **Power**: acceso a las opciones de energía.
  - **Apagar**: apaga el dispositivo.
  - **Reiniciar**: vuelve a iniciar el dispositivo.
- **Ayuda**: abre la página de ayuda.

Usa los vínculos del panel de navegación en el lado izquierdo de la página para navegar a las herramientas de administración y supervisión disponibles del dispositivo.

Aquí se describen las herramientas comunes en todas las familias de dispositivos. Puede que haya otras opciones disponibles en función del dispositivo. Para obtener más información, consulte la página específica del tipo de dispositivo.

### <a name="apps-manager"></a>Administrador de aplicaciones

El administrador de aplicaciones proporciona funcionalidad de instalación, desinstalación y administración de paquetes de aplicaciones y agrupaciones en el dispositivo host.

![Página administrador de aplicaciones del portal de dispositivos](images/device-portal/WDP_AppsManager2.png)

* **Implementar aplicaciones**: implemente aplicaciones empaquetadas desde hosts locales, de red o Web y registre archivos sueltos de recursos compartidos de red.
* **Aplicaciones instaladas**: Use el menú desplegable para quitar o iniciar aplicaciones que están instaladas en el dispositivo.
* **Aplicaciones en ejecución**: Obtenga información sobre las aplicaciones que se están ejecutando actualmente y ciérrelas según sea necesario.

#### <a name="install-sideload-an-app"></a>Instalación de una aplicación (transferir localmente)

Puede transferir localmente aplicaciones durante el desarrollo mediante el portal de dispositivos de Windows:

1. Cuando haya creado un paquete de la aplicación, puede instalarlo de forma remota en el dispositivo. Después de compilarlo en Visual Studio, se genera una carpeta de salida.

    ![Instalación de aplicaciones](images/device-portal/iot-installapp0.png)

2. En el portal de dispositivos de Windows, vaya a la página **Administrador de aplicaciones** .

3. En la sección **implementar aplicaciones** , seleccione **almacenamiento local**.

4. En **Seleccione el paquete de aplicación**, seleccione **elegir archivo** y busque el paquete de la aplicación que desea transferir localmente.

5. En **Seleccionar archivo de certificado (. cer) usado para firmar el paquete de aplicación**, seleccione **elegir archivo** y busque el certificado asociado a ese paquete de la aplicación.

6. Active las casillas correspondientes si desea instalar paquetes opcionales o de Framework junto con la instalación de la aplicación y seleccione **siguiente** para elegirlos.

7. Seleccione **instalar** para iniciar la instalación.

8. Si el dispositivo ejecuta Windows 10 en modo S y es la primera vez que se ha instalado en el dispositivo el certificado especificado, reinicie el dispositivo.

#### <a name="install-a-certificate"></a>Instalar un certificado

Como alternativa, puede instalar el certificado a través del portal de dispositivos de Windows e instalar la aplicación a través de otros medios:

1. En el portal de dispositivos de Windows, vaya a la página **Administrador de aplicaciones** .

2. En la sección **implementar aplicaciones** , seleccione **instalar certificado**.

3. En **Seleccionar archivo de certificado (. cer) usado para firmar un paquete de aplicación**, seleccione **elegir archivo** y busque el certificado asociado al paquete de la aplicación que desea transferir localmente.

4. Seleccione **instalar** para iniciar la instalación.

5. Si el dispositivo ejecuta Windows 10 en modo S y es la primera vez que se ha instalado en el dispositivo el certificado especificado, reinicie el dispositivo.

#### <a name="uninstall-an-app"></a>Desinstalar una aplicación

1. Asegúrate de que la aplicación no se esté ejecutando.
2. Si es así, vaya a **aplicaciones en ejecución** y ciérrelo. Si intenta desinstalar mientras se ejecuta la aplicación, se producirán problemas al intentar volver a instalar la aplicación.
3. Seleccione la aplicación en la lista desplegable y haga clic en **quitar**.

### <a name="running-processes"></a>Procesos en ejecución

Esta página muestra detalles sobre los procesos que se ejecutan actualmente en el dispositivo host. Esto incluye aplicaciones y procesos del sistema. En algunas plataformas (escritorio, IoT y HoloLens), puede terminar los procesos.

![Página de procesos de ejecución del portal de dispositivos](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorador de archivos

Esta página permite ver y manipular los archivos almacenados por las aplicaciones de instalación de prueba. Consulte la entrada de blog [uso del explorador de archivos de la aplicación](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) para obtener más información sobre el explorador de archivos y cómo usarlo.

![Página del explorador de archivos del portal de dispositivos](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Rendimiento

La página rendimiento muestra gráficos en tiempo real de información de diagnóstico del sistema, como el uso de energía, la velocidad de fotogramas y la carga de la CPU.

Estas son las métricas disponibles:

- **CPU**: porcentaje del uso total de CPU disponible
- **Memoria**: total, en uso, disponible, confirmada, paginada y no paginada
- **E/s**: cantidades de datos de lectura y escritura
- **Red**: datos recibidos y enviados
- **GPU**: porcentaje del uso total del motor de GPU disponible

![Página de rendimiento del portal de dispositivos](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Registro de seguimiento de eventos para Windows (ETW)

La página de registro de ETW administra la información de seguimiento de eventos para Windows (ETW) en tiempo real en el dispositivo.

![Página registro de ETW del portal de dispositivos](images/device-portal/mob-device-portal-etw.png)

Activa **Ocultar proveedores** para mostrar solamente la lista de eventos.

- **Proveedores registrados**: seleccione el proveedor de eventos y el nivel de seguimiento. El nivel de seguimiento es uno de estos valores:
  1. Terminación o salida anómala
  2. Errores graves
  3. Warnings
  4. Advertencias sin errores
  5. Seguimiento detallado

  Haz clic o pulsa en **Activar** para iniciar el seguimiento. El proveedor se agrega a la lista desplegable de **Proveedores habilitados**.
- **Proveedores personalizados**: selecciona un proveedor ETW personalizado y el nivel de seguimiento. Identifica el proveedor por su GUID. No incluya corchetes en el GUID.
- **Proveedores habilitados**: se enumeran los proveedores habilitados. Selecciona un proveedor de la lista desplegable y haz clic o pulsa en **Desactivar** para detener el seguimiento. Haz clic o pulsa en **Detener todo** para suspender todos los seguimientos.
- **Historial de proveedores**: muestra los proveedores ETW que se habilitaron durante la sesión actual. Haz clic o pulsa en **Activar** para activar un proveedor deshabilitado. Haz clic o pulsa en **Borrar** para borrar el historial.
- **Filtros/eventos**: la sección **eventos** muestra los eventos ETW de los proveedores seleccionados en formato de tabla. La tabla se actualiza en tiempo real. Use el menú **filtros** para configurar los filtros personalizados para los que se mostrarán los eventos. Haga clic en el botón **Borrar** para eliminar todos los eventos ETW de la tabla. Esta acción no deshabilita ningún proveedor. Puede hacer clic en **Guardar en archivo** para exportar los eventos ETW recopilados actualmente a un archivo CSV local.

Para más información sobre el uso del registro de ETW, consulte la entrada de blog [uso del portal de dispositivos para ver registros de depuración](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) .

### <a name="performance-tracing"></a>Seguimiento del rendimiento

La página seguimiento del rendimiento le permite ver los seguimientos de la [grabadora de rendimiento de Windows (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) del dispositivo host.

![Página de seguimiento de rendimiento del portal de dispositivos](images/device-portal/mob-device-portal-perf-tracing.png)

- **Available profiles**: selecciona el perfil de WPR en la lista desplegable y pulsa o haz clic en **Inicio** para iniciar el seguimiento.
- **Custom profiles**: haz clic o pulsa en **Examinar** para elegir un perfil de WPR desde tu equipo. Haz clic o pulsa en **Cargar e iniciar** para iniciar el seguimiento.

Para detener el seguimiento, haz clic en **Detener**. Permanezca en esta página hasta el archivo de seguimiento (. ETL) ha terminado de descargarse.

Obtuvo. Los archivos ETL se pueden abrir para su análisis en el [analizador de rendimiento de Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Administrador de dispositivos

En la página Administrador de dispositivos se enumeran todos los periféricos conectados al dispositivo. Puede hacer clic en los iconos de configuración para ver las propiedades de cada uno.

![Página administrador de dispositivos del portal de dispositivos](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Funciones de red

La página redes administra las conexiones de red en el dispositivo. A menos que esté conectado al portal de dispositivos a través de USB, es probable que el cambio de esta configuración se desconecte del portal de dispositivos.

- **Redes disponibles**: muestra las redes Wi-Fi disponibles para el dispositivo. Al pulsar o hacer clic en una red, podrás conectarte a ella y proporcionar una clave de paso si es necesario. El portal de dispositivos todavía no admite la autenticación empresarial. También puede usar la lista desplegable **perfiles** para intentar conectarse a cualquiera de los perfiles de Wi-Fi que conoce el dispositivo.
- **Configuración de IP**: muestra información de dirección acerca de cada uno de los puertos de red del dispositivo host.

![Página de redes del portal de dispositivos](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Características y notas del servicio

### <a name="dns-sd"></a>DNS-SD

Device Portal anuncia su presencia en la red local mediante DNS-SD. Todas las instancias de Device Portal, independientemente del tipo de dispositivo, se anuncian en "WDP._wdp._tcp.local". Los registros TXT de la instancia del servicio proporcionan lo siguiente:

Tecla | Tipo | Descripción
----|------|-------------
S | entero | Puerto seguro para Device Portal. Si es 0 (cero), Device Portal no escucha las conexiones HTTPS.
D | string | Tipo de dispositivo. Tendrá el formato "Windows. *", por ejemplo, Windows. Xbox o Windows. Desktop
A | string | Arquitectura del dispositivo. Será ARM, x86 o AMD64.  
T | lista de cadenas delineada con carácter nulo | Etiquetas aplicadas por el usuario para el dispositivo. Consulta como usarlas en la API de REST de etiquetas. La lista finaliza con carácter nulo doble.  

Se sugiere la conexión en el puerto HTTPS, ya que no todos los dispositivos escuchan en el puerto HTTP anunciado por el registro de DNS-SD.

### <a name="csrf-protection-and-scripting"></a>Protección CSRF y scripting

A fin de ofrecer protección frente a [ataques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), se requiere un token único en todas las solicitudes no GET. Este token, el encabezado de la solicitud X-CSRF-Token, se deriva de una cookie de sesión, CSRF-Token. En la interfaz de usuario web de Device Portal, la cookie CSRF-Token se copia en el encabezado X-CSRF-Token en cada solicitud.

> [!IMPORTANT]
> Esta protección evita los usos de las API de REST desde un cliente independiente (por ejemplo, utilidades de línea de comandos). Esto puede resolverse de 3 maneras:
> - Use un nombre de usuario "automático". Los clientes que antepongan "auto-" a su nombre de usuario omitirán la protección CSRF. Es importante que este nombre de usuario no se use para iniciar sesión en Device Portal a través del explorador, ya que abrirá el servicio a los ataques CSRF. Ejemplo: Si el nombre de usuario de Device Portal es "admin", debe usarse ```curl -u auto-admin:password <args>``` para omitir la protección CSRF.
> - Implementa el esquema de cookie a encabezado en el cliente. Se requiere una solicitud GET para establecer la cookie de sesión y, después, la inclusión del encabezado y la cookie en todas las solicitudes posteriores.
> - Deshabilita la autenticación y usa HTTP. La protección CSRF solo se aplica a los extremos HTTPS, para que las conexiones en extremos HTTP no tengan que realizar las acciones anteriores.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protección contra Cross-Site WebSocket Hijacking (CSWSH)

Para protegerse de los [ataques de CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos los clientes que abran una conexión WebSocket al Device Portal también deben proporcionar un encabezado Origin que coincida con el encabezado Host. Esto demuestra a Device Portal que la solicitud proviene de la interfaz de usuario de Device Portal o de una aplicación cliente válida. Sin el encabezado Origin, la solicitud se rechazará.

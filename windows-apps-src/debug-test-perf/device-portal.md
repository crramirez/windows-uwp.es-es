---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Introducción a Windows Device Portal
description: Obtén información sobre cómo Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB.
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: a4fc5cc5b8bc99e830d3c31604e581f8e57c1007
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173639"
---
# <a name="windows-device-portal-overview"></a>Introducción a Windows Device Portal

Windows Device Portal te permite configurar y administrar de forma remota el dispositivo mediante una red o una conexión USB. También proporciona herramientas de diagnóstico para ayudarte a solucionar problemas y ver el rendimiento en tiempo real de tu dispositivo Windows.

Portal de dispositivos Windows es un servidor web del dispositivo al que te puedes conectar desde un explorador web en el equipo. Si el dispositivo tiene un explorador web, también puedes conectarte localmente con el explorador en el dispositivo.

Portal de dispositivos Windows está disponible en cada familia de dispositivos, pero las características y la configuración varían en función de los requisitos de cada dispositivo. En este artículo se proporciona una descripción general de Device Portal y vínculos a artículos con información más específica para cada familia de dispositivos.

La funcionalidad de Windows Device Portal se implementa con [API REST](device-portal-api-core.md) que puedes usar directamente para acceder a los datos y controlar el dispositivo mediante programación.

## <a name="setup"></a>Instalación

Cada dispositivo tiene instrucciones específicas para la conexión a Device Portal, pero todos requieren estos pasos generales:

1. Habilita el modo de desarrollador y Device Portal en el dispositivo (establecido en la aplicación de configuración).

2. Conecta el dispositivo y el equipo a través de la red local o con USB.

3. Navega a la página Device Portal en tu explorador. En esta tabla se muestran los puertos y los protocolos que usa cada familia de dispositivos.

Familia de dispositivos | ¿De forma predeterminada? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sí, en el modo de desarrollador | 80 (predeterminado) | 443 (predeterminado) | http://127.0.0.1:10080
IoT | Sí, en el modo de desarrollador | 8080 | Habilitar a través de la clave del Registro | N/A
Xbox | Habilitar dentro del modo de desarrollador | Deshabilitada | 11443 | N/A
Escritorio| Habilitar dentro del modo de desarrollador | 50080\* | 50043\* | N/A
Teléfono | Habilitar dentro del modo de desarrollador | 80| 443 | http://127.0.0.1:10080

\* No siempre es el caso, ya que Device Portal para escritorio reclama puertos del rango efímero (> 50 000) para evitar conflictos con reclamaciones de puertos existentes en el dispositivo. Para obtener más información, consulta la sección de [Configuración de puertos](device-portal-desktop.md#registry-based-configuration-for-device-portal) referente a los equipos de escritorio.  

Para obtener instrucciones específicas sobre la configuración del dispositivo, consulta:

- [Portal de dispositivos para HoloLens](./device-portal-hololens.md)
- [Portal de dispositivos para IoT](/windows/iot-core/manage-your-device/DevicePortal)
- [Portal de dispositivos para dispositivos móviles](device-portal-mobile.md)
- [Portal de dispositivos para Xbox](../xbox-apps/device-portal-xbox.md)
- [Portal de dispositivos para dispositivos de escritorio](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Características

### <a name="toolbar-and-navigation"></a>Barra de herramientas y navegación

La barra de herramientas de la parte superior de la página proporciona acceso a las características usadas frecuentemente.

- **Energía**: acceso alas opciones de energía.
  - **Apagar**: apaga el dispositivo.
  - **Reiniciar**: apaga y vuelve a encender el dispositivo.
- **Ayuda**: abre la página de ayuda.

Usa los vínculos del panel de navegación en el lado izquierdo de la página para navegar a las herramientas de administración y supervisión disponibles del dispositivo.

Aquí se describen las herramientas que son comunes en todas las familias de dispositivos. Puede que haya otras opciones disponibles en función del dispositivo. Para obtener más información, consulta la página específica del tipo de dispositivo.

### <a name="apps-manager"></a>Administrador de aplicaciones

El Administrador de aplicaciones proporciona la funcionalidad de administración y de instalación o desinstalación de paquetes de aplicaciones y agrupaciones en el dispositivo host.

![Página Administrador de aplicaciones del Portal de dispositivos](images/device-portal/WDP_AppsManager2.png)

* **Implementar aplicaciones**: implementa aplicaciones empaquetadas desde hosts locales, de red o web y registra archivos dinámicos desde recursos compartidos de red.
* **Aplicaciones instaladas**: usa el menú desplegable para quitar o iniciar aplicaciones que están instaladas en el dispositivo.
* **Aplicaciones en ejecución**: obtén información sobre las aplicaciones que se están ejecutando actualmente y ciérralas según sea necesario.

#### <a name="install-sideload-an-app"></a>Instalación (transferencia local) de una aplicación

Puedes transferir localmente aplicaciones durante el desarrollo mediante el Portal de dispositivos Windows:

1. cuando hayas creado un paquete de la aplicación, podrás instalarlo remotamente en el dispositivo. Después de compilarlo en Visual Studio, se genera una carpeta de salida.

    ![Instalación de aplicaciones](images/device-portal/iot-installapp0.png)

2. En el Portal de dispositivos Windows, ve a la página del **Administrador de aplicaciones**.

3. En la sección **Implementar aplicaciones**, selecciona **Almacenamiento local**.

4. En la sección para **seleccionar el paquete de aplicación**, selecciona **Elegir archivo** y busca el paquete de la aplicación que desea transferir localmente.

5. En la sección para **seleccionar un archivo de certificado (.cer) para firmar el paquete de aplicación**, selecciona **Elegir archivo** y busca el certificado asociado a ese paquete de aplicación.

6. Activa las casillas correspondientes si deseas instalar paquetes opcionales o de marcos junto con la instalación de la aplicación y selecciona **Siguiente** para elegirlos.

7. Selecciona **Instalar** para iniciar la instalación.

8. Si el dispositivo ejecuta Windows 10 en modo S y es la primera vez que se has instalado ese certificado en el dispositivo, reinícialo.

#### <a name="install-a-certificate"></a>Instalación de un certificado

Como alternativa, puedes instalar el certificado a través del Portal de dispositivos Windows e instalar la aplicación a través de otros medios:

1. En el Portal de dispositivos Windows, ve a la página del **Administrador de aplicaciones**.

2. En la sección **Implementar aplicaciones**, selecciona **Instalar certificado**.

3. En la sección para **seleccionar un archivo de certificado (.cer) para firmar el paquete de aplicación**, selecciona **Elegir archivo** y busca el certificado asociado al paquete de la aplicación que quieres transferir localmente.

4. Selecciona **Instalar** para iniciar la instalación.

5. Si el dispositivo ejecuta Windows 10 en modo S y es la primera vez que se has instalado ese certificado en el dispositivo, reinícialo.

#### <a name="uninstall-an-app"></a>Desinstalar una aplicación

1. Asegúrate de que la aplicación no se esté ejecutando.
2. Si es así, ve a **Aplicaciones en ejecución** y ciérrala. Si intentas desinstalar la aplicación mientras se ejecuta, tendrás problemas cuando la intentes instalar de nuevo.
3. Selecciona la aplicación en la lista desplegable y haz clic en **Quitar**.

### <a name="running-processes"></a>Procesos en ejecución

Esta página muestra detalles sobre los procesos que se ejecutan actualmente en el dispositivo host. Esto incluye aplicaciones y procesos del sistema. En algunas plataformas (escritorio, IoT y HoloLens), puedes finalizar los procesos.

![Página de procesos en ejecución del Portal de dispositivos](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorador de archivos

Esta página te permite ver y manipular los archivos almacenados por las aplicaciones transferidas localmente. Consulta la entrada de blog [Uso del explorador de archivos de la aplicación](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) para obtener más información sobre el explorador de archivos y cómo usarlo.

![Página del explorador de archivos del Portal de dispositivos](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Rendimiento

La página Rendimiento muestra gráficos en tiempo real de la información de diagnóstico del sistema, como el uso de energía, la velocidad de fotogramas y la carga de la CPU.

Estas son las métricas disponibles:

- **CPU**: porcentaje de la utilización de la CPU disponible total
- **Memoria:** total, en uso, disponible, confirmada, paginada y no paginada
- **E/S**: cantidades de datos de lectura y escritura
- **Red**: datos enviados y recibidos
- **GPU**: porcentaje de uso del motor de GPU disponible total

![Página Rendimiento del Portal de dispositivos](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Registro de seguimiento de eventos para Windows (ETW)

La página de registro de ETW administra la información de seguimiento de eventos para Windows (ETW) en tiempo real en el dispositivo.

![Página de registro de ETW del Portal de dispositivos](images/device-portal/mob-device-portal-etw.png)

Activa **Ocultar proveedores** para mostrar solamente la lista de eventos.

- **Registered providers** (Proveedores registrados): selecciona el proveedor de eventos y el nivel de seguimiento. El nivel de seguimiento es uno de estos valores:
  1. Terminación o salida anómala
  2. Errores graves
  3. Warnings
  4. Advertencias sin errores
  5. Seguimiento detallado

  Haz clic o pulsa en **Activar** para iniciar el seguimiento. El proveedor se agrega a la lista desplegable de **Proveedores habilitados**.
- **Proveedores personalizados**: selecciona el proveedor ETW personalizado y el nivel de seguimiento. Identifica el proveedor por su GUID. No incluyas corchetes en el GUID.
- **Proveedores habilitados**: esto enumera los proveedores habilitados. Selecciona un proveedor de la lista desplegable y haz clic o pulsa en **Desactivar** para detener el seguimiento. Haz clic o pulsa en **Detener todo** para suspender todos los seguimientos.
- **Providers history** (Historial de proveedores): esto muestra los proveedores ETW habilitados durante la sesión actual. Haz clic o pulsa en **Activar** para activar un proveedor deshabilitado. Haz clic o pulsa en **Borrar** para borrar el historial.
- **Filtros / Eventos**: la sección **Eventos** enumera los eventos ETW de los proveedores seleccionados en formato de tabla. Esta tabla se actualiza en tiempo real. Usa el menú **Filtros** para configurar filtros personalizados para los que se mostrarán los eventos. Haz clic en el botón **Borrar** para eliminar todos los eventos ETW de la tabla. Esta acción no deshabilita ningún proveedor. Puedes hacer clic en **Guardar en archivo** para exportar los eventos ETW recopilados actualmente en un archivo CSV local.

Para más información sobre el uso del registro de ETW, consulta la entrada de blog [Uso del Portal de dispositivos para ver los registros de depuración](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/).

### <a name="performance-tracing"></a>Seguimiento del rendimiento

La página Seguimiento del rendimiento te permite ver los seguimientos de [Windows Performance Recorder (WPR)](/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) del dispositivo host.

![Página Seguimiento del rendimiento del Portal de dispositivos](images/device-portal/mob-device-portal-perf-tracing.png)

- **Perfiles disponibles**: selecciona el perfil de WPR en la lista desplegable y pulsa o haz clic en **Inicio** para iniciar el seguimiento.
- **Perfiles personalizados**: haz clic o pulsa en **Examinar** para elegir un perfil de WPR de tu equipo. Haz clic o pulsa en **Cargar e iniciar** para iniciar el seguimiento.

Para detener el seguimiento, haz clic en **Detener**. Permanece en esta página hasta que el archivo de seguimiento (. ETL) se haya terminado de descargar.

Los archivos ETL capturados se pueden abrir para su análisis en [Windows Performance Analyzer](/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Administrador de dispositivos

La página Administrador de dispositivos enumera todos los periféricos conectados al dispositivo. Puedes hacer clic en los iconos de configuración para ver las propiedades de cada uno.

![Página Administrador de dispositivos del Portal de dispositivos](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Funciones de red

La página Funciones de red administra las conexiones de red en el dispositivo. A menos que estés conectado a Device Portal a través de USB, al cambiar esta configuración, es probable que te desconectes de Device Portal.

- **Redes disponibles**: muestra las redes Wi-Fi disponibles para el dispositivo. Al pulsar o hacer clic en una red, podrás conectarte a ella y proporcionar una clave de paso si es necesario. el Portal de dispositivos aún no admite la autenticación empresarial. También puedes usar la lista desplegable **Perfiles** para intentar conectarte a cualquiera de los perfiles Wi-Fi que conoce el dispositivo.
- **Configuración IP**: muestra la información de dirección acerca de cada uno de los puertos de red del dispositivo host.

![Página de redes del Portal de dispositivos](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Notas y características del servicio

### <a name="dns-sd"></a>DNS-SD

Device Portal anuncia su presencia en la red local mediante DNS-SD. Todas las instancias de Device Portal, independientemente del tipo de dispositivo, se anuncian en "WDP._wdp._tcp.local". Los registros TXT de la instancia del servicio proporcionan lo siguiente:

Clave | Tipo | Descripción
----|------|-------------
S | int | Puerto seguro para Device Portal. Si es 0 (cero), Device Portal no escucha las conexiones HTTPS.
D | cadena | Tipo de dispositivo. Tendrá el formato "Windows.*"; por ejemplo, Windows.Xbox o Windows.Desktop.
A | cadena | Arquitectura del dispositivo. Será ARM, x86 o AMD64.  
T | lista de cadenas delineada con carácter nulo | Etiquetas aplicadas por el usuario para el dispositivo. Consulta como usarlas en la API de REST de etiquetas. La lista finaliza con carácter nulo doble.  

Se sugiere la conexión en el puerto HTTPS, ya que no todos los dispositivos escuchan en el puerto HTTP anunciado por el registro de DNS-SD.

### <a name="csrf-protection-and-scripting"></a>Protección CSRF y scripting

A fin de ofrecer protección frente a [ataques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), se requiere un token único en todas las solicitudes no GET. Este token, el encabezado de la solicitud X-CSRF-Token, se deriva de una cookie de sesión, CSRF-Token. En la interfaz de usuario web de Device Portal, la cookie CSRF-Token se copia en el encabezado X-CSRF-Token en cada solicitud.

> [!IMPORTANT]
> Esta protección impide usar las API REST desde un cliente independiente (por ejemplo, las utilidades de línea de comandos). Esto puede resolverse de 3 maneras:
> - usa un nombre de usuario con "auto" delante. Los clientes que antepongan "auto-" a su nombre de usuario omitirán la protección CSRF. Es importante que este nombre de usuario no se use para iniciar sesión en Device Portal a través del explorador, ya que abrirá el servicio a los ataques CSRF. Ejemplo: Si el nombre de usuario del Portal de dispositivos es "admin", debe usarse ```curl -u auto-admin:password <args>``` para omitir la protección CSRF.
> - Implementa el esquema de cookie a encabezado en el cliente. Se requiere una solicitud GET para establecer la cookie de sesión y, después, la inclusión del encabezado y la cookie en todas las solicitudes posteriores.
> - Deshabilita la autenticación y usa HTTP. La protección CSRF solo se aplica a los extremos HTTPS, para que las conexiones en extremos HTTP no tengan que realizar las acciones anteriores.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protección contra Cross-Site WebSocket Hijacking (CSWSH)

Para protegerse de los [ataques de CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos los clientes que abran una conexión WebSocket al Device Portal también deben proporcionar un encabezado Origin que coincida con el encabezado Host. Esto demuestra a Device Portal que la solicitud proviene de la interfaz de usuario de Device Portal o de una aplicación cliente válida. Sin el encabezado Origin, la solicitud se rechazará.
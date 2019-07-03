---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal para Xbox
description: Aprende a habilitar Device Portal para Xbox One.
ms.date: 4/9/2019
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: c701dc7eef4d37f28db9f9cabd7bbb0f39a92a4c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322085"
---
# <a name="device-portal-for-xbox"></a>Device Portal para Xbox

## <a name="set-up-device-portal-on-xbox"></a>Configurar Device Portal en Xbox

Los siguientes pasos muestran cómo habilitar el Portal de dispositivos para Xbox, lo que proporciona acceso remoto a Xbox para desarrollo.

1. Abre Dev Home. Esto debería abrirse de manera predeterminada al arrancar Xbox para desarrollo, pero también puede abrir desde la pantalla principal.

    ![Página principal para desarrolladores de Device Portal](images/device-portal-xbox-1.png)

2. En Dev Home, en la pestaña **Inicio**, en **Acceso remoto**, selecciona **Configuración de acceso remoto**.

    ![Herramienta Administración remota del Portal de dispositivos](images/device-portal-xbox-15.png)

3. Selecciona la opción **Habilitar Portal de dispositivos para Xbox**.

4. En **Autenticación**, selecciona **Establecer nombre de usuario y contraseña**. Escribe el **nombre de usuario** y la **contraseña** que usarás para autenticar el acceso a tu kit de desarrollo desde un navegador y haz clic en **Guardar** para guardarlos.

5. **Cierra** la página **Acceso remoto** y anota la dirección URL aparece debajo de **Acceso remoto** en la pestaña **Inicio**.

6. Escribe la dirección URL en el explorador y, a continuación, inicia sesión con las credenciales que has configurado.

7. Recibirás una advertencia acerca del certificado proporcionado, similar a la que aparece a continuación. En Edge, haz clic en **Detalles** y después en **Acceder a la página web** para acceder al Portal de dispositivos para Xbox. En el cuadro de diálogo que aparecerá, escribe el nombre de usuario y la contraseña que hayas especificado anteriormente en tu Xbox.

    ![Error de certificado de Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Páginas de Device Portal

El Portal de dispositivos para Xbox proporciona un conjunto de páginas estándar similares a las que están disponibles en el Portal de dispositivos Windows, así como varias páginas exclusivas. Para obtener descripciones detalladas de las primeras, consulta [Introducción al Portal de dispositivos Windows](../debug-test-perf/device-portal.md). Las siguientes secciones describen las páginas que son exclusivas para el Portal de dispositivos para Xbox.

### <a name="home"></a>Inicio

Similar a la página **Administrador de aplicaciones** del Portal de dispositivos Windows, la página **Inicio** del Portal de dispositivos para Xbox muestra una lista de juegos y aplicaciones instalados en **Mis juegos y aplicaciones**. Puedes hacer clic en el nombre de un juego o una aplicación para ver más detalles sobre este, como el **Nombre de familia de paquete**. En la lista desplegable **Acciones**, puedes realizar acciones respecto al juego o la aplicación, como **Iniciar**.

En **Cuentas de prueba de Xbox Live**, puedes administrar las cuentas asociadas con tu Xbox. Puedes agregar usuarios y cuentas de invitado, crear nuevos usuarios, iniciar y cerrar sesiones de usuarios y quitar cuentas.

![Inicio](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (partidas guardadas)

Tanto el Portal de dispositivos Windows como el Portal de dispositivos para Xbox tienen una página **Xbox Live**. Sin embargo, el Portal de dispositivos para Xbox tiene una sección exclusiva, **Partidas guardadas de Xbox Live**, donde puedes guardar datos de juegos instalados en tu Xbox. Escribe el **Id. de configuración de servicio (SCID)** (consulta [Configuración del servicio de Xbox Live](https://docs.microsoft.com/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids) para obtener más información), el **Nombre del miembro (MSA)** y el **Nombre de familia de paquete (PFN)** asociados con el título y la partida guardar, busca el **Archivo de entrada (.json o .xml)** y a continuación selecciona uno de los botones (**Restablecer**, **Importar**, **Exportar** y **Eliminar**) para manipular los datos guardados.

En la sección **Generar**, puedes generar datos ficticios y guardar en el archivo de entrada especificado. Solo tienes que escribir los **Contenedores (valor predeterminado 2)** , **Blobs (valor predeterminado 3)** y **Tamaño del blob (valor predeterminado 1024)** y seleccionar **Generar**.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>Supervisor de HTTP

El supervisor de HTTP te permite ver el tráfico HTTP y HTTPS descifrado desde la aplicación o el juego cuando estos se ejecutan en Xbox One.

![Supervisor de HTTP](images/device-portal-xbox-18.png)

Para habilitarlo, abre Dev Home en tu Xbox One, ve a la pestaña **Configuración** y en la casilla **Configuración del supervisor de HTTP**, selecciona **Activar el supervisor de HTTP**.

![Página principal de desarrollo: Funciones de red](images/device-portal-xbox-14.png)

Una vez habilitado, en el Portal de dispositivos de Xbox, puedes **Detener**, **Borrar** y **Guardar en archivo** el tráfico HTTP y HTTPS seleccionando los botones respectivos.

### <a name="network-fiddler-tracing"></a>Red (seguimiento de Fiddler)

La página **Red** del Portal de dispositivos para Xbox es casi idéntica a la Página **Redes** del Portal de dispositivos Windows, con excepción de **Seguimiento de Fiddler**, que es exclusivo del Portal de dispositivos para Xbox. Esto te permite ejecutar Fiddler en el equipo para registrar e inspeccionar el tráfico HTTP y HTTPS entre tu Xbox One e Internet. Consulta [Cómo usar Fiddler con Xbox One al desarrollar para la UWP](../xbox-apps/uwp-fiddler.md) para obtener más información.

![Red](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Captura multimedia

En la página **Captura multimedia**, puedes seleccionar **Capturar pantalla** para realizar capturas de pantalla de la Xbox One. Cuando lo hagas, el navegador te preguntará cómo quieres descargar el archivo. Puedes marcar **Preferir HDR** si quieres realizar la captura de pantalla en HDR (si la consola lo admite).

![Captura multimedia](images/device-portal-xbox-12.png)

### <a name="settings"></a>Configuración

En la página **Configuración**, puedes ver y modificar varios ajustes de la Xbox One. En la parte superior, puedes seleccionar **Importar** para importar la configuración de un archivo y **Exportar** para exportar la configuración actual a un archivo .txt. La importación de la configuración puede facilitar realizar una edición masiva, en especial si se configuran varias consolas. Para crear un archivo de configuración para importar, cambia los ajustes como quieres que estén y luego exporta la configuración. Luego puedes usar este archivo para importar la configuración rápida y fácilmente a otras consolas.

Hay varias secciones con diferentes ajustes para ver o editar, que se explican a continuación.

![Configuración](images/device-portal-xbox-20.png)

![Configuración](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Información del dispositivo

* **Nombre de dispositivo**: El nombre del dispositivo. Para editarlo, cambia el nombre en el cuadro y selecciona **Guardar**.

* **Versión del sistema operativo**: Es de sólo lectura. El número de versión del sistema operativo.

* **Edición del SO**: Es de sólo lectura. El nombre de la versión principal del sistema operativo.

* **Id. de dispositivo Xbox Live**: Es de sólo lectura.

* **Id. de consola**: Es de sólo lectura.

* **Número de serie**: Es de sólo lectura.

* **Tipo de consola**: Es de sólo lectura. El tipo de dispositivo de Xbox One (Xbox One, Xbox One S o Xbox One X).

* **Modo de desarrollo**: Es de sólo lectura. El modo de desarrollador en el que está el dispositivo.

#### <a name="audio-settings"></a>Configuración de audio

* **Formato de secuencia de bits de audio**: El formato de los datos de audio.

* **Audio HDMI**: El tipo de audio a través del puerto HDMI.

* **Formato de auriculares**: El formato de audio que circulan a través de auriculares.

* **Audio óptico**: El tipo de audio a través del puerto óptico.

* **Usar HDMI o auriculares de audio ópticos**: Active esta casilla si está utilizando un auricular conectado a través de HDMI o medios óptico.

#### <a name="display-settings"></a>Configuración de pantalla

* **Profundidad de color**: El número de bits usados para cada componente de color de un solo píxel.

* **Espacio de colores**: La gama de colores disponible para la presentación.

* **Resolución de pantalla**: La resolución de la pantalla.

* **Mostrar conexiones**: El tipo de conexión a la pantalla.

* **Permitir alto rango dinámico (HDR)** : Habilita HDR en la pantalla. Solo está disponible para pantallas compatibles.

* **Permitir 4K**: Habilita la resolución de 4K en la pantalla. Solo está disponible para pantallas compatibles.

* **Permitir que la frecuencia de actualización de Variable (VRR)** : Habilitar VRR en la pantalla. Solo está disponible para pantallas compatibles.

#### <a name="kinect-settings"></a>Configuración de Kinect

Para cambiar estos ajustes, es necesario que haya un sensor Kinect conectado a la consola.

* **Habilitar Kinect**: Habilitar el sensor Kinect adjunto.

* **Forzar Kinect volver a cargar en el cambio de la aplicación**: Cada vez que se ejecuta una aplicación diferente o un juego, vuelva a cargar el sensor Kinect adjunto.

#### <a name="localization-settings"></a>Configuración de ubicación

* **Región geográfica**: La región geográfica que el dispositivo se establece en. Debe ser el código de país específico de 2 caracteres (por ejemplo, **US** para Estados Unidos).

* **Preferido idiomas**: El idioma que el dispositivo se establece en.

* **Zona horaria**: La zona horaria que el dispositivo se establece en.

#### <a name="network-settings"></a>Configuración de red

* **Configuración de radio inalámbrica**: La configuración inalámbrica del dispositivo (si determinados aspectos, como redes LAN inalámbricas son activado o desactivado).

#### <a name="power-settings"></a>Configuración de energía

* **Cuando está inactivo, dim pantalla tras (minutos)** : Después de que el dispositivo ha estado inactivo durante este período de tiempo, se atenúa la pantalla. Establece el ajuste en **0** para no atenuar nunca la pantalla.

* **Cuando está inactivo, desactivar después**: El dispositivo se apagará después de que ha estado inactiva durante este período de tiempo.

* **Modo de energía**: El modo de alimentación del dispositivo. Consulta [Acerca de los modos de energía Inicio inmediato y de ahorro de energía](https://support.xbox.com/xbox-one/console/learn-about-power-modes) para obtener más información.

* **Consola de arranque automáticamente cuando se conecta a la potencia**: El dispositivo se enciende automáticamente cuando está conectado a una fuente de alimentación.

#### <a name="preference-settings"></a>Configuración de preferencias

* **Default experiencia doméstica**: Establece qué pantalla principal aparece cuando el dispositivo está activado.

* **Permitir conexiones desde la aplicación Xbox**: La aplicación Xbox en otro dispositivo (por ejemplo, un equipo con Windows 10) puede conectarse a esta consola.

* **Tratar las aplicaciones para UWP como juegos predeterminada**: Juegos y aplicaciones obtención diferentes recursos asignados a ellos en Xbox. Si seleccionas esta casilla, todos los paquetes para UWP se identifican como juegos y, por tanto, obtienen más recursos.

#### <a name="user-settings"></a>Configuración de usuario

* **Auto de usuario de inicio de sesión**: Iniciará sesión automáticamente en el usuario seleccionado cuando el dispositivo está activado.

* **Auto de controlador de usuario de inicio de sesión**: Un tipo de controlador determinado se asocia automáticamente con un usuario determinado.

#### <a name="xbox-live-sandbox"></a>Espacio aislado de Xbox Live

Aquí puedes cambiar el espacio aislado de Xbox Live en el que está el dispositivo. Escribe el nombre del espacio aislado en el cuadro y selecciona **Cambiar**.

### <a name="scratch"></a>Borrador

Se trata de un área de trabajo en blanco que puedes personalizar a tu gusto. Puedes usar el menú (haz clic en el botón Menú en la parte superior izquierda) para agregar herramientas (selecciona **Agregar herramientas al área de trabajo**, luego las herramientas que quieras agregar y finalmente **Agregar**). Ten en cuenta que puedes usar este menú para agregar herramientas a cualquier área de trabajo, así como para administrar las propias áreas de trabajo.

![Agregar herramientas al área de trabajo](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Datos de eventos de juego

En el **datos de eventos de juegos** página, puede ver un gráfico en tiempo real que se transmite por secuencias en el número de eventos de seguimiento de eventos para Windows (ETW) juegos grabada actualmente en Xbox One. Si hay eventos de juegos se registran en el sistema, también puede ver los detalles (nombre del evento, repetición del evento y el título del juego) que describe cada evento en la tabla debajo del gráfico de datos. La tabla solo está disponible si hay eventos registrados.

![Datos de eventos de juego](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Vea también

* [Información general de Windows Device Portal](../debug-test-perf/device-portal.md)
* [Núcleo de Portal de dispositivo referencia de API](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal para HoloLens
description: Obtén información sobre cómo Windows Device Portal para HoloLens te permite configurar y administrar de forma remota tu dispositivo HoloLens.
ms.date: 01/03/2019
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 92f4cc44f93348e1dba0f45b780e2efdda6949a7
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713884"
---
# <a name="device-portal-for-hololens"></a>Device Portal para HoloLens


## <a name="set-up-device-portal-on-hololens"></a>Configurar Device Portal en HoloLens

### <a name="enable-device-portal"></a>Habilitar Device Portal

1. Enciende HoloLens y colócalo en el dispositivo.
2. Realiza el gesto [nástico](https://developer.microsoft.com/mixed-reality#Bloom) para iniciar el menú principal.
3. Mira el icono **Configuración** y realiza el gesto de [toque en el aire](https://developer.microsoft.com/mixed-reality#Press_and_release). Realiza un segundo toque en el aire para colocar la aplicación en tu entorno. La aplicación Configuración se iniciará una vez la hayas colocado.
4. Selecciona el elemento de menú **Actualizar**.
5. Selecciona el elemento de menú **Para desarrolladores**.
6. Habilita el **Modo de desarrollador**.
7. [Desplázate hacia abajo](https://developer.microsoft.com/mixed-reality#Navigation) y habilita Device Portal.


### <a name="pair-your-device"></a>Emparejar el dispositivo

#### <a name="connect-over-wi-fi"></a>Conectarse a través de Wi-Fi 

1. Conecta HoloLens a una red Wi-Fi.
2. Buscar la dirección IP de su dispositivo. Buscar la dirección IP del dispositivo en **configuración > red e Internet > Wi-Fi > Propiedades de Hardware**. También puede decir: "Hola Cortana, ¿cuál es mi dirección IP?"

3. Desde un explorador web de tu equipo, ve a `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - El explorador mostrará el mensaje siguiente: "Hay un problema con el certificado de seguridad del sitio Web". Esto ocurre porque el certificado emitido en Device Portal es un certificado de prueba. Puedes ignorar este error de certificado por ahora y continuar.

#### <a name="connect-over-usb"></a>Conectarse a través de USB 

1. Instala las herramientas para asegurarte de que tienes Visual Studio Update 1 con las Herramientas de desarrollo de Windows 10 instaladas en el equipo. Esto permite la conectividad USB.
2. Conecta HoloLens en el equipo con un cable micro-USB.
3. Desde un explorador web de tu equipo, ve a `http://127.0.0.1:10080`.

> [!IMPORTANT]
> Si no se encuentra el dispositivo de su PC, pruebe con la dirección IP de red real del dispositivo HoloLens, en lugar de `http://127.0.0.1:10080`.

#### <a name="connect-to-an-emulator"></a>Conectarse a un emulador 

También puedes usar Device Portal con el emulador. Para conectarte a Device Portal, usa la barra de herramientas. Haz clic en este icono:
- Abra el Portal de dispositivos: abre el Portal de dispositivos Windows correspondiente al sistema operativo de HoloLens en el emulador.

#### <a name="create-a-username-and-password"></a>Crear un nombre de usuario y una contraseña. 

La primera vez que te conectes a Device Portal en HoloLens, debes crear un nombre de usuario y una contraseña.
1. En un explorador web de tu equipo, escribe la dirección IP de HoloLens. Se abrirá la página de acceso Configuración.
2. Haz clic o pulsa Solicitar PIN y mira la pantalla de HoloLens para obtener el PIN generado.
3. Escribe el PIN en el PIN que se muestra en el cuadro de texto del dispositivo.
4. Escribe el nombre de usuario que usarás para conectarte a Device Portal. No es necesario que sea un nombre de cuenta de Microsoft (MSA) ni un nombre de dominio.
5. Escribe una contraseña y confírmala. La contraseña debe tener al menos siete caracteres de longitud. No es necesario que sea una MSA ni una contraseña de dominio.
6. Haz clic a Emparejar para conectarte a Windows Device Portal en HoloLens.

Si quieres cambiar este nombre de usuario o contraseña en cualquier momento, puedes repetir este proceso. Para ello, visita la página de seguridad del dispositivo haciendo clic en el vínculo de seguridad a lo largo de la parte superior derecha, o bien navega a: `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`.

#### <a name="security-certificate"></a>Certificado de seguridad 

Si ves un "error de certificado" en el explorador, puedes corregirlo mediante la creación de una relación de confianza con el dispositivo.

Cada HoloLens genera un certificado autofirmado único para su conexión de SSL. De manera predeterminada, este certificado no es de confianza por parte del explorador web de tu equipo y puede que recibas un "error de certificado". Al descargar este certificado desde HoloLens (a través de USB o una red Wi-Fi en la que confíes) y confiar en el equipo, puedes conectarte a tu dispositivo de forma segura.
1. Asegúrate de que estés en una red segura (USB o una red Wi-Fi de confianza).
2. Descarga el certificado de este dispositivo desde la página "Seguridad" de Device Portal. Haz clic en el vínculo Seguridad desde la lista superior derecha de los iconos o navega a: `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`

3. Instale el certificado en el "confianza entidades de certificación raíz" almacenar en su equipo.: en el menú de Windows, escriba: Administrar certificados de equipo e inicie el applet.
    - Expande la carpeta de la entidad de certificación raíz de confianza.
    - Haz clic en la carpeta Certificados.
    - En el menú Acción, seleccione: Todas las tareas > Importar...
    - Completa el Asistente para importación de certificados con el archivo de certificado descargado desde Device Portal.

4. Reinicia el explorador.


## <a name="device-portal-pages"></a>Páginas de Device Portal 

### <a name="home"></a>Inicio 

La sesión de Device Portal se inicia en la página principal. Accede a otras páginas desde la barra de navegación en el lado izquierdo de la página principal.

La barra de herramientas de la parte superior de la página proporciona acceso al estado usado frecuentemente y a las características.
- **En línea**: Indica si el dispositivo está conectado a Wi-Fi.
- **Shutdown**: Desactiva el dispositivo.
- **Restart**: Los ciclos de energía en el dispositivo.
- **Seguridad**: Abre la página seguridad del dispositivo.
- **Genial**: Indica la temperatura del dispositivo.
- **A/C**: Indica si el dispositivo está conectado y de carga.
- **Ayudar a**: Se abre la página de documentación de la interfaz REST.

En la página principal se muestra la siguiente información:
- Estado del **dispositivo**: supervisa el estado del dispositivo y notifica los errores graves.
- **Información de Windows**: muestra el nombre de HoloLens y la versión de Windows instalada actualmente.
- La sección **Preferencias** contiene las siguientes configuraciones:
    - **IPD**: Establece la distancia interpupillary (IPD), que es la distancia, en milímetros, entre el centro de los alumnos del usuario al buscar recta. La configuración surte efecto inmediatamente. El valor predeterminado se calculó automáticamente al configurar el dispositivo.
    - **Nombre de dispositivo**: Asigne un nombre a la HoloLens. Debes reiniciar el dispositivo después de cambiar este valor para que surta efecto. Después de hacer clic en Guardar, en un cuadro de diálogo se te preguntará si quieres reiniciar el dispositivo inmediatamente o más tarde.
    - **Configuración de suspensión**: Establece la longitud de tiempo de espera antes de que el dispositivo entra en suspensión cuando está conectado y si se encuentra en la batería.

### <a name="3d-view"></a>Vista 3D 

Usa la página Vista 3D para ver cómo HoloLens interpreta el entorno. Navega por la vista con el mouse:
- **Girar**: haz clic con el botón izquierdo + mouse;
- **Panorámica**: haz clic con el botón derecho + mouse;
- **Zoom**: desplazamiento del mouse.
- **Opciones de seguimiento**: Activar seguimiento visual continua activando el seguimiento visual de fuerza. Pausa detiene el seguimiento visual.
- **Ver las opciones de**: Establecer las opciones de la vista 3D:: seguimiento: Indica si el seguimiento visual está activa.
- **Mostrar floor**: Muestra un plano a cuadros de límite inferior.
- **Mostrar frustum**: Muestra el frustum de vista.
- **Mostrar el plano de estabilización**: Muestra el plano de HoloLens se usa para la estabilización del movimiento.
- **Mostrar malla**: Muestra la malla de superficie de asignación que representa su entorno.
- **Mostrar detalles**: Muestra entrega posiciones, cuaterniones rotación principal y el vector de origen del dispositivo según cambian en tiempo real.
- **Botón de pantalla completa**: Muestra la vista 3D en modo de pantalla completa. Presiona ESC para salir de la vista de pantalla completa.

- Reconstrucción de superficie: Haga clic o pulse Actualizar para mostrar la malla de asignación espacial más reciente desde el dispositivo. Un recorrido completo puede tardar unos segundos en completarse. La malla no se actualiza automáticamente en la vista 3D y debes hacer clic manualmente en Actualizar para obtener la malla más reciente desde el dispositivo. Haz clic en Guardar para guardar la malla de asignación espacial actual como un archivo obj en tu equipo.

### <a name="mixed-reality-capture"></a>Captura de realidad mixta 

Usa la página Mixed Reality Capture (Captura de realidad mixta) para guardar flujos multimedia desde HoloLens.
- Configuración: Controlar las secuencias de multimedia que se capturan mediante la comprobación de las siguientes opciones:-hologramas: Captura el contenido en la secuencia de vídeo holográfico. Los hologramas se representan en mono y no estéreo.
- **Cámara PV**: Captura de la secuencia de vídeo desde la cámara de vídeo y fotos.
- **MIC Audio**: Captura de audio de la matriz de micrófono.
- **Aplicación Audio**: Captura de audio desde la aplicación que se está ejecutando.
- **Calidad de vista previa de Live**: Seleccione la resolución de pantalla, la velocidad de fotogramas y la velocidad de transmisión por secuencias para la vista previa activa.

- Haz clic o pulsa el botón Vista previa dinámica para mostrar el flujo de captura. La opción Stop live preview detiene el flujo de captura.
- Haz clic o pulsa en Grabar para comenzar a grabar el flujo de realidad mixta, con la configuración especificada. La opción Detener grabación finaliza la grabación y la guarda.
- Haz clic o pulsa en Tomar foto para tomar una imagen estática del flujo de captura.
- **Fotos y vídeos**: Muestra una lista de capturas de vídeo y fotos tomadas en el dispositivo.

Ten en cuenta que las aplicaciones de HoloLens no podrán capturar una foto o un vídeo MRC mientras grabas o haces streaming de una Vista previa dinámica des de Device Portal.

### <a name="system-performance"></a>Rendimiento del sistema 

La herramienta Rendimiento del sistema de HoloLens tiene 3 métricas adicionales que se pueden grabar. 

Estas son las métricas disponibles:
- **SoC power**: Instantánea del sistema en chip energético, promediado por minuto
- **Alimentación del sistema**: Utilización de energía del sistema instantáneo, promediado por minuto
- **Velocidad de fotogramas**: Fotogramas por segundo, perdió VBlanks por segundo y consecutivos VBlanks perdidas

### <a name="app-crash-dumps-page"></a>Página de volcados de memoria de la aplicación 

En esta página puedes recopilar los volcados de memoria de las aplicaciones transferidas localmente. Activa la casilla Volcados de memoria de cada aplicación de la que quieras recopilar volcados de memoria. Vuelve a esta página para recopilar los volcados de memoria. Los archivos de volcado de memoria se pueden abrir en Visual Studio para su depuración.

### <a name="kiosk-mode"></a>Pantalla completa 

Habilita la pantalla completa, lo que limita la capacidad del usuario para iniciar aplicaciones nuevas o cambiar la aplicación en ejecución. Cuando la pantalla completa está habilitada, se deshabilitan el gesto Nástico y Cortana y no se muestran las aplicaciones colocadas en el entorno del usuario.

Activa Enable Kiosk Mode para poner HoloLens en pantalla completa. Selecciona la aplicación para que se ejecute en el inicio de la lista desplegable de la aplicación de inicio. Haz clic o pulsa en Guardar para confirmar la configuración.

Ten en cuenta que la aplicación se ejecutará en el inicio, incluso si no está habilitada la pantalla completa. Selecciona Ninguna para que no se ejecute ninguna aplicación en el inicio.

### <a name="simulation"></a>Simulation 

Te permite grabar y reproducir datos de entrada para las pruebas.
- **Capturar sala**: Se utiliza para descargar un archivo de sala simulado que contenga la malla de asignación espacial para el entorno del usuario. Asigna un nombre al espacio y, a continuación, haz clic en Capturar para guardar los datos como un archivo .xef en tu equipo. Puedes cargar este archivo de espacio en el emulador HoloLens.
- **Grabación**: Compruebe las secuencias para registrar, nombre de la grabación y haga clic o pulse en el registro para iniciar la grabación. Realiza acciones con HoloLens y, a continuación, haz clic en Detener para guardar los datos como un archivo .xef en tu equipo. Este archivo se puede cargar en el dispositivo o emulador HoloLens.
- **Reproducción**: Haga clic o pulse la carga de grabación para seleccionar un archivo xef desde su PC y enviar los datos a la HoloLens.
- **Modo de control**: Seleccione predeterminado o simulación en la lista desplegable y haga clic en o pulse el botón de conjunto para seleccionar el modo en el HoloLens. Si eliges "Simulación", se deshabilitan los sensores reales de HoloLens y se usan los datos simulados cargados en su lugar. Si cambias a "Simulación", HoloLens no responderá al usuario real hasta que vuelva al "Valor predeterminado".


### <a name="virtual-input"></a>Entrada virtual 

Envía la entrada de teclado desde la máquina remota a HoloLens.

Haz clic o pulsa en la región de debajo del teclado virtual para habilitar el envío de pulsaciones de teclas a HoloLens. Escribe texto de entrada en el cuadro de texto y haz clic o pulsa en Enviar para enviar las pulsaciones de teclas a la aplicación activa.

## <a name="see-also"></a>Vea también

* [Información general de Windows Device Portal](device-portal.md)
* [Referencia de API de principales del Portal de dispositivos](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core) (API comunes a todos los dispositivos Windows 10)
* [Referencia de API de realidad mixta del Portal de dispositivos](https://docs.microsoft.com/windows/mixed-reality/device-portal-api-reference) (una lista ampliada de todas las API de REST disponibles para HoloLens)

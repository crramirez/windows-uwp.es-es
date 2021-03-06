---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal para HoloLens
description: Obtén información sobre cómo Windows Device Portal para HoloLens te permite configurar y administrar de forma remota tu dispositivo HoloLens.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: 5cf8dc0912420895091815e54f6399235fca552f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173589"
---
# <a name="device-portal-for-hololens"></a>Device Portal para HoloLens


## <a name="set-up-device-portal-on-hololens"></a>Configurar Device Portal en HoloLens

### <a name="enable-device-portal"></a>Habilitar Device Portal

1. Enciende HoloLens y colócalo en el dispositivo.
2. Realiza el [gesto Inicio](/hololens/hololens2-basic-usage#start-gesture) o de [eclosión](https://developer.microsoft.com/mixed-reality#Bloom) para HoloLens (1.ª generación) para iniciar el menú principal.
3. Mira el icono **Configuración** y realiza el gesto de [pulsación](https://developer.microsoft.com/mixed-reality#Press_and_release) en HoloLens (1.ª generación) o selecciónalo en HoloLens 2 [tocándolo o usando un haz de mano](/hololens/hololens2-basic-usage). La aplicación Configuración se iniciará una vez la hayas seleccionado.
4. Selecciona el elemento de menú **Actualizar**.
5. Selecciona el elemento de menú **Para desarrolladores**.
6. Habilita el **Modo de desarrollador**.
7. [Desplázate hacia abajo](https://developer.microsoft.com/mixed-reality#Navigation) y habilita Device Portal.


### <a name="pair-your-device"></a>Emparejar el dispositivo

#### <a name="connect-over-wi-fi"></a>Conectarse a través de Wi-Fi 

1. Conecta HoloLens a una red Wi-Fi.
2. Busca la dirección IP de tu dispositivo. Busca la dirección IP del dispositivo en **Configuración > Red e Internet > Wi-Fi > Propiedades de hardware**. También puede decir: "Hola Cortana, ¿cuál es mi dirección IP?"

3. Desde un explorador web de tu equipo, ve a `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - Se mostrará el siguiente mensaje en el explorador: "Hay un problema con el certificado de seguridad de este sitio web". Esto ocurre porque el certificado emitido en Device Portal es un certificado de prueba. Puedes ignorar este error de certificado por ahora y continuar.

#### <a name="connect-over-usb"></a>Conectarse a través de USB 

1. Instala las herramientas para asegurarte de que tienes Visual Studio Update 1 con las Herramientas de desarrollo de Windows 10 instaladas en el equipo. Esto permite la conectividad USB.
2. Conecta tu HoloLens a tu equipo con un cable micro USB para HoloLens (1.ª generación) o USB-C para HoloLens 2.
3. Desde un explorador web de tu equipo, ve a `http://127.0.0.1:10080`.

> [!IMPORTANT]
> Si el equipo no encuentra el dispositivo, prueba a usar la dirección IP de red real del dispositivo HoloLens, en lugar de `http://127.0.0.1:10080`.

#### <a name="connect-to-an-emulator"></a>Conectarse a un emulador 

También puedes usar Device Portal con el emulador. Para conectarte a Device Portal, usa la barra de herramientas. Haz clic en este icono:
- Abre el Portal de dispositivos: abre el Portal de dispositivos Windows correspondiente al sistema operativo de HoloLens en el emulador.

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

3. Instala el certificado en el almacén "Entidades de certificación raíz de confianza" en tu equipo. Desde el menú Windows, escribe: Administrar certificados de equipo e inicia el applet.
    - Expande la carpeta de la entidad de certificación raíz de confianza.
    - Haz clic en la carpeta Certificados.
    - En el menú Acción, selecciona: Todas las tareas > Importar...
    - Completa el Asistente para importación de certificados con el archivo de certificado descargado desde Device Portal.

4. Reinicia el explorador.


## <a name="device-portal-pages"></a>Páginas de Device Portal 

### <a name="home"></a>Inicio 

La sesión de Device Portal se inicia en la página principal. Accede a otras páginas desde la barra de navegación en el lado izquierdo de la página principal.

La barra de herramientas de la parte superior de la página proporciona acceso al estado usado frecuentemente y a las características.
- **En línea**: indica si el dispositivo está conectado a Wi-Fi.
- **Apagar**: apaga el dispositivo.
- **Reiniciar**: apaga y vuelve a encender el dispositivo.
- **Seguridad**: abre la página Seguridad del dispositivo.
- **Frío**: indica la temperatura del dispositivo.
- **CA**: indica si el dispositivo está enchufado y cargándose.
- **Ayuda**: abre la página de documentación de la interfaz REST.

En la página principal se muestra la siguiente información:
- Estado del **dispositivo**: supervisa el estado del dispositivo y notifica los errores graves.
- **Información de Windows**: muestra el nombre de HoloLens y la versión de Windows instalada actualmente.
- La sección **Preferencias** contiene las siguientes configuraciones:
    - **IPD**: establece la distancia interpupilar (IPD), que es la distancia, en milímetros, entre el centro de las pupilas del usuario cuando mira al frente. La configuración surte efecto inmediatamente. El valor predeterminado se calculó automáticamente al configurar el dispositivo. **Válido solo para HoloLens (1ª generación); Hololens 2 calcula la posición de los ojos.** 
    - **Nombre del dispositivo**: asigna un nombre a HoloLens. Debes reiniciar el dispositivo después de cambiar este valor para que surta efecto. Después de hacer clic en Guardar, en un cuadro de diálogo se te preguntará si quieres reiniciar el dispositivo inmediatamente o más tarde.
    - **Configuración de suspensión**: establece el período de tiempo de espera antes de que el dispositivo entre en el modo de suspensión cuando está conectado y cuando funciona con batería.

### <a name="3d-view"></a>Vista 3D 

Usa la página Vista 3D para ver cómo HoloLens interpreta el entorno. Navega por la vista con el mouse:
- **Girar**: haz clic con el botón izquierdo + mouse;
- **Panorámica**: haz clic con el botón derecho + mouse;
- **Zoom**: desplazamiento del mouse.
- **Opciones de seguimiento**: activa el seguimiento visual continuo marcando la opción Force visual tracking (Forzar seguimiento visual). Pausa detiene el seguimiento visual.
- **Opciones de vista**: establece las opciones en la vista 3D. Seguimiento: indica si el seguimiento visual está activo.
- **Show floor** (Mostrar suelo): muestra un plano de la planta a cuadros.
- **Show frustum** (Mostrar tronco): muestra el tronco de la vista.
- **Show stabilization plane** (Mostrar plano de estabilización): muestra el plano que HoloLens usa para estabilizar el movimiento.
- **Show mesh** (Mostrar malla): muestra la malla de asignación de superficie que representa el entorno.
- **Mostrar detalles**: muestra las posiciones de la mano, los cuaterniones de rotación de la cabeza y el vector de origen del dispositivo cuando cambian en tiempo real.
- **Botón Pantalla completa**: muestra la vista 3D en modo de pantalla completa. Presiona ESC para salir de la vista de pantalla completa.

- Reconstrucción de superficie: haz clic o pulsa en Actualizar para mostrar la última malla de asignación espacial del dispositivo. Un recorrido completo puede tardar unos segundos en completarse. La malla no se actualiza automáticamente en la vista 3D y debes hacer clic manualmente en Actualizar para obtener la malla más reciente desde el dispositivo. Haz clic en Guardar para guardar la malla de asignación espacial actual como un archivo obj en tu equipo.

### <a name="mixed-reality-capture"></a>Captura de realidad mixta 

Usa la página Mixed Reality Capture (Captura de realidad mixta) para guardar flujos multimedia desde HoloLens.
- Configuración: controla las secuencias multimedia que se capturan comprobando la siguiente configuración. Hologramas: captura el contenido holográfico en la secuencia de vídeo. Los hologramas se representan en mono y no estéreo.
- **PV camera** (Cámara de vídeo o fotos): captura la secuencia de vídeo de la cámara de vídeo o fotos.
- **Mic Audio** (Audio de micrófono): captura el audio de la matriz del micrófono.
- **App Audio** (Audio de la aplicación): captura el audio de la aplicación que está en ejecución actualmente.
- **Live preview quality** (Calidad de vista previa dinámica): selecciona la resolución de pantalla, la velocidad de fotogramas y la velocidad de streaming de la vista previa dinámica.

- Haz clic o pulsa el botón Vista previa dinámica para mostrar el flujo de captura. La opción Stop live preview detiene el flujo de captura.
- Haz clic o pulsa en Grabar para comenzar a grabar el flujo de realidad mixta, con la configuración especificada. La opción Detener grabación finaliza la grabación y la guarda.
- Haz clic o pulsa en Tomar foto para tomar una imagen estática del flujo de captura.
- **Vídeos y fotos**: muestra una lista de las capturas de vídeo y fotos tomadas en el dispositivo.

Ten en cuenta que las aplicaciones de HoloLens no podrán capturar una foto o un vídeo MRC mientras grabas o haces streaming de una Vista previa dinámica des de Device Portal.

### <a name="system-performance"></a>Rendimiento del sistema 

La herramienta Rendimiento del sistema de HoloLens tiene 3 métricas adicionales que se pueden grabar. 

Estas son las métricas disponibles:
- **SoC power** (Energía SoC): uso de energía instantánea system-on-chip, con una media superior a un minuto
- **Energía del sistema**: uso de energía instantánea del sistema, con una media superior a un minuto
- **Velocidad de fotogramas**: fotogramas por segundo, VBlank (intervalos verticales) perdidos por segundo y VBlank perdidos consecutivos

### <a name="app-crash-dumps-page"></a>Página de volcados de memoria de la aplicación 

En esta página puedes recopilar los volcados de memoria de las aplicaciones transferidas localmente. Activa la casilla Volcados de memoria de cada aplicación de la que quieras recopilar volcados de memoria. Vuelve a esta página para recopilar los volcados de memoria. Los archivos de volcado de memoria se pueden abrir en Visual Studio para su depuración.

### <a name="kiosk-mode"></a>Pantalla completa 

Habilita la pantalla completa, lo que limita la capacidad del usuario para iniciar aplicaciones nuevas o cambiar la aplicación en ejecución. Cuando la pantalla completa está habilitada, se deshabilitan el gesto Nástico y Cortana y no se muestran las aplicaciones colocadas en el entorno del usuario.

Activa Enable Kiosk Mode para poner HoloLens en pantalla completa. Selecciona la aplicación para que se ejecute en el inicio de la lista desplegable de la aplicación de inicio. Haz clic o pulsa en Guardar para confirmar la configuración.

Ten en cuenta que la aplicación se ejecutará en el inicio, incluso si no está habilitada la pantalla completa. Selecciona Ninguna para que no se ejecute ninguna aplicación en el inicio.

### <a name="simulation"></a>Simulation 

Te permite grabar y reproducir datos de entrada para las pruebas.
- **Capture room** (Espacio de captura): se usa para descargar un archivo de espacio simulado que contiene la malla espacial de asignación del entorno del usuario. Asigna un nombre al espacio y, a continuación, haz clic en Capturar para guardar los datos como un archivo .xef en tu equipo. Puedes cargar este archivo de espacio en el emulador HoloLens.
- **Grabación**: marca los flujos que quieres grabar, asigna un nombre a la grabación y haz clic o pulsa en Grabar para iniciar la grabación. Realiza acciones con HoloLens y, a continuación, haz clic en Detener para guardar los datos como un archivo .xef en tu equipo. Este archivo se puede cargar en el dispositivo o emulador HoloLens.
- **Reproducción**: haz clic o pulsa en Cargar grabación para seleccionar un archivo .xef desde tu equipo y enviar los datos a HoloLens.
- **Modo de control**: selecciona Valor predeterminado o Simulación en la lista desplegable y pulsa o haz clic en el botón Establecer para seleccionar el modo en HoloLens. Si eliges "Simulación", se deshabilitan los sensores reales de HoloLens y se usan los datos simulados cargados en su lugar. Si cambias a "Simulación", HoloLens no responderá al usuario real hasta que vuelva al "Valor predeterminado".


### <a name="virtual-input"></a>Entrada virtual 

Envía la entrada de teclado desde la máquina remota a HoloLens.

Haz clic o pulsa en la región de debajo del teclado virtual para habilitar el envío de pulsaciones de teclas a HoloLens. Escribe texto de entrada en el cuadro de texto y haz clic o pulsa en Enviar para enviar las pulsaciones de teclas a la aplicación activa.

## <a name="see-also"></a>Consulta también

* [Introducción a Windows Device Portal](device-portal.md)
* [Referencia de API principal del Portal de dispositivos](./device-portal-api-core.md) (API comunes a todos los dispositivos Windows 10)
* [Referencia de API de realidad mixta del Portal de dispositivos](/windows/mixed-reality/device-portal-api-reference) (lista ampliada de todas las API REST disponibles para HoloLens)
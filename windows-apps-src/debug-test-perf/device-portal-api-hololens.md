---
author: mcleblanc
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: Referencia de API de Device Portal para HoloLens
description: "Obtén información sobre las API de REST de Windows Device Portal para HoloLens que puedes usar para acceder a los datos y controlar el dispositivo mediante programación."
ms.openlocfilehash: 638ebca167b2ca56f00a83aab13b15c57b2dca2a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="device-portal-api-reference-for-hololens"></a>Referencia de API de Device Portal para HoloLens

Todo lo que contiene Windows Device Portal se basa en las API de REST que puedes usar para acceder a los datos y controlar el dispositivo mediante programación.

## <a name="holographic-os"></a>SO Holographic
---
### <a name="get-https-requirements-for-the-device-portal"></a>Obtener los requisitos de HTTPS para Device Portal

**Solicitud**

Puedes obtener los requisitos de HTTPS de Device Portal mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/os/webmanagement/settings/https


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-stored-interpupillary-distance-ipd"></a>Obtener la distancia interpupilar almacenada (IPD)

**Solicitud**

Puedes obtener el valor de IPD almacenado mediante el siguiente formato de solicitud. El valor se devuelve en milímetros.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/os/settings/ipd


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-a-list-of-hololens-specific-etw-providers"></a>Obtener una lista de proveedores de ETW específicos de HoloLens

**Solicitud**

Puedes obtener una lista de proveedores de ETW específicos de HoloLens que no estén registrados en el sistema mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/os/etw/customproviders


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="return-the-state-for-all-active-services"></a>Devolver el estado de todos los servicios activos

**Solicitud**

Puedes obtener el estado de todos los servicios que se están ejecutando mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/os/services


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="set-the-https-requirement-for-the-device-portal"></a>Establece el requisito de HTTPS para Device Portal.

**Solicitud**

Puedes establecer los requisitos de HTTPS de Device Portal mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/management/settings/https


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
obligatorio   | (**obligatorio**) Determina si HTTPS es necesario o no para Device Portal. Los valores posibles son **yes**, **no** y **default**.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="set-the-interpupillary-distance-ipd"></a>Establecer la distancia interpupilar (IPD)

**Solicitud**

Puedes establecer el valor de IPD almacenado mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/os/settings/ipd


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
ipd   | (**obligatorio**) Nuevo valor de IPD que debe almacenarse. Este valor debe estar en milímetros.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
## Holographic perception
---
### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>Aceptar las actualizaciones de WebSocket y ejecutar un cliente de Mirage que envíe actualizaciones

**Solicitud**

Puedes aceptar las actualizaciones de WebSocket y ejecutar un cliente de Mirage que envíe actualizaciones a 30 FPS mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET/WebSocket | /api/holographic/perception/client


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
clientmode   | (**obligatorio**) Determina el modo de seguimiento. Un valor **active** fuerza el modo de seguimiento visual cuando no se puede establecer de manera pasiva.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
## Holographic thermal
---
### <a name="get-the-thermal-stage-of-the-device"></a>Obtener la fase térmica del dispositivo

**Solicitud**

Puedes obtener la fase térmica del dispositivo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Los valores posibles se indican en la siguiente tabla.

Valor | Descripción
--- | ---
1 | Normal
2 | Caliente
3 | Crítico

**Código de estado**

- Códigos de estado estándar.

---
## HSimulation control
---
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>Crear un flujo de control o publicar datos en un flujo creado

**Solicitud**

Puedes crear un flujo de control o publicar datos en un flujo creado mediante el siguiente formato de solicitud. Se espera que los datos publicados sean del tipo **application/octet-stream**.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/control/stream


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
prioridad   | (**es obligatorio si se crea un flujo de creación control**) Indica la prioridad del flujo.
streamid   | (**es obligatorio si se publica en un flujo creado**) Identificador del flujo al que se debe publicar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="delete-a-control-stream"></a>Eliminar un flujo de control

**Solicitud**

Puedes eliminar un flujo de control mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/simulation/control/stream


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-a-control-stream"></a>Obtener un flujo de control

**Solicitud**

Puedes abrir una conexión de socket web para un flujo de control mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET/WebSocket | /api/holographic/simulation/control/stream


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-simluation-mode"></a>Obtener el modo de simulación

**Solicitud**

Puedes obtener el modo de simulación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/control/mode


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="set-the-simluation-mode"></a>Establecer el modo de simulación

**Solicitud**

Puedes establecer el modo de simulación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simluation/control/mode


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
mode   | (**obligatorio**) Indica el modo de simulación. Los valores posibles son **default**, **simulation**, **remote** y **legacy**.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
## HSimulation playback
---
### <a name="delete-a-recording"></a>Eliminar una grabación

**Solicitud**

Puedes eliminar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/simulation/playback/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que se debe eliminar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-all-recordings"></a>Obtener todas las grabaciones

**Solicitud**

Puedes obtener todas las grabaciones disponibles mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/playback/files


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-types-of-data-in-a-loaded-recording"></a>Obtener los tipos de datos en una grabación cargada

**Solicitud**

Puedes obtener los tipos de datos en una grabación cargada mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/playback/session/types


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que te interesa.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-all-the-loaded-recordings"></a>Obtener todas las grabaciones cargadas

**Solicitud**

Puedes obtener todas las grabaciones cargadas mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/playback/session/files


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-current-playback-state-of-a-recording"></a>Obtener el estado de reproducción actual de una grabación 

**Solicitud**

Puedes obtener el estado de reproducción actual de una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/playback/session


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que te interesa.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="load-a-recording"></a>Cargar una grabación

**Solicitud**

Puedes cargar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que se debe cargar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="pause-a-recording"></a>Pausar una grabación

**Solicitud**

Puedes pausar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/pause


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que se debe pausar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="play-a-recording"></a>Reproducir una grabación

**Solicitud**

Puedes reproducir una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/play


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que se debe reproducir.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="stop-a-recording"></a>Detener una grabación

**Solicitud**

Puedes detener una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/stop


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que se debe detener.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="unload-a-recording"></a>Descargar una grabación

**Solicitud**

Puedes descargar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/simulation/playback/session/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
recording   | (**obligatorio**) Nombre de la grabación que se debe descargar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="upload-a-recording"></a>Cargar una grabación

**Solicitud**

Puedes cargar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/file


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
## HSimulation recording
---
### <a name="get-the-recording-state"></a>Obtener el estado de grabación

**Solicitud**

Puedes obtener el estado de grabación actual mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/recording/status


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="start-a-recording"></a>Iniciar una grabación

**Solicitud**

Puedes iniciar una grabación mediante el siguiente formato de solicitud. Solo puede haber una grabación activa a la vez. 
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/recording/start


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
head   | (**consulta más adelante**) Establece este valor en 1 para indicar que el sistema debe registrar los datos de encabezado.
hands   | (**consulta más adelante**) Establece este valor en 1 para indicar que el sistema debe registrar los datos de manos.
spatialMapping   | (**consulta más adelante**) Establece este valor en 1 para indicar que el sistema debe registrar los datos de asignación espacial.
environment   | (**consulta más adelante**) Establece este valor en 1 para indicar que el sistema debe registrar los datos de entorno.
name   | (**obligatorio**) Nombre de la grabación.
singleSpatialMappingFrame   | (**opcional**) Establece este valor en 1 para indicar que solo se debe registrar un marco de asignación espacial.

De estos parámetros, exactamente uno debe establecerse en 1: *head*, *hands*, *spatialMapping* o *environment*.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="stop-the-current-recording"></a>Detener la grabación actual

**Solicitud**

Puedes detener la grabación actual mediante el siguiente formato de solicitud. La grabación se devolverá como un archivo.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/recording/stop


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
## Mixed reality capture
---
### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>Eliminar una grabación de captura de realidad mixta (MRC) del dispositivo

**Solicitud**

Puedes eliminar una grabación de MRC mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/mrc/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
filename   | (**obligatorio**) Nombre del archivo de vídeo que se debe eliminar. El nombre debe estar codificado mediante hex64.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="download-a-mixed-reality-capture-mrc-file"></a>Descargar un archivo de captura realidad mixta (MRC)

**Solicitud**

Puedes descargar un archivo de MRC desde el dispositivo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/mrc/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
filename   | (**obligatorio**) Nombre del archivo de vídeo que quieres obtener. El nombre debe estar codificado mediante hex64.
op   | (**opcional**) Establece este valor en **stream** si quieres descargar un flujo.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-mixed-reality-capture-mrc-settings"></a>Obtener la configuración de captura de realidad mixta (MRC)

**Solicitud**

Puedes obtener la configuración de MRC mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/mrc/settings


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>Obtener el estado de la grabación de captura de realidad mixta (MRC)

**Solicitud**

Puedes obtener el estado de grabación de MRC mediante el siguiente formato de solicitud. Los posibles valores son **running** y **stopped**.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/mrc/status


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>Obtener la lista de los archivos de captura de realidad mixta (MRC)

**Solicitud**

Puedes obtener los archivos MRC almacenados en el dispositivo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/mrc/files


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="set-the-mixed-reality-capture-mrc-settings"></a>Establecer la configuración de captura de realidad mixta (MRC)

**Solicitud**

Puedes establecer la configuración de MRC mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/mrc/settings


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>Inicia una grabación de captura de realidad mixta (MRC)

**Solicitud**

Puedes iniciar una grabación de MRC mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/mrc/video/control/start


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>Detener la grabación de captura de realidad mixta (MRC) actual

**Solicitud**

Puedes detener la grabación de MRC actual mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/mrc/video/control/stop


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="take-a-mixed-reality-capture-mrc-photo"></a>Tomar una foto de captura de realidad mixta (MRC)

**Solicitud**

Puedes tomar una foto de MRC mediante el siguiente formato de solicitud. La foto se devuelve en formato JPEG.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/mrc/photo


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
## Mixed reality streaming
---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la calidad de forma predeterminada.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
pv   | (**opcional**) Indica si se debe capturar la cámara PV. Debe ser **true** o **false**.
holo   | (**opcional**) Indica si se deben capturar hologramas. Debe ser **true** o **false**.
mic   | (**opcional**) Indica si se debe capturar el micrófono. Debe ser **true** o **false**.
loopback   | (**opcional**) Indica si se debe capturar el audio de la aplicación. Debe ser **true** o **false**.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la alta calidad.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live_high.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
pv   | (**opcional**) Indica si se debe capturar la cámara PV. Debe ser **true** o **false**.
holo   | (**opcional**) Indica si se deben capturar hologramas. Debe ser **true** o **false**.
mic   | (**opcional**) Indica si se debe capturar el micrófono. Debe ser **true** o **false**.
loopback   | (**opcional**) Indica si se debe capturar el audio de la aplicación. Debe ser **true** o **false**.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la calidad baja.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live_low.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
pv   | (**opcional**) Indica si se debe capturar la cámara PV. Debe ser **true** o **false**.
holo   | (**opcional**) Indica si se deben capturar hologramas. Debe ser **true** o **false**.
mic   | (**opcional**) Indica si se debe capturar el micrófono. Debe ser **true** o **false**.
loopback   | (**opcional**) Indica si se debe capturar el audio de la aplicación. Debe ser **true** o **false**.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la calidad media.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live_med.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
pv   | (**opcional**) Indica si se debe capturar la cámara PV. Debe ser **true** o **false**.
holo   | (**opcional**) Indica si se deben capturar hologramas. Debe ser **true** o **false**.
mic   | (**opcional**) Indica si se debe capturar el micrófono. Debe ser **true** o **false**.
loopback   | (**opcional**) Indica si se debe capturar el audio de la aplicación. Debe ser **true** o **false**.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

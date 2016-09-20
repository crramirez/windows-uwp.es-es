---
author: mcleblanc
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: Referencia de API de Device Portal para HoloLens
description: "Obtén información sobre las API de REST de Windows Device Portal para HoloLens que puedes usar para acceder a los datos y controlar el dispositivo mediante programación."
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5ed8ffe0a409569777fbf4e56a90ab3b80cd395c

---
# Referencia de API de Device Portal para HoloLens

Todo lo que contiene Windows Device Portal se basa en las API de REST que puedes usar para acceder a los datos y controlar el dispositivo mediante programación.

## SO Holographic
---
### Obtener los requisitos de HTTPS para Device Portal

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
### Obtener la distancia interpupilar almacenada (IPD)

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
### Obtener una lista de proveedores de ETW específicos de HoloLens

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
### Devolver el estado de todos los servicios activos

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
### Establece el requisito de HTTPS para Device Portal.

**Solicitud**

Puedes establecer los requisitos de HTTPS de Device Portal mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/management/settings/https


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Establecer la distancia interpupilar (IPD)

**Solicitud**

Puedes establecer el valor de IPD almacenado mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/os/settings/ipd


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
## Percepción holográfica
---
### Aceptar las actualizaciones de WebSocket y ejecutar un cliente de Mirage que envíe actualizaciones

**Solicitud**

Puedes aceptar las actualizaciones de WebSocket y ejecutar un cliente de Mirage que envíe actualizaciones a 30 FPS mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET/WebSocket | /api/holographic/perception/client


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
## Fase térmica holográfica
---
### Obtener la fase térmica del dispositivo

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
## Control de HSimulation
---
### Crear un flujo de control o publicar datos en un flujo creado

**Solicitud**

Puedes crear un flujo de control o publicar datos en un flujo creado mediante el siguiente formato de solicitud. Se espera que los datos publicados sean del tipo **application/octet-stream**.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/control/stream


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Eliminar un flujo de control

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
### Obtener un flujo de control

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
### Obtener el modo de simulación

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
### Establecer el modo de simulación

**Solicitud**

Puedes establecer el modo de simulación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simluation/control/mode


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
## Reproducción de HSimulation
---
### Eliminar una grabación

**Solicitud**

Puedes eliminar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/simulation/playback/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Obtener todas las grabaciones

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
### Obtener los tipos de datos en una grabación cargada

**Solicitud**

Puedes obtener los tipos de datos en una grabación cargada mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/playback/session/types


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Obtener todas las grabaciones cargadas

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
### Obtener el estado de reproducción actual de una grabación 

**Solicitud**

Puedes obtener el estado de reproducción actual de una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/simulation/playback/session


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Cargar una grabación

**Solicitud**

Puedes cargar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Pausar una grabación

**Solicitud**

Puedes pausar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/pause


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Reproducir una grabación

**Solicitud**

Puedes reproducir una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/play


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Detener una grabación

**Solicitud**

Puedes detener una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/playback/session/stop


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Descargar una grabación

**Solicitud**

Puedes descargar una grabación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/simulation/playback/session/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Cargar una grabación

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
## Grabación de HSimulation
---
### Obtener el estado de grabación

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
### Iniciar una grabación

**Solicitud**

Puedes iniciar una grabación mediante el siguiente formato de solicitud. Solo puede haber una grabación activa a la vez. 
 
Método      | URI de solicitud
:------     | :-----
POST | /api/holographic/simulation/recording/start


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Detener la grabación actual

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
## Captura de realidad mixta
---
### Eliminar una grabación de captura de realidad mixta (MRC) del dispositivo

**Solicitud**

Puedes eliminar una grabación de MRC mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/holographic/mrc/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Descargar un archivo de captura realidad mixta (MRC)

**Solicitud**

Puedes descargar un archivo de MRC desde el dispositivo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/mrc/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Obtener la configuración de captura de realidad mixta (MRC)

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
### Obtener el estado de la grabación de captura de realidad mixta (MRC)

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
### Obtener la lista de los archivos de captura de realidad mixta (MRC)

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
### Establecer la configuración de captura de realidad mixta (MRC)

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
### Inicia una grabación de captura de realidad mixta (MRC)

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
### Detener la grabación de captura de realidad mixta (MRC) actual

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
### Tomar una foto de captura de realidad mixta (MRC)

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
## Streaming de realidad mixta
---
### Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la calidad de forma predeterminada.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la alta calidad.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live_high.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la calidad baja.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live_low.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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
### Inicia una descarga fragmentada de un archivo mp4 fragmentado

**Solicitud**

Puedes iniciar una descarga fragmentada de un archivo mp4 fragmentado con el siguiente formato de solicitud. Esta API utiliza la calidad media.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/holographic/stream/live_med.mp4


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

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



<!--HONumber=Jun16_HO4-->



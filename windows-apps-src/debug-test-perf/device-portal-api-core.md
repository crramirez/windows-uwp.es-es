---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referencia de API principal de Device Portal
description: Obtén información sobre las API de REST principales de Windows Device Portal que puedes usar para acceder a los datos y controlar el dispositivo mediante programación.
---

# Referencia de API principal de Device Portal

Todo lo que contiene Windows Device Portal se basa en las API de REST que puedes usar para acceder a los datos y controlar el dispositivo mediante programación.

## Implementación de la aplicación

---
### Instalar una aplicación

**Solicitud**

Puede instalar una aplicación mediante el siguiente formato de solicitud.

Método      | URI de solicitud
:------     | :-----
POST | /api/appx/packagemanager/package

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
paquete   | (**obligatorio**) Nombre de archivo del paquete que debe instalarse.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Obtener el estado de instalación de la aplicación

**Solicitud**

Puedes obtener el estado de la instalación de una aplicación que está en curso mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/appx/packagemanager/state

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

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Desinstalar una aplicación

**Solicitud**

Puedes desinstalar una aplicación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/appx/packagemanager/package


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

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Obtener aplicaciones instaladas

**Solicitud**

Puedes obtener una lista de aplicaciones instaladas en el sistema mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/appx/packagemanager/packages


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de los paquetes instalados con detalles asociados.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Administrador de dispositivos
---
### Obtener los dispositivos instalados en la máquina

**Solicitud**

Puedes obtener una lista de dispositivos que están instalados en la máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/devicemanager/devices


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye una estructura JSON que contiene un árbol de dispositivos jerárquico.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* IoT

---
## Colección de volcados de memoria
---
### Obtener la lista de todos los volcados de memoria de aplicaciones

**Solicitud**

Puedes obtener la lista de todos los volcados de memoria disponibles para todas las aplicaciones transferidas localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/dumps


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye una lista de volcados de memoria de cada aplicación transferida localmente.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Obtener la configuración de la colección de volcado de memoria de una aplicación

**Solicitud**

Puedes obtener la configuración de colección de volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Eliminar un volcado de memoria de una aplicación transferida localmente

**Solicitud**

Puedes eliminar un volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
fileName   | (**obligatorio**) Nombre del archivo de volcado de memoria que se debe eliminar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Deshabilitar volcados de memoria de una aplicación transferida localmente

**Solicitud**

Puedes deshabilitar los volcados de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Descargar el volcado de memoria de una aplicación transferida localmente

**Solicitud**

Puedes descargar un volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/crashdump


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
fileName   | (**obligatorio**) Nombre del archivo de volcado de memoria que quieres descargar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye un archivo de volcado de memoria. Puedes usar WinDbg o Visual Studio para examinar el archivo de volcado de memoria.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Habilitar volcados de memoria de una aplicación transferida localmente

**Solicitud**

Puedes habilitar los volcados de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Obtener la lista de archivos de comprobación de errores

**Solicitud**

Puedes obtener la lista de archivos de minivolcado de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/kernel/dumplist


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye una lista de nombres de archivo de volcado de memoria y los tamaños de estos archivos.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Descargar un archivo de volcado de memoria de comprobación de errores

**Solicitud**

Puedes descargar un archivo de volcado de memoria de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/kernel/dump


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
filename   | (**obligatorio**) Nombre del archivo de volcado de memoria. Puedes encontrarlo mediante la API para obtener la lista de volcado de memoria.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye el archivo de volcado de memoria. Puedes inspeccionar este archivo con WinDbg.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Obtener la configuración de control de bloqueo de comprobación de errores

**Solicitud**

Puedes obtener la configuración del control de bloqueo de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye la configuración de control de bloqueo. Para obtener más información sobre CrashControl, consulta el artículo [CrashControl](https://technet.microsoft.com/library/cc951703.aspx).

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Obtener un volcado de memoria de kernel dinámico

**Solicitud**

Puedes obtener un volcado de memoria de kernel dinámico mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/livekernel


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye el volcado de memoria de modo kernel completo. Puedes inspeccionar este archivo con WinDbg.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Obtener un volcado de memoria de un proceso de usuario dinámico

**Solicitud**

Puedes obtener el volcado de memoria del proceso de usuario dinámico mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/live


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
pid   | (**obligatorio**) Identificador de proceso único para el proceso que te interesa.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye del volcado de memoria del proceso. Puedes inspeccionar este archivo con WinDbg o Visual Studio.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Establecer la configuración de control de bloqueo de comprobación de errores

**Solicitud**

Puedes establecer la configuración para recopilar datos de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
autoreboot   | (**opcional**) True o false. Esto indica si el sistema se reinicia automáticamente después de que se produzca un error o se bloquee.
dumptype   | (**opcional**) Tipo de volcado de memoria. Para obtener los valores admitidos, consulta [CrashDumpType Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**opcional**) Número máximo de volcados de memoria que se deben guardar.
sobrescribir   | (**opcional**) True o false. Indica si se deben sobrescribir o no los volcados de memoria antiguos cuando se alcanza el límite del contador de volcado de memoria especificado por *maxdumpcount*.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
## ETW
---
### Crear una sesión ETW en tiempo real a través de un WebSocket

**Solicitud**

Puedes crear una sesión ETW en tiempo real mediante el siguiente formato de solicitud. Se administrará a través de un websocket.
 
Método      | URI de solicitud
:------     | :-----
GET/WebSocket | /api/etw/session/realtime


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye los eventos ETW de los proveedores habilitados.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Enumerar los proveedores de ETW registrados

**Solicitud**

Puedes enumerar los proveedores registrados con el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/etw/providers


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye la lista de proveedores de ETW. La lista incluirá el nombre descriptivo y el GUID de cada proveedor.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
## Redes
---
### Obtener la configuración IP actual

**Solicitud**

Puedes obtener la configuración IP actual mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/networking/ipconfig


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye la configuración IP

**Código de estado**

En la siguiente tabla se muestran los códigos de estado posibles adicionales que pueden devolverse como resultado de esta operación.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La operación se completó correctamente.
500 | Se produjo un error interno del servidor

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Información del SO
---
### Obtener el nombre de la máquina

**Solicitud**

Puedes obtener el nombre de una máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/os/machinename


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

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Obtener la información del sistema operativo

**Solicitud**

Puedes obtener la información del sistema operativo de una máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/os/info


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

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Establecer el nombre de la máquina

**Solicitud**

Puedes establecer el nombre de una máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/os/machinename


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
nombre | (**obligatorio**) Nuevo nombre de la máquina.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Datos de rendimiento
---
### Obtener la lista de procesos en ejecución

**Solicitud**

Puedes obtener la lista de procesos que se encuentran en ejecución actualmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/resourcemanager/processes


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye una lista de procesos con detalles de cada proceso. La información está en formato JSON.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Obtener las estadísticas de rendimiento del sistema

**Solicitud**

Puedes obtener las estadísticas de rendimiento del sistema mediante el siguiente formato de solicitud. Esto incluye información como, por ejemplo, leer y escribir ciclos y la cantidad de memoria que se ha usado.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/resourcemanager/systemperf


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta incluye las estadísticas de rendimiento del sistema, como el uso de la CPU y GPU, el acceso a la memoria y el acceso a la red. Esta información está en formato JSON.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Energía
---
### Obtener el estado actual de la batería

**Solicitud**

Puedes obtener el estado actual de la batería mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/battery


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Obtener el plan de energía activo

**Solicitud**

Puedes obtener el plan de energía activo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/activecfg


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Obtener el subvalor de un plan de energía

**Solicitud**

Puedes obtener el subvalor de un plan de energía mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Obtener el estado de energía del sistema

**Solicitud**

Puedes comprobar el estado de energía del sistema mediante el siguiente formato de solicitud. Esto te permitirá comprobar si se encuentra en un estado de energía bajo.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/state


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Obtener un informe de estudio de suspensión

**Solicitud**

Puedes obtener un informe de estudio de suspensión mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/sleepstudy/reports


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
FileName | (**obligatorio**) Nombre de archivo del informe de estudio de suspensión que quieres descargar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Establecer el plan de energía activo

**Solicitud**

Puedes establecer el plan de energía activo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/power/activecfg


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
scheme | (**obligatorio**) GUID del esquema que quiere establecer como plan de energía activo para el sistema.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Establecer el subvalor de un plan de energía

**Solicitud**

Puedes establecer el subvalor de un plan de energía mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
valueAC | (**obligatorio**) Valor que se usará para alimentación CA.
valueDC | (**obligatorio**) Valor que se usará para la energía de la batería.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Enumerar los informes de estudio de suspensión disponibles

**Solicitud**

Puedes enumerar los informes de estudio de suspensión disponibles mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/sleepstudy/reports


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### Obtener la transformación de estudio de suspensión

**Solicitud**

Puedes obtener la transformación de estudio de suspensión mediante el siguiente formato de solicitud. Esta transformación es de tipo XSLT que convierte el informe de estudio de suspensión en un formato XML que puede leer una persona.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/sleepstudy/reports


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
## Control remoto
---
### Reiniciar el equipo de destino

**Solicitud**

Puedes reiniciar el equipo de destino mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/control/restart


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

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Apagar el equipo de destino

**Solicitud**

Puedes apagar el equipo de destino mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/control/shutdown


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

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Administrador de tareas
---
### Iniciar una aplicación moderna

**Solicitud**

Puedes iniciar una aplicación moderna mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/taskmanager/app


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
appid   | (**obligatorio**) PRAID de la aplicación que quieres iniciar. Este valor debe estar codificado mediante hex64.
package   | (**obligatorio**) Nombre completo del paquete de la aplicación que quiere iniciar. Este valor debe estar codificado mediante hex64.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Detener una aplicación moderna

**Solicitud**

Puedes detener una aplicación moderna mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/taskmanager/app


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
package   | (**obligatorio**) Nombre completo de los paquetes de la aplicación que quiere detener. Este valor debe estar codificado mediante hex64.
forcestop   | (**opcional**) Un valor de **yes** indica que el sistema debe forzar la detención de todos los procesos.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Wi-Fi
---
### Enumerar las interfaces de red inalámbrica

**Solicitud**

Puedes enumerar las interfaces de red inalámbrica disponibles mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wifi/interfaces


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Lista de las interfaces inalámbricas disponibles con detalles. Los detalles incluirán elementos como un GUID, una descripción y un nombre descriptivo, entre otros.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Enumerar redes inalámbricas

**Solicitud**

Puedes enumerar la lista de redes inalámbricas en la interfaz especificada mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wifi/networks


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
interfaz   | (**obligatorio**) GUID de la interfaz de red que se usa para buscar redes inalámbricas.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Lista de redes inalámbricas encontradas en la *interfaz* proporcionada. Incluye los detalles de las redes.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Conectar y desconectar en una red Wi-Fi.

**Solicitud**

Puedes conectarte a una red Wi-Fi o desconectarte de ella mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wifi/network


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
interfaz   | (**obligatorio**) GUID de la interfaz de red que se usa para conectarse a la red.
op   | (**obligatorio**) Indica la acción que se debe realizar. Los valores posibles son connect o disconnect.
ssid   | (**obligatorio si *op* == conectar**) SSID al que se debe conectar.
key   | (**obligatorio si *op* == conectar**) Clave compartida.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### Eliminar un perfil de Wi-Fi

**Solicitud**

Puedes eliminar un perfil asociado con una red en una interfaz específica mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/wifi/network


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
interface   | (**obligatorio**) GUID de la interfaz de red asociado con el perfil que se debe eliminar.
profile   | (**obligatorio**) Nombre del perfil que se debe eliminar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## Informe de errores de Windows (WER)
---
### Descargar un archivo de Informe de errores de Windows (WER)

**Solicitud**

Puedes descargar un archivo WER mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wer/reports/file


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
user   | (**obligatorio**) Nombre de usuario asociado con el informe.
type   | (**obligatorio**) Tipo de informe. Esto se puede **consultar** o **archivar**.
name   | (**obligatorio**) Nombre del informe.
file   | (**obligatorio**) Nombre del archivo que se debe descargar del informe.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Enumerar archivos en un Informe de errores de Windows (WER)

**Solicitud**

Puedes enumerar los archivos en un informe WER mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wer/reports/files


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
user   | (**obligatorio**) Usuario asociado con el informe.
type   | (**obligatorio**) Tipo de informe. Esto se puede **consultar** o **archivar**.
name   | (**obligatorio**) Nombre del informe.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### Enumerar en una lista los Informes de errores de Windows (WER)

**Solicitud**

Puedes obtener los informes WER mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wer/reports


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

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### Iniciar el seguimiento con un perfil personalizado

**Solicitud**

Puedes cargar un perfil WPR e iniciar un seguimiento con dicho perfil mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wpr/customtrace


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Un cuerpo HTTP que conforma varias partes que contiene el perfil WPR personalizado.

**Respuesta**

- Devuelve el estado de sesión WPR.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Iniciar una sesión de seguimiento del rendimiento de arranque

**Solicitud**

Puedes iniciar una sesión de seguimiento de WPR de arranque mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wpr/boottrace


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
profile   | (**obligatorio**) Este parámetro es necesario en el inicio. Nombre del perfil que debe iniciar una sesión de seguimiento del rendimiento. Los perfiles posibles se almacenan en perfprofiles/profiles.json.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- En el inicio, esta API devuelve el estado de sesión de WPR.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Detener una sesión de seguimiento del rendimiento de arranque

**Solicitud**

Puedes detener una sesión de seguimiento de WPR de arranque mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/boottrace


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Devuelve el archivo de seguimiento ETL.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Iniciar una sesión de seguimiento del rendimiento

**Solicitud**

Puedes iniciar una sesión de seguimiento de WPR mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wpr/trace


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de solicitud:

Parámetro de URI | Descripción
:---          | :---
profile   | (**obligatorio**) Nombre del perfil que debe iniciar una sesión de seguimiento del rendimiento. Los perfiles posibles se almacenan en perfprofiles/profiles.json.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- En el inicio, esta API devuelve el estado de sesión de WPR.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Detener una sesión de seguimiento del rendimiento

**Solicitud**

Puedes detener una sesión de seguimiento de WPR mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/trace


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Devuelve el archivo de seguimiento ETL.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### Recuperar el estado de una sesión de seguimiento

**Solicitud**

Puedes recuperar el estado de la sesión actual de WPR mediante el siguiente formato de solicitud
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/status


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Estado de la sesión de seguimiento de WPR.

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT


<!--HONumber=Mar16_HO5-->



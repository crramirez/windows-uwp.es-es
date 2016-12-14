---
author: dbirtolo
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referencia de API principal de Device Portal
description: "Obtén información sobre las API de REST principales de Windows Device Portal que puedes usar para acceder a los datos y controlar el dispositivo mediante programación."
translationtype: Human Translation
ms.sourcegitcommit: b4222774dc4b0f9cdcac871311f5ead69c1e70a9
ms.openlocfilehash: 3bacb9ac42e157afaed5e9e0e6438654db03ff28

---

# <a name="device-portal-core-api-reference"></a>Referencia de API principal de Device Portal

Todo lo que contiene Windows Device Portal se basa en las API de REST que puedes usar para acceder a los datos y controlar el dispositivo mediante programación.

## <a name="app-deployment"></a>Implementación de la aplicación

---
### <a name="install-an-app"></a>Instalar una aplicación

**Solicitud**

Puedes instalar una aplicación mediante el siguiente formato de solicitud.

Método      | URI de solicitud
:------     | :-----
POST | /api/app/packagemanager/package
<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
package   | (**obligatorio**) Nombre de archivo del paquete que debe instalarse.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- El archivo .appx o .appxbundle, así como las dependencias necesarias de la aplicación. 
- El certificado que se usa para firmar la aplicación, si el dispositivo es IoT o escritorio de Windows. Otras plataformas no requieren el certificado. 

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Implementar la solicitud aceptada y que se está procesando
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-app-installation-status"></a>Obtener el estado de instalación de la aplicación

**Solicitud**

Puedes obtener el estado de la instalación de una aplicación que está en curso mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/app/packagemanager/state
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | El resultado de la última implementación
204 | La instalación se está ejecutando
404 | No se encontró ninguna acción de instalación
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="uninstall-an-app"></a>Desinstalar una aplicación

**Solicitud**

Puedes desinstalar una aplicación mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/app/packagemanager/package
<br />

**Parámetros de URI**

Parámetro de URI | Descripción
:---          | :---
package   | (**obligatorio**) El elemento PackageFullName (de GET /api/app/packagemanager/packages) de la aplicación de destino

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-installed-apps"></a>Obtener aplicaciones instaladas

**Solicitud**

Puedes obtener una lista de aplicaciones instaladas en el sistema mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/app/packagemanager/packages
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de los paquetes instalados con detalles asociados. La plantilla para esta respuesta es la siguiente.
```
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## <a name="device-manager"></a>Administrador de dispositivos
---
### <a name="get-the-installed-devices-on-the-machine"></a>Obtener los dispositivos instalados en la máquina

**Solicitud**

Puedes obtener una lista de dispositivos que están instalados en la máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/devicemanager/devices
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una matriz JSON de los dispositivos conectados al dispositivo.
``` 
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* IoT

---
## <a name="dump-collection"></a>Colección de volcados de memoria
---
### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>Obtener la lista de todos los volcados de memoria de aplicaciones

**Solicitud**

Puedes obtener la lista de todos los volcados de memoria disponibles para todas las aplicaciones transferidas localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/dumps
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de volcados de memoria de cada aplicación transferida localmente.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>Obtener la configuración de la colección de volcado de memoria de una aplicación

**Solicitud**

Puedes obtener la configuración de colección de volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta tiene el siguiente formato.
```
{"CrashDumpEnabled": bool}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>Eliminar un volcado de memoria de una aplicación transferida localmente

**Solicitud**

Puedes eliminar un volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
fileName   | (**obligatorio**) Nombre del archivo de volcado de memoria que se debe eliminar.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>Deshabilitar volcados de memoria de una aplicación transferida localmente

**Solicitud**

Puedes deshabilitar los volcados de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol

<br />
**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>Descargar el volcado de memoria de una aplicación transferida localmente

**Solicitud**

Puedes descargar un volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/crashdump
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
fileName   | (**obligatorio**) Nombre del archivo de volcado de memoria que quieres descargar.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye un archivo de volcado de memoria. Puedes usar WinDbg o Visual Studio para examinar el archivo de volcado de memoria.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>Habilitar los volcados de memoria de una aplicación transferida localmente

**Solicitud**

Puedes habilitar los volcados de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Correcto
<br />
**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="get-the-list-of-bugcheck-files"></a>Obtener la lista de archivos de comprobación de errores

**Solicitud**

Puedes obtener la lista de archivos de minivolcado de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/kernel/dumplist
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de nombres de archivo de volcado de memoria y los tamaños de estos archivos. Esta lista tendrá el siguiente formato. 
```
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="download-a-bugcheck-dump-file"></a>Descargar un archivo de volcado de memoria de comprobación de errores

**Solicitud**

Puedes descargar un archivo de volcado de memoria de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/kernel/dump
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
filename   | (**obligatorio**) Nombre del archivo de volcado de memoria. Puedes encontrarlo mediante la API para obtener la lista de volcado de memoria.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el archivo de volcado de memoria. Puedes inspeccionar este archivo con WinDbg.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-the-bugcheck-crash-control-settings"></a>Obtener la configuración de control de bloqueo de comprobación de errores

**Solicitud**

Puedes obtener la configuración del control de bloqueo de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol

<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la configuración de control de bloqueo. Para obtener más información sobre CrashControl, consulta el artículo [CrashControl](https://technet.microsoft.com/library/cc951703.aspx). La plantilla para la respuesta es la siguiente.
```
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**Tipos de volcado**

0: Deshabilitado

1: Volcado de memoria completo (recopila toda la memoria en uso)

2: Volcado de memoria de kernel (omite la memoria del modo de usuario)

3: Minivolcado de kernel limitado

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-a-live-kernel-dump"></a>Obtener un volcado de memoria de kernel dinámico

**Solicitud**

Puedes obtener un volcado de memoria de kernel dinámico mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/livekernel
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el volcado de memoria de modo kernel completo. Puedes inspeccionar este archivo con WinDbg.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-a-dump-from-a-live-user-process"></a>Obtener un volcado de memoria de un proceso de usuario dinámico

**Solicitud**

Puedes obtener el volcado de memoria del proceso de usuario dinámico mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/debug/dump/usermode/live
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
pid   | (**obligatorio**) Identificador de proceso único para el proceso que te interesa.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye del volcado de memoria del proceso. Puedes inspeccionar este archivo con WinDbg o Visual Studio.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="set-the-bugcheck-crash-control-settings"></a>Establecer la configuración de control de bloqueo de comprobación de errores

**Solicitud**

Puedes establecer la configuración para recopilar datos de comprobación de errores mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
autoreboot   | (**opcional**) True o false. Esto indica si el sistema se reinicia automáticamente después de que se produzca un error o se bloquee.
dumptype   | (**opcional**) Tipo de volcado de memoria. Para obtener los valores admitidos, consulta [CrashDumpType Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**opcional**) Número máximo de volcados de memoria que se deben guardar.
sobrescribir   | (**opcional**) True o false. Indica si se deben sobrescribir o no los volcados de memoria antiguos cuando se alcanza el límite del contador de volcado de memoria especificado por *maxdumpcount*.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
## <a name="etw"></a>ETW
---
### <a name="create-a-realtime-etw-session-over-a-websocket"></a>Crear una sesión ETW en tiempo real a través de un WebSocket

**Solicitud**

Puedes crear una sesión ETW en tiempo real mediante el siguiente formato de solicitud. Se administrará a través de un websocket.  Los eventos ETW se procesan por lotes en el servidor y se envían al cliente una vez por segundo. 
 
Método      | URI de solicitud
:------     | :-----
GET/WebSocket | /api/etw/session/realtime
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye los eventos ETW de los proveedores habilitados.  Ver los comandos de ETW WebSocket a continuación. 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>Comandos de ETW WebSocket
Estos comandos se envían desde el cliente al servidor.

Comando | Descripción
:----- | :-----
el proveedor *{guid}* habilita *{level}* | Habilita el proveedor marcado con *{guid}* (sin corchetes) en el nivel especificado. *{level}* es un **int** de 1 (mínimo detalle) a 5 (detallado).
el proveedor *{guid}* deshabilita | Deshabilitar el proveedor con la marca *{guid}* (sin corchetes).

Esta respuesta se envía desde el servidor al cliente. Esto se envía como texto y tú obtienes el siguiente formato analizando el JSON.
```
{
    "Events":[
        {
            "Timestamp": int,
            "Provider": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

Los objetos de carga son pares de clave-valor adicionales (string:string) que se proporcionan en el evento ETW original.

Ejemplo:
```
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

---
### <a name="enumerate-the-registered-etw-providers"></a>Enumerar los proveedores de ETW registrados

**Solicitud**

Puedes enumerar los proveedores registrados con el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/etw/providers
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la lista de proveedores de ETW. La lista incluye el nombre descriptivo y el GUID para cada proveedor en el siguiente formato.
```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>Enumerar los proveedores de ETW personalizados expuestos por la plataforma.

**Solicitud**

Puedes enumerar los proveedores registrados con el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/etw/customproviders
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

200 Correcto. La respuesta incluye la lista de proveedores de ETW. La lista incluirá el nombre descriptivo y el GUID de cada proveedor.

```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de estado**

- Códigos de estado estándar.
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
## <a name="os-information"></a>Información del SO
---
### <a name="get-the-machine-name"></a>Obtener el nombre de la máquina

**Solicitud**

Puedes obtener el nombre de una máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/os/machinename
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el nombre del equipo en el siguiente formato. 

```
{"ComputerName": string}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-the-operating-system-information"></a>Obtener la información del sistema operativo

**Solicitud**

Puedes obtener la información del sistema operativo de una máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/os/info
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la información del sistema operativo en el siguiente formato.

```
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-the-device-family"></a>Obtén la familia de dispositivos. 

**Solicitud**

Puedes obtener la familia de dispositivos (Xbox, Teléfono, escritorio, etc.) mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/os/devicefamily
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la familia de dispositivos (SKU - escritorio, Xbox, etc.).

```
{
   "DeviceType" : string
}
```

DeviceType tendrá un aspecto como "Windows.Xbox", "Windows.Desktop", etc. 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="set-the-machine-name"></a>Establecer el nombre de la máquina

**Solicitud**

Puedes establecer el nombre de una máquina mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/os/machinename
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
name | (**obligatorio**) Nuevo nombre de la máquina.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## <a name="performance-data"></a>Datos de rendimiento
---
### <a name="get-the-list-of-running-processes"></a>Obtener la lista de procesos en ejecución

**Solicitud**

Puedes obtener la lista de procesos que se encuentran en ejecución actualmente mediante el siguiente formato de solicitud.  esto puede actualizarse a una conexión WebSocket también, con los mismos datos de JSON que se envían al cliente una vez por segundo. 
 
Método      | URI de solicitud
:------     | :-----
GET | /api/resourcemanager/processes
GET/WebSocket | /api/resourcemanager/processes
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de procesos con detalles de cada proceso. La información está en formato JSON y tiene la siguiente plantilla.
```
{"Processes": [
    {
        "CPUUsage": int,
        "ImageName": string,
        "PageFileUsage": int,
        "PrivateWorkingSet": int,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": int,
        "WorkingSetSize": int
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="get-the-system-performance-statistics"></a>Obtener las estadísticas de rendimiento del sistema

**Solicitud**

Puedes obtener las estadísticas de rendimiento del sistema mediante el siguiente formato de solicitud. Esto incluye información como, por ejemplo, leer y escribir ciclos y la cantidad de memoria que se ha usado.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/resourcemanager/systemperf
GET/WebSocket | /api/resourcemanager/systemperf
<br />
Esto también se pueden actualizar a una conexión WebSocket.  Proporciona los mismos datos de JSON a continuación una vez por segundo. 

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye las estadísticas de rendimiento del sistema, como el uso de la CPU y GPU, el acceso a la memoria y el acceso a la red. Esta información está en formato JSON y tiene la siguiente plantilla.
```
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## <a name="power"></a>Energía
---
### <a name="get-the-current-battery-state"></a>Obtener el estado actual de la batería

**Solicitud**

Puedes obtener el estado actual de la batería mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/battery
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La información del estado actual de la batería se devuelve mediante el siguiente formato.
```
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="get-the-active-power-scheme"></a>Obtener el plan de energía activo

**Solicitud**

Puedes obtener el plan de energía activo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/activecfg
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

El plan de energía activo tiene el siguiente formato.
```
{"ActivePowerScheme": string (guid of scheme)}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-the-sub-value-for-a-power-scheme"></a>Obtener el subvalor de un plan de energía

**Solicitud**

Puedes obtener el subvalor de un plan de energía mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*
<br />
Opciones:
- SCHEME_CURRENT

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

Hay una lista completa de estados de energía disponible por aplicación y la configuración para marcar varios estados de energía, como bajo y crítico. 

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-the-power-state-of-the-system"></a>Obtener el estado de energía del sistema

**Solicitud**

Puedes comprobar el estado de energía del sistema mediante el siguiente formato de solicitud. Esto te permitirá comprobar si se encuentra en un estado de energía bajo.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/state
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La información de estado de energía tiene la siguiente plantilla.
```
{"LowPowerStateAvailable": bool}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="set-the-active-power-scheme"></a>Establecer el plan de energía activo

**Solicitud**

Puedes establecer el plan de energía activo mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/power/activecfg
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
scheme | (**obligatorio**) GUID del esquema que quiere establecer como plan de energía activo para el sistema.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="set-the-sub-value-for-a-power-scheme"></a>Establecer el subvalor de un plan de energía

**Solicitud**

Puedes establecer el subvalor de un plan de energía mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
valueAC | (**obligatorio**) Valor que se usará para alimentación CA.
valueDC | (**obligatorio**) Valor que se usará para la energía de la batería.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-a-sleep-study-report"></a>Obtener un informe de estudio de suspensión

**Solicitud**

Método      | URI de solicitud
:------     | :-----
GET | /api/power/sleepstudy/report
<br />
Puedes obtener un informe de estudio de suspensión mediante el siguiente formato de solicitud.

**Parámetros de URI**
Parámetro de URI | Descripción
:---          | :---
FileName | (**obligatorio**) Nombre completo del archivo que quieres descargar. Este valor debe estar codificado mediante hex64.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta es un archivo que contiene el estudio de suspensión. 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="enumerate-the-available-sleep-study-reports"></a>Enumerar los informes de estudio de suspensión disponibles

**Solicitud**

Puedes enumerar los informes de estudio de suspensión disponibles mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/sleepstudy/reports
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La lista de los informes disponibles tiene la siguiente plantilla.

```
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
### <a name="get-the-sleep-study-transform"></a>Obtener la transformación de estudio de suspensión

**Solicitud**

Puedes obtener la transformación de estudio de suspensión mediante el siguiente formato de solicitud. Esta transformación es de tipo XSLT que convierte el informe de estudio de suspensión en un formato XML que puede leer una persona.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/power/sleepstudy/transform
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta contiene la transformación de estudio de suspensión.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

---
## <a name="remote-control"></a>Control remoto
---
### <a name="restart-the-target-computer"></a>Reiniciar el equipo de destino

**Solicitud**

Puedes reiniciar el equipo de destino mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/control/restart
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="shut-down-the-target-computer"></a>Apagar el equipo de destino

**Solicitud**

Puedes apagar el equipo de destino mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/control/shutdown
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## <a name="task-manager"></a>Administrador de tareas
---
### <a name="start-a-modern-app"></a>Iniciar una aplicación moderna

**Solicitud**

Puedes iniciar una aplicación moderna mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/taskmanager/app
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
appid   | (**obligatorio**) PRAID de la aplicación que quieres iniciar. Este valor debe estar codificado mediante hex64.
package   | (**obligatorio**) Nombre completo del paquete de la aplicación que quiere iniciar. Este valor debe estar codificado mediante hex64.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="stop-a-modern-app"></a>Detener una aplicación moderna

**Solicitud**

Puedes detener una aplicación moderna mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/taskmanager/app
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
package   | (**obligatorio**) Nombre completo de los paquetes de la aplicación que quiere detener. Este valor debe estar codificado mediante hex64.
forcestop   | (**opcional**) Un valor de **yes** indica que el sistema debe forzar la detención de todos los procesos.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## <a name="networking"></a>Redes
---
### <a name="get-the-current-ip-configuration"></a>Obtener la configuración IP actual

**Solicitud**

Puedes obtener la configuración IP actual mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/networking/ipconfig
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la configuración IP en la siguiente plantilla.

```
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

--
### <a name="enumerate-wireless-network-interfaces"></a>Enumerar las interfaces de red inalámbrica

**Solicitud**

Puedes enumerar las interfaces de red inalámbrica disponibles mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wifi/interfaces
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Una lista de las interfaces inalámbricas disponibles con detalles en el siguiente formato.

``` 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="enumerate-wireless-networks"></a>Enumerar redes inalámbricas

**Solicitud**

Puedes enumerar la lista de redes inalámbricas en la interfaz especificada mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wifi/networks
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
interfaz   | (**obligatorio**) GUID de la interfaz de red que se usa para buscar redes inalámbricas, sin corchetes. 
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Lista de redes inalámbricas encontradas en la *interfaz* proporcionada. Esto incluye detalles para las redes en el siguiente formato.

```
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>Conectar y desconectar en una red Wi-Fi.

**Solicitud**

Puedes conectarte a una red Wi-Fi o desconectarte de ella mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wifi/network
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
interfaz   | (**obligatorio**) GUID de la interfaz de red que se usa para conectarse a la red.
op   | (**obligatorio**) Indica la acción que se debe realizar. Los valores posibles son connect o disconnect.
ssid   | (**obligatorio si *op* == connect**) SSID al que se debe conectar.
clave   | (**necesario si *op* == connect and network requires authentication**) La clave compartida.
createprofile | (**obligatorio**) Crea un perfil de la red en el dispositivo.  Esto hará que el dispositivo se conecte a la red automáticamente en el futuro. Esto puede ser **sí** o **no**. 

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="delete-a-wi-fi-profile"></a>Eliminar un perfil de Wi-Fi

**Solicitud**

Puedes eliminar un perfil asociado con una red en una interfaz específica mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/wifi/network
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
interface   | (**obligatorio**) GUID de la interfaz de red asociado con el perfil que se debe eliminar.
profile   | (**obligatorio**) Nombre del perfil que se debe eliminar.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
## <a name="windows-error-reporting-wer"></a>Informe de errores de Windows (WER)
---
### <a name="download-a-windows-error-reporting-wer-file"></a>Descargar un archivo de Informe de errores de Windows (WER)

**Solicitud**

Puedes descargar un archivo relacionado con WER mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wer/report/file
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
user   | (**obligatorio**) Nombre de usuario asociado con el informe.
type   | (**obligatorio**) Tipo de informe. Esto se puede **consultar** o **archivar**.
name   | (**obligatorio**) Nombre del informe. Esto debe estar codificado en Base64. 
file   | (**obligatorio**) Nombre del archivo que se debe descargar del informe. Esto debe estar codificado en Base64. 
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta contiene el archivo solicitado. 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>Enumerar archivos en un Informe de errores de Windows (WER)

**Solicitud**

Puedes enumerar los archivos en un informe WER mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wer/report/files
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
user   | (**obligatorio**) Usuario asociado con el informe.
type   | (**obligatorio**) Tipo de informe. Esto se puede **consultar** o **archivar**.
name   | (**obligatorio**) Nombre del informe. Esto debe estar codificado en Base64. 
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

```
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="list-the-windows-error-reporting-wer-reports"></a>Enumerar en una lista los Informes de errores de Windows (WER)

**Solicitud**

Puedes obtener los informes WER mediante el siguiente formato de solicitud.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wer/reports
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Los informes WER en el siguiente formato.

```
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

---
## <a name="windows-performance-recorder-wpr"></a>Windows Performance Recorder (WPR) 
---
### <a name="start-tracing-with-a-custom-profile"></a>Iniciar el seguimiento con un perfil personalizado

**Solicitud**

Puedes cargar un perfil WPR e iniciar un seguimiento con dicho perfil mediante el siguiente formato de solicitud.  Solo se puede ejecutar un seguimiento cada vez. El perfil no permanecerá en el dispositivo. 
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wpr/customtrace
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Un cuerpo HTTP que conforma varias partes que contiene el perfil WPR personalizado.

**Respuesta**

El estado de sesión WPR en el siguiente formato.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="start-a-boot-performance-tracing-session"></a>Iniciar una sesión de seguimiento del rendimiento de arranque

**Solicitud**

Puedes iniciar una sesión de seguimiento de WPR de arranque mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wpr/boottrace
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
profile   | (**obligatorio**) Este parámetro es necesario en el inicio. Nombre del perfil que debe iniciar una sesión de seguimiento del rendimiento. Los perfiles posibles se almacenan en perfprofiles/profiles.json.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

En Inicio, esta API devuelve el estado de sesión WPR en el siguiente formato.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="stop-a-boot-performance-tracing-session"></a>Detener una sesión de seguimiento del rendimiento de arranque

**Solicitud**

Puedes detener una sesión de seguimiento de WPR de arranque mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/boottrace
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

-  Ninguna.  **Nota:** Esta es una operación de larga duración.  Devolverá cuando el archivo ETL haya terminado de escribirse en disco.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="start-a-performance-tracing-session"></a>Iniciar una sesión de seguimiento del rendimiento

**Solicitud**

Puedes iniciar una sesión de seguimiento de WPR mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.  Solo se puede ejecutar un seguimiento cada vez. 
 
Método      | URI de solicitud
:------     | :-----
POST | /api/wpr/trace
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
profile   | (**obligatorio**) Nombre del perfil que debe iniciar una sesión de seguimiento del rendimiento. Los perfiles posibles se almacenan en perfprofiles/profiles.json.
<br />
**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

En Inicio, esta API devuelve el estado de sesión WPR en el siguiente formato.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="stop-a-performance-tracing-session"></a>Detener una sesión de seguimiento del rendimiento

**Solicitud**

Puedes detener una sesión de seguimiento de WPR mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/trace
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguna.  **Nota:** Esta es una operación de larga duración.  Devolverá cuando el archivo ETL haya terminado de escribirse en disco.  

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="retrieve-the-status-of-a-tracing-session"></a>Recuperar el estado de una sesión de seguimiento

**Solicitud**

Puedes recuperar el estado de la sesión actual de WPR mediante el siguiente formato de solicitud
 
Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/status
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

El estado de la sesión de seguimiento de WPR en el siguiente formato.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="list-completed-tracing-sessions-etls"></a>Lista de sesiones de seguimiento completadas (ETL)

**Solicitud**

Puedes obtener una lista del seguimientos de ETL en el dispositivo mediante el siguiente formato de solicitud. 

Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/tracefiles
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La lista de sesiones de seguimiento completadas se proporciona en el siguiente formato.

```
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="download-a-tracing-session-etl"></a>Descargar una sesión de seguimiento (ETL)

**Solicitud**

Puedes descargar un archivo de seguimiento (seguimiento de arranque o el seguimiento de modo de usuario) mediante el siguiente formato de solicitud. 

Método      | URI de solicitud
:------     | :-----
GET | /api/wpr/tracefile
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
filename   | (**obligatorio**) El nombre del seguimiento ETL para descargar.  Los encontrarás en /api/wpr/tracefiles

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Devuelve el archivo de seguimiento ETL.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
### <a name="delete-a-tracing-session-etl"></a>Eliminar una sesión de seguimiento (ETL)

**Solicitud**

Puedes eliminar un archivo de seguimiento (seguimiento de arranque o el seguimiento de modo de usuario) mediante el siguiente formato de solicitud. 

Método      | URI de solicitud
:------     | :-----
DELETE | /api/wpr/tracefile
<br />

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro de URI | Descripción
:---          | :---
filename   | (**obligatorio**) Nombre del seguimiento ETL que se debe eliminar.  Los encontrarás en /api/wpr/tracefiles

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Devuelve el archivo de seguimiento ETL.

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

---
## <a name="dns-sd-tags"></a>Etiquetas de DNS-SD 
---
### <a name="view-tags"></a>Ver etiquetas

**Solicitud**

Te permite ver las etiquetas aplicadas actualmente para el dispositivo.  Estas se anuncian a través de los registros TXT de DNS-SD en la tecla T.  
 
Método      | URI de solicitud
:------     | :-----
GET | /api/dns-sd/tags
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta** Las etiquetas aplicadas actualmente con el siguiente formato. 
```
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
5XX | Error del servidor 

<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="delete-tags"></a>Eliminar etiquetas

**Solicitud**

Elimina todas las etiquetas anunciadas actualmente por DNS-SD.   
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/dns-sd/tags
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**
 - Ninguna

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
5XX | Error del servidor 

<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

---
### <a name="delete-tag"></a>Eliminar etiqueta

**Solicitud**

Elimina una etiqueta anunciada actualmente por DNS-SD.   
 
Método      | URI de solicitud
:------     | :-----
DELETE | /api/dns-sd/tag
<br />

**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
tagValue | (**obligatorio**) La etiqueta que se debe quitar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**
 - Ninguna

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar

<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT
 
---
### <a name="add-a-tag"></a>Agregar una etiqueta

**Solicitud**

Agrega una etiqueta al anuncio de DNS-SD.   
 
Método      | URI de solicitud
:------     | :-----
POST | /api/dns-sd/tag
<br />

**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
tagValue | (**obligatorio**) La etiqueta que se debe agregar.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**
 - Ninguna

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
401 | Desbordamiento del espacio de la etiqueta.  Se produce si la etiqueta propuesta es demasiado larga para el registro de servicio de DNS-SD resultante.  

<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>Explorador de archivos de la aplicación

---
### <a name="get-known-folders"></a>Obtener carpetas conocidas

**Solicitud**

Obtén una lista de carpetas accesibles de nivel superior.

Método      | URI de solicitud
:------     | :-----
GET | /api/filesystem/apps/knownfolders
<br />

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta** Las carpetas disponibles con el siguiente formato. 
```
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Implementar la solicitud aceptada y que se está procesando
4XX | Códigos de error
5XX | Códigos de error
<br />

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

---
### <a name="get-files"></a>Obtener archivos

**Solicitud**

Obtén una lista de los archivos que hay en una carpeta.

Método      | URI de solicitud
:------     | :-----
GET | /api/filesystem/apps/files
<br />

**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
knownfolderid | (**obligatorio**) El directorio de nivel superior donde quieres la lista de archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. 
packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. 
path | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. 

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta** Las carpetas disponibles con el siguiente formato. 
```
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

---
### <a name="download-a-file"></a>Descargar un archivo

**Solicitud**

Obtener un archivo de una carpeta conocida o appLocalData.

Método      | URI de solicitud
:------     | :-----
GET | /api/filesystem/apps/file

**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
knownfolderid | (**obligatorio**) El directorio de nivel superior donde deseas descargar los archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. 
filename | (**obligatorio**) El nombre del archivo que se va a descargar. 
packagefullname | (**obligatorio si *knownfolderid* == LocalAppData **) El nombre completo del paquete que te interesa. 
path | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- El archivo solicitado, si se encuentra presente.

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | El archivo solicitado
404 | Archivo no encontrado
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

---
### <a name="rename-a-file"></a>Cambiar el nombre de un archivo

**Solicitud**

Cambiar el nombre de un archivo de una carpeta.

Método      | URI de solicitud
:------     | :-----
POST | /api/filesystem/apps/rename

<br />
**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
knownfolderid | (**obligatorio**) El directorio de nivel superior en el que se encuentra el archivo. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. 
filename | (**obligatorio**) El nombre original del archivo cuyo nombre se va a cambiar. 
newfilename | (**obligatorio**) El nuevo nombre del archivo.
packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. 
path | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. 

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguna

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar. Se cambia el nombre del archivo.
404 | Archivo no encontrado
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

---
### <a name="delete-a-file"></a>Eliminar un archivo

**Solicitud**

Elimina un archivo de una carpeta.

Método      | URI de solicitud
:------     | :-----
DELETE | /api/filesystem/apps/file
<br />
**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
knownfolderid | (**obligatorio**) El directorio de nivel superior del que quieres eliminar los archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. 
filename | (**obligatorio**) El nombre del archivo que se va a eliminar. 
packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. 
path | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguna 

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar. Se elimina el archivo.
404 | Archivo no encontrado
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

---
### <a name="upload-a-file"></a>Cargar un archivo

**Solicitud**

Carga un archivo en una carpeta.  Esto sobrescribirá cualquier archivo existente que tenga el mismo nombre, pero no creará carpetas nuevas. 

Método      | URI de solicitud
:------     | :-----
POST | /api/filesystem/apps/file
<br />
**Parámetros de URI**

Parámetro de URI | Descripción
:------     | :-----
knownfolderid | (**obligatorio**) El directorio de nivel superior donde quieres cargar los archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente.
packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. 
path | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente.

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Aceptar. El archivo se carga.
4XX | Códigos de error
5XX | Códigos de error
<br />
**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT


<!--HONumber=Dec16_HO1-->



---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referencia de API principal de Device Portal
description: Obtén información sobre las API de REST principales de Windows Device Portal que puedes usar para acceder a los datos y controlar el dispositivo mediante programación.
ms.date: 03/22/2017
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 5f541a27a96b88b37d3f32b98246ba0ccbe2c8cf
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59067860"
---
# <a name="device-portal-core-api-reference"></a>Referencia de API principal de Device Portal

Toda la funcionalidad del Portal de dispositivos se basa en las API de REST que los desarrolladores pueden llamar directamente para acceder a recursos y controlar sus dispositivos mediante programación.

## <a name="app-deployment"></a>Implementación de la aplicación

### <a name="install-an-app"></a>Instalar una aplicación

**Solicitud**

Puedes instalar una aplicación mediante el siguiente formato de solicitud.

| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/app/packagemanager/package |

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| paquete   | (**obligatorio**) Nombre de archivo del paquete que debe instalarse. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- El archivo .appx o .appxbundle, así como las dependencias necesarias de la aplicación. 
- El certificado que se usa para firmar la aplicación, si el dispositivo es IoT o escritorio de Windows. Otras plataformas no requieren el certificado. 

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
| 200 | Implementar la solicitud aceptada y que se está procesando |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Instalar un conjunto relacionado

**Solicitud**

Puedes instalar un [conjunto relacionado](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) mediante el siguiente formato de solicitud.

| Método      | URI de la solicitud |
| :------     | :------ |
| EXPONER | /api/app/packagemanager/package |

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| paquete   | (**obligatorio**) Los nombres de archivo de paquetes que deben instalarse. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud** 
- Agregue ".opt" a los nombres de archivo de los paquetes opcionales cuando los especifique como un parámetro, por ejemplo: "foo.appx.opt" o "bar.appxbundle.opt". 
- El archivo .appx o .appxbundle, así como las dependencias necesarias de la aplicación. 
- El certificado que se usa para firmar la aplicación, si el dispositivo es IoT o escritorio de Windows. Otras plataformas no requieren el certificado. 

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
| 200 | Implementar la solicitud aceptada y que se está procesando |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Registrar una aplicación en una carpeta dinámica

**Solicitud**

Puede registrar una aplicación en una carpeta dinámica mediante el siguiente formato de solicitud.

| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/app/packagemanager/networkapp |

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    }
}
```

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
| 200 | Implementar la solicitud aceptada y que se está procesando |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Registrar un conjunto relacionado en carpetas dinámicas

**Solicitud**

Puede registrar un [conjunto relacionado](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) en carpetas dinámicas mediante el siguiente formato de solicitud.

| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/app/packagemanager/networkapp |

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    },
    "optionalpackages" :
    [
        {
            "networkshare" : "\\some\share\path2",
            "username" : "optional_username2",
            "password" : "optional_password2"
        },
        ...
    ]
}
```

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
| 200 | Implementar la solicitud aceptada y que se está procesando |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtener el estado de instalación de la aplicación

**Solicitud**

Puedes obtener el estado de la instalación de una aplicación que está en curso mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/app/packagemanager/state |

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
| 200 | El resultado de la última implementación |
| 204 | La instalación se está ejecutando |
| 404 | No se encontró ninguna acción de instalación |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Desinstalar una aplicación

**Solicitud**

Puedes desinstalar una aplicación mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/app/packagemanager/package |

**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------          | :------ |
| paquete   | (**obligatorio**) El elemento PackageFullName (de GET /api/app/packagemanager/packages) de la aplicación de destino |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtener aplicaciones instaladas

**Solicitud**

Puedes obtener una lista de aplicaciones instaladas en el sistema mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/app/packagemanager/packages |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de los paquetes instalados con detalles asociados. La plantilla para esta respuesta es la siguiente.
```json
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

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
## Bluetooth
<hr>

### <a name="get-the-bluetooth-radios-on-the-machine"></a>Obtener las radios Bluetooth en la máquina

**Solicitud**

Puedes obtener una lista de radios Bluetooth que están instaladas en la máquina mediante el siguiente formato de solicitud. Esto se puede actualizar a una conexión de WebSocket, también con los mismos datos JSON.
 
| Método        | URI de la solicitud |
| :------          | :------ |
| GET           | /api/bt/getradios |
| GET/WebSocket | /api/bt/getradios |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una matriz JSON de las radios Bluetooth conectadas al dispositivo.
```json
{"BluetoothRadios" : [
    {
        "BluetoothAddress" : int64,
        "DisplayName" : string,
        "HasUnknownUsbDevice" : boolean,
        "HasProblem" : boolean,
        "ID" : string,
        "ProblemCode" : int,
        "State" : string
    },...
]}
```
**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP | Descripción |
| :------             | :------ |
| 200              | Aceptar |
| 4XX              | Códigos de error |
| 5XX              | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Activa o desactiva la radio Bluetooth.

**Solicitud**

Activa o desactivar una radio Bluetooth específica.
 
| Método | URI de la solicitud |
| :------   | :------ |
| EXPONER   | /api/bt/setradio |

**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| Id.            | (**obligatorio**) El id. de dispositivo de la radio Bluetooth y debe codificarse con base 64. |
| Estado         | (**requiere**) puede ser `"On"` o `"Off"`. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP | Descripción |
| :------             | :------ |
| 200              | Aceptar |
| 4XX              | Códigos de error |
| 5XX              | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
## Administrador de dispositivos
<hr>
### Obtener los dispositivos instalados en la máquina

**Solicitud**

Puedes obtener una lista de dispositivos que están instalados en la máquina mediante el siguiente formato de solicitud.

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/devicemanager/devices |

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una matriz JSON de los dispositivos conectados al dispositivo.
```json
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

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* IoT

<hr>
### Obtener datos de dispositivos o concentradores USB conectados

**Solicitud**

Puedes obtener una lista de los descriptores USB de los dispositivos y concentradores USB conectados mediante el siguiente formato de solicitud.

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /ext/devices/usbdevices |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta es un archivo JSON que incluye el DeviceID del dispositivo USB junto con los descriptores USB y la información de puerto de los concentradores.
```json
{
    "DeviceList": [
        {
        "ID": string,
        "ParentID": string, // Will equal an "ID" within the list, or be blank
        "Description": string, // optional
        "Manufacturer": string, // optional
        "ProblemCode": int, // optional
        "StatusCode": int // optional
        },
        ...
    ]
}
```

**Ejemplo de datos devueltos**
```json
{
    "DeviceList": [{
        "ID": "System",
        "ParentID": ""
    }, {
        "Class": "USB",
        "Description": "Texas Instruments USB 3.0 xHCI Host Controller",
        "ID": "PCI\\VEN_104C&DEV_8241&SUBSYS_1589103C&REV_02\\4&37085792&0&00E7",
        "Manufacturer": "Texas Instruments",
        "ParentID": "System",
        "ProblemCode": 0,
        "StatusCode": 25174026
    }, {
        "Class": "USB",
        "Description": "USB Composite Device",
        "DeviceDriverKey": "{36fc9e60-c465-11cf-8056-444553540000}\\0016",
        "ID": "USB\\VID_045E&PID_00DB\\8&2994096B&0&1",
        "Manufacturer": "(Standard USB Host Controller)",
        "ParentID": "USB\\VID_0557&PID_8021\\7&2E9A8711&0&4",
        "ProblemCode": 0,
        "StatusCode": 25182218
    }]
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
## Colección de volcados de memoria
<hr>
### Obtener la lista de todos los volcados de memoria de aplicaciones

**Solicitud**

Puedes obtener la lista de todos los volcados de memoria disponibles para todas las aplicaciones transferidas localmente mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/usermode/dumps |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de volcados de memoria de cada aplicación transferida localmente.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Obtener la configuración de la colección de volcado de memoria de una aplicación

**Solicitud**

Puedes obtener la configuración de colección de volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashcontrol |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta tiene el siguiente formato.
```json
{"CrashDumpEnabled": bool}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Eliminar un volcado de memoria de una aplicación transferida localmente

**Solicitud**

Puedes eliminar un volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/debug/dump/usermode/crashdump |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente. |
| fileName   | (**obligatorio**) Nombre del archivo de volcado de memoria que se debe eliminar. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Deshabilitar volcados de memoria de una aplicación transferida localmente

**Solicitud**

Puedes deshabilitar los volcados de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/debug/dump/usermode/crashcontrol |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Descargar el volcado de memoria de una aplicación transferida localmente

**Solicitud**

Puedes descargar un volcado de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashdump |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente. |
| fileName   | (**obligatorio**) Nombre del archivo de volcado de memoria que quieres descargar. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye un archivo de volcado de memoria. Puedes usar WinDbg o Visual Studio para examinar el archivo de volcado de memoria.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Habilitar los volcados de memoria de una aplicación transferida localmente

**Solicitud**

Puedes habilitar los volcados de memoria de una aplicación transferida localmente mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/debug/dump/usermode/crashcontrol |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| packageFullname   | (**obligatorio**) Nombre completo del paquete de la aplicación transferida localmente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 

**Familias de dispositivos disponibles**

* Windows Mobile (en el Programa Windows Insider)
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Obtener la lista de archivos de comprobación de errores

**Solicitud**

Puedes obtener la lista de archivos de minivolcado de comprobación de errores mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dumplist |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de nombres de archivo de volcado de memoria y los tamaños de estos archivos. Esta lista tendrá el siguiente formato. 
```json
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Descargar un archivo de volcado de memoria de comprobación de errores

**Solicitud**

Puedes descargar un archivo de volcado de memoria de comprobación de errores mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dump |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| nombreDeArchivo   | (**obligatorio**) Nombre del archivo de volcado de memoria. Puedes encontrarlo mediante la API para obtener la lista de volcado de memoria. |


**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el archivo de volcado de memoria. Puedes inspeccionar este archivo con WinDbg.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener la configuración de control de bloqueo de comprobación de errores

**Solicitud**

Puedes obtener la configuración del control de bloqueo de comprobación de errores mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/kernel/crashcontrol |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la configuración de control de bloqueo. Para obtener más información sobre CrashControl, consulta el artículo [CrashControl](https://technet.microsoft.com/library/cc951703.aspx). La plantilla para la respuesta es la siguiente.
```json
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**Tipos de volcado**

0: Deshabilitada

1: Volcado de memoria completo (recopila toda la memoria en uso)

2: Volcado de memoria del kernel (ignora la memoria en modo de usuario)

3: Minivolcado kernel limitado

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener un volcado de memoria de kernel dinámico

**Solicitud**

Puedes obtener un volcado de memoria de kernel dinámico mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/livekernel |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el volcado de memoria de modo kernel completo. Puedes inspeccionar este archivo con WinDbg.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener un volcado de memoria de un proceso de usuario dinámico

**Solicitud**

Puedes obtener el volcado de memoria del proceso de usuario dinámico mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/debug/dump/usermode/live |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| pid   | (**obligatorio**) Identificador de proceso único para el proceso que te interesa. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye del volcado de memoria del proceso. Puedes inspeccionar este archivo con WinDbg o Visual Studio.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Establecer la configuración de control de bloqueo de comprobación de errores

**Solicitud**

Puedes establecer la configuración para recopilar datos de comprobación de errores mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/debug/dump/kernel/crashcontrol |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| autoreboot   | (**opcional**) True o false. Esto indica si el sistema se reinicia automáticamente después de que se produzca un error o se bloquee. |
| dumptype   | (**opcional**) Tipo de volcado de memoria. Para obtener los valores admitidos, consulta [CrashDumpType Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).|
| maxdumpcount   | (**opcional**) Número máximo de volcados de memoria que se deben guardar. |
| sobrescribir   | (**opcional**) True o false. Indica si se deben sobrescribir o no los volcados de memoria antiguos cuando se alcanza el límite del contador de volcado de memoria especificado por *maxdumpcount*. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
## ETW
<hr>
### Crear una sesión ETW en tiempo real a través de un WebSocket

**Solicitud**

Puedes crear una sesión ETW en tiempo real mediante el siguiente formato de solicitud. Se administrará a través de un websocket.  Los eventos ETW se procesan por lotes en el servidor y se envían al cliente una vez por segundo. 
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET/WebSocket | /api/etw/session/realtime |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye los eventos ETW de los proveedores habilitados.  Ver los comandos de ETW WebSocket a continuación. 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>Comandos de ETW WebSocket
Estos comandos se envían desde el cliente al servidor.

| Comando | Descripción |
| :----- | :----- |
| el proveedor *{guid}* habilita *{level}* | Habilita el proveedor marcado con *{guid}* (sin corchetes) en el nivel especificado. *{level}* es un **int** de 1 (mínimo detalle) a 5 (detallado). |
| el proveedor *{guid}* deshabilita | Deshabilitar el proveedor con la marca *{guid}* (sin corchetes). |

Esta respuesta se envía desde el servidor al cliente. Esto se envía como texto y tú obtienes el siguiente formato analizando el JSON.
```json
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
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

Por ejemplo:
```json
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

<hr>
### Enumerar los proveedores de ETW registrados

**Solicitud**

Puedes enumerar los proveedores registrados con el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/etw/providers |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la lista de proveedores de ETW. La lista incluye el nombre descriptivo y el GUID para cada proveedor en el siguiente formato.
```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
|  200 | Aceptar | 

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Enumerar los proveedores de ETW personalizados expuestos por la plataforma.

**Solicitud**

Puedes enumerar los proveedores registrados con el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/etw/customproviders |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

200 Correcto. La respuesta incluye la lista de proveedores de ETW. La lista incluirá el nombre descriptivo y el GUID de cada proveedor.

```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de estado**

- Códigos de estado estándar.

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
## Ubicación
<hr>

### <a name="get-location-override-mode"></a>Obtener el modo de invalidación de ubicación

**Solicitud**

Puedes obtener el estado de invalidación de la pila de ubicaciones del dispositivo mediante el siguiente formato de solicitud. El modo de desarrollador debe estar activado para que esta llamada pueda tener éxito.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /ext/location/override |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el estado de invalidación del dispositivo en el siguiente formato. 

```json
{"Override" : bool}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
|  200 | Aceptar | 
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

### <a name="set-location-override-mode"></a>Establecer el modo de invalidación de ubicación

**Solicitud**

Puedes establecer el estado de invalidación de la pila de ubicaciones del dispositivo mediante el siguiente formato de solicitud. Cuando está habilitada, la pila de ubicaciones permite la inyección de posición. El modo de desarrollador debe estar activado para que esta llamada pueda tener éxito.

| Método      | URI de la solicitud |
| :------     | :----- |
| PUT | /ext/location/override |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

```json
{"Override" : bool}
```

**Respuesta**

La respuesta incluye el estado de invalidación que ha establecido el dispositivo en el siguiente formato. 

```json
{"Override" : bool}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

### <a name="get-the-injected-position"></a>Obtener la posición inyectada

**Solicitud**

Puedes obtener la ubicación inyectada del dispositivo (sin precisión) mediante el siguiente formato de solicitud. Se debe establecer una ubicación inyectada, o se producirá un error.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /ext/location/position |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye los valores de longitud y latitud inyectados en el siguiente formato. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

|  Código de estado HTTP      | Descripción | 
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

### <a name="set-the-injected-position"></a>Establecer la posición inyectada

**Solicitud**

Puedes establecer la ubicación inyectada del dispositivo (sin precisión) mediante el siguiente formato de solicitud. Primero debe estar habilitado el modo de invalidación de ubicación en el dispositivo y la ubicación establecida debe ser una ubicación válida, o se producirá un error.

| Método      | URI de la solicitud |
| :------     | :----- |
| PUT | /ext/location/override |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Respuesta**

La respuesta incluye la ubicación que se ha establecido en el siguiente formato. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
## Información del SO
<hr>
### Obtener el nombre de la máquina

**Solicitud**

Puedes obtener el nombre de una máquina mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/os/machinename |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye el nombre del equipo en el siguiente formato. 

```json
{"ComputerName": string}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtener la información del sistema operativo

**Solicitud**

Puedes obtener la información del sistema operativo de una máquina mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/os/info |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la información del sistema operativo en el siguiente formato.

```json
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Obtén la familia de dispositivos. 

**Solicitud**

Puedes obtener la familia de dispositivos (Xbox, Teléfono, escritorio, etc.) mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/os/devicefamily |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la familia de dispositivos (SKU - escritorio, Xbox, etc.).

```json
{
   "DeviceType" : string
}
```

DeviceType tendrá un aspecto como "Windows.Xbox", "Windows.Desktop", etc. 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Establecer el nombre de la máquina

**Solicitud**

Puedes establecer el nombre de una máquina mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/os/machinename |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| NAME | (**obligatorio**) Nuevo nombre de la máquina. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
## Información de usuario
<hr>
### Obtener el usuario activo

**Solicitud**

Puedes obtener el nombre del usuario activo en el dispositivo mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/users/activeuser |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la información del usuario en el siguiente formato. 

Correcto: 
```json
{
    "UserDisplayName" : string, 
    "UserSID" : string
}
```
Error:
```json
{
    "Code" : int, 
    "CodeText" : string, 
    "Reason" : string, 
    "Success" : bool
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
## Datos de rendimiento
<hr>
### Obtener la lista de procesos en ejecución

**Solicitud**

Puedes obtener la lista de procesos que se encuentran en ejecución actualmente mediante el siguiente formato de solicitud.  esto puede actualizarse a una conexión WebSocket también, con los mismos datos de JSON que se envían al cliente una vez por segundo. 
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/resourcemanager/processes |
| GET/WebSocket | /api/resourcemanager/processes |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye una lista de procesos con detalles de cada proceso. La información está en formato JSON y tiene la siguiente plantilla.
```json
{"Processes": [
    {
        "CPUUsage": float,
        "ImageName": string,
        "PageFileUsage": long,
        "PrivateWorkingSet": long,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": long,
        "WorkingSetSize": long
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Obtener las estadísticas de rendimiento del sistema

**Solicitud**

Puedes obtener las estadísticas de rendimiento del sistema mediante el siguiente formato de solicitud. Esto incluye información como, por ejemplo, leer y escribir ciclos y la cantidad de memoria que se ha usado.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/resourcemanager/systemperf |
| GET/WebSocket | /api/resourcemanager/systemperf |

Esto también se pueden actualizar a una conexión WebSocket.  Proporciona los mismos datos de JSON a continuación una vez por segundo. 

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye las estadísticas de rendimiento del sistema, como el uso de la CPU y GPU, el acceso a la memoria y el acceso a la red. Esta información está en formato JSON y tiene la siguiente plantilla.
```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
## Alimentación
<hr>
### Obtener el estado actual de la batería

**Solicitud**

Puedes obtener el estado actual de la batería mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/battery |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La información del estado actual de la batería se devuelve mediante el siguiente formato.
```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Obtener el plan de energía activo

**Solicitud**

Puedes obtener el plan de energía activo mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/activecfg |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

El plan de energía activo tiene el siguiente formato.
```json
{"ActivePowerScheme": string (guid of scheme)}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener el subvalor de un plan de energía

**Solicitud**

Puedes obtener el subvalor de un plan de energía mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/cfg/*<power scheme path>* |

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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener el estado de energía del sistema

**Solicitud**

Puedes comprobar el estado de energía del sistema mediante el siguiente formato de solicitud. Esto te permitirá comprobar si se encuentra en un estado de energía bajo.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/state |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La información de estado de energía tiene la siguiente plantilla.
```json
{"LowPowerState" : false, "LowPowerStateAvailable" : true }
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Establecer el plan de energía activo

**Solicitud**

Puedes establecer el plan de energía activo mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/power/activecfg |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| scheme | (**obligatorio**) GUID del esquema que quiere establecer como plan de energía activo para el sistema. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Establecer el subvalor de un plan de energía

**Solicitud**

Puedes establecer el subvalor de un plan de energía mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/power/cfg/*<power scheme path>* |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| valueAC | (**obligatorio**) Valor que se usará para alimentación CA. |
| valueDC | (**obligatorio**) Valor que se usará para la energía de la batería. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener un informe de estudio de suspensión

**Solicitud**

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/sleepstudy/report |

Puedes obtener un informe de estudio de suspensión mediante el siguiente formato de solicitud.

**Parámetros de URI**
| Parámetro de URI | Descripción |
| :------          | :------ |
| nombreDeArchivo | (**obligatorio**) Nombre completo del archivo que quieres descargar. Este valor debe estar codificado mediante hex64. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta es un archivo que contiene el estudio de suspensión. 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Enumerar los informes de estudio de suspensión disponibles

**Solicitud**

Puedes enumerar los informes de estudio de suspensión disponibles mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/sleepstudy/reports |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La lista de los informes disponibles tiene la siguiente plantilla.

```json
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
### Obtener la transformación de estudio de suspensión

**Solicitud**

Puedes obtener la transformación de estudio de suspensión mediante el siguiente formato de solicitud. Esta transformación es de tipo XSLT que convierte el informe de estudio de suspensión en un formato XML que puede leer una persona.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/power/sleepstudy/transform |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta contiene la transformación de estudio de suspensión.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* IoT

<hr>
## Control remoto
<hr>
### Reiniciar el equipo de destino

**Solicitud**

Puedes reiniciar el equipo de destino mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/control/restart |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Apagar el equipo de destino

**Solicitud**

Puedes apagar el equipo de destino mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/control/shutdown |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
## Administrador de tareas
<hr>
### Iniciar una aplicación moderna

**Solicitud**

Puedes iniciar una aplicación moderna mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/taskmanager/app |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| appid   | (**obligatorio**) PRAID de la aplicación que quieres iniciar. Este valor debe estar codificado mediante hex64. |
| paquete   | (**obligatorio**) Nombre completo del paquete de la aplicación que quiere iniciar. Este valor debe estar codificado mediante hex64. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Detener una aplicación moderna

**Solicitud**

Puedes detener una aplicación moderna mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/taskmanager/app |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :---          | :--- |
| paquete   | (**obligatorio**) Nombre completo de los paquetes de la aplicación que quiere detener. Este valor debe estar codificado mediante hex64. |
| forcestop   | (**opcional**) Un valor de **yes** indica que el sistema debe forzar la detención de todos los procesos. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Eliminar el proceso por PID

**Solicitud**

Puedes eliminar el proceso mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/taskmanager/process |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| pid   | (**obligatorio**) Identificador de proceso único para detener el proceso. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
## Funciones de red
<hr>
### Obtener la configuración IP actual

**Solicitud**

Puedes obtener la configuración IP actual mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/networking/ipconfig |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La respuesta incluye la configuración IP en la siguiente plantilla.

```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Establecer una dirección IP estática (configuración de IPV4)

**Solicitud**

Establece la configuración de IPV4 con estático de direcciones IP y DNS. Si no se especifica una dirección IP estática, habilita DHCP. Si se especifica una dirección IP estática, DNS debe especificarse también.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| PUT | /api/networking/ipv4config |


**Parámetros de URI**

| Parámetro de URI | Descripción |
| :---          | :--- |
| AdapterName | (**requiere**) el GUID de la interfaz de red. |
| IPAddress | Para establecer la dirección IP estática. |
| SubnetMask | (**requiere** si *IPAddress* no es null) la máscara de subred estático. |
| DefaultGateway | (**requiere** si *IPAddress* no es null) la puerta de enlace estática. |
| PrimaryDNS | (**requiere** si *IPAddress* no es null) el DNS principal estático para establecer. |
| SecondayDNS | (**requiere** si *PrimaryDNS* no es null) el DNS estático secundario para establecer. |

Para mayor claridad, para establecer una interfaz a DHCP, serializar solamente el `AdapterName` en la conexión:
```
{
    "AdapterName":"{82F86C1B-2BAE-41E3-B08D-786CA44FEED7}"
}
```

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT
<hr>
### Enumerar las interfaces de red inalámbrica

**Solicitud**

Puedes enumerar las interfaces de red inalámbrica disponibles mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wifi/interfaces |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Una lista de las interfaces inalámbricas disponibles con detalles en el siguiente formato.

```json 
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Enumerar redes inalámbricas

**Solicitud**

Puedes enumerar la lista de redes inalámbricas en la interfaz especificada mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wifi/networks |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| interfaz   | (**obligatorio**) GUID de la interfaz de red que se usa para buscar redes inalámbricas, sin corchetes. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Lista de redes inalámbricas encontradas en la *interfaz* proporcionada. Esto incluye detalles para las redes en el siguiente formato.

```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Conectar y desconectar en una red Wi-Fi.

**Solicitud**

Puedes conectarte a una red Wi-Fi o desconectarte de ella mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/wifi/network |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| interfaz   | (**obligatorio**) GUID de la interfaz de red que se usa para conectarse a la red. |
| op   | (**obligatorio**) Indica la acción que se debe realizar. Los valores posibles son connect o disconnect.|
| ssid   | (**obligatorio si *op* == conectar**) SSID al que se debe conectar. |
| key   | (**obligatorio si *op* == connect and network requires authentication**) La clave compartida. |
| createprofile | (**obligatorio**) Crea un perfil de la red en el dispositivo.  Esto hará que el dispositivo se conecte a la red automáticamente en el futuro. Esto puede ser **sí** o **no**. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Eliminar un perfil de Wi-Fi

**Solicitud**

Puedes eliminar un perfil asociado con una red en una interfaz específica mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/wifi/profile |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| interfaz   | (**obligatorio**) GUID de la interfaz de red asociado con el perfil que se debe eliminar. |
| perfil   | (**obligatorio**) Nombre del perfil que se debe eliminar. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
## Informe de errores de Windows (WER)
<hr>
### Descargar un archivo de Informe de errores de Windows (WER)

**Solicitud**

Puedes descargar un archivo relacionado con WER mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wer/report/file |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| usuario   | (**obligatorio**) Nombre de usuario asociado con el informe. |
| Tipo   | (**obligatorio**) Tipo de informe. Esto se puede **consultar** o **archivar**. |
| NAME   | (**obligatorio**) Nombre del informe. Esto debe estar codificado en Base64. |
| file   | (**obligatorio**) Nombre del archivo que se debe descargar del informe. Esto debe estar codificado en Base64. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- La respuesta contiene el archivo solicitado. 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Enumerar archivos en un Informe de errores de Windows (WER)

**Solicitud**

Puedes enumerar los archivos en un informe WER mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wer/report/files |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| usuario   | (**obligatorio**) Usuario asociado con el informe. |
| Tipo   | (**obligatorio**) Tipo de informe. Esto se puede **consultar** o **archivar**. |
| NAME   | (**obligatorio**) Nombre del informe. Esto debe estar codificado en Base64. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

```json
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Enumerar en una lista los Informes de errores de Windows (WER)

**Solicitud**

Puedes obtener los informes WER mediante el siguiente formato de solicitud.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wer/reports |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

Los informes WER en el siguiente formato.

```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Escritorio de Windows
* HoloLens
* IoT

<hr>
## Windows Performance Recorder (WPR) 
<hr>
### Iniciar el seguimiento con un perfil personalizado

**Solicitud**

Puedes cargar un perfil WPR e iniciar un seguimiento con dicho perfil mediante el siguiente formato de solicitud.  Solo se puede ejecutar un seguimiento cada vez. El perfil no permanecerá en el dispositivo. 
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/wpr/customtrace |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Un cuerpo HTTP que conforma varias partes que contiene el perfil WPR personalizado.

**Respuesta**

El estado de sesión WPR en el siguiente formato.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Iniciar una sesión de seguimiento del rendimiento de arranque

**Solicitud**

Puedes iniciar una sesión de seguimiento de WPR de arranque mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/wpr/boottrace |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| perfil   | (**obligatorio**) Este parámetro es necesario en el inicio. Nombre del perfil que debe iniciar una sesión de seguimiento del rendimiento. Los perfiles posibles se almacenan en perfprofiles/profiles.json. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

En Inicio, esta API devuelve el estado de sesión WPR en el siguiente formato.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Detener una sesión de seguimiento del rendimiento de arranque

**Solicitud**

Puedes detener una sesión de seguimiento de WPR de arranque mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wpr/boottrace |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

-  Ninguno.  **Nota:** Se trata de una operación de larga ejecución.  Devolverá cuando el archivo ETL haya terminado de escribirse en disco.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Iniciar una sesión de seguimiento del rendimiento

**Solicitud**

Puedes iniciar una sesión de seguimiento de WPR mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.  Solo se puede ejecutar un seguimiento cada vez. 
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/wpr/trace |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| perfil   | (**obligatorio**) Nombre del perfil que debe iniciar una sesión de seguimiento del rendimiento. Los perfiles posibles se almacenan en perfprofiles/profiles.json. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

En Inicio, esta API devuelve el estado de sesión WPR en el siguiente formato.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Detener una sesión de seguimiento del rendimiento

**Solicitud**

Puedes detener una sesión de seguimiento de WPR mediante el siguiente formato de solicitud. También se conoce como una sesión de seguimiento del rendimiento.
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wpr/trace |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno.  **Nota:** Se trata de una operación de larga ejecución.  Devolverá cuando el archivo ETL haya terminado de escribirse en disco.  

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Recuperar el estado de una sesión de seguimiento

**Solicitud**

Puedes recuperar el estado de la sesión actual de WPR mediante el siguiente formato de solicitud
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wpr/status |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

El estado de la sesión de seguimiento de WPR en el siguiente formato.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Lista de sesiones de seguimiento completadas (ETL)

**Solicitud**

Puedes obtener una lista del seguimientos de ETL en el dispositivo mediante el siguiente formato de solicitud. 

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wpr/tracefiles |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

La lista de sesiones de seguimiento completadas se proporciona en el siguiente formato.

```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Descargar una sesión de seguimiento (ETL)

**Solicitud**

Puedes descargar un archivo de seguimiento (seguimiento de arranque o el seguimiento de modo de usuario) mediante el siguiente formato de solicitud. 

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/wpr/tracefile |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| nombreDeArchivo   | (**obligatorio**) El nombre del seguimiento ETL para descargar.  Los encontrarás en /api/wpr/tracefiles |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Devuelve el archivo de seguimiento ETL.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
### Eliminar una sesión de seguimiento (ETL)

**Solicitud**

Puedes eliminar un archivo de seguimiento (seguimiento de arranque o el seguimiento de modo de usuario) mediante el siguiente formato de solicitud. 

| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/wpr/tracefile |


**Parámetros de URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

| Parámetro de URI | Descripción |
| :------          | :------ |
| nombreDeArchivo   | (**obligatorio**) Nombre del seguimiento ETL que se debe eliminar.  Los encontrarás en /api/wpr/tracefiles |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Devuelve el archivo de seguimiento ETL.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* IoT

<hr>
## Etiquetas de DNS-SD 
<hr>
### Ver etiquetas

**Solicitud**

Te permite ver las etiquetas aplicadas actualmente para el dispositivo.  Estas se anuncian a través de los registros TXT de DNS-SD en la tecla T.  
 
| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/dns-sd/tags |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta** Las etiquetas aplicadas actualmente con el formato siguiente. 
```json
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 5XX | Error del servidor |


**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Eliminar etiquetas

**Solicitud**

Elimina todas las etiquetas anunciadas actualmente por DNS-SD.   
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/dns-sd/tags |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**
 - Ninguno

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 5XX | Error del servidor |


**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

<hr>
### Eliminar etiqueta

**Solicitud**

Elimina una etiqueta anunciada actualmente por DNS-SD.   
 
| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/dns-sd/tag |


**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| tagValue | (**obligatorio**) La etiqueta que se debe quitar. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**
 - Ninguno

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |


**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT
 
<hr>
### Agregar una etiqueta

**Solicitud**

Agrega una etiqueta al anuncio de DNS-SD.   
 
| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/dns-sd/tag |


**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| tagValue | (**obligatorio**) La etiqueta que se debe agregar. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**
 - Ninguno

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 401 | Desbordamiento del espacio de la etiqueta.  Se produce si la etiqueta propuesta es demasiado larga para el registro de servicio de DNS-SD resultante. |


**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>Explorador de archivos de la aplicación

<hr>
### Obtener carpetas conocidas

**Solicitud**

Obtén una lista de carpetas accesibles de nivel superior.

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/filesystem/apps/knownfolders |


**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta** Las carpetas disponibles con el siguiente formato. 
```json
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Implementar la solicitud aceptada y que se está procesando |
| 4XX | Códigos de error |
| 5XX | Códigos de error |


**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

<hr>
### Obtener archivos

**Solicitud**

Obtén una lista de los archivos que hay en una carpeta.

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/filesystem/apps/files |


**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| knownfolderid | (**obligatorio**) El directorio de nivel superior donde quieres la lista de archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. |
| packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. |
| ruta de acceso | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta** Las carpetas disponibles con el siguiente formato. 
```json
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

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

<hr>
### Descargar un archivo

**Solicitud**

Obtener un archivo de una carpeta conocida o appLocalData.

| Método      | URI de la solicitud |
| :------     | :----- |
| GET | /api/filesystem/apps/file |

**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| knownfolderid | (**obligatorio**) El directorio de nivel superior donde deseas descargar los archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. |
| nombreDeArchivo | (**obligatorio**) El nombre del archivo que se va a descargar. |
| packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete que te interesa. |
| ruta de acceso | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- El archivo solicitado, si se encuentra presente.

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | El archivo solicitado |
| 404 | Archivo no encontrado |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

<hr>
### Cambiar el nombre de un archivo

**Solicitud**

Cambiar el nombre de un archivo de una carpeta.

| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/filesystem/apps/rename |


**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| knownfolderid | (**obligatorio**) El directorio de nivel superior en el que se encuentra el archivo. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. |
| nombreDeArchivo | (**obligatorio**) El nombre original del archivo cuyo nombre se va a cambiar. |
| newfilename | (**obligatorio**) El nuevo nombre del archivo.|
| packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. |
| ruta de acceso | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |. Se cambia el nombre del archivo.
| 404 | Archivo no encontrado |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

<hr>
### Eliminar un archivo

**Solicitud**

Elimina un archivo de una carpeta.

| Método      | URI de la solicitud |
| :------     | :----- |
| SUPRIMIR | /api/filesystem/apps/file |

**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| knownfolderid | (**obligatorio**) El directorio de nivel superior del que quieres eliminar los archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. |
| nombreDeArchivo | (**obligatorio**) El nombre del archivo que se va a eliminar. |
| packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. |
| ruta de acceso | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

- Ninguno 

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |. Se elimina el archivo. |
| 404 | Archivo no encontrado |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

<hr>
### Cargar un archivo

**Solicitud**

Carga un archivo en una carpeta.  Esto sobrescribirá cualquier archivo existente que tenga el mismo nombre, pero no creará carpetas nuevas. 

| Método      | URI de la solicitud |
| :------     | :----- |
| EXPONER | /api/filesystem/apps/file |

**Parámetros de URI**

| Parámetro de URI | Descripción |
| :------     | :----- |
| knownfolderid | (**obligatorio**) El directorio de nivel superior donde quieres cargar los archivos. Usa **LocalAppData** para obtener acceso a las aplicaciones transferidas localmente. |
| packagefullname | (**obligatorio si *knownfolderid* == LocalAppData**) El nombre completo del paquete de la aplicación que te interesa. |
| ruta de acceso | (**opcional**) El subdirectorio dentro de la carpeta o el paquete especificado anteriormente. |

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

| Código de estado HTTP      | Descripción |
| :------     | :----- |
| 200 | Aceptar |. El archivo se carga. |
| 4XX | Códigos de error |
| 5XX | Códigos de error |

**Familias de dispositivos disponibles**

* Windows Mobile
* Escritorio de Windows
* HoloLens
* Xbox
* IoT

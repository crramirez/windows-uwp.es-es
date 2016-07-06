---
author: WilliamsJason
title: "Referencia de API de registro de carpeta dinámica de Device Portal"
description: "Obtén información acerca de cómo tener acceso a las API de registro de carpeta dinámica mediante programación."
ms.sourcegitcommit: ef0f1339b77a8d1f60a677b2ff19a63b68f0d6cd
ms.openlocfilehash: 41e4cc67120b9e32fac34404ca918edcf58ba267

---

# Registrar una aplicación en una carpeta dinámica  

**Solicitud**

Puede registrar una aplicación en una carpeta dinámica mediante el siguiente formato de solicitud.

Método      | URI de la solicitud
:------     | :------
POST | /api/app/packagemanager/register
<br />
**Parámetros del URI**

Puedes especificar los siguientes parámetros adicionales en el URI de la solicitud:

Parámetro del URI      | Descripción
:------     | :-----
carpeta (necesaria) | El nombre de la carpeta de destino del paquete que se va a registrar. Esta carpeta debe existir en d:\developmentfiles\LooseApps en la consola. El nombre de esta carpeta debe estar codificado en Base64, ya que puede contener separadores de ruta de acceso si la carpeta es una subcarpeta de LooseApps.
<br />

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

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

* Windows Xbox

**Notas**

Hay al menos tres modos diferentes de obtener la aplicación dinámica en la consola en la carpeta que quieras. El más fácil es simplemente copiar los archivos a través de SMB a \\<IP_Address>\DevelopmentFiles\LooseApps. Esto requiere un nombre de usuario y una contraseña en los kits de UWA que pueden obtenerse mediante [/ext/smb/developerfolder](wdp-smb-api.md). 

La segunda forma es copiar los archivos individuales a la ubicación correcta realizando un POST /api/filesystem/apps/file donde knownfolderid es DevelopmentFiles, packagefullname está vacío y nombre de archivo y ruta de acceso se suministran correctamente (la ruta de acceso debe comenzar con LooseApps).

La tercera forma consiste en copiar una carpeta completa cada vez mediante [/api/app/packagemanager/upload](wdp-folder-upload.md) donde destinationFolder es el nombre de la carpeta que se colocará en d:\developmentfiles\looseapps y la carga tiene múltiples partes que conforman un cuerpo http del contenido del directorio.




<!--HONumber=Jun16_HO4-->


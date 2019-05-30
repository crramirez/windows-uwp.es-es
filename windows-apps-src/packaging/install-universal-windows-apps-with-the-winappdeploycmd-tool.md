---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Instalar aplicaciones con la herramienta WinAppDeployCmd.exe
description: Implementación de aplicaciones de Windows (WinAppDeployCmd.exe) es una herramienta de línea de comandos que puede usar para implementar una aplicación de plataforma Universal de Windows (UWP) desde un equipo con Windows 10 en cualquier dispositivo Windows 10.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06107691a4551ae2af05e63c1db810485273dc9b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372814"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>Instalar aplicaciones con la herramienta WinAppDeployCmd.exe


Implementación de aplicaciones de Windows (WinAppDeployCmd.exe) es una herramienta de línea de comandos que puede usar para implementar una aplicación de plataforma Universal de Windows (UWP) desde un equipo con Windows 10 en cualquier dispositivo Windows 10. Puede usar esta herramienta para implementar un paquete de aplicación cuando el dispositivo de Windows 10 está conectado mediante USB o disponible en la misma subred sin necesidad de Microsoft Visual Studio o la solución para esa aplicación. También puedes implementar la aplicación sin empaquetarla primero en un equipo remoto o en Xbox One. Este artículo describe cómo instalar aplicaciones para UWP con esta herramienta.

Basta con el SDK de Windows 10 instalado para ejecutar la herramienta WinAppDeployCmd desde un símbolo del sistema o un archivo de script. Cuando se instala una aplicación con WinAppDeployCmd.exe, utiliza el archivo.appx/.msix o AppxManifest (para archivos separados) para instalaciones de prueba de la aplicación en un dispositivo Windows 10. Este comando no instala el certificado necesario para la aplicación. Para ejecutar la aplicación, el dispositivo Windows 10 debe estar en modo de programador o ya tiene instalado el certificado.

Para implementar en dispositivos móviles, primero debes crear un paquete. Puedes obtener más información [aquí](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

El **WinAppDeployCmd.exe** herramienta se encuentra aquí en tu PC con Windows 10: **C:\\archivos de programa (x86)\\Windows Kits\\10\\bin\\&lt;SDK versión&gt;\\x86\\WinAppDeployCmd.exe** () según la ruta de instalación para el SDK). 
> [!NOTE]
> En la versión 15063 y posteriores del SDK, el SDK se instala en paralelo dentro de carpetas específicas de la versión.  Los SDK anteriores (antes de 14393 incluido) se escriben directamente en la carpeta principal.

En primer lugar, conecte el dispositivo de Windows 10 a la misma subred o conéctelo directamente a la máquina de Windows 10 con una conexión USB. A continuación, usa la siguiente sintaxis y los ejemplos de este comando que se incluyen más adelante en este artículo para implementar la aplicación para UWP:

## <a name="winappdeploycmd-syntax-and-options"></a>Opciones y sintaxis de WinAppDeployCmd

Esta es la sintaxis general que se usa para **WinAppDeployCmd.exe**:
```syntax
WinAppDeployCmd command -option <argument>
```

Estos son algunos ejemplos de sintaxis adicionales para el uso de distintos comandos:
```syntax
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

Puedes instalar o desinstalar una aplicación en el dispositivo de destino, o bien actualizar una aplicación que ya está instalada. Para mantener los datos o la configuración guardados por una aplicación que ya está instalada, usa las opciones de **update** en lugar de las opciones de **install** .

La siguiente tabla describe los comandos de **WinAppDeployCmd.exe**.


| **Command**  | **Descripción**                                                     |
|--------------|---------------------------------------------------------------------|
| dispositivos      | Muestra la lista de dispositivos de red disponibles.                         |
| instalar      | Instala un paquete de la aplicación para UWP en el dispositivo de destino.                     |
| actualización       | Actualiza una aplicación para UWP que ya esté instalada en el dispositivo de destino.    |
| list         | Muestra la lista de aplicaciones para UWP instaladas en el dispositivo de destino especificado. |
| uninstall    | Desinstala el paquete de la aplicación especificado del dispositivo de destino.         |
| deployfiles  | Copia la aplicación de archivos sueltos que está la ruta de destino a la ruta relativa remota del dispositivo.|
| registerfiles| Registra la aplicación de archivos sueltos en el directorio de implementación remoto.         |
| addcreds     | Agrega credenciales a una consola Xbox para que pueda acceder a una ubicación de red para el registro de la aplicación.|
| getcreds     | Obtiene credenciales de red para los usos de destino cuando se ejecuta una aplicación desde un recurso compartido de red.|
| deletecreds  | Elimina credenciales de red que el destino usa cuando ejecuta una aplicación desde un recurso compartido de red.|


La siguiente tabla describe las opciones de **WinAppDeployCmd.exe**.


| **Command**  | **Descripción**  |
|--------------|------------------|
| -h (-help)       | Muestra los comandos, las opciones y los argumentos. |
| -ip              | Dirección IP del dispositivo de destino. |
| -g (-guid)       | Identificador único del dispositivo de destino.|
| -d (-dependency) | (Opcional) Especifica la ruta de dependencia de cada una de las dependencias del paquete. Si no se especifica ninguna ruta, la herramienta busca dependencias en el directorio raíz del paquete de la aplicación y los directorios del SDK.|
| -f (-file)       | Ruta de archivo del paquete de la aplicación que se va a instalar, actualizar o desinstalar.|
| -p (-package)    | Nombre completo del paquete de la aplicación que se va a desinstalar. (Puedes usar el comando de la lista para encontrar los nombres completos de los paquetes ya instalados en el dispositivo) |
| -pin             | Pin si es necesario para establecer una conexión con el dispositivo de destino. (Se te pedirá que vuelvas a intentarlo con la opción -pin si se requiere autenticación) |
| -credserver      | El nombre del servidor de las credenciales de red para su uso por parte del destino. |
| -credusername    | El nombre de usuario de las credenciales de red para su uso por parte del destino. |
| -credpassword    | La contraseña de las credenciales de red para su uso por parte del destino. |
| -connecttimeout  | El tiempo de espera en segundos que se usa para conectar con el dispositivo. |
| -remotedeploydir | Ruta de acceso y nombre del directorio relativo en la que copiar los archivos en el dispositivo remoto; se trata de una carpeta de implementación remota conocida determinada automáticamente. |
| -deleteextrafile | Cambia para indicar si se deben purgar los archivos existentes en el directorio remoto para que coincidan con los del directorio de origen. |


La siguiente tabla describe las opciones de **WinAppDeployCmd.exe**.

| **Argumento**           | **Descripción**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | Tiempo de expiración en segundos. (El valor predeterminado es 10)                                          |
| &lt;address&gt;        | Dirección IP o identificador único del dispositivo de destino.                        |
| &lt;a&gt;&lt;b&gt; ... | Ruta de dependencia de cada una de las dependencias del paquete de la aplicación.                    |
| &lt;p&gt;              | PIN alfanumérico que se muestra en la configuración del dispositivo para establecer una conexión. |
| &lt;path&gt;           | Ruta del sistema de archivos.                                                            |
| &lt;name&gt;           | El nombre completo del paquete de la aplicación que se va a desinstalar.                          |
| &lt;server&gt;         | El servidor de la red de archivos.                                                  |
| &lt;username&gt;       | El usuario para las credenciales con acceso al servidor de la red de archivos.      |
| &lt;password&gt;       | La contraseña para las credenciales con acceso al servidor de la red de archivos. |
| &lt;remotedeploydir&gt;| El directorio del dispositivo relativo a la ubicación de implementación                      |

 
## <a name="winappdeploycmdexe-examples"></a>Ejemplos de WinAppDeployCmd.exe

Estos son algunos ejemplos de implementación desde la línea de comandos mediante la sintaxis de **WinAppDeployCmd.exe**.

Muestra los dispositivos que están disponibles para la implementación. El comando expira en 3 segundos.

``` syntax
WinAppDeployCmd devices 3
```

Instala la aplicación desde el paquete MyApp.appx que se encuentra en el directorio de descargas de su PC a un dispositivo Windows 10 con la dirección IP 192.168.0.1 con un PIN de A1B2C3 para establecer una conexión con el dispositivo

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Desinstala el paquete especificado (basado en su nombre completo) de un dispositivo con Windows 10 y la dirección IP 192.168.0.1. Puedes usar el comando list para ver los nombres completos de todos los paquetes que están instalados en un dispositivo.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Actualiza la aplicación que ya está instalada en el dispositivo Windows 10 con una dirección IP 192.168.0.1, mediante el paquete de aplicación especificado.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Implementa los archivos de una aplicación que esté en un equipo o Xbox con la dirección IP 192.168.0.1 y en la misma carpeta que AppxManifest en el directorio app1_F5, en la ruta de implementación del dispositivo.

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Registra la aplicación que está en el directorio app1_F5 en la ruta de acceso de implementación del equipo o Xbox en 192.168.0.1.

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>Uso de WinAppDeployCmd para configurar la implementación de Run from PC en Xbox One

Run from PC permite implementar una aplicación para UWP en una consola Xbox One sin copiar los archivos binarios; en su lugar, los archivos binarios se hospedan en un recurso compartido de red en la misma red que la consola Xbox.  Para hacerlo, necesitas una consola Xbox One desbloqueada por el desarrollador y una aplicación para UWP de archivos sueltos en una unidad de red a la que pueda acceder la consola Xbox.

Ejecuta lo siguiente para registrar la aplicación:
``` syntax
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```

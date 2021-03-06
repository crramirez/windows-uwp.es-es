---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Instalar aplicaciones con la herramienta WinAppDeployCmd.exe
description: Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación de la Plataforma universal de Windows (UWP) desde un equipo con Windows 10 a cualquier dispositivo con Windows 10.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6c8383a5b0041d5edf6e0c2c8d94acf82572d13
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "70808442"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>Instalar aplicaciones con la herramienta WinAppDeployCmd.exe

Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación de la Plataforma universal de Windows (UWP) desde un equipo con Windows 10 a cualquier dispositivo con Windows 10. Puedes usar esta herramienta para implementar un paquete de la aplicación si el dispositivo con Windows 10 está conectado mediante USB o disponible en la misma subred sin necesidad de Microsoft Visual Studio ni de la solución para dicha aplicación. También puedes implementar la aplicación sin empaquetarla primero en un equipo remoto o en Xbox One. Este artículo describe cómo instalar aplicaciones para UWP con esta herramienta.

Solo necesitas tener instalado el SDK de Windows 10 para ejecutar la herramienta WinAppDeployCmd desde un símbolo del sistema o un archivo de script. Cuando se instala una aplicación con WinAppDeployCmd.exe, esta usa el archivo .appx o .msix, o AppxManifest (para los archivos sueltos), para transferir localmente la aplicación a un dispositivo con Windows 10. Este comando no instala el certificado necesario para la aplicación. Para ejecutar la aplicación, el dispositivo con Windows 10 debe estar en modo de desarrollador o tener el certificado instalado.

Para implementar en dispositivos móviles, primero debes crear un paquete. Puedes obtener más información [aquí](/windows/msix/package/packaging-uwp-apps).

La herramienta **WinAppDeployCmd.exe** se encuentra aquí en el equipo con Windows 10: **C:\\Archivos de programa (x86)\\Windows Kits\\10\\bin\\&lt;SDK Version&gt;\\x86\\WinAppDeployCmd.exe** (según la ruta de acceso de instalación del SDK).

> [!NOTE]
> El SDK de la versión 15063 y posteriores se instala en paralelo dentro de carpetas específicas de la versión. Los SDK de la versión 14393 y anteriores se escriben directamente en la carpeta principal.

En primer lugar, conecta el dispositivo con Windows 10 a la misma subred o directamente al equipo con Windows 10 mediante una conexión USB. A continuación, usa la siguiente sintaxis y los ejemplos de este comando que se incluyen más adelante en este artículo para implementar la aplicación para UWP:

## <a name="winappdeploycmd-syntax-and-options"></a>Opciones y sintaxis de WinAppDeployCmd

Esta es la sintaxis general que se usa para **WinAppDeployCmd.exe**:

```CMD
WinAppDeployCmd command -option <argument>
```

Estos son algunos ejemplos de sintaxis adicionales para el uso de distintos comandos:

```CMD
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

| **Comando**  | **Descripción**                                                     |
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

| **Comando**  | **Descripción**  |
|--------------|------------------|
| -h (-help)       | Muestra los comandos, las opciones y los argumentos. |
| -ip              | Dirección IP del dispositivo de destino. |
| -g (-guid)       | Identificador único del dispositivo de destino.|
| -d (-dependency) | (Opcional) Especifica la ruta de dependencia de cada una de las dependencias del paquete. Si no se especifica ninguna ruta, la herramienta busca dependencias en el directorio raíz del paquete de la aplicación y los directorios del SDK.|
| -f (-file)       | Ruta de archivo del paquete de la aplicación que se va a instalar, actualizar o desinstalar.|
| -p (-package)    | Nombre completo del paquete de la aplicación que se va a desinstalar. (Puedes usar el comando de la lista para encontrar los nombres completos de los paquetes ya instalados en el dispositivo). |
| -pin             | Pin si es necesario para establecer una conexión con el dispositivo de destino. (Se te pedirá que vuelvas a intentarlo con la opción -pin si se requiere autenticación). |
| -credserver      | El nombre del servidor de las credenciales de red para su uso por parte del destino. |
| -credusername    | El nombre de usuario de las credenciales de red para su uso por parte del destino. |
| -credpassword    | La contraseña de las credenciales de red para su uso por parte del destino. |
| -connecttimeout  | El tiempo de espera en segundos que se usa para conectar con el dispositivo. |
| -remotedeploydir | Nombre y ruta de acceso relativa del directorio en el que se copiarán los archivos en el dispositivo remoto; se trata de una carpeta de implementación remota conocida que se determina automáticamente. |
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

A continuación, presentamos algunos ejemplos de implementación desde la línea de comandos mediante la sintaxis de **WinAppDeployCmd.exe**.

Muestra los dispositivos que están disponibles para la implementación. El comando expira en 3 segundos.

``` CMD
WinAppDeployCmd devices 3
```

Instala la aplicación desde el paquete MyApp.appx, que se encuentra en el directorio Descargas de tu PC, en un dispositivo con Windows 10 con la dirección IP 192.168.0.1 y el PIN A1B2C3 para establecer una conexión con el dispositivo.

``` CMD
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Desinstala el paquete especificado (basado en su nombre completo) de un dispositivo con Windows 10 y la dirección IP 192.168.0.1. Puedes usar el comando list para ver los nombres completos de todos los paquetes que están instalados en un dispositivo.

``` CMD
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Actualiza la aplicación que ya está instalada en el dispositivo con Windows 10 y la dirección IP 192.168.0.1 con el paquete de la aplicación especificado.

``` CMD
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Implementa los archivos de una aplicación que esté en un equipo o Xbox con la dirección IP 192.168.0.1 y en la misma carpeta que AppxManifest en el directorio app1_F5, en la ruta de implementación del dispositivo.

``` CMD
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Registra la aplicación que está en el directorio app1_F5 en la ruta de acceso de implementación del equipo o Xbox en 192.168.0.1.

``` CMD
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>Uso de WinAppDeployCmd para configurar la implementación de Run from PC en Xbox One

Run from PC permite implementar una aplicación para UWP en una consola Xbox One sin copiar los archivos binarios; en su lugar, los archivos binarios se hospedan en un recurso compartido de red en la misma red que la consola Xbox.  Para hacerlo, necesitas una consola Xbox One desbloqueada por el desarrollador y una aplicación para UWP de archivos sueltos en una unidad de red a la que pueda acceder la consola Xbox.

Ejecuta lo siguiente para registrar la aplicación:

``` CMD
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```

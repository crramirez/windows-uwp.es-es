---
author: msatranjr
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Instalar aplicaciones con la herramienta WinAppDeployCmd.exe
description: Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación para la Plataforma universal de Windows (UWP) de un equipo con Windows 10 en cualquier dispositivo con Windows 10 Mobile.
---
# Instalar aplicaciones con la herramienta WinAppDeployCmd.exe

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows Application Deployment (WinAppDeployCmd.exe) es una herramienta de línea de comandos que se puede usar para implementar una aplicación para la Plataforma universal de Windows (UWP) de un equipo con Windows 10 en cualquier dispositivo con Windows 10 Mobile. Puedes usar esta herramienta para implementar un paquete .appx si el dispositivo con Windows 10 Mobile está conectado mediante USB o disponible en la misma subred sin necesidad de Microsoft Visual Studio o la solución para dicha aplicación. Este artículo describe cómo instalar aplicaciones para UWP con esta herramienta.

Solo necesitas el SDK de Windows 10 instalado para ejecutar la herramienta WinAppDeployCmd desde un símbolo del sistema o un archivo de script. Cuando se instala una aplicación con WinAppDeployCmd.exe, esta usa el archivo .appx para transferir localmente la aplicación a un dispositivo con Windows 10 Mobile. Este comando no instala el certificado necesario para la aplicación. Para ejecutar la aplicación, el dispositivo con Windows 10 Mobile debe estar en modo de desarrollador o tener el certificado instalado.

Para poder implementar la aplicación en dispositivos con Windows 10 Mobile, debes crear primero un paquete. Para obtener más información, consulta \[parent link here\].

La herramienta **WinAppDeployCmd.exe** se encuentra aquí en tu equipo Windows 10: **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe** (en función de la ruta de acceso de instalación del SDK). En primer lugar, conecta el dispositivo con Windows 10 Mobile a la misma subred o directamente al equipo con Windows 10 mediante una conexión USB. A continuación, usa la siguiente sintaxis y los ejemplos de este comando que se incluyen más adelante en este artículo para implementar el paquete .appx:

## Opciones y sintaxis de WinAppDeployCmd

Esta es la sintaxis posible que puedes usar para **WinAppDeployCmd.exe**

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
```

Puedes instalar o desinstalar una aplicación en el dispositivo de destino, o bien actualizar una aplicación que ya está instalada. Para mantener los datos o la configuración guardados por una aplicación que ya está instalada, usa las opciones de **update** en lugar de las opciones de **install** .

La siguiente tabla describe los comandos de **WinAppDeployCmd.exe**.

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **Comando** | **Descripción**                                                     |
| Dispositivos     | Muestra la lista de dispositivos de red disponibles.                         |
| instalar     | Instala un paquete de la aplicación para UWP en el dispositivo de destino.                     |
| update      | Actualiza una aplicación para UWP que ya esté instalada en el dispositivo de destino.    |
| list        | Muestra la lista de aplicaciones para UWP instaladas en el dispositivo de destino especificado. |
| uninstall   | Desinstala el paquete de la aplicación especificado del dispositivo de destino.         |

 

La siguiente tabla describe las opciones de **WinAppDeployCmd.exe**.

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Comando**      | **Descripción**                                                                                                                                                                                               |
| -h (-help)       | Muestra los comandos, las opciones y los argumentos.                                                                                                                                                                     |
| -ip              | Dirección IP del dispositivo de destino.                                                                                                                                                                              |
| -g (-guid)       | Identificador único del dispositivo de destino.                                                                                                                                                                       |
| -d (-dependency) | (Opcional) Especifica la ruta de dependencia de cada una de las dependencias del paquete. Si no se especifica ninguna ruta, la herramienta busca dependencias en el directorio raíz del paquete de la aplicación y los directorios del SDK. |
| -f (-file)       | Ruta de archivo del paquete de la aplicación que se va a instalar, actualizar o desinstalar.                                                                                                                                                |
| -p (-package)    | Nombre completo del paquete de la aplicación que se va a desinstalar. (Puedes usar el comando de la lista para encontrar los nombres completos de los paquetes ya instalados en el dispositivo).                                                   |
| -pin             | Pin si es necesario para establecer una conexión con el dispositivo de destino. (Se te pedirá que vuelvas a intentarlo con la opción -pin si se requiere autenticación).                                                 |

 

La siguiente tabla describe las opciones de **WinAppDeployCmd.exe**.

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **Argumento**           | **Descripción**                                                              |
| &lt;x&gt;              | Tiempo de expiración en segundos. (El valor predeterminado es 10)                                          |
| &lt;address&gt;        | Dirección IP o identificador único del dispositivo de destino.                        |
| &lt;a&gt;&lt;b&gt; ... | Ruta de dependencia de cada una de las dependencias del paquete de la aplicación.                    |
| &lt;p&gt;              | PIN alfanumérico que se muestra en la configuración del dispositivo para establecer una conexión. |
| &lt;path&gt;           | Ruta del sistema de archivos.                                                            |
| &lt;name&gt;           | Nombre completo del paquete de la aplicación que se va a desinstalar.                          |

 
## Ejemplos de WinAppDeployCmd.exe

Estos son algunos ejemplos de implementación desde la línea de comandos mediante la sintaxis de **WinAppDeployCmd.exe**.

Muestra los dispositivos que están disponibles para la implementación. El comando expira en 3 segundos.

``` syntax
WinAppDeployCmd devices 3
```

Instala la aplicación desde el paquete MyApp.appx que se encuentra en el directorio Descargas de tu PC en un dispositivo con Windows 10 Mobile con la dirección IP 192.168.0.1 y el PIN A1B2C3 para establecer una conexión con el dispositivo

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Desinstala el paquete especificado (basado en su nombre completo) de un dispositivo con Windows 10 Mobile y la dirección IP 192.168.0.1. Puedes usar el comando de la lista para ver los nombres completos de todos los paquetes que están instalados en un dispositivo.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Actualiza la aplicación que ya está instalada en el dispositivo con Windows 10 Mobile y la dirección IP 192.168.0.1 con el paquete .appx especificado.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```



<!--HONumber=May16_HO2-->



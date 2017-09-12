---
author: normesta
Description: "En este artículo se ofrece un análisis más profundo sobre cómo funciona el Puente de dispositivo de escritorio en modo no visible."
title: Segundo plano del Puente de dispositivo de escritorio
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.openlocfilehash: 050499baaf383fc135d833ae1e4733c95f2b5fa1
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2017
---
# <a name="behind-the-scenes-of-the-desktop-bridge"></a>Segundo plano del Puente de dispositivo de escritorio

En este artículo se ofrece un análisis más profundo sobre cómo funciona el Puente de dispositivo de escritorio en modo no visible.

Un objetivo clave del Puente de dispositivo de escritorio es separar el estado de la aplicación del estado del sistema tanto como sea posible a la vez que se mantiene la compatibilidad con otras aplicaciones. Para ello, el puente coloca la aplicación dentro de un paquete de Plataforma universal de Windows (UWP) y, después, detecta y redirige algunos cambios que realiza en el sistema de archivos y en el Registro en tiempo de ejecución.

Los paquetes que creas para tu aplicación de escritorio son solo de escritorio y las aplicaciones de plena confianza no se virtualizan ni están en un espacio aislado. Esto les permite interactuar con otras aplicaciones de la misma forma que las aplicaciones de escritorio clásicas.

## <a name="installation"></a>Instalación

Los paquetes de aplicaciones se instalan en *C:\Archivos de programa\WindowsApps\nombre_paquete*, con el archivo ejecutable denominado *nombre_aplicación.exe*. Cada carpeta de paquete contiene un manifiesto (denominado AppxManifest.xml) que incluye un espacio de nombres XML especial para las aplicaciones empaquetadas. En ese archivo de manifiesto hay un elemento ```<EntryPoint>``` que hace referencia a una aplicación de plena confianza. Cuando se inicia la aplicación, no se ejecuta dentro de un contenedor de la aplicación, sino que, en su lugar, se ejecuta como el usuario con normalidad.

Después de la implementación, el sistema operativo marca los archivos del paquete como de solo lectura y los bloquea completamente. Windows evita que las aplicaciones se inicien en caso de que se manipulen estos archivos.

## <a name="file-system"></a>Sistema de archivos

Para incluir el estado de la aplicación, el puente intenta capturar los cambios que realiza la aplicación en AppData. Todo lo que se escribe en la carpeta AppData del usuario (por ejemplo, *C:\Usuarios\nombre_usuario\AppData*), como las operaciones de creación, eliminación y actualización, se copia en escritura a una ubicación privada por usuario y por aplicación. Esto crea la ilusión de que la aplicación empaquetada está editando la carpeta AppData real, pero, en realidad, está modificando una copia privada. Al redireccionar las escrituras de este modo, el sistema puede realizar un seguimiento de todas las modificaciones de archivos que realiza la aplicación. Esto permite que el sistema limpie esos archivos cuando se desinstala la aplicación; por lo tanto, se reduce el "deterioro" del sistema y se ofrece una mejor experiencia de eliminación para el usuario.

Además de redirigir AppData, el puente también combina dinámicamente las carpetas conocidas de Windows (System32, Archivos de programa (x86), etcétera) con los directorios correspondientes del paquete de la aplicación. Cada paquete contiene una carpeta denominada "VFS" en su raíz. Las lecturas de directorios o archivos en el directorio VFS se combinan en tiempo de ejecución con sus respectivos equivalentes nativos. Por ejemplo, una aplicación podría contener *C:\Archivos de pograma\WindowsApps\nombre_paquete\VFS\SystemX86\vc10.dll* como parte de su paquete de la aplicación, pero el archivo aparecería como instalado en *C:\Windows\System32\vc10.dll*.  Esto mantiene la compatibilidad con las aplicaciones de escritorio que pudieran esperar que los archivos se encuentren en ubicaciones distintas del paquete.

No se permiten las escrituras en archivos o carpetas del paquete de la aplicación. El puente ignora las escrituras en archivos y carpetas que no forman parte del paquete, y se permiten si el usuario tiene permisos.

### <a name="common-operations"></a>Operaciones comunes

En la siguiente tabla de referencia breve se muestran las operaciones comunes del sistema de archivos y cómo las controla el puente.

Operación | Resultado | Ejemplo
:--- | :--- | :---
Lectura o enumeración de un archivo o una carpeta de Windows conocidos | Una combinación dinámica de *C:\Archivos de programa\nombre_paquete\VFS\carpeta_conocida* con el equivalente del sistema local. | La lectura de *C:\Windows\System32* devuelve el contenido de *C:\Windows\System32*, además del contenido de *C:\Archivos de programa\WindowsApps\nombre_paquete\VFS\SystemX86*.
Escritura en AppData | Copia en escritura a una ubicación por usuario, por aplicación. | Normalmente, AppData es *C:\Usuarios\nombre_usuario\AppData*.  
Escritura dentro el paquete | No permitido. El paquete es de solo lectura. | No se permiten las escrituras en *C:\Archivos de programa\WindowsApps\nombre_paquete*.
Escrituras fuera del paquete | Omitidas por el puente. Se permiten si el usuario tiene permisos. | Una operación de escritura en *C:\Windows\System32\foo.dll* se permite si el paquete no contiene *C:\Archivos de programa\WindowsApps\nombre_paquete\VFS\SystemX86\foo.dll* y el usuario tiene permisos.

### <a name="packaged-vfs-locations"></a>Ubicaciones de VFS empaquetadas

En la tabla siguiente se muestra en qué ubicación del sistema se superponen los archivos que se envían como parte del paquete para la aplicación. La aplicación considerará que estos archivos están en las ubicaciones del sistema que figuran en la lista, pero, en realidad, se encuentran en las ubicaciones redirigidas dentro de *C:\Archivos de programa\WindowsApps\nombre_paquete\VFS*. Las ubicaciones de FOLDERID proceden de las constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

Ubicación del sistema | Ubicación redirigida (en [RutaDelPaquete]\VFS\) | Válido en las arquitecturas
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | AppData comunes | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Registro

El puente controla el Registro de forma similar al sistema de archivos. Los paquetes de aplicaciones contienen un archivo registry.dat, que actúa como el equivalente lógico de *HKLM\Software* en el Registro real. En tiempo de ejecución, este Registro virtual combina el contenido de este subárbol en el subárbol del sistema nativo para proporcionar una vista especial de ambos. Por ejemplo, si registry.dat contiene una sola clave "Foo", entonces una operación de lectura de *HKLM\Software* en tiempo de ejecución también mostrará que contiene "Foo" (además de todas las claves del sistema nativo).

Solo las claves que se encuentran en *HKLM\Software* forman parte del paquete, no las claves que se encuentran en *HKCU* o en otros lugares del Registro. No se permiten las escrituras en las claves o los valores del paquete. El puente omite las escrituras en las claves o los valores que no forman parte del paquete y dichas escrituras se permiten si el usuario tiene permisos.

Todas las escrituras en HKCU se copian en escritura a una ubicación privada por usuario y por aplicación. Esto ofrece las mismas ventajas que el control del sistema de archivos que realiza el puente en relación con la limpieza de desinstalaciones. Tradicionalmente, los desinstaladores no pueden limpiar *HKEY_CURRENT_USER* porque los datos del Registro de los usuarios que cerraron sesión se desmontan y no están disponibles.

Todas las escrituras se conservan durante la actualización del paquete y únicamente se eliminan cuando la aplicación se quita por completo.

### <a name="common-operations"></a>Operaciones comunes

En la siguiente tabla de referencia breve se muestran las operaciones del Registro y cómo las controla el puente.

Operación | Resultado | Ejemplo
:--- | :--- | :---
Lectura o enumeración de *HKLM\Software* | Una combinación dinámica del subárbol del paquete con el equivalente del sistema local. | Si registry.dat contiene una sola clave "Foo", en tiempo de ejecución una operación de lectura de *HKLM\Software* mostrará el contenido de *HKLM\Software*, además de *HKLM\Software\Foo*.
Escrituras en HKCU | Copia en escritura a una ubicación privada por usuario, por aplicación. | Lo mismo que AppData para archivos.
Escrituras dentro el paquete. | No permitido. El paquete es de solo lectura. | No se permiten las escrituras en *HKLM\Software* si existe una clave o un valor correspondiente en el subárbol del paquete.
Escrituras fuera del paquete | Omitidas por el puente. Se permiten si el usuario tiene permisos. | Las escrituras en *HKLM\Software* se permiten si no existe una clave y un valor correspondiente en el subárbol del paquete y si el usuario tiene los permisos de acceso adecuados.

## <a name="uninstallation"></a>Desinstalación

Cuando el usuario desinstala un paquete, se quitan todos los archivos y las carpetas ubicados en *C:\Archivos de programa\WindowsApps\nombre_paquete*, así como las escrituras redirigidas a AppData o al Registro que capturó el puente.

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.

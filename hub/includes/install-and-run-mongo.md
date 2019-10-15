---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314889"
---
Para instalar MongoDB:

1. Abra el terminal de WSL (es decir, Ubuntu 18,04).
2. Actualización de los paquetes Ubuntu: `sudo apt update`
3. Una vez actualizados los paquetes, instale MongoDB con: `sudo apt-get install mongodb`
4. Confirmar la instalación y obtener el número de versión: `mongod --version`

Hay 3 comandos que debe conocer una vez instalado MongoDB:

1. `sudo service mongodb status` para comprobar el estado de la base de datos.
2. `sudo service mongodb start` para empezar a ejecutar la base de datos.
3. `sudo service mongodb stop` para dejar de ejecutar la base de datos.

> [!NOTE]
> Es posible que vea el comando `sudo systemctl status mongodb` usado en tutoriales o artículos. Para seguir siendo ligero, WSL no incluye `systemd` (un sistema de administración de servicios en Linux). En su lugar, usa SysVinit para iniciar los servicios en el equipo. No debe notar una diferencia, pero si un tutorial recomienda usar `sudo systemctl`, en su lugar use: `sudo /etc/init.d/`. Por ejemplo, `sudo systemctl status mongodb`, para WSL sería `sudo /etc/inid.d/mongodb status`... también puede usar `sudo service mongodb status`.

### <a name="run-your-mongo-database-in-a-local-server"></a>Ejecutar la base de datos Mongo en un servidor local

1. Compruebe el estado de la base de datos: `sudo service mongodb status` debería ver una respuesta [error], a menos que ya haya iniciado la base de datos.

2. Inicie la base de datos: `sudo service mongodb start` ahora debería ver una respuesta [OK].

3. Para comprobarlo, conéctese al servidor de base de datos y ejecute un comando de diagnóstico: `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` generará la versión actual de la base de datos, la dirección del servidor y el puerto, y la salida del comando status. Un valor de `1` para el campo "Aceptar" en la respuesta indica que el servidor está funcionando.

4. Para detener la ejecución del servicio MongoDB, escriba: `sudo service mongodb stop`

> [!NOTE]
> MongoDB tiene varios parámetros predeterminados, incluido el almacenamiento de datos en/Data/dB y la ejecución en el puerto 27017. Además, `mongod` es el demonio (proceso de host para la base de datos) y `mongo` es el shell de línea de comandos que se conecta a una instancia específica de `mongod`.

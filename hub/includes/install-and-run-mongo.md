---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314889"
---
Para instalar MongoDB

1. Abre el terminal de WSL (es decir, Ubuntu 18.04).
2. Actualiza los paquetes de Ubuntu: `sudo apt update`.
3. Una vez actualizados los paquetes, instala MongoDB con `sudo apt-get install mongodb`.
4. Confirma la instalación y obtén el número de versión `mongod --version`.

Hay 3 comandos que debes conocer una vez instales MongoDB:

1. `sudo service mongodb status` para comprobar el estado de la base de datos.
2. `sudo service mongodb start` para empezar a ejecutar la base de datos.
3. `sudo service mongodb stop` para detener la ejecución de la base de datos.

> [!NOTE]
> Es posible que veas que el comando `sudo systemctl status mongodb` se utiliza en tutoriales o artículos. Para mantenerse ligero, WSL no incluye `systemd` (un sistema de administración de servicios de Linux). En su lugar, usa SysVinit para iniciar los servicios en la máquina. No debías notar ninguna diferencia, pero si un tutorial recomienda usar `sudo systemctl`, usa `sudo /etc/init.d/` en su lugar. Por ejemplo, para WSL, `sudo systemctl status mongodb` sería `sudo /etc/inid.d/mongodb status`, pero también puedes usar `sudo service mongodb status`.

### <a name="run-your-mongo-database-in-a-local-server"></a>Ejecución de la base de datos Mongo en un servidor local

1. Comprueba el estado de la base de datos: `sudo service mongodb status`. Deberías ver una respuesta de tipo [Error], a menos que ya hayas iniciado la base de datos.

2. Inicia la base de datos: `sudo service mongodb start`. Ahora deberías ver una respuesta de tipo [Aceptar].

3. Para verificarlo, conéctate al servidor de la base de datos y ejecuta un comando de diagnóstico: `mongo --eval 'db.runCommand({ connectionStatus: 1 })'`. Se generará la versión actual de la base de datos, la dirección del servidor y el puerto, y la salida del comando de estado. El valor `1` para el campo "Aceptar" en la respuesta indica que el servidor funciona.

4. Para detener la ejecución del servicio MongoDB, escribe `sudo service mongodb stop`.

> [!NOTE]
> MongoDB tiene varios parámetros predeterminados, incluido el almacenamiento de datos en /data/db y la ejecución en el puerto 27017. Así mismo, `mongod` es el demonio (proceso de host para la base de datos) y `mongo` es el shell de la línea de comandos que se conecta a una instancia específica de `mongod`.

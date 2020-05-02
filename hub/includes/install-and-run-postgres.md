---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314899"
---
Para instalar PostgreSQL:

1. Abre el terminal de WSL (es decir, Ubuntu 18.04).
2. Actualiza los paquetes de Ubuntu: `sudo apt update`.
3. Una vez que los paquetes se hayan actualizado, instala PostgreSQL (y el paquete -contrib que tenga algunas utilidades útiles) con: `sudo apt install postgresql postgresql-contrib`.
4. Confirma la instalación y obtén el número de versión: `psql --version`.

Hay 3 comandos que debes conocer una vez instales PostgreSQL:

1. `sudo service postgresql status` para comprobar el estado de la base de datos.
2. `sudo service postgresql start` para empezar a ejecutar la base de datos.
3. `sudo service postgresql stop` para detener la ejecución de la base de datos.

### <a name="postgresql-user-setup"></a>Configuración de usuario de PostgreSQL

El usuario administrador predeterminado, `postgres`, necesita tener una contraseña asignada para poder conectarse a una base de datos. Para establecer una contraseña:

1. Escribe el comando: `sudo passwd postgres`.
2. Recibirás un aviso para escribir la nueva contraseña.
3. Cierra y vuelve a abrir el terminal.

### <a name="run-postgresql-with-psql-shell"></a>Ejecución de PostgreSQL con el shell de psql

[psql](https://www.postgresql.org/docs/10/app-psql.html) es un front-end basado en terminal de PostgreSQL. Te permite escribir consultas de forma interactiva, emitirlas a PostgreSQL y ver los resultados de la consulta. Como alternativa, la entrada también puede realizarse desde un archivo. Además, proporciona una serie de metacomandos y diversas características similares a los shells a fin de facilitar la escritura de scripts y la automatización de una amplia variedad de tareas.

Para iniciar el shell de psql:

1. Inicia el servicio Postgres: `sudo service postgresql start`.
2. Conéctate al servicio Postgres y abre el shell de psql: `sudo -u postgres psql`.

Una vez que hayas escrito correctamente el shell de psql, verás que el cambio de la línea de comandos tiene el siguiente aspecto: `postgres=#`.

> [!NOTE]
> Como alternativa, puedes abrir el shell de psql si cambias al usuario Postgres por `su - postgres` y, a continuación, escribes el comando `psql`.

Para salir de postgres=#, escribe `\q` o usa la tecla de método abreviado Control + D.

Para ver qué cuentas de usuario se han creado en la instalación de PostgreSQL, usa `psql -c "\du"`desde el terminal WSL o simplemente `\du`, si tienes abierto el shell de psql. Este comando mostrará las columnas siguientes: Nombre de usuario de la cuenta, List of Roles Attributes (Lista de atributos de roles) y Member of role group(s) (Miembro de los grupos de roles). Para salir y volver a la línea de comandos, escribe `q`.

---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314899"
---
Para instalar PostgreSQL:

1. Abra el terminal de WSL (es decir, Ubuntu 18,04).
2. Actualización de los paquetes Ubuntu: `sudo apt update`
3. Una vez que los paquetes se hayan actualizado, instale PostgreSQL (y el paquete-contrib que tenga algunas utilidades útiles) con: `sudo apt install postgresql postgresql-contrib`
4. Confirmar la instalación y obtener el número de versión: `psql --version`

Hay 3 comandos que debe conocer una vez instalado PostgreSQL:

1. `sudo service postgresql status` para comprobar el estado de la base de datos.
2. `sudo service postgresql start` para empezar a ejecutar la base de datos.
3. `sudo service postgresql stop` para dejar de ejecutar la base de datos.

### <a name="postgresql-user-setup"></a>Configuración de usuario de PostgreSQL

El usuario administrador predeterminado, `postgres`, necesita una contraseña asignada para poder conectarse a una base de datos. Para establecer una contraseña:

1. Escriba el comando: `sudo passwd postgres`
2. Recibirá un mensaje para escribir la nueva contraseña.
3. Cierre y vuelva a abrir el terminal.

### <a name="run-postgresql-with-psql-shell"></a>Ejecución de PostgreSQL con el shell de psql

[psql](https://www.postgresql.org/docs/10/app-psql.html) es un front-end basado en terminal en PostgreSQL. Le permite escribir consultas de forma interactiva, emitirlos a PostgreSQL y ver los resultados de la consulta. Como alternativa, la entrada puede ser de un archivo. Además, proporciona una serie de metacomandos y diversas características similares a los shells para facilitar la escritura de scripts y la automatización de una amplia variedad de tareas.

Para iniciar el shell de psql:

1. Inicie el servicio Postgres: `sudo service postgresql start`
2. Conéctese al servicio Postgres y abra el shell de psql: `sudo -u postgres psql`

Una vez que haya escrito correctamente el shell de psql, verá que el cambio de la línea de comandos tiene el siguiente aspecto: `postgres=#`

> [!NOTE]
> Como alternativa, puede abrir el shell de psql cambiando al usuario postgres con: `su - postgres` y, a continuación, escribiendo el comando: `psql`.

Para salir de Postgres = # Enter: `\q` o use la tecla de método abreviado: Ctrl+D

Para ver qué cuentas de usuario se han creado en la instalación de PostgreSQL, use desde el terminal WSL: `psql -c "\du"`... o simplemente `\du` si tiene abierto el shell de psql. Este comando mostrará las columnas: Nombre de usuario de la cuenta, lista de atributos de roles y miembro de los grupos de roles. Para salir a la línea de comandos, escriba: `q`.

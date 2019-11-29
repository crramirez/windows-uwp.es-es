---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314909"
---
Escribir `sudo service mongodb start` o `sudo service postgres start` y `sudo -u postgrest psql` puede resultar tedioso.  Sin embargo, puedes considerar la posibilidad de configurar los alias en el archivo `.profile` de WSL para que estos comandos sean más rápidos de usar y más fáciles de recordar. 

Para configurar tu propio alias personalizado, o acceso directo, a fin de ejecutar estos comandos:

1. Abre el terminal de WSL y escribe `cd ~` para asegurarte de que te encuentras en el directorio raíz.
2. Abre el archivo `.profile`, que controla la configuración del terminal, con el editor de texto de terminal Nano: `sudo nano .profile`.
3. En la parte inferior del archivo (no cambies la configuración `# set PATH`), agrega lo siguiente:

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Esto te permitirá escribir `start-pg` para empezar a ejecutar el servicio PostgreSQL y `run-pg` para abrir el shell psql. Puedes cambiar `start-pg` y `run-pg` a cualquier nombre que quieras, pero ten cuidado de no sobrescribir un comando que Postgres ya uses.

4. Una vez que hayas agregado los nuevos alias, sal del editor de texto de Nano con **Control + X**, selecciona `Y` (Sí) cuando se te pida que guardes y presiona Entrar (el nombre de archivo debe ser `.profile`).
5. Cierra y vuelve a abrir el terminal de WSL y, a continuación, prueba los nuevos comandos de alias.

---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314909"
---
Escribir `sudo service mongodb start` o `sudo service postgres start` y `sudo -u postgrest psql` puede resultar tedioso.  Sin embargo, puede considerar la posibilidad de configurar los alias en el archivo `.profile` en WSL para que estos comandos sean más fáciles de usar y más fáciles de recordar. 

Para configurar su propio alias personalizado, o acceso directo, para ejecutar estos comandos:

1. Abra el terminal WSL y escriba `cd ~` para asegurarse de que se encuentra en el directorio raíz.
2. Abra el archivo `.profile`, que controla la configuración del terminal, con el editor de texto de terminal, nano: `sudo nano .profile`
3. En la parte inferior del archivo (no cambie la configuración de `# set PATH`), agregue lo siguiente:

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Esto le permitirá escribir `start-pg` para empezar a ejecutar el servicio PostgreSQL y `run-pg` para abrir el shell psql. Puede cambiar `start-pg` y `run-pg` a cualquier nombre que desee, pero tenga cuidado de no sobrescribir un comando que Postgres ya use.

4. Una vez que haya agregado los nuevos alias, salga del editor de texto de nano con **Ctrl + X** --Select `Y` (sí) cuando se le pida que guarde y escriba (el nombre de archivo debe ser `.profile`).
5. Cierre y vuelva a abrir el terminal de WSL y, a continuación, pruebe los nuevos comandos de alias.

---
title: Introducción al uso de contenedores de Docker con Node.js
description: Una guía detallada que te ayudará a empezar a usar contenedores de Docker con las aplicaciones Node.js.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "75683678"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Introducción al uso de contenedores de Docker con Node.js

Una guía detallada que te ayudará a empezar a usar contenedores de Docker con las aplicaciones Node.js.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya has completado los pasos para [configurar el entorno de desarrollo de Node.js con WSL 2](./setup-on-wsl2.md), que incluyen:

- Instalar la compilación 18932 o posterior de Windows 10 Insider Preview.
- Habilitar la característica WSL 2 en Windows.
- Instalar una distribución de Linux (Ubuntu 18.04 para nuestros ejemplos). Puedes comprobarlo con: `wsl lsb_release -a`.
- Asegurarte de que la distribución de Ubuntu 18.04 se está ejecutando en modo WSL 2. (WSL puede ejecutar distribuciones en los modos V1 o V2). Para comprobarlo, abre PowerShell y escribe: `wsl -l -v`.
- Con PowerShell, establece Ubuntu 18.04 como la distribución predeterminada, con: `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Introducción a los contenedores de Docker

**Docker** es una herramienta que se usa para crear, implementar y ejecutar aplicaciones mediante contenedores. Los contenedores permiten a los desarrolladores empaquetar una aplicación con todas las partes que necesita (bibliotecas, marcos de trabajo, dependencias, etc.) y enviar todo como un paquete. El uso de un contenedor garantiza que la aplicación se ejecutará de la misma forma, independientemente de la configuración personalizada o de las bibliotecas instaladas anteriormente en el equipo en el que se ejecute, que podría diferir del equipo que se usó para escribir y probar el código de la aplicación. Esto permite a los desarrolladores centrarse en la escritura de código sin preocuparse por el sistema en el que se ejecutará el código.

Los contenedores de Docker son similares a las máquinas virtuales, pero no crean un sistema operativo virtual completo. En su lugar, Docker permite a la aplicación usar el mismo kernel de Linux que el sistema en el que se ejecuta. Esto permite que el paquete de la aplicación solo requiera partes que aún no existen en el equipo host, lo que reduce el tamaño del paquete y mejora el rendimiento.

La disponibilidad continua, mediante el uso de contenedores de Docker con herramientas como [Kubernetes](https://docs.microsoft.com/azure/aks/), es otro motivo para la popularidad de los contenedores. Esto permite crear varias versiones del contenedor de la aplicación en momentos diferentes. En lugar de tener que deshacer todo el sistema para las actualizaciones o el mantenimiento, cada contenedor y sus microservicios específicos se pueden reemplazar sobre la marcha. Puedes preparar un nuevo contenedor con todas las actualizaciones, configurar el contenedor para producción y simplemente apuntar al nuevo contenedor una vez que esté listo. También puedes archivar versiones diferentes de la aplicación mediante contenedores y mantenerlas en ejecución como reserva de seguridad si es necesario.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Instalación de Docker Desktop WSL 2 Tech Preview

Anteriormente, WSL 1 no podía ejecutar directamente el demonio de Docker, pero cambió con WSL 2 y dio lugar a mejoras significativas en la velocidad y el rendimiento con Docker Desktop para WSL 2.

Para instalar y ejecutar Docker Desktop WSL 2 Tech Preview:

1. Descarga el [Instalador de Docker Desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe). (Puedes hacer referencia a los [documentos del instalador](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) si es necesario).

2. Abre el instalador de Docker que acabas de descargar. El Asistente para la instalación te preguntará si deseas "usar contenedores Windows en lugar de contenedores Linux"; no actives esta casilla, ya que vamos a usar el subsistema Linux. Docker se instalará en un directorio administrado en la distribución predeterminada de WSL 2 e incluirá el demonio de Docker, la CLI y la CLI de Compose.

    ![Inicio de Docker Desktop](../images/install-docker-1.png)

3. Si aún no tienes un identificador de Docker, tendrás que configurar uno visitando: [https://hub.docker.com/signup](https://hub.docker.com/signup). El identificador debe tener todos los caracteres alfanuméricos en minúsculas.

4. Una vez instalado, inicia el escritorio de Docker seleccionando el icono de acceso directo en el escritorio o buscándolo en el menú Inicio de Windows. El icono de Docker aparecerá en el menú de iconos ocultos de la barra de tareas. Haz clic con el botón derecho en el icono para mostrar el menú de comandos de Docker y selecciona "WSL 2 Tech Preview".

5. Una vez que se abra la ventana de la vista previa técnica, selecciona **Iniciar** para empezar a ejecutar el demonio de Docker (proceso en segundo plano) en WSL 2. Cuando se inicia el demonio de Docker de WSL 2, se crea automáticamente un contexto de la CLI de Docker.

    ![Inicio de Docker Desktop](../images/start-docker.gif)

6. Para confirmar que se ha instalado Docker y mostrar el número de versión, abre una línea de comandos (WSL o PowerShell) y escribe: `docker --version`

7. Comprueba que la instalación funciona correctamente mediante la ejecución de una imagen de Docker integrada simple: `docker run hello-world`

Estos son algunos comandos de Docker que debes conocer:

- Enumerar los comandos disponibles en la CLI de Docker, para lo que debes escribir: `docker`
- Enumerar información de un comando específico con: `docker <COMMAND> --help`
- Enumerar las imágenes de Docker en el equipo (que es simplemente la imagen de Hola mundo en este momento), con: `docker image ls --all`
- Enumerar los contenedores del equipo, con: `docker container ls --all`
- Enumerar los recursos y las estadísticas del sistema de Docker (CPU y memoria) disponibles en el contexto de WSL 2, con: `docker info`
- Mostrar dónde se ejecuta Docker actualmente, con: `docker context ls`

Puedes ver que hay dos contextos en los que se ejecuta Docker: `default` (el demonio de Docker clásico) y `wsl` (nuestra recomendación mediante la versión preliminar técnica). (Además, el comando `ls` es corto para `list` y se puede usar indistintamente).

![Contexto de presentación de Docker en PowerShell](../images/docker-context.png)

> [!TIP]
> Intenta crear una imagen de Docker de ejemplo con este [tutorial sobre Docker Hub](https://hub.docker.com/?overlay=onboarding). Docker Hub también contiene muchos miles de imágenes de código abierto que podrían coincidir con el tipo de aplicación que deseas incluir en un contenedor. Puedes descargar imágenes, como este [contenedor del marco de trabajo de Gatsby.js](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) o este [contenedor del marco de trabajo de Nuxt.js](https://hub.docker.com/r/hobord/nuxtexpress), y ampliarlas con tu propio código de la aplicación. Puedes buscar en el registro mediante [Docker desde la línea de comandos](https://docs.docker.com/engine/reference/commandline/search/) o desde el [sitio web de Docker Hub](https://hub.docker.com/search/?type=image).

## <a name="install-the-docker-extension-on-vs-code"></a>Instalación de la extensión de Docker en VS Code

La extensión de Docker facilita la compilación, administración e implementación de aplicaciones en contenedor desde Visual Studio Code.

1. Abre la ventana **Extensiones** (Ctrl+Mayús+X) en VS Code y busca **Docker**.

2. Selecciona la [extensión de Docker para Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) e **instálala**. Tendrás que volver a cargar VS Code después de instalar para habilitar la extensión.

    ![Extensión de Docker en VS Code en Remote-WSL](../images/docker-vscode-extension.png)

Mediante la instalación de la extensión de Docker en VS Code, ahora podrás abrir una lista de comandos `Dockerfile` que se usan en la sección siguiente con el método abreviado: `Ctrl+Space`.

Obtén más información sobre cómo [trabajar con Docker en VS Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Creación de una imagen de contenedor con DockerFile

Una **imagen de contenedor** almacena el código de la aplicación, las bibliotecas, los archivos de configuración, las variables de entorno y el tiempo de ejecución. El uso de una imagen garantiza que el entorno del contenedor esté normalizado y contenga solo lo necesario para compilar y ejecutar la aplicación.

Un archivo **DockerFile** contiene las instrucciones necesarias para compilar una nueva imagen del contenedor. En otras palabras, este archivo compila la imagen de contenedor que define el entorno de la aplicación para que pueda reproducirse en cualquier lugar.

Vamos a compilar una imagen de contenedor con la siguiente aplicación Next.js configurada en la guía de [marcos web](./web-frameworks.md).

1. Abre la siguiente aplicación Next.js en VS Code (asegurándote de que la extensión Remote-WSL se está ejecutando como se indica en la pestaña verde inferior izquierda). Abre el terminal WSL integrado en VS Code (**Ver > Terminal**) y asegúrate de que la ruta de acceso del terminal apunta al directorio del proyecto Next.js siguiente (por ejemplo, `~/NextProjects/my-next-app$`).

2. Crea un archivo llamado `Dockerfile` en la raíz del proyecto Next.js y agrega lo siguiente:

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. Para compilar la imagen de Docker, ejecuta el siguiente comando desde la raíz del proyecto (pero reemplaza `<your_docker_username>` por el nombre de usuario creado en Docker Hub): `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker debe ejecutarse con WSL Tech Preview para que este comando funcione. Para obtener un recordatorio de cómo iniciar Docker, consulta el [paso 4](#install-docker-desktop-wsl-2-tech-preview) de la sección de instalación. La marca `-t` especifica el nombre de la imagen que se va a crear, "my-nextjs-app:v1" en nuestro caso. Se recomienda que siempre [uses una versión # en los nombres de etiqueta](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) al crear una imagen. Asegúrate de incluir el punto al final del comando, que especifica que se debe usar el directorio de trabajo actual para buscar y copiar los archivos de compilación para la aplicación Next.js.

4. Para ejecutar esta nueva imagen de Docker de la aplicación Next.js en un contenedor, escribe el comando: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. La marca `-p` enlaza el puerto "3000" (el puerto en el que se ejecuta la aplicación dentro del contenedor) al puerto local "3333" en el equipo, por lo que ahora puedes apuntar el explorador web a [http://localhost:3333](http://localhost:3333) y ver la aplicación Next.js representada en el servidor que se ejecuta como una imagen de contenedor de Docker.

> [!TIP]
> Hemos creado nuestra imagen de contenedor con `FROM node:12`, que hace referencia a la imagen predeterminada de Node.js, versión 12, almacenada en Docker Hub. Esta imagen predeterminada de Node.js se basa en un sistema Debian/Ubuntu Linux; sin embargo, hay muchas imágenes de Node.js diferentes entre las que elegir, y debes considerar la posibilidad de usar algo más ligero o adaptado a tus necesidades. Obtén más información en el [Registro de imágenes de Node.js en Docker Hub](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Carga de la imagen de un contenedor en un repositorio

Un **repositorio de contenedores** almacena la imagen de contenedor en la nube. A menudo, un repositorio de contenedores incluye realmente una colección de imágenes relacionadas, como versiones diferentes, que están disponibles para facilitar la instalación y una rápida implementación. Normalmente, puedes acceder a las imágenes de los repositorios de contenedores a través de puntos de conexión HTTPS seguros, lo que permite extraer, insertar o administrar imágenes a través de cualquier sistema, hardware o instancia de máquina virtual.

Por otro lado, un **registro de contenedor** almacena una colección de repositorios, así como índices, reglas de control de acceso y rutas de acceso de API. Estos se pueden hospedar de forma pública o privada. [Docker Hub](https://hub.docker.com/) es un registro de Docker de código abierto y la opción predeterminada utilizada para ejecutar los comandos `docker push` y `docker pull`. Es gratis para los repositorios públicos y requiere una tarifa para los repositorios privados.

Para cargar la nueva imagen de contenedor en un repositorio hospedado en Docker Hub:

1. Inicia sesión en Docker Hub. Se te pedirá que escribas el nombre de usuario y la contraseña utilizados para crear la cuenta de Docker Hub durante el paso de instalación. Para iniciar sesión en Docker en el terminal, escribe: `docker login`

2. Para obtener una lista de las imágenes de contenedor de Docker que has creado en el equipo, escribe: `docker image ls --all`

3. Inserta la imagen de contenedor en Docker Hub y crea ahí un repositorio para ella mediante este comando: `docker push <your_docker_username>/my-nextjs-app:v1`

4. Ahora puedes ver el repositorio en Docker Hub, escribir una descripción y vincular la cuenta de GitHub (si lo deseas); para ello, visita: https://cloud.docker.com/repository/list

5. También puedes ver una lista de los contenedores de Docker activos con: `docker container ls` o `docker ps`

6. Deberías ver que el contenedor "my-nextjs-app:v1" está activo en el puerto 3333 ->3000/tcp. También puedes ver el "ID. DE CONTENEDOR" que se muestra aquí. Para detener la ejecución del contenedor, escribe el comando: `docker stop <container ID>`

7. Normalmente, una vez que se detiene un contenedor, también se debe quitar. Al quitar un contenedor, se limpian todos los recursos incluidos en él. Una vez que se quita un contenedor, los cambios que se hayan realizado dentro de su sistema de archivos de imagen se perderán de forma permanente. Tendrás que crear una imagen para representar los cambios. Para quitar el contenedor, usa el comando: `docker rm <container ID>`.

Más información sobre la [creación de una aplicación web en contenedor con Docker](https://docs.microsoft.com/learn/modules/intro-to-containers/)

## <a name="deploy-to-azure-container-registry"></a>Implementación en Azure Container Registry

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) permite almacenar, administrar y mantener las imágenes de contenedor seguras en repositorios privados y autenticados. ACR, que es compatible con los comandos estándar de Docker, puede controlar las tareas críticas de forma automática, como la supervisión y el mantenimiento del estado del contenedor, además del emparejamiento con [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) para crear sistemas de orquestación escalables. Compila a petición o automatiza completamente las compilaciones con desencadenadores, como confirmaciones de código fuente y actualizaciones de imágenes base. ACR también usa la amplia red de Azure Cloud para administrar la latencia de red y las implementaciones globales y crear una experiencia nativa sin problemas para cualquier persona que use [Azure App Service](https://docs.microsoft.com/azure/app-service/) (para hospedaje web, back-ends móviles y API de REST) u [otros servicios en la nube de Azure](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Necesitas tu propia suscripción de Azure para implementar un contenedor en Azure, y puede que se aplique algún cargo. Si aún no tienes una suscripción de Azure, [crea una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

Para obtener ayuda para crear una instancia de Azure Container Registry e implementar la imagen de contenedor de la aplicación, consulta el ejercicio: [Implementación de una imagen de Docker en una instancia de Azure Container](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Recursos adicionales

- [Node.js en Azure](https://azure.microsoft.com/develop/nodejs/)
- Inicio rápido: [Creación de una aplicación web Node.js en Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Curso en línea: [Administración de contenedores en Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Usar VS Code: [Trabajar con Docker](https://code.visualstudio.com/docs/azure/docker)
- Documentos de Docker: [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)

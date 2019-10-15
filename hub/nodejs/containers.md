---
title: Introducción al uso de contenedores de Docker con node. js
description: Una guía paso a paso que le ayudará a empezar a usar contenedores de Docker con sus aplicaciones de node. js.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 16b1421606d3c8271141256b80ae2600ec9ca49d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315129"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Introducción al uso de contenedores de Docker con node. js

Una guía paso a paso que le ayudará a empezar a usar contenedores de Docker con sus aplicaciones de node. js.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya ha completado los pasos para [configurar el entorno de desarrollo de node. js con WSL 2](./setup-on-wsl2.md), entre los que se incluyen:

- Instale Windows 10 Insider Preview compilación 18932 o posterior.
- Habilite la característica WSL 2 en Windows.
- Instale una distribución de Linux (Ubuntu 18,04 para nuestros ejemplos). Puede comprobarlo con: `wsl lsb_release -a`.
- Asegúrese de que la distribución de Ubuntu 18,04 se está ejecutando en el modo WSL 2. (WSL puede ejecutar distribuciones en el modo V1 o V2). Para comprobarlo, abra PowerShell y escriba: `wsl -l -v`.
- Con PowerShell, establezca Ubuntu 18,04 como su distribución predeterminada, con: `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Información general sobre los contenedores de Docker

**Docker** es una herramienta que se usa para crear, implementar y ejecutar aplicaciones mediante contenedores. Los contenedores permiten a los desarrolladores empaquetar una aplicación con todas las partes que necesita (bibliotecas, marcos de trabajo, dependencias, etc.) y enviarlo todo como un paquete. El uso de un contenedor garantiza que la aplicación se ejecutará de la misma forma, independientemente de la configuración personalizada o de las bibliotecas instaladas anteriormente en el equipo en el que se ejecute, que podría diferir de la máquina que se usó para escribir y probar el código de la aplicación. Esto permite a los desarrolladores centrarse en la escritura de código sin preocuparse por el sistema en el que se ejecutará el código.

Los contenedores de Docker son similares a las máquinas virtuales, pero no crean un sistema operativo virtual completo. En su lugar, Docker permite a la aplicación usar el mismo kernel de Linux que el sistema en el que se está ejecutando. Esto permite que el paquete de la aplicación solo requiera partes no existentes en el equipo host, lo que reduce el tamaño del paquete y mejora el rendimiento.

La disponibilidad continua, el uso de contenedores de Docker con herramientas como [Kubernetes](https://docs.microsoft.com/azure/aks/), es otro motivo para la popularidad de los contenedores. Esto permite crear varias versiones del contenedor de la aplicación en momentos diferentes. En lugar de tener que deshacer todo el sistema para las actualizaciones o el mantenimiento, cada contenedor (y sus microservicios específicos) se puede reemplazar sobre la marcha. Puede preparar un nuevo contenedor con todas las actualizaciones, configurar el contenedor para producción y simplemente apuntar al nuevo contenedor una vez que esté listo. También puede archivar versiones diferentes de la aplicación mediante contenedores y mantenerlas en ejecución como reserva de seguridad si es necesario.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Instalación de Docker Desktop WSL 2 Tech Preview

Anteriormente, WSL 1 no podía ejecutar directamente el demonio de Docker, pero cambió con WSL 2 y dio lugar a mejoras significativas en la velocidad y el rendimiento con Docker Desktop para WSL 2.

Para instalar y ejecutar Docker Desktop WSL 2 Tech Preview:

1. Descargue el [instalador de Docker Desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe). (Puede hacer referencia a los [documentos del instalador](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) si es necesario).

2. Abra el instalador de Docker que acaba de descargar. El Asistente para la instalación le preguntará si desea "usar contenedores de Windows en lugar de contenedores de Linux". no active esta casilla, ya que vamos a usar el subsistema Linux. Docker se instalará en un directorio administrado en la distribución predeterminada de WSL 2 e incluirá el demonio de Docker, la CLI y la CLI de redacción.

    ![Inicio de Docker Desktop](../images/install-docker-1.png)

3. Si aún no tiene un identificador de Docker, tendrá que configurar uno visitando: [https://hub.docker.com/signup](https://hub.docker.com/signup). El identificador debe tener todos los caracteres alfanuméricos en minúsculas.

4. Una vez instalado, inicie el escritorio de Docker seleccionando el icono de acceso directo en el escritorio o buscándolo en el menú Inicio de Windows. El icono de Docker aparecerá en el menú iconos ocultos de la barra de tareas. Haga clic con el botón derecho en el icono para mostrar el menú de comandos de Docker y seleccione "WSL 2 Tech Preview".

5. Una vez que se abra la ventana de vista previa técnica, seleccione **iniciar** para empezar a ejecutar el demonio de Docker (proceso en segundo plano) en WSL 2. Cuando se inicia el demonio de Docker de WSL 2, se crea automáticamente un contexto de la CLI de Docker.

    ![Inicio de Docker Desktop](../images/start-docker.gif)

6. Para confirmar que se ha instalado Docker y mostrar el número de versión, abra una línea de comandos (WSL o PowerShell) y escriba: `docker --version`

7. Compruebe que la instalación funciona correctamente mediante la ejecución de una imagen de Docker integrada simple: `docker run hello-world`

Estos son algunos comandos de Docker que debe conocer:

- Para enumerar los comandos disponibles en la CLI de Docker, escriba: `docker`
- Mostrar información de un comando específico con: `docker <COMMAND> --help`
- Enumerar las imágenes de Docker en el equipo (que es simplemente la imagen de Hola mundo en este momento), con: `docker image ls --all`
- Enumerar los contenedores del equipo, con: `docker container ls --all`
- Enumere los recursos y las estadísticas del sistema de Docker (CPU &) disponibles en el contexto de WSL 2, con: `docker info`
- Mostrar dónde se ejecuta Docker actualmente, con: `docker context ls`

Puede ver que hay dos contextos en los que se ejecuta Docker: `default` (el demonio de Docker clásico) y `wsl` (nuestra recomendación mediante la versión preliminar técnica). (Además, el comando `ls` es breve para `list` y se puede usar indistintamente).

![Contexto de pantalla de Docker en PowerShell](../images/docker-context.png)

> [!TIP]
> Intente crear una imagen de Docker de ejemplo con este [tutorial en Docker Hub](https://hub.docker.com/?overlay=onboarding). Docker Hub también contiene muchos miles de imágenes de código abierto que podrían coincidir con el tipo de aplicación que desea incluir en un contenedor. Puede descargar imágenes, como este contenedor de [.net Gatsby. js](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) o este [contenedor de .NET Framework de Nuxt. js](https://hub.docker.com/r/hobord/nuxtexpress), y ampliarla con su propio código de aplicación. Puede buscar en el registro mediante [Docker desde la línea de comandos o desde](https://docs.docker.com/engine/reference/commandline/search/) el [sitio web de Docker Hub](https://hub.docker.com/search/?type=image).

## <a name="install-the-docker-extension-on-vs-code"></a>Instale la extensión de Docker en VS Code

La extensión Docker facilita la compilación, administración e implementación de aplicaciones en contenedor desde Visual Studio Code.

1. Abra la ventana **extensiones** (Ctrl + Mayús + X) en vs Code y busque **Docker**.

2. Seleccione la [extensión de Docker de Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) e **instálelo**. Tendrá que volver a cargar VS Code después de instalar para habilitar la extensión.

    ![Extensión de Docker en VS Code en Remote-WSL](../images/docker-vscode-extension.png)

Al instalar la extensión de Docker en VS Code, ahora podrá abrir una lista de comandos `Dockerfile` que se usan en la sección siguiente con el método abreviado: `Ctrl+Space`

Más información sobre cómo [trabajar con Docker en vs Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Creación de una imagen de contenedor con DockerFile

Una **imagen de contenedor** almacena el código de la aplicación, las bibliotecas, los archivos de configuración, las variables de entorno y el tiempo de ejecución. El uso de una imagen garantiza que el entorno del contenedor esté normalizado y contenga solo lo necesario para compilar y ejecutar la aplicación.

Un **DockerFile** contiene las instrucciones necesarias para compilar la nueva imagen de contenedor. En otras palabras, este archivo crea la imagen de contenedor que define el entorno de la aplicación para que pueda reproducirse en cualquier lugar.

Vamos a compilar una imagen de contenedor con la siguiente aplicación. js configurada en la guía de [Marcos web](./web-frameworks.md) .

1. Abra la siguiente aplicación. js en VS Code (asegurándose de que la extensión Remote-WSL se está ejecutando como se indica en la pestaña verde inferior izquierda). Abra el terminal WSL integrado en VS Code (**Ver terminal de >** ) y asegúrese de que la ruta de acceso del terminal apunta al directorio del proyecto. js siguiente (es decir, `~/NextProjects/my-next-app$`).

2. Cree un nuevo archivo llamado `Dockerfile` en la raíz del proyecto. js siguiente y agregue lo siguiente:

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

3. Para compilar la imagen de Docker, ejecute el siguiente comando desde la raíz del proyecto (pero reemplace `<your_docker_username>` por el nombre de usuario que creó en Docker hub): `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker debe ejecutarse con WSL Tech Preview para que este comando funcione. Para obtener un recordatorio de cómo iniciar Docker, consulte el [paso #4](#install-docker-desktop-wsl-2-tech-preview) de la sección instalación. La marca `-t` especifica el nombre de la imagen que se va a crear, "My-nextjs-App: v1" en nuestro caso. Se recomienda [usar siempre una versión # en los nombres de etiqueta](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) al crear una imagen. Asegúrese de incluir el punto al final del comando, que especifica que se debe usar el directorio de trabajo actual para buscar y copiar los archivos de compilación de la aplicación siguiente. js.

4. Para ejecutar esta nueva imagen de Docker de la siguiente aplicación. js en un contenedor, escriba el comando: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. La marca `-p` enlaza el puerto ' 3000 ' (el puerto en el que se ejecuta la aplicación dentro del contenedor) al puerto local ' 3333 ' en el equipo, por lo que ahora puede apuntar el explorador Web a [http://localhost:3333](http://localhost:3333) y ver la siguiente aplicación representada del lado servidor que se ejecuta como Docker. imagen de contenedor.

> [!TIP]
> Hemos creado nuestra imagen de contenedor mediante `FROM node:12`, que hace referencia a la imagen predeterminada de node. js versión 12 almacenada en Docker Hub. Esta imagen predeterminada de node. js se basa en un sistema Debian/Ubuntu Linux, sin embargo, hay muchas imágenes de node. js diferentes entre las que elegir, y puede que desee considerar la posibilidad de usar algo más ligero o adaptado a sus necesidades. Obtenga más información en el [registro de imágenes de node. js en Docker Hub](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Carga de la imagen de contenedor en un repositorio

Un **repositorio de contenedor** almacena la imagen de contenedor en la nube. A menudo, un repositorio de contenedores contiene realmente una colección de imágenes relacionadas, como versiones diferentes, que están disponibles para facilitar la instalación y la rápida implementación. Normalmente, puede tener acceso a las imágenes de los repositorios de contenedores a través de puntos de conexión HTTPs seguros, lo que le permite extraer, insertar o administrar imágenes a través de cualquier sistema, hardware o instancia de máquina virtual.

Por otro lado, un **registro de contenedor**almacena una colección de repositorios, así como índices, reglas de control de acceso y rutas de acceso de API. Estos se pueden hospedar de forma pública o privada. [Docker Hub](https://hub.docker.com/) es un registro de Docker de código abierto y el valor predeterminado que se usa cuando se ejecutan los comandos `docker push` y `docker pull`. Es gratis para los repositorios públicos y requiere una tarifa por repositorios privados.

Para cargar la nueva imagen de contenedor en un repositorio hospedado en Docker Hub:

1. Inicie sesión en Docker Hub. Se le pedirá que escriba el nombre de usuario y la contraseña que usó para crear su cuenta de Docker Hub durante el paso de instalación. Para iniciar sesión en Docker en el terminal, escriba: `docker login`

2. Para obtener una lista de las imágenes de contenedor de Docker que ha creado en el equipo, escriba: `docker image ls --all`

3. Inserte la imagen de contenedor en el concentrador de Docker y cree un nuevo repositorio para ella allí con este comando: `docker push <your_docker_username>/my-nextjs-app:v1`

4. Ahora puede ver el repositorio en Docker Hub, escribir una descripción y vincular la cuenta de GitHub (si lo desea), visitando: https://cloud.docker.com/repository/list

5. También puede ver una lista de los contenedores de Docker activos con: `docker container ls` (o `docker ps`)

6. Debería ver que el contenedor "My-nextjs-App: v1" está activo en el puerto 3333-> 3000/TCP. También puede ver el "ID. de contenedor" que se muestra aquí. Para detener la ejecución del contenedor, escriba el comando: `docker stop <container ID>`

7. Normalmente, una vez que se detiene un contenedor, también se debe quitar. Al quitar un contenedor se limpian todos los recursos que mantiene detrás. Una vez que se quita un contenedor, los cambios que se hayan realizado dentro de su sistema de archivos de imagen se perderán de forma permanente. Tendrá que crear una nueva imagen para representar los cambios. PARA quitar el contenedor, use el comando: `docker rm <container ID>`

Más información sobre [la creación de una aplicación web en contenedor con Docker](https://docs.microsoft.com/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Implementar en Azure Container Registry

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) le permite almacenar, administrar y mantener las imágenes de contenedor seguras en repositorios privados y autenticados. Compatible con los comandos estándar de Docker, ACR puede controlar las tareas críticas que le gustan, como la supervisión y el mantenimiento del mantenimiento del contenedor, emparejando con [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) para crear sistemas de orquestación escalables. Compile a petición o automatice las compilaciones con desencadenadores como confirmaciones de código fuente y actualizaciones de imagen base. ACR también aprovecha la vasta red en la nube de Azure para administrar la latencia de red, las implementaciones globales y crear una experiencia nativa sin problemas para cualquier persona que use [Azure App Service](https://docs.microsoft.com/azure/app-service/) (para hospedaje Web, back-end móvil, API de REST) u [otros servicios en la nube de Azure. ](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Necesita su propia suscripción de Azure para implementar un contenedor en Azure y puede recibir un cargo. Si aún no tiene una suscripción a Azure, [cree una cuenta gratuita antes de](https://azure.microsoft.com/free/) comenzar.

Para obtener ayuda para crear un Azure Container Registry e implementar la imagen de contenedor de la aplicación, consulte el ejercicio: [Implemente una imagen de Docker en una instancia de Azure Container Instance](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Recursos adicionales

- [Node. js en Azure](https://azure.microsoft.com/en-us/develop/nodejs/)
- Inicio rápido [Creación de una aplicación Web de node. js en Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Curso en línea: [Administración de contenedores en Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Usar VS Code: [Trabajar con Docker](https://code.visualstudio.com/docs/azure/docker)
- Documentos de Docker: [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)

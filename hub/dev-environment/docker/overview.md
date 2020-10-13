---
title: Introducción a Docker para el desarrollo remoto con contenedores
description: Guía para empezar a trabajar con Docker Desktop en Windows o WSL.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, desarrollo remoto, contenedores, Docker Desktop, Windows frente a WSL
ms.date: 09/24/2020
ms.openlocfilehash: b3fc2509aa6a623bebd9f4566f3b8e75301251db
ms.sourcegitcommit: c65f62bda57563f6196691e7b9c25cbf5a8b16e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91780597"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>Información general sobre el desarrollo remoto con Docker en Windows

El uso de contenedores para el desarrollo remoto y la implementación de aplicaciones con la plataforma Docker es una solución muy popular que aporta muchas ventajas. Obtenga más información sobre la variedad de compatibilidades que ofrecen las herramientas y servicios de Microsoft, incluido el Subsistema de Windows para Linux (WSL), Visual Studio, Visual Studio Code, .NET y una amplia variedad de servicios de Azure.

## <a name="docker-on-windows-10"></a>Docker en Windows 10

:::row:::
    :::column:::
       [![Icono de Docker Docs](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[Instalación de Docker Desktop para Windows](https://docs.docker.com/docker-for-windows/install/)**<br>
        Busque los pasos de instalación, los requisitos del sistema, lo que se incluye en el instalador, cómo desinstalar, las diferencias entre las versiones estable y perimetral, y cómo cambiar entre los contenedores de Windows y Linux.
    :::column-end:::
    :::column:::
       [![Captura de pantalla de Docker en ejecución](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[Introducción a Docker](https://docs.docker.com/get-started/)**<br>
        Documentos de guía e instalación de Docker con instrucciones paso a paso sobre cómo empezar, incluido un tutorial en vídeo.
    :::column-end:::
    :::column:::
       [![Captura de pantalla del curso de Docker en Microsoft Learn](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[Curso de MS Learn: Introducción a los contenedores de Docker](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn ofrece un curso de introducción gratuito a los contenedores de Docker, además de una [variedad de cursos](/learn/browse/?terms=docker) sobre la introducción a Docker y la conexión con los servicios de Azure.
    :::column-end:::
    :::column:::
       [![Captura de pantalla del menú WSL2 de Docker Desktop](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[Introducción a los contenedores remotos de Docker en WSL 2](/windows/wsl/tutorials/wsl-containers)**<br>
        Obtenga información sobre cómo configurar Docker Desktop para Windows para usarlo con una línea de comandos de Linux (Ubuntu, Debian, SUSE, etc.) mediante WSL 2 (Subsistema de Windows para Linux, versión 2).
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code y Docker

:::row:::
    :::column:::
       [![Gráfico de contenedor remoto de VS Code](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[Cree un contenedor de Docker con VS Code](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        Configure un entorno de desarrollo completo dentro de un contenedor con la [extensión Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) y busque tutoriales para configurar un [contenedor de NodeJS](https://code.visualstudio.com/docs/containers/quickstart-node), un [contenedor de Python](https://code.visualstudio.com/docs/containers/quickstart-python) o un [contenedor de ASP.Net Core](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core).
    :::column-end:::
    :::column:::
       [![Captura de pantalla para asociar VSCode a Docker](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[Asociación de VS Code a un contenedor de Docker](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        Obtenga información sobre cómo asociar Visual Studio Code a un contenedor de Docker que ya esté en ejecución o a un [contenedor en un clúster de Kubernetes](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster).
    :::column-end:::
    :::column:::
       [![Captura de pantalla del menú de contenedor de VSCode](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[Configuración avanzada del contenedor](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        Obtenga información sobre los escenarios de configuración avanzada para usar contenedores de Docker con Visual Studio Code o lea este artículo sobre cómo [inspeccionar contenedores](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers) para depurar con VS Code.
    :::column-end:::
    :::column:::
       [![Captura de pantalla de Docker Desktop con WSL en VSCode](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[Uso de contenedores remotos en WSL 2](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        Lea sobre el uso de contenedores de Docker con WSL 2 (Subsistema de Windows para Linux, versión 2) y cómo configurar todo con VS Code. También puede leer sobre [cómo funciona](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works).
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio y Docker

:::row:::
    :::column:::
       [![Icono de Visual Studio](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Compatibilidad con Docker en Visual Studio](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        Obtenga información sobre la compatibilidad con Docker disponible para proyectos de ASP.NET, proyectos de ASP.NET Core y proyectos de consola de .NET Core y .NET Framework en Visual Studio, además de la compatibilidad con la orquestación de contenedores.
    :::column-end:::
    :::column:::
       [![Menú de Docker en Visual Studio](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[Inicio rápido: Docker en Visual Studio](/visualstudio/containers/container-tools)**<br>
        Obtenga información sobre cómo compilar, depurar y ejecutar aplicaciones de .NET, ASP.NET y ASP.NET Core en contenedores, y publicarlas en Azure Container Registry (ACR), Docker Hub, Azure App Service o su propio registro de contenedores con Visual Studio.
    :::column-end:::
    :::column:::
       [![Captura de pantalla del tutorial de VS](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[Tutorial: Creación de una aplicación de varios contenedores con Docker Compose](/visualstudio/containers/tutorial-multicontainer)**<br>
        Obtenga información sobre cómo administrar más de un contenedor y comunicarse entre ellos al usar Herramientas de contenedor en Visual Studio. También puede encontrar vínculos a tutoriales, como [Uso de Docker con una aplicación de página única de React](/visualstudio/containers/container-tools-react).
    :::column-end:::
    :::column:::
       [![Vínculos de contenedor de VS](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Herramientas de contenedor en Visual Studio](/visualstudio/containers)**<br>
        Busque temas en los que se explica cómo ejecutar las herramientas de compilación en un contenedor, [depurar aplicaciones de Docker](/visualstudio/containers/edit-and-refresh), solucionar problemas de las herramientas de desarrollo, implementar contenedores de Docker y Bridge to Kubernetes con Visual Studio.
    :::column-end:::
:::row-end:::

![Infografía de la taxonomía básica de Docker para contenedores, imágenes y registros](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core y Docker

:::row:::
    :::column:::
       [![Portada de la guía de microservicios de .NET](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[Guía de .NET: Aplicaciones y contenedores de microservicios](/dotnet/architecture/microservices/)**<br>
        Guía de introducción a las aplicaciones basadas en microservicios administradas con contenedores.
    :::column-end:::
    :::column:::
       [![Infografía de Docker](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[¿Qué es Docker?](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        Explicación básica de los contenedores de Docker, incluida [Comparación de los contenedores de Docker con las máquinas virtuales](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines) y una [taxonomía básica de los términos y conceptos de Docker](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries), donde se explica la diferencia entre los contenedores, las imágenes y los registros.
    :::column-end:::
    :::column:::
       [![Infografía de la taxonomía de Docker](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[Tutorial: Inclusión de una aplicación de .NET Core en un contenedor](/dotnet/core/docker/build-container?tabs=windows)**<br>
        Obtenga información sobre cómo incluir en un contenedor una aplicación de .NET Core con Docker, incluida la creación de un Dockerfile, comandos esenciales y la limpieza de recursos.
    :::column-end:::
    :::column:::
       [![Infografía de flujo de trabajo de desarrollo de bucle interno con Docker](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Flujo de trabajo de desarrollo para aplicaciones de Docker](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        Describe el flujo de trabajo de desarrollo de bucle interno para las aplicaciones basadas en contenedores de Docker.
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure Container Services

:::row:::
    :::column:::
       [![Captura de pantalla de Azure Container Instances](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure Container Instances](/azure/container-instances/)**<br>
        Aprenda a ejecutar contenedores de Docker a petición en un entorno de Azure administrado y sin servidor, que incluye formas de implementar con la CLI de Docker, ARM, Azure Portal, crear grupos de varios contenedores, compartir datos entre contenedores, conectarse a una red virtual, etc.
    :::column-end:::
    :::column:::
       [![Captura de pantalla de Azure Container Registry](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure Container Registry](/azure/container-registry)**<br>
        Obtenga información sobre cómo crear, almacenar y administrar imágenes de contenedor y artefactos en un registro privado para todos los tipos de implementaciones de contenedor. Cree registros de contenedor de Azure para las canalizaciones de implementación y desarrollo de contenedores existentes, configure tareas de automatización y aprenda a administrar sus registros, como la replicación geográfica y los procedimientos recomendados.
    :::column-end:::
    :::column:::
       [![Captura de pantalla de Azure Service Fabric](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        Obtenga información sobre Azure Service Fabric, una plataforma de sistemas distribuidos para empaquetar, implementar y administrar microservicios y contenedores escalables y de confianza.
    :::column-end:::
    :::column:::
       [![Captura de pantalla de Azure App Service](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure App Service](/azure/app-service)**<br>
        Aprenda a crear y hospedar aplicaciones web, back-ends móviles y las API de RESTful en el lenguaje de programación que prefiera sin necesidad de administrar la infraestructura. Pruebe el [curso de Azure App Service en MS Learn](/learn/modules/deploy-run-container-app-service) para implementar una aplicación web basada en una imagen de Docker y configurar la implementación continua.
    :::column-end:::
:::row-end:::

Obtenga información sobre más [servicios de Azure que admiten contenedores](https://azure.microsoft.com/overview/containers/).

## <a name="docker-containers-explainer-video"></a>Vídeo de explicación de contenedores de Docker

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Vídeo de explicación sobre la orquestación de contenedores y Kubernetes

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Contenedores en Windows

:::row:::
    :::column:::
       [![Icono de contenedores de Windows Server](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Documentos de contenedores en Windows](/virtualization/windowscontainers)**<br>
        Empaquete aplicaciones con sus dependencias y aproveche la virtualización de nivel de sistema operativo para ofrecer entornos rápidos y totalmente aislados en un único sistema. Obtenga información sobre los [contenedores de Windows](/virtualization/windowscontainers/about), como guías de inicio rápido, guías de implementación y ejemplos.
    :::column-end:::
    :::column:::
       [![Icono de preguntas frecuentes](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[Preguntas frecuentes sobre los contenedores de Windows](/virtualization/windowscontainers/about/faq)**<br>
        Busque las preguntas frecuentes sobre los contenedores. Consulte también esta explicación en StackOverflow en "[¿Cuál es la diferencia entre Docker para Windows y Docker en Windows?](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748)".
    :::column-end:::
    :::column:::
       [![icono de contenedor de windows](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[Configuración del entorno](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        Obtenga información sobre cómo configurar Windows 10 o Windows Server para crear, ejecutar e implementar contenedores, incluidos los requisitos previos, la instalación de Docker y el uso de [imágenes base de contenedor de Windows](/virtualization/windowscontainers/manage-containers/container-base-images).
    :::column-end:::
    :::column:::
       [![Icono de AKS](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[Creación de un contenedor de Windows Server en Azure Kubernetes Service (AKS)](/azure/aks/windows-container-cli)**<br>
        Obtenga información sobre cómo implementar una aplicación de ejemplo de ASP.NET de un contenedor de Windows Server en un clúster de AKS mediante la CLI de Azure.
    :::column-end:::
:::row-end:::

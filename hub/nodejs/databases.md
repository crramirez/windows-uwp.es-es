---
title: Introducción a la conexión de aplicaciones de Node.js a una base de datos
description: Empiece a usar a la conexión de aplicaciones de Node.js a una base de datos en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, aprendizaje de nodejs, node en windows, node en wsl, node en linux en windows, instalar node en windows, nodejs con vs code, desarrollar con node en windows, desarrollar con nodejs en windows, instalar node en WSL, NodeJS en el Subsistema de Windows para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517818"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Introducción al uso de MongoDB o PostgreSQL con Node.js en Windows

A menudo, las aplicaciones Node.js necesitan conservar los datos, lo que puede ocurrir a través de archivos, almacenamiento local, servicios en la nube o bases de datos. Esta guía detallada te ayudará a comenzar a conectar tu aplicación de Node.js a una base de datos. Decidimos centrarnos en dos opciones conocidas: MongoDB y PostgreSQL.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya has completado los pasos para [configurar el entorno de desarrollo de Node.js con WSL 2](./setup-on-wsl2.md), que incluyen:

- Instalar la compilación 18932 o posterior de Windows 10 Insider Preview.
- Habilitar la característica WSL 2 en Windows.
- Instalar una distribución de Linux (Ubuntu 18.04 para nuestros ejemplos). Puedes comprobarlo con: `wsl lsb_release -a`.
- Asegurarte de que la distribución de Ubuntu 18.04 se está ejecutando en modo WSL 2. (WSL puede ejecutar distribuciones en los modos V1 o V2). Para comprobarlo, abre PowerShell y escribe: `wsl -l -v`.
- Con PowerShell, establece Ubuntu 18.04 como la distribución predeterminada, con: `wsl -s ubuntu 18.04`.

## <a name="differences-between-mongodb-and-postgresql"></a>Diferencias entre MongoDB y PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Instalación de MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Compatibilidad de VS Code con MongoDB

VS Code permite trabajar con bases de datos de MongoDB mediante la [extensión de Azure CosmosDB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb). Puedes crear, administrar y consultar las bases de datos de MongoDB desde VS Code, así como conectarte a ellas.

Para obtener más información, visita la documentación de VS Code: [Trabajar con MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Obtén más información en los documentos de MongoDB:

- [Introducción al uso de MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Creación de usuarios](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Conexión a una instancia de MongoDB en un host remoto](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: crear, leer, actualizar y eliminar](https://docs.mongodb.com/manual/crud/)
- [Documentos de referencia](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Instalación de PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Compatibilidad de VS Code con PostgreSQL

VS Code permite trabajar con bases de datos de PostgreSQL mediante la [extensión de PostgreSQL ](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql). Puedes crear, administrar y consultar las bases de datos de PostgreSQL desde VS Code, así como conectarte a ellas.

## <a name="set-up-profile-aliases"></a>Configuración de alias de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]

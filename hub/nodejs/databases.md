---
title: Introducción a la conexión de aplicaciones de node. js a una base de datos
description: Introducción a la conexión de aplicaciones de node. js a una base de datos de Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, Learning NodeJS, nodo en Windows, nodo en WSL, nodo en Linux en Windows, nodo de instalación en Windows, NodeJS con vs Code, desarrollar con nodo en Windows, desarrollar con NodeJS en Windows, instalar nodo en WSL, NodeJS en Windows Subsistema para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517818"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Introducción al uso de MongoDB o PostgreSQL con node. js en Windows

A menudo, las aplicaciones node. js necesitan conservar los datos, lo que puede ocurrir a través de archivos, almacenamiento local, servicios en la nube o bases de datos. Esta guía paso a paso le ayudará a comenzar a conectar la aplicación node. js a una base de datos de. Decidimos centrarnos en dos opciones populares: MongoDB y PostgreSQL.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya ha completado los pasos para [configurar el entorno de desarrollo de node. js con WSL 2](./setup-on-wsl2.md), entre los que se incluyen:

- Instale Windows 10 Insider Preview compilación 18932 o posterior.
- Habilite la característica WSL 2 en Windows.
- Instale una distribución de Linux (Ubuntu 18,04 para nuestros ejemplos). Puede comprobarlo con: `wsl lsb_release -a`
- Asegúrese de que la distribución de Ubuntu 18,04 se está ejecutando en el modo WSL 2. (WSL puede ejecutar distribuciones en el modo V1 o V2). Para comprobarlo, abra PowerShell y escriba: `wsl -l -v`
- Con PowerShell, establezca Ubuntu 18,04 como su distribución predeterminada, con: `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>Diferencias entre MongoDB y PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Instalación de MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Compatibilidad de VS Code con MongoDB

VS Code admite el trabajo con bases de datos de MongoDB a través de la [extensión CosmosDB de Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), puede crear, administrar y consultar bases de datos de mongodb desde vs Code.

Para obtener más información, visite el VS Code docs: [trabajar con MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Obtenga más información en los documentos de MongoDB:

- [Introducción al uso de MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Crear usuarios](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Conexión a una instancia de MongoDB en un host remoto](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: crear, leer, actualizar, eliminar](https://docs.mongodb.com/manual/crud/)
- [Documentos de referencia](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Instalación de PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Compatibilidad de VS Code con PostgreSQL

VS Code admite el trabajo con bases de datos de PostgreSQL a través de la [extensión PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), puede crear, conectarse a, administrar y consultar las bases de datos de postgresql desde vs Code.

## <a name="set-up-profile-aliases"></a>Configurar alias de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]

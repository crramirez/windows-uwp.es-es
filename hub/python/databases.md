---
title: Introducción al uso de Python en Windows con una base de datos
description: Una guía que te ayudará a empezar a usar PostgreSQL o MongoDB con Python en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, python en windows, instalar postgresql en windows, instalar mongodb en windows, usar postgresql con python, usar mongodb con python, postgresql en WSL, mongodb en WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517778"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Introducción al uso de PostgreSQL o MongoDB con Python en Windows

Esta guía detallada te ayudará a comenzar a conectar tu aplicación de Python a una base de datos. Decidimos centrarnos en dos opciones conocidas: PostgreSQL y MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Diferencias entre MongoDB y PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> También te recomendamos que consideres en qué medida el marco y las herramientas que usas están integrados con un sistema de base de datos determinado. El [marco web de Django](./web-frameworks.md#hello-world-tutorial-for-django) parece estar mejor integrado con PostgreSQL (consulta la [documentación de Django](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) y [psycopg2](https://github.com/psycopg/psycopg2)). El [marco web de Flask](./web-frameworks.md#hello-world-tutorial-for-flask) parece estar mejor integrado con MongoDB (consulta [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) y [PyMongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Instalación de PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Compatibilidad de VS Code con PostgreSQL

VS Code permite trabajar con bases de datos de PostgreSQL mediante la [extensión de PostgreSQL ](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql). Puedes crear, administrar y consultar las bases de datos de PostgreSQL desde VS Code, así como conectarte a ellas.

## <a name="install-mongodb"></a>Instalación de MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Compatibilidad de VS Code con MongoDB

VS Code permite trabajar con bases de datos de MongoDB mediante la [extensión de Azure CosmosDB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb). Puedes crear, administrar y consultar las bases de datos de MongoDB desde VS Code, así como conectarte a ellas.

Para obtener más información, visita la documentación de VS Code: [Trabajar con MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Configuración de alias de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]

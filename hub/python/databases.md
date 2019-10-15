---
title: Introducción al uso de Python en Windows con una base de datos
description: Una guía para ayudarle a empezar a usar PostgreSQL o MongoDB con Python en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, PostgreSQL, MongoDB, Postgres, Mongo, Microsoft, Python en Windows, instalación de PostgreSQL en Windows, instalación de MongoDB en Windows, uso de PostgreSQL con Python, uso de MongoDB con Python, PostgreSQL en WSL, MongoDB en WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 42a257361cffec974d060a6518dfdf5254d62082
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314859"
---
# <a name="get-started-using-databases-with-python-on-windows"></a>Introducción al uso de bases de datos con Python en Windows

Las aplicaciones de Python suelen necesitar conservar los datos, lo que puede ocurrir a través de archivos, almacenamiento local, servicios en la nube o bases de datos. Esta guía paso a paso le ayudará a comenzar a conectar su aplicación de Python a una base de datos de. Decidimos centrarnos en dos opciones populares: PostgreSQL y MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Diferencias entre MongoDB y PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> También puede ser conveniente tener en cuenta cómo se integra el marco de trabajo y las herramientas que se usan con un sistema de base de datos determinado. Parece que el [marco Web de Django](./web-frameworks.md#hello-world-tutorial-for-django) está mejor integrado con PostgreSQL (consulte los [documentos de Django](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) y [psycopg2](https://github.com/psycopg/psycopg2)). El [marco Web de frasco](./web-frameworks.md#hello-world-tutorial-for-flask) parece estar mejor integrado con MongoDB (consulte [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) y [PyMongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Instalación de PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Compatibilidad de VS Code con PostgreSQL

VS Code admite el trabajo con bases de datos de PostgreSQL a través de la [extensión PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), puede crear, conectarse a, administrar y consultar las bases de datos de postgresql desde vs Code.

## <a name="install-mongodb"></a>Instalación de MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Compatibilidad de VS Code con MongoDB

VS Code admite el trabajo con bases de datos de MongoDB a través de la [extensión CosmosDB de Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), puede crear, conectar, administrar y consultar bases de datos de mongodb desde vs Code.

Para obtener más información, visite el VS Code docs: [Trabajar con MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Configurar alias de perfil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]

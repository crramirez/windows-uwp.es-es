---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Acceso a datos
description: En esta sección se explica cómo almacenar los datos del dispositivo en una base de datos privada mediante la asignación relacional de objetos en aplicaciones para la Plataforma universal de Windows (UWP).
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, data, database, relational, tables, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: 8ce2bbb1b72eaa33001e86c9aa81b334e338cb53
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362609"
---
# <a name="data-access"></a>Acceso a datos

Puedes almacenar datos en el dispositivo del usuario mediante una base de datos de SQLite. También puedes conectar la aplicación directamente a una base de datos de SQL Server sin tener que usar ninguna clase de nivel de servicio.

| Tema | Descripción|
|-------|------------|
| [Usar una base de datos de SQLite en una aplicación para UWP](sqlite-databases.md) | Se muestra cómo usar SQLite para almacenar y recuperar datos en una base de datos ligera en el dispositivo del usuario. SQLite es un motor de bases de datos incrustado sin servidor. |
| [Usar una base de datos de SQL Server en una aplicación para UWP](sql-server-databases.md) | Muestra cómo conectarse directamente a una base de datos de SQL Server y luego almacenar y recuperar datos mediante clases en el espacio de nombres [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN). Sin necesidad de capa de servicio. |

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database)

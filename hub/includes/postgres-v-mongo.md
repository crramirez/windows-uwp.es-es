---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314879"
---
Dos [opciones populares](https://insights.stackoverflow.com/survey/2019#technology-_-databases) para un sistema de base de datos son [MongoDB](https://www.mongodb.com/what-is-mongodb) y [PostgreSQL](https://www.postgresql.org/about/). 

MongoDB es una base de datos de documentos NoSQL diseñada para trabajar con JSON y almacenar datos sin esquemas. Resulta útil para la flexibilidad y los datos no estructurados, el almacenamiento en caché de análisis en tiempo real y el escalado horizontal. 

PostgreSQL (a veces conocido como Postgres) es una base de datos relacional de SQL que pone énfasis en la extensibilidad y el cumplimiento de los estándares. Ahora también puede controlar JSON, pero por lo general funciona mejor para los datos estructurados, el escalado vertical y las necesidades compatibles con ACID, como el comercio electrónico y las transacciones financieras.

Esquemas:

**PostgreSQL:** Tabla | Columna | Valor | Registros.

**MongoDB (NoSQL):** Colección | Clave | Valor | Document.

El tipo de base de datos que elijas debería depender del tipo de aplicación con la que vayas a usar la base de datos. Te recomendamos que busques las ventajas y desventajas de las bases de datos estructuradas y no estructuradas y elijas la que te vaya mejor según el caso de uso. También existen algunos otros sistemas de base de datos que debes considerar además de PostgreSQL y MongoDB.
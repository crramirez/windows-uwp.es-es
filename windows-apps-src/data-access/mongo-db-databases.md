---
title: Uso de una base de datos de MongoDB en una aplicación para UWP
description: Obtenga información sobre cómo conectar una aplicación para la Plataforma universal de Windows (UWP) directamente a una base de datos de MongoDB y probar la conexión mediante programación.
ms.date: 03/28/2019
ms.topic: article
keywords: Windows 10, UWP, MongoDB, base de datos
ms.localizationpriority: medium
ms.openlocfilehash: aaa035393e49d6bdad49faa806485cc51d21bb84
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043517"
---
# <a name="use-a-mongodb-database"></a>Usar una base de datos de MongoDB
Este artículo contiene los pasos necesarios para trabajar con una base de datos de MongoDB desde una aplicación para UWP. Además, incluye un pequeño fragmento de código en el que se muestra cómo se puede interactuar con la base de datos en el código.

## <a name="set-up-your-solution"></a>Configuración de la solución

Para conectar la aplicación directamente a una base de datos de MongoDB, asegúrate de que la versión mínima del proyecto está destinada a Fall Creators Update (compilación 16299).  Puedes encontrar esta información en la página de propiedades de tu proyecto de UWP.

![Imagen del panel de propiedades de destino en Visual Studio en el que se muestran las versiones mínima y de destino establecidas en Fall Creators Update](images/min-version-fall-creators.png)

Abre la **consola del Administrador de paquetes** (Ver -> Otras ventanas -> Consola del Administrador de paquetes). Usa el comando **Install-Package MongoDB.Driver** para instalar el controlador de MongoDB. Esto te permitirá acceder mediante programación a las bases de datos de MongoDB.

## <a name="test-your-connection-using-sample-code"></a>Prueba de la conexión con el código de ejemplo
En el ejemplo de código siguiente se obtiene una colección de un cliente remoto de MongoDB y se agrega un nuevo documento a dicha colección. Después, se usan API de MongoDB para recuperar el nuevo tamaño de la colección, así como el documento insertado, y se imprimen. Ten en cuenta que la dirección IP y los nombres de las bases de datos deben personalizarse.

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```

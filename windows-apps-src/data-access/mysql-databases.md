---
title: Uso de una base de datos de MySQL en una aplicación para UWP
description: Usa una base de datos de MySQL en una aplicación para UWP.
ms.date: 3/28/2019
ms.topic: article
keywords: Windows 10, UWP, MySQL, base de datos
ms.localizationpriority: medium
ms.openlocfilehash: a7708ca082647aef6bbf2261922d2ebd6723923e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785489"
---
# <a name="use-a-mysql-database"></a>Usar una base de datos de MySQL
Este artículo contiene los pasos necesarios para trabajar con una base de datos de MySQL desde una aplicación para UWP. Además, incluye un pequeño fragmento de código en el que se muestra cómo se puede interactuar con la base de datos en el código.

## <a name="set-up-your-solution"></a>Configuración de la solución

Para conectar la aplicación directamente a una base de datos de MySQL, asegúrate de que la versión mínima del proyecto está destinada a Fall Creators Update (compilación 16299).  Puedes encontrar esta información en la página de propiedades de tu proyecto de UWP.

![Imagen del panel de propiedades de destino en Visual Studio en el que se muestran las versiones mínima y de destino establecidas en Fall Creators Update](images/min-version-fall-creators.png)

Abre la **consola del Administrador de paquetes** (Ver -> Otras ventanas -> Consola del Administrador de paquetes). Usa el comando **Install-Package MySql.Data** para instalar el controlador de MySQL. Esto te permitirá acceder mediante programación a las bases de datos de MySQL.

## <a name="test-your-connection-using-sample-code"></a>Prueba de la conexión con el código de ejemplo
En el siguiente ejemplo se muestra cómo conectarse a una base de datos de MySQL remota y leer su contenido. Ten en cuenta que la dirección IP, las credenciales y el nombre de la base de datos debe personalizarse.

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```
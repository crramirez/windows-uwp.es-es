---
author: normesta
title: Usar una base de datos de SQLite en una aplicación para UWP
description: Usar una base de datos de SQLite en una aplicación para UWP.
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, SQLite, base de datos
ms.localizationpriority: medium
ms.openlocfilehash: 01cac3c1b8c18e968c35acb01b3d3918d9efe60d
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018601"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>Usar una base de datos de SQLite en una aplicación para UWP
Puedes usar SQLite para almacenar y recuperar datos en una base de datos ligera en el dispositivo del usuario. Esta guía te muestra cómo hacerlo.

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>Algunas ventajas de usar SQLite para el almacenamiento local

: heavy_check_mark: SQLite es ligero y autocontenido. Se trata de una biblioteca de código sin ninguna otra dependencia. No hay que configurar nada.

: heavy_check_mark: No hay ningún servidor de bases de datos. El cliente y el servidor se ejecutan en el mismo proceso.

: heavy_check_mark: SQLite es de dominio público, por lo que puedes utilizarlo y distribuirlo libremente con tu aplicación.

: heavy_check_mark: SQLite funciona en diversas plataformas y arquitecturas.

Puedes leer más sobre SQLite [aquí](https://sqlite.org/about.html).

## <a name="choose-an-abstraction-layer"></a>Elegir una capa de abstracción

Te recomendamos que uses el Entity Framework Core o la [biblioteca SQLite](https://github.com/aspnet/Microsoft.Data.Sqlite/) de código abierto creados por Microsoft.

### <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) es un asignador relacional de objetos que puedes usar para trabajar con datos relacionales mediante el uso de objetos específicos del dominio. Si ya has usado este marco para trabajar con datos de otras aplicaciones. NET, puedes migrar ese código a una aplicación para UWP y funcionará con los cambios adecuados en la cadena de conexión.

Para probarlo, consulta [Introducción a EF Core en la Plataforma universal de Windows (UWP) con una nueva base de datos](https://docs.microsoft.com/ef/core/get-started/uwp/getting-started).

### <a name="sqlite-library"></a>Biblioteca de SQLite

La biblioteca de [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) implementa las interfaces en el espacio de nombres [System.Data.Common](https://msdn.microsoft.com/library/system.data.common.aspx). Microsoft mantiene estas implementaciones de forma activa, y estas proporcionan un contenedor intuitivo para la API de SQLite nativa de bajo nivel.

El resto de esta guía te ayudará a usar esta biblioteca.

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>Configurar la solución para que utilice biblioteca Microsoft.Data.SQlite

Empezaremos con un proyecto para UWP básico, agregar una biblioteca de clases, y luego instalaremos los paquetes de NuGet adecuados.

El tipo de biblioteca de clases que agregues a tu solución y los paquetes específicos que instalarás dependen de la versión mínima del Windows SDK a la que esté destinada la aplicación. Puedes encontrar esta información en la página de propiedades de tu proyecto para la UWP.

![Versión mínima del Windows SDK](images/min-version.png)

Usa una de las siguientes secciones en función de la versión mínima del Windows SDK a la que esté destinado el proyecto para la UWP.

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>La versión mínima del proyecto no está destinada a Fall Creators Update

Si usas Visual Studio 2015, haz clic en **Ayuda**->**Acerca de Microsoft Visual Studio**. A continuación, en la lista de programas instalados, asegúrate de que tienes la versión **3.5** o superior del Administrador de paquetes de NuGet. Si es el número de versión es inferior, instala una versión posterior de NuGet [aquí](https://www.nuget.org/downloads). En esa página encontrarás todas las versiones de NuGet bajo el encabezado **Visual Studio 2015**.

A continuación, agrega la biblioteca de clases a tu solución. No tienes que usar una biblioteca de clases para contener el código de acceso a datos, pero vamos a utilizar una en nuestro ejemplo. Llamaremos a la biblioteca **DataAccessLibrary** y llamaremos a la clase de la biblioteca **DataAccess**.

![Biblioteca de clases](images/class-library.png)

Haz clic con el botón derecho en la solución y luego haz clic en **Administrar paquetes NuGet para la solución**.

![Administrar paquetes NuGet](images/manage-nuget.png)

Si usas Visual Studio 2015, elige la pestaña **Instalados** y asegúrate de que el número de versión del paquete **Microsoft.NETCore.UniversalWindowsPlatform** es **5.2.2** o superior.

![Versión de .NETCore](images/package-version.png)

Si no es así, actualiza el paquete a una versión más reciente.

Elige la pestaña **Examinar** y busca el paquete **Microsoft.Data.SQLite**. Instala la versión **1.1.1** (o inferior) del paquete.

![Paquete de SQLite](images/sqlite-package.png)

Ve a la sección [Agregar y recuperar datos de una base de datos de SQLite](#use-data) de esta guía.

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>La versión mínima del proyecto está destinada a Fall Creators Update

Hay un par de ventajas si elevas la versión mínima de tu proyecto para UWP a la Fall Creators Update.

En primer lugar, puedes usar las bibliotecas de .NET Standard 2.0 en lugar de bibliotecas de clases normales. Esto significa que puedes compartir el código de acceso a datos con cualquier otra aplicación basada .NET, como aplicaciones WPF, Windows Forms, Android, iOS o ASP.NET.

En segundo lugar, la aplicación no tiene bibliotecas de SQLite de paquetes. En su lugar, la aplicación puede usar la versión de SQLite que viene instalada con Windows. Esto te ayudará de varias maneras.

: heavy_check_mark: Reduce el tamaño de la aplicación porque no es necesario que descargues el archivo binario de SQLite y luego lo empaquetes como parte de la aplicación.

: heavy_check_mark: Evita que tengas que enviar una nueva versión de la aplicación a los usuarios en caso de que SQLite publique correcciones críticas para errores y vulnerabilidades de seguridad de SQLite. Microsoft mantiene la versión de Windows de SQLite en coordinación con SQLite.org.

: heavy_check_mark: El tiempo de carga de la aplicación tiene el potencial de ser más rápido porque, muy probablemente, la versión del SDK de SQLite ya estará cargada en la memoria.

Empecemos por agregar a la solución una biblioteca de clases de .NET Standard 2.0. No es necesario que uses una biblioteca de clases para contener el código de acceso a datos, pero vamos a utilizar una en nuestro ejemplo. Llamaremos a la biblioteca **DataAccessLibrary** y llamaremos a la clase de la biblioteca **DataAccess**.

![Biblioteca de clases](images/dot-net-standard.png)

Haz clic con el botón derecho en la solución y luego haz clic en **Administrar paquetes NuGet para la solución**.

![Administrar paquetes NuGet](images/manage-nuget-2.png)

En este punto, tiene que realizar una elección. Puedes usar la versión de SQLite que se incluye con Windows o, si tienes algún motivo para usar una versión específica de SQLite, puedes incluir la biblioteca de SQLite en el paquete.

Empecemos con cómo usar la versión de SQLite que se incluye con Windows.

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>Para usar la versión de SQLite que se instala con Windows

Elige la pestaña **Examinar**, busca el paquete **Microsoft.Data.SQLite.core** e instálalo.

![Paquete de SQLite Core](images/sqlite-core-package.png)

Busca el paquete **SQLitePCLRaw.bundle_winsqlite3** y, a continuación, instálalo únicamente en el proyecto para la UWP de la solución.

![Paquete de PCL sin procesar de SQLite](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>Para incluir SQLite con la aplicación

No tienes que hacer esto. Pero si tienes un motivo para incluir una versión específica de SQLite con tu aplicación, elige la pestaña **Examinar** y busca el paquete **Microsoft.Data.SQLite**. Instala la versión **2.0** (o inferior) del paquete.

![Paquete de SQLite](images/sqlite-package-v2.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>Agregar y recuperar datos en una base de datos de SQLite

Haremos lo siguiente:

:one: Preparar la clase de acceso a datos.

:two: Inicializar la base de datos de SQLite.

:three: Insertar datos en la base de datos de SQLite.

:four: Recuperar datos de la base de datos de SQLite.

:five: Agregar una interfaz de usuario básica.

### <a name="prepare-the-data-access-class"></a>Preparar la clase de acceso a datos

Desde el proyecto para la UWP, agrega una referencia al proyecto **DataAccessLibrary** de la solución.

![Biblioteca de clases de acceso a datos](images/ref-class-library.png)

Agrega la siguiente instrucción ``using`` a los archivos **App.xaml.cs** y **MainPage.xaml.cs** del proyecto de UWP.

```csharp
using DataAccessLibrary;
```

Abre la clase **DataAccess** de tu solución **DataAccessLibrary** y haz la clase estática.

>[!NOTE]
>Aunque nuestro ejemplo colocará el código de acceso a datos en una clase estática, es simplemente una opción de diseño totalmente opcional.

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

Agrega la siguiente instrucción using en la parte superior de este archivo.

```csharp
using Microsoft.Data.Sqlite;
```

<a id="initialize" />

### <a name="initialize-the-sqlite-database"></a>Inicializar la base de datos de SQLite.

Agrega un método a la clase **DataAccess** que inicialice la base de datos de SQLite.

```csharp
public static void InitializeDatabase()
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

Este código crea la base de datos de SQLite y la almacena en el almacén de datos local de la aplicación.

En este ejemplo, llamaremos a la base de datos ``sqlliteSample.db``, pero puedes usar el nombre que quieras siempre que emplees dicho nombre en todos los objetos [SqliteConnection](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0) de los que crees una instancia.

En el constructor del archivo **App.xaml.cs** de tu proyecto para la UWP, llama al método ``InitializeDatabase`` de la clase **DataAccess**.

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```

<a id="insert" />

### <a name="insert-data-into-the-sqlite-database"></a>Insertar datos en la base de datos de SQLite.

Agrega un método a la clase **DataAccess** que inserte datos en la base de datos de SQLite. Este código utiliza parámetros en la consulta para evitar ataques de inserción de SQL.

```csharp
public static void AddData(string inputText)
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```

<a id="retrieve" />

### <a name="retrieve-data-from-the-sqlite-database"></a>Recuperar datos de la base de datos de SQLite.

Agrega un método que obtenga filas de datos de una base de datos SQLite.

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

El método [Read](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_Read) avanza por las filas de datos devueltos. Devuelve **true** si hay filas a la izquierda, y si no devuelve **false**.

El método [GetString](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_) devuelve el valor de la columna especificado como una cadena. Acepta un valor entero que represente el ordinal de la columna con base cero de los datos que desees. Puedes usar métodos similares, como [GetDataTime](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) y [GetBoolean](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_). Elige un método basado en el tipo de datos que contenga la columna.

El parámetro ordinal no es tan importante en este ejemplo porque vamos a seleccionar todas las entradas de una sola columna. Sin embargo, si hay varias columnas que formen parte de la consulta, usa el valor ordinal para obtener la columna de la que quieras extraer datos.

## <a name="add-a-basic-user-interface"></a>Agregar una interfaz de usuario básica

En el archivo **MainPage.xaml** del proyecto para la UWP, agrega el siguiente código XAML.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

Esta interfaz de usuario básica proporciona al usuario un ``TextBox`` que puede usar para escribir una cadena que agregaremos a la base de datos de SQLite. Conectaremos el ``Button`` de esta interfaz de usuario a un controlador de eventos que recuperará datos de la base de datos de SQLite y luego los mostrará en la ``ListView``.

En el archivo **MainPage.xaml.cs**, agrega el siguiente controlador. Este es el método que hemos asociados con el evento ``Click`` de ``Button`` en la interfaz de usuario.

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

Eso es todo. Explora [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) para ver qué otras cosas que puedes hacer con tu base de datos de SQLite. Consulta los vínculos de debajo para obtener información sobre otras formas de usar datos en tu aplicación para UWP.

## <a name="next-steps"></a>Pasos siguientes

**Conectar tu aplicación directamente a una base de datos de SQL Server**

Consulta [Usar una base de datos de SQL Server en una aplicación para UWP](sql-server-databases.md).

**Compartir código entre diferentes aplicaciones de distintas plataformas**

Consulta [Compartir código entre una aplicación de escritorio y una aplicación para UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Agregar páginas de detalles maestras con back-ends SQL de Azure**

Consulta [Muestra de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database).

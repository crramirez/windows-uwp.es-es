---
title: Uso de una base de datos de SQL Server en una aplicación para UWP
description: Usa una base de datos SQL Server en una aplicación para UWP.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp, SQL Server, database
ms.localizationpriority: medium
ms.openlocfilehash: d5c5c0f107a9e9a2adb0587b544a2c74b8321886
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448345"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>Uso de una base de datos de SQL Server en una aplicación para UWP
Tu aplicación puede conectarse directamente a una base de datos de SQL Server y a continuación almacenar y recuperar datos mediante clases en el espacio de nombres [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient).

En esta guía te mostraremos un modo de hacerlo. Si instalas la base de datos de ejemplo [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) en la instancia de SQL Server y luego usas estos fragmentos de código, terminarás con una interfaz de usuario básica que muestre los productos de esta base de datos.

![Productos Northwind](images/products-northwind.png)

Los fragmentos de código que aparecen en esta guía se basan en este [ejemplo más completo](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo).

## <a name="first-set-up-your-solution"></a>En primer lugar, configura la solución

Para conectar la aplicación directamente a una base de datos SQL Server, asegúrate de que la versión mínima del proyecto está destinada a Fall Creators Update.  Puedes encontrar esa información en la página de propiedades del proyecto para UWP.

![Versión mínima de Windows SDK](images/min-version-fall-creators.png)

Abre el archivo **Package.appxmanifest** de tu proyecto para UWP en el diseñador de manifiestos.

En la pestaña **Funcionalidades**, activa la casilla **Enterprise Authentication** (Autenticación empresarial) si usas la autenticación de Windows para la autenticación de SQL Server.

![Funcionalidad de autenticación empresarial](images/enterprise-authentication.png)

> [!IMPORTANT]
> También debe seleccionar **Internet (cliente y servidor)** , **Internet (cliente)** y **Redes privadas (cliente y servidor)** , independientemente de si usa o no la autenticación de Windows.

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>Adición y recuperación de datos en una base de datos de SQL Server

En esta sección, haremos lo siguiente:

:one: Agregar una cadena de conexión

:two: Crear una clase que contenga los datos de productos

:three: Recuperar productos de la base de datos de SQL Server

:four: Agregar una interfaz de usuario básica.

:five: Llenar la interfaz de usuario con productos

>[!NOTE]
> En esta sección se muestra una manera de organizar el código de acceso a los datos. Está concebida únicamente para proporcionar un ejemplo de cómo puedes usar [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) para almacenar y recuperar datos de una base de datos de SQL Server. Puedes organizar el código de la manera que tenga más sentido para el diseño de la aplicación.

### <a name="add-a-connection-string"></a>Adición de una cadena de conexión

En el archivo **App.xaml.cs**, agrega una propiedad a la clase ``App``, que proporciona a otras clases de la solución acceso a la cadena de conexión.

Nuestra cadena de conexión apunta a la base de datos Northwind en una instancia de SQL Server Express.

```csharp
sealed partial class App : Application
{
    // Connection string for using Windows Authentication.
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    // This is an example connection string for using SQL Server Authentication.
    // private string connectionString =
    //     @"Data Source=YourServerName\YourInstanceName;Initial Catalog=DatabaseName; User Id=XXXXX; Password=XXXXX";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>Creación de una clase que contenga los datos de productos

Vamos a crear una clase que implemente el evento [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged) para que podamos enlazar atributos de la interfaz de usuario XAML a las propiedades de esta clase.

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>Recuperación de productos de la base de datos de SQL Server

Crea un método que obtenga productos de la base de datos de ejemplo Northwind y luego los devuelva como una colección [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) de instancias de ``Product``.

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>Adición de una interfaz de usuario básica

 Agrega el siguiente código XAML al archivo **MainPage.xaml** del proyecto para UWP.

 Este código XAML crea un elemento [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) para mostrar cada producto que devuelvas en el fragmento de código anterior y enlaza los atributos de cada fila de [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) a las propiedades que hemos definido en la clase ``Product``.

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>Presentación de productos en ListView

Abre el archivo **MainPage.xaml.cs** y agrega código al constructor de la clase ``MainPage`` que establece la propiedad **ItemSource** de [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) en [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) de las instancias de ``Product``.

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

Inicia el proyecto y verás que los productos de la base de datos de ejemplo Northwind aparecen en la interfaz de usuario.

![Productos Northwind](images/products-northwind.png)

Explora el espacio de nombres [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) para ver qué otras cosas puedes hacer con los datos en la base de datos de SQL Server.

## <a name="trouble-connecting-to-your-database"></a>¿Tienes problemas para conectarte a la base de datos?

En la mayoría de los casos, es necesario cambiar algún aspecto de la configuración de SQL Server. Si puedes conectarte a la base de datos desde otro tipo de aplicación de escritorio, como una aplicación WPF o Windows Forms, asegúrate de que has habilitado TCP/IP para SQL Server. Puedes hacerlo en la consola de **Administración de equipos**.

![Administrar equipos](images/computer-management.png)

A continuación, asegúrate de que se está ejecutando el servicio de SQL Server Browser.

![Servicio de SQL Server Browser](images/sql-browser-service.png)

## <a name="next-steps"></a>Pasos siguientes

**Usar una base de datos ligera para almacenar datos en el dispositivo del usuario**

Consulta [Usar una base de datos de SQLite en una aplicación para UWP](sqlite-databases.md).

**Compartir código entre diferentes aplicaciones de distintas plataformas**

Consulta [Compartir código entre una aplicación de escritorio y una aplicación para UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Agregar páginas de detalles maestras con servidores back-end de Azure SQL**

Consulta [Ejemplo de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database).

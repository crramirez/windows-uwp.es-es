---
Description: Compartir código entre una aplicación de escritorio y una aplicación para UWP
Search.Product: eADQiWindows 10XVcnh
title: Compartir código entre una aplicación de escritorio y una aplicación para UWP
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 151584f15013c9d4ab7d9566e175b957a7a84149
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644350"
---
# <a name="share-code-between-a-desktop-application-and-a-uwp-app"></a>Compartir código entre una aplicación de escritorio y una aplicación para UWP

Puedes mover el código a las bibliotecas de .NET Standard y, a continuación, crear una app de la Plataforma universal de Windows (UWP) para tener acceso a todos los dispositivos de Windows 10. Aunque no hay ninguna herramienta que pueda convertir una aplicación de escritorio en una aplicación para UWP, puedes reutilizar gran parte de tu código existente, lo que disminuirá el costo de la creación de uno. Esta guía te muestra cómo hacerlo.

## <a name="share-code-in-a-net-standard-20-library"></a>Compartir código en una biblioteca de .NET Standard 2.0

Colocar todo el código que puedas en las bibliotecas de clases de .NET Standard 2.0.  Siempre que tu código use API que se definen en la configuración estándar, puedes reutilizarlo en una aplicación para UWP. Es más fácil que nunca compartir código en una biblioteca .NET Standard, porque hay muchas más API incluidas en .NET Standard 2.0.

Este es un vídeo excelente en el que se te presenta más información sobre el tema.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

### <a name="add-net-standard-libraries"></a>Agregar bibliotecas de .NET Standard

Primero, agrega una o más bibliotecas de clases de .NET Standard a la solución.  

![Agregar el proyecto estándar de dotnet](images/desktop-to-uwp/dot-net-standard-project-template.png)

El número de bibliotecas que agregues a la solución depende de cómo tengas pensado organizar el código.

Asegúrate de que cada biblioteca de clase está orientada a **.NET Standard 2.0**.

![.NET Standard 2.0 de destino](images/desktop-to-uwp/target-standard-20.png)

Encontrarás este ajuste en las páginas de propiedades del proyecto de biblioteca de clase.

Desde el proyecto de aplicación de escritorio, agrega una referencia al proyecto de biblioteca de clase.

![Referencia de la biblioteca de clases](images/desktop-to-uwp/class-library-reference.png)

A continuación, usa herramientas para determinar cuánto código cumple el estándar. De esa manera, antes de pasar código a la biblioteca, puedes decidir qué partes se pueden reutilizar, qué partes requieren una modificación mínima y qué partes seguirán siendo específicas de la aplicación.

### <a name="check-library-and-code-compatibility"></a>Comprobar la compatibilidad de la biblioteca y el código

Comenzaremos con paquetes Nuget y otros archivos .dll que obtuviste de un tercero.

Si tu aplicación usa cualquiera de ellas, determina si son compatibles con .NET Standard 2.0. Puedes usar una extensión de Visual Studio o una utilidad de línea de comandos para ello.

Usa estas mismas herramientas para analizar el código. Descarga las herramientas aquí ([dotnet apiport](https://github.com/Microsoft/dotnet-apiport/releases)) y, a continuación, ve este vídeo para aprender a usarlas.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

Si el código no es compatible con el estándar, considera otras maneras en las que podrías implementar ese código. Empieza por abrir el [Explorador de API de .NET](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0). Puedes usar ese navegador para revisar las API que están disponible en .NET Standard 2.0. Asegúrate de definir el ámbito de la lista a .NET Standard 2.0.

![Opción de .NET](images/desktop-to-uwp/dot-net-option.png)

Parte del código será específico de la plataforma y deberá permanecer en el proyecto de la aplicación de escritorio.

### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Por ejemplo: Migrar código de acceso a datos en una biblioteca de .NET Standard 2.0

Supongamos que tenemos una aplicación de Windows Forms muy básica que se muestra a los clientes de nuestra base de datos de ejemplo Northwind.

![Aplicación de Windows Forms](images/desktop-to-uwp/win-forms-app.png)

El proyecto contiene una biblioteca de clases de .NET Standard 2.0 con una clase estática denominada **Northwind**. Si se mueve este código a la clase **Northwind**, no se compilará porque usa las clases ``SQLConnection``, ``SqlCommand`` y ``SqlDataReader`` y esas clases no están disponibles en .NET Standard 2.0.

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
Podemos usar el [Explorador de API de .NET](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0) para buscar una alternativa. Las clases ``DbConnection``, ``DbCommand`` y ``DbDataReader`` están disponibles en .NET Standard 2.0 para poder usarlas en su lugar.  

Esta versión revisada usa esas clases para obtener una lista de clientes, pero para crear una clase ``DbConnection``, tendremos que pasar un objeto de fábrica que creemos en la aplicación cliente.

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

En la página de código subyacente de Windows Forms, solo podemos crear la instancia de fábrica y pasarla a nuestro método.

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

## <a name="reach-all-windows-devices"></a>Llegar a todos los dispositivos Windows

Ya estás listo para agregar una aplicación para UWP a la solución.

![imagen del puente de dispositivo de escritorio a UWP](images/desktop-to-uwp/adaptive-ui.png)

Aún tendrás que diseñar páginas de interfaz de usuario en XAML y crear código específico de plataforma o dispositivo, pero cuando hayas terminado, podrás llegar a una completa variedad de dispositivos Windows 10 y las páginas de la aplicación tendrán un aspecto moderno que se adapta bien a diferentes resoluciones y tamaños de pantalla.

La aplicación responderá a mecanismos de entrada distintos de un teclado y un mouse, y las características y las opciones de configuración serán intuitivas en todos los dispositivos. Esto significa que los usuarios aprenden a hacer cosas una vez y después funciona de manera muy familiar con independencia del dispositivo.

Estas son solo algunas de las ventajas de UWP. Para obtener más información, consulta [Crear fantásticas experiencias con Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

### <a name="add-a-uwp-project"></a>Agregar un proyecto de UWP

En primer lugar, agrega un proyecto de UWP a la solución.

![Proyecto para UWP](images/desktop-to-uwp/new-uwp-app.png)

A continuación, desde el proyecto de UWP, agrega una referencia al proyecto de biblioteca de .NET Standard 2.0.

![Referencia de la biblioteca de clases](images/desktop-to-uwp/class-library-reference2.png)

### <a name="build-your-pages"></a>Crear tus páginas

Agrega páginas XAML y llama al código en tu biblioteca de .NET Standard 2.0.

![Aplicación para UWP](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```


Para comenzar con UWP, consulta [¿Qué es una aplicación para UWP?](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

## <a name="reach-ios-and-android-devices"></a>Llegar a dispositivos iOS y Android

Para llegar a dispositivos Android e iOS, agrega proyectos Xamarin.  

![Aplicaciones Xamarin](images/desktop-to-uwp/xamarin-apps.png)

Estos proyectos te permiten usar C# para crear aplicaciones de Android e iOS con acceso completo a API específicas de plataforma y dispositivo. Estas aplicaciones sacan provecho de la aceleración de hardware específico de plataforma y se compilan para rendimiento nativo.

Tienen acceso a toda la gama de funcionalidad expuesta por la plataforma subyacente y el dispositivo, incluidas las capacidades específicas de plataforma como iBeacons y fragmentos de Android y usará controles de interfaz de usuario nativo estándar para crear interfaces de usuario que tengan el aspecto que los usuarios esperan.

Al igual que las UWP, el costo para agregar una aplicación de Android o iOS es menor, porque puedes volver a usar la lógica empresarial en una biblioteca de clases de .NET Standard 2.0. Tendrás que diseñar tus páginas de interfaz de usuario en XAML y escribir cualquier código específico de plataforma o dispositivo.

### <a name="add-a-xamarin-project"></a>Agregar un proyecto Xamarin

En primer lugar, agrega un proyecto **Android**, **iOS** o **Multiplataforma** a tu solución.

Encontrarás estas plantillas en el cuadro de diálogo **Agregar nuevo proyecto** del grupo **Visual C#**.

![Aplicaciones Xamarin](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Los proyectos multiplataforma son excelentes para aplicaciones con poca funcionalidad específica de plataforma. Puedes usarlos para crear una interfaz de usuario basada en XAML nativo que se ejecute en iOS, Android y Windows. Obtenga más información [aquí](https://www.xamarin.com/forms).

A continuación, desde tu proyecto Android, iOS o multiplataforma, agrega una referencia al proyecto de biblioteca de clases.

![Referencia de la biblioteca de clases](images/desktop-to-uwp/class-library-reference3.png)

### <a name="build-your-pages"></a>Crear tus páginas

Nuestro ejemplo muestra una lista de clientes en una app de Android.

![Aplicación de Android](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Para comenzar con proyectos Android, iOS y multiplataforma, consulta el [portal para desarrolladores de Xamarin](https://developer.xamarin.com/).

## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

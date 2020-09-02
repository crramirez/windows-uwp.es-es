---
title: Compartir código entre una aplicación de escritorio y una aplicación para UWP
description: Obtenga información sobre cómo migrar una aplicación de escritorio desde .NET Framework (con WPF y Windows Forms) o las API Win32 de C++ a Plataforma universal de Windows (UWP) y Windows 10.
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ed23f77936378f2348abf868a67041be84978123
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304687"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>Traslado de una aplicación de escritorio a UWP

Si tiene una aplicación de escritorio existente que se compiló mediante el .NET Framework (incluidas WPF y Windows Forms) o las API Win32 de C++, tiene varias opciones para pasar a la Plataforma universal de Windows (UWP) y Windows 10.

## <a name="package-your-desktop-application-in-an-msix-package"></a>Empaquetar la aplicación de escritorio en un paquete de MSIX

Puede empaquetar la aplicación de escritorio en un paquete de MSIX para obtener acceso a muchas más características de Windows 10. MSIX es un formato moderno de paquete de la aplicación de Windows que proporciona una experiencia de empaquetado universal para todas las aplicaciones de Windows, incluidas las aplicaciones de UWP, WPF, Windows Forms y Win32. El empaquetado de las aplicaciones de escritorio de Windows en paquetes MSIX le permite acceder a una sólida experiencia de instalación y actualización, a un modelo de seguridad administrado con un sistema de funcionalidades flexible, a compatibilidad con Microsoft Store, a la administración empresarial y a muchos modelos de distribución personalizados. Puede empaquetar la aplicación si tiene el código fuente o si solo tiene un archivo de instalador existente (por ejemplo, un instalador MSI o App-V). Después de empaquetar la aplicación, puede integrar características de UWP, como extensiones de paquete y otros componentes de UWP.

Para obtener más información, vea [aplicaciones de escritorio de paquetes (puente de escritorio)](/windows/msix/desktop/desktop-to-uwp-root) y [características que requieren la identidad del paquete](/windows/apps/desktop/modernize/modernize-packaged-apps).

## <a name="use-windows-runtime-apis"></a>Usar API de Windows Runtime

Puedes llamar a muchas API de Windows Runtime directamente en la aplicación de escritorio de WPF, Windows Forms o Win32 de C++ para integrar las experiencias más actuales creadas para los usuarios de Windows 10. Por ejemplo, puedes llamar a las API de Windows Runtime para agregar notificaciones del sistema a la aplicación de escritorio.

Para obtener más información, consulta [Uso de API de Windows Runtime para aplicaciones de escritorio](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>Migración de una aplicación .NET Framework a una aplicación de UWP

Si la aplicación se ejecuta en el .NET Framework, puede migrarla a una aplicación de UWP aprovechando .NET Standard 2,0. Mueva todo el código que pueda tener en .NET Standard bibliotecas de clases de 2,0 y, a continuación, cree una aplicación para UWP que haga referencia a las bibliotecas de .NET Standard 2,0. 

### <a name="share-code-in-a-net-standard-20-library"></a>Compartir código en una biblioteca .NET Standard 2,0

Si la aplicación se ejecuta en el .NET Framework, coloque todo el código que pueda tener en .NET Standard bibliotecas de clases de 2,0. Siempre que el código use las API que se definen en el estándar, puede volver a usarlas en una aplicación para UWP. Es más fácil que nunca compartir código en una biblioteca de .NET Standard porque se incluyen muchas más API en el .NET Standard 2,0.

Este es un vídeo que le indica más información.

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>Agregar bibliotecas de .NET Standard

En primer lugar, agregue una o más bibliotecas de clases de .NET Standard a la solución.  

![Agregar proyecto dotnet Standard](images/desktop-to-uwp/dot-net-standard-project-template.png)

El número de bibliotecas que agregue a la solución depende de cómo planee organizar el código.

Asegúrese de que cada biblioteca de clases tiene como destino el **.NET Standard 2,0**.

![Destino .NET Standard 2,0](images/desktop-to-uwp/target-standard-20.png)

Puede encontrar este valor en las páginas de propiedades del proyecto de biblioteca de clases.

En el proyecto de aplicación de escritorio, agregue una referencia al proyecto de biblioteca de clases.

![Referencia de la biblioteca de clases](images/desktop-to-uwp/class-library-reference.png)

A continuación, use las herramientas de para determinar qué parte del código se ajusta al estándar. De este modo, antes de trasladar el código a la biblioteca, puede decidir qué partes puede reutilizar, qué partes requieren una modificación mínima y qué partes permanecerán específicas de la aplicación.

#### <a name="check-library-and-code-compatibility"></a>Comprobar la compatibilidad de la biblioteca y el código

Comenzaremos con paquetes de Nuget y otros archivos DLL que obtuvo de un tercero.

Si su aplicación utiliza cualquiera de ellos, determine si son compatibles con el .NET Standard 2,0. Para ello, puede usar una extensión de Visual Studio o una utilidad de línea de comandos.

Use estas mismas herramientas para analizar el código. Descargue las herramientas aquí ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases)) y, a continuación, vea este vídeo para aprender a usarlas.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

Si el código no es compatible con el estándar, tenga en cuenta otras formas de implementar ese código. Para empezar, abra el [Explorador de API de .net](/dotnet/api/?view=netstandard-2.0). Puede usar ese explorador para revisar las API que están disponibles en el .NET Standard 2,0. Asegúrese de establecer el ámbito de la lista en el .NET Standard 2,0.

![opción dot net](images/desktop-to-uwp/dot-net-option.png)

Algunos de sus códigos serán específicos de la plataforma y deberán permanecer en el proyecto de aplicación de escritorio.

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Ejemplo: migrar código de acceso de datos a una biblioteca .NET Standard 2,0

Supongamos que tenemos una aplicación Windows Forms muy básica que muestra los clientes de nuestra base de datos de ejemplo Northwind.

![Windows Forms aplicación](images/desktop-to-uwp/win-forms-app.png)

El proyecto contiene una biblioteca de clases .NET Standard 2,0 con una clase estática denominada **Northwind**. Si pasamos este código a la clase **Northwind** , no se compilará porque utiliza las ``SQLConnection`` ``SqlCommand`` clases, y ``SqlDataReader`` , y las clases que no están disponibles en el .net Standard 2,0.

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
No obstante, podemos usar el [Explorador de API de .net](/dotnet/api/?view=netstandard-2.0) para buscar una alternativa. Las ``DbConnection`` ``DbCommand`` clases, y ``DbDataReader`` están disponibles en la .net Standard 2,0 para que se puedan usar en su lugar.  

Esta versión revisada usa esas clases para obtener una lista de clientes, pero para crear una ``DbConnection`` clase, es necesario pasar un objeto de generador que se crea en la aplicación cliente.

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

En la página de código subyacente del formulario de Windows, se puede crear una instancia de generador y pasarla a nuestro método.

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

### <a name="create-a-uwp-app"></a>Creación de una aplicación UWP

Ahora está listo para agregar una aplicación para UWP a la solución.

![imagen de puente de escritorio a UWP](images/desktop-to-uwp/adaptive-ui.png)

Todavía tendrá que diseñar páginas de la interfaz de usuario en XAML y escribir cualquier código específico de la plataforma o del dispositivo, pero cuando haya terminado, podrá alcanzar toda la amplitud de dispositivos de Windows 10 y las páginas de la aplicación tendrán un aspecto moderno que se adapta bien a diferentes tamaños de pantalla y resoluciones.

La aplicación responderá a mecanismos de entrada que no son simplemente un teclado y un mouse, y las características y la configuración serán intuitivas en todos los dispositivos. Esto significa que los usuarios aprenden cómo realizar las tareas una vez y, después, funcionan de forma muy familiar con independencia del dispositivo.

Estas son solo algunas de las buenas ventajas que incluye UWP. Para obtener más información, consulte [crear fantásticas experiencias con Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

#### <a name="add-a-uwp-project"></a>Agregar un proyecto de UWP

En primer lugar, agregue un proyecto de UWP a la solución.

![Proyecto de UWP](images/desktop-to-uwp/new-uwp-app.png)

A continuación, en el proyecto de UWP, agregue una referencia al proyecto de biblioteca .NET Standard 2,0.

![Referencia de la biblioteca de clases](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>Compilar las páginas

Agregue páginas XAML y llame al código de la biblioteca .NET Standard 2,0.

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

Para empezar a trabajar con UWP, vea [Qué es una aplicación de UWP](../get-started/universal-application-platform-guide.md).

### <a name="reach-ios-and-android-devices"></a>Llegar a dispositivos iOS y Android

Puede acceder a dispositivos Android e iOS agregando proyectos de Xamarin.  

![Aplicaciones de Xamarin](images/desktop-to-uwp/xamarin-apps.png)

Estos proyectos permiten usar C# para compilar aplicaciones de iOS y Android con acceso completo a las API específicas de la plataforma y del dispositivo. Estas aplicaciones aprovechan la aceleración de hardware específica de la plataforma y se compilan para obtener un rendimiento nativo.

Tienen acceso a todo el espectro de funciones expuestas por la plataforma y el dispositivo subyacentes, incluidas capacidades específicas de la plataforma como iBeacons y fragmentos de Android, y se usarán controles de interfaz de usuario nativos estándar para crear interfaces de usuario que tengan la apariencia y el modo en que los usuarios las esperan.

Al igual que UWPs, el costo para agregar una aplicación Android o iOS es más bajo porque puede reutilizar la lógica de negocios en una biblioteca de clases .NET Standard 2,0. Tendrá que diseñar las páginas de la interfaz de usuario en XAML y escribir cualquier código específico de la plataforma o del dispositivo.

#### <a name="add-a-xamarin-project"></a>Agregar un proyecto de Xamarin

En primer lugar, agregue un proyecto de **Android**, **iOS**o **multiplataforma** a la solución.

Puede encontrar estas plantillas en el cuadro de diálogo **Agregar nuevo proyecto** en el grupo de **Visual C#** .

![Aplicaciones de Xamarin](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Los proyectos multiplataforma son ideales para aplicaciones con poca funcionalidad específica de la plataforma. Puede usarlos para compilar una interfaz de usuario nativa basada en XAML que se ejecute en iOS, Android y Windows. Obtenga más información [aquí](/xamarin/xamarin-forms/).

A continuación, desde el proyecto de Android, iOS o multiplataforma, agregue una referencia al proyecto de biblioteca de clases.

![Referencia de la biblioteca de clases](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>Compilar las páginas

En nuestro ejemplo se muestra una lista de clientes en una aplicación Android.

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

Para empezar a trabajar con los proyectos de Android, iOS y multiplataforma, vea el [portal para desarrolladores de Xamarin](/xamarin).

## <a name="next-steps"></a>Pasos siguientes

**Encontrar respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias de características**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
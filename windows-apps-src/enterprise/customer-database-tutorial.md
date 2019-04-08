---
title: Crear una aplicación de base de datos de clientes
description: Crear una aplicación de base de datos de clientes y obtenga información sobre cómo implementar funciones de aplicaciones empresariales básicos.
keywords: cliente de empresa, tutorial, datos, crear autenticación de eliminación, REST, leer, actualizar
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 9c09e0fb73e42fd8a3d0c70bbb5396be32624387
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623250"
---
# <a name="tutorial-create-a-customer-database-application"></a>Tutorial: Crear una aplicación de base de datos de clientes

Este tutorial crea una aplicación sencilla para administrar una lista de clientes. De esta forma, presenta una selección de los conceptos básicos para aplicaciones empresariales en UWP. Aquí aprenderás a hacer lo siguiente:

* Implementar la creación, lectura, actualización y eliminar operaciones en una base de datos SQL local.
* Agregar una cuadrícula de datos, para mostrar y editar datos del cliente en la interfaz de usuario.
* Organiza los elementos de interfaz de usuario juntos en un diseño de forma más básica.

El punto de partida para este tutorial es una aplicación de página única con un mínimo de interfaz de usuario y funcionalidad, basada en una versión simplificada de la [aplicación de ejemplo de la base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Se ha escrito C# y XAML y se espera que tiene un conocimiento básico ambos de esos lenguajes.

![La página principal de la aplicación en funcionamiento](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Requisitos previos

* [Asegúrese de que tener la versión más reciente de Visual Studio y el SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Clonar o descargar el ejemplo de Tutorial de base de datos de cliente](https://aka.ms/customer-database-tutorial)

Después de clonar o descargar el repositorio, puede editar el proyecto, abra **CustomerDatabaseTutorial.sln** con Visual Studio.

> [!NOTE]
> Consulte la [ejemplo completo de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver la aplicación en este tutorial se basa en.

## <a name="part-1-code-of-interest"></a>1ª parte: Código de interés

Si ejecuta la aplicación inmediatamente después de abrirla, verá algunos botones en la parte superior de una pantalla en blanco. Aunque no es visible para usted, la aplicación ya incluye una base de datos SQLite local aprovisionado con algunos clientes de prueba. Desde aquí, deberá iniciar mediante la implementación de un control de interfaz de usuario para mostrar a los clientes y, a continuación, continuar y agregar en las operaciones en la base de datos. Antes de comenzar, aquí es donde podrá seguir trabajando.

### <a name="views"></a>Vistas

**CustomerListPage.xaml** es la vista de la aplicación, que define la interfaz de usuario de la única página en este tutorial. Cada vez que necesite agregar o cambiar un elemento visual en la interfaz de usuario, deberá hacerlo en este archivo. Este tutorial le guiará a través de agregar estos elementos:

* Un **RadDataGrid** para mostrar y editar sus clientes. 
* Un **StackPanel** para establecer los valores iniciales de un cliente nuevo.

### <a name="viewmodels"></a>Modelos de vista

**ViewModels\CustomerListPageViewModel.cs** es donde se encuentra la lógica fundamental de la aplicación. Cada acción del usuario realizada en la vista se pasarán a este archivo para su procesamiento. En este tutorial, deberá agregar determinado código nuevo e implementar los métodos siguientes:

* **CreateNewCustomerAsync**, que inicializa un nuevo objeto CustomerViewModel.
* **DeleteNewCustomerAsync**, lo que elimina un cliente nuevo antes de que se muestre en la interfaz de usuario.
* **DeleteAndUpdateAsync**, que controla la lógica del botón de eliminación.
* **GetCustomerListAsync**, que recupera una lista de clientes de la base de datos.
* **SaveInitialChangesAsync**, que agrega información de un cliente nuevo a la base de datos.
* **UpdateCustomersAsync**, que actualiza la interfaz de usuario para reflejar los clientes agregan o eliminan.

**CustomerViewModel** es un contenedor de información de un cliente que realiza el seguimiento de si se ha modificado recientemente. No tendrá que agregar nada a esta clase, pero la parte del código que se va a agregar en otro lugar harán referencia a él.

Para obtener más información acerca de cómo está construido en el ejemplo, consulte el [información general de la estructura de aplicación](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>2ª parte: Agregar el control DataGrid

Antes de empezar a trabajar en los datos de cliente, deberá agregar un control de interfaz de usuario para mostrar a los clientes. Para ello, vamos a usar un tercero prefabricado **RadDataGrid** control. El **Telerik.UI.for.UniversalWindowsPlatform** paquete NuGet ya se ha incluido en este proyecto. Vamos a agregar la cuadrícula a nuestro proyecto.

1. Abra **Views\CustomerListPage.xaml** desde el Explorador de soluciones. Agregue la siguiente línea de código dentro de la **página** etiqueta para declarar una asignación para el espacio de nombres de Telerik que contiene la cuadrícula de datos.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. A continuación el **CommandBar** dentro de los principales **RelativePanel** de la vista, agregue un **RadDataGrid** control, con algunas opciones de configuración básica:

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. Se ha agregado a la cuadrícula de datos, pero necesita datos para mostrar. Agregue las siguientes líneas de código en él:

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Ahora que ha definido un origen de datos para mostrar, **RadDataGrid** controlará la mayor parte de la lógica de la interfaz de usuario para usted. Sin embargo, si ejecuta el proyecto, todavía no ve ningún dato en pantalla. Eso es porque el modelo de vista no está cargando aún.

![Aplicación en blanco, con ningún cliente](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Parte 3: Clientes de lectura

Cuando se inicializa, **ViewModels\CustomerListPageViewModel.cs** llamadas la **GetCustomerListAsync** método. Que las necesidades de método para recuperar la datos del cliente de prueba de la SQLite base de datos se incluye en el tutorial.

1. En **ViewModels\CustomerListPageViewModel.cs**, actualice su **GetCustomerListAsync** método con este código:

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    El **GetCustomerListAsync** método se llama cuando se carga el modelo de vista, pero antes de este paso, no hace nada. En este caso, hemos agregado una llamada a la **GetAsync** método **repositorio/SqlCustomerRepository**. Esto le permite ponerse en contacto con el repositorio para recuperar una colección enumerable de objetos Customer. , A continuación, analiza en objetos individuales, antes de agregarlos a su internal **ObservableCollection** para que se puedan mostrar y editar.

2. Ejecutar la aplicación: ahora verá la cuadrícula de datos donde se muestra la lista de clientes.

![Lista inicial de los clientes](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Parte 4: Editar clientes

Puede editar las entradas de la cuadrícula de datos haciendo doble clic en ellos, pero debe asegurarse de que los cambios que realice en la interfaz de usuario también se realizan en la colección de clientes en el código subyacente. Esto significa que tendrá que implementar el enlace de datos bidireccional. Si desea más información al respecto, consulte nuestra [Introducción al enlace de datos](../get-started/display-customers-in-list-learning-track.md).

1. Primero, declare que **ViewModels\CustomerListPageViewModel.cs** implementa el **INotifyPropertyChanged** interfaz:

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. A continuación, en el cuerpo principal de la clase, agregue el evento y el método siguiente:

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    El **OnPropertyChanged** método facilita a los establecedores generar el **PropertyChanged** eventos, que es necesario para el enlace de datos bidireccional.

3. Actualizar el establecedor para **SelectedCustomer** con esta llamada de función:

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. En **Views\CustomerListPage.xaml**, agregue el **SelectedCustomer** propiedad a la cuadrícula de datos.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Esto asocia la selección del usuario en la cuadrícula de datos con el objeto de cliente correspondiente en el código subyacente. El modo de enlace TwoWay permite que los cambios realizados en la interfaz de usuario en reflejarse en ese objeto.

5. Ejecute la aplicación. Ahora puede ver a los clientes que se muestran en la cuadrícula y realizar cambios en los datos subyacentes a través de la interfaz de usuario.

![Edición de un cliente en la cuadrícula de datos](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Parte 5: Actualizar clientes

Ahora que puede ver y editar a sus clientes, deberá poder insertar los cambios en la base de datos y para extraer las actualizaciones que se han realizado otros usuarios.

1. Vuelva a **ViewModels\CustomerListPageViewModel.cs**y navegue hasta la **UpdateCustomersAsync** método. Actualícelo con este código, para insertar los cambios en la base de datos y recuperar cualquier información nueva:

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    Este código utiliza el **IsModified** propiedad de **ViewModels\CustomerViewModel.cs**, que se actualiza automáticamente cada vez que se cambia el cliente. Esto le permite evitar llamadas innecesarias y solo insertar cambios de los clientes actualizados en la base de datos.

## <a name="part-6-create-a-new-customer"></a>Parte 6: Crear un nuevo cliente

Agregar a un nuevo cliente presenta un desafío, como el cliente aparecerá como una fila en blanco si se agrega a la interfaz de usuario antes de proporcionar valores para sus propiedades. Esto no es un problema, pero aquí le lo hacemos más fácil establecer valores iniciales de un cliente. En este tutorial, vamos a agregar un panel que puede contraerse simple, pero si tuviera más información para agregar podría crear una página independiente para este propósito.

### <a name="update-the-code-behind"></a>Actualizar el código subyacente

1. Agregar un nuevo campo privado y una propiedad pública para **ViewModels\CustomerListPageViewModel.cs**. Esto se usará para controlar o no está visible el panel.

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. Agregar una nueva propiedad pública para el modelo de vista, un inverso del valor de **AddingNewCustomer**. Esto se usará para deshabilitar los botones de barra de comandos normal cuando está visible el panel.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Ahora, necesitará una forma de mostrar el panel que se pueden contraer y para crear un cliente para editar dentro de él. 

3. Agregue un nuevo estudioso privada y una propiedad pública al ViewModel, para contener al cliente recién creado.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if {_newCustomer != value}
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Actualización de su **CreateNewCustomerAsync** método para crear un nuevo cliente, agregarlo en el repositorio y establézcalo como el cliente seleccionado:

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Actualizar el **SaveInitialChangesAsync** método para agregar un cliente recién creado en el repositorio, actualizar la interfaz de usuario y cierre el panel.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Agregue la siguiente línea de código como la última línea en el establecedor para **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Esto garantizará que **EnableCommandBar** se actualiza automáticamente cada vez que **AddingNewCustomer** se cambia.

### <a name="update-the-ui"></a>Actualización de la interfaz de usuario

1. Vuelva a **Views\CustomerListPage.xaml**y agregue un **StackPanel** con las siguientes propiedades entre su **CommandBar** y la cuadrícula de datos:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    El **x: Load** atributo garantiza que este panel solo aparece cuando va a agregar un nuevo cliente.

2. Realice el siguiente cambio en la posición de la cuadrícula de datos, para asegurarse de que se desplaza hacia abajo cuando aparece el nuevo panel:

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Actualizar el panel de apilamiento con cuatro **TextBox** controles. Podrá enlazar a las propiedades individuales del nuevo cliente y permitirle modificar sus valores antes de agregarlo a la cuadrícula de datos.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. Agregue un botón simple a su panel de pila nuevo para guardar al cliente recién creado:

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Actualizar el **CommandBar**, por lo que el regular create, delete y update botones están deshabilitados cuando está visible el panel de pila:

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Ejecute la aplicación. Ahora puede crear a un cliente y sus datos en el panel de apilamiento de entrada.

![Crear un nuevo cliente](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Parte 7: Eliminar un cliente

Eliminación de un cliente es la operación final básica que necesita para implementar. Cuando se elimina un cliente que ha seleccionado en la cuadrícula de datos, conviene llama inmediatamente a **UpdateCustomersAsync** con el fin de actualizar la interfaz de usuario. Sin embargo, no es necesario llamar a ese método, si elimina a un cliente que acaba de crear.

1. Vaya a **ViewModels\CustomerListPageViewModel.cs**y actualizar el **DeleteAndUpdateAsync** método:

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. En **Views\CustomerListPage.xaml**, actualizar el panel de pila para agregar un nuevo cliente de manera que contenga un segundo botón:

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. En **ViewModels\CustomerListPageViewModel.cs**, actualice el **DeleteNewCustomerAsync** método para eliminar el nuevo cliente:

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. Ejecute la aplicación. Ahora puede eliminar a los clientes, dentro de la cuadrícula de datos o en el panel de apilamiento.

![Eliminar a un cliente nuevo](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Conclusión

Enhorabuena. Con todo esto hace, la aplicación tiene ahora una gama completa de las operaciones de base de datos local. Puede crear, leer, actualizar y eliminar a clientes dentro de la interfaz de usuario, y estos cambios se guardan en la base de datos y se conservarán entre distintas lanzamientos de la aplicación.

Ahora que haya terminado, tenga en cuenta lo siguiente:
* Si no lo ha hecho ya, consulte el [información general de la estructura de aplicación](../enterprise/customer-database-app-structure.md) para obtener más información sobre por qué se compila la aplicación, cómo lo está.
* Explore la [ejemplo completo de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver la aplicación en este tutorial se basa en.

O bien, si está en un desafío, puede continuar y versiones posteriores...

## <a name="going-further-connect-to-a-remote-database"></a>Si continúa: Conectarse a una base de datos remoto

Hemos proporcionado un tutorial paso a paso de cómo implementar estas llamadas en una base de datos SQLite local. Pero ¿qué ocurre si desea usar una base de datos remota, en su lugar?

Si desea probarlo, necesitará su propia cuenta de Azure Active Directory (AAD) y la capacidad de hospedar su propio origen de datos.

Deberá agregar funciones para controlar las llamadas REST y, a continuación, cree una base de datos remoto para interactuar con la autenticación. Hay código en el completo [ejemplo de la base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) que puede hacer referencia para cada operación necesaria.

### <a name="settings-and-configuration"></a>Opciones y configuración

Los pasos necesarios para conectarse a su propia base de datos remota se establece en el [archivo Léame del ejemplo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Deberá hacer lo siguiente:

* Proporcione su Id. de cliente de cuenta de Azure para [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Proporcione la dirección url de la base de datos remoto [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Proporcionar la cadena de conexión de la base de datos [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Asociar la aplicación a la Microsoft Store.
* Copiar a través de la [proyecto de servicio](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) en su aplicación e implementarla en Azure.

### <a name="authentication"></a>Authentication

Deberá crear un botón para iniciar una secuencia de autenticación y un elemento emergente o una página independiente para recopilar información del usuario. Una vez se haya creado, deberá proporcionar código que solicita la información del usuario y lo usa para adquirir un token de acceso. El ejemplo de la base de datos de pedidos de cliente encapsula las llamadas de Microsoft Graph con el **WebAccountManager** biblioteca para adquirir un token y controlar la autenticación en una cuenta de AAD.

* Se implementa la lógica de autenticación en [ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* Se muestra el proceso de autenticación personalizado [ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) control.

### <a name="rest-calls"></a>Llamadas de REST

No tendrá que modificar cualquiera del código se ha agregado en este tutorial para implementar las llamadas REST. En su lugar, deberá hacer lo siguiente:

* Creación de nuevas implementaciones de la **ICustomerRepository** y **ITutorialRepository** interfaces, implementar el mismo conjunto de funciones a través de REST en lugar de SQLite. Se debe serializar y deserializar JSON y puede ajustar las llamadas REST en otro **HttpHelper** si necesita la clase. Consulte [el ejemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) para obtener información específica.
* En **App.xaml.cs**, cree una nueva función para inicializar el repositorio REST y llámelo en lugar de **SqliteDatabase** cuando se inicializa la aplicación. Nuevamente, consulte [el ejemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Una vez completados todos los tres pasos, debe poder autenticarse en su cuenta AAD a través de la aplicación. Llamadas de REST a la base de datos remoto reemplazará las llamadas de SQLite locales, pero la experiencia del usuario debe ser el mismo. Si se siente más ambicioso, puede agregar una página de configuración para permitir al usuario cambiar dinámicamente entre los dos.
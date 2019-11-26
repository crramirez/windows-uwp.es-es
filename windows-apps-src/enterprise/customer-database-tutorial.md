---
title: Crear una aplicación de base de datos de clientes
description: Cree una aplicación de base de datos de cliente y aprenda a implementar funciones básicas de aplicación empresarial.
keywords: Enterprise, tutorial, cliente, datos, crear lectura actualizar eliminar, REST, autenticación
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258616"
---
# <a name="tutorial-create-a-customer-database-application"></a>Tutorial: Crear una aplicación de base de datos de clientes

En este tutorial se crea una aplicación sencilla para administrar una lista de clientes. Al hacerlo, se presenta una selección de conceptos básicos para aplicaciones empresariales en UWP. Aquí aprenderás a hacer lo siguiente:

* Implementar operaciones de creación, lectura, actualización y eliminación en una base de datos SQL local.
* Agregue una cuadrícula de datos para mostrar y editar los datos del cliente en la interfaz de usuario.
* Organice los elementos de la interfaz de usuario en un diseño de formulario básico.

El punto de partida de este tutorial es una aplicación de una página con una interfaz de usuario y una funcionalidad mínimas, basada en una versión simplificada de la [aplicación de ejemplo de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Está escrito en C# y XAML, y esperamos que tenga una familiaridad básica con ambos lenguajes.

![Página principal de la aplicación de trabajo](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Requisitos previos

* [Asegúrese de que tiene la versión más reciente de Visual Studio y el SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Clonar o descargar el ejemplo de tutorial de base de datos Customer](https://github.com/microsoft/windows-tutorials-customer-database)

Después de clonar o descargar el repositorio, puede editar el proyecto abriendo **CustomerDatabaseTutorial. sln** con Visual Studio.

> [!NOTE]
> Consulte el [ejemplo de base de datos de pedidos de clientes completos](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver la aplicación en la que se basó este tutorial.

## <a name="part-1-code-of-interest"></a>Parte 1: código de interés

Si ejecuta la aplicación inmediatamente después de abrirla, verá algunos botones en la parte superior de una pantalla en blanco. Aunque no es visible para usted, la aplicación ya incluye una base de datos SQLite local aprovisionada con algunos clientes de prueba. Desde aquí, comenzará implementando un control de interfaz de usuario para mostrar esos clientes y, a continuación, pasará a agregar operaciones en la base de datos. Antes de empezar, aquí es donde trabajará.

### <a name="views"></a>Vistas

**CustomerListPage. Xaml** es la vista de la aplicación, que define la interfaz de usuario para la única página de este tutorial. Siempre que necesite agregar o cambiar un elemento visual en la interfaz de usuario, lo hará en este archivo. Este tutorial le guiará a través de la adición de estos elementos:

* Un **RadDataGrid** para mostrar y editar a los clientes. 
* Un **StackPanel** para establecer los valores iniciales de un nuevo cliente.

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.CS** es el lugar donde se encuentra la lógica fundamental de la aplicación. Cada acción del usuario realizada en la vista se pasará a este archivo para su procesamiento. En este tutorial, agregará código nuevo e implementará los siguientes métodos:

* **CreateNewCustomerAsync**, que inicializa un nuevo objeto CustomerViewModel.
* **DeleteNewCustomerAsync**, que quita un cliente nuevo antes de que se muestre en la interfaz de usuario.
* **DeleteAndUpdateAsync**, que controla la lógica del botón Eliminar.
* **GetCustomerListAsync**, que recupera una lista de clientes de la base de datos.
* **SaveInitialChangesAsync**, que agrega la información de un nuevo cliente a la base de datos.
* **UpdateCustomersAsync**, que actualiza la interfaz de usuario para reflejar los clientes agregados o eliminados.

**CustomerViewModel** es un contenedor para la información de un cliente, que realiza un seguimiento de si se ha modificado o no recientemente. No necesitará agregar nada a esta clase, pero parte del código que agregará en otra parte hará referencia a ella.

Para obtener más información sobre cómo se construye el ejemplo, consulte la [información general sobre la estructura](../enterprise/customer-database-app-structure.md)de la aplicación.

## <a name="part-2-add-the-datagrid"></a>Parte 2: agregar el control DataGrid

Antes de empezar a trabajar con los datos del cliente, deberá agregar un control de interfaz de usuario para mostrar esos clientes. Para ello, usaremos un control **RadDataGrid** de terceros prediseñado. El paquete de NuGet **Telerik. UI. for. UniversalWindowsPlatform** ya se ha incluido en este proyecto. Vamos a agregar la cuadrícula a nuestro proyecto.

1. Abra **Views\CustomerListPage.Xaml** desde el explorador de soluciones. Agregue la siguiente línea de código dentro de la etiqueta de **Página** para declarar una asignación al espacio de nombres Telerik que contiene la cuadrícula de datos.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Debajo de la **barra de datos en el** **RelativePanel** principal de la vista, agregue un control **RadDataGrid** con algunas opciones de configuración básicas:

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

3. Ha agregado la cuadrícula de datos, pero necesita que los datos se muestren. Agregue las siguientes líneas de código:

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Ahora que ha definido el origen de datos que se va a mostrar, **RadDataGrid** controlará la mayor parte de la lógica de la interfaz de usuario. Sin embargo, si ejecuta el proyecto, todavía no verá los datos en pantalla. Esto se debe a que ViewModel todavía no se carga.

![Aplicación vacía, sin clientes](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Parte 3: lectura de los clientes

Cuando se inicializa, **ViewModels\CustomerListPageViewModel.CS** llama al método **GetCustomerListAsync** . Ese método debe recuperar los datos del cliente de prueba de la base de datos SQLite que se incluye en el tutorial.

1. En **ViewModels\CustomerListPageViewModel.CS**, actualice el método **GetCustomerListAsync** con este código:

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
    Se llama al método **GetCustomerListAsync** cuando se carga ViewModel, pero antes de este paso, no hace nada. Aquí hemos agregado una llamada al método **GetAsync** en **repository/SqlCustomerRepository**. Esto le permite ponerse en contacto con el repositorio para recuperar una colección enumerable de objetos de cliente. A continuación, los analiza en objetos individuales, antes de agregarlos a su **ObservableCollection** interno para que se puedan mostrar y editar.

2. Ejecutar la aplicación: ahora verá la cuadrícula de datos que muestra la lista de clientes.

![Lista inicial de clientes](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Parte 4: edición de clientes

Puede editar las entradas de la cuadrícula de datos haciendo doble clic en ellas, pero debe asegurarse de que los cambios que realice en la interfaz de usuario también se realicen en la colección de clientes en el código subyacente. Esto significa que tendrá que implementar el enlace de datos bidireccional. Si desea obtener más información al respecto, consulte nuestra [Introducción al enlace de datos](../get-started/display-customers-in-list-learning-track.md).

1. En primer lugar, declare que **ViewModels\CustomerListPageViewModel.CS** implementa la interfaz **INotifyPropertyChanged** :

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Después, en el cuerpo principal de la clase, agregue el evento y el método siguientes:

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    El método **OnPropertyChanged** facilita a los establecedores la generación del evento **PropertyChanged** , que es necesario para el enlace de datos bidireccional.

3. Actualice el establecedor para **SelectedCustomer** con esta llamada de función:

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

4. En **Views\CustomerListPage.Xaml**, agregue la propiedad **SelectedCustomer** a la cuadrícula de datos.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Esto asocia la selección del usuario en la cuadrícula de datos con el objeto Customer correspondiente en el código subyacente. El modo de enlace TwoWay permite que los cambios realizados en la interfaz de usuario se reflejen en ese objeto.

5. Ejecute la aplicación. Ahora puede ver los clientes que se muestran en la cuadrícula y realizar cambios en los datos subyacentes a través de la interfaz de usuario.

![Edición de un cliente en la cuadrícula de datos](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Parte 5: actualizar clientes

Ahora que puede ver y editar sus clientes, necesitará poder insertar los cambios en la base de datos y extraer las actualizaciones realizadas por otros usuarios.

1. Vuelva a **ViewModels\CustomerListPageViewModel.CS**y navegue hasta el método **UpdateCustomersAsync** . Actualícelo con este código para insertar los cambios en la base de datos y recuperar cualquier información nueva:

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
    Este código emplea la propiedad **IsModified** de **ViewModels\CustomerViewModel.CS**, que se actualiza automáticamente cada vez que se cambia el cliente. Esto le permite evitar llamadas innecesarias y solo enviar los cambios de los clientes actualizados a la base de datos.

## <a name="part-6-create-a-new-customer"></a>Parte 6: creación de un nuevo cliente

Agregar un nuevo cliente presenta un desafío, ya que el cliente aparecerá como una fila en blanco si lo agrega a la interfaz de usuario antes de proporcionar valores para sus propiedades. Esto no es un problema, pero aquí facilitaremos la configuración de los valores iniciales de un cliente. En este tutorial, agregaremos un panel contraíble sencillo, pero si tuviera más información para agregar, podría crear una página independiente para este propósito.

### <a name="update-the-code-behind"></a>Actualizar el código subyacente

1. Agregue un nuevo campo privado y una propiedad pública a **ViewModels\CustomerListPageViewModel.CS**. Se usará para controlar si el panel está visible o no.

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

2. Agregue una nueva propiedad pública a ViewModel, una inversa del valor de **AddingNewCustomer**. Se usará para deshabilitar los botones normales de la barra de comandos cuando el panel esté visible.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Ahora necesitará una manera de mostrar el panel contraíble y crear un cliente para editarlo en él. 

3. Agregue un nuevo Fiend privado y una propiedad pública a ViewModel para que contenga el cliente recién creado.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Actualice el método **CreateNewCustomerAsync** para crear un nuevo cliente, agréguelo al repositorio y establézcalo como el cliente seleccionado:

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Actualice el método **SaveInitialChangesAsync** para agregar un cliente recién creado al repositorio, actualizar la interfaz de usuario y cerrar el panel.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Agregue la siguiente línea de código como la última línea del establecedor para **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Así se asegurará de que **EnableCommandBar** se actualiza automáticamente cada vez que se cambia **AddingNewCustomer** .

### <a name="update-the-ui"></a>Actualizar la interfaz de usuario

1. Vuelva a **Views\CustomerListPage.Xaml**y agregue un **StackPanel** con las siguientes propiedades entre el **CommandBar** y la cuadrícula de datos:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    El atributo **x:Load** garantiza que este panel solo aparece cuando se agrega un nuevo cliente.

2. Realice el siguiente cambio en la posición de la cuadrícula de datos para asegurarse de que se desplaza hacia abajo cuando aparece el nuevo panel:

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Actualice el panel de pila con cuatro controles de **cuadro de texto** . Se enlazarán a las propiedades individuales del nuevo cliente y le permitirá editar sus valores antes de agregarlos a la cuadrícula de datos.

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

4. Agregue un botón simple al nuevo panel de pila para guardar el cliente recién creado:

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Actualice el objeto **CommandBar**, de modo que los botones crear, eliminar y actualizar normales estén deshabilitados cuando el panel de pila esté visible:

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Ejecute la aplicación. Ahora puede crear un cliente y especificar sus datos en el panel de apilamiento.

![Creación de un nuevo cliente](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Parte 7: eliminación de un cliente

La eliminación de un cliente es la operación básica final que se debe implementar. Cuando elimine un cliente que haya seleccionado en la cuadrícula de datos, querrá llamar inmediatamente a **UpdateCustomersAsync** para actualizar la interfaz de usuario. Sin embargo, no es necesario llamar a este método si va a eliminar un cliente que acaba de crear.

1. Vaya a **ViewModels\CustomerListPageViewModel.CS**y actualice el método **DeleteAndUpdateAsync** :

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

2. En **Views\CustomerListPage.Xaml**, actualice el panel de pila para agregar un nuevo cliente para que contenga un segundo botón:

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

3. En **ViewModels\CustomerListPageViewModel.CS**, actualice el método **DeleteNewCustomerAsync** para eliminar el nuevo cliente:

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

4. Ejecute la aplicación. Ahora puede eliminar clientes, ya sea dentro de la cuadrícula de datos o en el panel de apilamiento.

![Eliminar un cliente nuevo](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Conclusión

Enhorabuena. Una vez hecho todo esto, la aplicación tiene ahora una gama completa de operaciones de base de datos local. Puede crear, leer, actualizar y eliminar clientes dentro de la interfaz de usuario, y estos cambios se guardan en la base de datos y se conservan en diferentes inicios de la aplicación.

Ahora que ya ha terminado, tenga en cuenta lo siguiente:
* Si todavía no lo ha hecho, consulte la [información general](../enterprise/customer-database-app-structure.md) sobre la estructura de la aplicación para obtener más información sobre por qué la aplicación se compila de la manera que es.
* Explore el [ejemplo de base de datos de pedidos de clientes completos](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver la aplicación en la que se basó este tutorial.

O bien, si es un desafío, puede continuar en adelante...

## <a name="going-further-connect-to-a-remote-database"></a>Más información: conectarse a una base de datos remota

Hemos proporcionado un tutorial paso a paso sobre cómo implementar estas llamadas en una base de datos SQLite local. Pero ¿qué ocurre si desea usar una base de datos remota, en su lugar?

Si desea realizar esta prueba, necesitará su propia cuenta de Azure Active Directory (AAD) y la capacidad de hospedar su propio origen de datos.

Deberá agregar autenticación, funciones para controlar las llamadas de REST y, después, crear una base de datos remota para interactuar con. Existe código en el ejemplo de [base de datos Full Customer Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database) , al que se puede hacer referencia para cada operación necesaria.

### <a name="settings-and-configuration"></a>Configuración y configuración

Los pasos necesarios para conectarse a su propia base de datos remota se escriben en el [archivo Léame del ejemplo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Deberá hacer lo siguiente:

* Proporcione el identificador de cliente de la cuenta de Azure en [constants.CS](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Proporcione la dirección URL de la base de datos remota a [constants.CS](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Proporcione la cadena de conexión de la base de datos a [constants.CS](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Asocie la aplicación a la Microsoft Store.
* Copie el [proyecto de servicio](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) en la aplicación e impleméntelo en Azure.

### <a name="authentication"></a>Authentication

Tendrá que crear un botón para iniciar una secuencia de autenticación y una ventana emergente o una página independiente para recopilar la información de un usuario. Una vez que lo haya creado, deberá proporcionar código que solicite la información de un usuario y la use para adquirir un token de acceso. El ejemplo de base de datos Orders Customer incluye Microsoft Graph llamadas con la biblioteca **WebAccountManager** para adquirir un token y controlar la autenticación en una cuenta de AAD.

* La lógica de autenticación se implementa en [**AuthenticationViewModel.CS**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* El proceso de autenticación se muestra en el control personalizado [**AuthenticationControl. Xaml**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) .

### <a name="rest-calls"></a>Llamadas de REST

No es necesario modificar ninguno de los códigos que se agregaron en este tutorial para implementar llamadas REST. En su lugar, deberá hacer lo siguiente:

* Cree nuevas implementaciones de las interfaces **ICustomerRepository** y **ITutorialRepository** , implementando el mismo conjunto de funciones a través de REST en lugar de SQLite. Deberá serializar y deserializar JSON, y puede ajustar las llamadas REST en una clase **HttpHelper** independiente si es necesario. Consulte [el ejemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) para obtener información específica.
* En **app.Xaml.CS**, cree una nueva función para inicializar el repositorio de REST y llámela en lugar de **SqliteDatabase** cuando se inicializa la aplicación. De nuevo, consulte [el ejemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Una vez completados los tres pasos, debe poder autenticarse en su cuenta de AAD a través de la aplicación. Las llamadas de REST a la base de datos remota reemplazarán las llamadas locales de SQLite, pero la experiencia del usuario debe ser la misma. Si es incluso más ambicioso, puede Agregar una página de configuración para permitir que el usuario cambie dinámicamente entre los dos.

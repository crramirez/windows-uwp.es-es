---
description: En este tutorial se muestra cómo facilitar la elección de la fecha de un informe de gastos en un dispositivo táctil.
title: Incorporación de un control CalendarView en UWP mediante islas XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 53a87f5803a6e4707b2dc6b86b32b7db003e59ef
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133048"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>3\.ª parte: Incorporación de un control CalendarView en UWP mediante islas XAML

Esta es la tercera parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso Expenses. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, consulta [Tutorial: Modernización de una aplicación WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya has completado la [parte 2](modernize-wpf-tutorial-2.md).

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso quiere facilitar la elección de la fecha de un informe de gastos en un dispositivo con el control táctil habilitado. En esta parte del tutorial, agregarás un control [CalendarView](/windows/uwp/design/controls-and-patterns/calendar-view) de UWP a la aplicación. Este es el mismo control que se usa en la funcionalidad de fecha y hora de Windows 10 en la barra de tareas.

![Imagen de CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

A diferencia del control **InkCanvas** que agregaste en la [parte 2](modernize-wpf-tutorial-2.md), el kit de herramientas de la comunidad de Windows no proporciona una versión ajustada de **CalendarView** de UWP que se pueda usar en aplicaciones de WPF. Como alternativa, hospedarás un elemento **InkCanvas** en el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) genérico. Puedes usar este control para hospedar cualquier control de UWP de origen proporcionado por la biblioteca de Windows SDK o WinUI o cualquier control personalizado de UWP creado por un tercero. El control **WindowsXamlHost** se proporciona mediante el paquete NuGet del paquete de `Microsoft.Toolkit.Wpf.UI.XamlHost`. Este paquete se incluye con el paquete NuGet de `Microsoft.Toolkit.Wpf.UI.Controls` que instalaste en la [parte 2](modernize-wpf-tutorial-2.md).

> [!NOTE]
> En este tutorial solo se muestra cómo usar [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar el control de  **CalendarView** de origen que proporciona Windows SDK. Para ver un tutorial que muestra cómo hospedar un control personalizado, consulta [Hospedaje de un control personalizado de UWP en una aplicación de WPF mediante islas XAML](host-custom-control-with-xaml-islands.md).

Para usar el control **WindowsXamlHost**, debes llamar directamente a las API de WinRT desde el código en la aplicación WPF. El paquete NuGet de `Microsoft.Windows.SDK.Contracts` contiene las referencias necesarias para que puedas llamar a las API de WinRT desde la aplicación. Este paquete se incluye también en el paquete NuGet de `Microsoft.Toolkit.Wpf.UI.Controls` que instalaste en la [parte 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Adición del control WindowsXamlHost

1. En el **Explorador de soluciones**, expande la carpeta **Vistas** del proyecto **ContosoExpenses.Core** y haz doble clic en el archivo **AddNewExpense.xaml**. Este es el formulario que se usa para agregar un nuevo gasto a la lista. Aquí se muestra cómo aparece en la versión actual de la aplicación.

    ![Adición de nuevo gasto](images/wpf-modernize-tutorial/AddNewExpense.png)

    El control de selector de fecha incluido en WPF está pensado para equipos tradicionales con mouse y teclado. La selección de una fecha con una pantalla táctil no es realmente factible, debido al reducido tamaño del control y al espacio limitado entre cada día del calendario.

2. En la parte superior del archivo **AddNewExpense.xaml**, agrega el atributo siguiente al elemento **Window**.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Después de agregar este atributo, el elemento **Window** debe tener ahora un aspecto como este.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. Cambia el atributo de **altura** del elemento **Window** de 450 a 800. Esto es necesario porque el control **CalendarView** de UWP ocupa más espacio que el selector de fecha de WPF.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. Busca el elemento `DatePicker` situado cerca de la parte inferior del archivo y reemplace este elemento por el código XAML siguiente.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Este XAML agrega el control **WindowsXamlHost**. La propiedad **InitialTypeName** indica el nombre completo del control de UWP que quiere hospedar (en este caso, **Windows.UI.Xaml.Controls.CalendarView**).

5. Presiona F5 para compilar y ejecutar la aplicación en el depurador. Elige un empleado de la lista y, a continuación, presiona el botón **Agregar gasto nuevo**. Confirma que la siguiente página hospeda el nuevo control **CalendarView** de UWP.

    ![Contenedor de CalendarView](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Cierra la aplicación.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interacción con el control WindowsXamlHost

A continuación, actualizarás la aplicación para procesar la fecha seleccionada, la mostrarás en la pantalla y rellenarás el objeto **Expense** para guardarlo en la base de datos.

El elemento [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) de UWP contiene dos miembros que son pertinentes para este escenario:

- La propiedad **SelectedDates** contiene la fecha seleccionada por el usuario.
- El evento **SelectedDatesChanged** se genera cuando el usuario selecciona una fecha.

Sin embargo, el control **WindowsXamlHost** es un control host genérico para *cualquier* tipo de control de UWP. Como tal, no expone una propiedad llamada **SelectedDates** ni un evento llamado **SelectedDatesChanged**, porque son específicos del control **CalendarView**. Para tener acceso a estos miembros, debes escribir código que convierta el elemento **WindowsXamlHost** al tipo **CalendarView**. El mejor método para hacerlo es responder al evento **ChildChanged** del control **WindowsXamlHost**, que se genera cuando se ha representado el control hospedado.

1. En el archivo **AddNewExpense.xaml**, agrega un controlador de eventos para el evento **ChildChanged** del control **WindowsXamlHost** que agregaste anteriormente. Cuando hayas terminado, el elemento **WindowsXamlHost** debe tener el siguiente aspecto.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. En el mismo archivo, busca el elemento **Grid.RowDefinitions** del objeto **Grid** principal. Agrega uno o varios elementos **RowDefinition** con el **alto** establecido en la opción **automática** al final de la lista de elementos secundarios. Cuando hayas terminado, el elemento **Grid.RowDefinitions** debe ser similar a este (ahora debería haber 9 elementos **RowDefinition**).

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. Agrega el siguiente código XAML después del elemento **WindowsXamlHost** y antes del elemento **Button** situado cerca del final del archivo.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Busca el elemento **Button** situado cerca del final del archivo y cambia la propiedad **Grid.Row** de **7** a **8**. De este modo, se desplaza el botón una fila hacia abajo en la cuadrícula porque agregaste una nueva fila.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Abre el archivo de código **AddNewExpense.xaml.cs**.

7. Agrega la siguiente instrucción al principio del archivo.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Agrega el siguiente controlador de eventos a la clase `AddNewExpense`. Este código implementa el evento **ChildChanged** del control **WindowsXamlHost** que se declaró anteriormente en el archivo XAML.

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    Este código usa la propiedad **Child** del control **WindowsXamlHost** para tener acceso al control **CalendarView** de UWP. Después, el código se suscribe al evento **SelectedDatesChanged**, que se desencadena cuando el usuario selecciona una fecha en el calendario. Este controlador de eventos pasa la fecha seleccionada a ViewModel, donde se crea el nuevo objeto **Expense** y se guarda en la base de datos. Para ello, el código usa la infraestructura de mensajería que proporciona el paquete NuGet MVVM Light. El código envía un mensaje llamado **SelectedDateMessage** a ViewModel, que lo recibirá y establecerá la propiedad **Date** con el valor seleccionado. En este escenario, el control **CalendarView** está configurado para el modo de selección única, por lo que la colección contendrá un solo elemento.

    En este momento, el proyecto todavía no se compilará porque falta la implementación de la clase **SelectedDateMessage**. En los pasos siguientes se implementa esta clase.

9. En el **Explorador de soluciones**, haz clic con el botón derecho en la carpeta **Mensajes** y elige **Agregar > Clase**. Asigna a la nueva clase el nombre **SelectedDateMessage** y haz clic en **Agregar**.

10. Reemplaza el contenido del archivo de código **SelectedDateMessage.cs** por el siguiente código. Este código agrega una instrucción using para el espacio de nombres `GalaSoft.MvvmLight.Messaging` (desde el paquete NuGet MVVM Light), hereda de la clase **MessageBase** y agrega la propiedad **DateTime** que se inicializa a través del constructor público. Cuando hayas terminado, la página debe tener el siguiente aspecto.

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. A continuación, actualizarás ViewModel para recibir este mensaje y rellenarás la propiedad **Date** de ViewModel. En **Explorador de soluciones**, expande la carpeta **ViewModels** y abre el archivo **AddNewExpenseViewModel.cs**.

12. Busca el constructor público para la clase `AddNewExpenseViewModel` y agrega el código siguiente al final del constructor. Este código se registra para recibir el valor **SelectedDateMessage** y extraer de él la fecha seleccionada a través de la propiedad **SelectedDate**, y se usa para establecer la propiedad **Date** expuesta por ViewModel. Dado que esta propiedad está enlazada al control **TextBlock** que agregaste anteriormente, puedes ver la fecha seleccionada por el usuario.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Cuando hayas terminado, el constructor `AddNewExpenseViewModel` debe tener el siguiente aspecto. 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > El método `SaveExpenseCommand` del mismo archivo de código realiza la tarea de guardar los gastos en la base de datos. Este método usa la propiedad **Date** para crear el nuevo objeto **Expense**. Esta propiedad se rellena ahora mediante el control **CalendarView** a través del mensaje de `SelectedDateMessage` que acabas de crear.

13. Presiona F5 para compilar y ejecutar la aplicación en el depurador. Elige uno de los empleados disponibles y, a continuación, haz clic en el botón **Agregar gasto nuevo**. Completa todos los campos del formulario y elige una fecha en el nuevo control **CalendarView**. Confirma que la fecha seleccionada se muestra como una cadena debajo del control.

14. Presiona el botón **Guardar**. El formulario se cerrará y el nuevo gasto se agrega al final de la lista de gastos. La primera columna con la fecha de gasto debe ser la fecha seleccionada en el control **CalendarView**.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, has reemplazado correctamente un control de fecha y hora de WPF por el control **CalendarView** de UWP, que admite bolígrafos digitales y táctiles, además de la entrada del mouse y del teclado. Aunque el kit de herramientas de la Comunidad Windows no proporciona una versión ajustada del control **CalendarView** de UWP que se pueda usar directamente en una aplicación de WPF, podías hospedar el control mediante el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) genérico.

Ahora ya estás listo para continuar con la [Parte 4: Incorporación de actividades y notificaciones del usuario de Windows 10](modernize-wpf-tutorial-4.md).
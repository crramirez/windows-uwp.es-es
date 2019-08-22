---
description: En este tutorial se muestra cómo agregar interfaces de usuario XAML de UWP, crear paquetes de MSIX e incorporar otros componentes modernos en la aplicación WPF.
title: Incorporación de un control CalendarView en UWP mediante islas XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643371"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Parte 3: Incorporación de un control CalendarView en UWP mediante islas XAML

Esta es la tercera parte de un tutorial que muestra cómo modernizar una aplicación de escritorio WPF de ejemplo denominada gastos de contoso. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de [ejemplo, vea Tutorial: Modernizar una aplicación](modernize-wpf-tutorial.md)de WPF. En este artículo se da por supuesto que ya ha completado la [parte 2](modernize-wpf-tutorial-2.md).

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso quiere facilitar la elección de la fecha de un informe de gastos en un dispositivo habilitado para Touch. En esta parte del tutorial, agregará un control [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) de UWP a la aplicación. Este es el mismo control que se usa en la funcionalidad de fecha y hora de Windows 10 en la barra de tareas.

![Imagen de CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

A diferencia del control **InkCanvas** agregado en la [parte 2](modernize-wpf-tutorial-2.md), el kit de herramientas de la comunidad de Windows no proporciona una versión ajustada del **CalendarView** de UWP que se puede usar en aplicaciones de WPF. Como alternativa, hospedará un **InkCanvas** en el control genérico [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) . Puede usar este control para hospedar cualquier control UWP de primera parte proporcionado por la biblioteca Windows SDK o WinUI o cualquier control personalizado de UWP creado por un tercero. El control **WindowsXamlHost** se proporciona mediante el `Microsoft.Toolkit.Wpf.UI.XamlHost` paquete NuGet del paquete. Este paquete se incluye con el `Microsoft.Toolkit.Wpf.UI.Controls` paquete de NuGet que instaló en la [parte 2](modernize-wpf-tutorial-2.md).

> [!NOTE]
> En este tutorial solo se muestra cómo usar [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar el control **CalendarView** de primera entidad proporcionado por el Windows SDK. Para ver un tutorial que muestra cómo hospedar un control personalizado, vea [hospedar un control de UWP personalizado en una aplicación WPF con islas XAML](host-custom-control-with-xaml-islands.md).

Para usar el control **WindowsXamlHost** , debe llamar directamente a las API de WinRT desde el código en la aplicación WPF. El `Microsoft.Windows.SDK.Contracts` paquete NuGet contiene las referencias necesarias para que pueda llamar a las API de WinRT desde la aplicación. Este paquete también se incluye en el `Microsoft.Toolkit.Wpf.UI.Controls` paquete NuGet que instaló en la [parte 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Agregar el control WindowsXamlHost

1. En **Explorador de soluciones**, expanda la carpeta views en el proyecto **ContosoExpenses. Core** y haga doble clic en el archivo **AddNewExpense. Xaml** . Este es el formulario que se usa para agregar un nuevo gasto a la lista. Aquí se muestra cómo aparece en la versión actual de la aplicación.

    ![Agregar nuevo gasto](images/wpf-modernize-tutorial/AddNewExpense.png)

    El control selector de fecha incluido en WPF está pensado para equipos tradicionales con Mouse y teclado. Elegir una fecha con una pantalla táctil no es realmente factible, debido al pequeño tamaño del control y al espacio limitado entre cada día del calendario.

2. En la parte superior del archivo **AddNewExpense. Xaml** , agregue el atributo siguiente al elemento **Window** .

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Después de agregar este atributo, el elemento de **ventana** debe tener el siguiente aspecto.

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

3. Cambie el atributo **height** del elemento **Window** de 450 a 800. Esto es necesario porque el control **CalendarView** de UWP tiene más espacio que el selector de fecha de WPF.

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

4. Busque el `DatePicker` elemento situado cerca de la parte inferior del archivo y reemplace este elemento por el código XAML siguiente.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Este código XAML agrega el control **WindowsXamlHost** . La propiedad **InitialTypeName** indica el nombre completo del control UWP que quiere hospedar (en este caso, **Windows. UI. Xaml. Controls. CalendarView**).

5. Presione F5 para compilar y ejecutar la aplicación en el depurador. Elija un empleado en la lista y, a continuación, presione el botón **Agregar nuevo gasto** . Confirme que la siguiente página hospeda el nuevo control **CalendarView** de UWP.

    ![Contenedor de CalendarView](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Cierra la aplicación.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interacción con el control WindowsXamlHost

A continuación, actualizará la aplicación para procesar la fecha seleccionada, la mostrará en la pantalla y rellenará el objeto de **gastos** para guardarlo en la base de datos.

El [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) de UWP contiene dos miembros que son relevantes para este escenario:

- La propiedad **SelectedDates** contiene la fecha seleccionada por el usuario.
- El evento **SelectedDatesChanged** se genera cuando el usuario selecciona una fecha.

Sin embargo, el control **WindowsXamlHost** es un control de host genérico para *cualquier* tipo de control de UWP. Como tal, no expone una propiedad denominada **SelectedDates** o un evento denominado **SelectedDatesChanged**, porque son específicos del control **CalendarView** . Para tener acceso a estos miembros, debe escribir código que convierta **WindowsXamlHost** en el tipo **CalendarView** . El mejor lugar para hacerlo es responder al evento **ChildChanged** del control **WindowsXamlHost** , que se genera cuando se representa el control hospedado.

1. En el archivo **AddNewExpense. Xaml** , agregue un controlador de eventos para el evento **ChildChanged** del control **WindowsXamlHost** que agregó anteriormente. Cuando haya terminado, el elemento **WindowsXamlHost** debería tener este aspecto.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. En el mismo archivo, busque el elemento **Grid. RowDefinitions** de la **cuadrícula**principal. Agregue un elemento **RowDefinition** más con el **alto** igual a **auto** al final de la lista de elementos secundarios. Cuando haya terminado, el elemento **Grid. RowDefinitions** debería tener este aspecto (ahora debería haber 9 elementos **RowDefinition** ).

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

4. Agregue el siguiente código XAML después del elemento **WindowsXamlHost** y antes del elemento **Button** situado cerca del final del archivo.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Busque el elemento **Button** cerca del final del archivo y cambie la propiedad **Grid. Row** de **7** a **8**. Esto desplaza el botón una fila hacia abajo en la cuadrícula porque agregó una nueva fila.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Abra el archivo de código **AddNewExpense.Xaml.CS** .

7. Agregue la siguiente instrucción al principio del archivo.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Agregue el siguiente controlador de eventos a `AddNewExpense` la clase. Este código implementa el evento **ChildChanged** del control **WindowsXamlHost** que declaró anteriormente en el archivo XAML.

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

    Este código usa la propiedad **secundaria** del control **WindowsXamlHost** para tener acceso al control **CalendarView** de UWP. Después, el código se suscribe al evento **SelectedDatesChanged** , que se desencadena cuando el usuario selecciona una fecha en el calendario. Este controlador de eventos pasa la fecha seleccionada al ViewModel en el que se crea el nuevo objeto de **gastos** y se guarda en la base de datos. Para ello, el código usa la infraestructura de mensajería que proporciona el paquete NuGet Light de MVVM. El código envía un mensaje denominado **SelectedDateMessage** al ViewModel, que lo recibirá y establecerá la propiedad **Date** con el valor seleccionado. En este escenario, el control **CalendarView** está configurado para el modo de selección única, por lo que la colección contendrá un solo elemento.

    En este momento, el proyecto todavía no se compilará porque falta la implementación de la clase **SelectedDateMessage** . En los pasos siguientes se implementa esta clase.

9. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta **mensajes** y elija **Agregar > clase**. Asigne a la nueva clase el nombre **SelectedDateMessage** y haga clic en **Agregar**.

10. Reemplace el contenido del archivo de código **SelectedDateMessage.CS** por el código siguiente. Este código agrega una instrucción using para el `GalaSoft.MvvmLight.Messaging` espacio de nombres (desde el paquete NuGet Light de MVVM), hereda de la clase **MessageBase** y agrega la propiedad **DateTime** que se inicializa a través del constructor público. Cuando haya terminado, el archivo debe tener este aspecto.

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

11. A continuación, actualizará el ViewModel para recibir este mensaje y rellenar la propiedad **Date** de ViewModel. En **Explorador de soluciones**, expanda la carpeta **ViewModels** y abra el archivo **AddNewExpenseViewModel.CS** .

12. Busque el constructor público para la `AddNewExpenseViewModel` clase y agregue el código siguiente al final del constructor. Este código se registra para recibir el **SelectedDateMessage**, extraer la fecha seleccionada a través de la propiedad **SelectedDate** y se usa para establecer la propiedad **Date** expuesta por el ViewModel. Dado que esta propiedad está enlazada con el control **TextBlock** que agregó anteriormente, puede ver la fecha seleccionada por el usuario.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Cuando haya terminado, el `AddNewExpenseViewModel` constructor debería tener este aspecto. 

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
    > El `SaveExpenseCommand` método en el mismo archivo de código realiza el trabajo de guardar los gastos en la base de datos. Este método usa la propiedad **Date** para crear el nuevo objeto de **gastos** . Esta propiedad se rellena ahora mediante el control **CalendarView** a través del `SelectedDateMessage` mensaje que acaba de crear.

13. Presione F5 para compilar y ejecutar la aplicación en el depurador. Elija uno de los empleados disponibles y, a continuación, haga clic en el botón **Agregar nuevo gasto** . Complete todos los campos del formulario y elija una fecha en el nuevo control **CalendarView** . Confirme que la fecha seleccionada se muestra como una cadena debajo del control.

14. Presione el botón **Guardar** . El formulario se cerrará y el nuevo gasto se agrega al final de la lista de gastos. La primera columna con la fecha de gasto debe ser la fecha seleccionada en el control **CalendarView** .

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha reemplazado correctamente un control de fecha y hora de WPF por el control **CalendarView** de UWP, que admite bolígrafos digitales y táctiles, además de la entrada del mouse y del teclado. Aunque el kit de herramientas de la comunidad de Windows no proporciona una versión ajustada del control **CalendarView** de UWP que se puede usar directamente en una aplicación WPF, pudo hospedar el control mediante el control genérico [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .

Ya está listo para [la parte 4: Agregue las actividades y notificaciones](modernize-wpf-tutorial-4.md)de usuario de Windows 10.

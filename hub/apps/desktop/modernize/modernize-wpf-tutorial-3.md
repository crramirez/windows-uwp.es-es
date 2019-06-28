---
description: Este tutorial muestra cómo agregar interfaces de usuario de UWP XAML y crear paquetes MSIX para incorporar otros componentes modernos en su aplicación WPF.
title: Agregar un control de UWP CalendarView mediante islas de XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420106"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Parte 3: Agregar un control de UWP CalendarView mediante islas de XAML

Se trata de la tercera parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso gastos. Para obtener información general del tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, vea [Tutorial: Modernizar una aplicación de WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya ha completado [parte 2](modernize-wpf-tutorial-2.md).

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso desea que sea más fácil elegir la fecha de un informe de gastos en un dispositivo habilitado para toque. En esta parte del tutorial, agregará una UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) control a la aplicación. Este es el mismo control que se usa en la funcionalidad de fecha y hora de Windows 10 en la barra de tareas.

![Imagen CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

A diferencia de la **InkCanvas** control que ha agregado en [parte 2](modernize-wpf-tutorial-2.md), el Kit de herramientas de la Comunidad de Windows no proporciona una versión de UWP ajustada **CalendarView** que puede utilizarse en aplicaciones WPF . Como alternativa, hospedarán un **InkCanvas** en el modelo genérico [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control. Puede utilizar este control para hospedar cualquier control UWP proporcionado por el proveedor de terceros plataforma o una 3. El **WindowsXamlHost** proporciona control el `Microsoft.Toolkit.Wpf.UI.XamlHost` paquete NuGet de paquete. Este paquete se incluye con el `Microsoft.Toolkit.Wpf.UI.Controls` paquete de NuGet que instaló en [parte 2](modernize-wpf-tutorial-2.md).

Para poder usar el **WindowsXamlHost** (control), deberá llamar directamente a WinRT APIs de código en la aplicación WPF. El `Microsoft.Windows.SDK.Contracts` paquete NuGet contiene las referencias necesarias para que pueda llamar a WinRT APIs desde la aplicación. Este paquete también se incluye en el `Microsoft.Toolkit.Wpf.UI.Controls` paquete de NuGet que instaló en [parte 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Agregar el control WindowsXamlHost

1. En **el Explorador de soluciones**, expanda el **vistas** carpeta en el **ContosoExpenses.Core** del proyecto y haga doble clic en el **AddNewExpense.xaml**archivo. Este es el formulario que se usa para agregar un gasto nuevo a la lista. Aquí es cómo aparece en la versión actual de la aplicación.

    ![Agregar nueva gastos](images/wpf-modernize-tutorial/AddNewExpense.png)

    El control de selector de fecha incluido en WPF está pensado para los equipos tradicionales con mouse y teclado. Elegir una fecha con una pantalla táctil, no es realmente factible, dado su pequeño tamaño del control y el espacio limitado entre cada día en el calendario.

2. En la parte superior de la **AddNewExpense.xaml** , agregue el atributo siguiente a la **ventana** elemento.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Después de agregar este atributo, el **ventana** elemento ahora debería parecerse a esto.

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

3. Cambiar el **alto** atributo de la **ventana** elemento de 450 a 800. Esto es necesario porque la UWP **CalendarView** control ocupa más espacio que el selector de fecha WPF.

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

4. Busque el `DatePicker` elemento cerca de la parte inferior del archivo y reemplace este elemento con el XAML siguiente.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Agrega este XAML el **WindowsXamlHost** control. El **InitialTypeName** propiedad indica el nombre completo del que desea hospedar el control de UWP (en este caso, **Windows.UI.Xaml.Controls.CalendarView**).

5. Presione F5 para compilar y ejecutar la aplicación en el depurador. Elija un empleado en la lista y, a continuación, presione el **Agregar gasto nuevo** botón. Confirme que la siguiente página hospeda el nuevo UWP **CalendarView** control.

    ![Contenedor CalendarView](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Cierra la aplicación.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interactuar con el control WindowsXamlHost

A continuación, actualizará la aplicación para procesar la fecha seleccionada, mostrarlo en la pantalla y rellenar el **gastos** objeto va a guardar en la base de datos.

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) contiene dos miembros que son relevantes para este escenario:

- El **SelectedDates** propiedad contiene la fecha seleccionada por el usuario.
- El **SelectedDatesChanged** evento se produce cuando el usuario selecciona una fecha.

Sin embargo, el **WindowsXamlHost** trata de un control host genérico para *cualquier* tipo de control UWP. Por lo tanto, no expone una propiedad denominada **SelectedDates** o un evento llamado **SelectedDatesChanged**, porque son específicas de la **CalendarView** control. Para obtener acceso a estos miembros, debe escribir código que convierte el **WindowsXamlHost** a la **CalendarView** tipo. Es el mejor lugar para hacer esto en respuesta a la **ChildChanged** eventos de la **WindowsXamlHost** control, que se produce cuando el control hospedado se ha representado.

1. En el **AddNewExpense.xaml** de archivos, agregar un controlador de eventos para el **ChildChanged** eventos de la **WindowsXamlHost** control agregado anteriormente. Cuando haya terminado, la **WindowsXamlHost** elemento debe tener este aspecto.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. En el mismo archivo, busque el **Grid.RowDefinitions** elemento de los principales **cuadrícula**. Agregar uno más **RowDefinition** elemento con el **alto** igual a **automática** al final de la lista de elementos secundarios. Cuando haya terminado, la **Grid.RowDefinitions** elemento debe tener este aspecto (ya debe haber 9 **RowDefinition** elementos).

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

4. Agregue el siguiente XAML después de la **WindowsXamlHost** elemento y antes de la **botón** elemento cerca del final del archivo.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Busque el **botón** elemento cerca del final del archivo y cambie el **Grid.Row** propiedad desde **7** a **8**. Se desplaza el botón de la fila en la cuadrícula porque se ha agregado una nueva fila.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Abra el **AddNewExpense.xaml.cs** archivo de código.

7. Agregue la siguiente instrucción al principio del archivo.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Agregue el siguiente controlador de eventos para el `AddNewExpense` clase. Este código implementa la **ChildChanged** eventos de la **WindowsXamlHost** control declaró anteriormente en el archivo XAML.

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

    Este código usa el **secundarios** propiedad de la **WindowsXamlHost** control para tener acceso a UWP **CalendarView** control. El código, a continuación, se suscribe a la **SelectedDatesChanged** evento, que se desencadena cuando el usuario selecciona una fecha del calendario. Este controlador de eventos pasa la fecha seleccionada al ViewModel donde el nuevo **gastos** objeto se crea y se guarda en la base de datos. Para ello, el código usa la infraestructura de mensajería proporcionada por el paquete NuGet de MVVM Light. El código envía un mensaje denominado **SelectedDateMessage** al ViewModel, que se reciben y se establecen el **fecha** propiedad con el valor seleccionado. En este escenario el **CalendarView** control está configurado para el modo de selección única, por lo que la colección contendrá un solo elemento.

    En este momento, el proyecto aún no se compilará porque falta la implementación para el **SelectedDateMessage** clase. Los pasos siguientes implementan esta clase.

9. En **el Explorador de soluciones**, haga clic en el **mensajes** carpeta y elija **Agregar -> clase**. Nombre de la nueva clase **SelectedDateMessage** y haga clic en **agregar**.

10. Reemplace el contenido de la **SelectedDateMessage.cs** archivo de código con el código siguiente. Este código agrega el uso de una instrucción para el `GalaSoft.MvvmLight.Messaging` hereda el espacio de nombres (desde el paquete NuGet de MVVM Light), el **MessageBase** clase y agrega **DateTime** propiedad que se inicializa a través de la constructor público. Cuando haya terminado, el archivo debe tener este aspecto.

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

11. A continuación, actualizará el ViewModel para recibir este mensaje y rellenar el **fecha** propiedad del ViewModel. En **el Explorador de soluciones**, expanda el **ViewModels** carpeta y abra el **AddNewExpenseViewModel.cs** archivo.

12. Busque el constructor público para el `AddNewExpenseViewModel` de clases y agregue el código siguiente al final del constructor. Este código registra para recibir el **SelectedDateMessage**, extraer la fecha seleccionada de ellos a través de la **SelectedDate** propiedad y se utiliza para establecer el **fecha** propiedad expone el modelo de vista. Dado que está enlazada esta propiedad con el **TextBlock** control agregado anteriormente, puede ver la fecha seleccionada por el usuario.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Cuando haya terminado, la `AddNewExpenseViewModel` constructor tendrá un aspecto similar al siguiente. 

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
    > El `SaveExpenseCommand` método en el mismo archivo de código realiza el trabajo de almacenamiento de los gastos en la base de datos. Este método usa la **fecha** propiedad que se va a crear el nuevo **gastos** objeto. Esta propiedad se rellena ahora por el **CalendarView** controlar a través de la `SelectedDateMessage` mensaje recién creado.

13. Presione F5 para compilar y ejecutar la aplicación en el depurador. Elija uno de los empleados disponibles y, a continuación, haga clic en el **Agregar gasto nuevo** botón. Complete todos los campos del formulario y elija una fecha de la nueva **CalendarView** control. Confirme que la fecha seleccionada se muestra como una cadena debajo del control.

14. Presione el **guardar** botón. Se cerrará el formulario y el gasto nueva se agrega al final de la lista de gastos. La primera columna con la fecha del gasto debe ser la fecha seleccionada en el **CalendarView** control.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha creado correctamente un control WPF fecha y hora con UWP **CalendarView** control, que es compatible con táctiles y lápices digitales además de mouse y teclado. Aunque el Kit de herramientas de la Comunidad de Windows no proporciona una versión de UWP ajustada **CalendarView** control que se puede usar directamente en una aplicación WPF, ha sido capaz de hospedar el control mediante el modelo genérico [WindowsXamlHost ](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control.

Ahora está listo para [parte 4: Agregar actividades de usuario de Windows 10 y notificaciones](modernize-wpf-tutorial-4.md).

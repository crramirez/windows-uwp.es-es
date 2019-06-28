---
description: Este tutorial muestra cómo agregar interfaces de usuario de UWP XAML y crear paquetes MSIX para incorporar otros componentes modernos en su aplicación WPF.
title: Agregar un control de UWP InkCanvas mediante islas de XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420085"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>2ª parte: Agregar un control de UWP InkCanvas mediante islas de XAML

Se trata de la segunda parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso gastos. Para obtener información general del tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, vea [Tutorial: Modernizar una aplicación de WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya ha completado [parte 1](modernize-wpf-tutorial-1.md).

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso desea agregar compatibilidad para las firmas digitales en la aplicación de gastos de Contoso. UWP **InkCanvas** control es una buena opción para este escenario, porque admite la entrada de lápiz digital y características con inteligencia artificial, como la capacidad de reconocer texto y formas. Para ello, usará el [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) ajusta el control UWP disponible en el Kit de herramientas de la Comunidad de Windows. Este control ajusta la interfaz y funcionalidad de UWP **InkCanvas** control para su uso en una aplicación WPF. Para obtener más información acerca de los controles UWP ajustados, consulte [XAML de UWP de Host se controla en aplicaciones de escritorio (Islas de XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurar el proyecto para usar islas de XAML

Antes de poder agregar un **InkCanvas** control a la aplicación de gastos de Contoso, primero es preciso configurar el proyecto para admitir las Islas de XAML para UWP.

1. En Visual Studio 2019, haga doble clic en el **ContosoExpenses.Core** proyecto **el Explorador de soluciones** y elija **administrar paquetes de NuGet**.

    ![Administrar menús de paquetes de NuGet en Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. En el **Administrador de paquetes de NuGet** ventana, haga clic en **examinar**. Seleccione el **incluir versión preliminar** opción, busque el `Microsoft.Toolkit.Wpf.UI.Controls` empaquetar e instalar la versión preliminar más reciente del paquete que se muestra en los resultados.

    > [!NOTE]
    > Este paquete contiene toda la infraestructura necesaria para hospedar islas de XAML para UWP en una aplicación WPF, incluida la **InkCanvas** ajusta el control UWP. Un paquete similar denominado `Microsoft.Toolkit.Forms.UI.Controls` está disponible para las aplicaciones de Windows Forms.

3. Haga clic en **ContosoExpenses.Core** proyecto **el Explorador de soluciones** y elija **Agregar -> nuevo elemento**.

4. Seleccione **el archivo de manifiesto de aplicación**, asígnele el nombre **app.manifest**y haga clic en **agregar**.

5. En el archivo de manifiesto abierto, busque el **compatibilidad** sección e identificar la siguiente entrada comentada.

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. Debajo de esta entrada, agregue el siguiente elemento.

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. Quite el **supportedOS** entrada para Windows 10. En esta sección ahora debería parecerse a esto.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > Esta entrada especifica que la aplicación requiere Windows 10, versión 1903 (compilación 18362) o una versión posterior. Se trata de la primera versión de Windows 10 que admita islas de XAML. Sin esta entrada del manifiesto de aplicación, la aplicación iniciará una excepción en tiempo de ejecución.

8. En el archivo de manifiesto, busque los siguientes comentados **aplicación** sección.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. Eliminar esta sección y reemplazarlo con el siguiente código XML. Esto configura la aplicación para que sea PPP identificador mejor y tenga en cuenta distintos factores de escala compatibles con Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. Guarde y cierre el `app.manifest` archivo.

12. En **el Explorador de soluciones**, haga clic en el **ContosoExpenses.Core** proyecto y elija **propiedades**.

13. En el **recursos** sección de la **aplicación** pestaña, asegúrese de que el **manifiesto** desplegable se establece en **app.manifest**.

    ![Manifiesto de la aplicación .NET core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. Guarde los cambios en las propiedades del proyecto.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Agregar un control InkCanvas a la aplicación

Ahora que ha configurado el proyecto para usar islas de XAML para UWP, ahora está listo para agregar un [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) ajusta el control UWP a la aplicación.

1. En **el Explorador de soluciones**, expanda el **vistas** carpeta de la **ContosoExpenses.Core** del proyecto y haga doble clic en el **ExpenseDetail.xaml**archivo.

2. En el **ventana** elemento cerca de la parte superior del archivo XAML, agregue el siguiente atributo. Esto hace referencia el espacio de nombres XAML para el [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) ajusta el control UWP.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Después de agregar este atributo, el **ventana** elemento ahora debería parecerse a esto.

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. En el **ExpenseDetail.xaml** de archivos, busque el cierre `</Grid>` etiqueta que precede inmediatamente a la `<!-- Chart -->` comentario. Agregue el XAML siguiente justo antes de cerrar `</Grid>` etiqueta. Este XAML agrega un **InkCanvas** control (precedida por el **toolkit** palabra clave que se definió anteriormente como un espacio de nombres) y una sencilla **TextBlock** que actúa como un encabezado para el control.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Guardar el **ExpenseDetail.xaml** archivo.

6. Presione F5 para ejecutar la aplicación en el depurador.

7. Elija a un empleado en la lista y, a continuación, elija uno de los gastos disponibles. Tenga en cuenta que la página de detalles de gastos contiene espacio para el **InkCanvas** control.

    ![Lápiz lienzo solo](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Si tiene un dispositivo que es compatible con un lápiz digital, como una superficie, y ejecuta esta práctica en una máquina física, se encienden e intenta utilizarla. Verá la entrada de lápiz digital que aparecen en la pantalla. Sin embargo, si no tiene un dispositivo compatible con lápiz y vuelva a iniciar sesión con el mouse, no sucederá nada. Esto sucede porque el **InkCanvas** control está habilitado solo para lápices digitales de forma predeterminada. Sin embargo, es posible cambiar este comportamiento.

8. Cierre la aplicación y haga doble clic en el **ExpenseDetail.xaml.cs** archivo bajo el **vistas** carpeta de la **ContosoExpenses.Core** proyecto.

9. Agregue la siguiente declaración de espacio de nombres en la parte superior de la clase:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Busque el `ExpenseDetail()` constructor.

11. Agregue la siguiente línea de código justo después de la `InitializeComponent()` método y guarde el archivo de código.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Puede usar el **InkPresenter** objeto para personalizar la experiencia de tinta de predeterminada. Este código usa el **InputDeviceTypes** propiedad para habilitar el mouse, así como de entrada de lápiz.

12. Presione F5 de nuevo para volver a generar y ejecutar la aplicación en el depurador. Elija a un empleado en la lista y, a continuación, elija uno de los gastos disponibles.

13. Pruebe ahora dibujar algo en el espacio de firma con el mouse. Esta vez, verá la tinta que aparecen en la pantalla.

    ![Firma](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha agregado correctamente una UWP **InkCanvas** control a la aplicación de gastos de Contoso. Ahora está listo para [parte 3: Agregar un control de UWP CalendarView mediante XAML Islas](modernize-wpf-tutorial-3.md).

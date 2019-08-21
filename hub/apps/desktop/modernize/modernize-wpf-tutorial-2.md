---
description: En este tutorial se muestra cómo agregar interfaces de usuario XAML de UWP, crear paquetes de MSIX e incorporar otros componentes modernos en la aplicación WPF.
title: Incorporación de un control InkCanvas en UWP mediante islas XAML
ms.topic: article
ms.date: 08/15/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fb7bb6d4e5af8992571f9740c1321e271b2e1672
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643424"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Parte 2: Incorporación de un control InkCanvas en UWP mediante islas XAML

Esta es la segunda parte de un tutorial que muestra cómo modernizar una aplicación de escritorio WPF de ejemplo denominada gastos de contoso. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de [ejemplo, vea Tutorial: Modernizar una aplicación](modernize-wpf-tutorial.md)de WPF. En este artículo se da por supuesto que ya ha completado la [parte 1](modernize-wpf-tutorial-1.md).

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso desea agregar compatibilidad con firmas digitales a la aplicación de gastos de contoso. El control **InkCanvas** de UWP es una opción excelente para este escenario, ya que admite características de entrada de lápiz digital y tecnología de AI, como la capacidad para reconocer texto y formas. Para ello, usará el control UWP ajustado de [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) disponible en el kit de herramientas de la comunidad de Windows. Este control ajusta la interfaz y la funcionalidad del control **InkCanvas** de UWP para su uso en una aplicación de WPF. Para obtener más información sobre los controles de UWP ajustados, consulta [hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurar el proyecto para usar islas XAML

Antes de poder agregar un control **InkCanvas** a la aplicación de gastos de Contoso, primero debe configurar el proyecto para que admita islas XAML de UWP.

1. En Visual Studio 2019, haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** en **Explorador de soluciones** y elija **administrar paquetes NuGet**.

    ![Menú administrar paquetes NuGet en Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. En la ventana **Administrador de paquetes NuGet** , haga clic en **examinar**. Seleccione la opción **incluir versión** preliminar, busque el `Microsoft.Toolkit.Wpf.UI.Controls` paquete e instale la versión preliminar más reciente del paquete que se muestra en los resultados. Asegúrese de instalar la versión 6.0.0-preview7 o una versión posterior.

    > [!NOTE]
    > Este paquete contiene toda la infraestructura necesaria para hospedar islas XAML de UWP en una aplicación WPF, incluido el control de UWP ajustado por **InkCanvas** . Un paquete similar denominado `Microsoft.Toolkit.Forms.UI.Controls` está disponible para Windows Forms aplicaciones.

3. Haga clic con el botón secundario en el proyecto **ContosoExpenses. Core** en **Explorador de soluciones** y elija **Agregar > nuevo elemento**.

4. Seleccione **archivo de manifiesto de aplicación**, asígnele el nombre **app. manifest**y haga clic en **Agregar**. Para obtener más información sobre los manifiestos de aplicación, vea [este artículo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests).

5. En el archivo de manifiesto, quite el comentario `<supportedOS>` del siguiente elemento para Windows 10.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. En el archivo de manifiesto, busque el siguiente `<application>` elemento comentado.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Elimine esta sección y reemplácela por el siguiente código XML. De esta forma, se configura la aplicación para que sea compatible con PPP y se controle mejor factores de escala diferentes compatibles con Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Guarde y cierre el `app.manifest` archivo.

9. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **propiedades**.

10. En la sección **recursos** de la pestaña **aplicación** , asegúrese de que la lista desplegable **manifiesto** está establecida en **app. manifest**.

    ![Manifiesto de aplicación de .NET Core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Guarde los cambios en las propiedades del proyecto.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Adición de un control InkCanvas a la aplicación

Ahora que ha configurado el proyecto para que use las islas XAML de UWP, ahora está listo para agregar un control de UWP ajustado por [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) a la aplicación.

1. En **Explorador de soluciones**, expanda la carpeta views del proyecto **ContosoExpenses. Core** y haga doble clic en el archivo **ExpenseDetail. Xaml** .

2. En el elemento **Window** situado cerca de la parte superior del archivo XAML, agregue el siguiente atributo. Esto hace referencia al espacio de nombres XAML para el control UWP ajustado por [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) .

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Después de agregar este atributo, el elemento de **ventana** debe tener el siguiente aspecto.

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

4. En el archivo **ExpenseDetail. Xaml** , busque la etiqueta `</Grid>` de cierre que precede inmediatamente `<!-- Chart -->` al comentario. Agregue el código XAML siguiente justo delante de `</Grid>` la etiqueta de cierre. Este código XAML agrega un control **InkCanvas** (precedido por la palabra clave **Toolkit** que definió anteriormente como un espacio de nombres) y un **TextBlock** simple que actúa como encabezado del control.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Guarde el archivo **ExpenseDetail. Xaml** .

6. Presione F5 para ejecutar la aplicación en el depurador.

7. Elija un empleado en la lista y, a continuación, elija uno de los gastos disponibles. Observe que la página de detalles de gastos contiene el espacio para el control **InkCanvas** .

    ![Solo lápiz de lienzo de tinta](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Si tiene un dispositivo que admite un lápiz digital, como una superficie, y está ejecutando este laboratorio en una máquina física, continúe e intente usarlo. Verá que la entrada de lápiz digital aparece en la pantalla. Sin embargo, si no tiene un dispositivo compatible con el lápiz y intenta iniciar sesión con el mouse, no ocurrirá nada. Esto sucede porque el control **InkCanvas** solo está habilitado para los lápices digitales de forma predeterminada. Sin embargo, podemos cambiar este comportamiento.

8. Cierre la aplicación y haga doble clic en el archivo **ExpenseDetail.Xaml.CS** en la carpeta views del proyecto **ContosoExpenses. Core** .

9. Agregue la siguiente declaración de espacio de nombres en la parte superior de la clase:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Busque el `ExpenseDetail()` constructor.

11. Agregue la siguiente línea de código después del `InitializeComponent()` método y guarde el archivo de código.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Puede usar el objeto **InkPresenter** para personalizar la experiencia de entrada manuscrita predeterminada. Este código usa la propiedad **InputDeviceTypes** para habilitar el mouse, así como la entrada manuscrita.

12. Vuelva a presionar F5 para volver a compilar y ejecutar la aplicación en el depurador. Elija un empleado en la lista y, a continuación, elija uno de los gastos disponibles.

13. Intente dibujar ahora algo en el espacio de la firma con el mouse. Esta vez, verá que la tinta aparece en la pantalla.

    ![Firma](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha agregado correctamente un control **InkCanvas** de UWP a la aplicación de gastos de contoso. Ya está listo para [la parte 3: Agregue un control CalendarView de UWP con islas](modernize-wpf-tutorial-3.md)XAML.

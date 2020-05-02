---
description: En este tutorial se muestra cómo agregar interfaces de usuario de XAML en UWP, crear paquetes MSIX e incorporar otros componentes actuales en la aplicación de WPF.
title: Incorporación de un control InkCanvas en UWP mediante islas XAML
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6bb90fb9cbe7c9f54f60fd1920f0e73e174a3772
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80482585"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Parte 2: Incorporación de un control InkCanvas en UWP mediante islas XAML

Esta es la segunda parte de un tutorial en el que se muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso Expenses. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, consulta [Tutorial: modernizar una aplicación de WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya has completado la [parte 1](modernize-wpf-tutorial-1.md).

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso desea agregar compatibilidad con firmas digitales a la aplicación Contoso Expenses. El control **InkCanvas** de UWP es una opción excelente para este escenario, ya que admite características de entrada de lápiz digital y con tecnología de AI, como la capacidad para reconocer texto y formas. Para ello, usarás el control [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado de UWP disponible en el kit de herramientas de la comunidad de Windows. Este control encapsula la interfaz y la funcionalidad del control **InkCanvas** encapsulado de UWP para su uso en una aplicación de WPF. Para obtener más información sobre los controles encapsulados de UWP, consulta [Hospedaje de controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configuración del proyecto para usar islas XAML

Antes de poder agregar un control **InkCanvas** a la aplicación Contoso Expenses, primero debes configurar el proyecto para que admita islas XAML de UWP.

1. En Visual Studio 2019, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** en el **Explorador de soluciones** y elige **Administrar paquetes NuGet**.

    ![Menú Administrar paquetes NuGet en Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. En la ventana **Administrador de paquetes NuGet**, haz clic en **Examinar**. Busca el paquete `Microsoft.Toolkit.Wpf.UI.Controls` e instala la versión 6.0.0 o una versión posterior.

    > [!NOTE]
    > Este paquete contiene toda la infraestructura necesaria para hospedar islas XAML de UWP en una aplicación de WPF, incluido el control **InkCanvas** encapsulado de UWP. Un paquete similar denominado `Microsoft.Toolkit.Forms.UI.Controls` está disponible para las aplicaciones de Windows Forms.

3. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** en el **Explorador de soluciones** y elige **Agregar -> Nuevo elemento**.

4. Selecciona **Archivo de manifiesto de aplicación**, asígnale el nombre **app.manifest** y haz clic en **Agregar**. Para obtener más información sobre los manifiestos de aplicación, consulta [este artículo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests).

5. En el archivo de manifiesto, quita la marca de comentario del siguiente elemento `<supportedOS>` para Windows 10.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. En el archivo de manifiesto, busca el siguiente elemento `<application>` comentado.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Elimina esta sección y sustitúyela por el siguiente código XML. De esta forma, se configura la aplicación para que sea compatible con PPP y controle mejor factores de escalado diferentes compatibles con Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Guarda y cierra el archivo `app.manifest`.

9. En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Propiedades**.

10. En la sección **Recursos** de la pestaña **Aplicación**, asegúrate de que el menú desplegable **Manifiesto** está establecido en **app.manifest**.

    ![Manifiesto de la aplicación .NET Core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Guarda los cambios en las propiedades del proyecto.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Adición de un control InkCanvas a la aplicación

Ahora que has configurado el proyecto para que use islas XAML de UWP, ya puedes agregar un control encapsulado de UWP [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) a la aplicación.

1. En el **Explorador de soluciones**, expande la carpeta **Vistas** del proyecto **ContosoExpenses.Core** y haz doble clic en el archivo **ExpenseDetail.xaml**.

2. En el elemento **Ventana** situado cerca de la parte superior del archivo XAML, agrega el atributo siguiente. Esto hace referencia al espacio de nombres de XAML para el control encapsulado de UWP [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas).

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Después de agregar este atributo, el elemento **Ventana** debe tener ahora un aspecto como este.

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

4. En el archivo **ExpenseDetail.xaml**, busca la etiqueta de cierre `</Grid>` que precede inmediatamente al comentario `<!-- Chart -->`. Agrega el siguiente código XAML justo delante de la etiqueta de cierre `</Grid>`. Este código XAML agrega un control **InkCanvas** (precedido por la palabra clave **toolkit** definida anteriormente como un espacio de nombres) y una clase **TextBlock** sencilla que actúa como encabezado del control.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Guarda el archivo **ExpenseDetail.xaml**.

6. Presiona F5 para ejecutar la aplicación en el depurador.

7. Elige un empleado en la lista y, a continuación, selecciona alguno de los gastos disponibles. Observa que la página de detalles de gastos contiene espacio para el control **InkCanvas**.

    ![InkCanvas solo para lápiz](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Si tienes un dispositivo que admite un lápiz digital, como Surface, y ejecutas este laboratorio en un equipo físico, continúa e intenta usarlo. Verás que la entrada de lápiz digital aparece en la pantalla. Sin embargo, si no tienes un dispositivo compatible con el lápiz e intentas iniciar sesión con el mouse, no ocurrirá nada. Esto sucede porque el control **InkCanvas** está habilitado solo para los lápices digitales de forma predeterminada. Sin embargo, podemos cambiar este comportamiento.

8. Cierra la aplicación y haz doble clic en el archivo **ExpenseDetail.xaml.cs** en la carpeta **Vistas** del proyecto **ContosoExpenses.Core**.

9. Agrega la siguiente declaración de espacio de nombres en la parte superior de la clase:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Busca el constructor `ExpenseDetail()`.

11. Agrega la siguiente línea de código después del método `InitializeComponent()` y guarda el archivo de código.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Puedes usar el objeto **InkPresenter** para personalizar la experiencia de entrada manuscrita predeterminada. Este código usa la propiedad **InputDeviceTypes** para habilitar el mouse, así como la entrada manuscrita.

12. Vuelve a presionar F5 para volver a compilar y ejecutar la aplicación en el depurador. Elige un empleado en la lista y, a continuación, selecciona alguno de los gastos disponibles.

13. Ahora intenta dibujar algo en el espacio de la firma con el mouse. Esta vez, verás que la entrada de lápiz aparece en la pantalla.

    ![Firma](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, has agregado correctamente un control encapsulado de UWP **InkCanvas** a la aplicación Contoso Expenses. Ahora ya estás listo para continuar con la [Parte 3: Incorporación de un control CalendarView en UWP mediante islas XAML](modernize-wpf-tutorial-3.md).

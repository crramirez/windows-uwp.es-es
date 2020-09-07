---
title: Formularios
description: Obtenga información sobre las directrices para diseñar y crear diseños XAML para formularios en una aplicación para la Plataforma universal de Windows (UWP).
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 0113cbf50601a3db717753ab2e12524fa281daba
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304697"
---
# <a name="forms"></a>Formularios
Un formulario es un grupo de controles que recopila y envía datos de los usuarios. Los formularios se usan normalmente para páginas de configuración, encuestas, creación de cuentas y mucho más. 

Este artículo contiene instrucciones para el diseño XAML de formularios.

![Ejemplo de formulario](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>¿Cuándo deberías usar un formulario?
Un formulario es una página dedicada para recopilar entradas de datos que están claramente relacionados entre sí. Los formularios deben utilizarse para recopilar explícitamente los datos de un usuario. Puedes crear un formulario para que un usuario:
- Inicie sesión en una cuenta.
- Se registre para obtener una cuenta.
- Cambie la configuración de la aplicación, por ejemplo, las opciones de privacidad o visualización.
- Realice una encuesta.
- Compre un artículo.
- Comentarios

## <a name="types-of-forms"></a>Tipos de formularios

Al pensar en cómo se envían y se muestran los datos especificados por el usuario, hay dos tipos de formularios:

### <a name="1-instantly-updating"></a>1. Actualización instantánea.
![página de configuración](images/control-examples/toggle-switch-news.png)

Usa un formulario de actualización instantánea si quieres que los usuarios vean inmediatamente los resultados cuando cambien los valores en el formulario. Por ejemplo, en las páginas de configuración, se muestran las opciones seleccionadas actualmente y los cambios realizados en las opciones seleccionadas se aplican inmediatamente. Para confirmar los cambios en la aplicación, debes [agregar un controlador de eventos](controls-and-events-intro.md) a cada control de entrada. Si un usuario cambia un control de entrada, la aplicación puede responder de forma adecuada.

### <a name="2-submitting-with-button"></a>2. Envío con un botón
El otro tipo de formulario permite al usuario elegir cuándo se deben enviar los datos con un clic de un botón.

![calendario agregar nueva página de eventos](images/calendar-form.png)

Este tipo de formulario ofrece flexibilidad al usuario para responder. Normalmente, este tipo de formulario contiene campos de entrada de formulario más libres y, por lo tanto, recibe una gran variedad de respuestas. Para asegurarse de que la entrada del usuario es válida y los datos tienen el formato correcto en el momento del envío, ten en cuenta las siguientes recomendaciones:

- Utiliza el control correcto para impedir que se envíe información no válida (es decir, usa CalendarDatePicker en lugar de TextBox para las fechas del calendario). En la sección Controles de entrada más adelante encontrarás información sobre cómo seleccionar los controles de entrada apropiados para un formulario.
- Cuando uses controles TextBox, proporciona a los usuarios una indicación del formato de entrada deseado con la propiedad [PlaceholderText](/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText).
- Indica la entrada que se espera en un control con la propiedad [InputScope](/uwp/api/windows.ui.xaml.input.inputscope) para proporcionar a los usuarios el teclado en pantalla apropiado.
- Marca las entradas obligatorias con un asterisco * en la etiqueta.
- Deshabilita el botón Enviar hasta que se rellene toda la información obligatoria.
- Si hay datos no válidos en el momento del envío, resalta los campos o los bordes de los controles que contengan una entrada no válida y pide al usuario que vuelva a enviar el formulario.
- Para otros errores, como errores de conexión de red, asegúrate de mostrar un mensaje de error adecuado al usuario. 


## <a name="layout"></a>Diseño

Para facilitar la experiencia del usuario y garantizar que los usuarios puedan introducir la entrada correcta, ten en cuenta las siguientes recomendaciones para diseñar formularios. 

### <a name="labels"></a>Etiquetas
Las [etiquetas](labels.md) debe alinearse a la izquierda y colocarse encima del control de entrada. Muchos controles tienen una propiedad Header integrada que sirve para mostrar la etiqueta. En los controles sin propiedad Header o para etiquetar grupos de controles, puedes usar [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) en su lugar.

Para crear [diseños con accesibilidad](../accessibility/accessibility.md), etiqueta los controles individuales y los grupos de controles para mayor claridad tanto para humanos como para los lectores de pantalla. 

Para los estilos de fuente, usa la [rampa de tipos de Windows](../style/typography.md) predeterminada. Usa `TitleTextBlockStyle` para los títulos de página, `SubtitleTextBlockStyle` para los encabezados de grupo y `BodyTextBlockStyle` para las etiquetas de control.

<div class="mx-responsive-img">
<table>
<tr><th>Cosas que hacer</th><th>Cosas que evitar</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>Espaciado
Para separar visualmente los grupos de controles entre sí, usa [alineación, márgenes y relleno](../layout/alignment-margin-padding.md). Los controles de entrada individuales tienen 80 píxeles de alto y deben tener una distancia de 24 píxeles entre sí. Los grupos de controles de entrada deben tener una distancia de 48 píxeles entre sí.

![grupos de formularios](images/forms-groups.png)

### <a name="columns"></a>Columnas
La creación de columnas puede reducir el espacio en blanco innecesario en formularios, especialmente en tamaños de pantalla más grandes. Sin embargo, si quieres crear un formulario de varias columnas, el número de columnas dependerá del número de controles de entrada en la página y del tamaño de pantalla de la ventana de la aplicación. En lugar de sobrecargar la pantalla con numerosos controles de entrada, considera la posibilidad de crear varias páginas para el formulario.  

<div class="mx-responsive-img">
<table>
<tr><th>Cosas que hacer</th><th>Cosas que evitar</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>Diseño dinámico
Los formularios deben cambiar de tamaño cuando cambie el tamaño de la pantalla o la ventana, para que los usuarios no pasen por alto ningún campo de entrada. Para más información, consulta las [técnicas de diseño dinámico](../layout/responsive-design.md). Por ejemplo, tal vez quieras dejar regiones específicas del formulario siempre a la vista, independientemente del tamaño de pantalla.

![foco en los formularios](images/forms-focus2.png)

### <a name="tab-stops"></a>Puntos de tabulación
Los usuarios pueden usar el teclado para navegar por los controles con los [puntos de tabulación](../input/keyboard-interactions.md#tab-stops). De forma predeterminada, el orden de tabulación de los controles refleja el orden con que se crean en XAML. Para invalidar el comportamiento predeterminado, cambia las propiedades **IsTabStop** o **TabIndex** del control. 

![foco de tabulación en un control de formulario](images/forms-focus1.png)

## <a name="input-controls"></a>Controles de entrada
Los controles de entrada son los elementos de la interfaz de usuario que permiten a los usuarios escribir información en formularios. A continuación se muestran algunos controles comunes que pueden agregarse a los formularios, así como información sobre cuándo utilizarlos.

### <a name="text-input"></a>Entrada de texto
Control | Use | Ejemplo
 - | - | -
[TextBox](text-box.md) | Capturar una o varias líneas de texto. | Nombres, respuestas de forma libre o comentarios.
[PasswordBox](password-box.md) | Recopilar datos privados ocultando los caracteres. | Contraseñas, números del seguro social, PIN, información de tarjetas de crédito. 
[AutoSuggestBox](auto-suggest-box.md) | Mostrar a los usuarios una lista de sugerencias de un conjunto de datos correspondiente a medida que escriben. | Búsqueda en bases de datos, línea mailto:, consultas anteriores.
[RichEditBox](rich-edit-box.md) | Editar archivos de texto con texto con formato, hipervínculos e imágenes. | Cargar archivos, mostrar una vista previa y editar en la aplicación.

### <a name="selection"></a>Selección
Control | Use | Ejemplo
- | - | - 
| [CheckBox](checkbox.md) | Seleccionar o anular la selección de uno o más elementos de acción. | Aceptar los términos y condiciones, agregar elementos opcionales, seleccionar todas las opciones aplicables.
[RadioButton](radio-button.md) | Seleccionar una opción entre dos o más opciones. | Elegir tipos, métodos de envío, etc.
[ToggleSwitch](toggles.md) | Elegir una de dos opciones mutuamente excluyentes. | Activar/desactivar.

> **Nota**: Si hay cinco o más elementos para seleccionar, utiliza un control de lista.

### <a name="lists"></a>Listas
Control | Use | Ejemplo
- | - | -
[ComboBox](combo-box.md) | Iniciar con un formato compacto y expandir para mostrar la lista de elementos seleccionables. | Seleccionar en una lista larga de elementos, como estados o países.
[ListView](./lists.md#list-views) | Clasificar elementos y asignar encabezados de grupo, arrastrar y colocar elementos, mantener el contenido y reordenar los elementos. | Opciones de clasificación.
[GridView](./lists.md#grid-views) | Organizar y examinar colecciones de imágenes. | Elegir una foto o un color, mostrar un tema.

### <a name="numeric-input"></a>Entradas numéricas
Control | Use | Ejemplo
- | - | -
[Control deslizante](slider.md) | Seleccionar un número de un intervalo de valores numéricos contiguos. | Porcentajes, volumen, velocidad de reproducción.
[Rating](rating.md) | Calificar con estrellas. | Comentarios de los clientes

### <a name="date-and-time"></a>Fecha y hora

Control | Use 
- | - 
[CalendarView](calendar-view.md) | Selecciona una fecha determinada o un intervalo de fechas de un calendario siempre visible. 
[CalendarDatePicker](calendar-date-picker.md) | Selecciona una fecha determinada de un calendario contextual. 
[DatePicker](date-picker.md) | Seleccionar una sola fecha cuando la información contextual no es importante.
[TimePicker](time-picker.md) | Seleccionar un único valor de hora.

### <a name="additional-controls"></a>Controles adicionales 
Para obtener una lista completa de controles UWP, consulta el [índice de controles por función](controls-by-function.md).

Para ver otros controles de interfaz de usuario más complejos y personalizados, consulta los recursos disponibles en empresas como [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/uwp-ui-controls), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP) y [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Ejemplo de formulario de una columna
En este ejemplo se usa una [vista de lista](lists.md) [maestro/detalles](master-details.md) Acrylic y un control [NavigationView](navigationview.md).
![Captura de pantalla de otro ejemplo de formulario](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>Ejemplo de formulario de dos columnas
Este ejemplo se usa el control [Pivot](pivot.md), un fondo [Acrylic](../style/acrylic.md) y el control [CommandBar](app-bars.md), además de los controles de entrada.
![Captura de pantalla del ejemplo de formulario](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>Ejemplo de base de datos de pedidos de clientes
![captura de pantalla de base de datos de pedidos de clientes](images/customerorderform.png) Para obtener información sobre cómo conectar la entrada de formulario con una base de datos de **Azure** y ver un formulario totalmente implementado, consulta el ejemplo de aplicación [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database).

## <a name="related-topics"></a>Temas relacionados
- [Controles de entrada](controls-and-events-intro.md)
- [Tipografía](../style/typography.md)
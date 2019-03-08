---
Description: Directrices de diseño de los formularios de aplicaciones para UWP.
title: Formularios
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658160"
---
# <a name="forms"></a>Formularios
Un formulario es un grupo de controles que recopilar y enviar datos de los usuarios. Formularios se usan normalmente para páginas de configuración, encuestas, creación de cuentas y mucho más. 

Este artículo describen las instrucciones de diseño para crear diseños XAML para los formularios.

![Ejemplo de formulario](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>¿Cuándo se deben usar un formulario?
Un formulario es una página dedicada para recopilar entradas de datos que son claramente relacionadas entre sí. Debe usar un formulario cuando necesite recopilar explícitamente los datos de un usuario. Puede crear un formulario para que un usuario:
- Inicie sesión en una cuenta
- Regístrese para obtener una cuenta
- Cambiar la configuración de la aplicación, por ejemplo, la privacidad o mostrar opciones
- Realice una encuesta
- Comprar un artículo
- Enviar comentarios

## <a name="types-of-forms"></a>Tipos de formularios

Al pensar en cómo se envían y se muestra proporcionados por el usuario, hay dos tipos de formularios:

### <a name="1-instantly-updating"></a>1. Actualizar al instante
![Página de configuración](images/control-examples/toggle-switch-news.png)

Usar un formulario de actualización al instante cuando desee que los usuarios a ver inmediatamente los resultados de cambiar los valores en el formulario. Por ejemplo, en las páginas de configuración, se muestran las selecciones actuales y los cambios realizados en las selecciones se aplican inmediatamente. Para confirmar los cambios en la aplicación, deberá [agregar un controlador de eventos](controls-and-events-intro.md) para cada control de entrada. Si un usuario cambia un control de entrada, la aplicación puede responder de forma adecuada.

### <a name="2-submitting-with-button"></a>2. Enviar con el botón
El otro tipo de formulario permite al usuario elegir cuándo se debe enviar los datos con un clic de un botón.

![calendario Agregar nueva página de eventos](images/calendar-form.png)

Este tipo de formulario ofrece la flexibilidad de usuario en responder. Normalmente, este tipo de formulario contiene campos de entrada de forma libre más y, por tanto, recibe una gran variedad de respuestas. Para asegurarse de entrada de usuario válido y datos con el formato correcto tras el envío, tenga en cuenta las siguientes recomendaciones:

- No resulta posible enviar información no válida mediante el control correcto (es decir, utilice un CalendarDatePicker en lugar de un cuadro de texto para las fechas del calendario). Obtenga más información sobre la selección de los controles de entrada correspondientes en el formulario en la sección de controles de entrada más adelante.
- Al usar controles de cuadro de texto, proporcionar a los usuarios una sugerencia del formato de entrada deseado con el [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) propiedad.
- Proporcionar a los usuarios con el teclado en pantalla, que indica que la entrada esperada de un control con el [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) propiedad.
- Mark requiere la entrada con un asterisco * en la etiqueta.
- Deshabilitar el botón Enviar hasta que se rellena toda la información necesaria.
- Si no hay datos no válidos tras el envío, marcar los controles de entrada no válida con los campos resaltados o bordes y exigir al usuario que vuelva a enviar el formulario.
- Para otros errores, como la conexión de red con errores, asegúrese de mostrar un mensaje de error adecuado al usuario. 


## <a name="layout"></a>Diseño

Para facilitar la experiencia del usuario y garantizar que los usuarios pueden introducir la entrada correcta, tenga en cuenta las siguientes recomendaciones para diseñar diseños de formularios. 

### <a name="labels"></a>Etiquetas
[Las etiquetas](labels.md) debe alineado a la izquierda y se coloca encima del control de entrada. Muchos controles tienen una propiedad de encabezado integrada para mostrar la etiqueta. En los controles sin propiedad Header o para etiquetar grupos de controles, puedes usar un [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock) en su lugar.

Para [diseño para accesibilidad](../accessibility/accessibility.md), etiquete todas individuales y grupos de controles para mayor claridad para ambos humanos y los lectores de pantalla. 

Para los estilos de fuente, use el valor predeterminado [rampa de tipos UWP](../style/typography.md). Use `TitleTextBlockStyle` para títulos de página, `SubtitleTextBlockStyle` para los encabezados de grupo, y `BodyTextBlockStyle` para las etiquetas de control.

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
Para separar visualmente los grupos de controles entre sí, use [alineación, márgenes y relleno](../layout/alignment-margin-padding.md). Controles de entrada individuales 80px en alto y deben 24 px espaciados de diferencia. Grupos de controles de entrada deben ser 48px con espacios separados.

![grupos de formularios](images/forms-groups.png)

### <a name="columns"></a>Columnas
Creación de columnas puede reducir el espacio en blanco innecesario en formularios, especialmente con tamaños de pantalla más grandes. Sin embargo, si desea crear un formulario de varias columna, el número de columnas debe depender en el número de controles de entrada en la página y el tamaño de pantalla de la ventana de la aplicación. En lugar de sobrecargar la pantalla con numerosos controles de entrada, considere la posibilidad de crear varias páginas para el formulario.  

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
Deben cambiar el tamaño de formularios como los cambios de tamaño de pantalla o ventana, por lo que los usuarios no olvide de los campos de entrada. Para obtener más información, consulte [técnicas de diseño dinámico](../layout/responsive-design.md). Por ejemplo, es posible que desee mantener regiones específicas del formulario siempre en la vista, independientemente del tamaño de pantalla.

![enfoque de formularios](images/forms-focus2.png)

### <a name="tab-stops"></a>Puntos de tabulación
Los usuarios pueden usar el teclado para navegar por los controles con [las tabulaciones](../input/keyboard-interactions.md#tab-stops). De forma predeterminada, el orden de tabulación de controles refleja el orden en que se crean en XAML. Para invalidar el comportamiento predeterminado, cambie la **IsTabStop** o **TabIndex** propiedades del control. 

![foco de la pestaña de control de formulario](images/forms-focus1.png)

## <a name="input-controls"></a>Controles de entrada
Los controles de entrada son los elementos de interfaz de usuario que permiten a los usuarios escribir información en formularios. Algunos controles comunes que pueden agregarse a los formularios se muestran a continuación, así como información sobre cuándo utilizarlas.

### <a name="text-input"></a>Entrada de texto
Control | Use | Ejemplo
 - | - | -
[TextBox](text-box.md) | Capturar una o varias líneas de texto | Los nombres, las respuestas de forma libre o comentarios
[PasswordBox](password-box.md) | Recopilar datos privados al ocultar los caracteres | Las contraseñas, números del seguro Social (SSN), PIN, información de tarjeta de crédito 
[AutoSuggestBox](auto-suggest-box.md) | Mostrar una lista de sugerencias de un conjunto de datos correspondiente a los usuarios a medida que escriben. | Búsqueda de la base de datos, correo a: línea, las consultas anteriores
[RichEditBox](rich-edit-box.md) | Editar archivos de texto con texto con formato, hipervínculos e imágenes | Cargar archivo, vista previa y editar en la aplicación

### <a name="selection"></a>Selección
Control | Use | Ejemplo
- | - | - 
| [CheckBox](checkbox.md) | Seleccione o anule la selección de uno o varios elementos de acción | Acepte los términos y condiciones, agregar elementos opcionales, todas las opciones aplicables seleccione
[RadioButton](radio-button.md) | Seleccione una opción entre dos o más opciones | Elegir el tipo, método, etcetera de envío.
[ToggleSwitch](toggles.md) | Elija una de las dos opciones mutuamente excluyentes | Activar/desactivar

> **Nota**: Si hay cinco o más elementos de selección, utilice un control de lista.

### <a name="lists"></a>Listas
Control | Use | Ejemplo
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Iniciar en estado compact y se expanden para mostrar la lista de elementos seleccionables | Seleccionar de una larga lista de elementos, como los Estados o países
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Clasificar elementos y asignar los encabezados de grupo, arrastre y coloque elementos, ajustar el contenido y reordenar los elementos | Opciones de rango
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Organizar y examinar las colecciones basadas en imágenes | Tome una foto, color, para mostrar el tema

### <a name="numeric-input"></a>Entradas numéricas
Control | Use | Ejemplo
- | - | -
[Control deslizante](slider.md) | Seleccione un número de un intervalo de valores numéricos no contiguos | Porcentajes, volumen, velocidad de reproducción
[Clasificación](rating.md) | Velocidad con estrellas | Comentarios del cliente

### <a name="date-and-time"></a>Fecha y hora

Control | Use 
- | - 
[CalendarView](calendar-view.md) | Elegir una sola fecha o un intervalo de fechas en un calendario siempre visible 
[CalendarDatePicker](calendar-date-picker.md) | Elegir una sola fecha en un calendario contextual 
[DatePicker](date-picker.md) | Elegir una sola fecha localizada cuando no es importante la información contextual
[TimePicker](time-picker.md) | Seleccionar un único valor

### <a name="additional-controls"></a>Controles adicionales 
Para obtener una lista completa de los controles UWP, consulte [índice de los controles por función](controls-by-function.md).

Para los controles de interfaz de usuario más complejos y personalizados, examine los recursos UWP disponibles de compañías como [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/products/uwp), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [ Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP), y [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Ejemplo de formulario de una columna
Este ejemplo usa un acrílico [principal-detalle](master-details.md) [vista de lista](lists.md) y [NavigationView](navigationview.md) control.
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
Este ejemplo se usa el [Pivot](pivot.md) control, [acrílico](../style/acrylic.md) en segundo plano, y [CommandBar](app-bars.md) además de los controles de entrada.
![Captura de pantalla de ejemplo de formulario](images/FormExample.png)
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

## <a name="customer-orders-database-sample"></a>Ejemplo de base de datos de pedidos de cliente
![captura de pantalla de base de datos de pedidos de clientes](images/customerorderform.png) para obtener información sobre cómo conectar la entrada de formulario a un **Azure** de base de datos y ver un formulario implementado totalmente, vea el [base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) ejemplo de aplicación.

## <a name="related-topics"></a>Temas relacionados
- [Controles de entrada](controls-and-events-intro.md)
- [Tipografía](../style/typography.md)

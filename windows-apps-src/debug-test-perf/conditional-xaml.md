---
author: jwmsft
title: XAML condicional
description: Usa las nuevas API en marcado XAML mientras mantienes la compatibilidad con versiones anteriores.
ms.author: jimwalk
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b6fad82b8610367f5a69b236c1783106454374f3
ms.sourcegitcommit: 73ea31d42a9b352af38b5eb5d3c06504b50f6754
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2017
---
# <a name="conditional-xaml"></a>XAML condicional

*El XAML condicional* proporciona una forma de usar el método [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsApiContractPresent_System_String_System_UInt16_) en el marcado XAML. Esto te permite establecer las propiedades y crear instancias de objetos en el marcado en función de la presencia de una API, sin tener que usar código subyacente. Analiza selectivamente elementos o atributos para determinar si estarán disponibles en el tiempo de ejecución. Las instrucciones condicionales se evalúan en el tiempo de ejecución, y los elementos calificados una etiqueta de XAML condicional se analizan si se evalúan como **verdadero**; de lo contrario, se ignoran.

El XAML condicional está disponible a partir de la actualización de Creators Update (versión 1703, compilación 15063). Para usar XAML condicional, la versión mínima de tu proyecto de Visual Studio debe establecerse en la compilación 15063 (Creators Update) o posterior, y configurar la versión de destino en una versión posterior a la versión mínima. Consulta [Aplicaciones adaptables para versiones](version-adaptive-apps.md) para obtener más información acerca de cómo configurar el proyecto de Visual Studio.

> [!IMPORTANT]
> **VERSIÓN PREVIA | Requiere la actualización Fall Creators Update**: Debes elegir el [SDK 16225 de Insider](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) y estar ejecutando la [Compilación 16226 de Insider](https://blogs.windows.com/windowsexperience/2017/06/21/announcing-windows-10-insider-preview-build-16226-pc/) o posterior para poder usar la XAML condicional.

> [!NOTE]
> Para crear una versión de la aplicación adaptable con una versión mínima menor que la de la compilación 15063, debes usar [código adaptativo para versiones](version-adaptive-code.md) y no XAML.

Para obtener más información general acerca de ApiInformation y contratos de API, consulta [Aplicaciones adaptables para versiones](version-adaptive-apps.md).

## <a name="conditional-namespaces"></a>Espacios de nombres condicionales

Para usar una instrucción condicional en XAML, primero debes declarar un [espacio de nombres XAML](../xaml-platform/xaml-namespaces-and-namespace-mapping.md) condicional en la parte superior de la página. Este es un ejemplo de seudocódigo de un espacio de nombres condicional:

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

Un espacio de nombres condicional se puede dividir en dos partes separadas por el delimitador '?'. El contenido que precede al delimitador indica el espacio de nombres o el esquema que contiene la API a la que se hace referencia. El contenido después del '?' delimitador representa el método condicional que determina si el espacio de nombres condicional se evalúa como **verdadero** o **falso**.

En la mayoría de los casos, el esquema será el espacio de nombres XAML predeterminado:

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

El XAML condicional es compatible con los siguientes métodos condicionales:

Método | Inverso
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

(Más información sobre estos métodos en las secciones siguientes).

> [!NOTE] 
> Te recomendamos que uses IsApiContractPresent y IsApiContractNotPresent. Otras elementos condicionales no son totalmente compatibles con la experiencia de diseño de Visual Studio.

## <a name="create-a-namespace-and-set-a-property"></a>Crear un nuevo espacio de nombres y establecer una propiedad

En este ejemplo, se mostrará, "Hola, Windows Insider" como el contenido de un bloque de texto si la aplicación se ejecuta en Windows Insider Preview (Fall Creators Update) y sin contenido alguna de forma predeterminada si se trata de una versión anterior.

En primer lugar, define un espacio de nombres personalizado con el prefijo 'insider' y usar el espacio de nombres XAML de forma predeterminada (http://schemas.microsoft.com/winfx/2006/xaml/presentation) como el esquema que contiene la propiedad [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock#Windows_UI_Xaml_Controls_TextBlock_Text). Para hacer que sea un espacio de nombres condicional, agrega '?'. Delimitador después del esquema.

A continuación, define una instrucción condicional que devuelve **verdadero** en dispositivos que ejecutan Windows Insider Preview o posterior. Usa el método ApiInformation **IsApiContractPresent** para probar la versión 5 de UniversalApiContract. La versión 5 de UniversalApiContract se lanzó con Windows Insider Preview (Fall Creators Update).

```xaml
xmlns:insider="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Después de definir el espacio de nombres, antepón el prefijo del espacio de nombres a la propiedad Text de tu control TextBox para que exista una propiedad que se debe establecer condicionalmente en tiempo de ejecución.

```xaml
<TextBlock insider:Text="Hello, Windows Insider"/>
```

Este el XAML completo.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:insider="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="textBlock" insider:Text="Hello, Windows Insider"/>
    </Grid>
</Page>
```

Cuando ejecutas este ejemplo en Insider Preview, se muestra el texto: "Soy un usuario de Windows Insider". Cuando se ejecuta en Creators Update, no se muestra ningún texto.

El XAML condicional te permite realizar las comprobaciones de API que puedes hacer en el código en el marcado en su lugar. Este es el código equivalente de esta comprobación.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Windows Insider";
}
```

Recuerda que aunque el método IsApiContractPresent toma una cadena el parámetro *contractName* no colocarás las comillas ("") en la declaración de espacio de nombres XAML.

## <a name="use-ifelse-conditions"></a>Usar condiciones if/else

En el ejemplo anterior, se establece la propiedad Text solo cuando la aplicación se ejecuta en Insider Preview. Pero, ¿qué ocurre si quieres mostrar texto diferente al ejecutarla en Creators Update? Podrías intentar establecer la propiedad Text sin un calificador condicional, como este.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" insider:Text="Hello, Windows Insider"/>
```

Esto funcionaría si se ejecuta en Creators Update pero si se ejecuta en Insider Preview, recibirás un mensaje de error diciendo que la propiedad Text se ha establecido más de una vez.

Para configurar texto diferente cuando la aplicación se ejecute en diferentes versiones de Windows 10, necesitas otra condición. El XAML condicinal proporciona una versión inversa de cada método ApiInformation compatible para que puedas crear escenarios condicionales con if/else como este.
 
El método IsApiContractPresent devuelve **verdadero** si el dispositivo actual contiene el número especificado de contrato y la versión. Por ejemplo, supongamos que la aplicación se ejecuta en Creators Update, que tiene la versión 4 del contrato de API universal.

Varias llamadas a IsApiContractPresent tendría estos resultados:

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true.

IsApiContractNotPresent devuelve lo contrario de IsApiContractPresent. Varias llamadas a IsApiContractNotPresent tendría estos resultados:

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

Para usar la inversa condición, crea un segundo espacio de nombres XAML condicional que use la condicional **IsApiContractNotPresent**. Aquí, tiene el prefijo 'notInsider'.

```xaml
xmlns:insider="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:notInsider="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"
```

Con los dos espacios de nombres definidos, puedes establecer la propiedad Text dos veces siempre y cuando uses en ellos el prefijo con los calificadores que aseguren que solo una configuración de propiedades se usa en el momento de ejecución, como esto:

```xaml
<TextBlock notInsider:Text="Hello, World" 
           insider:Text="Hello, Windows Insider"/>
```

Este es otro ejemplo que establece el fondo de un botón. La característica [material acrílico](../style/acrylic.md) está disponible a partir de Insider Preview (Fall Creators Update), así que usaremos Acrylic para el fondo cuando la aplicación se ejecute en Insider Preview. No está disponible en versiones anteriores, por lo que en estos casos, el fondo será de color rojo.

```xaml
<Button Content="Button"
        notInsider:Background="Red"
        insider:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>Crear controles y enlazar propiedades

Hasta ahora, has visto cómo establecer propiedades con XAML condicional, pero también puedes crear condicionalmente instancias de controles basados en el contrato de API, disponible en el tiempo de ejecución.

Aquí, un control [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) crea una instancia cuando la aplicación se ejecuta en Insider Preview, donde el control está disponible. ColorPicker no está disponible en versiones anteriores a Insider Preview, por lo que cuando la aplicación se ejecuta en estas versiones anteriores, tienes que usar un [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) para proporcionar opciones de color simplificadas para el usuario.

```xaml
<insider:ColorPicker x:Name="colorPicker" Grid.Column="1"/>

<notInsider:ComboBox x:Name="colorComboBox" PlaceholderText="Pick a color"
                     Grid.Column="1" VerticalAlignment="Center">
```

Puedes usar calificadores condicionales con distintas formas de [sintaxis de propiedad XAML ](../xaml-platform/xaml-syntax-guide.md). Aquí, la propiedad Fill del rectángulo se establece usando la sintaxis de elemento de propiedad para Insider Preview y la sintaxis de atributo para las versiones anteriores.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           notInsider:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <insider:Rectangle.Fill>
        <SolidColorBrush insider:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </insider:Rectangle.Fill>
</Rectangle>
```

Al enlazar una propiedad a otra propiedad que depende de un espacio de nombres condicional, debes usar la misma condición en ambas propiedades. Aquí, `colorPicker.Color` depende del espacio de nombres condicional del usuario de Insider, por lo que también se debe colocar el prefijo 'insider' en la propiedad SolidColorBrush.Color. (O tambiés puedes colocar el prefijo 'insider' en el SolidColorBrush en lugar de en la propiedad de Color). Si no lo haces, obtendrás un error en tiempo de compilación.

```xaml
<SolidColorBrush insider:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

Este es el XAML completo que demuestra estos escenarios. Este ejemplo contiene un rectángulo y una interfaz de usuario que te permite establecer el color del rectángulo.

Cuando la aplicación se ejecuta en Insider Preview, usa un componente ColorPicker para permitir al usuario establecer el color. ColorPicker no está disponible antes de Insider Preview, por lo que cuando la aplicación se ejecute en versiones anteriores, usan un cuadro combinado para proporcionar opciones de color simplificada al usuario.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:insider="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:notInsider="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   notInsider:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <insider:Rectangle.Fill>
                <SolidColorBrush insider:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </insider:Rectangle.Fill>
        </Rectangle>

        <insider:ColorPicker x:Name="colorPicker" Grid.Column="1"/>

        <notInsider:ComboBox x:Name="colorComboBox" PlaceholderText="Pick a color"
                     Grid.Column="1" VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </notInsider:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>Artículos relacionados

- [Guía de aplicaciones para UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contratos de API](https://channel9.msdn.com/Events/Build/2015/3-733) (Vídeo de compilación de 2015)
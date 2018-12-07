---
title: XAML condicional
description: Usa las nuevas API en marcado XAML mientras mantienes la compatibilidad con versiones anteriores.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c75a6c487fe4a7f7cb56deff869b36309a4b9c7
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8786829"
---
# <a name="conditional-xaml"></a>XAML condicional

*El XAML condicional* proporciona una forma de usar el método [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) en el marcado XAML. Esto te permite establecer las propiedades y crear instancias de objetos en el marcado en función de la presencia de una API, sin tener que usar código subyacente. Analiza selectivamente elementos o atributos para determinar si estarán disponibles en el tiempo de ejecución. Las instrucciones condicionales se evalúan en el tiempo de ejecución, y los elementos calificados con una etiqueta de XAML condicional se analizan si se evalúan como **true** (verdadero); de lo contrario, se ignoran.

El XAML condicional está disponible a partir de la actualización de Creators Update (versión 1703, compilación 15063). Para usar XAML condicional, la versión mínima de tu proyecto de Visual Studio debe establecerse en la compilación 15063 (Creators Update) o posterior, y configurar la versión de destino en una versión posterior a la versión mínima. Consulta [Aplicaciones adaptables para versiones](version-adaptive-apps.md) para obtener más información acerca de cómo configurar el proyecto de Visual Studio.

> [!NOTE]
> Para crear una versión de la aplicación adaptable con una versión mínima menor que la compilación 15063, debes usar [código adaptativo para versiones](version-adaptive-code.md) y no XAML.

Para obtener más información general acerca de ApiInformation y contratos de API, consulta [Aplicaciones adaptables para versiones](version-adaptive-apps.md).

## <a name="conditional-namespaces"></a>Espacios de nombres condicionales

Para usar un método condicional en XAML, primero debes declarar un [espacio de nombres XAML](../xaml-platform/xaml-namespaces-and-namespace-mapping.md) condicional en la parte superior de la página. Este es un ejemplo de seudocódigo de un espacio de nombres condicional:

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

Un espacio de nombres condicional se puede dividir en dos partes separadas por el delimitador '?'. 

- El contenido que precede al delimitador indica el espacio de nombres o el esquema que contiene la API a la que se hace referencia. 
- El contenido después del '?' delimitador representa el método condicional que determina si el espacio de nombres condicional se evalúa como **verdadero** o **falso**.

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

Comentaremos estos métodos en más profundidad en secciones posteriores de este artículo.

> [!NOTE]
> Te recomendamos que uses IsApiContractPresent e IsApiContractNotPresent. Otras elementos condicionales no son totalmente compatibles con la experiencia de diseño de Visual Studio.

## <a name="create-a-namespace-and-set-a-property"></a>Crear un espacio de nombres y establecer una propiedad

En este ejemplo, se mostrará "Hello, Conditional XAML" como el contenido de un bloque de texto si la aplicación se ejecuta en Fall Creators Update o una versión posterior, y de forma predeterminada no se mostrará ningún contenido si se trata de una versión anterior.

En primer lugar, define un espacio de nombres personalizado con el prefijo 'contract5Present' y usar el espacio de nombres XAML predeterminado (http://schemas.microsoft.com/winfx/2006/xaml/presentation) como el esquema que contiene la propiedad [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text). Para hacer que sea un espacio de nombres condicional, agrega '?'. Delimitador después del esquema.

A continuación define una instrucción condicional que devuelva **true** (verdadero) en dispositivos que ejecuten Fall Creators Update o una versión posterior. Usa el método ApiInformation **IsApiContractPresent** para probar la versión 5 de UniversalApiContract. La versión 5 de UniversalApiContract se publicó con Fall Creators Update (SDK 16299).

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Después de definir el espacio de nombres, antepón el prefijo del espacio de nombres a la propiedad Text de tu control TextBox para que exista una propiedad que se debe establecer condicionalmente en tiempo de ejecución.

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

Este el XAML completo.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

Cuando ejecutas este ejemplo en Fall Creators Update, se muestra el texto: "Hello, Conditional XAML". Cuando se ejecuta en Creators Update, no se muestra ningún texto.

El XAML condicional te permite realizar las comprobaciones de API que puedes hacer en el código en el marcado en su lugar. Este es el código equivalente de esta comprobación.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

Recuerda que aunque el método IsApiContractPresent toma una cadena el parámetro *contractName* no colocarás las comillas ("") en la declaración de espacio de nombres XAML.

## <a name="use-ifelse-conditions"></a>Usar condiciones if/else

En el ejemplo anterior, la propiedad Text solo se establece cuando la aplicación se ejecuta en Fall Creators Update. Pero, ¿qué ocurre si quieres mostrar texto diferente al ejecutarla en Creators Update? Podrías intentar establecer la propiedad Text sin ningún calificador condicional, así.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

Esto funcionaría si se ejecuta en Creators Update, pero si se ejecuta en Fall Creators Update, recibirás un mensaje de error diciendo que la propiedad Text se ha establecido más de una vez.

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

Para usar la condición inversa, crea un segundo espacio de nombres XAML condicional que use la instrucción condicional **IsApiContractNotPresent**. Aquí, tiene el prefijo 'contract5NotPresent'.

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Con los dos espacios de nombres definidos, puedes establecer la propiedad Text dos veces siempre y cuando uses en ellos el prefijo con los calificadores que aseguren que solo una configuración de propiedades se usa en el momento de ejecución, como esto:

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

Este es otro ejemplo que establece el fondo de un botón. La característica [material acrílico](../design/style/acrylic.md) está disponible a partir de Fall Creators Update, así que usaremos Acrylic para el fondo cuando la aplicación se ejecute en Fall Creators Update. No está disponible en versiones anteriores, por lo que en estos casos, el fondo será de color rojo.

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>Crear controles y enlazar propiedades

Hasta ahora, has visto cómo establecer propiedades con XAML condicional, pero también puedes crear condicionalmente instancias de controles basados en el contrato de API, disponible en el tiempo de ejecución.

Aquí se crea una instancia de un control [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) cuando la aplicación se ejecuta en Fall Creators Update, donde el control está disponible. ColorPicker no está disponible en versiones anteriores a Fall Creators Update, por lo que cuando la aplicación se ejecuta en estas versiones anteriores, tienes que emplear [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) para proporcionar al usuario opciones de color simplificadas.

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

Puedes usar calificadores condicionales con distintas formas de [sintaxis de propiedad XAML ](../xaml-platform/xaml-syntax-guide.md). Aquí, la propiedad Fill del rectángulo se establece usando la sintaxis de elemento de propiedad para Fall Creators Update y la sintaxis de atributo para las versiones anteriores.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

Al enlazar una propiedad con otra propiedad que dependa de un espacio de nombres condicional, debes usar la misma condición en ambas propiedades. Aquí, `colorPicker.Color` depende del espacio de nombres condicional 'contract5Present', por lo que también se debe colocar el prefijo 'contract5Present' en la propiedad SolidColorBrush.Color. (O también puedes colocar el prefijo 'contract5Present' en SolidColorBrush en lugar de en la propiedad Color). Si no lo haces, obtendrás un error en el tiempo de compilación.

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

Este es el XAML completo que demuestra estos escenarios. Este ejemplo contiene un rectángulo y una interfaz de usuario que te permite establecer el color del rectángulo.

Si la aplicación se ejecuta en Fall Creators Update, usa ColorPicker para permitir al usuario establecer el color. ColorPicker no está disponible en versiones anteriores a Fall Creators Update, por lo que cuando la aplicación se ejecuta en estas versiones anteriores, tienes que emplear un cuadro combinado para proporcionar al usuario opciones de color simplificadas.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
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
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>Artículos relacionados

- [Guía de aplicaciones para UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contratos de API](https://channel9.msdn.com/Events/Build/2015/3-733) (Vídeo de compilación de 2015)
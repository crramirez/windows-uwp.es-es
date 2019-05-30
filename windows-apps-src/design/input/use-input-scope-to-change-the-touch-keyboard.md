---
Description: Para ayudar a que los usuarios escriban datos con el teclado táctil o con el panel de entrada por software (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que escriba el usuario.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Usar el ámbito de entrada para cambiar el teclado táctil
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: teclado,accesibilidad,navegación,foco,texto,entrada,interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: c522e21c45a3edd08a14b081cc227a83f19a3ea0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365361"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Usar el ámbito de entrada para cambiar el teclado táctil

Para ayudar a que los usuarios escriban datos con el teclado táctil o con el panel de entrada por software (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que escriba el usuario.

### <a name="important-apis"></a>API importantes
- [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


El teclado táctil se puede usar para escribir texto cuando la aplicación se ejecuta en un dispositivo con pantalla táctil. El teclado táctil se invoca cuando el usuario pulsa en un campo de entrada editable, como una **[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** o **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** . Es posible conseguir que los usuarios escriban datos en la aplicación de forma mucho más rápida y sencilla, si estableces el *ámbito de entrada* del control de texto para que coincida con el tipo de datos que esperas que el usuario escriba. El ámbito de entrada proporciona una sugerencia al sistema sobre el tipo de entrada de texto que espera el control para que el sistema pueda proporcionar una distribución del teclado táctil especializada para el tipo de entrada.

Por ejemplo, si un cuadro de texto se usa únicamente para escribir un PIN de 4 dígitos, establece la propiedad [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) en **Number**. Esto indica al sistema que debe mostrar el diseño de teclado numérico, lo cual facilita al usuario la inserción del PIN.

> [!IMPORTANT]
> - Esta información se aplica únicamente al SIP. No se aplica a teclados de hardware o al teclado en pantalla disponible en las opciones de accesibilidad de Windows.
> - El ámbito de entrada no implica que se realice ninguna validación de entrada y tampoco impide que el usuario proporcione cualquier entrada a través de un teclado de hardware u otro dispositivo de entrada. Sigues siendo responsable de la validación de la entrada en tu código, según sea necesario.

## <a name="changing-the-input-scope-of-a-text-control"></a>Cambio del ámbito de entrada de un control de texto

Los ámbitos de entrada que están disponibles para tu aplicación de Windows forman parte de la enumeración **[InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** . Puedes establecer la propiedad **InputScope** de un **[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** o de un **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** en uno de estos valores.

> [!IMPORTANT]
> El **[InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** propiedad **[PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** solo admite la **contraseña** y  **NumericPin** valores. Se ignorará cualquier otro valor.

Aquí, cambias el ámbito de entrada de varios cuadros de texto para que coincida con los datos que esperan cada uno de ellos.

**Para cambiar el ámbito de entrada en XAML**

1.  En el archivo XAML de la página, ubica la etiqueta para el control de texto que deseas cambiar.
2.  Agrega el atributo [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) a la etiqueta y especifica el valor [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) que coincida con la entrada esperada.

    Estos son algunos cuadros de texto que pueden aparecer en un formulario típico de contacto del cliente. Si se establece [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope), para cada cuadro de texto se muestra un teclado táctil con un diseño adecuado para los datos.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Para cambiar el ámbito de entrada en el código**

1.  En el archivo XAML de la página, ubica la etiqueta para el control de texto que deseas cambiar. Si no se está establecido, establece el atributo [x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute) para que puedas hacer referencia al control en el código.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Crea una instancia del objeto [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope).

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Crea una instancia del objeto [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName).
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Establece la propiedad [**NameValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) del objeto [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) en un valor de la enumeración [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue).

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Agrega el objeto [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) a la colección [**Names**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope.names) del objeto [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope).

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Establece el objeto [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) con el valor de la propiedad [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) del control de texto.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

Este es el código al completo.

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

Es posible condensar los mismos pasos en este código abreviado.

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>Predicción de texto, revisión ortográfica y autocorrección

Los controles [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) y [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) tienen varias propiedades que influyen en el comportamiento del SIP. Para proporcionar la mejor experiencia para los usuarios, es importante comprender cómo afectan estas propiedades a la introducción de texto con la entrada táctil.

-   [**IsSpellCheckEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)— al corrector ortográfico está habilitado para un control de texto, el control interactúa con el motor de revisar la ortografía del sistema para marcar las palabras que no se reconocen. Puedes presionar una palabra para ver una lista de las correcciones sugeridas. La revisión ortográfica está habilitada de manera predeterminada.

    Para el ámbito de entrada **Default**, esta propiedad también activa el uso de mayúsculas automático de la primera palabra de una frase y la corrección automática de palabras a medida que escribes. Estas funciones de corrección automática pueden estar deshabilitadas en otros ámbitos de entrada. Para obtener más información, consulta las tablas que figuran más adelante en este tema.

-   [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled): cuando está habilitada la predicción de texto para un control de texto, el sistema muestra una lista de palabras que se podrían ser a partir de al tipo. Puedes seleccionarlas de la lista para no tener que escribir toda la palabra. La predicción está habilitada de manera predeterminada.

    La predicción de texto puede estar deshabilitada si el ámbito de entrada es distinto de **Default**, incluso si la propiedad [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) es **true**. Para obtener más información, consulta las tablas que figuran más adelante en este tema.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus): cuando esta propiedad es **true**, impide que el sistema que muestra el SIP cuando el foco se establece mediante programación en un control de texto. En su lugar, el teclado se muestra únicamente cuando el usuario interactúa con el control.

## <a name="touch-keyboard-index-for-windows"></a>Índice de teclado táctil de Windows

Estas tablas muestran los diseños de Panel entrada software (SIP) de Windows para los valores de ámbito de entrada común. El efecto del ámbito de entrada en las funciones que habilitan las propiedades **IsSpellCheckEnabled** y **IsTextPredictionEnabled** se muestran para cada ámbito de entrada. Esta no es una lista completa de los ámbitos de entrada disponibles.

> [!Tip] 
> Puede alternar la mayoría de los teclados interacción entre un diseño es un carácter alfabético y un diseño de números y símbolos presionando el **& 123** clave para cambiar a la presentación de números y símbolos y presione la **abcd** clave a cambiar el diseño es un carácter alfabético.

### <a name="default"></a>Default

`<TextBox InputScope="Default"/>`

Teclado táctil de Windows de la predeterminada.

![Teclado táctil de Windows predeterminado](images/input-scopes/default.png)
- Revisión ortográfica: habilitada si **IsSpellCheckEnabled** = **true**, deshabilitada si **IsSpellCheckEnabled** = **false**
- Autocorrección: habilitada si **IsSpellCheckEnabled** = **true**, deshabilitada si **IsSpellCheckEnabled** = **false**
- Uso de mayúsculas automático: habilitada si **IsSpellCheckEnabled** = **true**, deshabilitada si **IsSpellCheckEnabled** = **false**
- Predicción de texto: habilitada si **IsTextPredictionEnabled** = **true**, deshabilitada si **IsTextPredictionEnabled** = **false**

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

La distribución de teclado de números y símbolos predeterminada.

![Teclado táctil de Windows para divisas](images/input-scopes/currencyamountandsymbol.png)

- Incluye las claves de la izquierda y derecha de página para mostrar símbolos más
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar
 
### <a name="url"></a>url

`<TextBox InputScope="Url"/>`

![Teclado táctil de Windows para direcciones URL](images/input-scopes/url.png)

- Incluye las teclas **.com** y ![go key](images/input-scopes/kbdgokey.png) (Ir). Presione y mantenga presionada la **.com** clave para mostrar las opciones adicionales ( **.org**, **.net**y sufijos específicos de la región)
- Incluye el **:** , **-** , y **/** claves
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: deshabilitada de manera predeterminada, se puede habilitar
- Uso de mayúsculas automático: deshabilitado de manera predeterminada, se puede habilitar
- Predicción de texto: deshabilitada de manera predeterminada, se puede habilitar


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Teclado táctil de Windows para direcciones de correo electrónico](images/input-scopes/emailsmtpaddress.png)
- Incluye las teclas **@** y **.com**. Presione y mantenga presionada la **.com** clave para mostrar las opciones adicionales ( **.org**, **.net**y sufijos específicos de la región)
- Incluye el **_** y **-** claves
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: deshabilitada de manera predeterminada, se puede habilitar
- Uso de mayúsculas automático: deshabilitado de manera predeterminada, se puede habilitar
- Predicción de texto: deshabilitada de manera predeterminada, se puede habilitar


### <a name="number"></a>Número

`<TextBox InputScope="Number"/>`

![Teclado táctil de Windows para números](images/input-scopes/number.png)
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![Teclado táctil de Windows para números de teléfono](images/input-scopes/telephonenumber.png)
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="search"></a>Buscar

`<TextBox InputScope="Search"/>`

![Teclado táctil de Windows para búsquedas](images/input-scopes/search.png)
- Incluye el **búsqueda** clave en lugar de la **ENTRAR** clave
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Teclado táctil de Windows para la búsqueda incremental](images/input-scopes/searchincremental.png)
- Mismo diseño que **predeterminado**
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: siempre deshabilitada
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: siempre deshabilitada

### <a name="formula"></a>Fórmula

`<TextBox InputScope="Formula"/>`

![Teclado táctil de Windows para la fórmula](images/input-scopes/formula.png)
- Incluye el **=** clave
- También incluye el **%** , **$** , y **+** claves
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![Teclado táctil de Windows predeterminado](images/input-scopes/default.png)
- Mismo diseño que **predeterminado**
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: habilitado de manera predeterminada, se puede deshabilitar
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Teclado táctil de Windows predeterminado](images/input-scopes/default.png)
- Mismo diseño que **predeterminado**
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: deshabilitada de manera predeterminada, se puede habilitar
- Uso automático de mayúsculas: desactivado de forma predeterminada, se puede habilitar (la primera letra de cada palabra en mayúscula)
- Predicción de texto: deshabilitada de manera predeterminada, se puede habilitar

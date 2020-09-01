---
Description: Para ayudar a los usuarios a escribir datos con el teclado táctil o con el panel de entrada de software (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que escriba el usuario.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Usar el ámbito de entrada para cambiar el teclado táctil
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: teclado,accesibilidad,navegación,foco,texto,entrada,interacción del usuario
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: e6e140a1967ca3ffe7775f427ccae7a7e07c5ca6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165819"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Usar el ámbito de entrada para cambiar el teclado táctil

Para ayudar a los usuarios a escribir datos con el teclado táctil o con el panel de entrada de software (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que escriba el usuario.

### <a name="important-apis"></a>API importantes
- [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


El teclado táctil se puede usar para escribir texto cuando la aplicación se ejecuta en un dispositivo con pantalla táctil. El teclado táctil se invoca cuando el usuario puntea en un campo de entrada modificable, como un **[cuadro de texto](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** o un **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)**. Puede hacer que los usuarios puedan escribir datos en la aplicación de forma mucho más rápida y sencilla estableciendo el *ámbito de entrada* del control de texto para que coincida con el tipo de datos que espera que el usuario escriba. El ámbito de entrada proporciona una sugerencia al sistema sobre el tipo de entrada de texto que espera el control para que el sistema pueda proporcionar un diseño de táctil especializado para el tipo de entrada.

Por ejemplo, si un cuadro de texto solo se usa para escribir un PIN de 4 dígitos, establezca la propiedad [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) en **Number**. Esto indica al sistema que debe mostrar el diseño de teclado numérico, lo cual facilita al usuario la entrada del PIN.

> [!IMPORTANT]
> - Esta información se aplica únicamente al SIP. No se aplica a teclados de hardware o al teclado en pantalla disponible en las opciones de accesibilidad de Windows.
> - El ámbito de entrada no implica que se realice ninguna validación de entrada y tampoco impide que el usuario proporcione cualquier entrada a través de un teclado de hardware u otro dispositivo de entrada. Sigues siendo responsable de la validación de la entrada en el código, según sea necesario.

## <a name="changing-the-input-scope-of-a-text-control"></a>Cambio del ámbito de entrada de un control de texto

Los ámbitos de entrada que están disponibles para tu aplicación de Windows forman parte de la enumeración **[InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)**. Puedes establecer la propiedad **InputScope** de un **[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** o de un **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** en uno de estos valores.

> [!IMPORTANT]
> La propiedad **[InputScope](/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** en **[PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** solo admite los valores de **contraseña** y **NumericPin** . Se ignorará cualquier otro valor.

Aquí, cambias el ámbito de entrada de varios cuadros de texto para que coincida con los datos que esperan cada uno de ellos.

**Para cambiar el ámbito de entrada en XAML**

1.  En el archivo XAML de la página, ubica la etiqueta para el control de texto que deseas cambiar.
2.  Agrega el atributo [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) a la etiqueta y especifica el valor [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) que coincida con la entrada esperada.

    Estos son algunos cuadros de texto que pueden aparecer en un formulario típico de contacto del cliente. Si se establece [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope), para cada cuadro de texto se muestra un teclado táctil con un diseño adecuado para los datos.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Para cambiar el ámbito de entrada en el código**

1.  En el archivo XAML de la página, ubica la etiqueta para el control de texto que deseas cambiar. Si no se está establecido, establece el atributo [x:Name](../../xaml-platform/x-name-attribute.md) para que puedas hacer referencia al control en el código.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Crea una instancia del objeto [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope).

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Crea una instancia del objeto [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName).
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Establece la propiedad [**NameValue**](/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) del objeto [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) en un valor de la enumeración [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue).

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Agrega el objeto [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) a la colección [**Names**](/uwp/api/windows.ui.xaml.input.inputscope.names) del objeto [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope).

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Establece el objeto [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) con el valor de la propiedad [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) del control de texto.

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

Los controles [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) y [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) tienen varias propiedades que influyen en el comportamiento del SIP. Para proporcionar la mejor experiencia para los usuarios, es importante comprender cómo afectan estas propiedades a la introducción de texto con la entrada táctil.

-   [**IsSpellCheckEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled): cuando la revisión ortográfica está habilitada para un control de texto, el control interactúa con el motor de corrección ortográfica del sistema para marcar las palabras que no se reconocen. Puedes presionar una palabra para ver una lista de las correcciones sugeridas. La revisión ortográfica está habilitada de manera predeterminada.

    Para el ámbito de entrada **Default**, esta propiedad también activa el uso de mayúsculas automático de la primera palabra de una frase y la corrección automática de palabras a medida que escribes. Estas funciones de corrección automática pueden estar deshabilitadas en otros ámbitos de entrada. Para obtener más información, consulta las tablas que figuran más adelante en este tema.

-   [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled): Cuando se habilita la predicción de texto para un control de texto, el sistema muestra una lista de palabras que es posible que estés empezando a escribir. Puedes seleccionarlas de la lista para no tener que escribir toda la palabra. La predicción está habilitada de manera predeterminada.

    La predicción de texto puede estar deshabilitada si el ámbito de entrada es distinto de **Default**, incluso si la propiedad [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) es **true**. Para obtener más información, consulta las tablas que figuran más adelante en este tema.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus): cuando esta propiedad es **true**, impide que el sistema muestre el teclado táctil cuando el foco se establece mediante programación en un control de texto. En su lugar, el teclado se muestra únicamente cuando el usuario interactúa con el control.

## <a name="touch-keyboard-index-for-windows"></a>Índice de teclado táctil para Windows

En estas tablas se muestran los diseños del panel de entrada de software (SIP) de Windows para los valores de ámbito de entrada comunes. El efecto del ámbito de entrada en las funciones que habilitan las propiedades **IsSpellCheckEnabled** y **IsTextPredictionEnabled** se muestran para cada ámbito de entrada. Esta no es una lista completa de los ámbitos de entrada disponibles.

> [!Tip] 
> Puede alternar la mayoría de los teclados táctiles entre un diseño Alfabético y un diseño de números y símbolos presionando la tecla **&123** para cambiar al diseño de números y símbolos, y presionar la tecla **ABCD** para cambiar a la distribución alfabética.

### <a name="default"></a>Valor predeterminado

`<TextBox InputScope="Default"/>`

Teclado táctil de Windows predeterminado.

![Teclado táctil de Windows predeterminado](images/input-scopes/default.png)
- Corrector ortográfico: habilitado si **IsSpellCheckEnabled**  =  **true**, deshabilitado si **IsSpellCheckEnabled**  =  **false**
- Corrección automática: habilitada si **IsSpellCheckEnabled**  =  **true**, deshabilitado si **IsSpellCheckEnabled**  =  **false**
- Capitalización automática: habilitada si **IsSpellCheckEnabled**  =  **true**, deshabilitada si **IsSpellCheckEnabled**  =  **false**
- Predicción de texto: habilitada si **IsTextPredictionEnabled**  =  **true**, Disabled if **IsTextPredictionEnabled**  =  **false**

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

La distribución de teclado de números y símbolos predeterminada.

![Teclado táctil de Windows para divisas](images/input-scopes/currencyamountandsymbol.png)

- Incluye las teclas izquierda/derecha de la página para mostrar más símbolos
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar
 
### <a name="url"></a>Url

`<TextBox InputScope="Url"/>`

![Teclado táctil de Windows para direcciones URL](images/input-scopes/url.png)

- Incluye las teclas **.com** y ![go key](images/input-scopes/kbdgokey.png) (Ir). Mantenga presionada la tecla **. com** para mostrar opciones adicionales (**. org**, **.net**y sufijos específicos de la región)
- Incluye las **claves:**, **-** y **/**
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: deshabilitada de manera predeterminada, se puede habilitar
- Uso de mayúsculas automático: deshabilitado de manera predeterminada, se puede habilitar
- Predicción de texto: deshabilitada de manera predeterminada, se puede habilitar


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Teclado táctil de Windows para direcciones de correo electrónico](images/input-scopes/emailsmtpaddress.png)
- Incluye las **@** claves y **. com** . Mantenga presionada la tecla **. com** para mostrar opciones adicionales (**. org**, **.net**y sufijos específicos de la región)
- Incluye las **teclas _** y **-**
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: deshabilitada de manera predeterminada, se puede habilitar
- Uso de mayúsculas automático: deshabilitado de manera predeterminada, se puede habilitar
- Predicción de texto: deshabilitada de manera predeterminada, se puede habilitar


### <a name="number"></a>Number

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

### <a name="search"></a>Search

`<TextBox InputScope="Search"/>`

![Teclado táctil de Windows para búsquedas](images/input-scopes/search.png)
- Incluye la clave de **búsqueda** en lugar de la tecla **entrar** .
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Teclado táctil de Windows para búsqueda incremental](images/input-scopes/searchincremental.png)
- El mismo diseño que el **predeterminado**
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: siempre deshabilitada
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: siempre deshabilitada

### <a name="formula"></a>Fórmula

`<TextBox InputScope="Formula"/>`

![Teclado táctil de Windows para fórmula](images/input-scopes/formula.png)
- Incluye la **=** clave
- También incluye las **%** **$** claves, y **+**
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: siempre deshabilitado
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![Teclado táctil de Windows predeterminado](images/input-scopes/default.png)
- El mismo diseño que el **predeterminado**
- Revisión ortográfica: habilitada de manera predeterminada, se puede deshabilitar
- Autocorrección: habilitada de forma predeterminada, se puede deshabilitar
- Uso de mayúsculas automático: habilitado de manera predeterminada, se puede deshabilitar
- Predicción de texto: habilitada de manera predeterminada, se puede deshabilitar

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Teclado táctil de Windows predeterminado](images/input-scopes/default.png)
- El mismo diseño que el **predeterminado**
- Revisión ortográfica: deshabilitada de manera predeterminada, se puede habilitar
- Autocorrección: deshabilitada de manera predeterminada, se puede habilitar
- Capitalización automática: desactivado de forma predeterminada, se puede habilitar (la primera letra de cada palabra está en mayúsculas)
- Predicción de texto: deshabilitada de manera predeterminada, se puede habilitar
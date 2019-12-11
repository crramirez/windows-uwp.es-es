---
Description: NumberBox es un control que se puede usar para mostrar y editar números.
title: Cuadro de número
template: detail.hbs
ms.date: 11/27/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: bcabe0867e21b20f97405d514b16bef2165821fb
ms.sourcegitcommit: be25b7628b1ff22175a682e670539947db2fa85a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74852867"
---
# <a name="number-box"></a>Cuadro de número

Representa un control que se puede usar para mostrar y editar números. Admite la validación, los incrementos paso a paso y el procesamiento de cálculos en línea de ecuaciones básicas, como la multiplicación, la división, la suma y la resta.


**API importantes:** [Clase NumberBox](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.NumberBox)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado? 

Puedes usar un control NumberBox para capturar y mostrar las entradas matemáticas. Si necesitas un cuadro de texto modificable que acepte otros valores y no solo números, usa el control [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Si necesitas un cuadro de texto modificable que acepte contraseñas u otros datos confidenciales, consulta [PasswordBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox). Si necesitas un cuadro de texto para escribir términos de búsqueda, consulta [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox). Si tienes que escribir o editar texto con formato, consulta [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TextBox">abrirla y ver NumberBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>Creación de un control NumberBox sencillo

Este es el código XAML para crear un NumberBox básico que muestra su apariencia predeterminada. Usa [x:Bind](/windows/uwp/xaml-platform/x-bind-markup-extension#property-path) para asegurarte de que los datos que se muestran al usuario permanecen sincronizados con los datos almacenados en la aplicación. 


```xaml
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```
![Un campo de entrada enfocado que muestra 0.](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>Etiquetado de NumberBox

Usa `Header` o `PlaceholderText` si no está claro el fin del NumberBox. `Header` es visible independientemente de si NumberBox tiene un valor o no. 

```xaml
<NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Un encabezado que "Escribe una expresión:" sobre un NumberBox.](images/numberbox-header.png)

`PlaceholderText` se muestra dentro de NumberBox y solo aparecerá cuando `Value` esté establecido en NaN o cuando el usuario borre la entrada.

```xaml
<NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Un NumberBox que contiene el texto de marcador de posición "A + B".](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>Activación de la compatibilidad con cálculos

Al establecer la propiedad `AcceptsExpression` en true, NumberBox puede evaluar las expresiones en línea básicas como la multiplicación, división, suma y resta siguiendo el orden estándar de las operaciones. La evaluación se desencadena cuando se pierde el foco o cuando el usuario presiona la tecla "Entrar". Una vez que se evalúa una expresión, no se conserva el formato original de la expresión.

XAML
```xaml
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>Incremento y decremento paso a paso

Usa la propiedad `SmallChange` para configurar cuánto se cambia el valor dentro de un NumberBox cuando el NumberBox tiene el foco y el usuario realiza alguna de las siguientes acciones:

- se desplaza;
- presiona la tecla de dirección arriba;
- presiona la tecla de dirección abajo.

Usa la propiedad `LargeChange` para configurar cuánto se cambia el valor dentro de un NumberBox cuando el NumberBox tiene el foco y el usuario presiona la tecla Re Pág o Av Pág. 

Usa la propiedad `SpinButtonPlacementMode` para habilitar los botones en los que se puede hacer clic para aumentar o disminuir el valor de NumberBox según la cantidad especificada en la propiedad `SmallChange`. Estos botones se deshabilitarán si otro paso superaría un valor máximo o mínimo. 

Establece `SpinButtonPlacementMode` en `Inline` para que los botones aparezcan junto al control. 

XAML
```XAML
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![Un control NumberBox con un botón de flecha hacia abajo y un botón de flecha hacia arriba junto a él.](images/numberbox-spinbutton-inline.png)

Establece `SpinButtonPlacementMode` en `Compact` para que los botones aparezcan como un control flotante solo cuando el control NumberBox tenga el foco.  

XAML
```XAML
<NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![Un control NumberBox con un pequeño icono dentro que muestra una flecha hacia arriba y una flecha hacia abajo.](images/numberbox-spinbutton-compact-non-visible.png)

![Un control NumberBox con un botón de flecha hacia abajo y un botón de flecha arriba flotante a su lado en una capa superior.](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>Activación de la validación de entrada

La configuración de `ValidationMode` en `InvalidInputOverwritten` permitirá a NumberBox sobrescribir entradas no válidas que no sean numéricas ni cumplan los requisitos de las fórmulas según el último valor válido. La evaluación se desencadenará cuando se pierda el foco o se presione la tecla "Entrar".

XAML
```XAML
<NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

Puedes establecer `ValidationMode` en `Disabled` para configurar una validación de entrada personalizada.  

Con respecto a las comas y puntos decimales, el formato utilizado por un usuario se reemplazará por el formato configurado para el control NumberBox. No se desencadenará un error de validación de entrada. 

### <a name="formatting-input"></a>Formato de las entradas 

El [formato de los números](/uwp/api/windows.globalization.numberformatting) se puede usar para dar formato al valor de un control Numberbox. Para ello, se configura una instancia de una clase de formato y se asigna a la propiedad `NumberFormatter`. Las cifras decimales, divisas, porcentajes y significativas son algunas de las clases de formato de número disponibles. Ten en cuenta que el redondeo también se define mediante las propiedades de formato de número.

A continuación se muestra un ejemplo del uso de DecimalFormatter para dar formato al valor de un control NumberBox para que tenga un dígito entero, dos dígitos de fracción y se redondee al 0,25 más cercano:  

XAML
```XAML
<NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

C#
```C#
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![Un control NumberBox que muestra el valor 0,00.](images/numberbox-formatted.png)

Con respecto a las comas y puntos decimales, el formato utilizado por un usuario se reemplazará por el formato configurado para el control NumberBox. No se desencadenará un error de validación de entrada. 

## <a name="remarks"></a>Observaciones

### <a name="input-scope"></a>Ámbito de entrada

Se usará `Number` para el [ámbito de entrada](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue). Este ámbito de entrada está pensado para trabajar con los dígitos del 0 al 9. Se puede sobrescribir, pero no se admiten explícitamente los tipos de InputScope alternativos. 

### <a name="not-a-number"></a>No es un número

Cuando se borra la entrada de un control NumberBox, `Value` se establece en `NaN` para indicar que no hay ningún valor numérico. 

### <a name="expression-evaluation"></a>Evaluación de expresiones 

NumberBox usa una notación infija para evaluar expresiones. En orden de prioridad, los operadores permitidos son:

* ^
* */
* +-

Ten en cuenta que se pueden utilizar paréntesis para invalidar las reglas de precedencia. 

## <a name="recommendations"></a>Recomendaciones

* Con `Text` y `Value` es fácil capturar el valor de un control NumberBox como una cadena o como un valor Double sin necesidad de realizar conversiones de tipo entre ambos. Si modificas mediante programación el valor de NumberBox, se recomienda que lo hagas a través de la propiedad `Value`. `Value` sobrescribirá a `Text` en la configuración inicial. Después de la configuración inicial, los cambios a uno de estos elementos se propagarán al otro. Sin embargo, realizar siempre los cambios mediante programación a través de `Value` ayuda a evitar que NumberBox acepte los caracteres no numéricos a través de `Text`.  
* Utiliza `Header` o `PlaceholderText` para informar a los usuarios de que el control NumberBox solo acepta caracteres numéricos como entrada. La representación ortográfica de los números, como "uno", no se resolverá como un valor aceptado. 

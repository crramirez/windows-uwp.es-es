---
author: Jwmsft
Description: "Analiza con qué frecuencia leemos el texto en nuestras vidas diarias, en correo electrónico, un libro, una señal de tráfico, los precios en una carta, la presión de los neumáticos, un letrero en un poste de la calle."
title: Controles de texto
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Text controls
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 1fec4bf13567507087eef08feea0a3ebc255b43f

---
# Controles de texto
Los controles de texto constan de cuadros de entrada de texto, cuadros de contraseña, cuadros de sugerencia automática y bloques de texto. El marco XAML brinda diversos controles para representar e ingresar texto, así como un conjunto de propiedades para aplicar formato al texto.

- Los controles para mostrar texto de solo lectura son [TextBlock](text-block.md) y [RichTextBlock](rich-text-block.md).
- Los controles de edición y entrada de texto son: [TextBox](text-block.md), [AutoSuggestBox](auto-suggest-box.md), [PasswordBox](password-box.md) y [RichEditBox](rich-edit-box.md). 


<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)
-   [**Clase PasswordBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)
-   [**Clase RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)
-   [**Clase RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)
-   [**Clase TextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)
-   [**Clase TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)

## ¿Es este el control adecuado?

El control de texto que uses dependerá del escenario. Usa esta información para seleccionar el control de texto adecuado para usarlo en tu aplicación.

### Presentar texto de solo lectura

Usa un **TextBlock** para mostrar la mayor parte del texto de solo lectura en la aplicación. Puedes usarlo para mostrar texto de una o varias líneas, hipervínculos en línea y texto con formato como, por ejemplo, negrita, cursiva o subrayado.

TextBlock suele ser más fácil de usar y proporciona mejor rendimiento de representación de texto que RichTextBlock, por lo que es preferible para gran parte del texto de la interfaz de usuario de la aplicación. Puedes acceder fácilmente y usar texto de un TextBlock en tu aplicación obteniendo el valor de la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx). 

También proporciona muchas de las mismas opciones de formato para personalizar la representación del texto. Aunque puedes incluir saltos de línea en el texto, TextBlock está diseñado para mostrar un único párrafo y no admite la sangría del texto.

Usa un **RichTextBlock** si necesitas admitir varios párrafos, texto de varias columnas u otros diseños de texto complejos, o elementos de interfaz de usuario en línea, como imágenes. RichTextBlock proporciona varias características de diseño de texto avanzado.

La propiedad de contenido de RichTextBlock es la propiedad [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), que admite el texto basado en párrafos a través del elemento [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). No tiene una propiedad **Text** que puedas usar para obtener acceso con facilidad a contenido de texto del control en la aplicación.  

### Entrada de texto

Usa un control **TextBox** para permitir al usuario escribir y editar texto sin formato, como en un formulario. Puedes usar la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) para obtener el texto y establecerlo en un TextBox.

Puedes hacer un TextBox de solo lectura, pero este debe ser un estado temporal y condicional. Si el texto nunca es editable, considera la posibilidad de usar un TextBlock en su lugar.

Usa un control **PasswordBox** para recopilar la contraseña u otros datos privados, como el número de la Seguridad social. Un cuadro de contraseña es un cuadro de entrada de texto que oculta los caracteres que se escriben en él para asegurar la privacidad. Un cuadro de contraseña parece un cuadro de entrada de texto, salvo que muestra viñetas, en vez del texto que se ha escrito. El carácter de viñeta se puede personalizar.

Usa un control **AutoSuggestBox** para mostrar al usuario una lista de sugerencias para que elija una a medida que escribe. Un cuadro de sugerencias automáticas es un cuadro de entrada de texto que desencadena una lista de sugerencias de búsqueda básicas. Los términos sugeridos pueden dibujarse a partir de una combinación de los términos de búsqueda más usados y el historial de los términos que el usuario ha escrito.

También debes usar un control AutoSuggestBox para implementar un cuadro de búsqueda.

Usa un **RichEditBox** para mostrar y editar los archivos de texto. No uses un RichEditBox para que el usuario haga entradas en la aplicación de la manera que usas otros cuadros de entrada de texto estándar. En su lugar, úsalo para trabajar con archivos de texto que sean independientes de la aplicación. En general, el texto que se escribe en un RichEditBox se guarda en un archivo .rtf.

**¿La entrada de texto es la mejor opción?**

Hay muchas maneras de obtener la entrada del usuario en la aplicación. Estas preguntas ayudarán a responder si uno de los cuadros de texto de entrada estándar u otro control es la mejor opción para obtener la entrada del usuario.

-   **¿Es práctico enumerar eficientemente todos los valores válidos?** De ser así, deberías usar uno de los controles de selección, como una [casilla](checkbox.md), una [lista desplegable](lists.md), un [botón de radio](radio-button.md), un [control deslizante](slider.md), un [modificador para alternar](toggles.md), un [selector de fecha](date-and-time.md) o un selector de hora.
-   **¿Existe un conjunto reducido de valores válidos?** Si es así, considera la posibilidad de usar una [lista desplegable](lists.md) o un cuadro de lista, especialmente si la longitud de los valores supera los pocos caracteres.
-   **¿Los datos válidos no tienen absolutamente ninguna restricción? ¿O están los datos válidos restringidos solo por el formato (restricción de longitud o tipos de caracteres)?** Si es así, usa un control de entrada de texto. Puedes limitar el número de caracteres que se pueden especificar y puedes validar el formato en el código de la aplicación.
-   **¿El valor representa un tipo de datos que tiene un control común especializado?** Si es así, usa el control adecuado en lugar de un control de entrada de texto. Por ejemplo, usa un [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681) en lugar de un control de entrada de texto para aceptar una entrada de fecha.
-   Si los datos son estrictamente numéricos:
    -   **¿El valor introducido es aproximado y/o relativo a otra cantidad mostrada en la misma página?** De ser así, usa un [control deslizante](slider.md).
    -   **¿Sería bueno que el usuario recibiera una respuesta instantánea del efecto de los cambios en la configuración?** De ser así, usa un [control deslizante](slider.md), posiblemente con un control adjunto.
    -   **¿Es probable que el valor introducido se ajuste tras observar el resultado (como, por ejemplo, el volumen o el brillo de pantalla)?** De ser así, usa un [control deslizante](slider.md).
    
## Ejemplos

Cuadro de texto

![Cuadro de texto](images/text-box.png)

Cuadro de sugerencias automáticas

![Ejemplo de control ampliado de la sugerencia automática](images/controls_autosuggest_expanded01.png)

Cuadro de contraseña

![Cuadro de contraseña con el foco y texto escribiéndose](images/passwordbox-focus-typing.png)

## Crear un control de texto

Consulte los siguientes artículos para obtener información y ejemplos específicos de cada control de texto.

-   [**AutoSuggestBox**](auto-suggest-box.md)
-   [**PasswordBox**](password-box.md)
-   [**RichEditBox**](rich-edit-box.md)
-   [**RichTextBlock**](rich-text-block.md)
-   [**TextBlock**](text-block.md)
-   [**TextBox**](text-box.md)

## Directrices de fuente y estilo
Consulta estos artículos para obtener directrices de fuentes:

- [**Directrices de fuentes**](fonts.md)
- [**Directrices y lista de iconos de Segoe MDL2**](segoe-ui-symbol-font.md)


## Elegir el teclado adecuado para el control de texto

**Se aplica a:** TextBox, PasswordBox RichEditBox

Para que los usuarios escriban datos con facilidad mediante el teclado táctil o con el panel de entrada suave (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que el usuario escriba.

>Sugerencia Esta información se aplica únicamente al SIP. No se aplica a teclados de hardware o al teclado en pantalla disponible en las opciones de accesibilidad de Windows.

El teclado táctil se puede usar para escribir texto cuando la aplicación se ejecuta en un dispositivo con pantalla táctil. El teclado táctil se invoca cuando el usuario pulsa en un campo de entrada editable, como un TextBox o RichEditBox. Es posible conseguir que los usuarios escriban datos en la aplicación de forma mucho más rápida y sencilla, si estableces el ámbito de entrada del control de texto para que coincida con el tipo de datos que esperas que el usuario escriba. El ámbito de entrada proporciona una sugerencia al sistema sobre el tipo de entrada de texto que espera el control para que el sistema pueda proporcionar una distribución del teclado táctil especializada para el tipo de entrada.

Por ejemplo, si un cuadro de texto se usa únicamente para escribir un PIN de 4 dígitos, establece la propiedad [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) en **Number**. Esto indica al sistema que debe mostrar el diseño de teclado numérico, lo cual facilita al usuario la inserción del PIN.

>Importante  
>El ámbito de entrada no implica que se realice ninguna validación de entrada y tampoco impide que el usuario proporcione cualquier entrada a través de un teclado de hardware u otro dispositivo de entrada. Sigues siendo responsable de la validación de la entrada en tu código, según sea necesario.

Para obtener más información, consulta [Usar el ámbito de entrada para cambiar el teclado táctil]().

## Colores de fuentes

**Se aplica a:** TextBlock, RichTextBlock, TextBox, RichEditBox

Windows tiene la posibilidad de que las fuentes incluyan varias capas de colores para cada glifo. Por ejemplo, la fuente Segoe UI Emoji define las versiones de color de los emoticonos y otros caracteres Emoji. 

Los controles estándar y de texto enriquecido admiten la visualización de las fuentes de color. De manera predeterminada, la propiedad **IsColorFontEnabled** es **true** y las fuentes con estas capas adicionales se representan en color. La fuente de color predeterminada del sistema es Segoe UI Emoji y los controles se revertirán a esta fuente para mostrar los glifos de color. 

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

El texto representado tiene este aspecto:

![Bloque de texto con fuente de color](images/text-block-color-fonts.png)

Para obtener más información, consulta la propiedad [**IsColorFontEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx).

## Directrices para los separadores de línea y de párrafo

**Se aplica a:** TextBlock, RichTextBlock, TextBox multilínea, RichEditBox

Usa el carácter separador de línea (0x2028) y el carácter separador de párrafo (0x2029) para dividir el texto sin formato. Se inicia una nueva línea después de cada separador de línea. Se inicia un nuevo párrafo detrás de cada separador de párrafo.

No es necesario iniciar la primera línea o párrafo de un archivo con estos caracteres o terminar la última línea o párrafo con ellos; al hacerlo así se indica que hay una línea o párrafo vacío en esa ubicación.

Tu aplicación puede usar el separador de línea para indicar un final de línea incondicional. Sin embargo, los separadores de línea no se corresponden con los caracteres de avance de línea y retorno de carro independientes o una combinación de estos caracteres. Los separadores de línea se deben procesar por separado de los caracteres de avance de línea y retorno de carro.

La aplicación puede insertar un separador de párrafo entre párrafos de texto. El uso de este separador permite la creación de archivos de texto sin formato que se pueden formatear con anchos de línea diferentes en diferentes sistemas operativos. El sistema de destino puede ignorar los separadores de línea y realizar saltos entre párrafos solo en los separadores de párrafo.



## Artículos relacionados

**Para diseñadores**
- [**Directrices de fuentes**](fonts.md)
- [**Directrices y lista de iconos de Segoe MDL2**](segoe-ui-symbol-font.md)
- [Directrices sobre revisión ortográfica](spell-checking-and-prediction.md)
- [Adición de búsqueda](https://msdn.microsoft.com/library/windows/apps/hh465231)

**Para desarrolladores (XAML)**
- [**Clase TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Clase Windows.UI.Xaml.Controls PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propiedad String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)



<!--HONumber=Jun16_HO3-->



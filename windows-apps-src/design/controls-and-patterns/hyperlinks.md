---
author: Jwmsft
Description: Hyperlinks navigate the user to another part of the app, to another app, or launch a specific uniform resource identifier (URI) using a separate browser app.
title: Hipervínculos
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e6679f789f2530a34a2bf527556c144e7a8e03c3
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7440086"
---
# <a name="hyperlinks"></a>Hipervínculos

 

Los hipervínculos llevan al usuario a otra parte de la aplicación, a otra aplicación o permiten iniciar un identificador uniforme de recursos (URI) específico con una aplicación de explorador diferente. Existen dos formas mediante las que se puede agregar un hipervínculo a una aplicación XAML: el elemento de texto **Hyperlink** y el control **HyperlinkButton**.

> **API importantes**: [Elemento de texto Hyperlink](https://msdn.microsoft.com/library/windows/apps/dn279356), [Control HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739)

![Botón de hipervínculo](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un hipervínculo cuando necesites texto que responda cuando se seleccione y dirija al usuario a más información sobre el texto que se ha seleccionado.

Elige el tipo correcto de hipervínculo según tus necesidades:

-   Usa un elemento **Hyperlink** incluido dentro de un control de texto. Un elemento Hyperlink fluye con otros elementos de texto y se puede usar en cualquier elemento InlineCollection. Usa un hipervínculo de texto si quieres ajuste de texto automático y no necesitas obligatoriamente un destino de gran alcance. El texto del hipervínculo puede ser pequeño y difícil de activar, especialmente con la entrada táctil.
-   Usa un elemento **HyperlinkButton** para los hipervínculos independientes. Un elemento HyperlinkButton es un control Button especializado que puedes usar en cualquier lugar en que usarías un elemento Button.
-   Usa un elemento **HyperlinkButton** con una [imagen](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) como su contenido para crear una imagen interactiva.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/HyperlinkButton">abrir la aplicación y ver HyperlinkButton en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>Crear un elemento de texto Hyperlink

En este ejemplo se muestra cómo usar un elemento de texto Hyperlink dentro de un bloque [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx).

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
El hipervínculo aparece en línea y fluye con el texto que le rodea:

![Ejemplo de un hipervínculo como elemento de texto](images/controls_hyperlink-element.png) 

> **Sugerencia**&nbsp;&nbsp;Cuando uses un elemento Hyperlink en un control de texto con otros elementos de texto en XAML, coloca el contenido en un contenedor [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) y aplica el atributo `xml:space="preserve"` al contenedor Span para mantener el espacio en blanco entre el elemento Hyperlink y otros elementos.

## <a name="create-a-hyperlinkbutton"></a>Crear un elemento HyperlinkButton

Aquí se muestra cómo usar un elemento HyperlinkButton, tanto con texto como con una imagen.

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```

Los botones de hipervínculo con contenido de texto aparecen como texto marcado. La imagen del logotipo de Contoso también es un hipervínculo interactivo:

![Ejemplo de un hipervínculo como control de botón](images/controls_hyperlink-button-image.png)

En este ejemplo se muestra cómo crear un HyperlinkButton en el código.

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>Controlar la navegación

En los dos tipos de hipervínculos, puedes controlar la navegación del mismo modo; puedes establecer la propiedad **NavigateUri** o controlar el evento **Click**.

**Navegar a un URI**

Para usar el hipervínculo para navegar a un URI, establece la propiedad NavigateUri. Cuando un usuario hace clic en el hipervínculo o lo pulsa, el URI especificado se abre en el explorador predeterminado. El explorador predeterminado se ejecuta en un proceso independiente de la aplicación.

> [!NOTE]
> Un URI se representa mediante la clase [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri). Cuando se programa con .NET, esta clase está oculta y se debe usar la [System.Uri](https://docs.microsoft.com/dotnet/api/system.uri). Para obtener más información, consulta las páginas de referencia de estas clases.

No es necesario usar esquemas **http:** o **https:**. Se pueden usar esquemas como **ms-appx:**, **ms-appdata:** o **ms-resources:**, si hay contenido de recursos en estas ubicaciones que sea adecuado para cargarlo en un explorador. Sin embargo, se bloquea específicamente el esquema **file:**. Para más información, consulta [Esquemas de URI](https://msdn.microsoft.com/library/windows/apps/jj655406.aspx).

Cuando un usuario hace clic en el hipervínculo, el valor de la propiedad NavigateUri se pasa a un controlador de sistema para esquemas y tipos de URI. El sistema inicia entonces la aplicación que esté registrada para el esquema del URI proporcionado para NavigateUri.

Si no quieres que el hipervínculo cargue contenido en un explorador web predeterminado (y no quieres que aparezca un explorador), no establezcas un valor para NavigateUri. En su lugar, controla el evento Click y escribe código que haga lo que quieres.


**Controlar el evento Click**

Usa el evento Click para las acciones que no sean iniciar un URI en un explorador, como la navegación dentro de la aplicación. Por ejemplo, si quieres cargar una nueva página de la aplicación, en lugar de abrir un explorador, llama a un método [Frame.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.navigate.aspx) en el controlador de eventos Click para navegar a la nueva página de la aplicación. Si quieres que un URI absoluto y externo se cargue dentro de un control [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) que también exista en la aplicación, llama a [WebView.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.navigate.aspx) como parte de la lógica del controlador de eventos Click.

Normalmente no se controla el evento Click a la vez que se especifica un valor de NavigateUri, ya que representan dos formas diferentes de usar el elemento de hipervínculo. Si tu intención es abrir el URI en el explorador predeterminado y se ha especificado un valor para NavigateUri, no controles el evento Click. Por el contrario, si controlas el evento Click, no especifiques un valor de NavigateUri.

No hay nada que se pueda hacer en el controlador de eventos Click para impedir que el explorador predeterminado cargue cualquier destino válido especificado para NavigateUri; la acción se realiza automáticamente (de forma asincrónica) cuando el hipervínculo se activa y no se puede cancelar desde el controlador de eventos Click. 

## <a name="hyperlink-underlines"></a>Subrayados de los hipervínculos
De manera predeterminada, los hipervínculos aparecen subrayados. Este subrayado es importante porque ayuda a cumplir los requisitos de accesibilidad. Los usuarios daltónicos se ayudan del subrayado para distinguir entre hipervínculos y otros tipos de texto. Si deshabilita los subrayados, considera la posibilidad de agregar algún otro tipo de diferencia de formato para distinguir los hipervínculos de otros tipos de texto, como FontWeight o FontStyle.

**Elementos de texto Hyperlink**

Puedes establecer la propiedad [UnderlineStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.hyperlink.underlinestyle.aspx) para deshabilitar el subrayado. Si lo haces, considera la posibilidad de usar [FontWeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontweight.aspx) o [FontStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontstyle.aspx) para diferenciar el texto de los vínculos.

**HyperlinkButton** 

De manera predeterminada, el elemento HyperlinkButton aparece como texto subrayado cuando se establece una cadena como el valor de la propiedad [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx).

El texto no aparece subrayado en los siguientes casos:
- Se establece un bloque [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) como el valor para la propiedad Content y se establece la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.text.aspx) en el bloque TextBlock.
- Se modifica la plantilla del elemento HyperlinkButton y se cambia el nombre de la parte de la plantilla [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx).

Si necesitas un botón que aparezca como texto sin subrayado, considera la posibilidad de usar un control Button estándar y aplicar el recurso de sistema `TextBlockButtonStyle` integrado a su propiedad Style.

## <a name="notes-for-hyperlink-text-element"></a>Notas para el elemento de texto Hyperlink

Esta sección se aplica únicamente al elemento de texto Hyperlink, no al control HyperlinkButton.

**Eventos de entrada**

Como un elemento Hyperlink no es un [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx), no tiene el conjunto de eventos de entrada de los elementos de la interfaz de usuario como Tapped, PointerPressed, etc. En su lugar, un elemento Hyperlink tiene su evento Click, así como el comportamiento implícito del sistema que carga cualquier URI que se especifique como el valor de NavigateUri. El sistema controla todas las acciones de entrada que deberían invocar las acciones Hyperlink y genera como respuesta el evento Click.

**Contenido**

El elemento Hyperlink tiene restricciones respecto al contenido que puede existir en su colección de [elementos Inline](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.inlines.aspx). En concreto, un elemento Hyperlink solo permite la clase [Run](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.run.aspx) y otros tipos de [Span]() que no sean otro elemento Hyperlink. [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.inlineuicontainer.aspx) no puede estar en la colección de elementos Inlines de un elemento Hyperlink. Si se intenta agregar contenido restringido, se genera una excepción de argumento no válido o una excepción de análisis XAML.

**Comportamiento de Hyperlink y el tema y estilo**

Hyperlink no hereda de [Control](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.aspx), de modo que no tiene una propiedad [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.style.aspx) ni una [Template](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.template.aspx). Se pueden modificar las propiedades que se heredan de [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.aspx) como, por ejemplo, Foreground o FontFamily, para cambiar la apariencia de un elemento Hyperlink, pero no se puede usar una plantilla ni un estilo comunes para aplicar los cambios. En lugar de usar una plantilla, considere la posibilidad de usar recursos comunes para los valores de las propiedades de Hyperlink a fin de ofrecer una experiencia coherente. Algunas propiedades de Hyperlink usan valores predeterminados de un valor de extensión de marcado {ThemeResource} que proporciona el sistema. Esto permite que la apariencia del elemento Hyperlink cambie de forma adecuada cuando el usuario modifique el tema del sistema en tiempo de ejecución.

El color predeterminado del hipervínculo es el color de énfasis del sistema. Puedes establecer la propiedad [Foreground](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.foreground.aspx) para reemplazar este comportamiento.

## <a name="recommendations"></a>Recomendaciones

-   Usa los hipervínculos solo para la navegación; no los uses para otras acciones.
-   Usa el estilo Body de la tabla de tipos para los hipervínculos basados en texto. Obtén información sobre [fuentes y la tabla de tipos de Windows 10](../style/typography.md).
-   Mantén los hipervínculos discretos y suficientemente separados para que el usuario pueda diferenciarlos y seleccionarlos con facilidad.
-   Agrega información sobre herramientas a los hipervínculos que indican dónde se dirigirá al usuario. Si se dirige al usuario a un sitio externo, incluye el nombre de dominio de nivel superior dentro de la información sobre herramientas y un estilo de texto con un color de fuente secundario.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Directrices para información sobre herramientas](tooltips.md)

**Para desarrolladores (XAML)**
- [Clase Windows.UI.Xaml.Documents.Hyperlink](https://msdn.microsoft.com/library/windows/apps/dn279356)
- [Clase Windows.UI.Xaml.Controls.HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739)

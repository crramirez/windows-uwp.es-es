---
description: Usa un control RichTextBlock con elementos RichTextBlockOverflow para crear diseños de texto avanzados.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d1e5619009bd3218dbb5b6796585296dcd873192
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035208"
---
# <a name="rich-text-block"></a>Bloque de texto enriquecido

 

Los bloques de texto enriquecido proporcionan varias características para el diseño avanzado de texto que puedes usar cuando necesitas admitir párrafos, elementos de interfaz de usuario en línea o diseños de texto complejos.

> **API de plataforma** : [Clase RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), [Clase RichTextBlockOverflow](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlockOverflow), [Clase Paragraph](/uwp/api/Windows.UI.Xaml.Documents.Paragraph), [Clase Typography](/uwp/api/Windows.UI.Xaml.Documents.Typography)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un **RichTextBlock** si necesitas admitir varios párrafos, varias columnas u otros diseños de texto complejos, o elementos de interfaz de usuario en línea, como imágenes.

Usa un **TextBlock** para mostrar la mayor parte del texto de solo lectura en la aplicación. Puedes usarlo para mostrar texto de una o varias líneas, hipervínculos en línea y texto con formato como, por ejemplo, negrita, cursiva o subrayado. TextBlock proporciona un modelo de contenido más simple, por lo que normalmente es más fácil de usar, y puede ofrecer un mejor rendimiento de representación de texto que RichTextBlock. Se prefiere para la mayoría del texto de la interfaz de usuario de la aplicación. Aunque puedes incluir saltos de línea en el texto, TextBlock está diseñado para mostrar un único párrafo y no admite la sangría del texto.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RichTextBlock">abrirla y ver RichTextBlock en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>Crear un bloque de texto enriquecido

La propiedad de contenido de RichTextBlock es la propiedad [Blocks](/uwp/api/windows.ui.xaml.controls.richtextblock.blocks), que admite el texto basado en párrafos a través del elemento [Paragraph](/uwp/api/Windows.UI.Xaml.Documents.Paragraph). No tiene una propiedad **Text** que puedas usar para obtener acceso con facilidad a contenido de texto del control en la aplicación. Sin embargo, RichTextBlock proporciona varias características únicas que TextBlock no ofrece. 

RichTextBlock admite:
- Varios párrafos. Configura la propiedad [TextIndent](/uwp/api/windows.ui.xaml.controls.richtextblock.textindent) para establecer la sangría de los párrafos.
- Elementos de la interfaz de usuario en línea. Usa un [InlineUIContainer](/uwp/api/Windows.UI.Xaml.Documents.InlineUIContainer) para visualizar los elementos de la interfaz de usuario, tales como las imágenes, en línea con el texto.
- Contenedores de desbordamiento. Usa elementos [RichTextBlockOverflow](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlockOverflow) para crear diseños de texto de varias columnas.

### <a name="paragraphs"></a>Párrafos

Los elementos [Paragraph](/uwp/api/Windows.UI.Xaml.Documents.Paragraph) se usan para definir los bloques de texto que se muestran dentro de un control RichTextBlock. Cada RichTextBlock debe incluir al menos un Paragraph. 

Para definir la cantidad de sangría de todos los párrafos de un RichTextBlock, se puede establecer la propiedad [RichTextBlock.TextIndent](/uwp/api/windows.ui.xaml.controls.richtextblock.textindent). Para invalidar esta configuración para párrafos específicos de un RichTextBlock, puedes establecer la propiedad [Paragraph.TextIndent](/uwp/api/windows.ui.xaml.documents.paragraph.textindent) en un valor diferente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>Elementos de la interfaz de usuario en línea

La clase [InlineUIContainer](/uwp/api/Windows.UI.Xaml.Documents.InlineUIContainer) permite insertar cualquier UIElement en línea con el texto. Un escenario común es colocar un objeto Image en línea con el texto, pero también puedes usar elementos interactivos, como un objeto Button o CheckBox.

Si quieres integrar más de un elemento en línea en la misma posición, considera usar un panel como el único elemento secundario InlineUIContainer y luego colocar los diversos elementos dentro de ese panel.

En este ejemplo se muestra cómo usar un InlineUIContainer para insertar una imagen en un RichTextBlock. 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>Contenedores de desbordamiento

Puedes usar RichTextBlock con elementos de [RichTextBlockOverflow](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlockOverflow) para crear diseños de página con varias columnas u otros diseños avanzados. El contenido de un elemento RichTextBlockOverflow siempre procede de un elemento RichTextBlock. Para vincular los elementos RichTextBlockOverflow, estos se establecen como el OverflowContentTarget de un RichTextBlock u otro RichTextBlockOverflow.

Este es un ejemplo simple que crea un diseño de dos columnas. Consulta la sección de ejemplos para conocer un ejemplo más complejo.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>Formato de texto

Aunque el RichTextBlock almacena texto sin formato, puedes aplicar diversas opciones de formato para personalizar la representación del texto en la aplicación. Puedes establecer las propiedades de control estándar, como FontFamily, FontSize, FontStyle, Foreground y CharacterSpacing, para cambiar el aspecto del texto. También puedes usar elementos de texto en línea y propiedades adjuntas a Typography para dar formato al texto. Estas opciones solo afectan al modo en que el RichTextBlock muestra el texto localmente, por lo que si copias y pegas el texto en un control de texto enriquecido, por ejemplo, no se aplica ningún formato.

### <a name="inline-elements"></a>Elementos en línea

El espacio de nombres [Windows.UI.Xaml.Documents](/uwp/api/Windows.UI.Xaml.Documents) proporciona una variedad de elementos de texto en línea que puedes usar para dar formato al texto, como Bold, Italic, Run, Span y LineBreak. Una forma habitual para aplicar formato a las secciones de texto es colocar el texto en un elemento Run o Span y luego establecer las propiedades en ese elemento.

Este es un Paragraph con la primera frase en texto azul de 16 pt y negrita.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Tipografía

Las propiedades adjuntas de la clase [Typography](/uwp/api/Windows.UI.Xaml.Documents.Typography) otorgan acceso a un conjunto de propiedades de tipografía de Microsoft OpenType. Puedes establecer estas propiedades adjuntas tanto en el RichTextBlock como en los elementos de texto en línea individuales, como aquí se muestra.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>Recomendaciones

Consulta las directrices para texto y tipografía.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

[Controles de texto](text-controls.md)

**Para diseñadores**
- [Directrices sobre revisión ortográfica](text-controls.md)
- [Adición de búsqueda](/previous-versions/windows/apps/hh465231(v=win.10))
- [Directrices sobre la entrada de texto](text-controls.md)

**Para desarrolladores (XAML)**
- [Clase TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Clase Windows.UI.Xaml.Controls PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)


**Para desarrolladores (otros)**
- [Propiedad String.Length](/dotnet/api/system.string.length)

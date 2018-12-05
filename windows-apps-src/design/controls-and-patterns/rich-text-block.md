---
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
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
ms.openlocfilehash: 3d71a7995bc56a87e6a9b0fc53ff35fdebe82596
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758735"
---
# <a name="rich-text-block"></a>Bloque de texto enriquecido

 

Los bloques de texto enriquecido proporcionan varias características para el diseño avanzado de texto que puedes usar cuando necesitas admitir párrafos, elementos de interfaz de usuario en línea o diseños de texto complejos.

> **API importantes**: [Clase RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [Clase RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx), [Clase Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx), [Clase Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un **RichTextBlock** si necesitas admitir varios párrafos, varias columnas u otros diseños de texto complejos, o elementos de interfaz de usuario en línea, como imágenes.

Usa un **TextBlock** para mostrar la mayor parte del texto de solo lectura en la aplicación. Puedes usarlo para mostrar texto de una o varias líneas, hipervínculos en línea y texto con formato como, por ejemplo, negrita, cursiva o subrayado. TextBlock proporciona un modelo de contenido más simple, por lo que normalmente es más fácil de usar, y puede ofrecer un mejor rendimiento de representación de texto que RichTextBlock. Se prefiere para la mayoría del texto de la interfaz de usuario de la aplicación. Aunque puedes incluir saltos de línea en el texto, TextBlock está diseñado para mostrar un único párrafo y no admite la sangría del texto.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RichTextBlock">abrir la aplicación y ver RichTextBlock en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>Crear un bloque de texto enriquecido

La propiedad de contenido de RichTextBlock es la propiedad [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), que admite el texto basado en párrafos a través del elemento [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). No tiene una propiedad **Text** que puedas usar para acceder fácilmente a contenido de texto del control en la aplicación. Sin embargo, RichTextBlock proporciona varias características únicas que TextBlock no ofrece. 

RichTextBlock admite:
- Varios párrafos. Configura la propiedad [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx) para establecer la sangría de los párrafos.
- Elementos de la interfaz de usuario en línea. Usa un [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) para visualizar los elementos de la interfaz de usuario, tales como las imágenes, en línea con el texto.
- Contenedores de desbordamiento. Usa elementos [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) para crear diseños de texto de varias columnas.

### <a name="paragraphs"></a>Párrafos

Los elementos [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) se usan para definir los bloques de texto que deben mostrarse dentro de un control RichTextBlock. Cada RichTextBlock debe incluir al menos un Paragraph. 

Para definir la cantidad de sangría de todos los párrafos de un RichTextBlock, se puede establecer la propiedad [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx). Para invalidar esta configuración para párrafos específicos de un RichTextBlock, puedes establecer la propiedad [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) en un valor diferente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>Elementos de la interfaz de usuario en línea

La clase [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) permite incrustar cualquier UIElement en línea con el texto. Un escenario común es colocar un objeto Image en línea con el texto, pero también puedes usar elementos interactivos, como un objeto Button o CheckBox.

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

Puedes usar un RichTextBlock con elementos [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) para crear diseños de página con varias columnas u otros diseños avanzados. El contenido de un elemento RichTextBlockOverflow siempre procede de un elemento RichTextBlock. Para vincular los elementos RichTextBlockOverflow, estos se establecen como el OverflowContentTarget de un RichTextBlock u otro RichTextBlockOverflow.

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

El espacio de nombres [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) proporciona una variedad de elementos de texto en línea que puedes usar para dar formato al texto, como Bold, Italic, Run, Span y LineBreak. Una forma habitual para aplicar formato a las secciones de texto es colocar el texto en un elemento Run o Span y luego establecer las propiedades en ese elemento.

Este es un Paragraph con la primera frase en texto azul de 16 pt y negrita.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Tipografía

Las propiedades adjuntas de la clase [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) otorgan acceso a un conjunto de propiedades de tipografía de Microsoft OpenType. Puedes establecer estas propiedades adjuntas tanto en el RichTextBlock como en los elementos de texto en línea individuales, como aquí se muestra.

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

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

[Controles de texto](text-controls.md)

**Para diseñadores**
- [Directrices sobre revisión ortográfica](text-controls.md)
- [Adición de búsqueda](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Directrices para la entrada de texto](text-controls.md)

**Para desarrolladores (XAML)**
- [Clase TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Clase Windows.UI.Xaml.Controls PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)


**Para desarrolladores (otros)**
- [Propiedad String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)

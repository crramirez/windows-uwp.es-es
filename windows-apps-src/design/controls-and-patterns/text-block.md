---
author: Jwmsft
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
Description: Text block is the primary control for displaying read-only text in apps.
title: Bloque de texto
label: Text block
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 423634acc2d3806b4652618331fafd68c908e477
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995976"
---
# <a name="text-block"></a>Bloque de texto

 

 El bloque de texto es el control principal para mostrar texto de solo lectura en las aplicaciones. Puedes usarlo para mostrar texto de una o varias líneas, hipervínculos en línea y texto con formato como, por ejemplo, negrita, cursiva o subrayado.
 
 > **API importantes**: [Clase TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [Propiedad Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx), [Propiedad Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Un bloque de texto suele ser más fácil de usar y proporciona mejor rendimiento de representación de texto que un bloque de texto enriquecido, por lo que es preferible para gran parte del texto de la interfaz de usuario de la aplicación. Puedes acceder fácilmente y usar texto de un bloque de texto en tu aplicación obteniendo el valor de la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx). También proporciona muchas de las mismas opciones de formato para personalizar la representación del texto.

Aunque puedes incluir saltos de línea en el texto, el bloque de texto está diseñado para mostrar un único párrafo y no admite la sangría del texto. Usa un **RichTextBlock** si necesitas admitir varios párrafos, texto de varias columnas u otros diseños de texto complejos, o elementos de interfaz de usuario en línea, como imágenes.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TextBlock">abrir la aplicación y ver TextBlock en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-text-block"></a>Crear un bloque de texto

Aquí te mostramos cómo definir un control TextBlock sencillo y establecer su propiedad Text en una cadena.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### <a name="content-model"></a>Modelo de contenido

Existen dos propiedades que puedes usar para agregar contenido a un TextBlock: [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) e [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx).

La manera más común de mostrar texto es establecer la propiedad Text en un valor de cadena, como se muestra en el ejemplo anterior.

También puedes agregar contenido mediante la colocación de elementos de contenido de flujo en línea en la propiedad TextBox.Inlines, como este.
```xaml
<TextBlock>Text can be <Bold>bold</Bold>, <Underline>underlined</Underline>, 
    <Italic>italic</Italic>, or a <Bold><Italic>combination</Italic></Bold>.</TextBlock>
```

Los elementos que se derivan de la clase Inline, como Bold, Italic, Run, Span y LineBreak, habilitan un formato diferente para las distintas partes del texto. Para obtener más información, consulta la sección [Formato de texto](). El elemento Hyperlink en línea te permite agregar un hipervínculo al texto. Sin embargo, al usar Inlines también se deshabilita la representación de texto de la ruta de acceso rápido, lo que se describe en la siguiente sección.


## <a name="performance-considerations"></a>Consideraciones de rendimiento

Siempre que sea posible, XAML usa una ruta de acceso de código más eficiente para el diseño del texto. Esta ruta de acceso rápido tanto disminuye el uso de memoria general como reduce en gran medida el tiempo de CPU para realizar una medición de texto y organización. Esta ruta de acceso rápido se aplica solo a TextBlock, por lo que es preferible cuando sea posible sobre RichTextBlock.

Ciertas condiciones requieren TextBlock para volver a una ruta de acceso más enriquecida de código intensivo de CPU para la representación de texto. Para mantener la representación de texto en la ruta de acceso rápido, asegúrate de seguir estas directrices al establecer las propiedades enumeradas aquí.
- [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): la condición más importante es que la ruta de acceso rápido solo se use cuando se establece texto; para ello, debes establecer explícitamente la propiedad Text en XAML o en el código (tal como se muestra en los ejemplos anteriores). Establecer el texto a través de la colección de Inlines de TextBlock (como `<TextBlock>Inline text</TextBlock>`), deshabilitará la ruta de acceso rápido, debido a la potencial complejidad de varios formatos.
- [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): solo el valor predeterminado de 0 es la ruta de acceso rápido.
- [TextTrimming](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): solo los valores **None**, **CharacterEllipsis** y **WordEllipsis** son la ruta de acceso rápido. El valor **Clip** deshabilita la ruta de acceso rápido.

> **Nota**&nbsp;&nbsp;Antes de la versión 1607 de Windows 10, las propiedades adicionales también afectaban a la ruta de acceso rápido. Si la aplicación se ejecuta en una versión anterior de Windows, estas condiciones provocarán que el texto se represente en la ruta de acceso lento. Para obtener más información acerca de las versiones, consulta el código adaptable Versión.
- [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): solo los valores predeterminados de las diversas propiedades Typography son considerados ruta de acceso rápido.
- [LineStackingStrategy](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): si [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) no es 0, los valores **BaselineToBaseline** y **MaxHeight** deshabilitan la ruta de acceso rápido.
- [IsTextSelectionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): solo el valor **false** es la ruta de acceso rápido. Establecer esta propiedad en **true** deshabilita la ruta de acceso rápido.

Puedes establecer la propiedad [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) en **true** durante la depuración para determinar si el texto usa representación de la ruta de acceso rápido. Cuando se establece esta propiedad en true, el texto que se encuentra en la ruta de acceso rápido se muestra en un color verde claro.

>**Sugerencia**&nbsp;&nbsp;Esta característica se explica en profundidad en esta sesión de Build 2015: [Techniques for Maximizing Universal Windows App Experiences Built with XAML (Rendimiento XAML: técnicas para maximizar experiencias de aplicación de Windows Universal creada con XAML.)](https://channel9.msdn.com/Events/Build/2015/3-698).



Normalmente se establece la configuración de depuración en el reemplazo del método [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) en la página de código subyacente de App.xaml, como este.
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

En este ejemplo, el primer TextBlock se representa con la ruta de acceso rápido, mientras que el segundo no lo es.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

Al ejecutar este XAML en modo de depuración con la propiedad IsTextPerformanceVisualizationEnabled establecida en true, el resultado tiene este aspecto.

![Texto representado en modo de depuración](images/text-block-rendering-performance.png)

>**Atención**&nbsp;&nbsp;El color del texto que no está en la ruta de acceso rápido no cambia. Si hay texto en la aplicación con el color especificado como verde claro, se sigue mostrando en verde claro cuando se encuentra en la ruta de acceso de representación más lenta. Ten cuidado de no confundir el texto establecido en verde en la aplicación con el texto que se encuentra en la ruta de acceso rápido y está establecido en verde debido a la configuración de depuración.

## <a name="formatting-text"></a>Formato de texto

Aunque la propiedad Text almacena texto sin formato, puedes aplicar diversas opciones de formato al control TextBlock para personalizar la representación del texto en la aplicación. Puedes establecer las propiedades de control estándar, como FontFamily, FontSize, FontStyle, Foreground y CharacterSpacing, para cambiar el aspecto del texto. También puedes usar elementos de texto en línea y propiedades adjuntas a Typography para dar formato al texto. Estas opciones solo afectan al modo en que el objeto TextBlock muestra el texto localmente, por lo que si copias y pegas el texto en un control de texto enriquecido, por ejemplo, no se aplica ningún formato.

>**Nota**&nbsp;&nbsp;Recuerda que, tal y como hemos explicado en la sección anterior, los elementos de texto en línea y los valores de tipografía que no son predeterminados no se representan en la ruta de acceso rápido.


### <a name="inline-elements"></a>Elementos en línea

El espacio de nombres [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) proporciona una variedad de elementos de texto en línea que puedes usar para dar formato al texto, como Bold, Italic, Run, Span y LineBreak.

Puedes mostrar una serie de cadenas en un TextBlock, donde cada cadena tiene formatos distintos. Para hacerlo, usa un elemento Run para mostrar cada cadena con su formato y separa cada elemento Run con un elemento LineBreak.

Estos son los pasos para definir varias cadenas de texto con formato distinto en un TextBlock mediante objetos Run separados con LineBreak.
```xaml
<TextBlock FontFamily="Segoe UI" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Segoe UI Light" FontSize="24">
        Segoe UI Light 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Georgia" FontSize="18" FontStyle="Italic">
        Georgia Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="Black" FontFamily="Arial" FontSize="14" FontWeight="Bold">
        Arial Bold 14
    </Run>
</TextBlock>
```

Este es el resultado.

![Texto con formato con elementos de ejecución](images/text-block-run-examples.png)

### <a name="typography"></a>Tipografía

Las propiedades adjuntas de la clase [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) otorgan acceso a un conjunto de propiedades de tipografía de Microsoft OpenType. Puedes establecer estas propiedades adjuntas tanto en el objeto TextBlock como en los elementos de texto en línea individuales. En estos ejemplos se muestran ambas opciones.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Directrices sobre revisión ortográfica](text-controls.md)
- [Adición de búsqueda](search.md)
- [Directrices para la entrada de texto](text-controls.md)
- [Clase TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Clase Windows.UI.Xaml.Controls PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propiedad String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)

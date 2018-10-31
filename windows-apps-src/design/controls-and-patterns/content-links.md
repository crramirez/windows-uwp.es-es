---
author: jwmsft
Description: Use content links to embed rich data in your text controls.
title: Enlaces de contenido en controles de texto
label: Content links
template: detail.hbs
ms.author: jimwalk
ms.date: 03/07/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3bbf7bdf1f41b1dff1049674f3b4463bac6ccb80
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5860086"
---
# <a name="content-links-in-text-controls"></a>Enlaces de contenido en controles de texto

Los nuevos vínculos de contenido proporcionan una forma de insertar datos enriquecidos en los controles de texto, lo que permite al usuario encontrar y usar más información acerca de una persona o un lugar sin abandonar el contexto de la aplicación.

Cuando el usuario agrega un prefijo a una entrada con un símbolo de Y comercial (@) en un RichEditBox, aparece una lista de sugerencias de contactos o lugares que coincidan con la entrada. A continuación, por ejemplo, cuando el usuario elige un lugar, se inserta un ContentLink para dicho lugar en el texto. Cuando el usuario invoca el vínculo de contenido de RichEditBox, se muestra un control flotante con un mapa y la información adicional sobre el lugar.

> **API importantes**: [clase ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink), [clase ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo), [clase RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> La API de vínculos de contenido se expanden en los siguientes espacios de nombres: Windows.UI.Xaml.Controls, Windows.UI.Xaml.Documents y Windows.UI.Text.



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>Comparativa entre vínculos de contenido de controles de edición con formato y bloque de texto

Hay dos maneras diferentes de usar vínculos de contenido:

1. En un [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), el usuario puede abrir un selector para agregar un vínculo de contenido incluyendo prefijos en texto con un símbolo de Y comercial. El vínculo de contenido se almacena como parte del contenido de texto enriquecido.
1. En un [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) o [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock), el vínculo de contenido es un elemento de texto que se usa y se comporta en gran medida como un [Hipervínculo](/uwp/api/windows.ui.xaml.documents.hyperlink).

Este es el aspecto predeterminado de los vínculos de contenido en un RichEditBox y en un TextBlock.

![vínculo de contenido en cuadro de edición enriquecido](images/content-link-default-richedit.png)
![vínculo de contenido en bloque de texto](images/content-link-default-textblock.png)

Las diferencias de uso, representación y comportamiento se tratan en detalle en las siguientes secciones. En esta tabla encontrarás una breve comparación de las diferencias principales entre un vínculo de contenido en un RichEditBox y un bloque de texto.

| Función   | RichEditBox | bloque de texto |
| --------- | ----------- | ---------- |
| Uso | Instancia de ContentLinkInfo | Elemento de texto ContentLink |
| Cursor | Determinado por el tipo de vínculo de contenido, no se puede cambiar | Determinado por la propiedad del Cursor, **null** de manera predeterminada |
| ToolTip | No representado | Muestra el texto secundario |

## <a name="enable-content-links-in-a-richeditbox"></a>Habilite los vínculos de contenido en un RichEditBox

El uso más común de un vínculo de contenido es permitir a un usuario agregar información rápidamente añadiendo un prefijo en el nombre de una persona o lugar con un símbolo de Y comercial (@) en el texto. Si se activa en un [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), se abre un selector y permite al usuario insertar a una persona de su lista de contactos, o un lugar cercano, dependiendo de lo que se haya habilitado.

Se puede guardar el vínculo de contenido con el contenido de texto enriquecido, y puede extraerlo para usarlo en otras partes de la aplicación. Por ejemplo, en una aplicación de correo electrónico, puede extraer la información de la persona y usarla para rellenar el cuadro Para con una dirección de correo.

> [!NOTE]
> El selector de vínculo de contenido es una aplicación que forma parte de Windows, por lo que se ejecuta en un proceso independiente de la aplicación.

### <a name="content-link-providers"></a>Proveedores de vínculos de contenido

Se activan vínculos de contenido en un RichEditBox agregando uno o varios proveedores de vínculos de contenido a la colección [RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders). Hay 2 proveedores de vínculos de contenido integrados en el marco XAML.

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider): busca contactos mediante la aplicación **Contactos**.
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider): busca lugares con la aplicación **Mapas**.

> [!IMPORTANT]
> El valor predeterminado de la propiedad RichEditBox.ContentLinkProviders es **null**, no una colección vacía. Necesitas crear explícitamente el [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection) antes de agregar proveedores de vínculos de contenido.

Aquí te mostramos cómo agregar los proveedores de vínculos de contenido en XAML.

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

También puedes agregar proveedores de vínculos de contenido en un estilo y aplicarlos en varios RichEditBoxes como este.

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

Aquí te mostramos cómo agregar los proveedores de vínculos de contenido en código.

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>Colores de los vínculos de contenido

La apariencia de un vínculo de contenido se determina por su primer plano, su segundo plano y su icono. En un RichEditBox, puedes establecer las propiedades de [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) y [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) para cambiar los colores predeterminados. 

No se puede configurar el cursor. El cursor se representa mediante el RichEditbox basándose en el tipo de vínculo de contenido: un cursor [Persona](/uwp/api/windows.ui.core.corecursortype) para obtener un enlace de persona o un cursor [Pin](/uwp/api/windows.ui.core.corecursortype) para obtener uno de lugar.

### <a name="the-contentlinkinfo-object"></a>El objeto ContentLinkInfo

Cuando el usuario realiza una selección desde el selector de contactos o lugares, el sistema crea un objeto [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo) y lo agrega a la propiedad **ContentLinkInfo** del actual [RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange).

El objeto ContentLinkInfo contiene la información que se usa para mostrar, invocar y administrar el vínculo de contenido.

- **DisplayText**: esta es la cadena que se muestra cuando se representa el vínculo de contenido. En un RichEditBox, el usuario puede editar el texto de un vínculo de contenido después de crearlo, lo que modifica el valor de esta propiedad.
- **SecondaryText**: esta cadena se muestra en la información sobre herramientas de un vínculo de contenido representado.
  - En el vínculo de contenido de un lugar creado por el selector, contiene la dirección de la ubicación, si está disponible.
- **Uri**: el vínculo para obtener más información sobre el tema del vínculo del contenido. Este Uri puede abrir una aplicación instalada o un sitio Web.
- **Id**: se trata de un contador de solo lectura, según el control, creado por el control de RichEditBox. Sirve para realizar un seguimiento de esta ContentLinkInfo durante acciones como eliminar o editar. Si se corta y se pega la ContentLinkInfo de nuevo en el control, recibirá un nuevo Id. Los valores de Id son incrementales.
- **LinkContentKind**: una cadena que describe el tipo de vínculo de contenido. Los tipos de contenido integrados son _Lugares_ y _Contactos_. El valor distingue mayúsculas de minúsculas.

#### <a name="link-content-kind"></a>Tipo de contenido de vínculo

Hay varias situaciones en las que el LinkContentKind es importante.

- Cuando un usuario copia un vínculo de contenido de un RichEditBox y lo pega en otro RichEditBox, ambos controles deben tener un ContentLinkProvider para ese tipo de contenido. De lo contrario, el vínculo se pega como texto.
- Puedes usar LinkContentKind en un controlador de eventos [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) para determinar qué hacer con un vínculo de contenido cuando se usa en otras partes de la aplicación. Consulta la sección Ejemplos para obtener más información.
- El LinkContentKind influye en la forma en que el sistema abre el Uri cuando se invoca el enlace. Se verá en la descripción del inicio de Uri a continuación.

#### <a name="uri-launching"></a>Inicio de Uri

La propiedad Uri funciona de manera muy similar a la propiedad NavigateUri de un hipervínculo. Cuando un usuario hace clic encima, inicia el Uri en el explorador predeterminado, o en la aplicación que esté registrada para el protocolo determinado que se especifica en el valor de Uri.

El comportamiento específico de los 2 tipos de contenido de vínculo integrados se describen aquí.

##### <a name="places"></a>Lugares

El selector de lugares, crea un ContentLinkInfo con una raíz de Uri de https://maps.windows.com/. Este enlace puede abrirse de 3 maneras:

- Si LinkContentKind = "Lugares", se abre una tarjeta de información en un control flotante. El control flotante es similar al control flotante del selector de vínculos de contenido. Forma parte de Windows y se ejecuta en un proceso independiente de la aplicación.
- Si LinkContentKind no es "Lugares", intenta abrir la aplicación **mapas** en la ubicación especificada. Por ejemplo, esto puede suceder si se ha modificado el LinkContentKind en el controlador de eventos ContentLinkChanged.
- Si no se puede abrir el Uri en la aplicación Mapas, se abre el mapa en el explorador predeterminado. Esto suele ocurrir cuando las _aplicaciones del usuario para la configuración de sitios Web_ no permite abrir al Uri mediante la aplicación **Mapas**.

##### <a name="people"></a>Contactos

El selector de contactos crea un ContentLinkInfo con un Uri que usa el protocolo **ms-people**.

- Si LinkContentKind = "Contactos", se abre una tarjeta de información en un control flotante. El control flotante es similar al control flotante del selector de vínculos de contenido. Forma parte de Windows y se ejecuta en un proceso independiente de la aplicación.
- Si LinkContentKind no es "Contactos", se abrirá la aplicación **Contactos**. Por ejemplo, esto puede suceder si se ha modificado el LinkContentKind en el controlador de eventos ContentLinkChanged.

> [!TIP]
> Para obtener más información acerca de cómo abrir otras aplicaciones y sitios Web desde la aplicación, consulta los temas en [Iniciar una aplicación con un Uri] (/ windows/uwp/inicio-reanudación/inicio-app-con-uri).

#### <a name="invoked"></a>Invocado

Cuando el usuario invoca un vínculo de contenido, se genera el evento [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked) antes de que ocurra el comportamiento predeterminado de iniciar el Uri. Puedes controlar este evento para invalidar o cancelar el comportamiento predeterminado.

Este ejemplo muestra cómo invalidar el comportamiento de inicio predeterminado del vínculo de contenido de un lugar. En lugar de abrir el mapa en una tarjeta de información de lugar, la aplicación mapas o el explorador web predeterminado, marca el evento como Handled y abre el mapa en un control [WebView](/uwp/api/windows.ui.xaml.controls.webview) en la aplicación.

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

Puedes usar el evento [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) para recibir una notificación cuando se agregue, modifique o elimine un vínculo de contenido. Esto te permite extraer el vínculo de contenido del texto y usarlo en otras partes de la aplicación. Esto se muestra más adelante en la sección Ejemplos.

Las propiedades de [ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs) son:

- **ChangedKind**: esta enumeración ContentLinkChangeKind es la acción que hay en la ContentLink. Por ejemplo, si se inserta, elimina o edita el ContentLink.
- **Información**: la ContentLinkInfo a la que estaba destinada la acción.

Se genera este evento por cada acción de ContentLinkInfo. Por ejemplo, si el usuario copia y pega a la vez varios vínculos de contenido en RichEditBox, se genera este evento por cada elemento agregado. O bien, si el usuario selecciona y elimina varios vínculos de contenido al mismo tiempo, se genera este evento por cada elemento eliminado.

Este evento se genera en RichEditBox después del evento **TextChanging** y antes del evento **TextChanged**.

#### <a name="enumerate-content-links-in-a-richeditbox"></a>Enumere los vínculos de contenido en un RichEditBox

Puedes usar la propiedad RichEditTextRange.ContentLink para obtener un vínculo de contenido de un documento de edición con formato. La enumeración TextRangeUnit tiene el valor ContentLink para especificar el vínculo de contenido como una unidad que se usará al navegar en un intervalo de texto.

Este ejemplo muestra cómo enumerar todos los vínculos de contenido en un RichEditBox y extraer las personas en una lista.

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>Elemento de texto ContentLink

Para usar un vínculo de contenido en un control TextBlock o RichTextBlock, usa el elemento de texto [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink) (desde el espacio de nombres Windows.UI.Xaml.Documents).

Unos orígenes habituales de un ContentLink en un bloque de texto son:

- Un vínculo de contenido creado por un selector extraído de un control RichTextBlock.
- Un vínculo de contenido de un lugar que crees en el código.

En otras situaciones, suele ser adecuado un elemento de texto de hipervínculo.

### <a name="contentlink-appearance"></a>Apariencia de ContentLink

La apariencia de un vínculo de contenido se determina por su primer plano, su segundo plano y el cursor. En un bloque de texto, se pueden configurar las propiedades de Primer plano (de TextElement) y [Segundo plano](/uwp/api/windows.ui.xaml.documents.contentlink.background) para cambiar los colores predeterminados.

De manera predeterminada, aparece el cursor [Mano](/uwp/api/windows.ui.core.corecursortype) cuando el usuario se desplaza sobre el vínculo de contenido. A diferencia de RichEditBlock, los controles de bloque de texto no cambian el cursor automáticamente en función del tipo de enlace. Se puede configurar la propiedad [Cursor](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor) para cambiar el cursor en función del tipo de enlace u otros factores.

Este es un ejemplo de un ContentLink usado en un TextBlock. Se crea la ContentLinkInfo en el código y se asigna a la propiedad Información del elemento de texto ContentLink creado en XAML.

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> Cuando uses un ContentLink en un control de texto con otros elementos de texto en XAML, coloca el contenido en un contenedor [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) y aplica el atributo `xml:space="preserve"` al contenedor Span para mantener el espacio en blanco entre el ContentLink y otros elementos.

## <a name="examples"></a>Ejemplos

En este ejemplo, un usuario puede incluir el vínculo de contenido de una persona o un lugar en un RickTextBlock. Se controla el evento ContentLinkChanged para extraer los vínculos de contenido y mantenerlos actualizados, bien en una lista de contactos o de lugares.

En las plantillas de elemento para las listas, se utiliza un TextBlock con un elemento de texto ContentLink para mostrar la información del vínculo de contenido. ListView proporciona su propio segundo plano para cada elemento, así que el segundo plano de ContentLink queda configurado como Transparente.

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```

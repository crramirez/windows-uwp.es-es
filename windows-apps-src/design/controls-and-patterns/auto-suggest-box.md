---
title: Directrices para los cuadros de sugerencias automáticas
description: Obtenga información sobre cómo AutoSuggestBox para proporcionar una lista de sugerencias para que el usuario seleccione una a medida que escribe.
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b77ef62e954fc0b6d8a41b091a4822fac22079f0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218958"
---
# <a name="auto-suggest-box"></a>Cuadro de sugerencias automáticas

Usa un AutoSuggestBox para proporcionar una lista de sugerencias para que el usuario seleccione una a medida que escribe.

![Un cuadro de sugerencias automáticas](images/controls_autosuggest_expanded01.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma**: [Clase AutoSuggestBox](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox), [Evento TextChanged](/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged), [Evento SuggestionChose](/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen), [Evento QuerySubmitted](/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Si quieres un control simple y personalizable que permita la búsqueda de texto con una lista de sugerencias, elige un cuadro de sugerencias automáticas.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/AutoSuggestBox">abrirla y ver AutoSuggestBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un cuadro de sugerencias automáticas en la aplicación Groove Música.

![Un cuadro de sugerencias automáticas en la aplicación Groove Música](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>Anatomía
El punto de entrada del cuadro de sugerencias automáticas consta de un encabezado opcional y de un cuadro de texto con la sugerencia opcional:

![Ejemplo de punto de entrada para el control de la sugerencia automática](images/controls_autosuggest_entrypoint.png)

La lista de resultados de sugerencias automáticas se rellena automáticamente cuando el usuario comienza a escribir texto. La lista de resultados puede aparecer encima o debajo del cuadro de entrada de texto. Aparece un botón "Borrar todo":

![Ejemplo de control ampliado de la sugerencia automática](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>Crear un cuadro de sugerencias automáticas

Para usar un AutoSuggestBox, debes responder a 3 acciones del usuario.

- Cambio de texto: actualizar la lista de sugerencias a medida que el usuario escribe texto.
- Elección de sugerencia: Actualizar el cuadro de texto cuando el usuario seleccione una sugerencia de la lista de sugerencias.
- Envío de consultas: cuando el usuario envíe una consulta, mostrar los resultados de la misma.

### <a name="text-changed"></a>Cambio de texto

El evento [TextChanged](/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged) se produce siempre que se actualiza el contenido del cuadro de texto. Usa la propiedad [Reason](/uwp/api/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason) de los argumentos de evento para determinar si el cambio se debe a una entrada del usuario. Si el motivo del cambio es **UserInput**, filtra los datos en función de la entrada. A continuación, establece los datos filtrados como el [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) del AutoSuggestBox para actualizar la lista de sugerencias.

Para controlar cómo se muestran los elementos en la lista de sugerencias, puedes usar [DisplayMemberPath](/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) o [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate).

- Para mostrar el texto de una sola propiedad de tu elemento de datos, establece la propiedad DisplayMemberPath para elegir qué propiedad del objeto mostrar en la lista de sugerencias.
- Para definir un aspecto personalizado para cada elemento en la lista, usa la propiedad ItemTemplate.

### <a name="suggestion-chosen"></a>Elección de sugerencia

Cuando un usuario navega por la lista de sugerencias con el teclado, debes actualizar el texto en el cuadro de texto para que ambos coincidan.

Puedes establecer la propiedad [TextMemberPath](/uwp/api/windows.ui.xaml.controls.autosuggestbox.textmemberpath) para elegir qué propiedad del objeto de datos mostrar en el cuadro de texto. Si especificas un TextMemberPath, el cuadro de texto se actualiza automáticamente. Normalmente debes especificar el mismo valor para DisplayMemberPath y TextMemberPath para que el texto de la lista de sugerencias y el del cuadro de texto sea el mismo.

Si necesitas mostrar más de una propiedad simple, administra el evento [SuggestionChosen](/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen) para rellenar el cuadro de texto con texto personalizado basado en el elemento seleccionado.

### <a name="query-submitted"></a>Envío de consultas

Gestiona el evento [QuerySubmitted](/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted) para realizar una acción de consulta adecuada para tu aplicación y mostrar el resultado al usuario.

El evento QuerySubmitted se produce cuando un usuario confirma una cadena de consulta. El usuario puede confirmar una consulta de una de estas maneras:
- Mientras el foco está en el cuadro de texto, presiona Entrar o haz clic en el icono de la consulta. La propiedad de los argumentos del evento [ChosenSuggestion](/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion) es **null**.
- Mientras el foco está en la lista de sugerencias, presiona Entrar, haz clic o pulsa en un elemento. La propiedad de los argumentos del evento ChosenSuggestion contiene el elemento que se seleccionó en la lista.

En todos los casos, la propiedad de los argumentos del evento [QueryText](/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext) contiene el texto del cuadro de texto.

Este es un AutoSuggestBox simple con los controladores de eventos necesarios.

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>Usa AutoSuggestBox para búsquedas

Usa un AutoSuggestBox para proporcionar una lista de sugerencias para que el usuario seleccione una a medida que escribe.

De manera predeterminada, el cuadro de entrada de texto no muestra un botón de consulta. Puedes establecer la propiedad [QueryIcon](/uwp/api/windows.ui.xaml.controls.autosuggestbox.queryicon) para agregar un botón con el icono especificado en la parte derecha del cuadro de texto. Por ejemplo, para que el AutoSuggestBox tenga el aspecto de un cuadro de búsqueda típico, agrega un icono "Buscar", como este.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

Aquí se muestra un AutoSuggestBox con un icono 'Buscar'.

![Ejemplo de punto de entrada para el control de la sugerencia automática](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

-   Cuando se usa un cuadro de sugerencias automáticas para realizar búsquedas y no existen resultados para el texto escrito, se muestra el mensaje de una línea "No hay resultados", para que los usuarios sepan que su solicitud se ha ejecutado:

    ![Ejemplo de un cuadro de sugerencias automáticas sin resultados de búsqueda](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.
- [Ejemplo de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Revisión ortográfica](text-controls.md)
- [Búsqueda](search.md)
- [Clase TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Clase Windows.UI.Xaml.Controls PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propiedad String.Length](/dotnet/api/system.string.length)

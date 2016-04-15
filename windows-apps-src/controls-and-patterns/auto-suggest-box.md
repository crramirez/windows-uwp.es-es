---
Description: Un cuadro de entrada de texto que proporciona sugerencias como tipos de usuarios.
title: Directrices para los cuadros de sugerencias automáticas
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
---
# Cuadro de sugerencias automáticas
Usa un AutoSuggestBox para proporcionar una lista de sugerencias para que el usuario seleccione una a medida que escribe.

![Un cuadro de sugerencias automáticas](images/controls/auto-suggest-box-open.png)

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)
-   [**Evento TextChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx)
-   [**Evento SuggestionChose**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx)
-   [**Evento QuerySubmitted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx)

## ¿Es este el control adecuado?

Si quieres un control simple y personalizable que permita la búsqueda de texto con una lista de sugerencias, elige un cuadro de sugerencias automáticas.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## Ejemplos

Un cuadro de sugerencias automáticas en la aplicación Groove Música.

![Un cuadro de sugerencias automáticas en la aplicación Groove Música](images/control-examples/auto-suggest-box-groove.png)

## Anatomía
El punto de entrada del cuadro de sugerencias automáticas consta de un encabezado opcional y de un cuadro de texto con la sugerencia opcional:

![Ejemplo de punto de entrada para el control de la sugerencia automática](images/controls_autosuggest_entrypoint.png)

La lista de resultados de sugerencias automáticas se rellena automáticamente cuando el usuario comienza a escribir texto. La lista de resultados puede aparecer encima o debajo del cuadro de entrada de texto. Aparece un botón "Borrar todo":

![Ejemplo de control ampliado de la sugerencia automática](images/controls_autosuggest_expanded01.png)

## Crear un cuadro de sugerencias automáticas

Para usar un AutoSuggestBox, debes responder a 3 acciones del usuario.

- Cambio de texto: actualizar la lista de sugerencias a medida que el usuario escribe texto.
- Elección de sugerencia: Actualizar el cuadro de texto cuando el usuario seleccione una sugerencia de la lista de sugerencias.
- Envío de consultas: cuando el usuario envíe una consulta, mostrar los resultados de la misma.

### Cambio de texto

El evento [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx) se produce siempre que se actualiza el contenido del cuadro de texto. Usa la propiedad [Reason](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason.aspx) de los argumentos de evento para determinar si el cambio se debe a una entrada del usuario. Si el motivo del cambio es **UserInput**, filtra los datos en función de la entrada. A continuación, establece los datos filtrados como el [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) del AutoSuggestBox para actualizar la lista de sugerencias.

Para controlar cómo se muestran los elementos en la lista de sugerencias, puedes usar [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) o [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx).

- Para mostrar el texto de una sola propiedad de tu elemento de datos, establece la propiedad DisplayMemberPath para elegir qué propiedad del objeto mostrar en la lista de sugerencias.
- Para definir un aspecto personalizado para cada elemento en la lista, usa la propiedad ItemTemplate.

### Elección de sugerencia

Cuando un usuario navega por la lista de sugerencias con el teclado, debes actualizar el texto en el cuadro de texto para que ambos coincidan.

Puedes establecer la propiedad [TextMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textmemberpath.aspx) para elegir qué propiedad del objeto de datos mostrar en el cuadro de texto. Si especificas un TextMemberPath, el cuadro de texto se actualiza automáticamente. Normalmente debes especificar el mismo valor para DisplayMemberPath y TextMemberPath para que el texto de la lista de sugerencias y el del cuadro de texto sea el mismo.

Si necesitas mostrar más de una propiedad simple, administra el evento [SuggestionChosen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx) para rellenar el cuadro de texto con texto personalizado basado en el elemento seleccionado.

### Envío de consultas

Gestiona el evento [QuerySubmitted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx) para realizar una acción de consulta adecuada para tu aplicación y mostrar el resultado al usuario.

El evento QuerySubmitted se produce cuando un usuario confirma una cadena de consulta. El usuario puede confirmar una consulta de una de estas maneras:
- Mientras el foco está en el cuadro de texto, presiona Entrar o haz clic en el icono de la consulta. La propiedad de los argumentos del evento [ChosenSuggestion](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion.aspx) es **null**.
- Mientras el foco está en la lista de sugerencias, presiona Entrar, haz clic o pulsa en un elemento. La propiedad de los argumentos del evento ChosenSuggestion contiene el elemento que se seleccionó en la lista.

En todos los casos, la propiedad de los argumentos del evento [QueryText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext.aspx) contiene el texto del cuadro de texto. 

### Usa AutoSuggestBox para búsquedas

Usa un AutoSuggestBox para proporcionar una lista de sugerencias para que el usuario seleccione una a medida que escribe.

De manera predeterminada, el cuadro de entrada de texto no muestra un botón de consulta. Puedes establecer la propiedad [QueryIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.queryicon.aspx) para agregar un botón con el icono especificado en la parte derecha del cuadro de texto. Por ejemplo, para que el AutoSuggestBox tenga el aspecto de un cuadro de búsqueda habitual, agrega un icono 'Buscar', como este.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

Aquí se muestra un AutoSuggestBox con un icono 'Buscar'.

![Ejemplo de punto de entrada para el control de la sugerencia automática](images/controls_autosuggest_entrypoint.png)

## Muestras

Para ver ejemplos completos del funcionamiento de AutoSuggestBox, consulta la [muestra de migración de AutoSuggestBox](http://go.microsoft.com/fwlink/p/?LinkId=619996) y la [Muestra de conceptos básicos de interfaz de usuario de XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992).

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

## Lo que se debe y no se debe hacer

-   Cuando se usa un cuadro de sugerencias automáticas para realizar búsquedas y no existen resultados para el texto escrito, se muestra el mensaje de una línea "No hay resultados", para que los usuarios sepan que su solicitud se ejecutó:

    ![Ejemplo de un cuadro de sugerencias automáticas sin resultados de búsqueda](images/controls_autosuggest_noresults.png)


## Artículos relacionados

- [Controles de texto](text-controls.md)
- [Revisión ortográfica](spell-checking-and-prediction.md)
- [Búsqueda](search.md)
- [**Clase TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Clase Windows.UI.Xaml.Controls PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propiedad String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)


<!--HONumber=Mar16_HO4-->



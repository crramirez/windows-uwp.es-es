---
description: Filtrado de los elementos de la colección a través de los datos proporcionados por el usuario.
title: Filtrado de colecciones
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: a62ec52fe2b8f6caac2ac27cfc4d002ec44a5b32
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034538"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>Filtrado de colecciones y listas mediante la entrada del usuario
Si la colección muestra muchos elementos o está estrechamente vinculada con la interacción del usuario, el filtrado es una característica que resulta útil implementar. El filtrado mediante el método descrito en este artículo se puede implementar en la mayoría de los controles de colección, incluidos [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView](/uwp/api/windows.ui.xaml.controls.gridview) e [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2). Se pueden usar muchos tipos de entrada de usuario para filtrar una colección (como las casillas de verificación, los botones de radio y los controles deslizantes), pero este artículo se centrará en tomar la entrada del usuario basada en texto y usarla para actualizar un control ListView en tiempo real, de acuerdo con la búsqueda del usuario. 

> [!NOTE]
> Este artículo se centrará en el filtrado con un control ListView. Ten en cuenta que el método de filtrado también se puede aplicar a otros controles de colecciones como GridView, ItemsRepeater o TreeView.

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>Configuración de la interfaz de usuario y XAML para el filtrado
Para implementar el filtrado, la aplicación debe tener un elemento ListView que debe aparecer junto a un TextBox u otro control que permita la entrada del usuario. El texto que el usuario escribe en el TextBox se utilizará como filtro; es decir, solo se mostrarán los resultados que contengan su consulta de entrada/búsqueda de texto. A medida que el usuario escribe en el TextBox, el control ListView se actualiza constantemente con los resultados filtrados. En concreto, cada vez que cambia el texto del TextBox, aunque solo sea por una letra, el control ListView pasará por sus elementos y los filtrará con ese término.

En el código siguiente se muestra una interfaz de usuario con un elemento ListView simple, su DataTemplate y el TextBox correspondiente. En este ejemplo, el control ListView muestra una colección de objetos Person. Person es una clase definida en el código subyacente (no se muestra en el ejemplo de código siguiente) y cada objeto Person tiene las siguientes propiedades: FirstName, LastName y Company.

Mediante el elemento TextBox, los usuarios pueden escribir un término de búsqueda o filtrado para filtrar la lista de objetos Person por apellido. Ten en cuenta que el elemento TextBox está enlazado con un nombre específico (`FilterByLName`) y tiene su propio evento TextChanged (`FilteredLV_LNameChanged`). El nombre enlazado nos permite acceder al contenido o texto del TextBox en el código subyacente, mientras que el evento TextChanged se activará siempre que el usuario escriba en el TextBox. Esto último nos permite realizar una operación de filtrado al recibir la entrada del usuario. 

Para que el filtrado funcione, el control ListView debe tener un origen de datos que se pueda manipular en el código subyacente, como una `ObservableCollection<>`. En este caso, la propiedad ItemsSource del control ListView se asigna a una `ObservableCollection<Person>` en el código subyacente. 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>Filtrado de los datos
Las consultas [LINQ](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) permiten agrupar, ordenar y seleccionar ciertos elementos de una colección. Para filtrar una lista, crearemos una consulta LINQ que solo seleccione los términos que coinciden con el término de filtrado o consulta de búsqueda que introdujo el usuario en el TextBox `FilterByLName`. El resultado de la consulta se puede asignar a un objeto de colección [IEnumerable<T>](/dotnet/api/system.collections.generic.ienumerable-1). Una vez que tenemos esta colección, podemos usarla para compararla con la lista original. Después, quitaremos los elementos que no coinciden y agregaremos los elementos que sí (en caso de que se presione Retroceso).

> [!NOTE]
> Para que ListView tenga una animación lo más intuitiva posible al agregar y quitar elementos, es importante quitar y agregar elementos a la colección de ItemsSource de ListView, en lugar de crear una nueva colección de objetos filtrados y asignarla a la propiedad ItemsSource de ListView.

Para empezar, es necesario inicializar el origen de datos original en una colección independiente, como `List<T>` o `ObservableCollection<T>`. En este ejemplo, tenemos una `List<Person>` denominada `People`, que contiene todos los objetos Person que se muestran en el control ListView (el rellenado e inicialización de esta lista no se muestra en el siguiente fragmento de código). También se necesitará una lista para almacenar los datos filtrados, que cambiarán constantemente cada vez que se aplique un filtro. Será una `ObservableCollection<Person>` llamada `PeopleFiltered` y, en el momento de la inicialización, tendrá el mismo contenido que `People`.
 
El código siguiente realiza la operación de filtrado mediante los pasos siguientes, que se muestran en el código siguiente:
 - Establece la propiedad ItemsSource de ListView en `PeopledFiltered`. 
 - Define el evento TextChanged [`FilteredLV_LNameChanged()`] para el TextBox `FilterByLName`. Dentro de esta función, filtra los datos.
 - Para filtrar los datos, accede a la consulta de búsqueda o al término de filtrado especificados por el usuario mediante `FilterByLName.Text`. Usa una consulta LINQ para seleccionar los elementos de `People` cuyos apellidos contengan el término `FilterByLName.Text` y agrega esos elementos coincidentes a una colección denominada `TempFiltered`.
 - Compara la colección de `PeopleFiltered` actual con los elementos recién filtrados en `TempFiltered`. Para ello, quita y agrega elementos de `PeopleFiltered` cuando sea necesario.
 - A medida que los elementos se quitan y se agregan desde `PeopleFiltered`, el control ListView se actualizará y animará en consecuencia.

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

Ahora, a medida que el usuario escribe los términos de filtrado en el TextBox `FilterByLName`, el control ListView se actualiza inmediatamente para mostrar solo las personas cuyo apellido contiene el término de filtrado.

## <a name="next-steps"></a>Pasos siguientes

### <a name="get-the-sample-code"></a>Obtención del código de ejemplo
- Si tienes instalada la aplicación XAML Controls Gallery</strong>, haz clic [aquí](xamlcontrolsgallery:/item/ListView) para abrir la aplicación y consultar un ejemplo más sólido y detallado de filtrado de listas en la página de ListView.
- Obtener la [aplicación XAML Controls Gallery (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT).

### <a name="related-articles"></a>Artículos relacionados
- [Listas](lists.md)
- [Vista de lista y vista de cuadrícula](listview-and-gridview.md)
- [Comandos de colecciones](collection-commanding.md)

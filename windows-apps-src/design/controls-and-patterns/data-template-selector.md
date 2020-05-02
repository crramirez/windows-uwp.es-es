---
Description: Usa los selectores de plantillas de datos para personalizar los estilos de los elementos en función de las propiedades de estos.
title: Selección de plantillas de datos
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: d388e1f4b3f1b1be4e265185934a02b6ccd20064
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "76123857"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>Selección de plantillas de datos: aplicación de estilos a los elementos según sus propiedades

El diseño personalizado de los controles de colección se administra mediante una plantilla [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate). Las plantillas de datos definen cómo se debe diseñar y aplicar el estilo a cada elemento, y el marcado que se aplica a todos los elementos de la colección. En este artículo se explica cómo usar [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) para aplicar diferentes plantillas de datos en una colección y seleccionar cuál de ellas se va a usar en función de ciertas propiedades de elementos o valores de tu elección.

> **API importantes**: [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector), [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) es una clase que habilita la lógica de selección de plantillas personalizada. Permite definir reglas que especifican la plantilla de datos que se va a usar para determinados elementos de una colección. Para implementar esta lógica, se crea una subclase de DataTemplateSelector en el código subyacente y se define la lógica que determina la plantilla de datos que se va a usar para cada categoría de elementos (por ejemplo, elementos de un determinado tipo o elementos con un determinado valor de propiedad, etc.). Puedes declarar una instancia de esta clase en la sección de recursos del archivo XAML junto con las definiciones de las plantillas de datos que vas a usar. Puedes identificar estos recursos con un valor `x:Key`, lo que te permitirá hacer referencia a ellos en el código XAML.

## <a name="prerequisites"></a>Requisitos previos

- Cómo [usar y crear un control de colección como ListView o GridView](listview-and-gridview.md).
- Cómo [personalizar la apariencia de los elementos mediante DataTemplate](item-containers-templates.md#data-template).

## <a name="when-not-to-use-a-datatemplateselector"></a>Cuándo no se debe usar DataTemplateSelector

Por lo general, no debes dar a todos los elementos de un control ListView o GridView un diseño o estilo completamente diferente: esto sería un uso poco eficiente de DataTemplateSelector y afectaría negativamente al rendimiento.

Algunos elementos de la presentación visual de un elemento de lista se pueden controlar empleando una sola plantilla de datos, mediante el enlace de determinadas propiedades. Por ejemplo, para que cada uno de los elementos pueda tener un icono diferente, enlázalos a una propiedad de origen de icono de la plantilla de datos y asigna a cada elemento un valor diferente para la propiedad de origen de icono. Con esto lograrías un rendimiento mejor que con el uso de DataTemplateSelector.

## <a name="when-to-use-a-datatemplateselector"></a>Cuándo se debe usar DataTemplateSelector

Puedes crear un objeto [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) si quieres usar varias plantillas de datos en un control de colección. `DataTemplateSelector` ofrece la flexibilidad de destacar determinados elementos y, al mismo tiempo, consigue que los elementos tengan un diseño similar. Hay muchos casos de uso en los que DataTemplateSelector es útil y algunos escenarios en los que es mejor replantearse el control y la estrategia que se está utilizando.

Normalmente, los controles de colección se enlazan a una colección de elementos que son todos de un tipo. Sin embargo, aunque los elementos sean del mismo tipo, pueden tener valores diferentes para determinadas propiedades o representar distintos significados. Algunos elementos también pueden ser más importantes que otros, o uno de ellos puede ser especialmente importante o diferente y es necesario destacarlo visualmente. En estos casos, DataTemplateSelector será muy útil.

También puedes enlazar a una colección que contiene diferentes tipos de elementos: la colección enlazada puede incluir una combinación de cadenas, números enteros, objetos de clase personalizados, etc. Esto hace que DataTemplateSelector sea especialmente útil, ya que puede asignar plantillas de datos diferentes en función del tipo de objeto de un elemento.

Estos son algunos ejemplos de cuándo es posible usar el selector de plantillas de datos:

- Representación de distintos niveles de empleados en un control ListView: cada tipo o nivel de empleado puede necesitar un fondo de color diferente para distinguirlo fácilmente.
- Representación de artículos de venta en una galería de productos que utiliza GridView: puede que un artículo necesite un fondo rojo o un color de fuente diferente para destacarlo de los artículos con un precio normal.
- Representación de fotos de ganadores o de las principales fotos de una galería fotográfica con FlipView.
- Cuando es necesario representar números positivos o negativos en un control ListView de manera diferente, o cadenas cortas y largas.

## <a name="create-a-datatemplateselector"></a>Creación de un objeto DataTemplateSelector

Cuando creas un selector de plantillas de datos, defines la lógica de selección de plantillas en el código y defines las plantillas de datos en el archivo XAML.

### <a name="code-behind-component"></a>Componente de código subyacente

Para usar un selector de plantillas de datos, primero debes crear una subclase de [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) (una clase que deriva de él) en el código subyacente. En la clase, se declara cada plantilla como una propiedad de la clase. Después, puedes invalidar el método [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) para incluir tu propia lógica de selección de plantillas.

Este es un ejemplo de una subclase de `DataTemplateSelector` simple, llamada `MyDataTemplateSelector`.

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

La clase `MyDataTemplateSelector` deriva de la clase `DataTemplateSelector` y define primero dos objetos [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate): `Normal` y `Accent`. Estas son declaraciones vacías por ahora, pero se "rellenarán" con marcado en el archivo XAML.

El método `SelectTemplateCore` toma un objeto de elemento (es decir, cada elemento de la colección) y lo sustituye por reglas que definen qué `DataTemplate` se devolverá según las circunstancias. En este caso, si el elemento es un número impar, recibe la plantilla de datos `Accent`, mientras que si es un número par, recibe la plantilla de datos `Normal`.

### <a name="xaml-component"></a>Componente de XAML

En segundo lugar, debes crear una instancia de esta nueva clase `MyDataTemplateSelector` en la sección de recursos del archivo XAML. Todos los recursos requieren `x:Key`, que se usa para enlazarlos con la propiedad `ItemTemplateSelector` del control de colección (en un paso posterior). También puedes crear dos instancias de objetos `DataTemplate` y definir su diseño en la sección de recursos. Puedes asignar estas plantillas de datos a las propiedades `Accent` y `Normal` que declaraste en la clase `MyDataTemplateSelector`.

A continuación se muestra un ejemplo de los recursos y el marcado XAML que se necesita:

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

Como puedes ver más arriba, se definen las dos plantillas de datos `Normal` y `Accent`; ambas muestran los elementos como botones y, sin embargo, la plantilla de datos `Accent` usa un pincel de color de énfasis para el fondo, mientras que la plantilla de datos `Normal` usa un pincel de color gris (`SystemChromeLowColor`). Estas dos plantillas de datos se asignan a los objetos `Normal` y `Accent` de DataTemplate, que son atributos de la clase MyDataTemplateSelector creados en el código C# subyacente.

El último paso es enlazar `DataTemplateSelector` a la propiedad `ItemTemplateSelector` del control de colección (en este caso, ListView). Esto elimina la necesidad de asignar un valor a la propiedad `ItemTemplate`. 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

Una vez que se compila el código, cada elemento de la colección se ejecutará con el método `SelectTemplateCore` invalidado en `MyDataTemplateSelector` y se representará con la plantilla DataTemplate adecuada.

> [!IMPORTANT]
> Cuando se usa `DataTemplateSelector` con [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2), se enlaza `DataTemplateSelector` a la propiedad `ItemTemplate`. `ItemsRepeater` no tiene ninguna propiedad `ItemTemplateSelector`.

## <a name="datatemplateselector-performance-considerations"></a>Consideraciones sobre el rendimiento de DataTemplateSelector

Cuando se usa un control ListView o GridView con una colección de datos grande, el rendimiento del desplazamiento y el movimiento panorámico pueden ser un problema. Para que el rendimiento de las colecciones grandes sea el adecuado, hay algunos pasos que puedes seguir para mejorar el rendimiento de las plantillas de datos. Estos se describen con más detalle en [Optimización de la interfaz de usuario de ListView y GridView](/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

- _Reducción de objetos por elemento_: mantén el número de objetos de interfaz de usuario de una plantilla de datos en un mínimo razonable.
- Reciclaje de contenedores con colecciones heterogéneas
  - Uso _del evento ChoosingItemContainer_: este evento es una buena manera de utilizar diferentes plantillas de datos para distintos elementos. Para lograr el mejor rendimiento, debes optimizar el almacenamiento en caché y seleccionar plantillas de datos para tus datos concretos.
  - Uso de un _selector de plantillas de elementos_: debe evitarse un selector de plantillas de elementos (`DataTemplateSelector`) en algunos casos debido a su impacto en el rendimiento.

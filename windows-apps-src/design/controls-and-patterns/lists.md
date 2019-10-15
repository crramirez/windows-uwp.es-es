---
Description: Mostrar listas y habilitar la interacción con el contenido basado en la colección.
title: Colecciones y listas
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e1167a57da6a3f54cabcc946cfbf7a592f301d2c
ms.sourcegitcommit: 9625f8fb86ff6473ac2851e600bc02e996993660
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72163741"
---
# <a name="collections-and-lists"></a>Colecciones y listas

Las colecciones y listas hacen referencia a la representación de varios elementos de datos relacionados que aparecen juntos. Las colecciones se pueden representar de varias maneras, mediante diferentes controles de colección (también se pueden denominar vistas de colección). Los controles de colección muestran y permiten las interacciones con el contenido basado de la colección, como una lista de contactos, una lista de fechas, una colección de imágenes, etc.  Los controles tratados en este artículo son:

- Vistas de lista, que se usan principalmente para mostrar colecciones de contenido con mucho texto
- Vistas de cuadrícula, que se usan principalmente para mostrar colecciones de contenido con muchas imágenes
- Vistas con función de pasar de página, que se usan principalmente para mostrar colecciones de contenido con una gran cantidad de imágenes y que requieren que solo un elemento esté en el foco cada vez
- Vistas de árbol, que se usan principalmente para mostrar colecciones de contenido con mucho texto en una jerarquía específica
- ItemsRepeater, que es un bloque de creación personalizable para crear controles de colección personalizados


A continuación, se proporcionan directrices de diseño, características y ejemplos para cada control.

Cada uno de estos controles (a excepción de ItemsRepeater) proporciona las funciones de aplicación de estilos e interacción integradas. Sin embargo, para personalizar aún más el aspecto visual de la vista de colección y de los elementos que contiene, se usa [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). Puedes encontrar información detallada sobre las plantillas de datos y la personalización del aspecto de una vista de colección en la página [Plantillas y contenedores de elementos](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/item-containers-templates).

Cada uno de estos controles (a excepción de ItemsRepeater) también tiene un comportamiento integrado para permitir la selección de uno o de varios elementos. Consulta [Selection modes overview](selection-modes.md) (Información general sobre los modos de selección) para obtener más información.

> **API importantes**: [clase ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [clase GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), [clase FlipView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flipview), [clase TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) y [clase ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

> <div id="main">
> <strong>Windows 10 Fall Creators Update: cambio de comportamiento</strong>
> </div>
> De manera predeterminada, en lugar de llevar a cabo la selección, ahora un lápiz activo se desplaza/realiza movimiento panorámico por una lista en aplicaciones para UWP (como la entrada táctil, el panel táctil y el lápiz pasivo).
> Si la aplicación depende del comportamiento anterior, puedes invalidar el desplazamiento de lápiz y revertir al comportamiento anterior. Para obtener más información, consulta el tema de referencia de API <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer">Clase ScrollViewer</a>.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, puedes ver <a href="xamlcontrolsgallery:/item/ListView">ListView</a>, <a href="xamlcontrolsgallery:/item/GridView">GridView</a>, <a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>, <a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> y <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>Vistas de lista

Las vistas de lista representan elementos con mucho texto, normalmente en un diseño de una sola columna y apilada verticalmente. Te permiten clasificar elementos y asignar encabezados de grupo, arrastrar y colocar elementos, mantener el contenido y reordenar los elementos.

### <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa una vista de lista para:

- Mostrar una colección que conste principalmente de elementos basados en texto, donde todos los elementos deben tener el mismo comportamiento visual y de interacción.
- Representar una colección de contenido único o por categorías.
- Acomodar una variedad de casos de uso, incluidos los casos comunes siguientes:
    - Crear una lista de mensajes o un registro de mensajes.
    - Crear una lista de contactos.
    - Crear el panel maestro en la [vista maestro y detalles](master-details.md). Maestro y detalles es el patrón que se suele usar en aplicaciones de correo electrónico, en las que un panel (el maestro) tiene una lista de elementos seleccionables, mientras que el otro panel tiene una vista detallada del elemento seleccionado.
    

### <a name="examples"></a>Ejemplos

Esta es una vista de lista simple en la que se muestra una lista de contactos y se agrupan los elementos de datos alfabéticamente. Los encabezados de grupo (las letras del alfabeto en este ejemplo) también se pueden personalizar para que permanezcan "adheridos" y aparezcan siempre en la parte superior de la vista de lista al desplazar.

![Vista de lista con datos agrupados](images/listview-grouped-example-resized-final.png)

Esta es una vista de lista que se ha invertido para mostrar un registro de mensajes, con los mensajes más recientes en la parte inferior. En una vista de lista invertida, los elementos aparecen en la parte inferior de la pantalla con una animación integrada.

![Vista de lista invertida](images/listview-inverted-2.png)

### <a name="related-articles"></a>Artículos relacionados
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Vista de lista y vista de cuadrícula</a></p></td>
<td align="left"><p>Aprende los conceptos básicos del uso de una vista de lista o una vista de cuadrícula en tu aplicación.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Plantillas y contenedores de elementos</a></p></td>
<td align="left"><p>Los elementos que se muestran en una vista de lista o cuadrícula pueden tener un rol importante en el aspecto general de la aplicación. Haz que tu aplicación tenga un magnífico aspecto mediante la personalización de la apariencia de los elementos de la colección con la modificación de las plantillas de control y las plantillas de datos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Plantillas de elemento para vistas de lista</a></p></td>
<td align="left"><p>Usa estas plantillas de elemento de ejemplo para una clase ListView y obtén así la apariencia de los tipos de aplicación más comunes.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Listas invertidas</a></p></td>
<td align="left"><p>Las listas invertidas tienen nuevos elementos agregados en la parte inferior, al igual que en una aplicación de chat. Sigue las instrucciones de este artículo para usar una lista invertida en tu aplicación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Deslizar para actualizar</a></p></td>
<td align="left"><p>El mecanismo de extraer para actualizar permite al usuario desplegar una lista de datos con la entrada táctil para recuperar más datos. Usa este artículo para implementar el mecanismo de extraer para actualizar en tu vista de lista.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interfaz de usuario anidada</a></p></td>
<td align="left"><p>La interfaz de usuario anidada es una interfaz de usuario (IU) que expone los controles accionables incluidos en un contenedor sobre el que un usuario puede actuar. Por ejemplo, es posible que tengas un elemento de la vista de lista que contenga un botón y que el usuario pueda seleccionar el elemento de lista o presionar el botón anidado en este. Sigue estos procedimientos recomendados para proporcionar la mejor experiencia de interfaz de usuario anidada a los usuarios.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Vistas de cuadrícula

Las vistas de cuadrícula son adecuadas para organizar y explorar las colecciones de contenido basado en imágenes. Un diseño de la vista de cuadrícula se desplaza verticalmente y se extiende en panorámica horizontal. Los elementos están colocados en un diseño ajustado, aparecen en un orden de lectura de izquierda a derecha y de arriba abajo.

### <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa una vista de cuadrícula para:

- Mostrar una colección de contenido en la que el punto focal de cada elemento sea una imagen y cada elemento deba tener el mismo comportamiento visual y de interacción.
- Mostrar las bibliotecas de contenido.
- Dar formato a las dos vistas de contenido asociadas con el [zoom semántico](semantic-zoom.md).
- Acomodar una variedad de casos de uso, incluidos los casos comunes siguientes:
    - Interfaz de usuario de tipo escaparate (es decir, exploración de aplicaciones, canciones, productos)
    - Bibliotecas fotográficas interactivas

### <a name="examples"></a>Ejemplos

Este ejemplo muestra un diseño de la vista de cuadrícula típico, en este caso para la exploración de aplicaciones. Los metadatos para los elementos de la vista de cuadrícula están normalmente restringidos a unas pocas líneas de texto y una clasificación de elemento.

![Ejemplo de diseño de una vista de cuadrícula](images/controls_gridview_example02.png)

Una vista de cuadrícula es una solución ideal para una biblioteca de contenido, que a menudo se usa para presentar elementos multimedia, como imágenes y vídeos. En una biblioteca de contenido, los usuarios esperan poder presionar un elemento para invocar una acción.

![Ejemplo de una biblioteca de contenido](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>Artículos relacionados
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Vista de lista y vista de cuadrícula</a></p></td>
<td align="left"><p>Aprende los conceptos básicos del uso de una vista de lista o una vista de cuadrícula en tu aplicación.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Plantillas y contenedores de elementos</a></p></td>
<td align="left"><p>Los elementos que se muestran en una vista de lista o cuadrícula pueden tener un rol importante en el aspecto general de la aplicación. Haz que tu aplicación tenga un magnífico aspecto mediante la personalización de la apariencia de los elementos de la colección con la modificación de las plantillas de control y las plantillas de datos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Plantillas de elemento para vistas de cuadrícula</a></p></td>
<td align="left"><p>Usa estas plantillas de elemento de ejemplo para una clase GridView y obtén así la apariencia de los tipos de aplicación más comunes.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interfaz de usuario anidada</a></p></td>
<td align="left"><p>La interfaz de usuario anidada es una interfaz de usuario (IU) que expone los controles accionables incluidos en un contenedor sobre el que un usuario puede actuar. Por ejemplo, es posible que tengas un elemento de la vista de cuadrícula que contenga un botón y que el usuario pueda seleccionar el elemento de cuadrícula o presionar el botón anidado en este. Sigue estos procedimientos recomendados para proporcionar la mejor experiencia de interfaz de usuario anidada a los usuarios.</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>Vistas con función de pasar página

Las vistas con función de pasar página son adecuadas para examinar colecciones de contenido basadas en imágenes, en concreto, donde la experiencia deseada es que solo una imagen esté visible cada vez. Una vista con función de pasar página permite al usuario desplazarse o "pasar página" a través de los elementos de la colección (vertical u horizontalmente), de modo que solo aparezca un elemento después de la interacción del usuario.

### <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa una vista con función de pasar página para:

- Mostar una colección pequeña o mediana (menos de 25 elementos), donde la colección esté formada por imágenes con pocos o sin metadatos.
- Mostrar los elementos de uno en uno y permitir que el usuario final pase página por los elementos a su propio ritmo.
- Acomodar una variedad de casos de uso, incluidos los casos comunes siguientes:
    - Galerías fotográficas
    - Galerías o presentaciones de productos

### <a name="examples"></a>Ejemplos

En los dos ejemplos siguientes se muestra una vista con función de pasar página horizontal y vertical, respectivamente.

![Vista con función de pasar página horizontal](images/controls_flipview_horizonal.jpg)

![Vista con función de pasar página vertical](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>Artículos relacionados
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="flipview.md">Vista para alternar</a></p></td>
<td align="left"><p>Obtén información de los aspectos básicos del uso de una vista con función de pasar página en la aplicación, junto con cómo personalizar la apariencia de los elementos en una vista con función de pasar página.</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>Vistas de árbol

Las vistas de árbol son adecuadas para mostrar colecciones basadas en texto con una jerarquía importante que se tiene que presentar. Los elementos de la vista de árbol se pueden contraer o expandir, se muestran en una jerarquía visual, se pueden complementar con iconos y se pueden arrastrar y colocar entre las vistas de árbol. Las vistas de árbol permiten el anidamiento de n niveles.

### <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa una vista de árbol para:

- Mostrar una colección de elementos anidados cuyo contexto y significado depende de una jerarquía o una cadena organizativa específica.
- Acomodar una variedad de casos de uso, incluidos los casos comunes siguientes:
    - Explorador de archivos
    - Organigrama de la compañía

### <a name="examples"></a>Ejemplos

Este es un ejemplo de una vista de árbol que representa un explorador de archivos y en el que se muestran muchos elementos anidados diferentes que se complementan por iconos.

![Vista de árbol con iconos](images/treeview-icons.png)

### <a name="related-articles"></a>Artículos relacionados
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tree-view.md">Vista de árbol</a></p></td>
<td align="left"><p>Obtén información sobre los aspectos básicos del uso de una vista de árbol en tu aplicación, junto con cómo personalizar la apariencia y el comportamiento de la interacción de los elementos en una vista de árbol.</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater es diferente del resto de los controles de colección que se muestran en esta página, ya que no proporciona ni aplicación de estilos ni interacción integradas cuando, por ejemplo, simplemente se coloca en una página sin definir ninguna propiedad. ItemsRepeater es más bien un bloque de creación que puedes usar para crear tu propio control de colecciones personalizado, específicamente uno que no se pueda conseguir mediante el uso de los otros controles de este artículo. ItemsRepeater es un panel controlado por datos y de alto rendimiento que se puede personalizar para adaptarlo a tus necesidades exactas.

### <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control ItemsRepeater si:

- Tienes una interfaz de usuario y una experiencia de usuario específicas que no se pueden crear con los controles de colección existentes.
- Tienes un origen de datos para tus elementos (por ejemplo, datos extraídos de Internet, una base de datos o una colección preexistente en el código subyacente).

### <a name="examples"></a>Ejemplos

Los tres ejemplos siguientes son controles ItemsRepeater que están enlazados al mismo origen de datos (una colección de números). La colección de números se representa de tres maneras y cada uno de los controles ItemsRepeaters siguientes usa una clase [Layout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layout) personalizada distinta y una propiedad [ItemTemplate](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2) personalizada diferente.

![ItemsRepeater con barras horizontales](images/itemsrepeater-1.png)
![ItemsRepeater con barras verticales](images/itemsrepeater-2.png)
![ItemsRepeater con representación circular](images/itemsrepeater-3.png)

### <a name="related-articles"></a>Artículos relacionados
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>Obtén información de los aspectos básicos del uso de un control ItemsRepeater en la aplicación, junto con cómo implementar todos los componentes de interacción y visuales necesarios para la vista de colección.</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>Lista de comprobación Globalización y localización

<table>
<tr>
<th>Ajuste</th><td>Permitir dos líneas para la etiqueta de la lista.</td>
</tr>
<tr>
<th>Expansión horizontal</th><td>Asegurarse de que los campos pueden acomodar la expansión del texto y de que son desplazables.</td>
</tr>
<tr>
<th>Espaciado vertical</th><td>Usar caracteres no latinos para el espaciado vertical para garantizar que los scripts no latinos se muestren correctamente.</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

**Directrices para el diseño y la experiencia de usuario**
- [Maestro/detalles](master-details.md)
- [Panel de navegación](navigationview.md)
- [Zoom semántico](semantic-zoom.md)
- [Arrastrar y colocar](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Imágenes en miniatura](../../files/thumbnails.md)

**Referencia de las API**
- [Clase ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Clase GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [Clase ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [Clase ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
